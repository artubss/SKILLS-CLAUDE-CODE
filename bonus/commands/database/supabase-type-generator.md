---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [generation-scope] | --all-tables | --specific-table | --functions | --enums | --views
description: Gerar tipos TypeScript a partir do schema Supabase com sincronização automática e validação
---

# Gerador de Tipos Supabase

Gerar tipos TypeScript abrangentes a partir do schema Supabase com sincronização automática: **$ARGUMENTS**

## Contexto de Tipos Atual

- Schema Supabase: Schema do banco de dados acessível via integração MCP
- Definições de tipos: !`find . -name "types" -type d -o -name "*.d.ts" | head -5` definições TypeScript existentes
- Uso na aplicação: !`find . -name "*.ts" -o -name "*.tsx" | xargs grep -l "Database\|Table\|Row" 2>/dev/null | head -3` padrões de uso de tipos
- Configuração de build: !`find . -name "tsconfig.json" -o -name "*.config.ts" | head -3` setup TypeScript

## Tarefa

Executar geração abrangente de tipos com sincronização de schema e integração da aplicação:

**Escopo de Geração**: Use $ARGUMENTS para gerar todos os tipos de tabela, tipos de tabela específica, assinaturas de função, definições de enum ou tipos de view

**Framework de Geração de Tipos**:
1. **Análise de Schema** - Extrair schema do banco de dados via MCP, analisar estruturas de tabela, identificar relacionamentos, mapear tipos de dados para TypeScript
2. **Geração de Tipos** - Gerar interfaces de tabela, criar tipos utilitários, implementar type guards, otimizar definições de tipo
3. **Setup de Integração** - Configurar caminhos de importação, setup de exportação de tipos, implementar auto-complete, integrar com processo de build
4. **Processo de Validação** - Validar tipos gerados, testar compatibilidade de tipos, verificar integração da aplicação, verificar sucesso do build
5. **Sincronização** - Monitorar mudanças de schema, regenerar tipos automaticamente, validar mudanças que quebram compatibilidade, notificar time de desenvolvimento
6. **Experiência do Desenvolvedor** - Implementar integração com IDE, fornecer dicas de tipo, criar exemplos de uso, otimizar workflow de desenvolvimento

**Recursos Avançados**: Atualizações automáticas de tipos, detecção de mudanças que quebram compatibilidade, transformações de tipo customizadas, geração de documentação, integração com plugin de IDE.

**Garantia de Qualidade**: Validação de precisão de tipos, testes de compatibilidade da aplicação, avaliação de impacto de performance, integração de feedback do desenvolvedor.

**Output**: Definições de tipos TypeScript completas com sincronização de schema, integração da aplicação, procedimentos de validação e documentação para desenvolvedores.