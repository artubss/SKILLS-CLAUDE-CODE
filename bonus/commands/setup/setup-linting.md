---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [language] | --javascript | --typescript | --python | --multi-language
description: Configure ferramentas abrangentes de linting de código e análise de qualidade com execução automatizada
---

# Configurar Linting de Código

Configure linting abrangente de código e análise de qualidade: **$ARGUMENTS**

## Estado Atual da Qualidade de Código

- Linguagens detectadas: !`find . -name "*.js" -o -name "*.ts" -o -name "*.py" -o -name "*.rs" | head -5`
- Linters existentes: @.eslintrc.* ou @pyproject.toml ou @tslint.json
- Gerenciador de pacotes: @package.json ou @requirements.txt ou @Cargo.toml
- Ferramentas de qualidade de código: !`which eslint flake8 pylint mypy clippy 2>/dev/null | wc -l`

## Tarefa

Configure um sistema abrangente de linting de código com análise de qualidade e execução automatizada:

**Foco de Linguagem**: Use $ARGUMENTS para configurar ESLint para JavaScript/TypeScript, linting Python, ou análise de qualidade multi-linguagem

**Configuração de Linting**:
1. **Instalação de Ferramentas** - ESLint, Flake8, Pylint, MyPy, Clippy, linters específicos de linguagem e plugins
2. **Configuração de Regras** - Regras de estilo de código, detecção de erros, boas práticas, padrões de segurança, diretrizes de desempenho
3. **Integração com IDE** - Linting em tempo real, destaque de erros, correções rápidas, configurações do workspace
4. **Portas de Qualidade** - Validação pré-commit, integração CI/CD, verificações em pull request, métricas de qualidade
5. **Regras Customizadas** - Padrões específicos do projeto, restrições arquiteturais, convenções do time
6. **Desempenho** - Linting incremental, estratégias de cache, execução paralela, otimização

**Funcionalidades Avançadas**: Linting de segurança, verificações de acessibilidade, análise de desempenho, análise de dependências, métricas de complexidade de código.

**Padrões do Time**: Configurações compartilhadas, guias de estilo, diretrizes de revisão, documentação de onboarding.

**Output**: Sistema completo de linting com portas de qualidade automatizadas, execução de padrões do time, e análise abrangente de código.