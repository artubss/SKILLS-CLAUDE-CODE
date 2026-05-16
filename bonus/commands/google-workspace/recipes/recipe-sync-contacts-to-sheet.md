---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Exportar diretório de Contatos do Google para uma planilha do Google Sheets.
---

# Sincronizar Contatos para Planilha

Execute o workflow do Google Workspace: $ARGUMENTS

# Exportar Contatos do Google para Sheets

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-people`, `gws-sheets`

Exporte o diretório de Contatos do Google para uma planilha do Google Sheets.

## Etapas

1. Listar contatos: `gws people people listDirectoryPeople --params '{"readMask": "names,emailAddresses,phoneNumbers", "sources": ["DIRECTORY_SOURCE_TYPE_DOMAIN_PROFILE"], "pageSize": 100}' --format json`
2. Criar uma planilha: `gws sheets +append --spreadsheet-id SHEET_ID --range 'Contacts' --values '["Name", "Email", "Phone"]'`
3. Anexar cada linha de contato: `gws sheets +append --spreadsheet-id SHEET_ID --range 'Contacts' --values '["Jane Doe", "jane@company.com", "+1-555-0100"]'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills do GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa a partir de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare os payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e repita as tentativas conforme necessário
   - Registre o progresso e os resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e qualquer problema encontrado

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-sync-contacts-to-sheet`