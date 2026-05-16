---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Enviar arquivos locais para um projeto do Google Apps Script.
---

# Deploy Apps Script

Execute fluxo de trabalho do Google Workspace: $ARGUMENTS

# Implantar um Projeto Apps Script

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-apps-script`

Envie arquivos locais para um projeto do Google Apps Script.

## Etapas

1. Listar projetos existentes: `gws apps-script projects list --format table`
2. Obter conteúdo do projeto: `gws apps-script projects getContent --params '{"scriptId": "SCRIPT_ID"}'`
3. Atualizar conteúdo: `gws apps-script projects updateContent --params '{"scriptId": "SCRIPT_ID"}' --json '{"files": [{"name": "Code", "type": "SERVER_JS", "source": "function main() { ... }"}]}'`
4. Criar uma nova versão: `gws apps-script projects versions create --params '{"scriptId": "SCRIPT_ID"}' --json '{"description": "v2 release"}'`

## Tarefa

Execute este fluxo de trabalho com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Faça parse dos parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Fluxo de Trabalho**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e retentativas
   - Registre o progresso e os resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-deploy-apps-script`