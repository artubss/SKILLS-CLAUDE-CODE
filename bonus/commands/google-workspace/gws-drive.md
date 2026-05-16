---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Drive: Gerenciar arquivos, pastas e unidades compartilhadas.
---

# Google Workspace Drive

Execute operações do Google Workspace Drive: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws drive --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# drive (v3)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se ausente, execute `gws generate-skills` para criar.

```bash
gws drive <resource> <method> [flags]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|-------------|
| [`+upload`](../gws-drive-upload/SKILL.md) | Fazer upload de um arquivo com metadados automáticos |

## Recursos da API

### about

  - `get` — Obtém informações sobre o usuário, seu Drive e recursos do sistema. Para mais informações, veja [Retornar informações do usuário](https://developers.google.com/workspace/drive/api/guides/user-info). Obrigatório: O parâmetro `fields` deve ser definido. Para retornar os campos exatos necessários, veja [Retornar campos específicos](https://developers.google.com/workspace/drive/api/guides/fields-parameter).

### accessproposals

  - `get` — Recupera uma proposta de acesso por ID. Para mais informações, veja [Gerenciar propostas de acesso pendentes](https://developers.google.com/workspace/drive/api/guides/pending-access).
  - `list` — Lista as propostas de acesso em um arquivo. Para mais informações, veja [Gerenciar propostas de acesso pendentes](https://developers.google.com/workspace/drive/api/guides/pending-access). Nota: Apenas aprovadores podem listar propostas de acesso em um arquivo. Se o usuário não for um aprovador, um erro 403 é retornado.
  - `resolve` — Aprova ou nega uma proposta de acesso. Para mais informações, veja [Gerenciar propostas de acesso pendentes](https://developers.google.com/workspace/drive/api/guides/pending-access).

### approvals

  - `get` — Obtém uma Aprovação por ID.
  - `list` — Lista as Aprovações em um arquivo.

### apps

  - `get` — Obtém um aplicativo específico. Para mais informações, veja [Retornar informações do usuário](https://developers.google.com/workspace/drive/api/guides/user-info).
  - `list` — Lista os aplicativos instalados de um usuário. Para mais informações, veja [Retornar informações do usuário](https://developers.google.com/workspace/drive/api/guides/user-info).

### changes

  - `getStartPageToken` — Obtém o pageToken inicial para listar alterações futuras. Para mais informações, veja [Recuperar alterações](https://developers.google.com/workspace/drive/api/guides/manage-changes).
  - `list` — Lista as alterações de um usuário ou unidade compartilhada. Para mais informações, veja [Recuperar alterações](https://developers.google.com/workspace/drive/api/guides/manage-changes).
  - `watch` — Inscreve-se para alterações de um usuário. Para mais informações, veja [Notificações para alterações de recursos](https://developers.google.com/workspace/drive/api/guides/push).

### channels

  - `stop` — Para de observar recursos através deste canal. Para mais informações, veja [Notificações para alterações de recursos](https://developers.google.com/workspace/drive/api/guides/push).

### comments

  - `create` — Cria um comentário em um arquivo. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments). Obrigatório: O parâmetro `fields` deve ser definido. Para retornar os campos exatos necessários, veja [Retornar campos específicos](https://developers.google.com/workspace/drive/api/guides/fields-parameter).
  - `delete` — Deleta um comentário. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments).
  - `get` — Obtém um comentário por ID. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments). Obrigatório: O parâmetro `fields` deve ser definido. Para retornar os campos exatos necessários, veja [Retornar campos específicos](https://developers.google.com/workspace/drive/api/guides/fields-parameter).
  - `list` — Lista os comentários de um arquivo. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments). Obrigatório: O parâmetro `fields` deve ser definido. Para retornar os campos exatos necessários, veja [Retornar campos específicos](https://developers.google.com/workspace/drive/api/guides/fields-parameter).
  - `update` — Atualiza um comentário com semântica de patch. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments). Obrigatório: O parâmetro `fields` deve ser definido. Para retornar os campos exatos necessários, veja [Retornar campos específicos](https://developers.google.com/workspace/drive/api/guides/fields-parameter).

### drives

  - `create` — Cria uma unidade compartilhada. Para mais informações, veja [Gerenciar unidades compartilhadas](https://developers.google.com/workspace/drive/api/guides/manage-shareddrives).
  - `delete` — Exclui permanentemente uma unidade compartilhada da qual o usuário é um `organizador`. A unidade compartilhada não pode conter nenhum item não descartado. Para mais informações, veja [Gerenciar unidades compartilhadas](https://developers.google.com/workspace/drive/api/guides/manage-shareddrives).
  - `get` — Obtém os metadados de uma unidade compartilhada por ID. Para mais informações, veja [Gerenciar unidades compartilhadas](https://developers.google.com/workspace/drive/api/guides/manage-shareddrives).
  - `hide` — Oculta uma unidade compartilhada da visualização padrão. Para mais informações, veja [Gerenciar unidades compartilhadas](https://developers.google.com/workspace/drive/api/guides/manage-shareddrives).
  - `list` — Lista as unidades compartilhadas do usuário. Este método aceita o parâmetro `q`, que é uma consulta de pesquisa combinando um ou mais termos de pesquisa. Para mais informações, veja o guia [Pesquisar unidades compartilhadas](/workspace/drive/api/guides/search-shareddrives).
  - `unhide` — Restaura uma unidade compartilhada para a visualização padrão. Para mais informações, veja [Gerenciar unidades compartilhadas](https://developers.google.com/workspace/drive/api/guides/manage-shareddrives).
  - `update` — Atualiza os metadados de uma unidade compartilhada. Para mais informações, veja [Gerenciar unidades compartilhadas](https://developers.google.com/workspace/drive/api/guides/manage-shareddrives).

### files

  - `copy` — Cria uma cópia de um arquivo e aplica as atualizações solicitadas com semântica de patch. Para mais informações, veja [Criar e gerenciar arquivos](https://developers.google.com/workspace/drive/api/guides/create-file).
  - `create` — Cria um arquivo. Para mais informações, veja [Criar e gerenciar arquivos](/workspace/drive/api/guides/create-file). Este método oferece suporte a um URI */upload* e aceita mídia enviada com as seguintes características: - *Tamanho máximo de arquivo:* 5.120 GB - *Tipos MIME de mídia aceitos:* `*/*` (Especifique um tipo MIME válido, em vez do valor literal `*/*`. O literal `*/*` é usado apenas para indicar que qualquer tipo MIME válido pode ser enviado.)
  - `delete` — Exclui permanentemente um arquivo do usuário sem movê-lo para a lixeira. Para mais informações, veja [Descartar ou deletar arquivos e pastas](https://developers.google.com/workspace/drive/api/guides/delete). Se o arquivo pertencer a uma unidade compartilhada, o usuário deve ser um `organizador` na pasta principal. Se o alvo for uma pasta, todos os descendentes do usuário também serão excluídos.
  - `download` — Faz download do conteúdo de um arquivo. Para mais informações, veja [Fazer download e exportar arquivos](https://developers.google.com/workspace/drive/api/guides/manage-downloads). As operações são válidas por 24 horas a partir da hora da criação.
  - `emptyTrash` — Exclui permanentemente todos os arquivos descartados do usuário. Para mais informações, veja [Descartar ou deletar arquivos e pastas](https://developers.google.com/workspace/drive/api/guides/delete).
  - `export` — Exporta um documento do Google Workspace para o tipo MIME solicitado e retorna o conteúdo de byte exportado. Para mais informações, veja [Fazer download e exportar arquivos](https://developers.google.com/workspace/drive/api/guides/manage-downloads). Observe que o conteúdo exportado é limitado a 10 MB.
  - `generateIds` — Gera um conjunto de IDs de arquivo que podem ser fornecidos em solicitações de criação ou cópia. Para mais informações, veja [Criar e gerenciar arquivos](https://developers.google.com/workspace/drive/api/guides/create-file).
  - `get` — Obtém os metadados ou conteúdo de um arquivo por ID. Para mais informações, veja [Pesquisar arquivos e pastas](/workspace/drive/api/guides/search-files). Se você fornecer o parâmetro de URL `alt=media`, a resposta incluirá o conteúdo do arquivo no corpo da resposta. Fazer download de conteúdo com `alt=media` funciona apenas se o arquivo estiver armazenado no Drive. Para fazer download de Google Docs, Sheets e Slides, use [`files.export`](/workspace/drive/api/reference/rest/v3/files/export).
  - `list` — Lista os arquivos do usuário. Para mais informações, veja [Pesquisar arquivos e pastas](/workspace/drive/api/guides/search-files). Este método aceita o parâmetro `q`, que é uma consulta de pesquisa combinando um ou mais termos de pesquisa. Este método retorna *todos* os arquivos por padrão, incluindo arquivos descartados. Se você não quiser que arquivos descartados apareçam na lista, use o parâmetro de consulta `trashed=false` para remover arquivos descartados dos resultados.
  - `listLabels` — Lista os rótulos em um arquivo. Para mais informações, veja [Listar rótulos em um arquivo](https://developers.google.com/workspace/drive/api/guides/list-labels).
  - `modifyLabels` — Modifica o conjunto de rótulos aplicados a um arquivo. Para mais informações, veja [Definir um campo de rótulo em um arquivo](https://developers.google.com/workspace/drive/api/guides/set-label). Retorna uma lista dos rótulos que foram adicionados ou modificados.
  - `update` — Atualiza os metadados, conteúdo ou ambos de um arquivo. Ao chamar este método, preencha apenas os campos na solicitação que deseja modificar. Ao atualizar campos, alguns podem ser alterados automaticamente, como `modifiedDate`. Este método oferece suporte a semântica de patch. Este método oferece suporte a um URI */upload* e aceita mídia enviada com as seguintes características: - *Tamanho máximo de arquivo:* 5.120 GB - *Tipos MIME de mídia aceitos:* `*/*` (Especifique um tipo MIME válido, em vez do valor literal `*/*`.)
  - `watch` — Inscreve-se para alterações em um arquivo. Para mais informações, veja [Notificações para alterações de recursos](https://developers.google.com/workspace/drive/api/guides/push).

### operations

  - `get` — Obtém o estado mais recente de uma operação de longa execução. Os clientes podem usar este método para pesquisar o resultado da operação em intervalos conforme recomendado pelo serviço da API.

### permissions

  - `create` — Cria uma permissão para um arquivo ou unidade compartilhada. Para mais informações, veja [Compartilhar arquivos, pastas e unidades](https://developers.google.com/workspace/drive/api/guides/manage-sharing). **Aviso:** Operações de permissões concorrentes no mesmo arquivo não são suportadas; apenas a última atualização é aplicada.
  - `delete` — Deleta uma permissão. Para mais informações, veja [Compartilhar arquivos, pastas e unidades](https://developers.google.com/workspace/drive/api/guides/manage-sharing). **Aviso:** Operações de permissões concorrentes no mesmo arquivo não são suportadas; apenas a última atualização é aplicada.
  - `get` — Obtém uma permissão por ID. Para mais informações, veja [Compartilhar arquivos, pastas e unidades](https://developers.google.com/workspace/drive/api/guides/manage-sharing).
  - `list` — Lista as permissões de um arquivo ou unidade compartilhada. Para mais informações, veja [Compartilhar arquivos, pastas e unidades](https://developers.google.com/workspace/drive/api/guides/manage-sharing).
  - `update` — Atualiza uma permissão com semântica de patch. Para mais informações, veja [Compartilhar arquivos, pastas e unidades](https://developers.google.com/workspace/drive/api/guides/manage-sharing). **Aviso:** Operações de permissões concorrentes no mesmo arquivo não são suportadas; apenas a última atualização é aplicada.

### replies

  - `create` — Cria uma resposta a um comentário. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments).
  - `delete` — Deleta uma resposta. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments).
  - `get` — Obtém uma resposta por ID. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments).
  - `list` — Lista as respostas de um comentário. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments).
  - `update` — Atualiza uma resposta com semântica de patch. Para mais informações, veja [Gerenciar comentários e respostas](https://developers.google.com/workspace/drive/api/guides/manage-comments).

### revisions

  - `delete` — Exclui permanentemente uma versão de arquivo. Você pode deletar revisões apenas para arquivos com conteúdo binário no Google Drive, como imagens ou vídeos. Revisões de outros arquivos, como Google Docs ou Sheets, e a última versão de arquivo restante não podem ser deletadas. Para mais informações, veja [Gerenciar revisões de arquivo](https://developers.google.com/drive/api/guides/manage-revisions).
  - `get` — Obtém os metadados ou conteúdo de uma revisão por ID. Para mais informações, veja [Gerenciar revisões de arquivo](https://developers.google.com/workspace/drive/api/guides/manage-revisions).
  - `list` — Lista as revisões de um arquivo. Para mais informações, veja [Gerenciar revisões de arquivo](https://developers.google.com/workspace/drive/api/guides/manage-revisions). **Importante:** A lista de revisões retornada por este método pode estar incompleta para arquivos com histórico de revisão grande, incluindo Google Docs, Sheets e Slides editados frequentemente. Revisões mais antigas podem ser omitidas da resposta, significando que a primeira revisão retornada pode não ser a revisão existente mais antiga.
  - `update` — Atualiza uma revisão com semântica de patch. Para mais informações, veja [Gerenciar revisões de arquivo](https://developers.google.com/workspace/drive/api/guides/manage-revisions).

### teamdrives

  - `create` — Descontinuado: Use `drives.create`.
  - `delete` — Descontinuado: Use `drives.delete`.
  - `get` — Descontinuado: Use `drives.get`.
  - `list` — Descontinuado: Use `drives.list`.
  - `update` — Descontinuado: Use `drives.update`.

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Procurar recursos e métodos
gws drive --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema drive.<resource>.<method>
```

Use a saída `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws drive --help

# Inspecionar schema do método antes de chamar
gws schema drive.<resource>.<method>

# Executar comando com argumentos
gws drive $ARGUMENTS
```

## Tarefa

Execute a operação do Drive solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws drive --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Gerenciar paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Tratar problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-drive`