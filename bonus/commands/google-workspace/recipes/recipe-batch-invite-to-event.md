---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Adicione uma lista de participantes a um evento existente do Google Calendar e envie notificações.
---

# Convite em Lote para Evento

Execute workflow do Google Workspace: $ARGUMENTS

# Adicionar Múltiplos Participantes a um Evento de Calendário

> **PRÉ-REQUISITO:** Carregue as seguintes competências para executar esta receita: `gws-calendar`

Adicione uma lista de participantes a um evento existente do Google Calendar e envie notificações.

## Etapas

1. Obter o evento: `gws calendar events get --params '{"calendarId": "primary", "eventId": "EVENT_ID"}'`
2. Adicionar participantes: `gws calendar events patch --params '{"calendarId": "primary", "eventId": "EVENT_ID", "sendUpdates": "all"}' --json '{"attendees": [{"email": "alice@company.com"}, {"email": "bob@company.com"}, {"email": "carol@company.com"}]}'`
3. Verificar participantes: `gws calendar events get --params '{"calendarId": "primary", "eventId": "EVENT_ID"}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se o CLI `gws` está instalado: `gws --version`
   - Confirme autenticação: `gws auth status`
   - Carregue as competências GWS necessárias (veja seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme se cada etapa foi concluída com sucesso
   - Verifique alterações no Google Workspace
   - Informe status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de fazer chamadas: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Competência Original**: `recipe-batch-invite-to-event`