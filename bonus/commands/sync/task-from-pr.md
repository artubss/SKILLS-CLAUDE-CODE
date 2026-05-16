---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [pr-number] | --team | --estimate | --batch-process | --auto-create
description: Criar tarefas Linear a partir de pull requests do GitHub com extração inteligente de conteúdo e dimensionamento de tarefas
---

# Tarefa a partir de PR

Criar tarefas Linear a partir de pull requests do GitHub com análise inteligente: **$ARGUMENTS**

## Ambiente Atual do PR

- Repositório: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`
- Status do PR: Baseado no número do PR de $ARGUMENTS ou critérios de processamento em lote
- Times Linear: Times disponíveis para atribuição de tarefas
- Mapeamentos de usuários: Correspondência de nome de usuário do GitHub para usuário Linear

## Tarefa

Gerar tarefas Linear a partir de pull requests do GitHub com análise abrangente de conteúdo:

**Origem do PR**: Use $ARGUMENTS para especificar número do PR, atribuição de time, estimativa de tamanho ou modo de processamento em lote

**Framework de Geração de Tarefas**:
1. **Análise do PR** - Extrair dados abrangentes do PR, analisar estrutura de descrição, identificar componentes-chave, analisar alterações
2. **Extração de Conteúdo** - Analisar seções estruturadas, extrair checklists, identificar detalhes técnicos, capturar requisitos
3. **Dimensionamento Inteligente** - Estimar complexidade da tarefa a partir de mudanças de código, contagem de arquivos, comentários de revisão, requisitos de testes
4. **Construção da Tarefa** - Criar tarefa Linear com formatação adequada, preservar contexto do PR, manter referências, estruturar conteúdo
5. **Atribuição de Time** - Mapear para time Linear apropriado, atribuir com base em áreas de código, definir prioridades a partir de labels
6. **Validação e Criação** - Verificar duplicatas, validar estrutura da tarefa, criar em Linear, estabelecer links bidirecionais

**Recursos Avançados**: Análise inteligente de conteúdo, estimativa automatizada de tamanho, mapeamento inteligente de time, validação abrangente, processamento em lote.

**Garantia de Qualidade**: Detecção de duplicatas, validação de conteúdo, formatação apropriada, manutenção de relacionamentos, tratamento abrangente de erros.

**Saída**: Tarefas Linear criadas com sucesso com contexto abrangente do PR, estimativas de tamanho precisas, atribuições de time apropriadas e linking bidirecional completo.