---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Classroom: Gerenciar turmas, listas de alunos e trabalhos do curso.
---

# Google Workspace Classroom

Execute operações do Google Workspace Classroom: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws classroom --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# classroom (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não existir, execute `gws generate-skills` para criar.

```bash
gws classroom <resource> <method> [flags]
```

## Recursos da API

### courses

  - `create` — Cria uma turma. O usuário especificado em `ownerId` é o proprietário da turma criada e adicionado como professor. Um usuário não administrador pode criar apenas uma turma com ele mesmo como proprietário. Administradores de domínio podem criar turmas de propriedade de qualquer usuário dentro do domínio. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para criar turmas ou em caso de erros de acesso. * `NOT_FOUND` se o professor principal não é um usuário válido.
  - `delete` — Deleta uma turma. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para deletar a turma solicitada ou em caso de erros de acesso. * `NOT_FOUND` se nenhuma turma existe com o ID solicitado.
  - `get` — Retorna uma turma. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para acessar a turma solicitada ou em caso de erros de acesso. * `NOT_FOUND` se nenhuma turma existe com o ID solicitado.
  - `getGradingPeriodSettings` — Retorna as configurações do período de avaliação em uma turma. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para acessar as configurações de período de avaliação na turma solicitada ou em caso de erros de acesso. * `NOT_FOUND` se a turma solicitada não existe.
  - `list` — Retorna uma lista de turmas que o usuário solicitante tem permissão para visualizar, restrita àquelas que correspondem à solicitação. As turmas retornadas são ordenadas por hora de criação, com as criadas mais recentemente vindo em primeiro lugar. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` em caso de erros de acesso. * `INVALID_ARGUMENT` se o argumento query está malformado. * `NOT_FOUND` se algum usuário especificado nos argumentos da query não existe.
  - `patch` — Atualiza um ou mais campos em uma turma. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para modificar a turma solicitada ou em caso de erros de acesso. * `NOT_FOUND` se nenhuma turma existe com o ID solicitado. * `INVALID_ARGUMENT` se campos inválidos forem especificados na máscara de atualização ou se nenhuma máscara de atualização for fornecida.
  - `update` — Atualiza uma turma. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para modificar a turma solicitada ou em caso de erros de acesso. * `NOT_FOUND` se nenhuma turma existe com o ID solicitado. * `FAILED_PRECONDITION` para os seguintes erros de solicitação: * CourseNotModifiable * CourseTitleCannotContainUrl
  - `updateGradingPeriodSettings` — Atualiza as configurações do período de avaliação de uma turma. Períodos de avaliação individuais podem ser adicionados, removidos ou modificados usando este método. O usuário solicitante e o proprietário da turma devem ser elegíveis para modificar Períodos de Avaliação. Para detalhes, consulte [requisitos de licença](https://developers.google.com/workspace/classroom/grading-periods/manage-grading-periods#licensing_requirements).
  - `aliases` — Operações no recurso 'aliases'
  - `announcements` — Operações no recurso 'announcements'
  - `courseWork` — Operações no recurso 'courseWork'
  - `courseWorkMaterials` — Operações no recurso 'courseWorkMaterials'
  - `posts` — Operações no recurso 'posts'
  - `studentGroups` — Operações no recurso 'studentGroups'
  - `students` — Operações no recurso 'students'
  - `teachers` — Operações no recurso 'teachers'
  - `topics` — Operações no recurso 'topics'

### invitations

  - `accept` — Aceita um convite, removendo-o e adicionando o usuário convidado aos professores ou alunos (conforme apropriado) da turma especificada. Apenas o usuário convidado pode aceitar um convite. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para aceitar o convite solicitado ou em caso de erros de acesso.
  - `create` — Cria um convite. Apenas um convite para um usuário e turma pode existir por vez. Delete e recrie um convite para fazer alterações. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para criar convites para esta turma ou em caso de erros de acesso. * `NOT_FOUND` se a turma ou o usuário não existe. * `FAILED_PRECONDITION`: * se a conta do usuário solicitado está desabilitada.
  - `delete` — Deleta um convite. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para deletar o convite solicitado ou em caso de erros de acesso. * `NOT_FOUND` se nenhum convite existe com o ID solicitado.
  - `get` — Retorna um convite. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para visualizar o convite solicitado ou em caso de erros de acesso. * `NOT_FOUND` se nenhum convite existe com o ID solicitado.
  - `list` — Retorna uma lista de convites que o usuário solicitante tem permissão para visualizar, restrita àqueles que correspondem à solicitação de lista. *Nota:* Pelo menos um de `user_id` ou `course_id` deve ser fornecido. Ambos os campos podem ser fornecidos. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` em caso de erros de acesso.

### registrations

  - `create` — Cria um `Registration`, fazendo com que o Classroom comece a enviar notificações do `feed` fornecido para o destino fornecido em `cloudPubSubTopic`. Retorna o `Registration` criado. Atualmente, será o mesmo do argumento, mas com campos atribuídos pelo servidor, como `expiry_time` e `id`, preenchidos. Observe que qualquer valor especificado para os campos `expiry_time` ou `id` será ignorado.
  - `delete` — Deleta um `Registration`, fazendo com que o Classroom pare de enviar notificações para esse `Registration`.

### userProfiles

  - `get` — Retorna um perfil de usuário. Este método retorna os seguintes códigos de erro: * `PERMISSION_DENIED` se o usuário solicitante não tem permissão para acessar este perfil de usuário, se nenhum perfil existe com o ID solicitado, ou em caso de erros de acesso.
  - `guardianInvitations` — Operações no recurso 'guardianInvitations'
  - `guardians` — Operações no recurso 'guardians'

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Navegar por recursos e métodos
gws classroom --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema classroom.<resource>.<method>
```

Use a saída `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws classroom --help

# Inspecionar schema do método antes de chamar
gws schema classroom.<resource>.<method>

# Executar comando com argumentos
gws classroom $ARGUMENTS
```

## Tarefa

Execute a operação do Classroom solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws classroom --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas e limites de taxa da API
   - Lidar com problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-classroom`