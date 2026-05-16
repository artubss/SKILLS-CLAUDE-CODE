---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [schema-approach] | --schema-first | --code-first | --federation
description: Implementar API GraphQL com schema abrangente, resolvers e subscriptions em tempo real
---

# Implementar API GraphQL

Implemente uma API GraphQL abrangente com melhores práticas modernas: **$ARGUMENTS**

## Contexto da Aplicação Atual

- Framework: @package.json ou @requirements.txt (detectar Apollo, GraphQL Yoga, etc.)
- API existente: !`find . -name "*.graphql" -o -name "*schema*" -o -name "*resolver*" | wc -l`
- Integração de banco de dados: @prisma/schema.prisma ou configs de conexão de banco de dados
- Autenticação: !`grep -r "auth\|jwt\|context" src/ 2>/dev/null | wc -l`

## Tarefa

Construa uma API GraphQL pronta para produção com funcionalidade abrangente e otimização de desempenho:

**Abordagem de Schema**: Use $ARGUMENTS para especificar arquitetura schema-first, code-first ou federation

**Implementação GraphQL**:
1. **Design de Schema** - Definições de tipos, queries, mutations, subscriptions, scalars customizados
2. **Arquitetura de Resolvers** - Busca de dados, autenticação, autorização, tratamento de erros
3. **Integração de DataLoader** - Prevenção de queries N+1, batch loading, estratégias de cache
4. **Recursos em Tempo Real** - Subscriptions via WebSocket, atualizações de dados ao vivo, gerenciamento de conexão
5. **Segurança e Desempenho** - Análise de complexidade de queries, depth limiting, rate limiting
6. **Ferramentas de Desenvolvimento** - GraphQL Playground, introspection, schema stitching

**Recursos Avançados**: Upload de arquivos, schemas federados, Apollo Federation, schema directives e monitoramento.

**Prontidão para Produção**: Implemente tratamento abrangente de erros, logging, métricas e estratégias de deploy.

**Output**: API GraphQL completa com resolvers otimizados, capacidades em tempo real, controles de segurança e documentação para desenvolvedores.