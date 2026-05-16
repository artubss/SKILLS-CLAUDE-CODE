---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar blocos de tempo de foco recorrentes no Google Calendar para proteger horas de trabalho profundo.
---

# Bloquear Tempo de Foco

Executar workflow do Google Workspace: $ARGUMENTS

# Bloquear Tempo de Foco no Google Calendar

> **PRÉ-REQUISITO:** Carregue as seguintes habilidades para executar esta receita: `gws-calendar`

Criar blocos de tempo de foco recorrentes no Google Calendar para proteger horas de trabalho profundo.

## Etapas

1. Criar bloco de foco recorrente: `gws calendar events insert --params '{"calendarId": "primary"}' --json '{"summary": "Focus Time", "description": "Protected deep work block", "start": {"dateTime": "2025-01-20T09:00:00", "timeZone": "America/New_York"}, "end": {"dateTime": "2025-01-20T11:00:00", "timeZone": "America/New_York"}, "recurrence": ["RRULE:FREQ=WEEKLY;BYDAY=MO,TU,WE,TH,FR"], "transparency": "opaque"}'`
2. Verificar se aparece como ocupado: `gws calendar +agenda`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se a CLI `gws` está instalada: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar habilidades GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analisar parâmetros de tarefa de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Seguir as etapas descritas acima
   - Substituir IDs de placeholder pelos valores reais
   - Tratar erros e tentar novamente
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada etapa foi concluída com sucesso
   - Verificar alterações no Google Workspace
   - Relatar status final e qualquer problema

## Dicas

- Use o flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique ajuda de comando para todos os flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Habilidade Original**: `recipe-block-focus-time`