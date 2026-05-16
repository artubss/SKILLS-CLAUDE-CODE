---
name: dotnet-upgrade
description: Realizar tarefas de manutenção em código C#/.NET incluindo limpeza, modernização e remediação de débito técnico.
tools: codebase, edit/editFiles, search, runCommands, runTasks, runTests, problems, changes, usages, findTestFiles, testFailure, terminalLastCommand, terminalSelection, fetch, microsoft.docs.mcp
---

# Coleção de Upgrade .NET

Especialista em upgrade de .NET Framework para migração abrangente de projetos

**Tags:** dotnet, upgrade, migration, framework, modernization

## Uso da Coleção

### Modo Chat .NET Upgrade

Descubra e planeje sua jornada de upgrade .NET!

```markdown, upgrade-analysis.prompt.md
---
mode: dotnet-upgrade
title: Analisar versões atuais do framework .NET e criar plano de upgrade
---
Analise o repositório e liste o TargetFramework atual de cada projeto 
juntamente com a versão LTS mais recente disponível no cronograma de lançamentos da Microsoft.
Crie uma estratégia de upgrade priorizando projetos menos dependentes primeiro.
```

O modo chat de upgrade se adapta automaticamente à versão .NET atual do seu repositório e fornece orientações de upgrade contextualizadas para a próxima versão estável.

Ele ajudará você a:
- Detectar automaticamente versões .NET em todos os projetos
- Gerar sequências de upgrade ideais
- Identificar mudanças significativas e oportunidades de modernização
- Criar fluxos de upgrade por projeto

---

### Instruções de Upgrade .NET

Execute upgrades abrangentes de framework .NET com orientação estruturada!

As instruções fornecem:
- Estratégias de upgrade sequenciais
- Análise de dependências e sequenciamento
- Direcionamento de framework e ajustes de código
- Gerenciamento de NuGet e dependências
- Atualizações de pipeline CI/CD
- Procedimentos de teste e validação

Use estas instruções ao implementar planos de upgrade para garantir execução e validação adequadas.

---

### Prompts de Upgrade .NET

Acesso rápido a prompts de análise de upgrade especializados!

A coleção de prompts inclui consultas prontas para:
- Descoberta e avaliação de projetos
- Estratégia de upgrade e sequenciamento
- Direcionamento de framework e ajustes de código
- Análise de mudanças significativas
- Atualizações de pipeline CI/CD
- Validação final e entrega

Use estes prompts para análise direcionada de aspectos específicos do upgrade.

---

## Início Rápido
1. Execute uma passagem de descoberta para enumerar todos os arquivos `*.sln` e `*.csproj` no repositório.
2. Detecte a(s) versão(ões) .NET atual(is) usada(s) em todos os projetos.
3. Identifique a versão .NET estável mais recente disponível (LTS preferida) — geralmente `+2` anos à frente da versão existente.
4. Gere um plano de upgrade para migrar de atual → próxima versão estável (ex: `net6.0 → net8.0`, ou `net7.0 → net9.0`).
5. Atualize um projeto por vez, valide builds, atualize testes e modifique o CI/CD accordingly.

---

## Detectar Automaticamente Versão .NET Atual
Para detectar automaticamente as versões de framework atuais em toda a solução:

```bash
# 1. Verificar SDKs globais instalados
dotnet --list-sdks

# 2. Detectar TargetFrameworks no nível de projeto
find . -name "*.csproj" -exec grep -H "<TargetFramework" {} \;

# 3. Opcional: resumir versões de framework únicas
grep -r "<TargetFramework" **/*.csproj | sed 's/.*<TargetFramework>//;s/<\/TargetFramework>//' | sort | uniq

# 4. Verificar ambiente de runtime
dotnet --info | grep "Version"
```

**Prompt de Chat:**
> "Analise o repositório e liste o TargetFramework atual de cada projeto juntamente com a versão LTS mais recente disponível no cronograma de lançamentos da Microsoft."

---

## Comandos de Descoberta & Análise
```bash
# Listar todos os projetos
dotnet sln list

# Verificar frameworks alvo atuais para cada projeto
grep -H "TargetFramework" **/*.csproj

# Verificar pacotes desatualizados
dotnet list <ProjectName>.csproj package --outdated

# Gerar grafo de dependências
dotnet msbuild <ProjectName>.csproj /t:GenerateRestoreGraphFile /p:RestoreGraphOutputPath=graph.json
```

**Prompt de Chat:**
> "Analise a solução e resuma o TargetFramework atual de cada projeto e sugira a versão LTS de upgrade apropriada."

---

## Regras de Classificação
- `TargetFramework` começa com `netcoreapp`, `net5.0+`, `net6.0+`, etc. → **.NET Moderno**
- `netstandard*` → **.NET Standard** (migrar para versão .NET atual)
- `net4*` → **.NET Framework** (migrar via etapa intermediária para .NET 6+)

---

## Sequência de Upgrade
1. **Comece com Bibliotecas Independentes:** Bibliotecas de classes menos dependentes primeiro.
2. **Próximo:** Componentes compartilhados e utilitários comuns.
3. **Depois:** Projetos de API, Web ou Function.
4. **Finalmente:** Testes, pontos de integração e pipelines.

**Prompt de Chat:**
> "Gere a ordem de upgrade ideal para este repositório, priorizando projetos menos dependentes primeiro."

---

## Fluxo de Upgrade Por Projeto
1. **Criar branch:** `upgrade/<project>-to-<targetVersion>`
2. **Editar `<TargetFramework>`** no `.csproj` para a versão sugerida (ex: `net9.0`)
3. **Restaurar & atualizar pacotes:**
   ```bash
   dotnet restore
   dotnet list package --outdated
   dotnet add package <PackageName> --version <LatestVersion>
   ```
4. **Build & teste:**
   ```bash
   dotnet build <ProjectName>.csproj
   dotnet test <ProjectName>.Tests.csproj
   ```
5. **Corrigir problemas** — resolver APIs descontinuadas, ajustar configurações, modernizar JSON/logging/DI.
6. **Commit & push** PR com evidência de teste e checklist.

---

## Mudanças Significativas & Modernização
- Use `.NET Upgrade Assistant` para recomendações iniciais.
- Aplique analisadores para detectar APIs obsoletas.
- Substitua SDKs desatualizados (ex: `Microsoft.Azure.*` → `Azure.*`).
- Modernize lógica de inicialização (`Startup.cs` → `Program.cs` com instruções top-level).

**Prompt de Chat:**
> "Liste APIs descontinuadas ou incompatíveis ao fazer upgrade de <currentVersion> para <targetVersion> em <ProjectName>."

---

## Atualizações de Configuração CI/CD
Certifique-se de que os pipelines usem a **versão alvo** detectada dinamicamente:

**Azure DevOps**
```yaml
- task: UseDotNet@2
  inputs:
    packageType: 'sdk'
    version: '$(TargetDotNetVersion).x'
```

**GitHub Actions**
```yaml
- uses: actions/setup-dotnet@v4
  with:
    dotnet-version: '${{ env.TargetDotNetVersion }}.x'
```

---

## Checklist de Validação
- [ ] TargetFramework atualizado para próxima versão estável
- [ ] Todos os pacotes NuGet compatíveis e atualizados
- [ ] Build e pipelines de teste bem-sucedidos localmente e no CI
- [ ] Testes de integração passam
- [ ] Deployado em ambiente inferior e verificado

---

## Estratégia de Branching & Rollback
- Use feature branches: `upgrade/<project>-to-<targetVersion>`
- Faça commits frequentes e mantenha mudanças atômicas
- Se o CI falhar após merge, reverta a PR e isole módulos com falha

**Prompt de Chat:**
> "Sugira um plano de rollback e validação se o upgrade de .NET em <ProjectName> introduzir regressões de build ou runtime."

---

## Automação & Dimensionamento
- Automatize detecção de upgrade com GitHub Actions ou Azure Pipelines.
- Agende execuções noturnas para verificar novas versões .NET via `dotnet --list-sdks`.
- Use agentes para automaticamente elevar PRs para frameworks desatualizados.

---

## Biblioteca de Prompts de Chatmode
1. "Liste todos os projetos com versões .NET atuais e recomendadas."
2. "Gere um plano de upgrade por projeto de <currentVersion> para <targetVersion>."
3. "Sugira edições de .csproj e pipeline para fazer upgrade de <ProjectName>."
4. "Resuma resultados de build/teste pós-upgrade para <ProjectName>."
5. "Crie descrição de PR e checklist para o upgrade."

---