---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Encontre mensagens do Gmail com anexos e salve-os em uma pasta do Google Drive.
---

# Salvar Anexos de Email

Execute o workflow do Google Workspace: $ARGUMENTS

# Salvar Anexos do Gmail no Google Drive

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-gmail`, `gws-drive`

Encontre mensagens do Gmail com anexos e salve-os em uma pasta do Google Drive.

## Etapas

1. Pesquise emails com anexos: `gws gmail users messages list --params '{"userId": "me", "q": "has:attachment from:client@example.com"}' --format table`
2. Obtenha detalhes da mensagem: `gws gmail users messages get --params '{"userId": "me", "id": "MESSAGE_ID"}'`
3. Baixe o anexo: `gws gmail users messages attachments get --params '{"userId": "me", "messageId": "MESSAGE_ID", "id": "ATTACHMENT_ID"}'`
4. Envie para a pasta do Drive: `gws drive +upload --file ./attachment.pdf --parent FOLDER_ID`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills necessárias do GWS (consulte a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa em $ARGUMENTS
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
   - Informe o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione os schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-save-email-attachments`