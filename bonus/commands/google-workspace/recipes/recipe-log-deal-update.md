---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Anexar uma atualização de status de negociação a uma planilha Google Sheets de rastreamento de vendas.
---

# Registrar Atualização de Negociação

Execute workflow Google Workspace: $ARGUMENTS

# Registrar Atualização de Negociação na Planilha

> **PRÉ-REQUISITO:** Carregue as seguintes competências para executar esta receita: `gws-sheets`, `gws-drive`

Anexe uma atualização de status de negociação a uma planilha Google Sheets de rastreamento de vendas.

## Etapas

1. Localizar a planilha de rastreamento: `gws drive files list --params '{"q": "name = '\''Sales Pipeline'\'' and mimeType = '\''application/vnd.google-apps.spreadsheet'\''"}'`
2. Ler dados atuais: `gws sheets +read --spreadsheet-id SHEET_ID --range 'Pipeline!A1:F'`
3. Anexar nova linha: `gws sheets +append --spreadsheet-id SHEET_ID --range 'Pipeline' --values '["2024-03-15", "Acme Corp", "Proposal Sent", "$50,000", "Q2", "jdoe"]'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-Requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as competências GWS necessárias (confira a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre o progresso e os resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Consulte a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Competência Original**: `recipe-log-deal-update`