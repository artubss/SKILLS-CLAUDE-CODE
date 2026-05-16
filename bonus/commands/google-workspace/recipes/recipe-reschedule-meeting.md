---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Mover um evento do Google Agenda para um novo horário e notificar automaticamente todos os participantes.
---

# Reagendar Reunião

Execute workflow do Google Workspace: $ARGUMENTS

# Reagendar uma Reunião do Google Agenda

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-calendar`

Mover um evento do Google Agenda para um novo horário e notificar automaticamente todos os participantes.

## Etapas

1. Localizar o evento: `gws calendar +agenda`
2. Obter detalhes do evento: `gws calendar events get --params '{"calendarId": "primary", "eventId": "EVENT_ID"}'`
3. Atualizar o horário: `gws calendar events patch --params '{"calendarId": "primary", "eventId": "EVENT_ID", "sendUpdates": "all"}' --json '{"start": {"dateTime": "2025-01-22T14:00:00", "timeZone": "America/New_York"}, "end": {"dateTime": "2025-01-22T15:00:00", "timeZone": "America/New_York"}}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se a CLI `gws` está instalada: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Fazer parsing dos parâmetros da tarefa de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Seguir as etapas descritas acima
   - Substituir IDs de placeholder pelos valores reais
   - Tratar erros e tentativas novamente
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada etapa foi concluída com sucesso
   - Verificar alterações no Google Workspace
   - Relatar status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-reschedule-meeting`