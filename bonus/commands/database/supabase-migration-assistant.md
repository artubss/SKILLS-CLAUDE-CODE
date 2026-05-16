---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [migration-type] | --create | --alter | --seed | --rollback
description: Gerar e gerenciar migrações de banco de dados Supabase com testes e validação automatizados
---

# Assistente de Migração Supabase

Gerar e gerenciar migrações Supabase com testes e validação abrangentes: **$ARGUMENTS**

## Contexto Atual de Migração

- Projeto Supabase: Integração MCP para gerenciamento e validação de migrações
- Arquivos de migração: !`find . -name "*migrations*" -type d -o -name "*.sql" | head -5` estrutura de migração existente
- Versão do schema: Estado atual do banco de dados e histórico de migrações
- Mudanças locais: !`git diff --name-only | grep -E "\\.sql$|\\.ts$" | head -3` modificações pendentes do banco de dados

## Tarefa

Executar gerenciamento abrangente de migrações com validação e testes automatizados:

**Tipo de Migração**: Use $ARGUMENTS para especificar criação de tabela, alterações de schema, semeadura de dados ou reversão de migração

**Framework de Gerenciamento de Migrações**:
1. **Planejamento de Migração** - Analisar requisitos de schema, projetar estratégia de migração, identificar dependências, planejar procedimentos de reversão
2. **Geração de Código** - Gerar arquivos SQL de migração, criar tipos TypeScript, implementar verificações de segurança, otimizar ordem de execução
3. **Testes de Validação** - Testar migração em dados de desenvolvimento, validar alterações de schema, verificar integridade de dados, verificar violações de constraint
4. **Integração Supabase** - Aplicar migrações via servidor MCP, monitorar status de execução, tratar condições de erro, validar estado final
5. **Geração de Tipos** - Gerar tipos TypeScript, atualizar interfaces de aplicação, sincronizar com schemas do lado do cliente, manter type safety
6. **Estratégia de Reversão** - Criar migrações de reversão, testar procedimentos de rollback, implementar preservação de dados, validar processo de recuperação

**Recursos Avançados**: Geração automatizada de tipos, testes de migração, análise de impacto de desempenho, colaboração em equipe, integração CI/CD.

**Medidas de Segurança**: Backups pré-migração, validação dry-run, testes de rollback, verificações de integridade de dados, monitoramento de desempenho.

**Output**: Suíte completa de migração com arquivos SQL, tipos TypeScript, validação de testes, procedimentos de rollback e documentação de deploy.