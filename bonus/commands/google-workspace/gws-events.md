---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Inscrever-se em eventos do Google Workspace.
---

# Eventos do Google Workspace

Execute operações de Eventos do Google Workspace: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws events --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# events (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

```bash
gws events <resource> <method> [flags]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|-----------|
| [`+subscribe`](../gws-events-subscribe/SKILL.md) | Inscrever-se em eventos do Workspace e transmiti-los como NDJSON |
| [`+renew`](../gws-events-renew/SKILL.md) | Renovar/reativar inscrições de Eventos do Workspace |

## Recursos da API

### message

  - `stream` — SendStreamingMessage é uma chamada de streaming que retornará um fluxo de eventos de atualização de tarefas até que a Tarefa esteja em estado interrompido ou terminal.

### operations

  - `get` — Obtém o estado mais recente de uma operação de longa duração. Os clientes podem usar este método para sondar o resultado da operação em intervalos conforme recomendado pelo serviço de API.

### subscriptions

  - `create` — Cria uma inscrição do Google Workspace. Para saber como usar este método, consulte [Criar uma inscrição do Google Workspace](https://developers.google.com/workspace/events/guides/create-subscription).
  - `delete` — Exclui uma inscrição do Google Workspace. Para saber como usar este método, consulte [Excluir uma inscrição do Google Workspace](https://developers.google.com/workspace/events/guides/delete-subscription).
  - `get` — Obtém detalhes sobre uma inscrição do Google Workspace. Para saber como usar este método, consulte [Obter detalhes sobre uma inscrição do Google Workspace](https://developers.google.com/workspace/events/guides/get-subscription).
  - `list` — Lista inscrições do Google Workspace. Para saber como usar este método, consulte [Listar inscrições do Google Workspace](https://developers.google.com/workspace/events/guides/list-subscriptions).
  - `patch` — Atualiza ou renova uma inscrição do Google Workspace. Para saber como usar este método, consulte [Atualizar ou renovar uma inscrição do Google Workspace](https://developers.google.com/workspace/events/guides/update-subscription).
  - `reactivate` — Reativa uma inscrição suspensa do Google Workspace. Este método redefine o campo `State` da sua inscrição para `ACTIVE`. Antes de usar este método, você deve corrigir o erro que suspendeu a inscrição. Este método ignorará ou rejeitará qualquer inscrição que não esteja atualmente em estado suspenso. Para saber como usar este método, consulte [Reativar uma inscrição do Google Workspace](https://developers.google.com/workspace/events/guides/reactivate-subscription).

### tasks

  - `cancel` — Cancelar uma tarefa do agente. Se houver suporte, você não deverá esperar mais atualizações de tarefa para a tarefa.
  - `get` — Obter o estado atual de uma tarefa do agente.
  - `subscribe` — TaskSubscription é uma chamada de streaming que retornará um fluxo de eventos de atualização de tarefas. Isso anexa o fluxo a uma tarefa existente em processo. Se a tarefa estiver completa, o fluxo retornará a tarefa concluída (como GetTask) e fechará o fluxo.
  - `pushNotificationConfigs` — Operações no recurso 'pushNotificationConfigs'

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Navegar por recursos e métodos
gws events --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema events.<resource>.<method>
```

Use a saída de `gws schema` para construir suas flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws events --help

# Inspecionar schema do método antes de chamar
gws schema events.<resource>.<method>

# Executar comando com argumentos
gws events $ARGUMENTS
```

## Tarefa

Execute a operação de Eventos solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws events --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construa comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Manipule paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique erros na saída do comando
   - Revise quotas da API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente em caso de falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-events`