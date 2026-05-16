---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Renomear múltiplos arquivos do Google Drive correspondentes a um padrão para seguir uma convenção de nomenclatura consistente.
---

# Renomear Arquivos em Lote

Execute workflow Google Workspace: $ARGUMENTS

# Renomear Arquivos do Google Drive em Lote

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-drive`

Renomear múltiplos arquivos do Google Drive correspondentes a um padrão para seguir uma convenção de nomenclatura consistente.

## Etapas

1. Localizar arquivos para renomear: `gws drive files list --params '{"q": "name contains '\''Report'\''"}' --format table`
2. Renomear um arquivo: `gws drive files update --params '{"fileId": "FILE_ID"}' --json '{"name": "2025-Q1 Report - Final"}'`
3. Verificar a renomeação: `gws drive files get --params '{"fileId": "FILE_ID", "fields": "name"}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se a CLI `gws` está instalada: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar as skills GWS necessárias (consulte a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analisar parâmetros da tarefa de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Seguir as etapas descritas acima
   - Substituir IDs de placeholder pelos valores reais
   - Lidar com erros e repetições
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada etapa foi concluída com sucesso
   - Verificar mudanças no Google Workspace
   - Relatar status final e eventuais problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as mudanças
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-batch-rename-files`