---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Encontrar mensagens do Gmail com um rótulo específico e encaminhá-las para outro endereço.
---

# Encaminhar Emails com Rótulo

Execute workflow do Google Workspace: $ARGUMENTS

# Encaminhar Mensagens Rotuladas do Gmail

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-gmail`

Encontre mensagens do Gmail com um rótulo específico e encaminhe-as para outro endereço.

## Etapas

1. Encontrar mensagens rotuladas: `gws gmail users messages list --params '{"userId": "me", "q": "label:needs-review"}' --format table`
2. Obter conteúdo da mensagem: `gws gmail users messages get --params '{"userId": "me", "id": "MSG_ID"}'`
3. Encaminhar via novo email: `gws gmail +send --to manager@company.com --subject 'FW: [Original Subject]' --body 'Encaminhando para sua revisão:

[Original Message Body]'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills do GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare os payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre o progresso e os resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-forward-labeled-emails`