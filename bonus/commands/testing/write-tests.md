---
allowed-tools: Ler, Escrever, Editar, Bash
argument-hint: [arquivo-alvo] | [tipo-teste] | --unit | --integration | --e2e | --component
description: Escrever testes abrangentes de unidade e integração com mocking apropriado e cobertura
---

# Escrever Testes

Escrever testes abrangentes de unidade e integração com práticas recomendadas específicas do framework: **$ARGUMENTS**

## Contexto Atual de Testes

- Framework de testes: !`find . -name "jest.config.*" -o -name "*.test.*" | head -1 && echo "Jest/Vitest detectado" || echo "Detectar framework"`
- Arquivo alvo: Análise de $ARGUMENTS para requisitos e complexidade de testes
- Padrões do projeto: !`find . -name "*.test.*" -o -name "*.spec.*" | head -3` padrões de testes existentes
- Configuração de cobertura: !`grep -l "coverage" package.json jest.config.* 2>/dev/null | head -1 || echo "Setup necessário"`

## Tarefa

Executar escrita abrangente de testes com otimizações específicas do framework e práticas recomendadas:

**Foco de Testes**: Use $ARGUMENTS para especificar arquivo alvo, testes de unidade, testes de integração, testes e2e ou testes de componente

**Framework de Escrita de Testes**:

1. **Análise de Código** - Analisar estrutura do código alvo, identificar funções testáveis, avaliar complexidade de dependências, analisar casos extremos
2. **Design de Estratégia de Testes** - Planejar organização de testes, desenhar hierarquias de testes, identificar requisitos de mock, otimizar isolamento de testes
3. **Integração do Framework** - Configurar padrões específicos do framework, configurar utilitários de teste, implementar assertions apropriadas, otimizar desempenho de testes
4. **Implementação de Mocks** - Desenhar mocks de dependências, implementar test doubles, criar factory functions, configurar manipulação assíncrona
5. **Geração de Casos de Teste** - Escrever testes de unidade, testes de integração, casos extremos, cenários de erro, testes de performance, testes de snapshot
6. **Garantia de Qualidade** - Garantir manutenibilidade de testes, otimizar velocidade de execução, validar cobertura, implementar cleanup apropriado

**Recursos Avançados**: Testes baseados em propriedades, testes de contrato, testes de regressão visual, testes de acessibilidade, benchmarking de performance.

**Suporte a Frameworks**: Jest/Vitest, React Testing Library, Vue Test Utils, Angular TestBed, Cypress, integração Playwright.

**Output**: Suite de testes abrangente com testes de unidade, testes de integração, mocking apropriado, utilitários de teste e otimização de cobertura.