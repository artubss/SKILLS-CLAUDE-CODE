---
name: mcp-builder
description: Guia para criar servidores MCP (Model Context Protocol) de alta qualidade que permitem que LLMs interajam com serviços externos por meio de ferramentas bem projetadas. Use ao criar servidores MCP para integrar APIs externas ou serviços, seja em Python (FastMCP) ou Node/TypeScript (MCP SDK).
license: Complete terms in LICENSE.txt
---

# Guia de Desenvolvimento de Servidor MCP

## Visão Geral

Crie servidores MCP (Model Context Protocol) que permitam que LLMs interajam com serviços externos através de ferramentas bem projetadas. A qualidade de um servidor MCP é medida por quão bem ele permite que LLMs realizem tarefas do mundo real.

---

# Processo

## 🚀 Fluxo de Trabalho de Alto Nível

Criar um servidor MCP de alta qualidade envolve quatro fases principais:

### Fase 1: Pesquisa Profunda e Planejamento

#### 1.1 Entender o Design Moderno de MCP

**Cobertura de API vs. Ferramentas de Workflow:**
Equilibre a cobertura abrangente de endpoints de API com ferramentas de workflow especializadas. Ferramentas de workflow podem ser mais convenientes para tarefas específicas, enquanto a cobertura abrangente oferece aos agentes flexibilidade para compor operações. O desempenho varia por cliente — alguns clientes se beneficiam da execução de código que combina ferramentas básicas, enquanto outros funcionam melhor com workflows de nível superior. Quando em dúvida, priorize a cobertura abrangente de API.

**Nomenclatura e Descoberta de Ferramentas:**
Nomes de ferramentas claros e descritivos ajudam agentes a encontrar a ferramenta certa rapidamente. Use prefixos consistentes (ex: `github_create_issue`, `github_list_repos`) e nomenclatura orientada por ação.

**Gerenciamento de Contexto:**
Agentes se beneficiam de descrições concisas de ferramentas e da capacidade de filtrar/paginar resultados. Projete ferramentas que retornem dados focados e relevantes. Alguns clientes suportam execução de código, o que pode ajudar agentes a filtrar e processar dados eficientemente.

**Mensagens de Erro Acionáveis:**
Mensagens de erro devem orientar agentes em direção a soluções com sugestões específicas e próximos passos.

#### 1.2 Estudar a Documentação do Protocolo MCP

**Navegue pela especificação do MCP:**

Comece com o mapa do site para encontrar páginas relevantes: `https://modelcontextprotocol.io/sitemap.xml`

Em seguida, busque páginas específicas com sufixo `.md` para formato markdown (ex: `https://modelcontextprotocol.io/specification/draft.md`).

Páginas-chave a revisar:
- Visão geral da especificação e arquitetura
- Mecanismos de transporte (HTTP streaming, stdio)
- Definições de tool, resource e prompt

#### 1.3 Estudar a Documentação do Framework

**Stack recomendado:**
- **Linguagem**: TypeScript (suporte SDK de alta qualidade e boa compatibilidade em muitos ambientes de execução, ex: MCPB. Além disso, modelos de IA são bons em gerar código TypeScript, se beneficiando de seu uso amplo, tipagem estática e boas ferramentas de linting)
- **Transporte**: HTTP streaming para servidores remotos, usando JSON sem estado (mais simples de escalar e manter, em oposição a sessões com estado e respostas em streaming). stdio para servidores locais.

**Carregue a documentação do framework:**

- **Melhores Práticas de MCP**: [📋 Ver Melhores Práticas](./reference/mcp_best_practices.md) - Diretrizes principais

**Para TypeScript (recomendado):**
- **TypeScript SDK**: Use WebFetch para carregar `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`
- [⚡ Guia TypeScript](./reference/node_mcp_server.md) - Padrões e exemplos de TypeScript

**Para Python:**
- **Python SDK**: Use WebFetch para carregar `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`
- [🐍 Guia Python](./reference/python_mcp_server.md) - Padrões e exemplos de Python

#### 1.4 Planejar Sua Implementação

**Entender a API:**
Revise a documentação da API do serviço para identificar endpoints-chave, requisitos de autenticação e modelos de dados. Use busca na web e WebFetch conforme necessário.

**Seleção de Ferramentas:**
Priorize cobertura abrangente de API. Liste endpoints a implementar, começando pelas operações mais comuns.

---

### Fase 2: Implementação

#### 2.1 Configurar Estrutura do Projeto

Veja guias específicos da linguagem para configuração do projeto:
- [⚡ Guia TypeScript](./reference/node_mcp_server.md) - Estrutura do projeto, package.json, tsconfig.json
- [🐍 Guia Python](./reference/python_mcp_server.md) - Organização de módulos, dependências

#### 2.2 Implementar Infraestrutura Principal

Crie utilitários compartilhados:
- Cliente de API com autenticação
- Helpers de tratamento de erros
- Formatação de resposta (JSON/Markdown)
- Suporte a paginação

#### 2.3 Implementar Ferramentas

Para cada ferramenta:

**Input Schema:**
- Use Zod (TypeScript) ou Pydantic (Python)
- Inclua restrições e descrições claras
- Adicione exemplos nas descrições de campos

**Output Schema:**
- Defina `outputSchema` quando possível para dados estruturados
- Use `structuredContent` em respostas de ferramentas (feature do TypeScript SDK)
- Ajuda clientes a entender e processar saídas de ferramentas

**Descrição da Ferramenta:**
- Resumo conciso da funcionalidade
- Descrições de parâmetros
- Schema de tipo de retorno

**Implementação:**
- Async/await para operações de I/O
- Tratamento adequado de erros com mensagens acionáveis
- Suporte a paginação quando aplicável
- Retorne conteúdo em texto e dados estruturados quando usar SDKs modernos

**Anotações:**
- `readOnlyHint`: true/false
- `destructiveHint`: true/false
- `idempotentHint`: true/false
- `openWorldHint`: true/false

---

### Fase 3: Revisão e Teste

#### 3.1 Qualidade do Código

Revise para:
- Sem código duplicado (princípio DRY)
- Tratamento de erro consistente
- Cobertura de tipo completa
- Descrições de ferramentas claras

#### 3.2 Build e Teste

**TypeScript:**
- Execute `npm run build` para verificar compilação
- Teste com MCP Inspector: `npx @modelcontextprotocol/inspector`

**Python:**
- Verifique sintaxe: `python -m py_compile your_server.py`
- Teste com MCP Inspector

Veja guias específicos da linguagem para abordagens de teste detalhadas e checklists de qualidade.

---

### Fase 4: Criar Avaliações

Após implementar seu servidor MCP, crie avaliações abrangentes para testar sua efetividade.

**Carregue [✅ Guia de Avaliação](./reference/evaluation.md) para diretrizes completas de avaliação.**

#### 4.1 Entender o Propósito da Avaliação

Use avaliações para testar se LLMs podem usar efetivamente seu servidor MCP para responder perguntas realistas e complexas.

#### 4.2 Criar 10 Questões de Avaliação

Para criar avaliações efetivas, siga o processo descrito no guia de avaliação:

1. **Inspeção de Ferramentas**: Liste ferramentas disponíveis e entenda suas capacidades
2. **Exploração de Conteúdo**: Use operações SOMENTE LEITURA para explorar dados disponíveis
3. **Geração de Questões**: Crie 10 questões complexas e realistas
4. **Verificação de Respostas**: Resolva cada questão você mesmo para verificar respostas

#### 4.3 Requisitos de Avaliação

Garanta que cada questão seja:
- **Independente**: Não dependente de outras questões
- **Somente leitura**: Apenas operações não-destrutivas necessárias
- **Complexa**: Requerendo múltiplas chamadas de ferramentas e exploração profunda
- **Realista**: Baseada em casos de uso reais que pessoas se importariam
- **Verificável**: Resposta única e clara que pode ser verificada por comparação de string
- **Estável**: A resposta não mudará ao longo do tempo

#### 4.4 Formato de Saída

Crie um arquivo XML com esta estrutura:

```xml
<evaluation>
  <qa_pair>
    <question>Find discussions about AI model launches with animal codenames. One model needed a specific safety designation that uses the format ASL-X. What number X was being determined for the model named after a spotted wild cat?</question>
    <answer>3</answer>
  </qa_pair>
<!-- More qa_pairs... -->
</evaluation>
```

---

# Arquivos de Referência

## 📚 Biblioteca de Documentação

Carregue estes recursos conforme necessário durante o desenvolvimento:

### Documentação Principal de MCP (Carregue Primeiro)
- **Protocolo MCP**: Comece com mapa do site em `https://modelcontextprotocol.io/sitemap.xml`, depois busque páginas específicas com sufixo `.md`
- [📋 Melhores Práticas de MCP](./reference/mcp_best_practices.md) - Diretrizes MCP universais incluindo:
  - Convenções de nomenclatura para servidor e ferramenta
  - Diretrizes de formato de resposta (JSON vs Markdown)
  - Melhores práticas de paginação
  - Seleção de transporte (HTTP streaming vs stdio)
  - Padrões de segurança e tratamento de erros

### Documentação SDK (Carregue Durante Fase 1/2)
- **Python SDK**: Busque de `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`
- **TypeScript SDK**: Busque de `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`

### Guias de Implementação Específicos da Linguagem (Carregue Durante Fase 2)
- [🐍 Guia de Implementação Python](./reference/python_mcp_server.md) - Guia completo Python/FastMCP com:
  - Padrões de inicialização de servidor
  - Exemplos de modelos Pydantic
  - Registro de ferramenta com `@mcp.tool`
  - Exemplos funcionais completos
  - Checklist de qualidade

- [⚡ Guia de Implementação TypeScript](./reference/node_mcp_server.md) - Guia TypeScript completo com:
  - Estrutura de projeto
  - Padrões de schema Zod
  - Registro de ferramenta com `server.registerTool`
  - Exemplos funcionais completos
  - Checklist de qualidade

### Guia de Avaliação (Carregue Durante Fase 4)
- [✅ Guia de Avaliação](./reference/evaluation.md) - Guia completo de criação de avaliação com:
  - Diretrizes de criação de questões
  - Estratégias de verificação de respostas
  - Especificações de formato XML
  - Exemplos de questões e respostas
  - Executando uma avaliação com scripts fornecidos