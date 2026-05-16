---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Leia dados de uma Planilha Google e crie um relatório formatado no Google Docs.
---

# Gerar Relatório a Partir de Planilha

Execute workflow do Google Workspace: $ARGUMENTS

# Gerar um Relatório do Google Docs a Partir de Dados de Planilha

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-sheets`, `gws-docs`, `gws-drive`

Leia dados de uma Planilha Google e crie um relatório formatado no Google Docs.

## Etapas

1. Ler os dados: `gws sheets +read --spreadsheet-id SHEET_ID --range 'Sales!A1:D'`
2. Criar o documento de relatório: `gws docs documents create --json '{"title": "Sales Report - January 2025"}'`
3. Escrever o relatório: `gws docs +write --document-id DOC_ID --text '## Sales Report - January 2025

### Summary
Total deals: 45
Revenue: $125,000

### Top Deals
1. Acme Corp - $25,000
2. Widget Inc - $18,000'`
4. Compartilhar com stakeholders: `gws drive permissions create --params '{"fileId": "DOC_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "cfo@company.com"}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se `gws` CLI está instalado: `gws --version`
   - Confirme autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros de tarefa de $ARGUMENTS
   - Valide entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e retentativas
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-generate-report-from-sheet`