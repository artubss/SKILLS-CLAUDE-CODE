---
allowed-tools: Read, Write, Edit, Bash, Glob
argument-hint: [tipo-projeto] [framework] | --react | --vue | --api | --cli
description: Inicializa novo projeto com estrutura essencial, configuração e ambiente de desenvolvimento pronto
---

# Inicializar Novo Projeto

Inicializa novo projeto com estrutura essencial: **$ARGUMENTS**

## Instruções

1. **Análise e Configuração do Projeto**
   - Analise o tipo de projeto e framework dos argumentos: `$ARGUMENTS`
   - Se nenhum argumento for fornecido, analise o diretório atual e solicite ao usuário o tipo de projeto e framework
   - Crie a estrutura de diretórios do projeto, se necessário
   - Valide que o framework escolhido é apropriado para o tipo de projeto

2. **Estrutura Base do Projeto**
   - Crie diretórios essenciais (src/, tests/, docs/, etc.)
   - Inicialize repositório git com .gitignore apropriado para o tipo de projeto
   - Crie README.md com descrição do projeto e instruções de configuração
   - Configure a estrutura de arquivos com base no tipo de projeto e framework

3. **Configuração Específica do Framework**
   - **Web/React**: Configure React com TypeScript, Vite/Next.js, ESLint, Prettier
   - **Web/Vue**: Configure Vue 3 com TypeScript, Vite, ESLint, Prettier
   - **Web/Angular**: Configure projeto Angular CLI com TypeScript e testes
   - **API/Express**: Crie servidor Express.js com TypeScript, middleware e roteamento
   - **API/FastAPI**: Configure FastAPI com Python, modelos Pydantic e suporte async
   - **Mobile/React Native**: Configure React Native com navegação e ferramentas de desenvolvimento
   - **Desktop/Electron**: Configure Electron com estrutura de processo renderer e main
   - **CLI/Node**: Crie CLI Node.js com commander.js e empacotamento apropriado
   - **Library/NPM**: Configure library com TypeScript, rollup/webpack e configuração de publicação

4. **Configuração do Ambiente de Desenvolvimento**
   - Configure gerenciador de pacotes (npm, yarn, pnpm) com package.json apropriado
   - Configure TypeScript com modo strict e path mapping
   - Configure linting com ESLint e regras específicas da linguagem
   - Configure formatação de código com Prettier e pre-commit hooks
   - Adicione EditorConfig para padronização de codificação

5. **Infraestrutura de Testes**
   - Instale e configure framework de testes (Jest, Vitest, Pytest, etc.)
   - Configure estrutura de diretório de testes e testes de exemplo
   - Configure relatório de cobertura de código
   - Adicione scripts de teste ao package.json/makefile

6. **Ferramentas de Build e Desenvolvimento**
   - Configure sistema de build (Vite, webpack, rollup, etc.)
   - Configure servidor de desenvolvimento com hot reloading
   - Configure gerenciamento de variáveis de ambiente
   - Adicione otimização de build e bundling

7. **Pipeline de CI/CD**
   - Crie workflow GitHub Actions para testes e deployment
   - Configure testes automatizados em pull requests
   - Configure atualização automática de dependências com Dependabot
   - Adicione badges de status ao README

8. **Documentação e Qualidade**
   - Gere README abrangente com instruções de instalação e uso
   - Crie CONTRIBUTING.md com diretrizes de desenvolvimento
   - Configure geração de documentação de API (JSDoc, Sphinx, etc.)
   - Adicione badges de qualidade de código e escudos

9. **Segurança e Boas Práticas**
   - Configure verificação de segurança com npm audit ou similar
   - Configure verificação de vulnerabilidades de dependências
   - Adicione headers de segurança para aplicações web
   - Configure definições de segurança específicas do ambiente

10. **Validação do Projeto**
    - Verifique se todas as dependências instalam corretamente
    - Execute build inicial para garantir que a configuração está funcionando
    - Execute suite de testes para validar configuração de testes
    - Verifique se regras de linting e formatação são aplicadas
    - Valide que o servidor de desenvolvimento inicia com sucesso
    - Crie commit inicial com estrutura apropriada do projeto