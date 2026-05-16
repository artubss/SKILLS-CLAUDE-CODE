---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Encontre mensagens do Gmail correspondentes a uma consulta e envie uma resposta padrão para cada uma.
---

# Responder em Lote a E-mails

Executar workflow do Google Workspace: $ARGUMENTS

# Responder em Lote a Mensagens Gmail Similares

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-gmail`

Encontre mensagens do Gmail correspondentes a uma consulta e envie uma resposta padrão para cada uma.

## Etapas

1. Encontrar mensagens que precisam de respostas: `gws gmail users messages list --params '{"userId": "me", "q": "is:unread from:customers label:support"}' --format table`
2. Ler uma mensagem: `gws gmail users messages get --params '{"userId": "me", "id": "MSG_ID"}'`
3. Enviar uma resposta: `gws gmail +send --to sender@example.com --subject 'Re: Your Request' --body 'Thank you for reaching out. We have received your request and will respond within 24 hours.'`
4. Marcar como lida: `gws gmail users messages modify --params '{"userId": "me", "id": "MSG_ID"}' --json '{"removeLabelIds": ["UNREAD"]}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se `gws` CLI está instalado: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar skills necessárias do GWS (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analisar parâmetros de tarefa de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Seguir as etapas descritas acima
   - Substituir IDs de placeholder por valores reais
   - Tratar erros e retentativas
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada etapa foi concluída com sucesso
   - Verificar alterações no Google Workspace
   - Relatar status final e eventuais problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-batch-reply-to-emails`