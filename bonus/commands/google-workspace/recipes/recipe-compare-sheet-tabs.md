---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Ler dados de duas abas em uma Planilha Google para comparar e identificar diferenças.
---

# Comparar Abas da Planilha

Execute workflow do Google Workspace: $ARGUMENTS

# Comparar Duas Abas de Planilhas Google

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-sheets`

Leia dados de duas abas em uma Planilha Google para comparar e identificar diferenças.

## Etapas

1. Leia a primeira aba: `gws sheets +read --spreadsheet-id SHEET_ID --range 'January!A1:D'`
2. Leia a segunda aba: `gws sheets +read --spreadsheet-id SHEET_ID --range 'February!A1:D'`
3. Compare os dados e identifique as alterações

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e tentativas novamente
   - Registre o progresso e os resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-compare-sheet-tabs`