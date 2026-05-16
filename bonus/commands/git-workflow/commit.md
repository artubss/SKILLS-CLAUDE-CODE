---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*), Bash(git log:*)
argument-hint: [message] | --no-verify | --amend
description: Criar commits bem formatados com formato de conventional commit e emoji
---

# Smart Git Commit

Criar commit bem formatado: $ARGUMENTS

## Estado Atual do Repositório

- Status do Git: !`git status --porcelain`
- Branch atual: !`git branch --show-current`
- Alterações em staging: !`git diff --cached --stat`
- Alterações não staged: !`git diff --stat`
- Commits recentes: !`git log --oneline -5`

## O Que Este Comando Faz

1. A menos que especificado com `--no-verify`, executa automaticamente verificações pré-commit:
   - `pnpm lint` para garantir qualidade do código
   - `pnpm build` para verificar se a compilação tem sucesso
   - `pnpm generate:docs` para atualizar documentação
2. Verifica quais arquivos estão em staging com `git status`
3. Se nenhum arquivo está em staging, adiciona automaticamente todos os arquivos modificados e novos com `git add`
4. Executa `git diff` para entender quais mudanças estão sendo commitadas
5. Analisa o diff para determinar se há múltiplas mudanças lógicas distintas
6. Se múltiplas mudanças distintas são detectadas, sugere dividir o commit em vários commits menores
7. Para cada commit (ou o único commit se não for dividido), cria uma mensagem de commit usando formato emoji conventional commit

## Melhores Práticas para Commits

- **Verifique antes de fazer commit**: Garanta que o código está lintado, compilado corretamente e a documentação foi atualizada
- **Commits atômicos**: Cada commit deve conter alterações relacionadas que servem um propósito único
- **Divida mudanças grandes**: Se as alterações tocam múltiplas preocupações, divida-as em commits separados
- **Formato conventional commit**: Use o formato `<tipo>: <descrição>` onde tipo é um dos seguintes:
  - `feat`: Uma nova funcionalidade
  - `fix`: Uma correção de bug
  - `docs`: Alterações em documentação
  - `style`: Alterações de estilo de código (formatação, etc)
  - `refactor`: Alterações de código que não corrigem bugs nem adicionam funcionalidades
  - `perf`: Melhorias de performance
  - `test`: Adicionando ou corrigindo testes
  - `chore`: Alterações no processo de compilação, ferramentas, etc.
- **Tempo presente, modo imperativo**: Escreva mensagens de commit como comandos (ex: "adicionar funcionalidade" e não "adicionada funcionalidade")
- **Primeira linha concisa**: Mantenha a primeira linha com menos de 72 caracteres
- **Emoji**: Cada tipo de commit é associado com um emoji apropriado:
  - ✨ `feat`: Nova funcionalidade
  - 🐛 `fix`: Correção de bug
  - 📝 `docs`: Documentação
  - 💄 `style`: Formatação/estilo
  - ♻️ `refactor`: Refatoração de código
  - ⚡️ `perf`: Melhorias de performance
  - ✅ `test`: Testes
  - 🔧 `chore`: Ferramentas, configuração
  - 🚀 `ci`: Melhorias de CI/CD
  - 🗑️ `revert`: Reverter mudanças
  - 🧪 `test`: Adicionar um teste falhando
  - 🚨 `fix`: Corrigir avisos de compilador/linter
  - 🔒️ `fix`: Corrigir problemas de segurança
  - 👥 `chore`: Adicionar ou atualizar contribuintes
  - 🚚 `refactor`: Mover ou renomear recursos
  - 🏗️ `refactor`: Fazer alterações arquiteturais
  - 🔀 `chore`: Mesclar branches
  - 📦️ `chore`: Adicionar ou atualizar arquivos compilados ou pacotes
  - ➕ `chore`: Adicionar uma dependência
  - ➖ `chore`: Remover uma dependência
  - 🌱 `chore`: Adicionar ou atualizar arquivos seed
  - 🧑‍💻 `chore`: Melhorar experiência do desenvolvedor
  - 🧵 `feat`: Adicionar ou atualizar código relacionado a multithreading ou concorrência
  - 🔍️ `feat`: Melhorar SEO
  - 🏷️ `feat`: Adicionar ou atualizar tipos
  - 💬 `feat`: Adicionar ou atualizar texto e literais
  - 🌐 `feat`: Internacionalização e localização
  - 👔 `feat`: Adicionar ou atualizar lógica de negócio
  - 📱 `feat`: Trabalhar em design responsivo
  - 🚸 `feat`: Melhorar experiência/usabilidade do usuário
  - 🩹 `fix`: Correção simples para um problema não crítico
  - 🥅 `fix`: Capturar erros
  - 👽️ `fix`: Atualizar código devido a mudanças de API externa
  - 🔥 `fix`: Remover código ou arquivos
  - 🎨 `style`: Melhorar estrutura/formato do código
  - 🚑️ `fix`: Hotfix crítico
  - 🎉 `chore`: Iniciar um projeto
  - 🔖 `chore`: Tags de release/versão
  - 🚧 `wip`: Work in progress
  - 💚 `fix`: Corrigir compilação CI
  - 📌 `chore`: Fixar dependências a versões específicas
  - 👷 `ci`: Adicionar ou atualizar sistema de compilação CI
  - 📈 `feat`: Adicionar ou atualizar código de analytics ou rastreamento
  - ✏️ `fix`: Corrigir erros de digitação
  - ⏪️ `revert`: Reverter mudanças
  - 📄 `chore`: Adicionar ou atualizar licença
  - 💥 `feat`: Introduzir breaking changes
  - 🍱 `assets`: Adicionar ou atualizar assets
  - ♿️ `feat`: Melhorar acessibilidade
  - 💡 `docs`: Adicionar ou atualizar comentários no código fonte
  - 🗃️ `db`: Realizar alterações relacionadas ao banco de dados
  - 🔊 `feat`: Adicionar ou atualizar logs
  - 🔇 `fix`: Remover logs
  - 🤡 `test`: Mock de coisas
  - 🥚 `feat`: Adicionar ou atualizar um easter egg
  - 🙈 `chore`: Adicionar ou atualizar arquivo .gitignore
  - 📸 `test`: Adicionar ou atualizar snapshots
  - ⚗️ `experiment`: Realizar experimentos
  - 🚩 `feat`: Adicionar, atualizar ou remover feature flags
  - 💫 `ui`: Adicionar ou atualizar animações e transições
  - ⚰️ `refactor`: Remover código morto
  - 🦺 `feat`: Adicionar ou atualizar código relacionado a validação
  - ✈️ `feat`: Melhorar suporte offline

## Diretrizes para Dividir Commits

Ao analisar o diff, considere dividir commits baseado nestes critérios:

1. **Preocupações diferentes**: Alterações em partes não relacionadas da codebase
2. **Diferentes tipos de alterações**: Misturar funcionalidades, correções, refatoração, etc.
3. **Padrões de arquivo**: Alterações para diferentes tipos de arquivos (ex: código fonte vs documentação)
4. **Agrupamento lógico**: Alterações que seriam mais fáceis de entender ou revisar separadamente
5. **Tamanho**: Alterações muito grandes que ficariam mais claras se divididas

## Exemplos

Boas mensagens de commit:
- ✨ feat: adicionar sistema de autenticação de usuário
- 🐛 fix: resolver vazamento de memória no processo de renderização
- 📝 docs: atualizar documentação da API com novos endpoints
- ♻️ refactor: simplificar lógica de tratamento de erros no parser
- 🚨 fix: resolver avisos de linter em arquivos de componente
- 🧑‍💻 chore: melhorar processo de configuração de ferramentas do desenvolvedor
- 👔 feat: implementar lógica de negócio para validação de transações
- 🩹 fix: corrigir inconsistência de estilo menor no header
- 🚑️ fix: corrigir vulnerabilidade de segurança crítica no fluxo de autenticação
- 🎨 style: reorganizar estrutura de componente para melhor legibilidade
- 🔥 fix: remover código legado descontinuado
- 🦺 feat: adicionar validação de entrada para formulário de registro de usuário
- 💚 fix: resolver testes falhando no pipeline CI
- 📈 feat: implementar rastreamento de analytics para engajamento do usuário
- 🔒️ fix: fortalecer requisitos de senha de autenticação
- ♿️ feat: melhorar acessibilidade de formulário para leitores de tela

Exemplo de dividir commits:
- Primeiro commit: ✨ feat: adicionar definições de tipo para nova versão solc
- Segundo commit: 📝 docs: atualizar documentação para novas versões solc
- Terceiro commit: 🔧 chore: atualizar dependências do package.json
- Quarto commit: 🏷️ feat: adicionar definições de tipo para novos endpoints da API
- Quinto commit: 🧵 feat: melhorar tratamento de concorrência em threads workers
- Sexto commit: 🚨 fix: resolver problemas de lint no novo código
- Sétimo commit: ✅ test: adicionar testes unitários para novos recursos de versão solc
- Oitavo commit: 🔒️ fix: atualizar dependências com vulnerabilidades de segurança

## Opções de Comando

- `--no-verify`: Pular as verificações pré-commit (lint, build, generate:docs)

## Notas Importantes

- Por padrão, verificações pré-commit (`pnpm lint`, `pnpm build`, `pnpm generate:docs`) serão executadas para garantir qualidade do código
- Se essas verificações falharem, você será perguntado se deseja prosseguir com o commit mesmo assim ou corrigir os problemas primeiro
- Se arquivos específicos já estão em staging, o comando apenas commitará esses arquivos
- Se nenhum arquivo está em staging, ele automaticamente fará staging de todos os arquivos modificados e novos
- A mensagem de commit será construída baseada nas alterações detectadas
- Antes de fazer commit, o comando revisará o diff para identificar se múltiplos commits seriam mais apropriados
- Se sugerindo múltiplos commits, ajudará você a fazer staging e committar as alterações separadamente
- Sempre revisa o diff do commit para garantir que a mensagem corresponde às alterações