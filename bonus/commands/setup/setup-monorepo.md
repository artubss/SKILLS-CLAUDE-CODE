---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [monorepo-tool] | --nx | --lerna | --rush | --turborepo | --yarn-workspaces
description: Configure estrutura de projeto monorepo com gerenciamento abrangente de workspace e orquestração de build
---

# Configurar Monorepo

Configure estrutura monorepo abrangente com gerenciamento avançado de workspace: **$ARGUMENTS**

## Estado Atual do Projeto

- Estrutura do repositório: !`find . -maxdepth 2 -type d | head -10`
- Gerenciador de pacotes: @package.json ou configuração de workspace existente
- Monorepo existente: @nx.json ou @lerna.json ou @rush.json ou @turbo.json
- Contagem de projetos: !`find . -name "package.json" -not -path "./node_modules/*" | wc -l`

## Tarefa

Implemente monorepo pronto para produção com gerenciamento avançado de workspace e orquestração de build:

**Ferramenta Monorepo**: Use $ARGUMENTS para configurar Nx, Lerna, Rush, Turborepo ou Yarn Workspaces

**Arquitetura Monorepo**:
1. **Estrutura de Workspace** - Organização de diretórios, arquitetura de pacotes, bibliotecas compartilhadas, separação de aplicações
2. **Gerenciamento de Dependências** - Dependências de workspace, gerenciamento de versão, hoisting de pacotes, resolução de conflitos
3. **Orquestração de Build** - Dependências de tarefas, builds paralelos, compilação incremental, detecção de pacotes afetados
4. **Fluxo de Desenvolvimento** - Hot reloading, depuração, estratégias de teste, coordenação de servidor de desenvolvimento
5. **Integração CI/CD** - Pipelines de build, detecção de projetos afetados, orquestração de deploy, gerenciamento de artefatos
6. **Configuração de Ferramentas** - Configurações compartilhadas, ferramentas de qualidade de código, frameworks de teste, documentação

**Recursos Avançados**: Cache de tarefas, execução distribuída, otimização de desempenho, integração do ecossistema de plugins.

**Produtividade do Time**: Otimização da experiência de desenvolvedor, automação de onboarding, procedimentos de manutenção.

**Output**: Setup monorepo completo com sistema de build otimizado, tooling abrangente e melhorias de produtividade do time.