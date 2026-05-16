---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [table-name] | --query [sql] | --export | --inspect
description: Explore and analyze Supabase database data with intelligent querying and visualization
---

# Explorador de Dados Supabase

Explore e analise banco de dados Supabase com querying inteligente e data insights: **$ARGUMENTS**

## Contexto de Dados Atual

- Supabase MCP: Conectado com acesso somente leitura para exploração segura de dados
- Tabela alvo: Análise de $ARGUMENTS para escopo de exploração de dados
- Queries locais: !`find . -name "*.sql" | head -5` arquivos SQL existentes para referência
- Modelos de dados: !`find . -name "types" -o -name "models" -type d | head -3` estruturas de dados da aplicação

## Tarefa

Execute exploração abrangente de banco de dados com análise e insights inteligentes:

**Foco de Exploração**: Use $ARGUMENTS para especificar inspeção de tabela, execução de query SQL, exportação de dados ou inspeção abrangente de banco de dados

**Framework de Exploração de Dados**:
1. **Database Discovery** - Explore estruturas de tabela, analise relacionamentos, identifique padrões de dados, avalie métricas de qualidade de dados
2. **Intelligent Querying** - Execute queries somente leitura via MCP, otimize desempenho de query, forneça análise de resultados, sugira melhorias de query
3. **Data Analysis** - Gere insights de dados, identifique tendências e anomalias, calcule sumários estatísticos, analise distribuição de dados
4. **Schema Inspection** - Examine schemas de tabela, analise relacionamentos de chave estrangeira, avalie efetividade de índices, revise validações de constraint
5. **Export & Visualization** - Exporte dados em múltiplos formatos, crie visualizações de dados, gere relatórios resumidos, otimize apresentação de dados
6. **Performance Analysis** - Analise planos de execução de query, identifique gargalos de desempenho, sugira estratégias de otimização, monitore uso de recursos

**Recursos Avançados**: Exploração interativa de dados, geração automatizada de insights, avaliação de qualidade de dados, mapeamento de relacionamentos, análise de tendências.

**Recursos de Segurança**: Operações somente leitura, validação de query, limitação de resultados, monitoramento de desempenho, tratamento de erros.

**Output**: Exploração abrangente de dados com insights, queries otimizadas, arquivos de exportação e recomendações de desempenho.