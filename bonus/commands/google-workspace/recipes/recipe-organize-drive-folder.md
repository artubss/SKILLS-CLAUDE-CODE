---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar uma estrutura de pastas no Google Drive e mover arquivos para os locais corretos.
---

# Organizar Pasta do Drive

Execute workflow do Google Workspace: $ARGUMENTS

# Organizar Arquivos em Pastas do Google Drive

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-drive`

Criar uma estrutura de pastas no Google Drive e mover arquivos para os locais corretos.

## Etapas

1. Criar pasta de projeto: `gws drive files create --json '{"name": "Q2 Project", "mimeType": "application/vnd.google-apps.folder"}'`
2. Criar subpastas: `gws drive files create --json '{"name": "Documents", "mimeType": "application/vnd.google-apps.folder", "parents": ["PARENT_FOLDER_ID"]}'`
3. Mover arquivos existentes para pasta: `gws drive files update --params '{"fileId": "FILE_ID", "addParents": "FOLDER_ID", "removeParents": "OLD_PARENT_ID"}'`
4. Verificar estrutura: `gws drive files list --params '{"q": "FOLDER_ID in parents"}' --format table`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se `gws` CLI está instalado: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Fazer parsing dos parâmetros da tarefa em $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Seguir as etapas descritas acima
   - Substituir IDs de placeholder pelos valores reais
   - Lidar com erros e tentativas
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada etapa foi concluída com sucesso
   - Verificar alterações no Google Workspace
   - Relatar status final e qualquer problema

## Dicas

- Use o flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de fazer chamadas: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todos os flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-organize-drive-folder`