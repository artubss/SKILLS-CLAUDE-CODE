---
name: azure-verified-modules-terraform
description: Crie, atualize ou revise infraestrutura como código (IaC) do Azure em Terraform usando Azure Verified Modules (AVM).
tools: changes, codebase, edit/editFiles, extensions, fetch, findTestFiles, githubRepo, new, openSimpleBrowser, problems, runCommands, runTasks, runTests, search, searchResults, terminalLastCommand, terminalSelection, testFailure, usages, vscodeAPI, microsoft.docs.mcp, azure_get_deployment_best_practices, azure_get_schema_for_Bicep
---

# Modo Azure AVM Terraform

Use Azure Verified Modules para Terraform para impor práticas recomendadas do Azure por meio de módulos pré-construídos.

## Descubra módulos

- Terraform Registry: procure por "avm" + recurso, filtre pela tag Partner.
- Índice AVM: `https://azure.github.io/Azure-Verified-Modules/indexes/terraform/tf-resource-modules/`

## Uso

- **Exemplos**: Copie o exemplo, substitua `source = "../../"` por `source = "Azure/avm-res-{service}-{resource}/azurerm"`, adicione `version`, configure `enable_telemetry`.
- **Personalizado**: Copie as Provision Instructions, defina inputs, fixe `version`.

## Versionamento

- Endpoint: `https://registry.terraform.io/v1/modules/Azure/{module}/azurerm/versions`

## Fontes

- Registry: `https://registry.terraform.io/modules/Azure/{module}/azurerm/latest`
- GitHub: `https://github.com/Azure/terraform-azurerm-avm-res-{service}-{resource}`

## Convenções de nomenclatura

- Recurso: Azure/avm-res-{service}-{resource}/azurerm
- Padrão: Azure/avm-ptn-{pattern}/azurerm
- Utilitário: Azure/avm-utl-{utility}/azurerm

## Práticas recomendadas

- Fixe versões de módulo e provider
- Comece com exemplos oficiais
- Revise inputs e outputs
- Habilite telemetria
- Use módulos utilitários AVM
- Siga os requisitos do provider AzureRM
- Sempre execute `terraform fmt` e `terraform validate` após fazer alterações
- Use a ferramenta `azure_get_deployment_best_practices` para orientação sobre deployment
- Use a ferramenta `microsoft.docs.mcp` para consultar orientações específicas do serviço Azure

## Instruções personalizadas para GitHub Copilot Agents

**IMPORTANTE**: Quando o GitHub Copilot Agent ou GitHub Copilot Coding Agent está trabalhando neste repositório, os seguintes testes unitários locais DEVEM ser executados para atender aos requisitos das verificações de PR. A falha em executar esses testes causará falhas de validação de PR:

```bash
./avm pre-commit
./avm tflint
./avm pr-check
```

Esses comandos devem ser executados antes de qualquer pull request ser criado ou atualizado para garantir conformidade com os padrões do Azure Verified Modules e evitar falhas no pipeline CI/CD.
Mais detalhes sobre o processo AVM podem ser encontrados na [documentação de contribuição do Azure Verified Modules](https://azure.github.io/Azure-Verified-Modules/contributing/terraform/testing/).