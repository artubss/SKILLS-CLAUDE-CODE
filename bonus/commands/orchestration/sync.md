# Comando de Sincronização de Orquestração

Sincronize o status das tarefas com commits do git, garantindo consistência entre controle de versão e rastreamento de tarefas.

## Uso

```
/orchestration/sync [options]
```

## Descrição

Analisa o histórico do git e o status das tarefas para identificar discrepâncias, atualizando automaticamente o rastreamento de tarefas com base em evidências de commits e mantendo consistência bidirecional.

## Comandos Básicos

### Sincronização Completa
```
/orchestration/sync
```
Realiza sincronização completa entre git e status das tarefas.

### Verificar Status de Sincronização
```
/orchestration/sync --check
```
Relata inconsistências sem fazer alterações.

### Sincronizar Orquestração Específica
```
/orchestration/sync --date 03_15_2024 --project auth_system
```

## Operações de Sincronização

### Git → Status da Tarefa
Atualiza o status das tarefas com base em mensagens de commit:
```
Found commits:
- feat(auth): implement JWT validation (TASK-003) ✓
  Status: in_progress → qa (based on commit)
  
- test(auth): add JWT validation tests (TASK-003) ✓
  Status: qa → completed (tests indicate completion)
  
- fix(auth): resolve token expiration (TASK-007) ✓
  Status: todos → in_progress (work started)
```

### Status da Tarefa → Git
Identifica tarefas marcadas como concluídas sem commits:
```
Status Discrepancies:
- TASK-005: Marked 'completed' but no commits found
- TASK-008: In 'qa' but no implementation commits
- TASK-010: Multiple commits but still in 'todos'
```

## Padrões de Detecção

### Correspondência de Padrões de Commit
```
Patterns detected:
- "feat(auth): implement" → Implementation complete
- "test(auth): add" → Testing phase
- "fix(auth): resolve" → Bug fix complete
- "docs(auth): update" → Documentation done
- "refactor(auth):" → Code improvement
```

### Extração de Referência de Tarefa
```
Scanning commits for task references:
- Explicit: "Task: TASK-003" ✓
- In body: "Implements TASK-003" ✓
- Branch name: "feature/TASK-003-jwt" ✓
- PR title: "TASK-003: JWT implementation" ✓
```

## Regras de Sincronização

### Atualizações Automáticas de Status
```yaml
sync_rules:
  commit_patterns:
    - pattern: "feat.*TASK-(\d+)"
      action: "move to qa if in_progress"
    - pattern: "test.*TASK-(\d+).*pass"
      action: "move to completed if in qa"
    - pattern: "fix.*TASK-(\d+)"
      action: "move to qa if in_progress"
    - pattern: "WIP.*TASK-(\d+)"
      action: "keep in in_progress"
```

### Resolução de Conflitos
```
Conflict detected for TASK-003:
- Git evidence: 3 commits, tests passing
- Task status: in_progress
- Recommended: Move to completed

Resolution options:
[1] Trust git (move to completed)
[2] Trust tracker (keep in_progress)
[3] Manual review
[4] Skip
```

## Relatórios de Análise

### Resumo de Sincronização
```
Synchronization Report
======================

Analyzed: 45 commits across 3 branches
Tasks referenced: 12
Status updates needed: 4

Updates to apply:
- TASK-003: in_progress → completed (3 commits)
- TASK-007: todos → in_progress (1 commit)
- TASK-009: qa → completed (tests added)
- TASK-011: on_hold → in_progress (blocker resolved)

Warnings:
- TASK-005: Completed without commits
- TASK-013: Commits without task reference
```

### Análise Detalhada
```
Task: TASK-003 - JWT Implementation
Current Status: in_progress
Git Evidence:
  - feat(auth): implement JWT validation (2 days ago)
  - test(auth): add validation tests (1 day ago)
  - fix(auth): handle edge cases (1 day ago)
  
Recommendation: Move to completed
Confidence: High (95%)
```

## Opções

### Execução de Teste
```
/orchestration/sync --dry-run
```
Mostra o que seria alterado sem aplicar as atualizações.

### Sincronização Forçada
```
/orchestration/sync --force
```
Aplica todas as recomendações sem solicitar confirmação.

### Intervalo de Tempo
```
/orchestration/sync --since "1 week ago"
```
Analisa apenas commits recentes.

### Específico por Branch
```
/orchestration/sync --branch feature/auth
```
Sincroniza apenas tarefas relacionadas a um branch específico.

## Recursos de Integração

### Atualizar Arquivos de Rastreamento
```
/orchestration/sync --update-trackers
```
Atualiza TASK-STATUS-TRACKER.yaml com:
```yaml
git_tracking:
  TASK-003:
    status_from_git: completed
    confidence: 0.95
    evidence:
      - commit: abc123
        message: "feat(auth): implement JWT"
        date: "2024-03-13"
      - commit: def456
        message: "test(auth): add tests"
        date: "2024-03-14"
```

### Gerar Relatório de Commits
```
/orchestration/sync --commit-report
```
Cria relatório de todos os commits relacionados a tarefas.

### Corrigir Commits Órfãos
```
/orchestration/sync --link-orphans
```
Associa commits sem referências de tarefas.

## Estratégias de Sincronização

### Conservadora
```
/orchestration/sync --conservative
```
Atualiza apenas com correspondências de alta confiança.

### Agressiva
```
/orchestration/sync --aggressive
```
Atualiza com base em qualquer evidência.

### Interativa
```
/orchestration/sync --interactive
```
Solicita confirmação para cada atualização potencial.

## Exemplos

### Exemplo 1: Sincronização Diária
```
/orchestration/sync --since yesterday

Quick sync results:
- 5 commits analyzed
- 2 tasks updated
- All changes applied successfully
```

### Exemplo 2: Sincronização de Merge de Branch
```
/orchestration/sync --after-merge feature/auth

Post-merge sync:
- 15 commits from feature/auth
- 5 tasks moved to completed
- 2 tasks have test failures (kept in qa)
```

### Exemplo 3: Modo Auditoria
```
/orchestration/sync --audit --report

Audit Report:
- Tasks with commits: 85%
- Commits with task refs: 92%
- Average commits per task: 2.3
- Orphaned commits: 3
```

## Integração com Webhook

### Sincronização Automática no Push
```yaml
git_hooks:
  post-commit: /orchestration/sync --last-commit
  post-merge: /orchestration/sync --branch HEAD
```

## Melhores Práticas

1. **Sincronizações Regulares**: Execute diariamente ou após commits importantes
2. **Revisar Antes de Forçar**: Verifique a saída do dry-run primeiro
3. **Manter Referências**: Inclua IDs de tarefas nos commits
4. **Lidar com Conflitos**: Não ignore avisos de sincronização
5. **Documentar Decisões**: Anote por que o status difere do git

## Configuração

### Preferências de Sincronização
```yaml
sync_config:
  auto_sync: true
  confidence_threshold: 0.8
  require_tests: true
  trust_git_over_tracker: true
  patterns:
    - implementation: "feat|feature"
    - testing: "test|spec"
    - completion: "done|complete|finish"
```

## Observações

- Requer acesso ao git em todos os branches relevantes
- Preserva sobrescritas manuais de status com flags
- Suporta padrões de mensagem de commit customizados
- Integra-se com CI/CD para sincronização automatizada