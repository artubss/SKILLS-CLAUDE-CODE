---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [analysis-type] | --current-workload | --skill-matching | --capacity-planning | --assignment-optimization
description: Analise e otimize a distribuição de carga de trabalho da equipe com correspondência de habilidades e planejamento de capacidade
---

# Balanceador de Carga de Trabalho da Equipe

Analise e otimize a distribuição de carga de trabalho da equipe com recomendações inteligentes de atribuição: **$ARGUMENTS**

## Contexto Atual da Equipe

- Tamanho da equipe: !`git log --format='%ae' --since='1 month ago' | sort -u | wc -l` membros ativos da equipe
- Tarefas ativas: Consulta Linear MCP para tarefas atuais do sprint e atribuições
- Atividade recente: !`git log --oneline --since='1 week ago' | wc -l` commits na última semana
- Métricas de capacidade: Análise da velocidade da equipe e padrões de contribuição individual

## Tarefa

Execute análise abrangente de carga de trabalho com otimização inteligente de atribuição:

**Tipo de Análise**: Use $ARGUMENTS para focar em avaliação de carga de trabalho atual, correspondência de habilidades, planejamento de capacidade ou otimização de atribuição

**Framework de Balanceamento de Carga**:
1. **Avaliação de Carga de Trabalho Atual** - Analise a distribuição de tarefas, avalie a capacidade individual, assess pressão de prazos, identifique membros sobrecarregados
2. **Análise de Correspondência de Habilidades** - Mapeie expertise dos membros da equipe, identifique lacunas de habilidades, assess oportunidades de aprendizado, otimize utilização de habilidades
3. **Planejamento de Capacidade** - Calcule capacidade disponível, projete carga de trabalho futura, planeje desenvolvimento de habilidades, otimize alocação de recursos
4. **Integração de Desempenho** - Analise desempenho histórico, identifique padrões de produtividade, assess efetividade de colaboração, considere restrições de disponibilidade
5. **Otimização de Atribuição** - Gere atribuições de tarefas ótimas, balance distribuição de carga, maximize utilização de habilidades, minimize gargalos
6. **Mitigação de Riscos** - Identifique pontos únicos de falha, planeje treinamento cruzado, assess distribuição de conhecimento, garanta cobertura de backup

**Recursos Avançados**: Modelagem preditiva de carga de trabalho, análise de lacunas de habilidades, prevenção de burnout, atribuição baseada em desempenho, recomendações de rebalanceamento dinâmico.

**Métricas de Qualidade**: Equidade de distribuição de carga, eficiência de utilização de habilidades, indicadores de satisfação da equipe, medidas de previsibilidade de entrega.

**Saída**: Análise abrangente de carga de trabalho com atribuições otimizadas, recomendações de capacidade, planos de desenvolvimento de habilidades e insights de saúde da equipe.