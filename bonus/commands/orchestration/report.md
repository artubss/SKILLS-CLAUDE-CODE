# Comando Task Report

Gera relatórios abrangentes sobre execução de tarefas, progresso e métricas.

## Uso

```
/task-report [report-type] [options]
```

## Descrição

Cria relatórios detalhados para gerenciamento de projetos, revisões de sprints e análise de desempenho. Suporta múltiplos tipos de relatório e formatos de saída.

## Tipos de Relatório

### Executive Summary
```
/task-report executive
```
Visão geral de alto nível para stakeholders com métricas-chave e progresso.

### Sprint Report
```
/task-report sprint --date 03_15_2024
```
Progresso detalhado do sprint com gráficos de burndown e velocity.

### Daily Standup
```
/task-report standup
```
O que foi concluído, está em progresso e está bloqueado.

### Performance Report
```
/task-report performance --period week
```
Métricas de desempenho da equipe e individual.

### Dependency Report
```
/task-report dependencies
```
Grafo visual de dependências e análise de gargalos.

## Exemplos de Saída

### Relatório Executive Summary
```
EXECUTIVE SUMMARY - Authentication System Project
================================================
Report Date: 2024-03-15
Project Start: 2024-03-13
Duration: 3 days (60% complete)

KEY METRICS
-----------
• Total Tasks: 24
• Completed: 12 (50%)
• In Progress: 3 (12.5%)
• Blocked: 2 (8.3%)
• Remaining: 7 (29.2%)

TIMELINE
--------
• Original Estimate: 5 days
• Current Projection: 5.5 days
• Risk Level: Low

HIGHLIGHTS
----------
✓ Core authentication API completed
✓ Database schema migrated
✓ Unit tests passing (98% coverage)

BLOCKERS
--------
⚠ Payment integration waiting on external API
⚠ UI components need design approval

NEXT MILESTONES
--------------
→ Complete JWT implementation (Today)
→ Integration testing (Tomorrow)
→ Security audit (Day 4)
```

### Relatório Sprint Burndown
```
/task-report burndown --sprint current
```
```
SPRINT BURNDOWN - Sprint 24
===========================

Tasks Remaining by Day:
Day 1: ████████████████████ 24
Day 2: ████████████████     20 
Day 3: ████████████         15 (TODAY)
Day 4: ████████             10 (projected)
Day 5: ████                 5  (projected)

Velocity Metrics:
- Average: 4.5 tasks/day
- Yesterday: 5 tasks
- Today: 3 tasks (in progress)

Risk Assessment: ON TRACK
```

### Relatório Performance
```
TEAM PERFORMANCE REPORT - Week 11
=================================

By Agent:
┌─────────────────┬────────┬───────────┬─────────┬────────────┐
│ Agent           │ Completed │ Avg Time │ Quality │ Efficiency │
├─────────────────┼────────┼───────────┼─────────┼────────────┤
│ dev-frontend    │    8   │   3.2h    │   95%   │    125%    │
│ dev-backend     │    6   │   4.1h    │   98%   │    110%    │
│ test-developer  │    4   │   2.8h    │   100%  │    115%    │
└─────────────────┴────────┴───────────┴─────────┴────────────┘

By Task Type:
- Features: 12 completed (avg 3.8h)
- Bugfixes: 4 completed (avg 1.5h)
- Tests: 8 completed (avg 2.2h)

Quality Metrics:
- First-time pass rate: 88%
- Rework required: 2 tasks
- Blocked time: 4.5 hours total
```

## Opções de Personalização

### Período de Tempo
```
/task-report summary --from 2024-03-01 --to 2024-03-15
/task-report summary --last 7d
/task-report summary --this-month
```

### Projeto Específico
```
/task-report sprint --project authentication_system
```

### Opções de Formato
```
/task-report executive --format markdown
/task-report executive --format html
/task-report executive --format pdf
```

### Incluir/Excluir
```
/task-report summary --include completed,qa
/task-report summary --exclude on_hold
```

## Relatórios Especializados

### Análise do Caminho Crítico
```
/task-report critical-path
```
Mostra tarefas que impactam diretamente o tempo de conclusão.

### Análise de Gargalos
```
/task-report bottlenecks
```
Identifica tarefas causando atrasos.

### Utilização de Recursos
```
/task-report resources
```
Mostra alocação de agentes e disponibilidade.

### Avaliação de Riscos
```
/task-report risks
```
Identifica atrasos e problemas potenciais.

## Opções de Visualização

### Gráfico de Gantt
```
/task-report gantt --weeks 2
```

### Grafo de Dependências
```
/task-report dependencies --visual
```

### Fluxo de Status
```
/task-report flow --animated
```

## Relatórios Automatizados

### Agendar Relatórios
```
/task-report schedule daily-standup --at "9am"
/task-report schedule weekly-summary --every friday
```

### Relatórios por Email
```
/task-report executive --email team@company.com
```

## Relatórios de Comparação

### Comparação de Sprints
```
/task-report compare --sprint 23 24
```

### Semana a Semana
```
/task-report trends --weeks 4
```

## Exemplos

### Exemplo 1: Status Matinal
```
/task-report standup --format slack
```
Gera relatório de standup formatado para Slack.

### Exemplo 2: Revisão de Sprint
```
/task-report sprint --include-velocity --include-burndown
```
Métricas de sprint abrangentes para reunião de revisão.

### Exemplo 3: Foco em Bloqueadores
```
/task-report blockers --show-dependencies --show-resolution
```
Análise profunda do que está bloqueando o progresso.

## Recursos de Integração

### Exportar para Ferramentas
```
/task-report export-jira
/task-report export-asana
/task-report export-github
```

### Endpoints de API
```
/task-report api --generate-endpoint
```
Cria endpoint de API para acesso externo.

## Melhores Práticas

1. **Revisões Diárias**: Execute relatório de standup cada manhã
2. **Resumos Semanais**: Gere relatórios de desempenho às sextas-feiras
3. **Planejamento de Sprint**: Use tendências de velocity para estimativas
4. **Atualizações de Stakeholders**: Agende resumos executivos automatizados

## Componentes do Relatório

Cada relatório pode incluir:
- Estatísticas resumidas
- Visualização de timeline
- Listas de tarefas por status
- Desempenho de agentes
- Análise de dependências
- Avaliação de riscos
- Recomendações
- Tendências históricas

## Observações

- Relatórios usam dados de todos os arquivos TASK-STATUS-TRACKER.yaml
- Tarefas concluídas são incluídas em métricas históricas
- Cálculos de tempo usam horas comerciais por padrão
- Todos os horários mostrados no fuso horário local
- Gráficos requerem suporte a unicode no terminal