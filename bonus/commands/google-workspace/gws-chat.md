---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [flags]
description: Google Chat: Gerenciar espaços e mensagens do Chat.
---

# Google Workspace Chat

Execute operações do Google Workspace Chat: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws chat --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# chat (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não existir, execute `gws generate-skills` para criar.

```bash
gws chat <recurso> <método> [flags]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|-----------|
| [`+send`](../gws-chat-send/SKILL.md) | Enviar uma mensagem para um espaço |

## Recursos da API

### customEmojis

  - `create` — Cria um emoji personalizado. Emojis personalizados estão disponíveis apenas para contas do Google Workspace, e o administrador deve ativar emojis personalizados para a organização. Para mais informações, consulte [Saiba mais sobre emojis personalizados no Google Chat](https://support.google.com/chat/answer/12800149) e [Gerenciar permissões de emoji personalizado](https://support.google.com/a/answer/12850085).
  - `delete` — Exclui um emoji personalizado. Por padrão, os usuários podem excluir apenas emojis personalizados que criaram. [Gerenciadores de emoji](https://support.google.com/a/answer/12850085) designados pelo administrador podem excluir qualquer emoji personalizado na organização. Consulte [Saiba mais sobre emojis personalizados no Google Chat](https://support.google.com/chat/answer/12800149). Emojis personalizados estão disponíveis apenas para contas do Google Workspace, e o administrador deve ativar emojis personalizados para a organização.
  - `get` — Retorna detalhes sobre um emoji personalizado. Emojis personalizados estão disponíveis apenas para contas do Google Workspace, e o administrador deve ativar emojis personalizados para a organização. Para mais informações, consulte [Saiba mais sobre emojis personalizados no Google Chat](https://support.google.com/chat/answer/12800149) e [Gerenciar permissões de emoji personalizado](https://support.google.com/a/answer/12850085).
  - `list` — Lista emojis personalizados visíveis para o usuário autenticado. Emojis personalizados estão disponíveis apenas para contas do Google Workspace, e o administrador deve ativar emojis personalizados para a organização. Para mais informações, consulte [Saiba mais sobre emojis personalizados no Google Chat](https://support.google.com/chat/answer/12800149) e [Gerenciar permissões de emoji personalizado](https://support.google.com/a/answer/12850085).

### media

  - `download` — Baixa mídia. O download é suportado na URI `/v1/media/{+name}?alt=media`.
  - `upload` — Carrega um anexo. Para um exemplo, consulte [Fazer upload de mídia como anexo de arquivo](https://developers.google.com/workspace/chat/upload-media-attachments).

### spaces

  - `completeImport` — Conclui o [processo de importação](https://developers.google.com/workspace/chat/import-data) para o espaço especificado e o torna visível para os usuários.
  - `create` — Cria um espaço. Pode ser usado para criar um espaço nomeado ou um grupo de chat em `modo de importação`. Para um exemplo, consulte [Criar um espaço](https://developers.google.com/workspace/chat/create-spaces).
  - `delete` — Exclui um espaço nomeado. Sempre executa uma exclusão em cascata, o que significa que os recursos secundários do espaço—como mensagens postadas no espaço e membros do espaço—também são excluídos. Para um exemplo, consulte [Excluir um espaço](https://developers.google.com/workspace/chat/delete-spaces).
  - `findDirectMessage` — Retorna a mensagem direta existente com o usuário especificado. Se nenhum espaço de mensagem direta for encontrado, retorna um erro `404 NOT_FOUND`. Para um exemplo, consulte [Localizar uma mensagem direta](/chat/api/guides/v1/spaces/find-direct-message). Com [autenticação de app](https://developers.google.com/workspace/chat/authenticate-authorize-chat-app), retorna o espaço de mensagem direta entre o usuário especificado e o Chat app chamador.
  - `get` — Retorna detalhes sobre um espaço. Para um exemplo, consulte [Obter detalhes sobre um espaço](https://developers.google.com/workspace/chat/get-spaces).
  - `list` — Lista espaços dos quais o chamador é membro. Grupos de chat e DMs não são listados até que a primeira mensagem seja enviada. Para um exemplo, consulte [Listar espaços](https://developers.google.com/workspace/chat/list-spaces).
  - `patch` — Atualiza um espaço. Para um exemplo, consulte [Atualizar um espaço](https://developers.google.com/workspace/chat/update-spaces). Se você estiver atualizando o campo `displayName` e receber a mensagem de erro `ALREADY_EXISTS`, tente um nome de exibição diferente.. Um espaço existente dentro da organização do Google Workspace pode já estar usando este nome de exibição.
  - `search` — Retorna uma lista de espaços em uma organização do Google Workspace com base em uma pesquisa do administrador. Na solicitação, defina `use_admin_access` como `true`. Para um exemplo, consulte [Pesquisar e gerenciar espaços](https://developers.google.com/workspace/chat/search-manage-admin).
  - `setup` — Cria um espaço e adiciona usuários especificados a ele. O usuário chamador é adicionado automaticamente ao espaço e não deve ser especificado como uma associação na solicitação. Para um exemplo, consulte [Configurar um espaço com membros iniciais](https://developers.google.com/workspace/chat/set-up-spaces). Para especificar os membros humanos a adicionar, adicione associações com o `membership.member.name` apropriado. Para adicionar um usuário humano, use `users/{user}`, onde `{user}` pode ser o endereço de email do usuário.
  - `members` — Operações no recurso 'members'
  - `messages` — Operações no recurso 'messages'
  - `spaceEvents` — Operações no recurso 'spaceEvents'

### users

  - `spaces` — Operações no recurso 'spaces'

## Descobrir Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Explorar recursos e métodos
gws chat --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema chat.<recurso>.<método>
```

Use a saída `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws chat --help

# Inspecionar schema do método antes de chamar
gws schema chat.<recurso>.<método>

# Executar comando com argumentos
gws chat $ARGUMENTS
```

## Tarefa

Execute a operação Chat solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws chat --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos e restrições de parâmetros

3. **Executar Operação**
   - Construir comando com flags apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas da API e limites de taxa
   - Lidar com problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-chat`