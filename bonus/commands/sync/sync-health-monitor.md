---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [scope] | --github | --linear | --webhooks | --performance | --report
description: Monitorar e diagnosticar a saúde da sincronização GitHub-Linear com análise de desempenho e solução automática de problemas
---

# Monitor de Saúde de Sincronização

Monitore a saúde e desempenho abrangentes da sincronização GitHub-Linear: **$ARGUMENTS**

## Ambiente Atual de Sincronização

- Status da API GitHub: !`gh api rate_limit -q '.rate | "GitHub: \(.remaining)/\(.limit) requests"' 2>/dev/null || echo "Verificação de API GitHub necessária"`
- Conectividade Linear: Status do servidor MCP Linear e validação de autenticação
- Status de webhook: Configurações de webhook ativas e saúde do processamento de eventos
- Desempenho de sincronização: Throughput atual, métricas de latência e taxas de erro

## Tarefa

Implemente monitoramento abrangente de saúde de sincronização com diagnósticos automatizados e otimização de desempenho:

**Escopo de Monitoramento**: Use $ARGUMENTS para especificar saúde do GitHub, conectividade Linear, diagnósticos de webhook, análise de desempenho ou relatório de saúde completo

**Framework de Monitoramento de Saúde**:
1. **Avaliação de Saúde da API** - Monitore status da API GitHub/Linear, limites de taxa, autenticação, problemas de conectividade
2. **Análise de Desempenho de Sincronização** - Rastreie métricas de throughput, padrões de latência, tempos de processamento, profundidades de fila
3. **Detecção de Padrão de Erros** - Identifique falhas recorrentes, classifique tipos de erro, analise tendências de falha
4. **Diagnósticos de Webhook** - Valide configurações de webhook, teste entrega de eventos, monitore latência de processamento
5. **Validação de Integridade de Dados** - Verifique consistência de sincronização, detecte registros órfãos, valide referências cruzadas
6. **Solução Automática de Problemas** - Execute testes diagnósticos, sugira correções, implemente procedimentos de recuperação automática

**Recursos Avançados**: Dashboards de saúde em tempo real, detecção preditiva de falhas, workflows de recuperação automática, profiling abrangente de desempenho.

**Capacidades de Diagnóstico**: Análise profunda de erros, identificação de gargalos, validação de configuração, suites de testes automatizados.

**Saída**: Avaliação de saúde completa com métricas de desempenho, análise de erros, otimizações recomendadas e relatórios diagnósticos automatizados.