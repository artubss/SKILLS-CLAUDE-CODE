---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Excluir um evento do Google Agenda e enviar um email de cancelamento via Gmail.
---

# Cancelar e Notificar

Execute o workflow do Google Workspace: $ARGUMENTS

# Cancelar Reunião e Notificar Participantes

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-calendar`, `gws-gmail`

Exclua um evento do Google Agenda e envie um email de cancelamento via Gmail.

> [!CAUTION]
> Deletar com sendUpdates envia emails de cancelamento para todos os participantes.

## Etapas

1. Encontre a reunião: `gws calendar +agenda --format json` e localize o ID do evento
2. Exclua o evento: `gws calendar events delete --params '{"calendarId": "primary", "eventId": "EVENT_ID", "sendUpdates": "all"}'`
3. Envie acompanhamento: `gws gmail +send --to attendees --subject 'Meeting Cancelled: [Title]' --body 'Apologies, this meeting has been cancelled.'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills do GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas necessárias
   - Prepare os payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre o progresso e os resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as mudanças no Google Workspace
   - Relate o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione os esquemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-cancel-and-notify`