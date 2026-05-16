---
allowed-tools: Bash(git:*), Bash(cat:*), Bash(pwd:*), Bash(ls:*)
description: Verificar status da worktree atual, branch e tarefa atribuída
---

# Verificação de Status da Worktree

Verifique o ambiente da worktree atual e exiba os detalhes da tarefa.

## Instruções

Você está dentro de uma worktree (ou do repositório principal). Reúna e exiba o status atual de forma clara.

### Etapa 1: Detectar Worktree

1. Obtenha o diretório atual: `pwd`
2. Liste todas as worktrees: `git worktree list`
3. Determine se o diretório atual é uma worktree (não a árvore de trabalho principal). A árvore de trabalho principal é listada primeiro na saída de `git worktree list` — se o caminho atual corresponder à primeira entrada, este é o repositório principal, não uma worktree.

Se **não** for uma worktree, informe ao usuário:
> Você está no repositório principal, não em uma worktree. Use `/worktree-init` para criar worktrees.

Em seguida, liste as worktrees existentes e encerre.

### Etapa 2: Exibir Informações do Branch

1. Obtenha o branch atual: `git branch --show-current`
2. Verifique se segue a convenção de nomenclatura `claude/*`, `claude-daniel/*` ou `review/*`
3. Mostre quantos commits estão à frente de origin/main: `git rev-list --count origin/main..HEAD`

### Etapa 3: Ler Tarefa

1. Verifique se `.worktree-task.md` existe na raiz da worktree
2. Se existir, leia e exiba seu conteúdo
3. Se não existir, registre que nenhum arquivo de tarefa foi encontrado (pode ter sido criado manualmente)

### Etapa 4: Exibir Status de Trabalho

Execute e exiba:
1. `git status --short` — mostre arquivos modificados, staged e não rastreados
2. `git diff --stat` — mostre um resumo das alterações unstaged

### Etapa 5: Exibir Resumo

Apresente um resumo limpo:

```
Status da Worktree
──────────────────────────────────
Branch:      claude/<name>
Tarefa:      <descrição da tarefa de .worktree-task.md>
Commits:     <N> à frente de main
Modificados: <N> arquivos
Staged:      <N> arquivos
Não rastreados: <N> arquivos
──────────────────────────────────
```

Se houver alterações prontas para entregar, sugira: "Execute `/worktree-deliver` quando estiver pronto para fazer commit, push e criar um PR."