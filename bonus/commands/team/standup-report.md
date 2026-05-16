---
allowed-tools: Read, Bash, Glob, Grep
argument-hint: [intervalo-de-tempo] | --ontem | --últimas-24h | --desde-sexta | --intervalo-customizado
description: Gerar relatórios abrangentes de standup diário com análise de atividade do time e rastreamento de progresso
---

# Relatório de Standup

Gerar relatórios abrangentes de standup diário com análise de atividade e progresso do time: **$ARGUMENTS**

## Contexto Atual de Standup

- Conexão Linear: status do servidor Linear MCP e sincronização de tarefas
- Intervalo de tempo: !`date -d 'yesterday' '+%Y-%m-%d'` a !`date '+%Y-%m-%d'` período de análise
- Membros do time: !`git log --format='%ae' --since='1 day ago' | sort -u | wc -l` contribuidores ativos
- Repositório: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`

## Tarefa

Gerar relatório abrangente de standup com análise de atividade do time e insights de progresso:

**Intervalo de Tempo**: Use $ARGUMENTS para especificar ontem, últimas 24 horas, desde sexta-feira, ou intervalo de datas customizado para análise

**Framework do Relatório de Standup**:
1. **Análise de Atividade Git** - Extrair atividade de commits, analisar mudanças de código, identificar contribuidores, avaliar escopo de impacto
2. **Progresso de Tarefas Linear** - Consultar atualizações de tarefas, analisar status de conclusão, rastrear progresso do sprint, identificar bloqueadores
3. **Atividade de Pull Requests** - Revisar submissões de PR, analisar atividade de revisão, rastrear status de merge, avaliar padrões de colaboração
4. **Colaboração do Time** - Analisar pair programming, participação em code review, compartilhamento de conhecimento, atividades de mentoria
5. **Rastreamento de Progresso** - Calcular métricas de velocidade, avaliar conclusão de objetivos, identificar tendências, prever resultados do sprint
6. **Bloqueadores e Impedimentos** - Identificar tarefas travadas, analisar padrões de atraso, avaliar necessidades de recursos, recomendar soluções

**Funcionalidades Avançadas**: Categorização automatizada de atividades, visualização de progresso, análise de tendências, insights preditivos, scoring de saúde do time.

**Qualidade do Relatório**: Insights acionáveis, indicadores de progresso claros, identificação de obstáculos, suporte à coordenação do time, otimização da eficiência de reuniões.

**Output**: Relatório abrangente de standup com resumo de atividade do time, métricas de progresso, identificação de bloqueadores e próximos passos acionáveis.