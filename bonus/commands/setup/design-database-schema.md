---
allowed-tools: Ler, Escrever, Editar, Bash
argument-hint: [tipo-schema] | --relacional | --nosql | --hibrido | --normalizar
description: Projete schemas de banco de dados otimizados com relacionamentos adequados, constraints e considerações de performance
---

# Projetar Schema de Banco de Dados

Projete schemas de banco de dados otimizados com modelagem de dados abrangente: **$ARGUMENTS**

## Contexto Atual do Projeto

- Tipo de aplicação: Baseado em $ARGUMENTS ou análise da base de código
- Requisitos de dados: @requirements/ ou documentação do projeto
- Schema existente: @prisma/schema.prisma ou @migrations/ ou dumps de banco de dados
- Necessidades de performance: Escala esperada, padrões de query e volume de dados

## Tarefa

Projete um schema de banco de dados abrangente com estrutura e performance otimizadas:

**Tipo de Schema**: Use $ARGUMENTS para especificar abordagem relacional, NoSQL, híbrida ou nível de normalização

**Framework de Design**:
1. **Análise de Requisitos** - Entidades de negócio, relacionamentos, fluxo de dados e padrões de acesso
2. **Modelagem de Entidades** - Tabelas/coleções, atributos, chaves primárias/estrangeiras, constraints
3. **Design de Relacionamentos** - Associações um-para-um, um-para-muitos, muitos-para-muitos
4. **Estratégia de Normalização** - Trade-offs entre consistência de dados e performance
5. **Otimização de Performance** - Estratégia de indexação, otimização de queries, particionamento
6. **Design de Segurança** - Controle de acesso, criptografia de dados, trilhas de auditoria

**Padrões Avançados**: Implemente dados temporais, soft deletes, campos JSONB, busca full-text, logging de auditoria e padrões de escalabilidade.

**Validação**: Garanta integridade referencial, consistência de dados, performance de query e extensibilidade futura.

**Output**: Design de schema completo com scripts DDL, diagramas ER, análise de performance e estratégia de migração.