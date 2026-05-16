---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [action] | --pull | --push | --diff | --validate
description: Sincronizar schema de banco de dados com Supabase usando integração MCP
---

# Sincronização de Schema Supabase

Sincronize schema de banco de dados entre local e Supabase com validação abrangente: **$ARGUMENTS**

## Contexto Supabase Atual

- Conexão MCP: Servidor Supabase MCP com acesso somente leitura configurado
- Schema local: !`find . -name "schema.sql" -o -name "migrations" -type d | head -3` arquivos de banco de dados locais
- Configuração do projeto: !`find . -name "supabase" -type d -o -name ".env*" | grep -v node_modules | head -3` arquivos de configuração
- Status Git: !`git status --porcelain | grep -E "\\.sql$|\\.ts$" | head -5` alterações relacionadas ao banco de dados

## Tarefa

Execute sincronização abrangente de schema com integração Supabase:

**Ação de Sincronização**: Use $ARGUMENTS para especificar pull do remoto, push para remoto, comparação de diff ou validação de schema

**Framework de Sincronização de Schema**:
1. **Integração MCP** - Conecte ao Supabase via servidor MCP, autentique com credenciais do projeto, valide status de conexão
2. **Análise de Schema** - Compare schema local vs remoto, identifique diferenças estruturais, analise requisitos de migração, avalie mudanças quebra-compatibilidade
3. **Operações de Sincronização** - Execute operações pull/push, aplique migrações de schema, resolva conflitos, valide integridade de dados
4. **Processo de Validação** - Verifique consistência de schema, valide restrições de chave estrangeira, verifique desempenho de índices, teste compatibilidade de queries
5. **Gerenciamento de Migrações** - Gere scripts de migração, rastreie histórico de versões, implemente procedimentos de rollback, otimize ordem de execução
6. **Verificações de Segurança** - Faça backup de dados críticos, valide permissões, verifique impacto em produção, implemente modo dry-run

**Funcionalidades Avançadas**: Resolução automática de conflitos, controle de versão de schema, análise de impacto de desempenho, workflows de colaboração em equipe, integração CI/CD.

**Garantia de Qualidade**: Validação de schema, verificações de integridade de dados, otimização de desempenho, prontidão para rollback, sincronização em equipe.

**Saída**: Sincronização completa de schema com relatórios de validação, scripts de migração, resolução de conflitos e atualizações de colaboração em equipe.