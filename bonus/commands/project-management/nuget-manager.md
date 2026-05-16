---
allowed-tools: Bash, Read
description: Gerenciar pacotes NuGet em projetos/soluções .NET. Use essa skill ao adicionar, remover ou atualizar versões de pacotes NuGet. Ela reforça o uso da CLI `dotnet` para gerenciamento de pacotes e fornece procedimentos rigorosos para edições diretas de arquivos apenas ao atualizar versões.
---

# Gerenciador NuGet

## Visão Geral

Essa skill garante gerenciamento consistente e seguro de pacotes NuGet em projetos .NET. Ela prioriza o uso da CLI `dotnet` para manter a integridade do projeto e reforça um workflow rigoroso de verificação e restauração para atualizações de versão.

## Pré-requisitos

- SDK do .NET instalado (tipicamente .NET 8.0 SDK ou superior, ou uma versão compatível com a solução alvo).
- CLI `dotnet` disponível em seu `PATH`.
- `jq` (processador JSON) OU PowerShell (para verificação de versão usando `dotnet package search`).

## Regras Essenciais

1.  **NUNCA** edite diretamente arquivos `.csproj`, `.props` ou `Directory.Packages.props` para **adicionar** ou **remover** pacotes. Sempre use os comandos `dotnet add package` e `dotnet remove package`.
2.  **EDIÇÃO DIRETA** é PERMITIDA APENAS para **alterar versões** de pacotes existentes.
3.  **ATUALIZAÇÕES DE VERSÃO** devem seguir o workflow obrigatório:
    - Verificar se a versão alvo existe no NuGet.
    - Determinar se as versões são gerenciadas por projeto (`.csproj`) ou centralmente (`Directory.Packages.props`).
    - Atualizar a string de versão no arquivo apropriado.
    - Executar imediatamente `dotnet restore` para verificar compatibilidade.

## Workflows

### Adicionar um Pacote
Use `dotnet add [<PROJECT>] package <PACKAGE_NAME> [--version <VERSION>]`.
Exemplo: `dotnet add src/MyProject/MyProject.csproj package Newtonsoft.Json`

### Remover um Pacote
Use `dotnet remove [<PROJECT>] package <PACKAGE_NAME>`.
Exemplo: `dotnet remove src/MyProject/MyProject.csproj package Newtonsoft.Json`

### Atualizar Versões de Pacotes
Ao atualizar uma versão, siga estas etapas:

1.  **Verificar Existência da Versão**:
    Verifique se a versão existe usando o comando `dotnet package search` com correspondência exata e formatação JSON.
    Usando `jq`:
    `dotnet package search <PACKAGE_NAME> --exact-match --format json | jq -e '.searchResult[].packages[] | select(.version == "<VERSION>")'`
    Usando PowerShell:
    `(dotnet package search <PACKAGE_NAME> --exact-match --format json | ConvertFrom-Json).searchResult.packages | Where-Object { $_.version -eq "<VERSION>" }`
    
2.  **Determinar Gerenciamento de Versão**:
    - Procure por `Directory.Packages.props` na raiz da solução. Se presente, as versões devem ser gerenciadas lá via `<PackageVersion Include="Package.Name" Version="1.2.3" />`.
    - Se ausente, verifique arquivos `.csproj` individuais para `<PackageReference Include="Package.Name" Version="1.2.3" />`.

3.  **Aplicar Alterações**:
    Modifique o arquivo identificado com a nova string de versão.

4.  **Verificar Estabilidade**:
    Execute `dotnet restore` no projeto ou solução. Se ocorrerem erros, reverta a alteração e investigue.

## Exemplos

### Usuário: "Adicionar Serilog ao projeto WebApi"
**Ação**: Execute `dotnet add src/WebApi/WebApi.csproj package Serilog`.

### Usuário: "Atualizar Newtonsoft.Json para 13.0.3 em toda a solução"
**Ação**:
1. Verificar se 13.0.3 existe: `dotnet package search Newtonsoft.Json --exact-match --format json` (e analisar a saída para confirmar que "13.0.3" está presente).
2. Encontrar onde está definido (ex: `Directory.Packages.props`).
3. Editar o arquivo para atualizar a versão.
4. Executar `dotnet restore`.