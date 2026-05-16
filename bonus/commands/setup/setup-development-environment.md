---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [environment-type] | --local | --docker | --cloud | --full-stack
description: Configura ambiente de desenvolvimento abrangente com ferramentas, configurações e workflows
---

# Configurar Ambiente de Desenvolvimento

Configura ambiente de desenvolvimento abrangente com ferramentas modernas: **$ARGUMENTS**

## Estado Atual do Ambiente

- Sistema operacional: !`uname -s` e detecção de arquitetura
- Ferramentas de desenvolvimento: !`node --version 2>/dev/null || python --version 2>/dev/null || echo "No runtime detected"`
- Gerenciadores de pacotes: !`which npm yarn pnpm pip poetry cargo 2>/dev/null | wc -l` gerenciadores disponíveis
- IDE/Editor: Verificar VS Code, IntelliJ ou outros ambientes de desenvolvimento

## Tarefa

Configurar ambiente de desenvolvimento completo com ferramentas e melhores práticas modernas:

**Tipo de Ambiente**: Use $ARGUMENTS para especificar setup local, baseado em Docker, ambiente em nuvem ou desenvolvimento full-stack

**Configuração do Ambiente**:
1. **Instalação de Runtime** - Linguagens de programação, gerenciadores de pacotes, gerenciadores de versão (nvm, pyenv, rustup)
2. **Ferramentas de Desenvolvimento** - Configuração de IDE, extensões, debuggers, profilers, clientes de banco de dados
3. **Sistema de Build** - Compiladores, bundlers, task runners, ferramentas CI/CD, frameworks de teste
4. **Qualidade de Código** - Linting, formatação, hooks de pré-commit, ferramentas de análise de código
5. **Configuração do Ambiente** - Variáveis de ambiente, gerenciamento de secrets, arquivos de configuração
6. **Sincronização de Equipe** - Configurações compartilhadas, documentação, guias de onboarding

**Funcionalidades Avançadas**: Hot reloading, configuração de debugging, monitoramento de performance, orquestração de containers.

**Automação**: Scripts de setup automatizados, gerenciamento de configuração, sincronização de ambiente de equipe.

**Saída**: Ambiente de desenvolvimento completo com processo de setup documentado, configurações de equipe e guias de resolução de problemas.