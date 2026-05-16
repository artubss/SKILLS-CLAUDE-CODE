---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Encontre e revise arquivos do Google Drive compartilhados fora da organização.
---

# Auditoria de Compartilhamento Externo

Execute workflow do Google Workspace: $ARGUMENTS

# Auditoria de Compartilhamento Externo no Drive

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-drive`

Encontre e revise arquivos do Google Drive compartilhados fora da organização.

> [!CAUTION]
> Revogar permissões remove o acesso imediatamente. Confirme com o proprietário do arquivo primeiro.

## Passos

1. Listar arquivos compartilhados externamente: `gws drive files list --params '{"q": "visibility = '\''anyoneWithLink'\''"}'`
2. Verificar permissões em um arquivo: `gws drive permissions list --params '{"fileId": "FILE_ID"}'`
3. Revogar se necessário: `gws drive permissions delete --params '{"fileId": "FILE_ID", "permissionId": "PERM_ID"}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se o CLI `gws` está instalado: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (confira a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa em $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e novas tentativas
   - Registre progresso e resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique alterações no Google Workspace
   - Informe o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todos os flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-audit-external-sharing`