---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [tipo-otimizacao] | --queries | --indexes | --storage | --rls | --functions
description: Otimizar o desempenho do banco de dados Supabase com análise inteligente e recomendações
---

# Otimizador de Desempenho Supabase

Otimize o desempenho do banco de dados Supabase com análise inteligente e melhorias automatizadas: **$ARGUMENTS**

## Contexto de Desempenho Atual

- Métricas Supabase: Dados de desempenho do banco de dados via integração MCP
- Padrões de query: !`find . -name "*.sql" -o -name "*.ts" -o -name "*.js" | xargs grep -l "from\|select\|insert\|update" 2>/dev/null | head -5` queries de aplicação
- Análise de schema: Estruturas de tabela atuais e complexidade de relacionamentos
- Logs de desempenho: Tempos de execução de query recentes e padrões de uso de recursos

## Tarefa

Execute otimização de desempenho abrangente com análise inteligente e melhorias automatizadas:

**Foco de Otimização**: Use $ARGUMENTS para focar em otimização de queries, gerenciamento de índices, otimização de armazenamento, políticas RLS, ou funções de banco de dados

**Framework de Otimização de Desempenho**:
1. **Análise de Desempenho** - Analise tempos de execução de queries, identifique operações lentas, avalie utilização de recursos, identifique gargalos
2. **Otimização de Índices** - Analise uso de índices, recomende novos índices, identifique índices redundantes, otimize estratégias de índices
3. **Otimização de Queries** - Revise queries de aplicação, sugira melhorias de query, implemente cache de queries, otimize operações de join
4. **Otimização de Armazenamento** - Analise padrões de armazenamento, recomende estratégias de arquivo morto, otimize tipos de dados, implemente compressão
5. **Análise de Políticas RLS** - Analise políticas Row Level Security, otimize desempenho de políticas, reduza complexidade de políticas, melhore eficiência de segurança
6. **Otimização de Funções** - Revise funções de banco de dados, otimize desempenho de funções, implemente estratégias de cache, melhore planos de execução

**Recursos Avançados**: Recomendações automatizadas de índices, análise de plano de execução, monitoramento de tendências de desempenho, otimização de custos, recomendações de escala.

**Integração de Monitoramento**: Rastreamento de desempenho em tempo real, configuração de alertas, detecção de regressão de desempenho, medição de impacto de otimização.

**Output**: Plano de otimização abrangente com melhorias de desempenho, recomendações de índices, otimizações de queries e configuração de monitoramento.