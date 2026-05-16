---
allowed-tools: Read, Write, Bash, Glob
argument-hint: [sprint-identifier] | --metrics | --insights | --action-items | --trends
description: Analisa retrospectivas de equipe com métricas quantitativas e geração de insights acionáveis
---

# Analisador de Retrospectivas

Analise retrospectivas de equipe com métricas abrangentes e insights de melhoria acionáveis: **$ARGUMENTS**

## Contexto Atual da Retrospectiva

- Período do sprint: !`git log --oneline --since='2 weeks ago' | wc -l` commits no sprint recente
- Atividade da equipe: Análise de padrões de colaboração recente e métricas de produtividade
- Sprint Linear: Dados do sprint atual e métricas de conclusão do Linear MCP
- Retrospectivas anteriores: Dados de retrospectivas históricas e rastreamento de melhorias

## Tarefa

Execute análise abrangente de retrospectiva com insights quantitativos e recomendações de melhoria:

**Foco da Análise**: Use $ARGUMENTS para especificar identificador do sprint, métricas quantitativas, geração de insights, rastreamento de itens de ação ou análise de tendências

**Framework de Análise de Retrospectiva**:
1. **Análise de Performance do Sprint** - Analise tendências de velocidade, taxas de conclusão, métricas de tempo de ciclo, indicadores de qualidade
2. **Avaliação de Colaboração da Equipe** - Avalie padrões de comunicação, efetividade de revisão de código, compartilhamento de conhecimento, impacto de programação em pares
3. **Efetividade do Processo** - Avalie eficiência de reuniões, acurácia de planejamento, resolução de impedimentos, otimização de fluxo de trabalho
4. **Métricas de Qualidade** - Analise taxas de bugs, acúmulo de dívida técnica, qualidade de revisão de código, efetividade de testes
5. **Contribuição Individual** - Avalie distribuição de carga de trabalho, desenvolvimento de habilidades, atividades de mentoria, progresso de treinamento cruzado
6. **Geração de Insights Acionáveis** - Identifique oportunidades de melhoria, priorize itens de ação, rastreie progresso, meça impacto

**Recursos Avançados**: Análise de tendências entre múltiplos sprints, modelagem preditiva de performance, correlação com satisfação da equipe, rastreamento de melhoria contínua.

**Qualidade de Insights**: Recomendações orientadas por dados, melhoria potencial quantificada, viabilidade de implementação, critérios de medição de sucesso.

**Output**: Análise abrangente de retrospectiva com métricas quantitativas, insights acionáveis, melhorias priorizadas e framework de rastreamento de progresso.