---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Revise sua semana no Google Calendar, identifique lacunas e adicione eventos para preenchê-las.
---

# Planejar Agenda Semanal

Execute workflow do Google Workspace: $ARGUMENTS

# Planeje sua Agenda Semanal do Google Calendar

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar essa receita: `gws-calendar`

Revise seu Google Calendar da semana, identifique lacunas e adicione eventos para preenchê-las.

## Passos

1. Verifique a agenda desta semana: `gws calendar +agenda`
2. Verifique disponibilidade/ocupação da semana: `gws calendar freebusy query --json '{"timeMin": "2025-01-20T00:00:00Z", "timeMax": "2025-01-25T00:00:00Z", "items": [{"id": "primary"}]}'`
3. Adicione um novo evento: `gws calendar +insert --summary 'Deep Work Block' --start '2025-01-21T14:00' --duration 120`
4. Revise a agenda atualizada: `gws calendar +agenda`

## Tarefa

Execute esse workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros de tarefa de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de espaço reservado pelos valores reais
   - Trate erros e novas tentativas
   - Registre progresso e resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de fazer chamadas: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-plan-weekly-schedule`