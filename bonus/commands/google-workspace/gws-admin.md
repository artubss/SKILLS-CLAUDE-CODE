---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [sinalizadores]
description: Google Workspace Admin SDK: Gerencie usuários, grupos e dispositivos.
---

# Google Workspace Admin

Execute operações do Google Workspace Admin: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws admin --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# admin (directory_v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, sinalizadores globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criar.

```bash
gws admin <recurso> <método> [sinalizadores]
```

## Recursos da API

### asps

  - `delete` — Deleta um ASP emitido por um usuário.
  - `get` — Obtém informações sobre um ASP emitido por um usuário.
  - `list` — Lista os ASPs emitidos por um usuário.

### channels

  - `stop` — Para de observar recursos através deste canal.

### chromeosdevices

  - `action` — Use [BatchChangeChromeOsDeviceStatus](https://developers.google.com/workspace/admin/directory/reference/rest/v1/customer.devices.chromeos/batchChangeStatus) em vez disso. Executa uma ação que afeta um dispositivo Chrome OS. Isso inclui desprovisionamento, desabilitação e reabilitação de dispositivos. *Aviso:* * O desprovisionamento de um dispositivo interromperá a sincronização da política do dispositivo e removerá impressoras no nível do dispositivo. Depois que um dispositivo é desprovisionado, ele deve ser apagado antes de poder ser re-registrado.
  - `get` — Recupera as propriedades de um dispositivo Chrome OS.
  - `list` — Recupera uma lista paginada de dispositivos Chrome OS dentro de uma conta.
  - `moveDevicesToOu` — Move ou insere vários dispositivos Chrome OS em uma unidade organizacional. Você pode mover até 50 dispositivos por vez.
  - `patch` — Atualiza as propriedades atualizáveis de um dispositivo, como `annotatedUser`, `annotatedLocation`, `notes`, `orgUnitPath` ou `annotatedAssetId`. Este método suporta [patch semantics](https://developers.google.com/workspace/admin/directory/v1/guides/performance#patch).
  - `update` — Atualiza as propriedades atualizáveis de um dispositivo, como `annotatedUser`, `annotatedLocation`, `notes`, `orgUnitPath` ou `annotatedAssetId`.

### customer

  - `devices` — Operações no recurso 'devices'

### customers

  - `get` — Recupera um cliente.
  - `patch` — Aplica patch a um cliente.
  - `update` — Atualiza um cliente.
  - `chrome` — Operações no recurso 'chrome'

### domainAliases

  - `delete` — Deleta um alias de domínio do cliente.
  - `get` — Recupera um alias de domínio do cliente.
  - `insert` — Insere um alias de domínio do cliente.
  - `list` — Lista os aliases de domínio do cliente.

### domains

  - `delete` — Deleta um domínio do cliente.
  - `get` — Recupera um domínio do cliente.
  - `insert` — Insere um domínio do cliente.
  - `list` — Lista os domínios do cliente.

### groups

  - `delete` — Deleta um grupo.
  - `get` — Recupera as propriedades de um grupo.
  - `insert` — Cria um grupo.
  - `list` — Recupera todos os grupos de um domínio ou de um usuário dado um userKey (paginado).
  - `patch` — Atualiza as propriedades de um grupo. Este método suporta [patch semantics](https://developers.google.com/workspace/admin/directory/v1/guides/performance#patch).
  - `update` — Atualiza as propriedades de um grupo.
  - `aliases` — Operações no recurso 'aliases'

### members

  - `delete` — Remove um membro de um grupo.
  - `get` — Recupera as propriedades de um membro do grupo.
  - `hasMember` — Verifica se o usuário especificado é membro do grupo. A associação pode ser direta ou aninhada, mas se aninhada, `memberKey` e `groupKey` devem ser entidades no mesmo domínio ou um erro `Invalid input` será retornado. Para verificar associações aninhadas que incluem entidades fora do domínio do grupo, use o método [`checkTransitiveMembership()`](https://cloud.google.com/identity/docs/reference/rest/v1/groups.memberships/checkTransitiveMembership) na API Cloud Identity Groups.
  - `insert` — Adiciona um usuário ao grupo especificado.
  - `list` — Recupera uma lista paginada de todos os membros em um grupo. Este método atinge o tempo limite após 60 minutos. Para mais informações, consulte [Solucionar códigos de erro](https://developers.google.com/workspace/admin/directory/v1/guides/troubleshoot-error-codes).
  - `patch` — Atualiza as propriedades de associação de um usuário no grupo especificado. Este método suporta [patch semantics](https://developers.google.com/workspace/admin/directory/v1/guides/performance#patch).
  - `update` — Atualiza a associação de um usuário no grupo especificado.

### mobiledevices

  - `action` — Executa uma ação que afeta um dispositivo móvel. Por exemplo, apagar remotamente um dispositivo.
  - `delete` — Remove um dispositivo móvel.
  - `get` — Recupera as propriedades de um dispositivo móvel.
  - `list` — Recupera uma lista paginada de todos os dispositivos móveis de propriedade do usuário para uma conta. Para recuperar uma lista que inclua dispositivos de propriedade da empresa, use a [API Devices](https://cloud.google.com/identity/docs/concepts/overview-devices) do Cloud Identity em vez disso. Este método atinge o tempo limite após 60 minutos. Para mais informações, consulte [Solucionar códigos de erro](https://developers.google.com/workspace/admin/directory/v1/guides/troubleshoot-error-codes).

### orgunits

  - `delete` — Remove uma unidade organizacional.
  - `get` — Recupera uma unidade organizacional.
  - `insert` — Adiciona uma unidade organizacional.
  - `list` — Recupera uma lista de todas as unidades organizacionais para uma conta.
  - `patch` — Atualiza uma unidade organizacional. Este método suporta [patch semantics](https://developers.google.com/workspace/admin/directory/v1/guides/performance#patch)
  - `update` — Atualiza uma unidade organizacional.

### privileges

  - `list` — Recupera uma lista paginada de todos os privilégios para um cliente.

### resources

  - `buildings` — Operações no recurso 'buildings'
  - `calendars` — Operações no recurso 'calendars'
  - `features` — Operações no recurso 'features'

### roleAssignments

  - `delete` — Deleta uma atribuição de função.
  - `get` — Recupera uma atribuição de função.
  - `insert` — Cria uma atribuição de função.
  - `list` — Recupera uma lista paginada de todas as atribuições de função.

### roles

  - `delete` — Deleta uma função.
  - `get` — Recupera uma função.
  - `insert` — Cria uma função.
  - `list` — Recupera uma lista paginada de todas as funções em um domínio.
  - `patch` — Aplica patch a uma função.
  - `update` — Atualiza uma função.

### schemas

  - `delete` — Deleta um schema.
  - `get` — Recupera um schema.
  - `insert` — Cria um schema.
  - `list` — Recupera todos os schemas para um cliente.
  - `patch` — Aplica patch a um schema.
  - `update` — Atualiza um schema.

### tokens

  - `delete` — Deleta todos os tokens de acesso emitidos por um usuário para um aplicativo.
  - `get` — Obtém informações sobre um token de acesso emitido por um usuário.
  - `list` — Retorna o conjunto de tokens que o usuário especificado emitiu para aplicativos de terceiros.

### twoStepVerification

  - `turnOff` — Desativa a verificação em 2 etapas para o usuário.

### users

  - `createGuest` — Cria um usuário convidado com acesso a um [subconjunto de recursos do Workspace](https://support.google.com/a/answer/16558545). Este recurso está atualmente em Alpha. Entre em contato com o suporte se você estiver interessado em testar este recurso.
  - `delete` — Deleta um usuário.
  - `get` — Recupera um usuário.
  - `insert` — Cria um usuário. Chamadas de mutação imediatamente após a criação do usuário podem às vezes falhar, pois o usuário não está totalmente criado devido ao atraso de propagação em nossos backends. Verifique os detalhes do erro para a mensagem "User creation is not complete" para ver se este é o caso. Tentar novamente as chamadas após algum tempo pode ajudar neste caso. Se `resolveConflictAccount` for definido como `true`, um código de resposta `202` significa que existe uma conta não gerenciada conflitante e foi convidada a se juntar à organização.
  - `list` — Recupera uma lista paginada de usuários deletados ou todos os usuários em um domínio.
  - `makeAdmin` — Torna um usuário super administrador.
  - `patch` — Atualiza um usuário usando patch semantics. O método update deve ser usado em vez disso, pois também suporta patch semantics e tem melhor desempenho. Se você está mapeando uma identidade externa para uma identidade Google, use o método [`update`](https://developers.google.com/workspace/admin/directory/v1/reference/users/update) em vez do método `patch`. Este método não consegue limpar campos que contêm objetos repetidos (`addresses`, `phones`, etc). Use o método update em vez disso.
  - `signOut` — Desconecta um usuário de todas as sessões web e dispositivo e redefine seus cookies de sign-in. O usuário terá que entrar autenticando novamente.
  - `undelete` — Restaura um usuário deletado.
  - `update` — Atualiza um usuário. Este método suporta patch semantics, o que significa que você só precisa incluir os campos que deseja atualizar. Campos que não estão presentes na solicitação serão preservados, e campos definidos como `null` serão apagados. Para campos repetidos que contêm arrays, itens individuais no array não podem ser corrigidos em partes; eles devem ser fornecidos no corpo da solicitação com os valores desejados para todos os itens.
  - `watch` — Observa mudanças na lista de usuários.
  - `aliases` — Operações no recurso 'aliases'
  - `photos` — Operações no recurso 'photos'

### verificationCodes

  - `generate` — Gera novos códigos de verificação de backup para o usuário.
  - `invalidate` — Invalida os códigos de verificação de backup atuais para o usuário.
  - `list` — Retorna o conjunto atual de códigos de verificação de backup válidos para o usuário especificado.

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Navegue por recursos e métodos
gws admin --help

# Inspecione os parâmetros necessários, tipos e padrões de um método
gws schema admin.<recurso>.<método>
```

Use a saída `gws schema` para construir seus sinalizadores `--params` e `--json`.

## Uso

```bash
# Liste recursos e métodos disponíveis
gws admin --help

# Inspecione o schema do método antes de chamar
gws schema admin.<recurso>.<método>

# Execute comando com argumentos
gws admin $ARGUMENTS
```

## Tarefa

Execute a operação Admin solicitada: $ARGUMENTS

1. **Verifique Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws admin --help`

2. **Inspecione o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Execute a Operação**
   - Construa comando com sinalizadores apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique erros na saída do comando
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-admin`