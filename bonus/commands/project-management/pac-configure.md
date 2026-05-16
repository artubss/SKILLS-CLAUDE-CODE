---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [project-name] | --minimal | --epic-name | --owner
description: Inicializar estrutura de projeto Product as Code (PAC) com templates e configuração
---

# Configurar Projeto PAC

Inicializar estrutura de projeto Product as Code (PAC): **$ARGUMENTS**

## Estado Atual do Projeto

- Status Git: !`git status --porcelain | wc -l` mudanças não confirmadas
- Estrutura PAC: !`ls -la .pac/ 2>/dev/null | head -5 || echo "Nenhum diretório PAC"`
- Épicos existentes: !`find .pac/epics/ -name "*.yaml" 2>/dev/null | wc -l`

## Tarefa

Configurar e inicializar estrutura de projeto PAC para gerenciamento de produtos versionado:

**Processo de Setup**:
1. **Análise de Projeto** - Validar repositório git e analisar estrutura PAC existente
2. **Criação de Diretórios** - Criar estrutura `.pac/` com épicos, tickets e templates
3. **Arquivos de Configuração** - Gerar `pac.config.yaml` com metadados e padrões do projeto
4. **Criação de Templates** - Criar templates de épicos e tickets seguindo especificação PAC v0.1.0
5. **Conteúdo Inicial** - Criar primeiro épico e ticket baseado na entrada do usuário
6. **Setup de Integração** - Configurar git hooks e scripts de validação

**Argumentos**: Use --minimal para estrutura básica, --epic-name para épico inicial, --owner para proprietário do produto.

**Próximos Passos**: Use `/project:pac-create-epic` e `/project:pac-create-ticket` para gerenciar desenvolvimento de produtos.