---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Inscreva-se para receber notificações de alterações em um arquivo ou pasta do Google Drive.
---

# Monitorar Alterações do Drive

Execute workflow do Google Workspace: $ARGUMENTS

# Monitorar Alterações do Drive

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-events`

Inscreva-se para receber notificações de alterações em um arquivo ou pasta do Google Drive.

## Etapas

1. Criar inscrição: `gws events subscriptions create --json '{"targetResource": "//drive.googleapis.com/drives/DRIVE_ID", "eventTypes": ["google.workspace.drive.file.v1.updated"], "notificationEndpoint": {"pubsubTopic": "projects/PROJECT/topics/TOPIC"}, "payloadOptions": {"includeResource": true}}'`
2. Listar inscrições ativas: `gws events subscriptions list`
3. Renovar antes do vencimento: `gws events +renew --subscription SUBSCRIPTION_ID`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se a CLI `gws` está instalada: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar as skills necessárias do GWS (consulte a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analisar parâmetros de tarefa de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Seguir as etapas descritas acima
   - Substituir IDs de placeholder pelos valores reais
   - Tratar erros e novas tentativas
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada etapa foi concluída com sucesso
   - Verificar alterações no Google Workspace
   - Relatar status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-watch-drive-changes`