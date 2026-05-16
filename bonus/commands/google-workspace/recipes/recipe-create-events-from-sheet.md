---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Ler dados de eventos de uma planilha do Google Sheets e criar entradas no Google Calendar para cada linha.
---

# Criar Eventos a Partir de Planilha

Execute workflow do Google Workspace: $ARGUMENTS

# Criar Eventos do Google Calendar a Partir de uma Planilha

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-sheets`, `gws-calendar`

Leia dados de eventos de uma planilha do Google Sheets e crie entradas no Google Calendar para cada linha.

## Etapas

1. Ler dados de eventos: `gws sheets +read --spreadsheet-id SHEET_ID --range 'Events!A2:D'`
2. Para cada linha, criar um evento no calendário: `gws calendar +insert --summary 'Team Standup' --start '2025-01-20T09:00' --duration 30 --attendees alice@company.com,bob@company.com`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme autenticação: `gws auth status`
   - Carregue as skills obrigatórias do GWS (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique alterações no Google Workspace
   - Relate status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-create-events-from-sheet`