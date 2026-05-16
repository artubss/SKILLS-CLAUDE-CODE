---
allowed-tools: Bash(git:*), Bash(rm:*), Bash(ls:*), Bash(pwd:*), Bash(grep:*)
argument-hint: --all | --branch claude/name | --dry-run
description: Limpar worktrees mescladas e seus branches
---

# Limpeza de Worktree

Remover worktrees e branches que foram mesclados: $ARGUMENTS

## Instruções

Você está no **repositório principal** (não em uma worktree). Limpe as worktrees concluídas.

### Padrões de Branch

Este projeto usa os seguintes prefixos de branch para worktrees:
- `claude/*` — worktrees auto-criadas pelo Claude Code
- `claude-daniel/*` — worktrees criadas pelo usuário
- `review/*` — worktrees de revisão de componentes

Todos os três prefixos devem ser verificados em cada etapa abaixo.

### Etapa 1: Validar Ambiente

1. Verifique se esta é a árvore de trabalho principal (primeira entrada em `git worktree list`)
2. Se estiver dentro de uma worktree, avise: "Execute `/worktree-cleanup` do repositório principal, não de uma worktree."
3. Busque o mais recente da origin: `git fetch origin --prune`
4. Obtenha o nome do branch principal (main ou master)

### Etapa 2: Analisar Argumentos

Analise `$ARGUMENTS` para opções:

- `--all` — limpar TODAS as worktrees e branches mesclados
- `--branch <prefix>/<name>` — limpar uma worktree/branch específica
- `--dry-run` — mostrar o que seria limpo sem fazer nada
- `--force-all` — remover TODAS as worktrees independentemente do status de mesclagem (pede confirmação por worktree)
- Sem argumentos — listar worktrees e perguntar qual limpar

### Etapa 3: Identificar Worktrees

1. Listar todas as worktrees: `git worktree list`
2. Listar todos os branches correspondentes:
   ```bash
   git branch --list 'claude/*' 'claude-daniel/*' 'review/*'
   ```
3. Para cada branch correspondente, verifique se foi mesclado em main:
   ```bash
   git branch --merged origin/<main-branch> | grep -E '^\s+(claude/|claude-daniel/|review/)'
   ```
4. Também verifique branches remotos:
   ```bash
   git branch -r --merged origin/<main-branch> | grep -E 'origin/(claude/|claude-daniel/|review/)'
   ```
5. Para branches mescladas com squash (não detectadas por `--merged`), verifique se o diff do branch está vazio em relação a main:
   ```bash
   # Um branch está efetivamente mesclado se suas mudanças já existem em main
   git diff origin/main...<branch> --stat
   ```
   Se o diff estiver vazio ou muito pequeno (apenas whitespace), considere como mesclado.

### Etapa 4: Exibir Status

Mostre uma tabela de todas as worktrees/branches:

```
| # | Worktree | Branch | Mesclado? | Limpo? | Ação |
|---|---------|--------|-----------|--------|--------|
| 1 | eager-mendeleev | claude/eager-mendeleev | Sim | Sim | Será removido |
| 2 | agent-a7e312d0 | review/code-reviewer-2026-04-01 | Não | Sim | Ignorado |
```

### Etapa 5: Confirmar e Executar

Se `--dry-run` foi especificado, mostre a tabela e pare.

Caso contrário, use AskUserQuestion para confirmar limpeza (a menos que `--all` tenha sido especificado com apenas branches mesclados).

Para cada worktree/branch a limpar:

1. Remover a worktree:
   ```bash
   git worktree remove <path>
   ```
   Se falhar (worktree com mudanças), avise e pule — **nunca force a remoção**.

2. Deletar o branch local:
   ```bash
   git branch -d <branch>
   ```
   Use `-d` (não `-D`) para branches mesclados. Para branches não mesclados com `--force-all`, use `-D` apenas após confirmação explícita do usuário.

3. Deletar o branch remoto (se existir):
   ```bash
   git push origin --delete <branch>
   ```
   Se o branch remoto não existir, ignore o erro silenciosamente.

### Etapa 6: Limpar

Após todas as remoções:

```bash
git worktree prune
```

### Etapa 7: Resumo

Mostre o que foi limpo:

```
Limpeza Concluída
──────────────────────────────────
Removidas:  <N> worktree(s)
Deletados:  <N> branch(es) local(is)
Deletados:  <N> branch(es) remoto(s)
Ignorados:  <N> branch(es) não mesclado(s)
──────────────────────────────────
```

Se algum branch não mesclado foi ignorado, liste-os e sugira:
- Mesclar o PR primeiro, depois executar limpeza novamente
- Ou use `git worktree remove <path>` e `git branch -D <branch>` manualmente se o trabalho foi realmente abandonado