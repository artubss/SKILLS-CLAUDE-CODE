---
allowed-tools: Ler, Escrever, Editar, Bash
argument-hint: [tipo-monitoramento] | setup | dashboard | alertas | métricas | saúde | performance
description: Monitoramento abrangente de implantações com observabilidade, alertas, verificações de saúde e rastreamento de desempenho
---

# Monitoramento de Implantações & Observabilidade

Configure monitoramento abrangente de implantações: $ARGUMENTS

## Estado Atual de Monitoramento

- Monitoramento existente: !`kubectl get pods -n monitoring 2>/dev/null || docker ps | grep -E "(prometheus|grafana|jaeger)" || echo "Nenhum monitoramento detectado"`
- Endpoints de saúde: !`curl -s https://api.example.com/health 2>/dev/null | jq -r '.status // "Unknown"' || echo "Endpoint de saúde necessário"`
- Exposição de métricas: !`curl -s https://api.example.com/metrics 2>/dev/null | head -5 || echo "Endpoint de métricas necessário"`
- Agregação de logs: !`kubectl get pods -n logging 2>/dev/null || echo "Configuração de agregação de logs necessária"`
- Integração de APM: Verificar configuração de monitoramento de performance de aplicação

## Tarefa

Implemente monitoramento abrangente e observabilidade para implantações com insights em tempo real, alertas e capacidades de resposta automatizada.

## Arquitetura de Monitoramento

### 1. **Stack de Monitoramento Principal**

#### Configuração do Prometheus
```yaml
# prometheus-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-config
  namespace: monitoring
data:
  prometheus.yml: |
    global:
      scrape_interval: 15s
      evaluation_interval: 15s
      
    rule_files:
      - "/etc/prometheus/rules/*.yml"
      
    scrape_configs:
      # Métricas de aplicação
      - job_name: 'myapp'
        kubernetes_sd_configs:
          - role: pod
        relabel_configs:
          - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
            action: keep
            regex: true
          - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
            action: replace
            target_label: __metrics_path__
            regex: (.+)
          - source_labels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
            action: replace
            regex: ([^:]+)(?::\d+)?;(\d+)
            replacement: $1:$2
            target_label: __address__
          - action: labelmap
            regex: __meta_kubernetes_pod_label_(.+)
            
      # Métricas de cluster Kubernetes
      - job_name: 'kubernetes-pods'
        kubernetes_sd_configs:
          - role: pod
        relabel_configs:
          - source_labels: [__meta_kubernetes_pod_phase]
            action: keep
            regex: Running
            
      # Node exporter para métricas de infraestrutura
      - job_name: 'node-exporter'
        kubernetes_sd_configs:
          - role: endpoints
        relabel_configs:
          - source_labels: [__meta_kubernetes_endpoints_name]
            action: keep
            regex: node-exporter
            
      # Métricas específicas de implantação
      - job_name: 'deployment-metrics'
        static_configs:
          - targets: ['deployment-exporter:9090']
        metrics_path: /metrics
        scrape_interval: 30s

    alerting:
      alertmanagers:
        - static_configs:
            - targets: ['alertmanager:9093']

---
# Implantação do Prometheus
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus
  namespace: monitoring
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus
  template:
    metadata:
      labels:
        app: prometheus
    spec:
      serviceAccountName: prometheus
      containers:
      - name: prometheus
        image: prom/prometheus:v2.40.0
        args:
          - '--config.file=/etc/prometheus/prometheus.yml'
          - '--storage.tsdb.path=/prometheus'
          - '--web.console.libraries=/etc/prometheus/console_libraries'
          - '--web.console.templates=/etc/prometheus/consoles'
          - '--storage.tsdb.retention.time=30d'
          - '--web.enable-lifecycle'
          - '--web.enable-admin-api'
        ports:
        - containerPort: 9090
        volumeMounts:
        - name: prometheus-config
          mountPath: /etc/prometheus
        - name: prometheus-storage
          mountPath: /prometheus
        resources:
          requests:
            memory: "512Mi"
            cpu: "250m"
          limits:
            memory: "2Gi"
            cpu: "1000m"
      volumes:
      - name: prometheus-config
        configMap:
          name: prometheus-config
      - name: prometheus-storage
        persistentVolumeClaim:
          claimName: prometheus-pvc
```

#### Configuração de Dashboard do Grafana
```yaml
# grafana-dashboard-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: deployment-dashboard
  namespace: monitoring
data:
  deployment-monitoring.json: |
    {
      "dashboard": {
        "id": null,
        "title": "Dashboard de Monitoramento de Implantações",
        "tags": ["deployment", "monitoring"],
        "timezone": "browser",
        "panels": [
          {
            "id": 1,
            "title": "Status de Implantação",
            "type": "stat",
            "targets": [
              {
                "expr": "up{job=\"myapp\"}",
                "legendFormat": "{{instance}}"
              }
            ],
            "fieldConfig": {
              "defaults": {
                "thresholds": {
                  "steps": [
                    {"color": "red", "value": 0},
                    {"color": "green", "value": 1}
                  ]
                }
              }
            }
          },
          {
            "id": 2,
            "title": "Taxa de Requisições",
            "type": "graph",
            "targets": [
              {
                "expr": "rate(http_requests_total[5m])",
                "legendFormat": "{{method}} {{status}}"
              }
            ]
          },
          {
            "id": 3,
            "title": "Taxa de Erros",
            "type": "graph",
            "targets": [
              {
                "expr": "rate(http_requests_total{status=~\"5..\"}[5m]) / rate(http_requests_total[5m]) * 100",
                "legendFormat": "Taxa de Erros %"
              }
            ]
          },
          {
            "id": 4,
            "title": "Tempo de Resposta",
            "type": "graph",
            "targets": [
              {
                "expr": "histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))",
                "legendFormat": "95º percentil"
              },
              {
                "expr": "histogram_quantile(0.50, rate(http_request_duration_seconds_bucket[5m]))",
                "legendFormat": "50º percentil"
              }
            ]
          },
          {
            "id": 5,
            "title": "Uso de Recursos do Pod",
            "type": "graph",
            "targets": [
              {
                "expr": "rate(container_cpu_usage_seconds_total{pod=~\"myapp-.*\"}[5m]) * 100",
                "legendFormat": "Uso de CPU - {{pod}}"
              },
              {
                "expr": "container_memory_usage_bytes{pod=~\"myapp-.*\"} / 1024 / 1024",
                "legendFormat": "Uso de Memória MB - {{pod}}"
              }
            ]
          },
          {
            "id": 6,
            "title": "Eventos de Implantação",
            "type": "logs",
            "targets": [
              {
                "expr": "{job=\"kubernetes-events\"} |= \"myapp\"",
                "legendFormat": ""
              }
            ]
          }
        ],
        "time": {
          "from": "now-1h",
          "to": "now"
        },
        "refresh": "30s"
      }
    }
```

### 2. **Monitoramento de Saúde da Aplicação**

#### Implementação de Verificação de Saúde
```javascript
// health-check.js - Endpoint de saúde da aplicação
const express = require('express');
const { promisify } = require('util');

class HealthMonitor {
  constructor() {
    this.checks = new Map();
    this.status = 'healthy';
    this.lastCheck = new Date();
  }

  registerCheck(name, checkFunction, options = {}) {
    this.checks.set(name, {
      check: checkFunction,
      timeout: options.timeout || 5000,
      critical: options.critical || false,
      lastStatus: null,
      lastCheck: null,
      errorCount: 0
    });
  }

  async runHealthChecks() {
    const results = {};
    let overallHealthy = true;
    
    for (const [name, config] of this.checks) {
      try {
        const startTime = Date.now();
        const result = await Promise.race([
          config.check(),
          new Promise((_, reject) => 
            setTimeout(() => reject(new Error('Health check timeout')), config.timeout)
          )
        ]);
        
        const duration = Date.now() - startTime;
        
        results[name] = {
          status: 'healthy',
          duration,
          details: result,
          lastCheck: new Date().toISOString()
        };
        
        config.lastStatus = 'healthy';
        config.errorCount = 0;
      } catch (error) {
        results[name] = {
          status: 'unhealthy',
          error: error.message,
          lastCheck: new Date().toISOString()
        };
        
        config.lastStatus = 'unhealthy';
        config.errorCount++;
        
        if (config.critical) {
          overallHealthy = false;
        }
      }
      
      config.lastCheck = new Date();
    }
    
    this.status = overallHealthy ? 'healthy' : 'unhealthy';
    this.lastCheck = new Date();
    
    return {
      status: this.status,
      timestamp: this.lastCheck.toISOString(),
      checks: results,
      uptime: process.uptime(),
      version: process.env.APP_VERSION || 'unknown'
    };
  }

  setupEndpoints(app) {
    // Probe de vivacidade - saúde básica da aplicação
    app.get('/health', async (req, res) => {
      const health = await this.runHealthChecks();
      const statusCode = health.status === 'healthy' ? 200 : 503;
      res.status(statusCode).json(health);
    });

    // Probe de prontidão - pronto para receber tráfego
    app.get('/ready', async (req, res) => {
      const health = await this.runHealthChecks();
      
      // Verificações adicionais de prontidão
      const readinessChecks = {
        memoryUsage: process.memoryUsage().heapUsed / process.memoryUsage().heapTotal < 0.9,
        activeConnections: true, // Verificar conexões ativas se aplicável
      };
      
      const isReady = health.status === 'healthy' && 
                     Object.values(readinessChecks).every(check => check);
      
      res.status(isReady ? 200 : 503).json({
        ...health,
        ready: isReady,
        readinessChecks
      });
    });

    // Probe de inicialização - aplicação iniciou
    app.get('/startup', (req, res) => {
      res.status(200).json({
        status: 'started',
        timestamp: new Date().toISOString(),
        pid: process.pid,
        uptime: process.uptime()
      });
    });
  }
}

// Exemplo de uso
const healthMonitor = new HealthMonitor();

// Registrar verificações de saúde
healthMonitor.registerCheck('database', async () => {
  // Verificação de conectividade de banco de dados
  await db.query('SELECT 1');
  return { connected: true };
}, { critical: true, timeout: 3000 });

healthMonitor.registerCheck('redis', async () => {
  // Verificação de conectividade Redis
  await redis.ping();
  return { connected: true };
}, { critical: false, timeout: 2000 });

healthMonitor.registerCheck('external-api', async () => {
  // Verificação de serviço externo
  const response = await fetch('https://api.external-service.com/health');
  return { status: response.status, healthy: response.ok };
}, { critical: false, timeout: 5000 });

module.exports = healthMonitor;
```

### 3. **Métricas Customizadas e Instrumentação**

#### Métricas da Aplicação
```javascript
// metrics.js - Coleta de métricas da aplicação
const promClient = require('prom-client');

class DeploymentMetrics {
  constructor() {
    // Métricas padrão
    promClient.collectDefaultMetrics({
      prefix: 'myapp_',
      timeout: 5000,
    });

    // Métricas customizadas de implantação
    this.deploymentInfo = new promClient.Gauge({
      name: 'myapp_deployment_info',
      help: 'Informações de implantação',
      labelNames: ['version', 'environment', 'commit_sha']
    });

    this.httpRequestsTotal = new promClient.Counter({
      name: 'myapp_http_requests_total',
      help: 'Total de requisições HTTP',
      labelNames: ['method', 'status_code', 'route']
    });

    this.httpRequestDuration = new promClient.Histogram({
      name: 'myapp_http_request_duration_seconds',
      help: 'Duração de requisições HTTP em segundos',
      labelNames: ['method', 'status_code', 'route'],
      buckets: [0.1, 0.5, 1, 2, 5]
    });

    this.activeConnections = new promClient.Gauge({
      name: 'myapp_active_connections',
      help: 'Número de conexões ativas'
    });

    this.deploymentEvents = new promClient.Counter({
      name: 'myapp_deployment_events_total',
      help: 'Eventos de implantação',
      labelNames: ['event_type', 'status']
    });

    this.healthCheckStatus = new promClient.Gauge({
      name: 'myapp_health_check_status',
      help: 'Status da verificação de saúde (1 = saudável, 0 = não saudável)',
      labelNames: ['check_name']
    });

    // Métricas de negócio
    this.businessMetrics = {
      activeUsers: new promClient.Gauge({
        name: 'myapp_active_users',
        help: 'Número de usuários ativos'
      }),
      
      transactionsTotal: new promClient.Counter({
        name: 'myapp_transactions_total',
        help: 'Total de transações processadas',
        labelNames: ['type', 'status']
      }),
      
      errorRate: new promClient.Gauge({
        name: 'myapp_error_rate',
        help: 'Taxa de erro da aplicação em porcentagem'
      })
    };

    this.initializeMetrics();
  }

  initializeMetrics() {
    // Definir informações de implantação
    this.deploymentInfo.set({
      version: process.env.APP_VERSION || 'unknown',
      environment: process.env.NODE_ENV || 'development',
      commit_sha: process.env.GIT_COMMIT_SHA || 'unknown'
    }, 1);
  }

  recordHttpRequest(req, res, duration) {
    const labels = {
      method: req.method,
      status_code: res.statusCode,
      route: req.route?.path || req.path
    };

    this.httpRequestsTotal.inc(labels);
    this.httpRequestDuration.observe(labels, duration);
  }

  recordDeploymentEvent(eventType, status) {
    this.deploymentEvents.inc({
      event_type: eventType,
      status: status
    });
  }

  updateHealthCheckStatus(checkName, isHealthy) {
    this.healthCheckStatus.set(
      { check_name: checkName },
      isHealthy ? 1 : 0
    );
  }

  updateActiveConnections(count) {
    this.activeConnections.set(count);
  }

  // Middleware para Express.js
  expressMiddleware() {
    return (req, res, next) => {
      const start = Date.now();
      
      res.on('finish', () => {
        const duration = (Date.now() - start) / 1000;
        this.recordHttpRequest(req, res, duration);
      });
      
      next();
    };
  }

  // Handler do endpoint de métricas
  getMetricsHandler() {
    return async (req, res) => {
      res.set('Content-Type', promClient.register.contentType);
      const metrics = await promClient.register.metrics();
      res.end(metrics);
    };
  }
}

module.exports = DeploymentMetrics;
```

### 4. **Configuração de Alertas**

#### Configuração do Alertmanager
```yaml
# alertmanager-config.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: alertmanager-config
  namespace: monitoring
data:
  alertmanager.yml: |
    global:
      smtp_smarthost: 'smtp.gmail.com:587'
      smtp_from: 'alerts@example.com'
      smtp_auth_username: 'alerts@example.com'
      smtp_auth_password: 'password'
      
    route:
      group_by: ['alertname', 'environment']
      group_wait: 10s
      group_interval: 10s
      repeat_interval: 1h
      receiver: 'default'
      routes:
      - match:
          severity: critical
        receiver: 'critical-alerts'
        continue: true
      - match:
          alertname: DeploymentFailed
        receiver: 'deployment-alerts'
        continue: true
      - match:
          service: myapp
        receiver: 'app-alerts'
        
    receivers:
    - name: 'default'
      slack_configs:
      - api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
        channel: '#monitoring'
        title: 'Alerta: {{ range .Alerts }}{{ .Annotations.summary }}{{ end }}'
        text: '{{ range .Alerts }}{{ .Annotations.description }}{{ end }}'
        
    - name: 'critical-alerts'
      slack_configs:
      - api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
        channel: '#critical-alerts'
        title: '🚨 CRÍTICO: {{ range .Alerts }}{{ .Annotations.summary }}{{ end }}'
        text: '{{ range .Alerts }}{{ .Annotations.description }}{{ end }}'
      email_configs:
      - to: 'oncall@example.com'
        subject: 'ALERTA CRÍTICO: {{ range .Alerts }}{{ .Annotations.summary }}{{ end }}'
        body: |
          Detalhes do Alerta:
          {{ range .Alerts }}
          - Alerta: {{ .Annotations.summary }}
          - Descrição: {{ .Annotations.description }}
          - Severidade: {{ .Labels.severity }}
          - Ambiente: {{ .Labels.environment }}
          {{ end }}
          
    - name: 'deployment-alerts'
      slack_configs:
      - api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
        channel: '#deployments'
        title: '🚀 Alerta de Implantação: {{ range .Alerts }}{{ .Annotations.summary }}{{ end }}'
        
    - name: 'app-alerts'
      slack_configs:
      - api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
        channel: '#app-monitoring'
        
    inhibit_rules:
    - source_match:
        severity: 'critical'
      target_match:
        severity: 'warning'
      equal: ['alertname', 'environment', 'service']
```

#### Regras de Alertas de Implantação
```yaml
# deployment-alert-rules.yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: deployment-monitoring-rules
  namespace: monitoring
spec:
  groups:
  - name: deployment-health
    rules:
    # Disponibilidade da aplicação
    - alert: ApplicationDown
      expr: up{job="myapp"} == 0
      for: 1m
      labels:
        severity: critical
        service: myapp
      annotations:
        summary: "Instância da aplicação está indisponível"
        description: "{{ $labels.instance }} está indisponível há mais de 1 minuto"
        runbook_url: "https://wiki.example.com/runbooks/app-down"
        
    # Taxa de erros alta
    - alert: HighErrorRate
      expr: rate(myapp_http_requests_total{status_code=~"5.."}[5m]) / rate(myapp_http_requests_total[5m]) * 100 > 5
      for: 2m
      labels:
        severity: critical
        service: myapp
      annotations:
        summary: "Taxa de erros elevada detectada"
        description: "Taxa de erros é {{ $value }}% nos últimos 5 minutos"
        
    # Tempos de resposta lentos
    - alert: SlowResponseTime
      expr: histogram_quantile(0.95, rate(myapp_http_request_duration_seconds_bucket[5m])) > 2
      for: 5m
      labels:
        severity: warning
        service: myapp
      annotations:
        summary: "Tempos de resposta lentos detectados"
        description: "Tempo de resposta do 95º percentil é {{ $value }}s"
        
    # Uso de memória elevado
    - alert: HighMemoryUsage
      expr: container_memory_usage_bytes{pod=~"myapp-.*"} / container_spec_memory_limit_bytes * 100 > 80
      for: 5m
      labels:
        severity: warning
        service: myapp
      annotations:
        summary: "Uso de memória elevado"
        description: "Uso de memória do pod {{ $labels.pod }} é {{ $value }}%"
        
    # Uso de CPU elevado
    - alert: HighCPUUsage
      expr: rate(container_cpu_usage_seconds_total{pod=~"myapp-.*"}[5m]) * 100 > 80
      for: 10m
      labels:
        severity: warning
        service: myapp
      annotations:
        summary: "Uso de CPU elevado"
        description: "Uso de CPU do pod {{ $labels.pod }} é {{ $value }}%"
        
  - name: deployment-events
    rules:
    # Implantação falhou
    - alert: DeploymentFailed
      expr: increase(kube_deployment_status_replicas_unavailable{deployment=~"myapp-.*"}[5m]) > 0
      for: 2m
      labels:
        severity: critical
        service: myapp
      annotations:
        summary: "Implantação tem pods com falha"
        description: "Implantação {{ $labels.deployment }} tem {{ $value }} réplicas indisponíveis"
        
    # Implantação travada
    - alert: DeploymentStuck
      expr: kube_deployment_spec_replicas{deployment=~"myapp-.*"} != kube_deployment_status_ready_replicas{deployment=~"myapp-.*"}
      for: 10m
      labels:
        severity: warning
        service: myapp
      annotations:
        summary: "Implantação parece travada"
        description: "Implantação {{ $labels.deployment }} está em progresso há mais de 10 minutos"
        
    # Pod em loop de reinicialização
    - alert: PodCrashLooping
      expr: rate(kube_pod_container_status_restarts_total{pod=~"myapp-.*"}[5m]) > 0.1
      for: 2m
      labels:
        severity: critical
        service: myapp
      annotations:
        summary: "Pod está reiniciando frequentemente"
        description: "Pod {{ $labels.pod }} está reiniciando com frequência"
        
  - name: business-metrics
    rules:
    # Taxa de falha de transação elevada
    - alert: HighTransactionFailureRate
      expr: rate(myapp_transactions_total{status="failed"}[5m]) / rate(myapp_transactions_total[5m]) * 100 > 1
      for: 5m
      labels:
        severity: warning
        service: myapp
      annotations:
        summary: "Taxa de falha de transação elevada"
        description: "Taxa de falha de transação é {{ $value }}%"
        
    # Usuários ativos baixos (indicador de potencial problema)
    - alert: LowActiveUsers
      expr: myapp_active_users < 10 and hour() > 8 and hour() < 18  # Durante horário comercial
      for: 15m
      labels:
        severity: warning
        service: myapp
      annotations:
        summary: "Contagem de usuários ativos inusitadamente baixa"
        description: "Apenas {{ $value }} usuários ativos durante horário comercial"
```

### 5. **Agregação e Análise de Logs**

#### Configuração do Fluentd
```yaml
# fluentd-configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluentd-config
  namespace: logging
data:
  fluent.conf: |
    <source>
      @type tail
      @id myapp_logs
      path /var/log/containers/myapp-*.log
      pos_file /var/log/fluentd-myapp.log.pos
      tag kubernetes.myapp
      format json
      time_key time
      time_format %Y-%m-%dT%H:%M:%S.%NZ
    </source>
    
    <filter kubernetes.myapp>
      @type kubernetes_metadata
      @id kubernetes_metadata
    </filter>
    
    <filter kubernetes.myapp>
      @type parser
      key_name log
      reserve_data true
      <parse>
        @type json
        time_key timestamp
        time_format %Y-%m-%dT%H:%M:%S.%L%z
      </parse>
    </filter>
    
    # Logs de eventos de implantação
    <filter kubernetes.myapp>
      @type record_transformer
      enable_ruby true
      <record>
        deployment_info ${record.dig("kubernetes", "labels", "deployment") || "unknown"}
        environment ${record.dig("kubernetes", "labels", "environment") || "unknown"}
        version ${record.dig("kubernetes", "labels", "version") || "unknown"}
        log_level ${record["level"] || "info"}
        component ${record["component"] || "application"}
      </record>
    </filter>
    
    # Alertas de logs de erro
    <filter kubernetes.myapp>
      @type grep
      <regexp>
        key log_level
        pattern /error|fatal|panic/i
      </regexp>
      <record>
        alert_type error
        needs_attention true
      </record>
    </filter>
    
    <match kubernetes.myapp>
      @type elasticsearch
      @id out_es_myapp
      hosts elasticsearch.logging.svc.cluster.local:9200
      logstash_format true
      logstash_prefix myapp-deployment
      include_tag_key true
      tag_key @log_name
      flush_interval 10s
      
      <buffer>
        @type file
        path /var/log/fluentd-buffers/myapp.buffer
        flush_mode interval
        retry_type exponential_backoff
        flush_thread_count 2
        flush_interval 5s
        retry_forever
        retry_max_interval 30
        chunk_limit_size 2M
        queue_limit_length 8
        overflow_action block
      </buffer>
    </match>
```

### 6. **Monitoramento de Performance**

#### Integração de APM com Jaeger
```javascript
// tracing.js - Configuração de rastreamento distribuído
const { NodeSDK } = require('@opentelemetry/sdk-node');
const { getNodeAutoInstrumentations } = require('@opentelemetry/auto-instrumentations-node');
const { JaegerExporter } = require('@opentelemetry/exporter-jaeger');
const { Resource } = require('@opentelemetry/resources');
const { SemanticResourceAttributes } = require('@opentelemetry/semantic-conventions');

const jaegerExporter = new JaegerExporter({
  endpoint: process.env.JAEGER_ENDPOINT || 'http://jaeger-collector:14268/api/traces',
});

const sdk = new NodeSDK({
  resource: new Resource({
    [SemanticResourceAttributes.SERVICE_NAME]: 'myapp',
    [SemanticResourceAttributes.SERVICE_VERSION]: process.env.APP_VERSION || 'unknown',
    [SemanticResourceAttributes.DEPLOYMENT_ENVIRONMENT]: process.env.NODE_ENV || 'development',
  }),
  traceExporter: jaegerExporter,
  instrumentations: [
    getNodeAutoInstrumentations({
      // Personalizar instrumentação
      '@opentelemetry/instrumentation-http': {
        requestHook: (span, request) => {
          span.setAttribute('deployment.version', process.env.APP_VERSION);
          span.setAttribute('deployment.environment', process.env.NODE_ENV);
        },
      },
    }),
  ],
});

sdk.start();

// Rastreamento customizado de implantação
const { trace, context } = require('@opentelemetry/api');

class DeploymentTracer {