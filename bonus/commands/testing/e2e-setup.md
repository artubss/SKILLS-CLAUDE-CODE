---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [framework] | --cypress | --playwright | --webdriver | --puppeteer | --mobile
description: Configure suíte abrangente de testes end-to-end com seleção de framework e integração com CI
---

# Configuração E2E

Configure suíte abrangente de testes end-to-end com otimização de framework: **$ARGUMENTS**

## Contexto Atual de E2E

- Tipo de aplicação: !`find . -name "index.html" -o -name "app.js" -o -name "App.tsx" | head -1 && echo "Aplicativo web" || echo "Detectar tipo de aplicativo"`
- Framework: !`grep -l "react\\|vue\\|angular" package.json 2>/dev/null || echo "Detectar framework"`
- Testes existentes: !`find . -name "cypress" -o -name "playwright" -o -name "e2e" | head -1 || echo "Sem configuração E2E"`
- Sistema de CI: !`find . -name ".github" -o -name ".gitlab-ci.yml" | head -1 || echo "Nenhuma CI detectada"`

## Tarefa

Implementar testes end-to-end abrangentes com seleção de framework e otimização:

**Foco do Framework**: Use $ARGUMENTS para especificar Cypress, Playwright, WebDriver, Puppeteer, testes mobile ou detectar melhor opção automaticamente

**Framework de Testes E2E**:

1. **Seleção e Configuração do Framework** - Escolher ferramenta E2E ideal, instalar dependências, configurar ajustes básicos, estruturar projeto
2. **Configuração do Ambiente de Teste** - Configurar ambientes de teste, configurar URLs base, implementar alternância de ambiente, otimizar isolamento de testes
3. **Padrões de Page Object** - Projetar modelo de page object, criar componentes reutilizáveis, implementar seletores de elemento, otimizar manutenibilidade
4. **Gerenciamento de Dados de Teste** - Configurar estratégias de dados de teste, implementar fixtures, configurar seeding de banco de dados, projetar procedimentos de limpeza
5. **Testes Cross-Browser** - Configurar execução em múltiplos browsers, configurar testes mobile, implementar testes responsivos, otimizar compatibilidade
6. **Integração com CI/CD** - Configurar execução automatizada, configurar testes paralelos, implementar relatórios, otimizar performance

**Recursos Avançados**: Testes de regressão visual, testes de acessibilidade, monitoramento de performance, integração com testes de API, testes em dispositivos móveis.

**Garantia de Qualidade**: Otimização da confiabilidade de testes, prevenção de testes instáveis, otimização de velocidade de execução, capacidades de debug.

**Output**: Configuração E2E completa com configuração de framework, suítes de teste, integração com CI e workflows de manutenção.