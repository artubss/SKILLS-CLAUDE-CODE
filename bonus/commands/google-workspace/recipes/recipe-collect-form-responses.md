---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Recuperar e revisar respostas de um Formulário Google.
---

# Coletar Respostas do Formulário

Executar workflow do Google Workspace: $ARGUMENTS

# Verificar Respostas do Formulário

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-forms`

Recuperar e revisar respostas de um Formulário Google.

## Etapas

1. Listar formulários: `gws forms forms list` (se você não tiver o ID do formulário)
2. Obter detalhes do formulário: `gws forms forms get --params '{"formId": "FORM_ID"}'`
3. Obter respostas: `gws forms forms responses list --params '{"formId": "FORM_ID"}' --format table`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se a CLI `gws` está instalada: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analisar parâmetros da tarefa a partir de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Seguir as etapas descritas acima
   - Substituir IDs de placeholder por valores reais
   - Lidar com erros e tentativas
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada etapa foi concluída com sucesso
   - Verificar alterações no Google Workspace
   - Relatar status final e quaisquer problemas

## Dicas

- Use o flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todos os flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-collect-form-responses`