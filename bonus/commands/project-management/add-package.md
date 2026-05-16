---
allowed-tools: Read, Write, Edit, Bash, Glob
argument-hint: [nome-pacote] [tipo-pacote] | --library | --application | --tool
description: Adicionar e configurar novo pacote no workspace com estrutura e dependências apropriadas
---

# Adicionar Pacote ao Workspace

Adicionar e configurar novas dependências de projeto: **$ARGUMENTS**

## Instruções

1. **Definição e Análise do Pacote**
   - Analisar nome e tipo do pacote a partir dos argumentos: `$ARGUMENTS` (formato: nome [tipo])
   - Se nenhum argumento for fornecido, solicitar nome e tipo do pacote
   - Validar que o nome do pacote segue as convenções de nomenclatura do workspace
   - Determinar tipo do pacote: library, application, tool, shared, service, component-library
   - Verificar conflitos de nomenclatura com pacotes existentes

2. **Criação da Estrutura do Pacote**
   - Criar diretório do pacote no local apropriado do workspace (packages/, apps/, libs/)
   - Configurar estrutura de diretórios padrão baseada no tipo:
     - `src/` para código-fonte
     - `tests/` ou `__tests__/` para testes
     - `docs/` para documentação do pacote
     - `examples/` para exemplos de uso (se for library)
     - `public/` para assets estáticos (se for application)
   - Criar arquivos de configuração específicos do pacote

3. **Configuração do Pacote**
   - Gerar package.json com metadados apropriados:
     - Nome seguindo convenções do workspace
     - Versão alinhada com estratégia do workspace
     - Dependencies e devDependencies
     - Scripts para build, test, lint, dev
     - Configuração de entry points e exports
   - Configurar TypeScript (tsconfig.json) estendendo definições do workspace
   - Configurar regras de linting e formatação específicas do pacote

4. **Configuração Específica do Tipo de Pacote**
   - **Library**: Configurar sistema de build, definições de export, documentação de API
   - **Application**: Configurar roteamento, configuração de ambiente, otimização de build
   - **Tool**: Configurar setup de CLI, exports de binários, definições de comandos
   - **Shared**: Configurar utilitários comuns, definições de tipos, constantes compartilhadas
   - **Service**: Configurar setup de servidor, rotas de API, conexões de banco de dados
   - **Component Library**: Configurar Storybook, exports de componentes, sistema de styling

5. **Integração com o Workspace**
   - Registrar pacote na configuração do workspace (nx.json, lerna.json, etc.)
   - Configurar dependências do pacote e peer dependencies
   - Configurar imports cross-package e referências
   - Configurar ordem de build em todo o workspace e dependências
   - Adicionar pacote aos scripts do workspace e task runners

6. **Ambiente de Desenvolvimento**
   - Configurar servidor de desenvolvimento específico do pacote (se aplicável)
   - Configurar hot reloading e watch mode
   - Configurar debugging e source maps
   - Configurar proxy de desenvolvimento e API mocking (se necessário)
   - Configurar gerenciamento de variáveis de ambiente

7. **Infraestrutura de Testes**
   - Configurar framework de testes para o pacote
   - Criar arquivos de testes iniciais e exemplos
   - Configurar relatório de cobertura de testes
   - Configurar scripts de teste específicos do pacote
   - Configurar testes de integração com outros pacotes do workspace

8. **Build e Deployment**
   - Configurar sistema de build para o tipo de pacote
   - Configurar artefatos de build e diretórios de saída
   - Configurar bundling e otimização
   - Configurar publicação do pacote (se for library)
   - Configurar scripts de deployment (se for application)

9. **Documentação e Exemplos**
   - Criar README do pacote com instruções de instalação e uso
   - Configurar geração de documentação de API
   - Criar exemplos de uso e demos
   - Documentar arquitetura e decisões de design do pacote
   - Adicionar pacote à documentação do workspace

10. **Validação e Testes de Integração**
    - Verificar que o pacote compila com sucesso
    - Testar instalação do pacote e imports
    - Validar resolução de dependências do workspace
    - Testar workflow de desenvolvimento e hot reloading
    - Verificar que o pipeline de CI/CD inclui o novo pacote
    - Testar funcionalidade cross-package e integração