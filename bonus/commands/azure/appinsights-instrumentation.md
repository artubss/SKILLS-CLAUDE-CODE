---
allowed-tools: Bash, Read
description: Instrumentar uma webapp para enviar dados de telemetria úteis para o Azure App Insights
---

# Instrumentação do AppInsights

Esta skill permite enviar dados de telemetria de uma webapp para o Azure App Insights para melhor observabilidade da saúde da aplicação.

## Quando usar esta skill

Use esta skill quando o usuário desejar habilitar telemetria para sua webapp.

## Pré-requisitos

A aplicação no workspace deve ser um destes tipos

- Uma aplicação ASP.NET Core hospedada no Azure
- Uma aplicação Node.js hospedada no Azure

## Diretrizes

### Coletar informações de contexto

Descubra a tupla (linguagem de programação, framework de aplicação, hospedagem) da aplicação à qual o usuário está tentando adicionar suporte de telemetria. Isso determina como a aplicação pode ser instrumentada. Leia o código-fonte para fazer uma estimativa bem informada. Confirme com o usuário qualquer coisa que você não souber. Você deve sempre perguntar ao usuário onde a aplicação está hospedada (por exemplo, em um computador pessoal, em um Azure App Service como código, em um Azure App Service como contêiner, em um Azure Container App, etc.).

### Prefira auto-instrumentação quando possível

Se a aplicação for uma aplicação C# ASP.NET Core hospedada no Azure App Service, use o [guia AUTO](references/AUTO.md) para ajudar o usuário a auto-instrumentar a aplicação.

### Instrumentar manualmente

Instrumente a aplicação manualmente criando o recurso AppInsights e atualizando o código da aplicação.

#### Criar recurso AppInsights

Use uma das seguintes opções que se adequa ao ambiente.

- Adicione AppInsights a um template Bicep existente. Veja [examples/appinsights.bicep](examples/appinsights.bicep) para o que adicionar. Esta é a melhor opção se houver arquivos de template Bicep existentes no workspace.
- Use Azure CLI. Veja [scripts/appinsights.ps1](scripts/appinsights.ps1) para o comando Azure CLI a executar para criar o recurso App Insights.

Não importa qual opção você escolha, recomende ao usuário criar o recurso App Insights em um grupo de recursos significativo que facilite o gerenciamento de recursos. Um bom candidato será o mesmo grupo de recursos que contém os recursos para a aplicação hospedada no Azure.

#### Modificar código da aplicação

- Se a aplicação for uma aplicação ASP.NET Core, veja o [guia ASPNETCORE](references/ASPNETCORE.md) para saber como modificar o código C#.
- Se a aplicação for uma aplicação Node.js, veja o [guia NODEJS](references/NODEJS.md) para saber como modificar o código JavaScript/TypeScript.
- Se a aplicação for uma aplicação Python, veja o [guia PYTHON](references/PYTHON.md) para saber como modificar o código Python.