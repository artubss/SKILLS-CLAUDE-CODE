---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Duplicar uma aba de modelo do Google Sheets para um novo mês de rastreamento.
---

# Copiar Planilha Para Novo Mês

Execute o workflow do Google Workspace: $ARGUMENTS

# Copiar uma Google Planilha para um Novo Mês

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-sheets`

Duplicar uma aba de modelo do Google Sheets para um novo mês de rastreamento.

## Etapas

1. Obter detalhes da planilha: `gws sheets spreadsheets get --params '{"spreadsheetId": "SHEET_ID"}'`
2. Copiar a aba de modelo: `gws sheets spreadsheets sheets copyTo --params '{"spreadsheetId": "SHEET_ID", "sheetId": 0}' --json '{"destinationSpreadsheetId": "SHEET_ID"}'`
3. Renomear a nova aba: `gws sheets spreadsheets batchUpdate --params '{"spreadsheetId": "SHEET_ID"}' --json '{"requests": [{"updateSheetProperties": {"properties": {"sheetId": 123, "title": "Fevereiro 2025"}, "fields": "title"}}]}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa em $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare os payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre o progresso e os resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione os schemas da API antes de fazer chamadas: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-copy-sheet-for-new-month`