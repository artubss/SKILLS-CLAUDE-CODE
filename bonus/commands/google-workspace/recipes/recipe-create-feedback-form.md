---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar um Google Form para feedback e compartilhá-lo via Gmail.
---

# Criar Formulário de Feedback

Execute o workflow do Google Workspace: $ARGUMENTS

# Criar e Compartilhar um Google Form

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-forms`, `gws-gmail`

Criar um Google Form para feedback e compartilhá-lo via Gmail.

## Etapas

1. Criar formulário: `gws forms forms create --json '{"info": {"title": "Event Feedback", "documentTitle": "Event Feedback Form"}}'`
2. Obtenha a URL do formulário da resposta (campo responderUri)
3. Enviar por email o formulário: `gws gmail +send --to attendees@company.com --subject 'Please share your feedback' --body 'Fill out the form: FORM_URL'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills obrigatórias do GWS (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Parse dos parâmetros da tarefa a partir de $ARGUMENTS
   - Validação de entradas obrigatórias
   - Preparação de payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder por valores reais
   - Trate erros e implementar retentativas
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione os schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-create-feedback-form`