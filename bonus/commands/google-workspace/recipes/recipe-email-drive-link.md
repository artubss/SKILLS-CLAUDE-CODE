---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Compartilhar um arquivo do Google Drive e enviar o link por email com uma mensagem para os destinatários.
---

# Enviar Link do Drive por Email

Execute workflow do Google Workspace: $ARGUMENTS

# Enviar Link de um Arquivo do Google Drive por Email

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-drive`, `gws-gmail`

Compartilhe um arquivo do Google Drive e envie o link por email com uma mensagem para os destinatários.

## Etapas

1. Encontrar o arquivo: `gws drive files list --params '{"q": "name = '\''Quarterly Report'\''"}'`
2. Compartilhar o arquivo: `gws drive permissions create --params '{"fileId": "FILE_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "client@example.com"}'`
3. Enviar email com o link: `gws gmail +send --to client@example.com --subject 'Quarterly Report' --body 'Hi, please find the report here: https://docs.google.com/document/d/FILE_ID'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills necessárias do GWS (veja seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa em $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e tente novamente quando necessário
   - Registre o progresso e os resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e qualquer problema encontrado

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-email-drive-link`