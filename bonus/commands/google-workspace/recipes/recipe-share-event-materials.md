---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Compartilhar arquivos do Google Drive com todos os participantes de um evento do Google Agenda.
---

# Compartilhar Materiais do Evento

Execute workflow do Google Workspace: $ARGUMENTS

# Compartilhar Arquivos com Participantes da Reunião

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-calendar`, `gws-drive`

Compartilhe arquivos do Google Drive com todos os participantes de um evento do Google Agenda.

## Etapas

1. Obter participantes do evento: `gws calendar events get --params '{"calendarId": "primary", "eventId": "EVENT_ID"}'`
2. Compartilhar arquivo com cada participante: `gws drive permissions create --params '{"fileId": "FILE_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "attendee@company.com"}'`
3. Verificar compartilhamento: `gws drive permissions list --params '{"fileId": "FILE_ID"}' --format table`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se `gws` CLI está instalado: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar as skills GWS obrigatórias (confira a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analisar parâmetros da tarefa de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique alterações no Google Workspace
   - Reporte status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-share-event-materials`