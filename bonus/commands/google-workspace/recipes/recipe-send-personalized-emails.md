---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Ler dados de destinatários do Google Sheets e enviar mensagens personalizadas do Gmail para cada linha.
---

# Enviar E-mails Personalizados

Execute workflow do Google Workspace: $ARGUMENTS

# Enviar E-mails Personalizados a Partir de uma Planilha

> **PRÉ-REQUISITO:** Carregue as seguintes competências para executar esta receita: `gws-sheets`, `gws-gmail`

Leia dados de destinatários do Google Sheets e envie mensagens personalizadas do Gmail para cada linha.

## Etapas

1. Ler lista de destinatários: `gws sheets +read --spreadsheet-id SHEET_ID --range 'Contacts!A2:C'`
2. Para cada linha, enviar um e-mail personalizado: `gws gmail +send --to recipient@example.com --subject 'Hello, Name' --body 'Hi Name, your report is ready.'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as competências GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa de $ARGUMENTS
   - Valide as entradas necessárias
   - Prepare payloads e flags em JSON

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e tentativas
   - Registre o progresso e resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todos os flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Competência Original**: `recipe-send-personalized-emails`