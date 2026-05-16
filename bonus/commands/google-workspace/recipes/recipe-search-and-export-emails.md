---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Encontrar mensagens do Gmail correspondentes a uma consulta e exportá-las para revisão.
---

# Pesquisar e Exportar Emails

Execute workflow do Google Workspace: $ARGUMENTS

# Pesquisar e Exportar Emails

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-gmail`

Encontre mensagens do Gmail correspondentes a uma consulta e exporte-as para revisão.

## Etapas

1. Pesquisar emails: `gws gmail users messages list --params '{"userId": "me", "q": "from:client@example.com after:2024/01/01"}'`
2. Obter mensagem completa: `gws gmail users messages get --params '{"userId": "me", "id": "MSG_ID"}'`
3. Exportar resultados: `gws gmail users messages list --params '{"userId": "me", "q": "label:project-x"}' --format json > project-emails.json`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se `gws` CLI está instalado: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills do GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare os payloads JSON e flags

3. **Executar as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e reexecuções
   - Registre o progresso e os resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com êxito
   - Verifique as alterações no Google Workspace
   - Relate o status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione os schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-search-and-export-emails`