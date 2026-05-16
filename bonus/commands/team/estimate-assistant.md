---
allowed-tools: Ler, Bash, Glob, Grep
argument-hint: [descrição-da-tarefa] | --histórico | --análise-complexidade | --velocidade-equipe | --intervalos-confiança
description: Gerar estimativas de tarefas precisas usando dados históricos, análise de complexidade e métricas de velocidade da equipe
---

# Assistente de Estimativas

Gere estimativas orientadas por dados com intervalos de confiança e rastreamento de precisão: **$ARGUMENTS**

## Contexto Atual de Estimativa

- Velocidade da equipe: !`git log --oneline --since='1 month ago' | wc -l` commits no último mês
- Dados históricos: Análise do histórico Git para padrões de conclusão de tarefas similares
- Complexidade do código: !`find . -name "*.js" -o -name "*.ts" -o -name "*.py" | head -5 | xargs wc -l 2>/dev/null | tail -1 || echo "No code files"`
- Rastreamento de sprint: Tempos de conclusão de tarefas lineares e precisão de estimativas

## Tarefa

Execute estimativa abrangente de tarefas com análise histórica e modelagem de confiança:

**Foco de Estimativa**: Use $ARGUMENTS para análise de descrição de tarefas, correspondência de padrões históricos, avaliação de complexidade ou cálculo de velocidade da equipe

**Framework de Estimativa**:
1. **Análise de Padrões Históricos** - Analise tarefas passadas similares, extraia padrões de tempo de conclusão, identifique tendências de velocidade, calcule métricas de precisão
2. **Avaliação de Complexidade** - Avalie complexidade técnica, assess incerteza de escopo, identifique fatores de risco, estime distribuição de esforço
3. **Integração de Velocidade da Equipe** - Calcule velocidade de sprint, analise capacidade individual, assess expertise da equipe, considere restrições de disponibilidade
4. **Modelagem de Confiança** - Gere intervalos de confiança, assess incerteza de estimativa, identifique fatores de risco, forneça faixas de precisão
5. **Análise de Calibração** - Compare estimativas passadas vs reais, identifique vieses sistemáticos, calcule precisão de estimativa, melhore modelos de previsão
6. **Integração de Contexto** - Considere carga atual do sprint, assess familiaridade da equipe, avalie dependências externas, integre pressão de prazo

**Recursos Avançados**: Estimativa multi-ponto, simulação Monte Carlo, previsão por classe de referência, rastreamento de precisão de estimativas, algoritmos de correção de vieses.

**Métricas de Qualidade**: Níveis de confiança de estimativa, tendências históricas de precisão, estabilidade de velocidade, análise de correlação de complexidade.

**Saída**: Estimativas orientadas por dados com intervalos de confiança, métricas de precisão histórica, avaliação de risco e recomendações de calibração.