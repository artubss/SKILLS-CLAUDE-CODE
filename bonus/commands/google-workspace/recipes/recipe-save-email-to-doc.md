---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Salvar corpo de mensagem do Gmail em um Google Doc para arquivo ou referência.
---

# Salvar Email em Doc

Execute workflow do Google Workspace: $ARGUMENTS

# Salvar Mensagem do Gmail em Google Docs

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-gmail`, `gws-docs`

Salve o corpo de uma mensagem do Gmail em um Google Doc para arquivo ou referência.

## Passos

1. Encontre a mensagem: `gws gmail users messages list --params '{"userId": "me", "q": "subject:important from:boss@company.com"}' --format table`
2. Obtenha o conteúdo da mensagem: `gws gmail users messages get --params '{"userId": "me", "id": "MSG_ID"}'`
3. Crie um doc com o conteúdo: `gws docs documents create --json '{"title": "Saved Email - Important Update"}'`
4. Escreva o corpo do email: `gws docs +write --document-id DOC_ID --text 'From: boss@company.com
Subject: Important Update

[EMAIL BODY]'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se o CLI `gws` está instalado: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (veja seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute os Passos do Workflow**
   - Siga os passos descritos acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e retentativas
   - Registre progresso e resultados

4. **Verifique Resultados**
   - Confirme que cada passo foi concluído com sucesso
   - Verifique alterações no Google Workspace
   - Reporte status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar mudanças
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-save-email-to-doc`