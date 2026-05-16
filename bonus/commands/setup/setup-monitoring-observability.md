---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [tipo-monitoramento] | --metrics | --logging | --tracing | --full-stack
description: Configure monitoramento e observabilidade abrangentes com métricas, logs, tracing e alertas
---

# Configurar Monitoramento & Observabilidade

Configure infraestrutura de monitoramento e observabilidade abrangente: **$ARGUMENTS**

## Estado Atual da Aplicação

- Tipo de aplicação: @package.json ou @requirements.txt (detecta framework e serviços)
- Monitoramento existente: !`find . -name "*prometheus*" -o -name "*grafana*" -o -name "*jaeger*" | wc -l`
- Infraestrutura: @docker-compose.yml ou @kubernetes/ ou detecção de plataforma cloud
- Configuração de logs: !`grep -r "winston\|logging\|console.log" src/ 2>/dev/null | wc -l`

## Tarefa

Implemente monitoramento e observabilidade pronto para produção com insights abrangentes:

**Tipo de Monitoramento**: Use $ARGUMENTS para focar em métricas, logs, tracing distribuído ou stack de observabilidade completa

**Stack de Observabilidade**:
1. **Coleta de Métricas** - Métricas de aplicação, monitoramento de infraestrutura, KPIs de negócio, dashboards personalizados
2. **Infraestrutura de Logs** - Logs centralizados, logs estruturados, agregação de logs, capacidades de busca
3. **Tracing Distribuído** - Rastreamento de requisições, análise de performance, identificação de gargalos, dependências entre serviços
4. **Sistema de Alertas** - Alertas inteligentes, políticas de escalação, canais de notificação, gerenciamento de incidentes
5. **Monitoramento de Performance** - Integração APM, monitoramento de usuários reais, monitoramento sintético, rastreamento de SLA
6. **Analytics & Relatórios** - Analytics de uso, tendências de performance, planejamento de capacidade, insights de negócio

**Integração de Plataforma**: Prometheus, Grafana, ELK Stack, Jaeger, DataDog, New Relic, soluções cloud-native.

**Recursos de Produção**: Alta disponibilidade, políticas de retenção de dados, controles de segurança, otimização de custos.

**Output**: Plataforma de observabilidade completa com monitoramento em tempo real, alertas inteligentes e dashboards de analytics abrangentes.