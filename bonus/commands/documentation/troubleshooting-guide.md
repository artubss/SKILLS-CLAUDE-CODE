---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [system-component] | --application | --database | --network | --deployment | --comprehensive
description: Gera documentação sistemática de troubleshooting com procedimentos de diagnóstico, problemas comuns e soluções automatizadas
---

# Gerador de Guia de Troubleshooting

Gera documentação de troubleshooting: $ARGUMENTS

## Contexto Atual do Sistema

- Arquitetura do sistema: @docker-compose.yml ou @k8s/ ou detecta tipo de deployment
- Locais de logs: !`find . -name "*log*" -type d | head -3`
- Setup de monitoramento: !`grep -r "prometheus\|grafana\|datadog" . 2>/dev/null | wc -l` referências de monitoramento
- Padrões de erro: !`find . -name "*.log" | head -3` logs recentes
- Endpoints de saúde: !`grep -r "health\|status" src/ 2>/dev/null | head -3`

## Tarefa

Cria guia de troubleshooting abrangente com procedimentos de diagnóstico sistemático: $ARGUMENTS

1. **Visão Geral e Arquitetura do Sistema**
   - Documenta a arquitetura e componentes do sistema
   - Mapeia dependências e integrações
   - Identifica caminhos críticos e pontos de falha
   - Cria diagramas de topologia do sistema
   - Documenta fluxos de dados e padrões de comunicação

2. **Identificação de Problemas Comuns**
   - Coleta tickets de suporte históricos e issues
   - Entrevista membros da equipe sobre problemas frequentes
   - Analisa logs de erro e dados de monitoramento
   - Revisa feedback e reclamações de usuários
   - Identifica padrões em falhas do sistema

3. **Framework de Troubleshooting**
   - Estabelece procedimentos de diagnóstico sistemático
   - Cria metodologias de isolamento de problemas
   - Documenta caminhos e procedimentos de escalação
   - Configura checkpoints de logging e monitoramento
   - Define níveis de severidade e tempos de resposta

4. **Ferramentas e Comandos de Diagnóstico**
   
   ```markdown
   ## Comandos de Diagnóstico Essenciais
   
   ### Saúde do Sistema
   ```bash
   # Verifica recursos do sistema
   top                    # Uso de CPU e memória
   df -h                 # Espaço em disco
   free -m               # Uso de memória
   netstat -tuln         # Conexões de rede
   
   # Logs da aplicação
   tail -f /var/log/app.log
   journalctl -u service-name -f
   
   # Conectividade do banco de dados
   mysql -u user -p -e "SELECT 1"
   psql -h host -U user -d db -c "SELECT 1"
   ```
   ```

5. **Categorias de Issues e Soluções**

   **Problemas de Performance:**
   ```markdown
   ### Tempos de Resposta Lentos
   
   **Sintomas:**
   - Respostas de API > 5 segundos
   - Interface do usuário congelando
   - Timeouts do banco de dados
   
   **Passos de Diagnóstico:**
   1. Verifica recursos do sistema (CPU, memória, disco)
   2. Revisa logs da aplicação para erros
   3. Analisa performance de queries do banco de dados
   4. Verifica conectividade e latência de rede
   
   **Causas Comuns:**
   - Esgotamento do pool de conexões do banco
   - Queries do banco de dados ineficientes
   - Memory leaks na aplicação
   - Limitações de largura de banda da rede
   
   **Soluções:**
   - Reinicia serviços da aplicação
   - Otimiza queries do banco de dados
   - Aumenta o tamanho do pool de conexões
   - Aumenta escala dos recursos de infraestrutura
   ```

6. **Documentação de Códigos de Erro**
   
   ```markdown
   ## Referência de Códigos de Erro
   
   ### Status Codes HTTP
   - **500 Internal Server Error**
     - Verifica logs da aplicação para stack traces
     - Valida conectividade do banco de dados
     - Verifica variáveis de ambiente
   
   - **404 Not Found**
     - Valida configuração de roteamento de URL
     - Verifica se recursos existem
     - Revisa documentação de endpoints da API
   
   - **503 Service Unavailable**
     - Verifica status de saúde do serviço
     - Valida configuração do load balancer
     - Verifica se há modo de manutenção ativo
   ```

7. **Problemas Específicos de Ambiente**
   - Documenta problemas de ambiente de desenvolvimento
   - Aborda issues de ambiente de staging/teste
   - Cobre troubleshooting específico de produção
   - Inclui problemas de setup de desenvolvimento local

8. **Troubleshooting de Banco de Dados**
   
   ```markdown
   ### Problemas de Conexão ao Banco de Dados
   
   **Sintomas:**
   - Erros de "Connection refused"
   - Erros de "Too many connections"
   - Performance lenta de queries
   
   **Comandos de Diagnóstico:**
   ```sql
   -- Verifica conexões ativas
   SHOW PROCESSLIST;
   
   -- Verifica tamanho do banco
   SELECT table_schema, 
          ROUND(SUM(data_length + index_length) / 1024 / 1024, 1) AS 'Tamanho DB em MB' 
   FROM information_schema.tables 
   GROUP BY table_schema;
   
   -- Verifica queries lentas
   SHOW VARIABLES LIKE 'slow_query_log';
   ```
   ```

9. **Problemas de Rede e Conectividade**
   
   ```markdown
   ### Troubleshooting de Rede
   
   **Conectividade Básica:**
   ```bash
   # Testa conectividade básica
   ping example.com
   telnet host port
   curl -v https://api.example.com/health
   
   # Resolução de DNS
   nslookup example.com
   dig example.com
   
   # Roteamento de rede
   traceroute example.com
   ```
   
   **Problemas de SSL/TLS:**
   ```bash
   # Verifica certificado SSL
   openssl s_client -connect example.com:443
   curl -vI https://example.com
   ```
   ```

10. **Troubleshooting Específico da Aplicação**
    
    **Problemas de Memória:**
    ```markdown
    ### Erros de Out of Memory
    
    **Aplicações Java:**
    ```bash
    # Verifica uso de heap
    jstat -gc [PID]
    jmap -dump:format=b,file=heapdump.hprof [PID]
    
    # Analisa dump de heap
    jhat heapdump.hprof
    ```
    
    **Aplicações Node.js:**
    ```bash
    # Monitora uso de memória
    node --inspect app.js
    # Use Chrome DevTools para memory profiling
    ```
    ```

11. **Problemas de Segurança e Autenticação**
    
    ```markdown
    ### Falhas de Autenticação
    
    **Sintomas:**
    - Respostas 401 Unauthorized
    - Erros de validação de token
    - Problemas de timeout de sessão
    
    **Passos de Diagnóstico:**
    1. Valida credenciais e tokens
    2. Verifica expiração de token
    3. Valida serviço de autenticação
    4. Revisa configuração de CORS
    
    **Soluções Comuns:**
    - Atualiza tokens de autenticação
    - Limpa cookies/cache do navegador
    - Valida headers de CORS
    - Verifica permissões de API key
    ```

12. **Problemas de Deployment e Configuração**
    
    ```markdown
    ### Falhas de Deployment
    
    **Problemas de Container:**
    ```bash
    # Verifica status do container
    docker ps -a
    docker logs container-name
    
    # Verifica limites de recursos
    docker stats
    
    # Debug de container
    docker exec -it container-name /bin/bash
    ```
    
    **Problemas de Kubernetes:**
    ```bash
    # Verifica status dos pods
    kubectl get pods
    kubectl describe pod pod-name
    kubectl logs pod-name
    
    # Verifica conectividade de serviço
    kubectl get svc
    kubectl port-forward pod-name 8080:8080
    ```
    ```

13. **Setup de Monitoramento e Alertas**
    - Configura health checks e monitoramento
    - Configura agregação e análise de logs
    - Implementa alertas para issues críticas
    - Cria dashboards para métricas do sistema
    - Documenta limiares de monitoramento

14. **Procedimentos de Escalação**
    
    ```markdown
    ## Matriz de Escalação
    
    ### Níveis de Severidade
    
    **Crítico (P1):** Sistema inativo, perda de dados
    - Resposta imediata necessária
    - Escalona para engenheiro on-call
    - Notifica gestão em 30 minutos
    
    **Alto (P2):** Funcionalidade maior prejudicada
    - Resposta em 2 horas
    - Escalona para engenheiro sênior
    - Fornece atualizações horárias
    
    **Médio (P3):** Issues menores de funcionalidade
    - Resposta em 8 horas
    - Atribui ao membro apropriado da equipe
    - Fornece atualizações diárias
    ```

15. **Procedimentos de Recuperação**
    - Documenta passos de recuperação do sistema
    - Cria procedimentos de backup e restore de dados
    - Estabelece procedimentos de rollback de deployments
    - Documenta processos de disaster recovery
    - Testa procedimentos de recuperação regularmente

16. **Medidas Preventivas**
    - Implementa monitoramento e alertas
    - Configura health checks automatizados
    - Cria procedimentos de validação de deployment
    - Estabelece processos de code review
    - Documenta procedimentos de manutenção

17. **Integração com Base de Conhecimento**
    - Vincula a documentação relevante
    - Referencia documentação de API
    - Inclui links para dashboards de monitoramento
    - Conecta a canais de comunicação da equipe
    - Integra com sistemas de ticketing

18. **Comunicação em Equipe**
    
    ```markdown
    ## Canais de Comunicação
    
    ### Resposta Imediata
    - Slack: canal #incidents
    - Telefone: Rotação on-call
    - Email: alerts@company.com
    
    ### Atualizações de Status
    - Página de status: status.company.com
    - Twitter: @company_status
    - Wiki interno: seção de troubleshooting
    ```

19. **Manutenção da Documentação**
    - Revisão regular e atualizações
    - Controle de versão para guias de troubleshooting
    - Coleta de feedback de usuários
    - Integração com post-mortems de incidentes
    - Processos de melhoria contínua

20. **Ferramentas Self-Service**
    - Cria scripts e ferramentas de diagnóstico
    - Constrói procedimentos de recuperação automatizados
    - Implementa sistemas self-healing
    - Fornece interfaces de diagnóstico amigáveis
    - Cria integração com chatbot para issues comuns

**Técnicas Avançadas de Troubleshooting:**

**Análise de Logs:**
```bash
# Busca por erros específicos
grep -i "error" /var/log/app.log | tail -50

# Analisa padrões de logs
awk '{print $1}' access.log | sort | uniq -c | sort -nr

# Monitora logs em tempo real
tail -f /var/log/app.log | grep -i "exception"
```

**Performance Profiling:**
```bash
# Performance do sistema
iostat -x 1
sar -u 1 10
vmstat 1 10

# Application profiling
strace -p [PID]
perf record -p [PID]
```

Lembre-se de:
- Manter guias de troubleshooting atualizados
- Testar todos os procedimentos documentados regularmente
- Coletar feedback de usuários e melhorar guias
- Incluir screenshots e recursos visuais quando úteis
- Tornar guias pesquisáveis e bem organizados