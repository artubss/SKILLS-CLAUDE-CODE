---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Cloud Identity: Gerenciar grupos de identidade e associações.
---

# Google Workspace Cloudidentity

Execute operações Google Workspace Cloudidentity: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws cloudidentity --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# cloudidentity (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não existir, execute `gws generate-skills` para criar.

```bash
gws cloudidentity <resource> <method> [flags]
```

## Recursos da API

### customers

  - `userinvitations` — Operações no recurso 'userinvitations'

### devices

  - `cancelWipe` — Cancela uma limpeza de dispositivo inacabada. Esta operação pode ser usada para cancelar a limpeza do dispositivo na lacuna entre a operação de limpeza retornar sucesso e o dispositivo ser limpo. Esta operação é possível quando o dispositivo está em estado "pending wipe". O dispositivo entra no estado "pending wipe" quando um comando de limpeza de dispositivo é emitido, mas ainda não foi enviado para o dispositivo. O cancelamento da limpeza falhará se o comando de limpeza já tiver sido emitido para o dispositivo.
  - `create` — Cria um dispositivo. Apenas dispositivos de propriedade da empresa podem ser criados. **Nota**: Este método está disponível apenas para clientes que possuem um dos seguintes SKUs: Enterprise Standard, Enterprise Plus, Enterprise for Education e Cloud Identity Premium
  - `delete` — Exclui o dispositivo especificado.
  - `get` — Recupera o dispositivo especificado.
  - `list` — Lista/Pesquisa dispositivos.
  - `wipe` — Limpa todos os dados do dispositivo especificado.
  - `deviceUsers` — Operações no recurso 'deviceUsers'

### groups

  - `create` — Cria um Group.
  - `delete` — Exclui um `Group`.
  - `get` — Recupera um `Group`.
  - `getSecuritySettings` — Obter Configurações de Segurança
  - `list` — Lista os recursos `Group` em um customer ou namespace.
  - `lookup` — Pesquisa o [nome do recurso](https://cloud.google.com/apis/design/resource_names) de um `Group` pela sua `EntityKey`.
  - `patch` — Atualiza um `Group`.
  - `search` — Pesquisa recursos `Group` correspondentes a uma consulta especificada.
  - `updateSecuritySettings` — Atualizar Configurações de Segurança
  - `memberships` — Operações no recurso 'memberships'

### inboundOidcSsoProfiles

  - `create` — Cria um InboundOidcSsoProfile para um customer. Quando o customer de destino ativou [Aprovação multi-parte para ações sensíveis](https://support.google.com/a/answer/13790448), a `Operation` na resposta terá `"done": false`, não terá uma resposta e os metadados terão `"state": "awaiting-multi-party-approval"`.
  - `delete` — Exclui um InboundOidcSsoProfile.
  - `get` — Obtém um InboundOidcSsoProfile.
  - `list` — Lista objetos InboundOidcSsoProfile para um customer corporativo do Google.
  - `patch` — Atualiza um InboundOidcSsoProfile. Quando o customer de destino ativou [Aprovação multi-parte para ações sensíveis](https://support.google.com/a/answer/13790448), a `Operation` na resposta terá `"done": false`, não terá uma resposta e os metadados terão `"state": "awaiting-multi-party-approval"`.

### inboundSamlSsoProfiles

  - `create` — Cria um InboundSamlSsoProfile para um customer. Quando o customer de destino ativou [Aprovação multi-parte para ações sensíveis](https://support.google.com/a/answer/13790448), a `Operation` na resposta terá `"done": false`, não terá uma resposta e os metadados terão `"state": "awaiting-multi-party-approval"`.
  - `delete` — Exclui um InboundSamlSsoProfile.
  - `get` — Obtém um InboundSamlSsoProfile.
  - `list` — Lista InboundSamlSsoProfiles para um customer.
  - `patch` — Atualiza um InboundSamlSsoProfile. Quando o customer de destino ativou [Aprovação multi-parte para ações sensíveis](https://support.google.com/a/answer/13790448), a `Operation` na resposta terá `"done": false`, não terá uma resposta e os metadados terão `"state": "awaiting-multi-party-approval"`.
  - `idpCredentials` — Operações no recurso 'idpCredentials'

### inboundSsoAssignments

  - `create` — Cria um InboundSsoAssignment para usuários e dispositivos em um `Customer` em um `Group` ou `OrgUnit` dado.
  - `delete` — Exclui um InboundSsoAssignment. Para desabilitar SSO, crie (ou atualize) um assignment que tenha `sso_mode` == `SSO_OFF`.
  - `get` — Obtém um InboundSsoAssignment.
  - `list` — Lista os InboundSsoAssignments para um `Customer`.
  - `patch` — Atualiza um InboundSsoAssignment. O corpo desta solicitação é o campo `inbound_sso_assignment` e o `update_mask` é relativo a isso. Por exemplo: um PATCH para `/v1/inboundSsoAssignments/0abcdefg1234567&update_mask=rank` com um corpo de `{ "rank": 1 }` move esse assignment de SSO direcionado para grupos para a prioridade mais alta e desloca qualquer outro assignment direcionado para grupos para baixo em prioridade.

### policies

  - `get` — Obter uma política.
  - `list` — Listar políticas.

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Navegar recursos e métodos
gws cloudidentity --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema cloudidentity.<resource>.<method>
```

Use a saída `gws schema` para construir suas flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws cloudidentity --help

# Inspecionar schema do método antes de chamar
gws schema cloudidentity.<resource>.<method>

# Executar comando com argumentos
gws cloudidentity $ARGUMENTS
```

## Tarefa

Execute a operação Cloudidentity solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws cloudidentity --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Manipular paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas e limites de taxa da API
   - Tratar problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-cloudidentity`