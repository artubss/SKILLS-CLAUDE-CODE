---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Configurar uma nova lista de tarefas do Google Tasks com tarefas iniciais.
---

# Criar Lista de Tarefas

Executar workflow do Google Workspace: $ARGUMENTS

# Criar uma Lista de Tarefas e Adicionar Tarefas

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-tasks`

Configure uma nova lista de tarefas do Google Tasks com tarefas iniciais.

## Passos

1. Criar lista de tarefas: `gws tasks tasklists insert --json '{"title": "Q2 Goals"}'`
2. Adicionar uma tarefa: `gws tasks tasks insert --params '{"tasklist": "TASKLIST_ID"}' --json '{"title": "Review Q1 metrics", "notes": "Pull data from analytics dashboard", "due": "2024-04-01T00:00:00Z"}'`
3. Adicionar outra tarefa: `gws tasks tasks insert --params '{"tasklist": "TASKLIST_ID"}' --json '{"title": "Draft Q2 OKRs"}'`
4. Listar tarefas: `gws tasks tasks list --params '{"tasklist": "TASKLIST_ID"}' --format table`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills necessárias do GWS (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Parse dos parâmetros de tarefa de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare os payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga os passos descritos acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e retentativas
   - Registre progresso e resultados

4. **Verificar Resultados**
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
**Skill Original**: `recipe-create-task-list`