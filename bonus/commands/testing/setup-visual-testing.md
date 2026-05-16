---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [testing-scope] | --components | --pages | --responsive | --cross-browser | --accessibility
description: Configurar testes de regressão visual abrangentes com testes responsivos e cross-browser
---

# Configurar Testes Visuais

Configurar testes de regressão visual abrangentes com validação responsiva e de acessibilidade: **$ARGUMENTS**

## Contexto Atual de Testes Visuais

- Framework frontend: !`grep -l "react\\|vue\\|angular" package.json 2>/dev/null || echo "Detect framework"`
- Componentes UI: !`find . -name "components" -o -name "src" | head -1 && echo "Component structure detected" || echo "Analyze structure"`
- Testes existentes: !`find . -name "cypress" -o -name "playwright" -o -name "storybook" | head -1 || echo "No visual testing"`
- Sistema CI: !`find . -name ".github" -o -name ".gitlab-ci.yml" | head -1 || echo "No CI detected"`

## Tarefa

Implementar testes visuais abrangentes com detecção de regressão e validação de acessibilidade:

**Escopo de Testes**: Use $ARGUMENTS para focar em testes de componentes, testes de página, testes responsivos, testes cross-browser ou testes de acessibilidade

**Framework de Testes Visuais**:

1. **Seleção e Configuração de Ferramentas** - Escolher ferramentas de testes visuais (Percy, Chromatic, BackstopJS, Playwright), configurar integração, configurar ambientes
2. **Criação de Baseline** - Capturar baselines visuais, organizar estrutura de screenshots, implementar controle de versão, otimizar gerenciamento de imagens
3. **Design de Cenários de Teste** - Criar testes de componentes, desenhar fluxos de página, implementar breakpoints responsivos, configurar matriz de browsers
4. **Configuração de Integração** - Configurar integração CI/CD, configurar execução automatizada, implementar workflows de revisão, otimizar performance
5. **Detecção de Regressão** - Configurar algoritmos de diff, gerenciar limites, implementar workflows de aprovação, otimizar precisão
6. **Testes Avançados** - Configurar testes de acessibilidade, validação cross-browser, testes responsivos, monitoramento de performance visual

**Recursos Avançados**: Testes visuais automatizados, análise de diff inteligente, verificação de conformidade de acessibilidade, validação de design responsivo, métricas de performance visual.

**Garantia de Qualidade**: Confiabilidade de testes, redução de falsos positivos, otimização de manutenibilidade, performance de execução.

**Saída**: Setup completo de testes visuais com gerenciamento de baseline, detecção de regressão, integração CI e workflows de validação abrangentes.