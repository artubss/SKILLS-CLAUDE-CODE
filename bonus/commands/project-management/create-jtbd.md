---
allowed-tools: Read, Write, Edit, Grep, Glob
argument-hint: [nome-da-funcionalidade] | --template | --interactive
description: Criar análise de Jobs-to-be-Done (JTBD) para funcionalidades do produto
---

# Criar Documento de Jobs-to-be-Done

Você é um Gerente de Produto experiente. Crie um documento de Jobs to be Done (JTBD) para uma funcionalidade que estamos adicionando ao produto: **$ARGUMENTS**

**IMPORTANTE:**
- Foque na funcionalidade e necessidades do usuário, não na implementação técnica
- Não inclua estimativas de tempo

## Documentação Obrigatória

1. **Documentação do Produto**: @product-development/resources/product.md (para entender o produto)
2. **Ideia da Funcionalidade**: @product-development/current-feature/feature.md (para entender a ideia da funcionalidade)

**IMPORTANTE**: Se não conseguir encontrar o arquivo da funcionalidade, interrompa o processo e notifique o usuário.

## Tarefa

Crie um documento JTBD que capture o porquê por trás do comportamento do usuário e foque no problema ou trabalho que o usuário está tentando realizar:

1. Use o template JTBD de `@product-development/resources/JTBD-template.md` 
2. Com base na ideia da funcionalidade, crie um documento JTBD que inclua:
   - Declarações de trabalho seguindo o padrão "Quando [situação], quero [motivação], para que eu possa [resultado esperado]"
   - Análise de necessidades e pontos de dor do usuário  
   - Resultados desejados da perspectiva do usuário
   - Análise competitiva através da lente JTBD
   - Avaliação de oportunidade de mercado

3. Exporte o documento JTBD para `product-development/current-feature/JTBD.md`

Foque em entender os trabalhos fundamentais que os usuários estão tentando realizar, em vez de funcionalidades técnicas.