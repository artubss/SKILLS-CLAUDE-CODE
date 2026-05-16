---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Transferir propriedade de arquivos do Google Drive de um usuário para outro.
---

# Transferir Propriedade de Arquivo

Execute workflow do Google Workspace: $ARGUMENTS

# Transferir Propriedade de Arquivo

> **PRÉ-REQUISITO:** Carregue as seguintes competências para executar esta receita: `gws-drive`

Transfira a propriedade de arquivos do Google Drive de um usuário para outro.

> [!CAUTION]
> A transferência de propriedade é irreversível sem a cooperação do novo proprietário.

## Etapas

1. Liste arquivos de propriedade do usuário: `gws drive files list --params '{"q": "'\''user@company.com'\'' in owners"}'`
2. Transfira a propriedade: `gws drive permissions create --params '{"fileId": "FILE_ID", "transferOwnership": true}' --json '{"role": "owner", "type": "user", "emailAddress": "newowner@company.com"}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as competências GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa de $ARGUMENTS
   - Valide as entradas necessárias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder por valores reais
   - Trate erros e tentativas
   - Registre o progresso e os resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione schemas de API antes de fazer chamadas: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Competência Original**: `recipe-transfer-file-ownership`