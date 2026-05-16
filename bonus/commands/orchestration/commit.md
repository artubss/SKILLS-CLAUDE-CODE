# Comando de Commit da Orquestração

Crie commits git alinhados com a conclusão de tarefas, mantendo um controle de versão limpo sincronizado com o progresso das tarefas.

## Uso

```
/orchestration/commit [TASK-ID] [options]
```

## Descrição

Cria automaticamente commits bem estruturados quando tarefas são movidas para QA ou conclusão, usando metadados da tarefa para gerar mensagens de commit significativas seguindo a especificação Conventional Commits.

## Comandos Básicos

### Fazer Commit da Tarefa Atual
```
/orchestration/commit
```
Faz commit das alterações da tarefa em progresso.

### Fazer Commit de Tarefa Específica
```
/orchestration/commit TASK-003
```
Faz commit das alterações relacionadas a uma tarefa específica.

### Commit em Lote
```
/orchestration/commit --batch
```
Agrupa tarefas concluídas relacionadas em commits lógicos.

## Geração de Mensagem de Commit

### Formato Automático
Baseado no tipo de tarefa e conteúdo:
```
feat(auth): implementar validação de token JWT

- Adicionar middleware de verificação de token
- Implementar lógica de token de atualização
- Adicionar tratamento de expiração

Task: TASK-003
Status: todos -> in_progress -> qa
Time: 4.5 hours
```

### Mapeamento de Tipo
```
Tipo de Tarefa    -> Tipo de Commit
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
feature           -> feat:
bugfix            -> fix:
refactor          -> refactor:
test              -> test:
docs              -> docs:
performance       -> perf:
security          -> fix:        (com nota de segurança)
```

## Integração com Workflow

### Auto-commit na Mudança de Status
```
/orchestration/move TASK-003 qa --auto-commit
```
Faz commit automaticamente ao mover para status QA.

### Validação Pré-commit
```
/orchestration/commit --validate
```
Verifica:
- Todos os testes passam
- Sem erros de linting
- Requisitos da tarefa atendidos
- Arquivos condizem com escopo da tarefa

## Opções

### Mensagem Personalizada
```
/orchestration/commit TASK-003 --message "Mensagem de commit personalizada"
```
Substitui a geração automática de mensagem.

### Detecção de Escopo
```
/orchestration/commit --detect-scope
```
Detecta automaticamente o escopo dos arquivos alterados:
- `auth` para arquivos relacionados a autenticação
- `api` para alterações de API
- `ui` para alterações de frontend

### Breaking Changes
```
/orchestration/commit --breaking
```
Adiciona indicador de breaking change:
```
feat(api)!: reestruturar endpoints de autenticação

BREAKING CHANGE: Endpoints de autenticação movidos de /auth para /api/v2/auth
```

## Operações em Lote

### Commit por Feature
```
/orchestration/commit --feature authentication
```
Agrupa todas as tarefas de autenticação concluídas em um commit.

### Commit por Status
```
/orchestration/commit --status qa
```
Faz commit de todas as tarefas atualmente em QA.

### Agrupamento Inteligente
```
/orchestration/commit --smart-group
```
Agrupa tarefas relacionadas inteligentemente:
```
Feature Group: Authentication (3 tasks)
- TASK-001: Database schema
- TASK-003: JWT implementation  
- TASK-005: Login endpoint

Suggested commit: feat(auth): implement complete authentication system
```

## Suporte a Worktree

### Commits Cientes de Worktree
```
/orchestration/commit --worktree
```
Detecta o worktree atual e faz commit apenas das tarefas relevantes.

### Status Entre Worktrees
```
/orchestration/commit --all-worktrees
```
Mostra status de commit entre todos os worktrees:
```
Worktree Status:
- feature/auth: 2 tasks ready to commit
- feature/payments: 1 task ready to commit
- feature/ui: No uncommitted changes
```

## Recursos de Validação

### Verificações Pré-commit
```
## Pre-commit Validation
✓ All tests passing
✓ No linting errors
✓ Task requirements met
✗ Uncommitted files outside task scope: src/unrelated.js

Proceed with commit? [y/n]
```

### Alinhamento de Tarefa
```
## Task Alignment Check
Changed files:
- src/auth/jwt.ts ✓ (matches TASK-003)
- src/auth/validate.ts ✓ (matches TASK-003)
- src/payments/stripe.ts ✗ (not in TASK-003 scope)

Warning: Changes outside task scope detected
```

## Recursos de Integração

### Vincular à Tarefa
```
/orchestration/commit --link-task
```
Adiciona URL/referência da tarefa ao commit:
```
feat(auth): implementar validação de JWT

Task: TASK-003
Link: http://orchestration/03_15_2024/auth_system/tasks/TASK-003
```

### Atualizar Rastreador de Status
```
/orchestration/commit --update-tracker
```
Atualiza TASK-STATUS-TRACKER.yaml com informações do commit:
```yaml
git_tracking:
  TASK-003:
    commits: ["abc123def"]
    commit_message: "feat(auth): implement JWT validation"
    committed_at: "2024-03-15T14:30:00Z"
```

## Exemplos

### Exemplo 1: Commit Simples de Tarefa
```
/orchestration/commit TASK-003

Generated commit:
feat(auth): implementar validação de token JWT

- Adicionar middleware de verificação
- Tratar expiração de token
- Implementar lógica de atualização

Task: TASK-003 (4.5 hours)
```

### Exemplo 2: Commit em Lote de Feature
```
/orchestration/commit --feature authentication --batch

Grouping 3 related tasks:
feat(auth): implementação completa do sistema de autenticação

- Configurar schema de banco de dados (TASK-001)
- Implementar validação JWT (TASK-003)
- Criar endpoints de login (TASK-005)

Tasks: TASK-001, TASK-003, TASK-005 (12 hours total)
```

### Exemplo 3: Fix com Teste
```
/orchestration/commit TASK-007

Generated commit:
fix(auth): resolver condição de corrida na expiração de token

- Corrigir problema de timing em validação assíncrona
- Adicionar cobertura de testes abrangente
- Prevenir edge case no fluxo de atualização

Fixes: #123
Task: TASK-007 (2 hours)
```

## Modelos de Commit

### Modelo de Feature
```
feat(<scope>): <task-title>

- <implementation-detail-1>
- <implementation-detail-2>
- <implementation-detail-3>

Task: <task-id> (<duration>)
Status: <status-transition>
```

### Modelo de Fix
```
fix(<scope>): <issue-description>

- <root-cause>
- <solution>
- <test-coverage>

Fixes: #<issue-number>
Task: <task-id>
```

## Boas Práticas

1. **Fazer Commit em Pontos Naturais**: Ao mover tarefas para QA
2. **Manter Commits Atômicos**: Uma mudança lógica por commit
3. **Usar Lote com Sabedoria**: Agrupar apenas tarefas verdadeiramente relacionadas
4. **Validar Primeiro**: Sempre executar validação antes de fazer commit
5. **Atualizar Status**: Garantir que o status da tarefa está atual

## Configuração

### Regras de Auto-commit
Defina na configuração de orquestração:
```yaml
auto_commit:
  on_qa: true
  on_complete: false
  require_tests: true
  require_validation: true
```

## Notas

- Integra-se com o agente task-commit-manager para cenários complexos
- Respeita .gitignore e arquivos excluídos
- Suporta especificação de conventional commits
- Mantém histórico rastreável entre tarefas e commits