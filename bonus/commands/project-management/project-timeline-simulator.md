---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [tipo-projeto] | --duração | --tamanho-time | --nível-risco
description: Simule resultados de projetos com modelagem de variáveis, avaliação de riscos e otimização de recursos
---

# Simulador de Cronograma de Projeto

Simule resultados de projetos com modelagem abrangente de variáveis e avaliação de riscos: **$ARGUMENTS**

## Contexto Atual do Projeto

- Tipo de projeto: Baseado em $ARGUMENTS ou análise de codebase
- Capacidade do time: !`git shortlog -sn --since="90 days ago" | wc -l` contribuidores
- Dados de velocidade: !`git log --oneline --since="30 days ago" | wc -l` commits/mês
- Indicadores de risco: @RISKS.md ou documentação do projeto

## Tarefa

Gere simulações abrangentes de cronograma de projeto com múltiplos cenários:

**Framework de Simulação**:
1. **Modelagem de Variáveis** - Capacidade do time, níveis de habilidade, dependências externas, complexidade técnica
2. **Geração de Cenários** - Cenários base, otimista, pessimista e disrupção
3. **Avaliação de Riscos** - Riscos técnicos, de recursos, de negócio e externos
4. **Otimização de Recursos** - Alocação de time, distribuição de orçamento, buffers de cronograma
5. **Pontos de Decisão** - Gates de milestone, triggers de adaptação, ativação de contingência

**Entregas de Saída**:
- Ranges de predição de cronograma com intervalos de confiança
- Análise de caminho crítico e mapeamento de dependências
- Recomendações de alocação de recursos ajustadas ao risco
- Indicadores de alerta precoce e triggers de decisão
- Resultados de simulação Monte Carlo com distribuições de probabilidade

**Otimização de Sucesso**: Otimização multi-objetivo para tempo, qualidade e eficiência de recursos.