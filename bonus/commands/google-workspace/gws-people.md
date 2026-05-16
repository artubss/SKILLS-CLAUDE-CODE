---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google People: Gerenciar contatos e perfis.
---

# Google Workspace People

Execute operações do Google Workspace People: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws people --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# people (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

```bash
gws people <resource> <method> [flags]
```

## Recursos de API

### contactGroups

  - `batchGet` — Obtenha uma lista de grupos de contatos pertencentes ao usuário autenticado, especificando uma lista de nomes de recursos de grupo de contatos.
  - `create` — Crie um novo grupo de contatos pertencente ao usuário autenticado. Os nomes de grupos de contatos criados devem ser exclusivos para os grupos de contatos do usuário. Tentar criar um grupo com um nome duplicado retornará um erro HTTP 409. Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `delete` — Exclua um grupo de contatos existente pertencente ao usuário autenticado, especificando um nome de recurso de grupo de contatos. Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `get` — Obtenha um grupo de contatos específico pertencente ao usuário autenticado, especificando um nome de recurso de grupo de contatos.
  - `list` — Liste todos os grupos de contatos pertencentes ao usuário autenticado. Os membros dos grupos de contatos não são preenchidos.
  - `update` — Atualize o nome de um grupo de contatos existente pertencente ao usuário autenticado. Os nomes de grupos de contatos atualizados devem ser exclusivos para os grupos de contatos do usuário. Tentar criar um grupo com um nome duplicado retornará um erro HTTP 409. Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `members` — Operações no recurso 'members'

### otherContacts

  - `copyOtherContactToMyContactsGroup` — Copia um "Outro contato" para um novo contato no grupo "myContacts" do usuário. Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `list` — Liste todos os "Outros contatos", ou seja, contatos que não estão em um grupo de contatos. "Outros contatos" são tipicamente contatos criados automaticamente a partir de interações. Tokens de sincronização expiram 7 dias após a sincronização completa. Uma solicitação com um token de sincronização expirado retornará um erro com um [google.rpc.ErrorInfo](https://cloud.google.com/apis/design/errors#error_info) com motivo "EXPIRED_SYNC_TOKEN". Em caso de tal erro, os clientes devem fazer uma solicitação de sincronização completa sem um `sync_token`.
  - `search` — Fornece uma lista de contatos nos outros contatos do usuário autenticado que correspondem à consulta de pesquisa. A consulta corresponde aos campos `names`, `emailAddresses` e `phoneNumbers` de um contato que são da fonte OTHER_CONTACT. **IMPORTANTE**: Antes de pesquisar, os clientes devem enviar uma solicitação de aquecimento com uma consulta vazia para atualizar o cache. Consulte https://developers.google.com/people/v1/other-contacts#search_the_users_other_contacts

### people

  - `batchCreateContacts` — Crie um lote de novos contatos e retorne os PersonResponses para os contatos recém-criados. Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `batchDeleteContacts` — Exclua um lote de contatos. Qualquer dado que não seja de contato não será excluído. Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `batchUpdateContacts` — Atualize um lote de contatos e retorne um mapa de nomes de recursos para PersonResponses dos contatos atualizados. Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `createContact` — Crie um novo contato e retorne o recurso de pessoa para esse contato. A solicitação retorna um erro 400 se mais de um campo for especificado em um campo que é um singleton para fontes de contato: * biographies * birthdays * genders * names Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `deleteContact` — Exclua uma pessoa de contato. Qualquer dado que não seja de contato não será excluído. Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `deleteContactPhoto` — Exclua a foto de um contato. Solicitações de mutação para o mesmo usuário devem ser feitas sequencialmente para evitar contenção de bloqueio.
  - `get` — Fornece informações sobre uma pessoa, especificando um nome de recurso. Use `people/me` para indicar o usuário autenticado. A solicitação retorna um erro 400 se 'personFields' não for especificado.
  - `getBatchGet` — Fornece informações sobre uma lista de pessoas específicas, especificando uma lista de nomes de recursos solicitados. Use `people/me` para indicar o usuário autenticado. A solicitação retorna um erro 400 se 'personFields' não for especificado.
  - `listDirectoryPeople` — Fornece uma lista de perfis de domínio e contatos de domínio no diretório de domínio do usuário autenticado. Quando o `sync_token` é especificado, recursos excluídos desde a última sincronização serão retornados como uma pessoa com `PersonMetadata.deleted` definido como true. Quando `page_token` ou `sync_token` for especificado, todos os outros parâmetros de solicitação devem corresponder à primeira chamada. Gravações podem ter um atraso de propagação de vários minutos para solicitações de sincronização. Sincronizações incrementais não são destinadas a casos de uso de leitura após gravação.
  - `searchContacts` — Fornece uma lista de contatos nos contatos agrupados do usuário autenticado que correspondem à consulta de pesquisa. A consulta corresponde aos campos `names`, `nickNames`, `emailAddresses`, `phoneNumbers` e `organizations` de um contato que são da fonte CONTACT. **IMPORTANTE**: Antes de pesquisar, os clientes devem enviar uma solicitação de aquecimento com uma consulta vazia para atualizar o cache. Consulte https://developers.google.com/people/v1/contacts#search_the_users_contacts
  - `searchDirectoryPeople` — Fornece uma lista de perfis de domínio e contatos de domínio no diretório de domínio do usuário autenticado que correspondem à consulta de pesquisa.
  - `updateContact` — Atualize dados de contato para uma pessoa de contato existente. Qualquer dado que não seja de contato não será modificado. Qualquer dado que não seja de contato na pessoa a ser atualizada será ignorado. Todos os campos especificados em `update_mask` serão substituídos. O servidor retorna um erro 400 se `person.metadata.sources` não for especificado para o contato a ser atualizado ou se não houver fonte de contato.
  - `updateContactPhoto` — Atualize a foto de um contato. Solicitações de mutação para o mesmo usuário devem ser enviadas sequencialmente para evitar latência aumentada e falhas.
  - `connections` — Operações no recurso 'connections'

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Procure recursos e métodos
gws people --help

# Inspecione os parâmetros, tipos e padrões necessários de um método
gws schema people.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Liste recursos e métodos disponíveis
gws people --help

# Inspecione o schema do método antes de chamar
gws schema people.<resource>.<method>

# Execute comando com argumentos
gws people $ARGUMENTS
```

## Tarefa

Execute a operação de People solicitada: $ARGUMENTS

1. **Verifique Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws people --help`

2. **Inspecione o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetro e restrições

3. **Execute a Operação**
   - Construa comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Gerencie paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique erros na saída do comando
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-people`