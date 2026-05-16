---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Compartilhar um documento Google Docs com acesso de edição e notificar colaboradores por email com o link.
---

# Compartilhar Doc e Notificar

Executar workflow do Google Workspace: $ARGUMENTS

# Compartilhar um Google Doc e Notificar Colaboradores

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-drive`, `gws-docs`, `gws-gmail`

Compartilhe um documento Google Docs com acesso de edição e notifique colaboradores por email com o link.

## Passos

1. Encontrar o doc: `gws drive files list --params '{"q": "name contains '\''Project Brief'\'' and mimeType = '\''application/vnd.google-apps.document'\''"}'`
2. Compartilhar com acesso de editor: `gws drive permissions create --params '{"fileId": "DOC_ID"}' --json '{"role": "writer", "type": "user", "emailAddress": "reviewer@company.com"}'`
3. Enviar email com o link: `gws gmail +send --to reviewer@company.com --subject 'Please review: Project Brief' --body 'I have shared the project brief with you: https://docs.google.com/document/d/DOC_ID'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se `gws` CLI está instalado: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar as skills GWS necessárias (consulte a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analisar parâmetros de tarefa de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Passos do Workflow**
   - Seguir os passos descritos acima
   - Substituir IDs de placeholder pelos valores reais
   - Gerenciar erros e tentativas
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada passo foi concluído com sucesso
   - Verificar alterações no Google Workspace
   - Relatar status final e qualquer problema

## Dicas

- Use o flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todos os flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-share-doc-and-notify`