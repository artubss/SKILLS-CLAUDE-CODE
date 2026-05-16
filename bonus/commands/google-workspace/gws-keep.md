---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Gerenciar notas do Google Keep.
---

# Google Workspace Keep

Execute operações do Google Workspace Keep: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws keep --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# keep (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criá-lo.

```bash
gws keep <resource> <method> [flags]
```

## Recursos da API

### media

  - `download` — Obtém um anexo. Para fazer download de mídia de anexo via REST, é necessário o parâmetro de query alt=media. Retorna um erro de solicitação 400 se a mídia do anexo não estiver disponível no tipo MIME solicitado.

### notes

  - `create` — Cria uma nova nota.
  - `delete` — Deleta uma nota. O chamador deve ter a função `OWNER` na nota para deletá-la. Deletar uma nota remove o recurso imediatamente e não pode ser desfeito. Qualquer colaborador perderá acesso à nota.
  - `get` — Obtém uma nota.
  - `list` — Lista notas. Cada chamada de lista retorna uma página de resultados com `page_size` como limite superior de itens retornados. Um `page_size` de zero permite que o servidor escolha o limite superior. ListNotesResponse contém no máximo `page_size` entradas. Se houver mais itens para listar, ele fornece um valor `next_page_token`. (Os tokens de página são valores opacos.) Para obter a próxima página de resultados, copie `next_page_token` do resultado para a próxima solicitação em `page_token`.
  - `permissions` — Operações no recurso 'permissions'

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Procurar recursos e métodos
gws keep --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema keep.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws keep --help

# Inspecionar schema do método antes de chamar
gws schema keep.<resource>.<method>

# Executar comando com argumentos
gws keep $ARGUMENTS
```

## Tarefa

Execute a operação Keep solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws keep --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar a saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-keep`