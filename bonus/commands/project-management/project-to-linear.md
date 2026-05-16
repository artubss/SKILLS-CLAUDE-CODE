---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [descrição-do-projeto] | --team-id | --create-new | --epic-name
description: Sincronize estrutura de projeto e requisitos com workspace Linear com breakdown abrangente de tarefas
---

# Project to Linear

Sincronize estrutura de projeto e requisitos com workspace Linear: **$ARGUMENTS**

## Status da Integração Linear

- Linear MCP: Verifique se o servidor Linear MCP está configurado
- Acesso ao workspace: !`echo "Teste a conexão Linear se MCP estiver disponível"`
- Contexto do projeto: @README.md ou documentação do projeto
- Requisitos: Baseado na análise de $ARGUMENTS

## Tarefa

Analise os requisitos do projeto e crie uma estrutura de tarefas Linear abrangente:

**Processo de Análise do Projeto**:
1. **Análise de Requisitos** - Parse da descrição do projeto e identificação de componentes principais
2. **Breakdown de Tarefas** - Criação de estrutura hierárquica de tarefas com epics e subtarefas
3. **Mapeamento de Dependências** - Identificação de dependências de tarefas e caminho crítico
4. **Integração Linear** - Criação de projeto, epics e tarefas no workspace Linear
5. **Validação** - Revisão da estrutura criada e fornecimento de visão geral do projeto

**Organização de Tarefas**:
- Recursos de nível epic e componentes principais
- Tarefas pai para áreas de funcionalidade
- Subtarefas detalhadas com critérios de aceitação
- Labeling apropriado (frontend, backend, testing, documentação)
- Estimativas de prioridade e esforço
- Relacionamentos de timeline e dependência

**Output**: Estrutura de projeto Linear completa com hierarquia de tarefas organizada, descrições claras e itens acionáveis.