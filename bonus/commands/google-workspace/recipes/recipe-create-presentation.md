---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar uma nova apresentação do Google Slides e adicionar slides iniciais.
---

# Criar Apresentação

Execute workflow do Google Workspace: $ARGUMENTS

# Criar uma Apresentação do Google Slides

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar essa receita: `gws-slides`

Crie uma nova apresentação do Google Slides e adicione slides iniciais.

## Etapas

1. Criar apresentação: `gws slides presentations create --json '{"title": "Quarterly Review Q2"}'`
2. Obtenha o ID da apresentação da resposta
3. Compartilhe com o time: `gws drive permissions create --params '{"fileId": "PRESENTATION_ID"}' --json '{"role": "writer", "type": "user", "emailAddress": "team@company.com"}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (confira a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder por valores reais
   - Lide com erros e tentativas
   - Registre o progresso e os resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relatar o status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione esquemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-create-presentation`