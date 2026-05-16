---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Copiar um template do Google Docs, preencher conteúdo e compartilhar com colaboradores.
---

# Criar Doc a partir de Template

Execute workflow do Google Workspace: $ARGUMENTS

# Criar um Google Doc a partir de um Template

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-drive`, `gws-docs`

Copie um template do Google Docs, preencha o conteúdo e compartilhe com colaboradores.

## Etapas

1. Copie o template: `gws drive files copy --params '{"fileId": "TEMPLATE_DOC_ID"}' --json '{"name": "Project Brief - Q2 Launch"}'`
2. Obtenha o ID do novo doc da resposta
3. Adicione conteúdo: `gws docs +write --document-id NEW_DOC_ID --text '## Project: Q2 Launch

### Objective
Launch the new feature by end of Q2.'`
4. Compartilhe com o time: `gws drive permissions create --params '{"fileId": "NEW_DOC_ID"}' --json '{"role": "writer", "type": "user", "emailAddress": "team@company.com"}'`

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
   - Siga as etapas descritas acima
   - Substitua IDs placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre o progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as mudanças no Google Workspace
   - Reporte o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as mudanças
- Sempre inspecione os schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-create-doc-from-template`