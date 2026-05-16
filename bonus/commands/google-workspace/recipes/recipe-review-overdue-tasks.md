---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Encontre tarefas do Google Tasks que estão vencidas e precisam de atenção.
---

# Revisar Tarefas Vencidas

Execute o workflow do Google Workspace: $ARGUMENTS

# Revisar Tarefas Vencidas

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-tasks`

Encontre tarefas do Google Tasks que estão vencidas e precisam de atenção.

## Etapas

1. Listar listas de tarefas: `gws tasks tasklists list --format table`
2. Listar tarefas com status: `gws tasks tasks list --params '{"tasklist": "TASKLIST_ID", "showCompleted": false}' --format table`
3. Revisar datas de vencimento e priorizar itens vencidos

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills do GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa em $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Reporte o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-review-overdue-tasks`