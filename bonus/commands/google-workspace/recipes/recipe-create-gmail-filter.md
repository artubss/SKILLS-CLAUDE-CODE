---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar um filtro do Gmail para rotular, destacar ou categorizar automaticamente mensagens recebidas.
---

# Criar Filtro do Gmail

Execute workflow do Google Workspace: $ARGUMENTS

# Criar um Filtro do Gmail

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-gmail`

Crie um filtro do Gmail para rotular, destacar ou categorizar automaticamente mensagens recebidas.

## Etapas

1. Listar labels existentes: `gws gmail users labels list --params '{"userId": "me"}' --format table`
2. Criar um novo label: `gws gmail users labels create --params '{"userId": "me"}' --json '{"name": "Receipts"}'`
3. Criar um filtro: `gws gmail users settings filters create --params '{"userId": "me"}' --json '{"criteria": {"from": "receipts@example.com"}, "action": {"addLabelIds": ["LABEL_ID"], "removeLabelIds": ["INBOX"]}}'`
4. Verificar filtro: `gws gmail users settings filters list --params '{"userId": "me"}' --format table`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills do GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas delineadas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e retries
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Reporte o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todos os flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-create-gmail-filter`