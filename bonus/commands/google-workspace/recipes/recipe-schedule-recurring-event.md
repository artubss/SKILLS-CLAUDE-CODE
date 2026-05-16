---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar um evento recorrente no Google Agenda com participantes.
---

# Agendar Evento Recorrente

Execute workflow do Google Workspace: $ARGUMENTS

# Agendar uma Reunião Recorrente

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-calendar`

Criar um evento recorrente no Google Agenda com participantes.

## Etapas

1. Criar evento recorrente: `gws calendar events insert --params '{"calendarId": "primary"}' --json '{"summary": "Weekly Standup", "start": {"dateTime": "2024-03-18T09:00:00", "timeZone": "America/New_York"}, "end": {"dateTime": "2024-03-18T09:30:00", "timeZone": "America/New_York"}, "recurrence": ["RRULE:FREQ=WEEKLY;BYDAY=MO"], "attendees": [{"email": "team@company.com"}]}'`
2. Verificar se foi criado: `gws calendar +agenda --days 14 --format table`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se a CLI `gws` está instalada: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Fazer parse dos parâmetros da tarefa a partir de $ARGUMENTS
   - Validar inputs obrigatórios
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Seguir as etapas descritas acima
   - Substituir IDs de placeholder pelos valores reais
   - Tratar erros e retries
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada etapa foi concluída com sucesso
   - Verificar as alterações no Google Workspace
   - Relatar status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-schedule-recurring-event`