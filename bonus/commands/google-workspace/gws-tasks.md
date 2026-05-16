---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Tasks: Gerenciar listas de tarefas e tarefas.
---

# Google Workspace Tasks

Execute operações do Google Workspace Tasks: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws tasks --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# tasks (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se ausente, execute `gws generate-skills` para criar.

```bash
gws tasks <resource> <method> [flags]
```

## Recursos da API

### tasklists

  - `delete` — Deleta a lista de tarefas especificada do usuário autenticado. Se a lista contiver tarefas atribuídas, tanto as tarefas atribuídas quanto as tarefas originais na superfície de atribuição (Docs, Chat Spaces) são deletadas.
  - `get` — Retorna a lista de tarefas especificada do usuário autenticado.
  - `insert` — Cria uma nova lista de tarefas e a adiciona às listas de tarefas do usuário autenticado. Um usuário pode ter até 2000 listas por vez.
  - `list` — Retorna todas as listas de tarefas do usuário autenticado. Um usuário pode ter até 2000 listas por vez.
  - `patch` — Atualiza a lista de tarefas especificada do usuário autenticado. Este método oferece suporte a semântica de patch.
  - `update` — Atualiza a lista de tarefas especificada do usuário autenticado.

### tasks

  - `clear` — Limpa todas as tarefas concluídas da lista de tarefas especificada. As tarefas afetadas serão marcadas como 'ocultas' e não serão mais retornadas por padrão ao recuperar todas as tarefas de uma lista de tarefas.
  - `delete` — Deleta a tarefa especificada da lista de tarefas. Se a tarefa for atribuída, tanto a tarefa atribuída quanto a tarefa original (em Docs, Chat Spaces) são deletadas. Para deletar apenas a tarefa atribuída, navegue até a superfície de atribuição e desatribua a tarefa de lá.
  - `get` — Retorna a tarefa especificada.
  - `insert` — Cria uma nova tarefa na lista de tarefas especificada. Tarefas atribuídas de Docs ou Chat Spaces não podem ser inseridas da API Pública de Tasks; elas só podem ser criadas atribuindo-as de Docs ou Chat Spaces. Um usuário pode ter até 20.000 tarefas não ocultas por lista e até 100.000 tarefas no total por vez.
  - `list` — Retorna todas as tarefas na lista de tarefas especificada. Não retorna tarefas atribuídas por padrão (de Docs, Chat Spaces). Um usuário pode ter até 20.000 tarefas não ocultas por lista e até 100.000 tarefas no total por vez.
  - `move` — Move a tarefa especificada para outra posição na lista de tarefas de destino. Se a lista de destino não for especificada, a tarefa é movida dentro de sua lista atual. Isso pode incluir colocá-la como tarefa filha sob um novo pai e/ou movê-la para uma posição diferente entre suas tarefas irmãs. Um usuário pode ter até 2.000 subtarefas por tarefa.
  - `patch` — Atualiza a tarefa especificada. Este método oferece suporte a semântica de patch.
  - `update` — Atualiza a tarefa especificada.

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Navegue por recursos e métodos
gws tasks --help

# Inspecione os parâmetros obrigatórios, tipos e padrões de um método
gws schema tasks.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws tasks --help

# Inspecionar schema do método antes de chamar
gws schema tasks.<resource>.<method>

# Executar comando com argumentos
gws tasks $ARGUMENTS
```

## Tarefa

Execute a operação de Tasks solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws tasks --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas e limites de taxa da API
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-tasks`