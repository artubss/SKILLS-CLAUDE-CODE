---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar um Google Shared Drive e adicionar membros com papéis apropriados.
---

# Criar Shared Drive

Execute workflow do Google Workspace: $ARGUMENTS

# Criar e Configurar um Shared Drive

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-drive`

Crie um Google Shared Drive e adicione membros com papéis apropriados.

## Passos

1. Criar shared drive: `gws drive drives create --params '{"requestId": "unique-id-123"}' --json '{"name": "Project X"}'`
2. Adicionar um membro: `gws drive permissions create --params '{"fileId": "DRIVE_ID", "supportsAllDrives": true}' --json '{"role": "writer", "type": "user", "emailAddress": "member@company.com"}'`
3. Listar membros: `gws drive permissions list --params '{"fileId": "DRIVE_ID", "supportsAllDrives": true}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme autenticação: `gws auth status`
   - Carregue as skills do GWS necessárias (confira a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa de $ARGUMENTS
   - Valide as entradas necessárias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga os passos descritos acima
   - Substitua IDs placeholder por valores reais
   - Trate erros e tentativas novamente
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as mudanças no Google Workspace
   - Reporte status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar mudanças
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-create-shared-drive`