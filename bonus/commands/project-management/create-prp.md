---
allowed-tools: Read, Write, Edit, WebSearch, Grep, Glob
argument-hint: [feature-description] | --research | --template | --validate
description: Criar Requisitos de Produto Abrangentes (PRP) com pesquisa e validação
---

# Criar Requisitos de Produto

Criar Requisitos de Produto (PRP) abrangentes seguindo processo de pesquisa estruturado: **$ARGUMENTS**

## Fundação do PRP

- Modelo base: @concept_library/cc_PRP_flow/PRPs/base_template_v1
- Conceito PRP: @concept_library/cc_PRP_flow/README.md
- PRPs existentes: !`find concept_library/cc_PRP_flow/PRPs/ -name "*.md" | head -5`
- Documentação: análise do diretório @ai_docs/

## Tarefa

Desenvolver PRP abrangente através de pesquisa sistemática e documentação estruturada:

**Processo de Pesquisa**:
1. **Revisão de Documentação** - Analisar ai_docs/ existente e documentação do projeto
2. **Pesquisa Web** - Coletar exemplos de implementação, docs de bibliotecas e melhores práticas
3. **Análise de Modelo** - Estudar estrutura do base_template_v1 e PRPs existentes
4. **Exploração da Base de Código** - Identificar padrões, dependências e pontos de integração
5. **Síntese de Contexto** - Compilar contexto abrangente de implementação

**Desenvolvimento do PRP**:
- Seguir estrutura do base_template_v1 exatamente
- Incluir referências específicas de arquivos e recursos web
- Fornecer inteligência curada da base de código
- Definir critérios de validação clara e métricas de sucesso
- Criar guia de implementação pronto para produção

**Lembre-se**: PRP = PRD + inteligência curada da base de código + agent/runbook—o pacote mínimo viável que um AI precisa para entregar código pronto para produção na primeira tentativa.