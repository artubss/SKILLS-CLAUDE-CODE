---
allowed-tools: Bash, Read
description: Criar, atualizar e gerenciar issues do GitHub usando ferramentas MCP. Use esta skill quando usuários quiserem criar relatórios de bugs, solicitar features ou issues de tarefas, atualizar issues existentes, adicionar labels/assignees/milestones, ou gerenciar workflows de issues. Ativa em requisições como "criar uma issue", "reportar um bug", "solicitar uma feature", "atualizar issue X", ou qualquer tarefa de gerenciamento de issues do GitHub.
---

# Issues do GitHub

Gerencie issues do GitHub usando o servidor MCP `@modelcontextprotocol/server-github`.

## Ferramentas MCP Disponíveis

| Ferramenta | Propósito |
|------|---------|
| `mcp__github__create_issue` | Criar novas issues |
| `mcp__github__update_issue` | Atualizar issues existentes |
| `mcp__github__get_issue` | Obter detalhes da issue |
| `mcp__github__search_issues` | Pesquisar issues |
| `mcp__github__add_issue_comment` | Adicionar comentários |
| `mcp__github__list_issues` | Listar issues do repositório |

## Fluxo de Trabalho

1. **Determine a ação**: Criar, atualizar ou consultar?
2. **Reúna contexto**: Obtenha informações do repo, labels existentes, milestones se necessário
3. **Estruture o conteúdo**: Use o template apropriado de [references/templates.md](references/templates.md)
4. **Execute**: Chame a ferramenta MCP apropriada
5. **Confirme**: Reporte a URL da issue ao usuário

## Criando Issues

### Parâmetros Obrigatórios

```
owner: proprietário do repositório (organização ou usuário)
repo: nome do repositório  
title: título claro e acionável
body: conteúdo markdown estruturado
```

### Parâmetros Opcionais

```
labels: ["bug", "enhancement", "documentation", ...]
assignees: ["username1", "username2"]
milestone: número do milestone (inteiro)
```

### Diretrizes de Título

- Comece com prefixo de tipo quando útil: `[Bug]`, `[Feature]`, `[Docs]`
- Seja específico e acionável
- Mantenha sob 72 caracteres
- Exemplos:
  - `[Bug] Login falha com SSO ativado`
  - `[Feature] Adicionar suporte a modo escuro`
  - `Adicionar testes unitários para módulo de autenticação`

### Estrutura do Body

Sempre use os templates em [references/templates.md](references/templates.md). Escolha baseado no tipo de issue:

| Requisição do Usuário | Template |
|--------------|----------|
| Bug, erro, quebrado, não funcionando | Relatório de Bug |
| Feature, melhoria, adicionar, novo | Solicitação de Feature |
| Tarefa, chore, refatoração, atualização | Tarefa |

## Atualizando Issues

Use `mcp__github__update_issue` com:

```
owner, repo, issue_number (obrigatório)
title, body, state, labels, assignees, milestone (opcional - apenas campos alterados)
```

Valores de state: `open`, `closed`

## Exemplos

### Exemplo 1: Relatório de Bug

**Usuário**: "Criar uma issue de bug - a página de login quebra ao usar SSO"

**Ação**: Chame `mcp__github__create_issue` com:
```json
{
  "owner": "github",
  "repo": "awesome-copilot",
  "title": "[Bug] Página de login quebra ao usar SSO",
  "body": "## Descrição\nA página de login quebra quando usuários tentam autenticar usando SSO.\n\n## Passos para Reproduzir\n1. Navegue até a página de login\n2. Clique em 'Entrar com SSO'\n3. A página quebra\n\n## Comportamento Esperado\nA autenticação SSO deve ser concluída e redirecionar para o dashboard.\n\n## Comportamento Atual\nA página fica sem resposta e exibe um erro.\n\n## Ambiente\n- Navegador: [A preencher]\n- SO: [A preencher]\n\n## Contexto Adicional\nRelatado pelo usuário.",
  "labels": ["bug"]
}
```

### Exemplo 2: Solicitação de Feature

**Usuário**: "Criar uma solicitação de feature para modo escuro com alta prioridade"

**Ação**: Chame `mcp__github__create_issue` com:
```json
{
  "owner": "github",
  "repo": "awesome-copilot",
  "title": "[Feature] Adicionar suporte a modo escuro",
  "body": "## Resumo\nAdicionar opção de tema modo escuro para melhor experiência do usuário e acessibilidade.\n\n## Motivação\n- Reduz fadiga visual em ambientes com pouca luz\n- Cada vez mais esperado pelos usuários\n- Melhora a acessibilidade\n\n## Solução Proposta\nImplementar alternância de tema com detecção de preferência do sistema.\n\n## Critérios de Aceitação\n- [ ] Interruptor de alternância nas configurações\n- [ ] Preserva preferência do usuário\n- [ ] Respeita preferência do sistema por padrão\n- [ ] Todos os componentes da UI suportam ambos os temas\n\n## Alternativas Consideradas\nNenhuma especificada.\n\n## Contexto Adicional\nSolicitação de alta prioridade.",
  "labels": ["enhancement", "high-priority"]
}
```

## Labels Comuns

Use esses labels padrão quando aplicável:

| Label | Use Para |
|-------|---------|
| `bug` | Algo não está funcionando |
| `enhancement` | Nova feature ou melhoria |
| `documentation` | Atualizações de documentação |
| `good first issue` | Bom para iniciantes |
| `help wanted` | Atenção extra necessária |
| `question` | Mais informações solicitadas |
| `wontfix` | Não será abordado |
| `duplicate` | Já existe |
| `high-priority` | Issues urgentes |

## Dicas

- Sempre confirme o contexto do repositório antes de criar issues
- Peça informações críticas ausentes em vez de adivinhar
- Vincule issues relacionadas quando conhecidas: `Relacionado a #123`
- Para atualizações, obtenha a issue atual primeiro para preservar campos inalterados