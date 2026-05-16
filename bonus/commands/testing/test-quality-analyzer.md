---
allowed-tools: Ler, Escrever, Editar, Bash
argument-hint: [tipo-analise] | --qualidade-cobertura | --efetividade-testes | --manutencibilidade | --analise-desempenho
description: Analisar qualidade do conjunto de testes com métricas abrangentes e recomendações de melhoria
---

# Analisador de Qualidade de Testes

Analise a qualidade do conjunto de testes com métricas abrangentes e insights de melhoria acionáveis: **$ARGUMENTS**

## Contexto de Qualidade Atual

- Cobertura de testes: !`find . -name "coverage" -type d | head -1 && echo "Dados de cobertura disponíveis" || echo "Sem dados de cobertura"`
- Arquivos de teste: !`find . -name "*.test.*" -o -name "*.spec.*" | wc -l` arquivos de teste
- Complexidade de testes: Análise de padrões de manutenibilidade e efetividade do conjunto de testes
- Métricas de desempenho: Tempos de execução atuais de testes e utilização de recursos

## Tarefa

Execute análise abrangente de qualidade de testes com recomendações de melhoria e estratégias de otimização:

**Tipo de Análise**: Use $ARGUMENTS para focar em qualidade de cobertura, efetividade de testes, análise de manutenibilidade ou análise de desempenho

**Framework de Análise de Qualidade de Testes**:

1. **Avaliação de Qualidade de Cobertura** - Analisar profundidade de cobertura, avaliar qualidade de cobertura, avaliar tratamento de casos extremos, identificar lacunas de cobertura
2. **Avaliação de Efetividade de Testes** - Medir capacidade de detecção de defeitos, analisar confiabilidade de testes, avaliar qualidade de asserções, avaliar valor de testes
3. **Análise de Manutenibilidade** - Avaliar qualidade do código de teste, analisar organização de testes, avaliar necessidades de refatoração, otimizar estrutura de testes
4. **Avaliação de Desempenho** - Analisar desempenho de execução, identificar gargalos, otimizar velocidade de testes, reduzir consumo de recursos
5. **Detecção de Anti-Padrões** - Identificar anti-padrões de testes, detectar testes instáveis, analisar code smells de testes, recomendar correções
6. **Rastreamento de Métricas de Qualidade** - Implementar pontuação de qualidade, rastrear tendências de melhoria, configurar gates de qualidade, otimizar processos de qualidade

**Funcionalidades Avançadas**: Avaliação de qualidade alimentada por IA, modelagem preditiva de qualidade, sugestões de melhoria automatizadas, análise de tendências de qualidade, comparação com benchmarks.

**Insights de Qualidade**: Análise de ROI de testes, análise de correlação de qualidade, avaliação de custo de manutenção, benchmarking de efetividade.

**Saída**: Análise abrangente de qualidade com métricas detalhadas, recomendações de melhoria, estratégias de otimização e framework de rastreamento de qualidade.