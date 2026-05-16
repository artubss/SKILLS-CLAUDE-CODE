# Comando de Retomada de Orquestração

Retome trabalho em orquestrações de tarefas existentes após perda de sessão ou troca de contexto.

## Uso

```
/orchestration/resume [options]
```

## Descrição

Restaura contexto completo para orquestrações ativas, mostrando progresso atual, identificando próximas ações e fornecendo todas as informações necessárias para continuar o trabalho sem interrupções.

## Comandos Básicos

### Listar Orquestrações Ativas
```
/orchestration/resume
```
Mostra todas as orquestrações com tarefas ativas (não concluídas).

### Retomar Orquestração Específica
```
/orchestration/resume --date 03_15_2024 --project auth_system
```
Carrega contexto completo para uma orquestração específica.

### Retomar Mais Recente
```
/orchestration/resume --latest
```
Retoma automaticamente a orquestração mais recentemente ativa.

## Formato de Saída

### Visualização de Lista de Orquestrações
```
Active Task Orchestrations
==========================

1. 03_15_2024/authentication_system
   Started: 3 days ago | Progress: 65% | Active Tasks: 3
   └─ Focus: JWT implementation, OAuth integration

2. 03_14_2024/payment_processing  
   Started: 4 days ago | Progress: 40% | Active Tasks: 2
   └─ Focus: Stripe webhooks, refund handling

3. 03_12_2024/admin_dashboard
   Started: 1 week ago | Progress: 85% | Active Tasks: 1
   └─ Focus: Final testing and deployment

Select orchestration to resume: [1-3] or use --date and --project
```

### Visualização Detalhada de Retomada
```
Resuming: authentication_system (03_15_2024)
============================================

## Current Status Summary
- Total Tasks: 24 (12 completed, 3 in progress, 2 on hold, 7 todos)
- Time Elapsed: 3 days
- Estimated Remaining: 2 days

## Tasks In Progress
┌──────────┬────────────────────────────┬───────────────┬──────────────┐
│ Task ID  │ Title                      │ Agent         │ Duration     │
├──────────┼────────────────────────────┼───────────────┼──────────────┤
│ TASK-003 │ JWT token validation       │ dev-backend   │ 2.5h         │
│ TASK-007 │ OAuth provider setup       │ dev-frontend  │ 1h           │
│ TASK-011 │ Integration tests          │ test-dev      │ 30m          │
└──────────┴────────────────────────────┴───────────────┴──────────────┘

## Blocked Tasks (Require Attention)
- TASK-005: User profile API - Blocked by TASK-003 (JWT validation)
- TASK-009: OAuth callback handling - Waiting for provider credentials

## Next Available Tasks (Ready to Start)
1. TASK-013: Password reset flow (4h, frontend)
   Files: src/auth/reset.tsx, src/api/auth.ts
   
2. TASK-014: Session management (3h, backend)
   Files: src/services/session.ts, src/middleware/auth.ts

## Recent Git Activity
- feature/jwt-auth: 2 commits behind, last commit 2h ago
- feature/oauth-setup: clean, last commit 1h ago

## Quick Actions
[1] Show TASK-003 details (current focus)
[2] Pick up TASK-013 (password reset)
[3] View dependency graph
[4] Show recent commits
[5] Generate status report
```

## Recursos de Recuperação de Contexto

### Contexto de Tarefa
```
/orchestration/resume --task TASK-003
```
Mostra:
- Descrição completa da tarefa e requisitos
- Progresso da implementação e notas
- Arquivos relacionados com mudanças recentes
- Requisitos e status de testes
- Dependências e bloqueadores

### Contexto de Arquivo
```
/orchestration/resume --show-files
```
Lista todos os arquivos mencionados em tarefas ativas com:
- Última hora de modificação
- Status git atual
- Quais tarefas as referem

### Contexto de Dependência
```
/orchestration/resume --deps
```
Mostra gráfico de dependências focado em tarefas ativas.

## Recuperação de Estado de Trabalho

### Resumo de Estado Git
```
## Git Working State
Current Branch: feature/jwt-auth
Status: 2 files modified, 1 untracked

Modified Files:
- src/auth/jwt.ts (related to TASK-003)
- tests/auth.test.ts (related to TASK-003)

Untracked:
- src/auth/jwt.config.ts (new file for TASK-003)

Recommendation: Commit current changes before switching tasks
```

### Resumo da Última Sessão
```
## Last Session (2 hours ago)
- Completed: TASK-002 (Database schema)
- Started: TASK-003 (JWT validation)
- Commits: 2 (feat: add user auth schema, test: auth unit tests)
- Next planned: Continue TASK-003, then TASK-005
```

## Opções de Filtro

### Por Status
```
/orchestration/resume --show in_progress,on_hold
```

### Por Intervalo de Data
```
/orchestration/resume --since "last week"
```

### Por Conclusão
```
/orchestration/resume --incomplete  # < 50% done
/orchestration/resume --nearly-done  # > 80% done
```

## Recursos de Integração

### Retomada Direta de Tarefa
```
/orchestration/resume --pickup TASK-013
```
Automaticamente:
1. Mostra detalhes da tarefa
2. Move para in_progress
3. Mostra arquivos relevantes
4. Cria branch de feature se necessário

### Integração de Verificação de Status
```
/orchestration/resume --with-status
```
Inclui relatório de status completo com contexto de retomada.

### Histórico de Commits
```
/orchestration/resume --commits 5
```
Mostra últimos 5 commits relacionados à orquestração.

## Padrões de Retomada Rápida

### Standup Matinal
```
/orchestration/resume --latest --with-status
```
Perfeito para standups diários - mostra no que você estava trabalhando e o estado atual.

### Troca de Contexto
```
/orchestration/resume --save-state
```
Salva estado de trabalho atual antes de trocar para outra orquestração.

### Handoff para Time
```
/orchestration/resume --handoff
```
Gera notas detalhadas de handoff para outro desenvolvedor.

## Exemplos

### Exemplo 1: Continuar Rapidamente
```
/orchestration/resume --latest --pickup-where-left-off
```
Retoma exatamente onde você parou, mostrando a tarefa em progresso.

### Exemplo 2: Segunda-feira de Manhã
```
/orchestration/resume --since friday --show-completed
```
Mostra o que foi feito sexta-feira e o que vem depois para segunda-feira.

### Exemplo 3: Múltiplos Projetos
```
/orchestration/resume --all --summary
```
Visão geral rápida de todas as orquestrações ativas.

## Persistência de Estado

O comando lê de:
- EXECUTION-TRACKER.md para métricas de progresso
- TASK-STATUS-TRACKER.yaml para estado atual
- Arquivos de tarefa para contexto detalhado
- Git para estado do diretório de trabalho

## Melhores Práticas

1. **Use no Início da Sessão**: Execute `/orchestration/resume` ao iniciar o trabalho
2. **Salve o Estado**: Use `--save-state` antes de pausas prolongadas
3. **Verifique Dependências**: Revise tarefas bloqueadas que podem estar desbloqueadas agora
4. **Faça Commits Regularmente**: Mantenha o estado git alinhado com o progresso das tarefas

## Observações

- Detecta automaticamente mudanças não commitadas relacionadas a tarefas
- Sugere próximas ações com base em dependências e prioridades
- Integra com git worktrees se em uso
- Preserva histórico de tarefas para contexto completo