---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [coverage-type] | --line | --branch | --function | --statement | --report
description: Analise e melhore a cobertura de testes com relatórios abrangentes e identificação de lacunas
---

# Cobertura de Testes

Analise e melhore a cobertura de testes com relatórios detalhados e análise de lacunas: **$ARGUMENTS**

## Contexto de Cobertura Atual

- Framework de teste: !`find . -name "jest.config.*" -o -name ".nycrc*" -o -name "coverage.xml" | head -1 || echo "Detectar framework"`
- Ferramentas de cobertura: !`npm ls nyc jest @jest/core 2>/dev/null | grep -E "nyc|jest" | head -2 || echo "Sem ferramentas de cobertura JS"`
- Cobertura existente: !`find . -name "coverage" -type d | head -1 && echo "Dados de cobertura existem" || echo "Sem dados de cobertura"`
- Arquivos de teste: !`find . -name "*.test.*" -o -name "*.spec.*" | wc -l` arquivos de teste

## Tarefa

Execute análise abrangente de cobertura com recomendações de melhoria e relatórios:

**Tipo de Cobertura**: Use $ARGUMENTS para focar em cobertura de linhas, cobertura de branches, cobertura de funções, cobertura de statements ou relatório abrangente

**Framework de Análise de Cobertura**:

1. **Configuração da Ferramenta de Cobertura** - Configure ferramentas apropriadas (Jest, NYC, Istanbul, Coverage.py, JaCoCo), configure coleta de dados, otimize desempenho, habilite relatórios
2. **Medição de Cobertura** - Gere relatórios de cobertura de linhas, cobertura de branches, cobertura de funções, cobertura de statements, identifique caminhos de código não cobertos
3. **Análise de Lacunas** - Identifique caminhos críticos não cobertos, analise qualidade de cobertura, avalie cobertura de lógica de negócio, avalie tratamento de casos extremos
4. **Gerenciamento de Limites** - Configure limites de cobertura, implemente quality gates, configure monitoramento de tendências, enforce padrões mínimos
5. **Relatórios e Visualização** - Gere relatórios detalhados, crie dashboards de cobertura, implemente análise de tendências, configure notificações automatizadas
6. **Planejamento de Melhoria** - Priorize lacunas de cobertura, recomende adições de testes, identifique oportunidades de refatoração, planeje aprimoramento de cobertura

**Recursos Avançados**: Análise de cobertura diferencial, monitoramento de tendências de cobertura, integração com revisão de código, alertas automatizados de cobertura, avaliação de impacto de desempenho.

**Insights de Qualidade**: Avaliação de qualidade de cobertura, análise de efetividade de testes, correlação com manutenibilidade, identificação de áreas de risco.

**Output**: Análise abrangente de cobertura com relatórios detalhados, identificação de lacunas, recomendações de melhoria e rastreamento de métricas de qualidade.