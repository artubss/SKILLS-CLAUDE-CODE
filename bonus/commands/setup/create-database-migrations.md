---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [migration-name] | --create-table | --add-column | --alter-table
description: Criar e gerenciar migrações de banco de dados com versionamento apropriado e suporte a rollback
---

# Criar Migrações de Banco de Dados

Criar e gerenciar migrações de banco de dados: **$ARGUMENTS**

## Estado Atual do Banco de Dados

- Detecção de ORM: @package.json ou @requirements.txt (detecta Sequelize, Prisma, Alembic, etc.)
- Arquivos de migração: !`find . -name "*migration*" -type f | head -5`
- Configuração do banco: @config/database.* ou @prisma/schema.prisma
- Schema atual: !`ls migrations/ 2>/dev/null | wc -l` migrações encontradas

## Tarefa

Criar migrações de banco de dados abrangentes com versionamento apropriado e capacidades confiáveis de rollback:

**Tipos de Migração**: Use $ARGUMENTS para especificar criação de tabela, adição de coluna, alteração de tabela ou migração de dados

**Framework de Migração**:
1. **Planejamento de Migração** - Analisar alterações de schema, dependências e impacto nos dados
2. **Geração de Migração** - Criar arquivos de migração com timestamp contendo métodos up/down
3. **Atualizações de Schema** - Criação de tabelas, modificações de colunas, gerenciamento de índices
4. **Migrações de Dados** - Transformações seguras de dados e preenchimentos retroativos
5. **Estratégia de Rollback** - Implementar procedimentos de rollback confiáveis para cada alteração
6. **Testes** - Validar migrações em ambientes de desenvolvimento e staging

**Melhores Práticas**: Seguir convenções específicas do banco de dados, manter integridade referencial, lidar eficientemente com grandes volumes de dados e garantir deployments sem tempo de inatividade.

**Output**: Arquivos de migração prontos para produção com suporte abrangente a rollback, indexação apropriada e medidas de segurança de dados.