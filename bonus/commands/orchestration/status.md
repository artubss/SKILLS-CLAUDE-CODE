# Comando de Status de Tarefas

Verifique o status atual das tarefas no sistema de orquestração com várias opções de filtro e relatório.

## Uso

```
/task-status [options]
```

## Descrição

Fornece visibilidade abrangente do progresso das tarefas, distribuição de status e métricas de execução em todas as orquestrações ativas.

## Variantes do Comando

### Visão Geral de Status Básico
```
/task-status
```
Mostra resumo de todas as tarefas em todas as orquestrações ativas.

### Tarefas de Hoje
```
/task-status --today
```
Mostra apenas tarefas das orquestrações de hoje.

### Orquestração Específica
```
/task-status --date 03_15_2024 --project payment_integration
```
Mostra tarefas de uma orquestração específica.

### Filtro de Status
```
/task-status --status in_progress
/task-status --status qa
/task-status --status on_hold
```
Mostra apenas tarefas com o status especificado.

### Visão Detalhada
```
/task-status --detailed
```
Mostra informações abrangentes para cada tarefa.

## Formatos de Saída

### Visão de Resumo (Padrão)
```
Task Orchestration Status Summary
=================================

Active Orchestrations: 3
Total Tasks: 47

Status Distribution:
┌─────────────┬───────┬────────────┐
│ Status      │ Count │ Percentage │
├─────────────┼───────┼────────────┤
│ completed   │  12   │    26%     │
│ qa          │   5   │    11%     │
│ in_progress │   3   │     6%     │
│ on_hold     │   2   │     4%     │
│ todos       │  25   │    53%     │
└─────────────┴───────┴────────────┘

Active Tasks (in_progress):
- TASK-001: Implement JWT authentication (Agent: dev-frontend)
- TASK-007: Create payment webhook handler (Agent: dev-backend)
- TASK-012: Write integration tests (Agent: test-developer)

Blocked Tasks (on_hold):
- TASK-004: User profile API (Blocked by: TASK-001)
- TASK-009: Payment confirmation UI (Blocked by: TASK-007)
```

### Visão Detalhada
```
Task Details for: 03_15_2024/authentication_system
==================================================

TASK-001: Implement JWT authentication
Status: in_progress
Agent: dev-frontend
Started: 2024-03-15T14:30:00Z
Duration: 3.5 hours
Progress: 75% (est. 1 hour remaining)
Dependencies: None
Blocks: TASK-004, TASK-005
Location: /task-orchestration/03_15_2024/authentication_system/tasks/in_progress/

Status History:
- todos → in_progress (2024-03-15T14:30:00Z) by dev-frontend
```

### Visão de Cronograma
```
/task-status --timeline
```
Mostra cronograma no estilo Gantt da execução das tarefas.

### Relatório de Velocidade
```
/task-status --velocity
```
Mostra taxas de conclusão e métricas de desempenho.

## Opções de Filtro

### Por Agent
```
/task-status --agent dev-frontend
```

### Por Prioridade
```
/task-status --priority high
```

### Por Tipo
```
/task-status --type feature
/task-status --type bugfix
```

### Múltiplos Filtros
```
/task-status --status todos --priority high --type security
```

## Ações Rápidas

### Mostrar Caminho Crítico
```
/task-status --critical-path
```
Destaca tarefas que estão bloqueando outras.

### Mostrar Vencidas
```
/task-status --overdue
```
Mostra tarefas que excedem o tempo estimado.

### Mostrar Disponíveis
```
/task-status --available
```
Mostra tarefas de todo prontas para serem selecionadas.

## Comandos de Integração

### Exportar Status
```
/task-status --export markdown
/task-status --export csv
```

### Modo de Monitoramento
```
/task-status --watch
```
Atualiza status em tempo real (recarrega a cada 30 segundos).

## Exemplos

### Exemplo 1: Visão de Daily Standup
```
/task-status --today --detailed
```

### Exemplo 2: Encontrar Trabalho Bloqueado
```
/task-status --status on_hold --show-blockers
```

### Exemplo 3: Carga de Trabalho do Agent
```
/task-status --by-agent --status in_progress
```

### Exemplo 4: Progresso do Sprint
```
/task-status --date 03_15_2024 --metrics
```

## Métricas e Análises

### Métricas de Conclusão
- Tempo médio por tarefa
- Tarefas concluídas por dia
- Tempos de transição de status

### Análise de Gargalos
- Tarefas mais bloqueadoras
- Duração mais longa em on_hold
- Duração do caminho crítico

### Desempenho do Agent
- Tarefas por agent
- Tempo médio de conclusão
- Carga de trabalho atual

## Boas Práticas

1. **Verificação Diária**: Execute `/task-status --today` cada manhã
2. **Revisão de Bloqueadores**: Verifique `/task-status --status on_hold` regularmente
3. **Acompanhamento de Progresso**: Use `/task-status --velocity` para tendências
4. **Planejamento de Recursos**: Monitore `/task-status --by-agent`

## Notas

- Os dados de status são lidos de arquivos TASK-STATUS-TRACKER.yaml
- Todos os horários são mostrados no fuso horário local
- Tarefas concluídas são incluídas em métricas mas não em listas ativas
- Use a flag `--all` para incluir orquestrações históricas