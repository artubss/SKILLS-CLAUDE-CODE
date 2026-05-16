---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Ler conteúdo de um Google Doc e usá-lo como corpo de uma mensagem Gmail.
---

# Rascunho de Email a Partir de Doc

Execute workflow Google Workspace: $ARGUMENTS

# Rascunhe uma Mensagem Gmail a Partir de um Google Doc

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-docs`, `gws-gmail`

Leia conteúdo de um Google Doc e use-o como corpo de uma mensagem Gmail.

## Etapas

1. Obtenha o conteúdo do documento: `gws docs documents get --params '{"documentId": "DOC_ID"}'`
2. Copie o texto do conteúdo do corpo
3. Envie o email: `gws gmail +send --to recipient@example.com --subject 'Newsletter Update' --body 'CONTENT_FROM_DOC'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de espaço reservado por valores reais
   - Trate erros e novas tentativas
   - Registre progresso e resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique alterações no Google Workspace
   - Relate o status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-draft-email-from-doc`