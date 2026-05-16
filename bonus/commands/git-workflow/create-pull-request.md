# Como Criar uma Pull Request Usando GitHub CLI

Este guia explica como criar pull requests usando GitHub CLI em nosso projeto.

## Pré-requisitos

1. Instale GitHub CLI, se não tiver feito ainda:

   ```bash
   # macOS
   brew install gh

   # Windows
   winget install --id GitHub.cli

   # Linux
   # Siga as instruções em https://github.com/cli/cli/blob/trunk/docs/install_linux.md
   ```

2. Autentique-se no GitHub:
   ```bash
   gh auth login
   ```

## Criando uma Nova Pull Request

1. Primeiro, prepare sua descrição de PR seguindo o template em `.github/pull_request_template.md`

2. Use o comando `gh pr create` para criar uma nova pull request:

   ```bash
   # Estrutura básica do comando
   gh pr create --title "✨(scope): Seu título descritivo" --body "Sua descrição de PR" --base main --draft
   ```

   Para descrições de PR mais complexas com formatação adequada, use a opção `--body-file` com a estrutura exata do template de PR:

   ```bash
   # Criar PR com estrutura de template adequada
   gh pr create --title "✨(scope): Seu título descritivo" --body-file <(echo -e "## Issue\n\n- resolve:\n\n## Por que essa mudança é necessária?\nSua descrição aqui.\n\n## Em que os revisores devem focar?\n- Ponto 1\n- Ponto 2\n\n## Verificação de Testes\nComo você testou essas mudanças.\n\n## O que foi feito\npr_agent:summary\n\n## Alterações Detalhadas\npr_agent:walkthrough\n\n## Notas Adicionais\nQuaisquer notas adicionais.") --base main --draft
   ```

## Melhores Práticas

1. **Formato do Título de PR**: Use o formato de commit convencional com emojis

   - Sempre inclua um emoji apropriado no início do título
   - Use o caractere emoji real (não a representação de código como `:sparkles:`)
   - Exemplos:
     - `✨(supabase): Adicionar configuração de remote de staging`
     - `🐛(auth): Corrigir problema de redirecionamento de login`
     - `📝(readme): Atualizar instruções de instalação`

2. **Template de Descrição**: Sempre use a estrutura do nosso template de PR em `.github/pull_request_template.md`:

   - Referência de issue
   - Por que a mudança é necessária
   - Pontos de foco para revisão
   - Verificação de testes
   - Seções do PR-Agent (mantenha as tags `pr_agent:summary` e `pr_agent:walkthrough` intactas)
   - Notas adicionais

3. **Precisão do Template**: Certifique-se de que sua descrição de PR segue precisamente a estrutura do template:

   - Não modifique ou renomeie as seções do PR-Agent (`pr_agent:summary` e `pr_agent:walkthrough`)
   - Mantenha todos os cabeçalhos de seção exatamente como aparecem no template
   - Não adicione seções personalizadas que não estão no template

4. **Draft PRs**: Comece como draft quando o trabalho está em andamento
   - Use a flag `--draft` no comando
   - Converta para pronto para revisão quando completo usando `gh pr ready`

### Erros Comuns a Evitar

1. **Cabeçalhos de Seção Incorretos**: Sempre use os cabeçalhos de seção exatos do template
2. **Modificar Seções do PR-Agent**: Não remova ou modifique os placeholders `pr_agent:summary` e `pr_agent:walkthrough`
3. **Adicionar Seções Personalizadas**: Mantenha-se fiel às seções definidas no template
4. **Usar Templates Desatualizados**: Sempre consulte o arquivo `.github/pull_request_template.md` atual

### Seções Ausentes

Sempre inclua todas as seções do template, mesmo que algumas sejam marcadas como "N/A" ou "Nenhuma"

## Comandos Adicionais do GitHub CLI para PR

Aqui estão alguns comandos adicionais úteis do GitHub CLI para gerenciar PRs:

```bash
# Listar suas pull requests abertas
gh pr list --author "@me"

# Verificar status da PR
gh pr status

# Visualizar uma PR específica
gh pr view <PR-NUMBER>

# Fazer checkout de um branch de PR localmente
gh pr checkout <PR-NUMBER>

# Converter uma PR draft para pronto para revisão
gh pr ready <PR-NUMBER>

# Adicionar revisores a uma PR
gh pr edit <PR-NUMBER> --add-reviewer username1,username2

# Fazer merge de uma PR
gh pr merge <PR-NUMBER> --squash
```

## Usando Templates para Criação de PR

Para simplificar a criação de PR com descrições consistentes, você pode criar um arquivo de template:

1. Crie um arquivo chamado `pr-template.md` com seu template de PR
2. Use-o ao criar PRs:

```bash
gh pr create --title "feat(scope): Seu título" --body-file pr-template.md --base main --draft
```

## Documentação Relacionada

- [PR Template](.github/pull_request_template.md)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Documentação do GitHub CLI](https://cli.github.com/manual/)