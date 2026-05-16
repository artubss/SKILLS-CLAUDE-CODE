---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [sinalizadores]
description: Google Sheets: Ler e escrever planilhas.
---

# Google Workspace Sheets

Execute operações do Google Workspace Sheets: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws sheets --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# sheets (v4)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, sinalizadores globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criar.

```bash
gws sheets <recurso> <método> [sinalizadores]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|-------------|
| [`+append`](../gws-sheets-append/SKILL.md) | Acrescentar uma linha a uma planilha |
| [`+read`](../gws-sheets-read/SKILL.md) | Ler valores de uma planilha |

## Recursos de API

### spreadsheets

  - `batchUpdate` — Aplica uma ou mais atualizações à planilha. Cada solicitação é validada antes de ser aplicada. Se alguma solicitação não for válida, toda a solicitação falhará e nada será aplicado. Algumas solicitações têm respostas para fornecer informações sobre como são aplicadas. As respostas espelham as solicitações. Por exemplo, se você aplicou 4 atualizações e a 3ª teve uma resposta, a resposta terá 2 respostas vazias, a resposta real e outra resposta vazia, nessa ordem.
  - `create` — Cria uma planilha, retornando a planilha recém-criada.
  - `get` — Retorna a planilha com o ID fornecido. O chamador deve especificar o ID da planilha. Por padrão, os dados dentro de grades não são retornados. Você pode incluir dados de grade de uma das duas maneiras: * Especifique uma [máscara de campo](https://developers.google.com/workspace/sheets/api/guides/field-masks) listando seus campos desejados usando o parâmetro de URL `fields` em HTTP * Defina o parâmetro de URL includeGridData como verdadeiro.
  - `getByDataFilter` — Retorna a planilha com o ID fornecido. O chamador deve especificar o ID da planilha. Para mais informações, consulte [Ler, escrever e pesquisar metadados](https://developers.google.com/workspace/sheets/api/guides/metadata). Este método difere de GetSpreadsheet porque permite selecionar quais subconjuntos de dados da planilha retornar especificando um parâmetro dataFilters. Múltiplos DataFilters podem ser especificados.
  - `developerMetadata` — Operações no recurso 'developerMetadata'
  - `sheets` — Operações no recurso 'sheets'
  - `values` — Operações no recurso 'values'

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Procurar recursos e métodos
gws sheets --help

# Inspecionar parâmetros, tipos e padrões necessários de um método
gws schema sheets.<recurso>.<método>
```

Use a saída de `gws schema` para criar seus sinalizadores `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws sheets --help

# Inspecionar schema do método antes de chamar
gws schema sheets.<recurso>.<método>

# Executar comando com argumentos
gws sheets $ARGUMENTS
```

## Tarefa

Execute a operação Sheets solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws sheets --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com sinalizadores apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Tratar problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-sheets`