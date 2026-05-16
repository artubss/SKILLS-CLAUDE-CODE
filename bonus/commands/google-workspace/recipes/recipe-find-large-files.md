---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Identificar arquivos grandes do Google Drive que consomem cota de armazenamento.
---

# Localizar Arquivos Grandes

Execute workflow do Google Workspace: $ARGUMENTS

# Localizar Arquivos Maiores no Drive

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-drive`

Identificar arquivos grandes do Google Drive que consomem cota de armazenamento.

> [!CAUTION]
> Deletar arquivos é permanente se a lixeira for esvaziada. Confirme antes de deletar.

## Etapas

1. Listar arquivos ordenados por tamanho: `gws drive files list --params '{"orderBy": "quotaBytesUsed desc", "pageSize": 20, "fields": "files(id,name,size,mimeType,owners)"}' --format table`
2. Revise o resultado e identifique arquivos para deletar ou mover
3. Delete se necessário: `gws drive files delete --params '{"fileId": "FILE_ID"}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas necessárias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e tentativas
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as mudanças no Google Workspace
   - Relate o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar mudanças
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-find-large-files`