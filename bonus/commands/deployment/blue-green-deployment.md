---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [strategy] | setup | deploy | switch | rollback | status
description: Implementar estratégia de blue-green deployment com troca sem tempo de inatividade, validação de saúde e rollback automático
---

# Estratégia de Blue-Green Deployment

Implementar blue-green deployment: $ARGUMENTS

## Estado Atual da Infraestrutura

- Configuração de load balancer: @nginx.conf ou @haproxy.cfg ou configuração de LB em cloud
- Deployment atual: !`curl -s https://api.example.com/version 2>/dev/null || echo "Endpoint de versão necessário"`
- Orquestração de containers: !`kubectl get deployments 2>/dev/null || docker service ls 2>/dev/null || echo "Detecção de plataforma de container necessária"`
- Endpoints de saúde: !`curl -s https://api.example.com/health 2>/dev/null | jq -r '.status // "Unknown"' || echo "Configuração de health check necessária"`
- Configuração DNS: Verificar capacidades de gerenciamento de DNS

## Tarefa

Implementar deployment em produção blue-green com validação e monitoramento abrangentes.

## Componentes da Arquitetura Blue-Green

### 1. **Configuração de Infraestrutura**

#### Configuração de Load Balancer (NGINX)
```nginx
upstream blue {
    server blue-app-1:3000;
    server blue-app-2:3000;
    server blue-app-3:3000;
}

upstream green {
    server green-app-1:3000;
    server green-app-2:3000;
    server green-app-3:3000;
}

# Ambiente ativo atual
upstream active {
    server blue-app-1:3000;
    server blue-app-2:3000;
    server blue-app-3:3000;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://active;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Environment $environment;
        
        # Configuração de health check
        proxy_connect_timeout 5s;
        proxy_send_timeout 5s;
        proxy_read_timeout 5s;
        
        # Configuração de retry
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503;
        proxy_next_upstream_tries 2;
    }

    # Endpoint de health check
    location /health {
        access_log off;
        proxy_pass http://active/health;
        proxy_connect_timeout 1s;
        proxy_send_timeout 1s;
        proxy_read_timeout 1s;
    }

    # Indicador de ambiente
    location /environment {
        access_log off;
        return 200 $environment;
        add_header Content-Type text/plain;
    }
}
```

#### Configuração HAProxy
```haproxy
global
    daemon
    log 127.0.0.1:514 local0
    stats socket /var/run/haproxy.sock mode 600 level admin

defaults
    mode http
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms
    option httplog
    option dontlognull

# Ambiente blue
backend blue_backend
    balance roundrobin
    option httpchk GET /health
    http-check expect status 200
    server blue1 blue-app-1:3000 check
    server blue2 blue-app-2:3000 check
    server blue3 blue-app-3:3000 check

# Ambiente green
backend green_backend
    balance roundrobin
    option httpchk GET /health
    http-check expect status 200
    server green1 green-app-1:3000 check
    server green2 green-app-2:3000 check
    server green3 green-app-3:3000 check

# Frontend com lógica de troca
frontend main_frontend
    bind *:80
    # Troca de ambiente via ACL
    use_backend blue_backend if { var(txn.environment) -m str blue }
    use_backend green_backend if { var(txn.environment) -m str green }
    default_backend blue_backend  # Padrão para blue

# Interface de estatísticas
frontend stats
    bind *:8404
    stats enable
    stats uri /stats
    stats refresh 5s
```

### 2. **Implementação Blue-Green em Kubernetes**

#### Gerenciamento de Serviço Blue-Green
```yaml
# blue-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: app-service-blue
  labels:
    app: myapp
    environment: blue
spec:
  selector:
    app: myapp
    environment: blue
  ports:
    - port: 80
      targetPort: 3000
  type: ClusterIP

---
# green-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: app-service-green
  labels:
    app: myapp
    environment: green
spec:
  selector:
    app: myapp
    environment: green
  ports:
    - port: 80
      targetPort: 3000
  type: ClusterIP

---
# active-service.yaml (aponta para o ambiente ativo atual)
apiVersion: v1
kind: Service
metadata:
  name: app-service-active
  labels:
    app: myapp
    environment: active
spec:
  selector:
    app: myapp
    environment: blue  # Alterar para 'green' durante o deployment
  ports:
    - port: 80
      targetPort: 3000
  type: LoadBalancer
```

#### Deployments Blue-Green
```yaml
# blue-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-blue
  labels:
    app: myapp
    environment: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
      environment: blue
  template:
    metadata:
      labels:
        app: myapp
        environment: blue
    spec:
      containers:
      - name: app
        image: myapp:v1.0.0
        ports:
        - containerPort: 3000
        env:
        - name: ENVIRONMENT
          value: "blue"
        - name: VERSION
          value: "v1.0.0"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "512Mi"
            cpu: "500m"

---
# green-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-green
  labels:
    app: myapp
    environment: green
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
      environment: green
  template:
    metadata:
      labels:
        app: myapp
        environment: green
    spec:
      containers:
      - name: app
        image: myapp:v1.1.0  # Nova versão
        ports:
        - containerPort: 3000
        env:
        - name: ENVIRONMENT
          value: "green"
        - name: VERSION
          value: "v1.1.0"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "512Mi"
            cpu: "500m"
```

### 3. **Scripts de Automação de Deployment**

#### Script de Deployment Blue-Green
```bash
#!/bin/bash
set -e

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
source "$SCRIPT_DIR/config.sh"

# Configuração
BLUE_ENV="blue"
GREEN_ENV="green"
HEALTH_CHECK_URL="${APP_URL}/health"
READY_CHECK_URL="${APP_URL}/ready"
VERSION_URL="${APP_URL}/version"

# Cores para output
RED='\033[0;31m'
GREEN='\033[0;32m'
BLUE='\033[0;34m'
YELLOW='\033[1;33m'
NC='\033[0m' # Sem cor

log() {
    echo -e "${GREEN}[$(date '+%Y-%m-%d %H:%M:%S')] $1${NC}"
}

warn() {
    echo -e "${YELLOW}[AVISO] $1${NC}"
}

error() {
    echo -e "${RED}[ERRO] $1${NC}"
    exit 1
}

# Obter ambiente ativo atual
get_current_env() {
    if kubectl get service app-service-active &>/dev/null; then
        kubectl get service app-service-active -o jsonpath='{.spec.selector.environment}'
    else
        echo "blue"  # Padrão para blue se o serviço não existir
    fi
}

# Obter ambiente inativo (oposto do atual)
get_inactive_env() {
    local current_env=$1
    if [ "$current_env" = "blue" ]; then
        echo "green"
    else
        echo "blue"
    fi
}

# Deploy para ambiente inativo
deploy_to_inactive() {
    local version=$1
    local current_env=$(get_current_env)
    local inactive_env=$(get_inactive_env "$current_env")
    
    log "Ambiente ativo atual: $current_env"
    log "Fazendo deploy da versão $version para o ambiente $inactive_env"
    
    # Atualizar deployment com nova imagem
    kubectl set image deployment/app-$inactive_env app=myapp:$version
    
    # Aguardar conclusão do rollout
    log "Aguardando conclusão do rollout do deployment..."
    kubectl rollout status deployment/app-$inactive_env --timeout=600s
    
    # Verificar se os pods estão em execução
    log "Verificando se os pods estão em execução..."
    kubectl wait --for=condition=ready pod -l app=myapp,environment=$inactive_env --timeout=300s
    
    log "Deploy para o ambiente $inactive_env concluído com sucesso"
}

# Função de health check
health_check() {
    local env=$1
    local service_url="http://app-service-$env.$NAMESPACE.svc.cluster.local"
    
    log "Executando health check para o ambiente $env..."
    
    # Usar kubectl port-forward para testes internos
    kubectl port-forward service/app-service-$env 8080:80 &
    local port_forward_pid=$!
    
    sleep 5  # Aguardar estabelecimento do port-forward
    
    local health_status=1
    local attempts=0
    local max_attempts=10
    
    while [ $attempts -lt $max_attempts ]; do
        if curl -f -s http://localhost:8080/health > /dev/null; then
            health_status=0
            break
        fi
        
        attempts=$((attempts + 1))
        log "Tentativa de health check $attempts/$max_attempts falhou, retentando..."
        sleep 10
    done
    
    # Limpar port-forward
    kill $port_forward_pid 2>/dev/null || true
    
    if [ $health_status -eq 0 ]; then
        log "Health check passou para o ambiente $env"
        return 0
    else
        error "Health check falhou para o ambiente $env após $max_attempts tentativas"
    fi
}

# Smoke tests
run_smoke_tests() {
    local env=$1
    log "Executando smoke tests para o ambiente $env..."
    
    # Port-forward para testes
    kubectl port-forward service/app-service-$env 8080:80 &
    local port_forward_pid=$!
    sleep 5
    
    local test_results=()
    
    # Teste 1: Endpoint de saúde
    if curl -f -s http://localhost:8080/health | jq -e '.status == "healthy"' > /dev/null; then
        test_results+=("✅ Endpoint de saúde")
    else
        test_results+=("❌ Endpoint de saúde")
    fi
    
    # Teste 2: Endpoint de versão
    if curl -f -s http://localhost:8080/version > /dev/null; then
        test_results+=("✅ Endpoint de versão")
    else
        test_results+=("❌ Endpoint de versão")
    fi
    
    # Teste 3: Endpoint da aplicação principal
    if curl -f -s http://localhost:8080/ > /dev/null; then
        test_results+=("✅ Endpoint principal")
    else
        test_results+=("❌ Endpoint principal")
    fi
    
    # Teste 4: Conectividade de banco de dados (se aplicável)
    if curl -f -s http://localhost:8080/db-health 2>/dev/null | jq -e '.connected == true' > /dev/null; then
        test_results+=("✅ Conectividade de banco de dados")
    else
        test_results+=("⚠️  Conectividade de banco de dados (não testada)")
    fi
    
    # Limpar port-forward
    kill $port_forward_pid 2>/dev/null || true
    
    # Exibir resultados
    log "Resultados dos smoke tests para $env:"
    printf '%s\n' "${test_results[@]}"
    
    # Verificar se todos os testes críticos passaram
    local failed_tests=$(printf '%s\n' "${test_results[@]}" | grep -c "❌" || true)
    if [ "$failed_tests" -gt 0 ]; then
        error "Smoke tests falharam com $failed_tests falhas"
    fi
    
    log "Todos os smoke tests passaram para o ambiente $env"
}

# Trocar tráfego para novo ambiente
switch_traffic() {
    local target_env=$1
    local current_env=$(get_current_env)
    
    if [ "$target_env" = "$current_env" ]; then
        warn "Ambiente alvo ($target_env) já está ativo"
        return 0
    fi
    
    log "Trocando tráfego de $current_env para $target_env"
    
    # Criar backup da configuração atual do serviço
    kubectl get service app-service-active -o yaml > "/tmp/service-backup-$(date +%Y%m%d-%H%M%S).yaml"
    
    # Atualizar seletor de serviço para apontar ao novo ambiente
    kubectl patch service app-service-active -p '{"spec":{"selector":{"environment":"'$target_env'"}}}'
    
    # Verificar a troca
    sleep 10
    local new_active_env=$(get_current_env)
    if [ "$new_active_env" = "$target_env" ]; then
        log "Tráfego trocado com sucesso para o ambiente $target_env"
    else
        error "Falha ao trocar tráfego para o ambiente $target_env"
    fi
    
    # Aguardar propagação de alterações no load balancer
    log "Aguardando propagação de alterações no load balancer (30 segundos)..."
    sleep 30
    
    # Verificar se tráfego externo está fluindo para novo ambiente
    local attempts=0
    local max_attempts=5
    while [ $attempts -lt $max_attempts ]; do
        local version=$(curl -s $VERSION_URL | jq -r '.version // "unknown"' 2>/dev/null || echo "unknown")
        if [ "$version" != "unknown" ]; then
            log "Verificação de tráfego externo bem-sucedida - Versão: $version"
            break
        fi
        attempts=$((attempts + 1))
        sleep 10
    done
}

# Rollback para ambiente anterior
rollback() {
    local current_env=$(get_current_env)
    local previous_env=$(get_inactive_env "$current_env")
    
    warn "Iniciando rollback de $current_env para $previous_env"
    
    # Verificar se ambiente anterior está saudável
    health_check "$previous_env"
    
    # Trocar tráfego de volta
    switch_traffic "$previous_env"
    
    log "Rollback concluído com sucesso"
}

# Monitorar deployment
monitor_deployment() {
    local duration=${1:-300}  # Padrão 5 minutos
    local start_time=$(date +%s)
    local end_time=$((start_time + duration))
    
    log "Monitorando deployment por ${duration} segundos..."
    
    while [ $(date +%s) -lt $end_time ]; do
        local health_status=$(curl -s $HEALTH_CHECK_URL | jq -r '.status // "unknown"' 2>/dev/null || echo "unknown")
        local version=$(curl -s $VERSION_URL | jq -r '.version // "unknown"' 2>/dev/null || echo "unknown")
        
        echo "$(date '+%H:%M:%S') - Saúde: $health_status, Versão: $version"
        
        # Verificar problemas críticos
        if [ "$health_status" = "unhealthy" ]; then
            error "Aplicação ficou não-saudável durante o período de monitoramento"
        fi
        
        sleep 30
    done
    
    log "Monitoramento concluído com sucesso"
}

# Processo completo de blue-green deployment
deploy() {
    local version=$1
    
    if [ -z "$version" ]; then
        error "Parâmetro de versão é obrigatório"
    fi
    
    log "Iniciando blue-green deployment para versão $version"
    
    # Etapa 1: Deploy para ambiente inativo
    deploy_to_inactive "$version"
    
    # Etapa 2: Health check para ambiente inativo
    local current_env=$(get_current_env)
    local inactive_env=$(get_inactive_env "$current_env")
    health_check "$inactive_env"
    
    # Etapa 3: Executar smoke tests
    run_smoke_tests "$inactive_env"
    
    # Etapa 4: Trocar tráfego
    switch_traffic "$inactive_env"
    
    # Etapa 5: Monitorar novo deployment
    monitor_deployment 300
    
    log "Blue-green deployment concluído com sucesso"
    log "Novo ambiente ativo: $inactive_env"
    log "Versão deployada: $version"
}

# Lógica principal do script
case "${1:-deploy}" in
    "setup")
        log "Configurando infraestrutura de blue-green deployment..."
        kubectl apply -f k8s/blue-green/
        log "Configuração de infraestrutura blue-green concluída"
        ;;
    "deploy")
        deploy "${2:-latest}"
        ;;
    "switch")
        local target_env="${2:-$(get_inactive_env $(get_current_env))}"
        switch_traffic "$target_env"
        ;;
    "rollback")
        rollback
        ;;
    "status")
        local current_env=$(get_current_env)
        local inactive_env=$(get_inactive_env "$current_env")
        
        echo "=== Status do Blue-Green Deployment ==="
        echo "Ambiente ativo atual: $current_env"
        echo "Ambiente inativo: $inactive_env"
        echo ""
        echo "=== Detalhes do Ambiente ==="
        kubectl get deployments -l app=myapp
        echo ""
        kubectl get services -l app=myapp
        echo ""
        echo "=== Status de Saúde ==="
        curl -s $HEALTH_CHECK_URL 2>/dev/null | jq . || echo "Health check indisponível"
        ;;
    "monitor")
        monitor_deployment "${2:-300}"
        ;;
    *)
        echo "Uso: $0 {setup|deploy|switch|rollback|status|monitor}"
        echo ""
        echo "Comandos:"
        echo "  setup                 - Inicializar infraestrutura blue-green"
        echo "  deploy <version>      - Fazer deploy de nova versão usando estratégia blue-green"
        echo "  switch [environment]  - Trocar tráfego entre ambientes"
        echo "  rollback             - Reverter para ambiente anterior"
        echo "  status               - Exibir status atual do deployment"
        echo "  monitor [duration]   - Monitorar deployment pela duração especificada"
        exit 1
        ;;
esac
```

### 4. **Gerenciamento de Configuração**

#### Configuração de Ambiente
```bash
# config.sh
#!/bin/bash

# Configuração da aplicação
APP_NAME="myapp"
APP_URL="https://api.example.com"
NAMESPACE="default"

# Registro de container
REGISTRY="your-registry.com"
REPOSITORY="myapp"

# Configuração de health check
HEALTH_CHECK_TIMEOUT=30
READY_CHECK_TIMEOUT=10
DEPLOYMENT_TIMEOUT=600

# Configuração de monitoramento
MONITORING_DURATION=300
SMOKE_TEST_TIMEOUT=60

# Configuração de notificação
SLACK_WEBHOOK_URL="${SLACK_WEBHOOK_URL:-}"
EMAIL_NOTIFICATIONS="${EMAIL_NOTIFICATIONS:-false}"

# Configuração de banco de dados (se aplicável)
DB_MIGRATION_STRATEGY="${DB_MIGRATION_STRATEGY:-forward-only}"
DB_BACKUP_BEFORE_DEPLOY="${DB_BACKUP_BEFORE_DEPLOY:-true}"
```

### 5. **Recursos Avançados**

#### Integração Canary
```yaml
# canary-service.yaml - Para releases canary dentro de blue-green
apiVersion: v1
kind: Service
metadata:
  name: app-service-canary
  labels:
    app: myapp
    environment: canary
spec:
  selector:
    app: myapp
    environment: green  # Rotear pequena porcentagem para green
  ports:
    - port: 80
      targetPort: 3000
  type: ClusterIP

---
# Ingress com divisão de tráfego
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    nginx.ingress.kubernetes.io/canary: "true"
    nginx.ingress.kubernetes.io/canary-weight: "10"  # 10% para canary
    nginx.ingress.kubernetes.io/canary-by-header: "X-Canary"
    nginx.ingress.kubernetes.io/canary-by-header-value: "true"
spec:
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service-canary
            port:
              number: 80
```

#### Estratégia de Migração de Banco de Dados
```bash
#!/bin/bash
# db-migration-strategy.sh

handle_database_migrations() {
    local version=$1
    local target_env=$2
    
    log "Tratando migrações de banco de dados para versão $version"
    
    case "$DB_MIGRATION_STRATEGY" in
        "forward-only")
            # Apenas rodar migrações forward, seguro para blue-green
            run_forward_migrations "$version"
            ;;
        "blue-green-safe")
            # Usar views/aliases de banco de dados para compatibilidade reversa
            setup_db_compatibility_layer "$version"
            run_forward_migrations "$version"
            ;;
        "separate-db")
            # Cada ambiente tem seu próprio banco de dados
            migrate_environment_database "$target_env" "$version"
            ;;
        "shared-compatible")
            # Garantir que migrações sejam compatíveis em reversa
            validate_migration_compatibility "$version"
            run_forward_migrations "$version"
            ;;
        *)
            warn "Estratégia desconhecida de migração de banco de dados: $DB_MIGRATION_STRATEGY"
            ;;
    esac
}

run_forward_migrations() {
    local version=$1
    
    # Backup de banco de dados antes das migrações
    if [ "$DB_BACKUP_BEFORE_DEPLOY" = "true" ]; then
        backup_database "pre-migration-$version-$(date +%Y%m%d-%H%M%S)"
    fi
    
    # Rodar migrações
    kubectl run migration-job-$version \
        --image=myapp:$version \
        --restart=Never \
        --command -- /bin/sh -c "npm run migrate"
    
    # Aguardar conclusão da migração
    kubectl wait --for=condition=complete job/migration-job-$version --timeout=300s
    
    # Verificar sucesso da migração
    local exit_code=$(kubectl get job migration-job-$version -o jsonpath='{.status.conditions[?(@.type=="Complete")].status}')
    if [ "$exit_code" != "True" ]; then
        error "Migração de banco de dados falhou"
    fi
    
    log "Migrações de banco de dados concluídas com sucesso"
}
```

#### Integração de Monitoramento
```yaml
# monitoring/prometheus-rules.yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: blue-green-deployment-rules
spec:
  groups:
  - name: blue-green-deployment
    rules:
    - alert: BlueGreenEnvironmentDown
      expr: up{job="myapp", environment=~"blue|green"} == 0
      for: 1m
      labels:
        severity: critical
      annotations:
        summary: "Ambiente blue-green {{ $labels.environment }} está inativo"
        description: "Ambiente {{ $labels.environment }} está inativo há mais de 1 minuto"
    
    - alert: BlueGreenHighErrorRate
      expr: rate(http_requests_total{job="myapp", status=~"5.."}[5m]) > 0.1
      for: 2m
      labels:
        severity: warning
      annotations:
        summary: "Taxa de erro alta detectada durante blue-green deployment"
        description: "Taxa de erro é {{ $value }} erros por segundo"
    
    - alert: BlueGreenDeploymentStuck
      expr: time() - kube_deployment_status_observed_generation{deployment=~"app-blue|app-green"} > 600
      for: 5m
      labels:
        severity: warning
      annotations:
        summary: "Blue-green deployment parece travado"
        description: "Deployment {{ $labels.deployment }} não atualizou há mais de 10 minutos"
```

Este sistema de blue-green deployment oferece deployments sem tempo de inatividade com validação e monitoramento abrangentes e recursos de rollback. A implementação suporta múltiplas plataformas (Kubernetes, Docker Swarm, deployments tradicionais) e inclui recursos avançados como tratamento de migração de banco de dados e releases canary.