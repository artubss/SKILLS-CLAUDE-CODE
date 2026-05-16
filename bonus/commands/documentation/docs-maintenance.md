---
allowed-tools: Read, Write, Edit, Bash, Grep
argument-hint: [maintenance-type] | --audit | --update | --validate | --optimize | --comprehensive
description: Use PROACTIVELY para implementar sistemas abrangentes de manutenção de documentação com garantia de qualidade, validação e atualizações automatizadas
---

# Manutenção de Documentação & Garantia de Qualidade

Implementar sistema abrangente de manutenção de documentação: $ARGUMENTS

## Saúde Atual da Documentação

- Arquivos de documentação: !`find . -name "*.md" -o -name "*.mdx" | wc -l` arquivos
- Últimas atualizações: !`find . -name "*.md" -exec stat -f "%m %N" {} \; | sort -n | tail -5`
- Links externos: !`grep -r "http" --include="*.md" . | wc -l` links para validar
- Referências de imagens: !`grep -r "!\[.*\]" --include="*.md" . | wc -l` imagens para verificar
- Estrutura de documentação: @docs/ ou detectar diretórios de documentação

## Tarefa

Criar framework sistemático de manutenção de documentação com garantia de qualidade automatizada, validação abrangente, otimização de conteúdo e procedimentos de atualização regulares.

## Framework de Manutenção de Documentação

### 1. Sistema de Auditoria de Qualidade de Conteúdo
- Descoberta e categorização abrangente de arquivos
- Análise de frescor de conteúdo e detecção de envelhecimento
- Avaliação de contagem de palavras, legibilidade e estrutura
- Identificação de seções ausentes e documentação incompleta
- Rastreamento de marcadores TODO/FIXME e planejamento de resolução

### 2. Validação de Links e Referências
- Monitoramento de saúde de links externos com lógica de repetição
- Validação de links internos e detecção de referências quebradas
- Verificação de referências de imagens e identificação de ativos ausentes
- Verificação de consistência de referências cruzadas
- Sugestões automatizadas de correção de links

### 3. Verificação de Estilo e Consistência
- Validação de sintaxe Markdown e padrões de formatação
- Consistência de hierarquia de headings e estrutura
- Uniformidade de formatação de listas e estilo de ênfase
- Formatação de blocos de código e especificação de linguagem
- Conformidade com acessibilidade (texto alternativo, links descritivos)

### 4. Otimização e Aprimoramento de Conteúdo
- Geração de índice para documentos longos
- Atualização de metadados e gerenciamento de frontmatter
- Correção de problemas de formatação comuns
- Validação de ortografia e gramática
- Análise de legibilidade e sugestões de melhoria

### 5. Sistema de Sincronização Automatizada
- Rastreamento de mudanças baseado em Git e atualizações de documentação
- Integração com controle de versão e gerenciamento de branches
- Geração automatizada de commits com changelogs detalhados
- Estratégias de resolução de conflitos de merge
- Procedimentos de rollback para atualizações com falha

### 6. Relatórios de Garantia de Qualidade
- Relatórios de auditoria abrangentes com classificação por severidade
- Sistemas de categorização e priorização de problemas
- Rastreamento de progresso e métricas de manutenção
- Sistemas de notificação automatizada para problemas críticos
- Criação de dashboard para monitoramento contínuo

## Requisitos de Implementação

### Configuração de Auditoria
- Limites de qualidade e regras de validação configuráveis
- Integração e aplicação de guias de estilo personalizados
- Configurações de otimização específicas da plataforma
- Integração de workflow de colaboração em equipe
- Agendamento automatizado e manutenção recorrente

### Processos de Validação
- Validação em múltiplos níveis com categorização de erros
- Processamento em lote para grandes conjuntos de documentação
- Otimização de desempenho para varreduras abrangentes
- Integração com pipelines CI/CD existentes
- Sistemas de monitoramento e alertas em tempo real

### Relatórios e Análises
- Relatórios detalhados de manutenção com insights acionáveis
- Análise de tendências históricas e rastreamento de melhorias
- Métricas de produtividade da equipe e pontuações de saúde da documentação
- Integração com ferramentas de gerenciamento de projetos
- Comunicação automatizada com stakeholders

## Entregas

1. **Arquitetura do Sistema de Manutenção**
   - Framework automatizado de auditoria e validação
   - Ferramentas de otimização e aprimoramento de conteúdo
   - Infraestrutura de relatórios de garantia de qualidade
   - Integração de controle de versão e sincronização

2. **Ferramentas de Validação e Qualidade**
   - Sistemas de validação de links e referências
   - Ferramentas de consistência de estilo e conformidade de acessibilidade
   - Analisadores de frescor e completude de conteúdo
   - Utilitários de correção e aprimoramento automatizados

3. **Relatórios e Monitoramento**
   - Relatórios de auditoria abrangentes com recomendações priorizadas
   - Dashboards de monitoramento em tempo real e sistemas de alertas
   - Rastreamento de progresso e documentação de histórico de manutenção
   - Integração com ferramentas de comunicação e projetos da equipe

4. **Documentação e Procedimentos**
   - Diretrizes de implementação e instruções de configuração
   - Integração de workflow da equipe e procedimentos de colaboração
   - Guias de resolução de problemas e práticas recomendadas de manutenção
   - Configuração de agendamento automatizado e manutenção recorrente

## Diretrizes de Integração

Implementar com plataformas de documentação existentes e workflows de desenvolvimento. Garantir escalabilidade para grandes conjuntos de documentação e colaboração em equipe, mantendo padrões de qualidade e conformidade com acessibilidade.