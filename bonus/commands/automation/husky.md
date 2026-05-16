---
allowed-tools: Bash, Read
argument-hint: [--skip-install] | [--only-lint] | [--skip-tests]
description: Executar verificações CI abrangentes e corrigir problemas até que o repositório esteja em estado funcional
---

# Verificações Husky CI Pré-commit

Executar verificações CI abrangentes e corrigir problemas: $ARGUMENTS

## Estado Atual do Repositório

- Status Git: !`git status --porcelain`
- Gerenciador de pacotes: !`which pnpm npm yarn | head -1`
- Branch atual: !`git branch --show-current`
- Package.json: @package.json
- Arquivo de ambiente: @.env (se existir)

## Tarefa

Verificar se o repositório está em estado funcional e corrigir problemas. Todos os comandos são executados a partir da raiz do repositório.

## Protocolo de Verificação CI

### Etapa 0: Configuração de Ambiente
- Atualizar dependências: `pnpm i` (a menos que --skip-install)
- Carregar ambiente: arquivo `.env` se existir

### Etapa 1: Linting
- Verificar se o linter passa: `pnpm lint`
- Corrigir problemas de formatação automaticamente quando possível

### Etapa 2: TypeScript e Build
- Executar verificações de build abrangentes:
  ```bash
  pnpm nx run-many --targets=build:types,build:dist,build:app,generate:docs,dev:run,typecheck
  ```
- Se um comando específico falhar, depurar esse comando individualmente
- Corrigir erros TypeScript e problemas de build

### Etapa 3: Cobertura de Testes
- Carregar arquivo `.env` primeiro, se existir
- Executar cobertura de testes: `pnpm nx run-many --target=test:coverage`
- **NUNCA** executar comando de teste normal (causa timeout)
- Executar pacotes individuais um por um para facilitar a depuração
- Para falhas de teste de snapshot: explicar a tese antes de atualizar snapshots

### Etapa 4: Validação de Pacotes
- Ordenar package.json: `pnpm run sort-package-json`
- Fazer lint de pacotes: `pnpm nx run-many --targets=lint:package,lint:deps`

### Etapa 5: Verificação Dupla
- Se correções foram feitas em qualquer etapa, re-executar todas as verificações anteriores
- Garantir que nenhuma regressão foi introduzida

### Etapa 6: Staging
- Verificar status: `git status`
- Adicionar arquivos: `git add`
- **EXCLUIR**: Submódulos Git nas pastas `lib/*`
- **NÃO FAZER COMMIT**: Apenas preparar (stage) os arquivos

## Protocolo de Tratamento de Erros

### 1. Diagnóstico
- Explicar por que o comando falhou com análise completa
- Citar código-fonte e logs que apoiam a tese
- Adicionar console.logs se necessário para confirmação
- Pedir ajuda se o contexto for insuficiente

### 2. Implementação de Correção
- Propor correção específica com explicação completa
- Explicar por que a correção funcionará
- Se a correção falhar, voltar à Etapa 1

### 3. Análise de Impacto
- Considerar se o mesmo bug existe em outros lugares
- Pesquisar no código por padrões semelhantes
- Corrigir proativamente problemas relacionados

### 4. Limpeza
- Remover todos os console.logs adicionados após corrigir
- Executar `pnpm run lint` para formatar arquivos
- Pedir permissão do usuário antes de preparar (stage) alterações
- Sugerir mensagem de commit (não fazer commit)

## Notas de Desenvolvimento

### Organização de Arquivos
- Funções/tipos como `createTevmNode` estão em:
  - Implementação: `createTevmNode.js`
  - Tipos: `TevmNode.ts`
  - Testes: `createTevmNode.spec.ts`

### Dicas Específicas de Ferramenta

#### pnpm i
- Se falhar, abortar a menos que seja erro simples de sintaxe (vírgula faltando)

#### pnpm lint (Biome)
- Faz lint de todo o código
- Corrige automaticamente a maioria dos problemas de formatação

#### TypeScript Builds
- Procurar tipos em node_modules se não for óbvio
- Para pacotes tevm, verificar a estrutura do monorepo
- Consultar documentação se houver múltiplas falhas

#### Execução de Testes
- Usar o test runner Vite
- Executar pacotes individualmente para depuração
- Adicionar console logs para validar pressupostos
- Explicar mudanças de snapshot antes de atualizar

## Critérios de Sucesso

Imprimir checklist ao final com ✅ para etapas aprovadas:
- ✅ Dependências atualizadas
- ✅ Linting aprovado
- ✅ TypeScript/Build aprovado
- ✅ Testes aprovados
- ✅ Validação de pacotes aprovada
- ✅ Arquivos preparados (nenhum commit feito)

## Diretrizes de Segurança

- **Corrigir erros proativamente** - TypeScript/testes detectarão regressões
- **Nunca fazer commit** - Apenas preparar (stage) alterações
- **Uma etapa por vez** - Não prosseguir até que a etapa atual seja aprovada
- **Pedir permissão** antes de preparar (stage) se correções foram feitas