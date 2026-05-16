# Comando Remover Orquestração

Remova com segurança uma tarefa do sistema de orquestração, atualizando todas as referências e dependências.

## Uso

```
/orchestration/remove TASK-ID [options]
```

## Descrição

Remove uma tarefa completamente do sistema de orquestração, gerenciando todas as dependências, referências e documentação relacionada. Fornece análise de impacto antes da remoção e garante consistência do sistema.

## Comandos Básicos

### Remover Tarefa Única
```
/orchestration/remove TASK-003
```
Exibe análise de impacto e confirma antes da remoção.

### Forçar Remoção
```
/orchestration/remove TASK-003 --force
```
Ignora confirmação (use com cuidado).

### Simulação
```
/orchestration/remove TASK-003 --dry-run
```
Mostra o que seria afetado sem fazer alterações.

## Análise de Impacto

Antes da remoção, o sistema analisa:

```
Task Removal Impact Analysis: TASK-003
======================================

Task Details:
- Title: JWT token validation
- Status: in_progress
- Location: /tasks/in_progress/TASK-003-jwt-validation.md

Dependencies:
- Blocks: TASK-005 (User profile API)
- Blocks: TASK-007 (Session management)
- Depends on: None

References Found:
- MASTER-COORDINATION.md: Line 45 (Wave 1 tasks)
- EXECUTION-TRACKER.md: Active task count
- TASK-005: Lists TASK-003 as dependency
- TASK-007: Lists TASK-003 as dependency

Git History:
- 2 commits reference this task
- Branch: feature/jwt-auth

Warning: This task has downstream dependencies!

Proceed with removal? [y/N]
```

## Processo de Remoção

### 1. Atualizar Tarefas Dependentes
```
Updating dependent tasks:
- TASK-005: Removing dependency on TASK-003
  New status: Ready to start (no blockers)
  
- TASK-007: Removing dependency on TASK-003
  Warning: Still blocked by TASK-009
```

### 2. Atualizar Arquivos de Rastreamento
```yaml
# TASK-STATUS-TRACKER.yaml updates:
status_history:
  TASK-003: [REMOVED - archived to .removed/]
  
current_status_summary:
  in_progress: [TASK-003 removed from list]

removal_log:
  - task_id: TASK-003
    removed_at: "2024-03-15T16:00:00Z"
    removed_by: "user"
    reason: "Requirement changed"
    final_status: "in_progress"
```

### 3. Atualizar Documentos de Coordenação
```
Updates applied:
✓ MASTER-COORDINATION.md - Removed from Wave 1
✓ EXECUTION-TRACKER.md - Updated task counts
✓ TASK-DEPENDENCIES.yaml - Removed all references
✓ Dependency graph regenerated
```

## Opções

### Arquivar em Vez de Deletar
```
/orchestration/remove TASK-003 --archive
```
Move para o diretório `.removed/` em vez de deletar.

### Remover Múltiplas Tarefas
```
/orchestration/remove TASK-003,TASK-005,TASK-008
```
Analisa e remove múltiplas tarefas em ordem de dependência.

### Remover por Padrão
```
/orchestration/remove --pattern "oauth-*"
```
Remove todas as tarefas correspondentes ao padrão.

### Remoção em Cascata
```
/orchestration/remove TASK-003 --cascade
```
Também remove tarefas que dependem desta tarefa.

## Tratamento de Casos Especiais

### Tarefa com Commits
```
Warning: TASK-003 has associated commits:
- abc123: "feat(auth): implement JWT validation"
- def456: "test(auth): add JWT tests"

Options:
[1] Keep commits, remove task only
[2] Add removal note to commit messages
[3] Cancel removal
```

### Tarefa em QA/Concluída
```
Warning: TASK-003 is in 'completed' status

This usually means work was done. Consider:
[1] Archive task instead of removing
[2] Document why it's being removed
[3] Check if commits should be reverted
```

### Tarefa no Caminho Crítico
```
ERROR: TASK-003 is on the critical path!

Removing this task will impact project timeline:
- Current completion: 5 days
- After removal: 7 days (due to replanning)

Override with --force-critical
```

## Estratégias de Remoção

### Remoção Suave (Padrão)
```
/orchestration/remove TASK-003
```
- Arquiva arquivo de tarefa
- Atualiza todas as referências
- Registra motivo da remoção
- Preserva histórico do git

### Remoção Permanente
```
/orchestration/remove TASK-003 --hard
```
- Deleta arquivo de tarefa permanentemente
- Remove todos os rastros
- Atualiza rastreamento do git
- Sem possibilidade de recuperação

### Remoção com Substituição
```
/orchestration/remove TASK-003 --replace-with TASK-015
```
- Transfere dependências para nova tarefa
- Atualiza todas as referências
- Mantém continuidade

## Capacidades de Desfazer

### Remoção Recente
```
/orchestration/remove --undo-last
```
Restaura a tarefa removida mais recentemente.

### Restaurar do Arquivo
```
/orchestration/remove --restore TASK-003
```
Restaura tarefa arquivada com todas as referências.

## Exemplos

### Exemplo 1: Recurso Obsoleto
```
/orchestration/remove TASK-008 --reason "Feature descoped"

Removing TASK-008: OAuth provider integration
- No dependencies
- No commits yet
- Safe to remove

Task removed successfully.
```

### Exemplo 2: Tarefa Duplicada
```
/orchestration/remove TASK-012 --replace-with TASK-005

Removing duplicate: TASK-012
Transferring to: TASK-005
- Dependencies transferred: 2
- References updated: 4

Duplicate removed, TASK-005 updated.
```

### Exemplo 3: Requisitos Alterados
```
/orchestration/remove TASK-003,TASK-004,TASK-005 --reason "Auth system redesigned"

Removing authentication task group:
- 3 tasks to remove
- 2 have commits (will archive)
- 5 dependent tasks need updates

Proceed? [y/N]
```

## Trilha de Auditoria

Todas as remoções são registradas:
```yaml
# .orchestration-audit.yaml
removals:
  - task_id: TASK-003
    removed_at: "2024-03-15T16:00:00Z"
    removed_by: "user-id"
    reason: "Requirement changed"
    status_at_removal: "in_progress"
    dependencies_affected: ["TASK-005", "TASK-007"]
    commits_preserved: ["abc123", "def456"]
    archived_to: ".removed/2024-03-15/TASK-003/"
```

## Melhores Práticas

1. **Sempre Verifique Dependências**: Revise impacto antes de remover
2. **Documente Motivo**: Forneça razão clara para a remoção
3. **Archive Trabalho Importante**: Use --archive para trabalho concluído
4. **Atualize o Time**: Notifique sobre remoções críticas
5. **Revise Commits**: Verifique se código precisa ser revertido

## Integração

### Com Outros Comandos
```
# Primeiro verifique o status
/orchestration/status --task TASK-003

# Depois remova se necessário
/orchestration/remove TASK-003
```

### Operações em Lote
```
# Encontre e remova todas as tarefas pausadas com mais de 30 dias
/orchestration/find --status on_hold --older-than 30d | /orchestration/remove --batch
```

## Recursos de Segurança

- Confirmação obrigatória (a menos que --force)
- Dependências verificadas e alertadas
- Commits preservados por padrão
- Trilha de auditoria mantida
- Capacidade de desfazer para remoções recentes

## Notas

- Tarefas removidas são arquivadas por 30 dias por padrão
- Commits do git nunca são automaticamente revertidos
- Dependências são gerenciadas graciosamente
- Consistência do sistema é mantida durante todo o processo