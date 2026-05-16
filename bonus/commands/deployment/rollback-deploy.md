---
allowed-tools: Read, Edit, Bash
argument-hint: [versão-alvo] | --previous | --emergency | --validate-first | --with-db
description: Fazer rollback da implantação para versão anterior com verificações de segurança, considerações de banco de dados e monitoramento
---

# Rollback de Implantação

Fazer rollback da implantação para versão anterior: $ARGUMENTS

## Estado Atual da Implantação

- Versão atual: !`curl -s https://api.example.com/version 2>/dev/null || kubectl get deployments -o wide 2>/dev/null | head -3 || echo "Detecção de versão necessária"`
- Versões disponíveis: !`git tag --sort=-version:refname | head -5`
- Status do container: !`docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}" 2>/dev/null | head -5 || echo "Sem containers"`
- Implantações K8s: !`kubectl get deployments 2>/dev/null || echo "Sem acesso K8s"`
- Status de integridade: !`curl -sf https://api.example.com/health 2>/dev/null && echo "✅ Íntegro" || echo "❌ Com problemas"`

## Protocolo de Rollback de Emergência

Procedimento sistemático de rollback: $ARGUMENTS

1. **Avaliação de Incidente e Tomada de Decisão**
   - Avaliar a severidade e impacto dos problemas atuais de implantação
   - Determinar se rollback é necessário ou se correção forward é melhor
   - Identificar sistemas, usuários e funções de negócio afetados
   - Considerar implicações de integridade e consistência de dados
   - Documentar a justificativa e cronograma da decisão

2. **Configuração de Resposta de Emergência**
   ```bash
   # Ativar time de resposta a incidentes
   # Configurar canais de comunicação
   # Notificar stakeholders imediatamente
   
   # Exemplo de notificação de emergência
   echo "🚨 ROLLBACK INICIADO
   Problema: Degradação crítica de desempenho após implantação v1.3.0
   Ação: Fazendo rollback para v1.2.9
   ETA: 15 minutos
   Impacto: Possível interrupção temporária do serviço
   Canal de status: #incident-rollback-202401"
   ```

3. **Verificações de Segurança Pré-Rollback**
   ```bash
   # Verificar versão de produção atual
   curl -s https://api.example.com/version
   kubectl get deployments -o wide
   
   # Verificar status do sistema
   curl -s https://api.example.com/health | jq .
   
   # Identificar versão alvo de rollback
   git tag --sort=-version:refname | head -5
   
   # Verificar se versão alvo existe e é implantável
   git show v1.2.9 --stat
   ```

4. **Considerações de Banco de Dados**
   ```bash
   # Verificar migrações de banco de dados desde última versão
   ./check-migrations.sh v1.2.9 v1.3.0
   
   # Se migrações existirem, planejar rollback de banco de dados
   # AVISO: Rollbacks de banco de dados podem causar perda de dados
   # Considere correção forward se migrações estiverem presentes
   
   # Criar backup de banco de dados antes do rollback
   ./backup-database.sh "pre-rollback-$(date +%Y%m%d-%H%M%S)"
   ```

5. **Preparação de Gerenciamento de Tráfego**
   ```bash
   # Preparar para redirecionar tráfego
   # Opção 1: Página de manutenção
   ./enable-maintenance-mode.sh
   
   # Opção 2: Gerenciamento de load balancer
   ./drain-traffic.sh --gradual
   
   # Opção 3: Ativação de circuit breaker
   ./activate-circuit-breaker.sh
   ```

6. **Rollback de Container/Kubernetes**
   ```bash
   # Rollback Kubernetes
   kubectl rollout history deployment/app-deployment
   kubectl rollout undo deployment/app-deployment
   
   # Ou fazer rollback para revisão específica
   kubectl rollout undo deployment/app-deployment --to-revision=3
   
   # Monitorar progresso de rollback
   kubectl rollout status deployment/app-deployment --timeout=300s
   
   # Verificar se pods estão rodando
   kubectl get pods -l app=your-app
   ```

7. **Rollback Docker Swarm**
   ```bash
   # Listar histórico de serviço
   docker service ps app-service --no-trunc
   
   # Fazer rollback para versão anterior
   docker service update --rollback app-service
   
   # Ou atualizar para imagem específica
   docker service update --image app:v1.2.9 app-service
   
   # Monitorar rollback
   docker service ps app-service
   ```

8. **Rollback de Implantação Tradicional**
   ```bash
   # Rollback de implantação Blue-Green
   ./switch-to-blue.sh  # ou green, dependendo da atual
   
   # Rollback de implantação rolling
   ./deploy-version.sh v1.2.9 --rolling
   
   # Rollback baseado em symlink
   ln -sfn /releases/v1.2.9 /current
   sudo systemctl restart app-service
   ```

9. **Atualizações de Load Balancer e CDN**
   ```bash
   # Atualizar load balancer para apontar para versão anterior
   aws elbv2 modify-target-group --target-group-arn $TG_ARN --targets Id=old-instance
   
   # Limpar cache de CDN se necessário
   aws cloudfront create-invalidation --distribution-id $DIST_ID --paths "/*"
   
   # Atualizar DNS se necessário (último recurso, tem atraso de propagação)
   # aws route53 change-resource-record-sets ...
   ```

10. **Rollback de Configuração**
    ```bash
    # Fazer rollback de arquivos de configuração
    git checkout v1.2.9 -- config/
    
    # Reiniciar serviços com configuração antiga
    sudo systemctl restart nginx
    sudo systemctl restart app-service
    
    # Fazer rollback de variáveis de ambiente
    ./restore-env-vars.sh v1.2.9
    
    # Atualizar feature flags
    ./update-feature-flags.sh --disable-new-features
    ```

11. **Rollback de Banco de Dados (se necessário)**
    ```sql
    -- EXTREMA CAUTELA: Pode causar perda de dados
    
    -- Verificar status de migração
    SELECT * FROM schema_migrations ORDER BY version DESC LIMIT 5;
    
    -- Fazer rollback de migrações específicas (depende do framework)
    -- Rails: rake db:migrate:down VERSION=20240115120000
    -- Django: python manage.py migrate app_name 0001
    -- Node.js: npm run migrate:down
    
    -- Verificar estado do banco de dados
    SHOW TABLES;
    DESCRIBE critical_table;
    ```

12. **Validação de Integridade do Serviço**
    ```bash
    # Script de verificação de integridade
    #!/bin/bash
    
    echo "Validando rollback..."
    
    # Verificar integridade da aplicação
    if curl -f -s https://api.example.com/health > /dev/null; then
        echo "✅ Verificação de integridade passou"
    else
        echo "❌ Verificação de integridade falhou"
        exit 1
    fi
    
    # Verificar endpoints críticos
    endpoints=(\n        "/api/users/me"\n        "/api/auth/status"\n        "/api/data/latest"\n    )
    
    for endpoint in "${endpoints[@]}"; do
        if curl -f -s "https://api.example.com$endpoint" > /dev/null; then
            echo "✅ $endpoint funcionando"
        else
            echo "❌ $endpoint falhou"
        fi
    done
    ```

13. **Validação de Desempenho e Métricas**
    ```bash
    # Verificar tempos de resposta
    curl -w "Tempo de resposta: %{time_total}s\n" -s -o /dev/null https://api.example.com/
    
    # Monitorar taxa de erros
    tail -f /var/log/app/error.log | head -20
    
    # Verificar recursos do sistema
    top -bn1 | head -10
    free -h
    df -h
    
    # Validar conectividade de banco de dados
    mysql -u app -p -e "SELECT 1;"
    ```

14. **Restauração de Tráfego**
    ```bash
    # Restaurar tráfego gradualmente
    ./restore-traffic.sh --gradual
    
    # Desabilitar modo de manutenção
    ./disable-maintenance-mode.sh
    
    # Reativar circuit breakers
    ./deactivate-circuit-breaker.sh
    
    # Monitorar padrões de tráfego
    ./monitor-traffic.sh --duration 300
    ```

15. **Monitoramento e Alertas**
    ```bash
    # Ativar monitoramento aprimorado durante rollback
    ./enable-enhanced-monitoring.sh
    
    # Observar métricas-chave
    watch -n 10 'curl -s https://api.example.com/metrics | jq .'
    
    # Monitorar logs em tempo real
    tail -f /var/log/app/*.log | grep -E "ERROR|WARN|EXCEPTION"
    
    # Verificar métricas da aplicação
    # - Tempos de resposta
    # - Taxas de erro
    # - Sessões de usuário
    # - Desempenho de banco de dados
    ```

16. **Comunicação com Usuários**
    ```markdown
    ## Atualização de Serviço - Rollback Concluído
    
    **Status:** ✅ Serviço Restaurado
    **Hora:** 15 de janeiro de 2024 às 15:45 UTC
    **Duração:** 12 minutos de desempenho degradado
    
    **O que Aconteceu:**
    Identificamos problemas de desempenho na versão mais recente e 
    realizamos um rollback para garantir qualidade ótima do serviço.
    
    **Status Atual:**
    - Todos os serviços operando normalmente
    - Métricas de desempenho voltadas à linha de base
    - Nenhuma perda de dados ocorreu
    
    **Próximos Passos:**
    Estamos investigando a causa raiz e forneceremos atualizações 
    em nossa página de status.
    ```

17. **Validação Pós-Rollback**
    ```bash
    # Período de monitoramento estendido
    ./monitor-extended.sh --duration 3600  # 1 hora
    
    # Executar testes de integração
    npm run test:integration:production
    
    # Verificar problemas reportados por usuários
    ./check-support-tickets.sh --since "1 hour ago"
    
    # Validar métricas de negócio
    ./check-business-metrics.sh
    ```

18. **Documentação e Relatório**
    ```markdown
    # Relatório de Incidente de Rollback
    
    **ID do Incidente:** INC-2024-0115-001
    **Versão de Rollback:** v1.2.9 (de v1.3.0)
    **Horário de Início:** 15 de janeiro de 2024 às 15:30 UTC
    **Horário de Término:** 15 de janeiro de 2024 às 15:42 UTC
    **Duração Total:** 12 minutos
    
    **Cronograma:**
    - 15:25 - Degradação de desempenho detectada
    - 15:30 - Decisão de rollback tomada
    - 15:32 - Tráfego drenado
    - 15:35 - Rollback iniciado
    - 15:38 - Rollback concluído
    - 15:42 - Tráfego totalmente restaurado
    
    **Impacto:**
    - 12 minutos de desempenho degradado
    - ~5% dos usuários experimentaram respostas lentas
    - Nenhuma perda ou corrupção de dados
    - Nenhuma implicação de segurança
    
    **Causa Raiz:**
    Vazamento de memória em novo recurso causando degradação de desempenho
    
    **Lições Aprendidas:**
    - Necessário teste de desempenho melhor em staging
    - Melhorar monitoramento de uso de memória
    - Considerar implantações canary para grandes releases
    ```

19. **Limpeza e Acompanhamento**
    ```bash
    # Limpar artefatos de implantação falha
    docker image rm app:v1.3.0
    
    # Atualizar status de implantação
    ./update-deployment-status.sh "rollback-completed"
    
    # Resetar feature flags se necessário
    ./reset-feature-flags.sh
    
    # Agendar revisão pós-incidente
    ./schedule-postmortem.sh --date "2024-01-16 10:00"
    ```

20. **Prevenção e Melhoria**
    - Analisar o que deu errado na implantação
    - Melhorar procedimentos de teste e validação
    - Aprimorar monitoramento e alertas
    - Atualizar procedimentos de rollback com base em aprendizados
    - Considerar implementar implantações canary

**Matriz de Decisão de Rollback:**

| Severidade do Problema | Impacto de Dados | Tempo para Correção | Decisão |
|----------------------|------------------|-------------------|---------|
| Crítica | Nenhum | > 30 min | Rollback |
| Alta | Menor | > 60 min | Rollback |
| Média | Nenhum | > 2 horas | Considerar rollback |
| Baixa | Nenhum | Qualquer | Correção forward |

**Template de Script de Rollback de Emergência:**
```bash
#!/bin/bash
set -e

# Script de rollback de emergência
PREVIOUS_VERSION="${1:-v1.2.9}"
CURRENT_VERSION=$(curl -s https://api.example.com/version)

echo "🚨 ROLLBACK DE EMERGÊNCIA"
echo "De: $CURRENT_VERSION"
echo "Para: $PREVIOUS_VERSION"
echo ""

# Confirmar rollback
read -p "Proceder com rollback? (sim/não): " confirm
if [ "$confirm" != "sim" ]; then
    echo "Rollback cancelado"
    exit 1
fi

# Executar rollback
echo "Iniciando rollback..."
kubectl set image deployment/app-deployment app=app:$PREVIOUS_VERSION
kubectl rollout status deployment/app-deployment --timeout=300s

# Validar
echo "Validando rollback..."
sleep 30
curl -f https://api.example.com/health

echo "✅ Rollback concluído com sucesso"
```

Lembre-se: Rollbacks devem ser o último recurso. Sempre considere correções forward primeiro, especialmente quando migrações de banco de dados estiverem envolvidas.