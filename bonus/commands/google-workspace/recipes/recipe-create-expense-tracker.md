---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Configure uma planilha Google Sheets para rastreamento de despesas com cabeçalhos e entradas iniciais.
---

# Criar Rastreador de Despesas

Execute workflow Google Workspace: $ARGUMENTS

# Criar um Rastreador de Despesas no Google Sheets

> **PRÉ-REQUISITO:** Carregue as seguintes competências para executar esta receita: `gws-sheets`, `gws-drive`

Configure uma planilha Google Sheets para rastreamento de despesas com cabeçalhos e entradas iniciais.

## Etapas

1. Criar planilha: `gws drive files create --json '{"name": "Expense Tracker 2025", "mimeType": "application/vnd.google-apps.spreadsheet"}'`
2. Adicionar cabeçalhos: `gws sheets +append --spreadsheet-id SHEET_ID --range 'Sheet1' --values '["Date", "Category", "Description", "Amount"]'`
3. Adicionar primeira entrada: `gws sheets +append --spreadsheet-id SHEET_ID --range 'Sheet1' --values '["2025-01-15", "Travel", "Flight to NYC", "450.00"]'`
4. Compartilhar com gerente: `gws drive permissions create --params '{"fileId": "SHEET_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "manager@company.com"}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme autenticação: `gws auth status`
   - Carregue as competências GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise parâmetros da tarefa em $ARGUMENTS
   - Valide entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de espaço reservado por valores reais
   - Trate erros e tentativas novamente
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique alterações no Google Workspace
   - Reporte status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Competência Original**: `recipe-create-expense-tracker`