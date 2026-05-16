---
allowed-tools: Bash(git:*), Bash(mkdir:*), Bash(ls:*), Bash(cat:*), Bash(basename:*), Bash(pwd:*), Bash(sed:*)
argument-hint: tarefa 1 | tarefa 2 | tarefa 3
description: Criar worktrees paralelas para desenvolvimento multi-tarefa com painéis Ghostty
---

# Inicialização de Worktree Paralela

Criar múltiplas worktrees git para desenvolvimento paralelo: $ARGUMENTS

## Instruções

Você está configurando worktrees paralelas para que o usuário possa trabalhar em múltiplas tarefas simultaneamente em painéis de terminal Ghostty separados, cada um executando sua própria instância Claude.

### Etapa 1: Validar Ambiente

1. Verificar se é um repositório git: `git rev-parse --is-inside-work-tree`
2. Obter o nome do repositório: `basename $(git rev-parse --show-toplevel)`
3. Obter o nome da branch principal (verificar `main` ou `master`): `git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@'` — se falhar, usar `main` como padrão
4. Garantir que a árvore de trabalho está limpa: `git status --porcelain`. Se houver mudanças, avisar o usuário e perguntar se deseja continuar.
5. Buscar atualizações: `git fetch origin`

### Etapa 2: Fazer Parse das Tarefas

Fazer parse das tarefas de `$ARGUMENTS`. As tarefas são separadas por `|` (caractere pipe).

Se `$ARGUMENTS` estiver vazio, usar AskUserQuestion para solicitar ao usuário que descreva suas tarefas (podem fornecer múltiplas separadas por `|`).

Para cada descrição de tarefa:
- Remover espaços em branco
- Gerar um nome de branch em kebab-case: `claude/<kebab-case-tarefa>` (máximo 50 caracteres, apenas alfanuméricos e hífens)
- Gerar um caminho de diretório para worktree: `../worktrees/<nome-repo>/claude-<kebab-case-tarefa>`

### Etapa 3: Criar Worktrees

Para cada tarefa:

1. Criar o diretório pai se necessário: `mkdir -p ../worktrees/<nome-repo>`
2. Criar a worktree:
   ```bash
   git worktree add -b claude/<nome> ../worktrees/<nome-repo>/claude-<nome> origin/<branch-principal>
   ```
3. Escrever um arquivo `.worktree-task.md` dentro da nova worktree com este conteúdo:
   ```markdown
   # Tarefa da Worktree

   **Branch:** claude/<nome>
   **Tarefa:** <descrição original da tarefa>
   **Criada:** <data ISO>
   **Repositório de origem:** <caminho para repo principal>
   ```

### Etapa 4: Verificar Dependências

Se um `package.json` existir na raiz do repositório, observar que cada worktree pode precisar de `npm install` (ou do gerenciador de pacotes apropriado).

Verificar:
- `package-lock.json` → npm install
- `yarn.lock` → yarn install
- `pnpm-lock.yaml` → pnpm install
- `bun.lockb` → bun install

### Etapa 5: Exibir Resumo

Exibir uma tabela de resumo clara:

```
| # | Tarefa | Branch | Caminho |
|---|--------|--------|---------|
| 1 | ... | claude/... | ../worktrees/repo/claude-... |
```

Depois exibir comandos prontos para copiar para painéis Ghostty. Para cada worktree:

```
# Painel <N>: <descrição da tarefa>
cd <caminho-absoluto-para-worktree> && claude
```

Se dependências foram detectadas, adicionar uma nota:
```
# Nota: Execute <gerenciador-pacotes> install em cada worktree antes de começar
```

Finalmente, relembrar o usuário:
- Abrir um novo painel Ghostty com `Cmd+D` (dividir à direita) ou `Cmd+Shift+D` (dividir para baixo)
- Quando terminar uma tarefa, usar `/worktree-deliver` para fazer commit, push e criar um PR
- Depois de mesclar todos os PRs, usar `/worktree-cleanup --all` do repositório principal