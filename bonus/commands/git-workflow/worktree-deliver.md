---
allowed-tools: Bash(git:*), Bash(gh:*), Bash(rm:*), Bash(cat:*), Bash(pwd:*), Bash(ls:*)
description: Fazer commit, push e criar PR a partir da worktree atual
---

# Worktree Deliver

Faça commit de todo o trabalho, push e crie um pull request a partir da worktree atual.

## Instruções

Você está dentro de uma worktree. Empacote o trabalho e entregue como PR.

### Etapa 1: Validar Ambiente

1. Verifique se esta é uma worktree (não a árvore de trabalho principal) usando `git worktree list`
2. Obtenha a branch atual: `git branch --show-current`
3. Verifique se a branch segue o padrão `claude/*`, `claude-daniel/*` ou `review/*`. Se não, avise o usuário e pergunte se deseja continuar.
4. Leia `.worktree-task.md` se existir para obter a descrição da tarefa original

### Etapa 2: Revisar Alterações

1. Execute `git diff --stat` e `git diff --cached --stat` para mostrar todas as alterações
2. Execute `git status --short` para obter a visão completa
3. Se não houver alterações (árvore de trabalho limpa, sem commits à frente da main), informe ao usuário que não há nada para entregar e pare.

### Etapa 3: Limpar Arquivo de Tarefa

Antes de fazer stage, remova o arquivo de tarefa da worktree para que não seja incluído no commit:

```bash
rm -f .worktree-task.md
```

### Etapa 4: Confirmar Arquivos para Commit

Use AskUserQuestion para mostrar ao usuário o que será feito commit e peça confirmação. Liste todos os arquivos modificados, adicionados e não rastreados.

Opções:
- "Fazer stage de todas as alterações" — fazer stage de tudo
- "Deixe-me escolher" — o usuário especificará quais arquivos incluir

Se o usuário quiser escolher, pergunte quais arquivos fazer stage.

### Etapa 5: Fazer Stage e Commit

1. Faça stage dos arquivos confirmados com `git add`
2. Gere uma mensagem de commit seguindo o formato conventional commits

**Estratégia de Mensagem de Commit:**

   1. **Analise o diff** para determinar o tipo de conventional commit:
      - `feat:` — Nova funcionalidade, novos arquivos, novas exportações, novos endpoints de API
      - `fix:` — Correções de bugs, correções de erros, corrigindo comportamento quebrado
      - `refactor:` — Reestruturação de código sem alterar o comportamento
      - `docs:` — Apenas alterações de documentação
      - `test:` — Adicionando ou modificando testes
      - `chore:` — Scripts de build, configs, tarefas de manutenção

   2. **Gere uma mensagem de commit** baseada em:
      - Descrição da tarefa em `.worktree-task.md` (se foi encontrado)
      - Um breve resumo do que o diff realmente mudou
      - Formato: `<type>: <subject>` (máximo 72 caracteres)

   3. **Mostre a mensagem proposta** ao usuário com AskUserQuestion:
      - Exiba a mensagem gerada claramente
      - Opções: "Usar esta mensagem" / "Deixe-me escrever a minha"

   4. **Se o usuário escolher escrever a sua:**
      - Peça que forneça sua mensagem de commit
      - Valide se segue o formato conventional commits (avise se não, mas permita)

   5. **Sempre inclua corpo e co-author:**
      - Adicione um breve corpo resumindo o que mudou (2-3 pontos se múltiplas alterações)
      - Inclua a linha de co-author padrão

3. Crie o commit com a mensagem usando um HEREDOC:
   ```bash
   git commit -m "$(cat <<'EOF'
   <mensagem de commit aqui>
   EOF
   )"
   ```

### Etapa 6: Push

Faça push da branch para origin:

```bash
git push -u origin HEAD
```

Se push falhar por não ter upstream, a flag `-u` deve lidar. Se falhar por outro motivo, mostre o erro e sugira correções.

### Etapa 7: Criar Pull Request

1. Determine a branch base (main ou master) usando a mesma detecção que worktree-init
2. Crie o PR usando `gh pr create`:

```bash
gh pr create --base <main-branch> --title "<PR title>" --body "$(cat <<'EOF'
## Resumo

<pontos descrevendo as alterações baseadas na descrição da tarefa e diff>

## Tarefa Original

<descrição da tarefa de .worktree-task.md>

## Alterações

<resumo git diff --stat>

---
Criado a partir da worktree `claude/<name>` usando `/worktree-deliver`
EOF
)"
```

3. Exiba a URL do PR de forma destacada

### Etapa 8: Próximas Etapas

Diga ao usuário:
- PR está pronto para revisão em `<URL>`
- Após fazer merge, execute `/worktree-cleanup` do repositório principal para limpar
- Pode fechar este painel de terminal