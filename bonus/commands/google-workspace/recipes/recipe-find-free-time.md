---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Consultar status de disponibilidade (free/busy) do Google Calendar para múltiplos usuários a fim de encontrar um horário de reunião.
---

# Encontrar Horários Livres

Executar workflow do Google Workspace: $ARGUMENTS

# Encontrar Horários Livres Entre Calendários

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-calendar`

Consultar status de disponibilidade (free/busy) do Google Calendar para múltiplos usuários a fim de encontrar um horário de reunião.

## Passos

1. Consultar disponibilidade: `gws calendar freebusy query --json '{"timeMin": "2024-03-18T08:00:00Z", "timeMax": "2024-03-18T18:00:00Z", "items": [{"id": "user1@company.com"}, {"id": "user2@company.com"}]}'`
2. Revisar o resultado para encontrar horários livres sobrepostos
3. Criar evento no horário livre: `gws calendar +insert --summary 'Meeting' --attendees user1@company.com,user2@company.com --start '2024-03-18T14:00:00' --duration 30`

## Tarefa

Executar este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se o CLI `gws` está instalado: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar as skills necessárias do GWS (verificar seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analisar parâmetros de tarefa de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Passos do Workflow**
   - Seguir os passos descritos acima
   - Substituir IDs de placeholder pelos valores reais
   - Tratar erros e novas tentativas
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada passo foi concluído com sucesso
   - Verificar mudanças no Google Workspace
   - Relatar status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione schemas de API antes de fazer chamadas: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-find-free-time`