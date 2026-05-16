---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [monitoring-type] | --apm | --rum | --custom
description: Configure monitoramento abrangente de desempenho de aplicações com métricas, alertas e observabilidade
---

# Adicionar Monitoramento de Desempenho

Configure monitoramento de desempenho de aplicações: **$ARGUMENTS**

## Instruções

1. **Estratégia de Monitoramento de Desempenho**
   - Defina indicadores-chave de desempenho (KPIs) e objetivos de nível de serviço (SLOs)
   - Identifique jornadas críticas do usuário e gargalos de desempenho
   - Planeje a arquitetura de monitoramento e a estratégia de coleta de dados
   - Avalie a infraestrutura de monitoramento existente e pontos de integração
   - Defina limites de alerta e procedimentos de escalação

2. **Monitoramento de Desempenho de Aplicações (APM)**
   - Configure uma solução APM abrangente (New Relic, Datadog, AppDynamics)
   - Configure rastreamento distribuído para visibilidade do ciclo de vida das requisições
   - Implemente métricas personalizadas e rastreamento de desempenho
   - Configure monitoramento de transações e rastreamento de erros
   - Configure análise de desempenho e diagnósticos

3. **Monitoramento de Usuários Reais (RUM)**
   - Implemente rastreamento de desempenho no cliente e monitoramento de web vitals
   - Configure coleta de métricas de experiência do usuário (LCP, FID, CLS, TTFB)
   - Configure métricas de desempenho personalizadas para interações do usuário
   - Monitore desempenho de carregamento de página e carregamento de recursos
   - Rastreie desempenho da jornada do usuário em diferentes dispositivos

4. **Monitoramento de Desempenho do Servidor**
   - Monitore métricas do sistema (CPU, memória, disco, rede)
   - Configure monitoramento no nível de processo e aplicação
   - Configure monitoramento de event loop lag e coleta de lixo
   - Implemente métricas de desempenho personalizadas do servidor
   - Monitore utilização de recursos e planejamento de capacidade

5. **Monitoramento de Desempenho de Banco de Dados**
   - Rastreie desempenho de consultas de banco de dados e identificação de consultas lentas
   - Monitore utilização do pool de conexões de banco de dados
   - Configure métricas e alertas de desempenho do banco de dados
   - Implemente análise de plano de execução de consultas
   - Monitore uso de recursos do banco de dados e oportunidades de otimização

6. **Rastreamento e Monitoramento de Erros**
   - Implemente rastreamento abrangente de erros (Sentry, Bugsnag, Rollbar)
   - Configure categorização de erros e análise de impacto
   - Configure sistemas de alerta e notificação de erros
   - Rastreie tendências de erros e métricas de resolução
   - Implemente contexto de erro e informações de depuração

7. **Métricas Personalizadas e Dashboards**
   - Implemente rastreamento de métricas de negócio (Prometheus, StatsD)
   - Crie dashboards de desempenho e visualizações
   - Configure regras de alerta personalizadas e limites
   - Configure análise de tendências de desempenho e relatórios
   - Implemente detecção de regressão de desempenho

8. **Sistema de Alertas e Notificações**
   - Configure alertas inteligentes com base em limites de desempenho
   - Configure notificações multi-canal (email, Slack, PagerDuty)
   - Implemente escalação de alertas e procedimentos de on-call
   - Configure prevenção de fadiga de alerta e redução de ruído
   - Configure workflows de gerenciamento de incidentes de desempenho

9. **Integração de Testes de Desempenho**
   - Integre monitoramento com testes de carga e desempenho
   - Configure testes de desempenho contínuos e monitoramento
   - Configure rastreamento e comparação de baseline de desempenho
   - Implemente análise e relatórios de resultados de testes de desempenho
   - Monitore desempenho em diferentes cenários de carga

10. **Recomendações de Otimização de Desempenho**
    - Gere insights e recomendações de desempenho acionáveis
    - Implemente análise e relatórios automáticos de desempenho
    - Configure rastreamento e medição de otimização de desempenho
    - Configure validação de melhorias de desempenho
    - Crie frameworks de priorização de otimização de desempenho

Priorize estratégias de monitoramento que forneçam insights acionáveis para otimização de desempenho. Garanta que a sobrecarga de monitoramento seja mínima e não impacte o desempenho da aplicação.