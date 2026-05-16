---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [scope] | --unit | --integration | --e2e | --visual | --performance | --full-stack
description: Configure infraestrutura completa de testes com configuração de framework e integração com CI
---

# Configurar Testes Abrangentes

Configure infraestrutura completa de testes com estratégia de testes em múltiplas camadas: **$ARGUMENTS**

## Infraestrutura de Testes Atual

- Tipo de projeto: !`[ -f package.json ] && echo "Node.js" || [ -f requirements.txt ] && echo "Python" || [ -f pom.xml ] && echo "Java" || echo "Multilíngue"`
- Testes existentes: !`find . -name "*.test.*" -o -name "*.spec.*" | wc -l` arquivos de teste
- Sistema de CI: !`find . -name ".github" -o -name ".gitlab-ci.yml" -o -name "Jenkinsfile" | head -1 || echo "Nenhum CI detectado"`
- Framework: !`grep -l "jest\\|vitest\\|pytest\\|junit" package.json requirements.txt pom.xml 2>/dev/null | head -1 || echo "Detectar framework"`

## Tarefa

Implementar infraestrutura completa de testes com estratégia de testes em múltiplas camadas:

**Escopo de Configuração**: Use $ARGUMENTS para focar em testes unitários, integração, e2e, visuais, performance ou implementação full-stack

**Framework Abrangente de Testes**:

1. **Design de Estratégia de Testes** - Analisar requisitos do projeto, definir pirâmide de testes, planejar metas de cobertura, otimizar investimento em testes
2. **Configuração de Testes Unitários** - Configurar framework primário (Jest, Vitest, pytest), setup de test runners, implementar utilitários de teste, otimizar execução
3. **Testes de Integração** - Setup de framework de teste de integração, configurar bancos de dados de teste, implementar testes de API, otimizar isolamento de testes
4. **Configuração de Testes E2E** - Setup de testes no navegador (Cypress, Playwright), configurar ambientes de teste, implementar page objects
5. **Testes Visuais e de Performance** - Setup de testes de regressão visual, configurar benchmarks de performance, implementar testes de acessibilidade
6. **Integração com CI/CD** - Configurar execução automatizada de testes, setup de testes em paralelo, implementar quality gates, otimizar performance do pipeline

**Recursos Avançados**: Testes de contrato, chaos engineering, teste de carga, testes de segurança, testes cross-browser, testes mobile.

**Qualidade da Infraestrutura**: Confiabilidade de testes, performance de execução, manutenibilidade, escalabilidade, otimização de custos.

**Saída**: Infraestrutura completa de testes com frameworks configurados, integração com CI, métricas de qualidade e workflows de manutenção.