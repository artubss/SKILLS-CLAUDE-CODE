---
allowed-tools: Bash, Read, Grep, Glob
argument-hint: [time-period] | --sprint | --quarter | --all
description: Rastrear e analisar progresso de marcos do projeto com análise preditiva
---

# Rastreador de Marcos

Rastreie e monitore o progresso de marcos do projeto com análise abrangente: **$ARGUMENTS**

## Contexto Atual do Projeto

- Atividade do projeto: !`git log --oneline --since="30 days ago" | wc -l` commits
- Branches ativos: !`git branch -r | wc -l` remote branches
- Releases recentes: !`git tag -l --sort=-creatordate | head -5`
- Dados de marcos: @.github/milestones/ ou integração Linear

## Tarefa

Gerar relatório abrangente de rastreamento de marcos analisando progresso de entrega do projeto:

**Período de Tempo**: Use $ARGUMENTS ou padrão para sprint/trimestre atual

**Dimensões de Análise**:
1. **Rastreamento de Progresso de Marcos**
   - Taxas de conclusão de marcos atuais
   - Tendências de velocidade e análise de burn-down
   - Identificação de caminho crítico
   - Mapeamento de dependências e avaliação de riscos

2. **Análise Preditiva**
   - Previsões de data de conclusão com intervalos de confiança
   - Recomendações de cronograma ajustadas ao risco
   - Otimização de alocação de recursos
   - Planejamento de cenários (análise e-se)

3. **Indicadores de Saúde**
   - Métricas de aderência ao cronograma
   - Utilização de capacidade do time
   - Identificação de bloqueadores e impacto
   - Equilíbrio entre qualidade e entrega

**Saída**: Dashboard interativo de marcos com indicadores visuais de progresso, análise preditiva, avaliações de risco e recomendações acionáveis para otimização de entrega de marcos.