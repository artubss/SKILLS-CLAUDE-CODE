---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [task-id] | --repo | --milestone | --close-linear | --skip-attachments
description: Converter tarefas Linear para issues do GitHub com preservação de relacionamentos e mapeamento de metadados
---

# Linear Task to Issue

Converter tarefas Linear para issues do GitHub com mapeamento abrangente de relacionamentos: **$ARGUMENTS**

## Contexto da Tarefa Atual

- Detalhes da tarefa: Baseado no identificador da tarefa $ARGUMENTS ou critérios de seleção
- Repositório alvo: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`
- Mapeamentos de usuários: Correspondência entre email Linear e username do GitHub
- Tratamento de anexos: Capacidades de acesso a anexos Linear e upload no GitHub

## Tarefa

Executar conversão precisa de tarefas Linear para issues do GitHub:

**Alvo da Tarefa**: Use $ARGUMENTS para especificar identificador da tarefa, repositório alvo, mapeamento de milestone ou preferências de processamento

**Framework de Conversão**:
1. **Análise da Tarefa** - Buscar dados completos da tarefa Linear, extrair relacionamentos, analisar estrutura do conteúdo, identificar prioridades
2. **Transformação de Conteúdo** - Construir corpo da issue do GitHub, mapear campos Linear, preservar formatação, manipular conteúdo enriquecido
3. **Integração GitHub** - Criar issue com estrutura adequada, aplicar labels, atribuir usuários, definir milestones, gerenciar relacionamentos
4. **Migração de Anexos** - Baixar anexos Linear, fazer upload no GitHub, atualizar referências, manter acessibilidade
5. **Importação de Comentários** - Transferir comentários com atribuição, preservar timestamps, manter contexto, manipular menções
6. **Configuração de Referências Cruzadas** - Criar links bidirecionais, atualizar tarefa Linear, manter banco de dados de sincronização, habilitar navegação

**Funcionalidades Avançadas**: Conversão de conteúdo enriquecido, tratamento de anexos, mapeamento de relacionamentos, tradução de menções de usuários, validação abrangente.

**Gerenciamento de Relacionamentos**: Preservar relacionamentos pai-filho, manter contexto do time, mapear associações de projeto, lidar com dependências.

**Output**: Issue do GitHub criada com sucesso com migração completa de dados, mapeamentos de campo precisos, relacionamentos preservados e relatório de conversão abrangente.