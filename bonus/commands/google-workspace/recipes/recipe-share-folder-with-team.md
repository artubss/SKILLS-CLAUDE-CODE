---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Compartilhar uma pasta do Google Drive e todos os seus conteúdos com uma lista de colaboradores.
---

# Compartilhar Pasta Com Time

Execute workflow Google Workspace: $ARGUMENTS

# Compartilhar uma Pasta do Google Drive com um Time

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-drive`

Compartilhe uma pasta do Google Drive e todos os seus conteúdos com uma lista de colaboradores.

## Etapas

1. Encontrar a pasta: `gws drive files list --params '{"q": "name = '\''Project X'\'' and mimeType = '\''application/vnd.google-apps.folder'\''"}'`
2. Compartilhar como editor: `gws drive permissions create --params '{"fileId": "FOLDER_ID"}' --json '{"role": "writer", "type": "user", "emailAddress": "colleague@company.com"}'`
3. Compartilhar como visualizador: `gws drive permissions create --params '{"fileId": "FOLDER_ID"}' --json '{"role": "reader", "type": "user", "emailAddress": "stakeholder@company.com"}'`
4. Verificar permissões: `gws drive permissions list --params '{"fileId": "FOLDER_ID"}' --format table`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se `gws` CLI está instalado: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar skills GWS necessárias (consulte a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Análise de parâmetros da tarefa a partir de $ARGUMENTS
   - Validação de entradas obrigatórias
   - Preparação de payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder por valores reais
   - Trate erros e repetições
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique mudanças no Google Workspace
   - Informe status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Consulte a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-share-folder-with-team`