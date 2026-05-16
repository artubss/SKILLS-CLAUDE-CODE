---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Exportar uma planilha do Google Sheets como arquivo CSV para backup local ou processamento.
---

# Backup Sheet As Csv

Execute o workflow do Google Workspace: $ARGUMENTS

# Exportar uma planilha do Google Sheets como CSV

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-sheets`, `gws-drive`

Exportar uma planilha do Google Sheets como arquivo CSV para backup local ou processamento.

## Etapas

1. Obter detalhes da planilha: `gws sheets spreadsheets get --params '{"spreadsheetId": "SHEET_ID"}'`
2. Exportar como CSV: `gws drive files export --params '{"fileId": "SHEET_ID", "mimeType": "text/csv"}'`
3. Ou ler valores diretamente: `gws sheets +read --spreadsheet-id SHEET_ID --range 'Sheet1' --format csv`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se o CLI `gws` está instalado: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (consulte a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros de tarefa de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre o progresso e os resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relatar status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-backup-sheet-as-csv`