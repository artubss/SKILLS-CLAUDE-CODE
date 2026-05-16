---
allowed-tools: Read, Write, Edit, Grep, Glob
argument-hint: [nome-feature] | --template | --interactive
description: Criar Documento de Requisitos de Produto (PRD) para novas funcionalidades
---

# Criar Documento de Requisitos de Produto

Você é um Gerente de Produto experiente. Crie um Documento de Requisitos de Produto (PRD) para uma funcionalidade que estamos adicionando ao produto: **$ARGUMENTS**

**IMPORTANTE:**
- Foque na funcionalidade e nas necessidades do usuário, não na implementação técnica
- Não inclua estimativas de tempo

## Contexto do Produto

1. **Documentação do Produto**: @product-development/resources/product.md (para compreender o produto)
2. **Documentação da Funcionalidade**: @product-development/current-feature/feature.md (para compreender a ideia da funcionalidade)
3. **Documentação JTBD**: @product-development/current-feature/JTBD.md (para compreender os Jobs to be Done)

## Tarefa

Crie um documento PRD abrangente que capture o quê, por quê e como do produto:

1. Use o template de PRD em `@product-development/resources/PRD-template.md`
2. Com base na documentação da funcionalidade, crie um PRD que defina:
   - Declaração do problema e necessidades do usuário
   - Especificações e escopo da funcionalidade
   - Métricas de sucesso e critérios de aceitação
   - Requisitos de experiência do usuário
   - Considerações técnicas (apenas alto nível)

3. Exporte o PRD completo para `product-development/current-feature/PRD.md`

Foque em criar um PRD abrangente que defina claramente os requisitos da funcionalidade, mantendo alinhamento com as necessidades do usuário e objetivos de negócio.