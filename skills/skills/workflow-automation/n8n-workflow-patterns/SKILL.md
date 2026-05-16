---
name: n8n-workflow-patterns
description: "Padrões arquiteturais comprovados para construir workflows n8n."
risk: unknown
source: community
---

# Padrões de Workflow n8n

Padrões arquiteturais comprovados para construir workflows n8n.

---

## Os 5 Padrões Principais

Baseado em análise de uso real de workflows:

1. **Processamento de Webhook** (Mais Comum)
   - Receber requisições HTTP → Processar → Saída
   - Padrão: Webhook → Validar → Transformar → Responder/Notificar

2. **[Integração de API HTTP]**
   - Buscar de APIs REST → Transformar → Armazenar/Usar
   - Padrão: Trigger → HTTP Request → Transformar → Ação → Tratamento de Erro

3. **Operações de Banco de Dados**
   - Ler/Escrever/Sincronizar dados de banco
   - Padrão: Schedule → Query → Transformar → Escrever → Verificar

4. **Workflow com Agente IA**
   - Agentes de IA com ferramentas e memória
   - Padrão: Trigger → Agente IA (Model + Ferramentas + Memória) → Saída

5. **Tarefas Agendadas**
   - Workflows de automação recorrentes
   - Padrão: Schedule → Buscar → Processar → Entregar → Registrar

---

## Guia de Seleção de Padrão

### Quando usar cada padrão:

**Processamento de Webhook** - Use quando:
- Receber dados de sistemas externos
- Construir integrações (comandos Slack, envios de formulários, webhooks GitHub)
- Precisar de resposta instantânea a eventos
- Exemplo: "Receber webhook de pagamento Stripe → Atualizar banco de dados → Enviar confirmação"

**Integração de API HTTP** - Use quando:
- Buscar dados de APIs externas
- Sincronizar com serviços de terceiros
- Construir pipelines de dados
- Exemplo: "Buscar issues GitHub → Transformar → Criar tickets Jira"

**Operações de Banco de Dados** - Use quando:
- Sincronizar entre bancos de dados
- Executar queries de banco de dados em schedule
- Workflows ETL
- Exemplo: "Ler registros Postgres → Transformar → Escrever em MySQL"

**Workflow com Agente IA** - Use quando:
- Construir IA conversacional
- Precisar de IA com acesso a ferramentas
- Tarefas de raciocínio em múltiplas etapas
- Exemplo: "Chat com IA que pode pesquisar docs, consultar banco de dados, enviar emails"

**Tarefas Agendadas** - Use quando:
- Relatórios ou resumos recorrentes
- Busca periódica de dados
- Tarefas de manutenção
- Exemplo: "Diariamente: Buscar analytics → Gerar relatório → Enviar para time"

---

## Componentes Comuns de Workflow

Todos os padrões compartilham esses blocos de construção:

### 1. Triggers
- **Webhook** - Endpoint HTTP (instantâneo)
- **Schedule** - Timing baseado em Cron (periódico)
- **Manual** - Clicar para executar (teste)
- **Polling** - Verificar mudanças (intervalos)

### 2. Fontes de Dados
- **HTTP Request** - APIs REST
- **Nós de banco de dados** - Postgres, MySQL, MongoDB
- **Nós de serviço** - Slack, Google Sheets, etc.
- **Code** - JavaScript/Python customizado

### 3. Transformação
- **Set** - Mapear/transformar campos
- **Code** - Lógica complexa
- **IF/Switch** - Roteamento condicional
- **Merge** - Combinar fluxos de dados

### 4. Saídas
- **HTTP Request** - Chamar APIs
- **Database** - Escrever dados
- **Communication** - Email, Slack, Discord
- **Storage** - Arquivos, armazenamento em nuvem

### 5. Tratamento de Erro
- **Error Trigger** - Capturar erros de workflow
- **IF** - Verificar condições de erro
- **Stop and Error** - Falha explícita
- **Continue On Fail** - Configuração por nó

---

## Checklist de Criação de Workflow

Ao construir QUALQUER workflow, siga este checklist:

### Fase de Planejamento
- [ ] Identificar o padrão (webhook, API, banco de dados, IA, agendado)
- [ ] Listar nós necessários (use search_nodes)
- [ ] Entender o fluxo de dados (entrada → transformar → saída)
- [ ] Planejar estratégia de tratamento de erro

### Fase de Implementação
- [ ] Criar workflow com trigger apropriado
- [ ] Adicionar nós de fonte de dados
- [ ] Configurar autenticação/credenciais
- [ ] Adicionar nós de transformação (Set, Code, IF)
- [ ] Adicionar nós de saída/ação
- [ ] Configurar tratamento de erro

### Fase de Validação
- [ ] Validar cada configuração de nó (validate_node)
- [ ] Validar workflow completo (validate_workflow)
- [ ] Testar com dados de amostra
- [ ] Tratar casos extremos (dados vazios, erros)

### Fase de Implementação
- [ ] Revisar configurações de workflow (ordem de execução, timeout, tratamento de erro)
- [ ] Ativar workflow usando operação `activateWorkflow`
- [ ] Monitorar primeiras execuções
- [ ] Documentar propósito do workflow e fluxo de dados

---

## Padrões de Fluxo de Dados

### Fluxo Linear
```
Trigger → Transform → Action → End
```
**Use quando**: Workflows simples com caminho único

### Fluxo com Ramificações
```
Trigger → IF → [Caminho True]
             └→ [Caminho False]
```
**Use quando**: Ações diferentes baseadas em condições

### Processamento Paralelo
```
Trigger → [Branch 1] → Merge
       └→ [Branch 2] ↗
```
**Use quando**: Operações independentes que podem rodar simultaneamente

### Padrão de Loop
```
Trigger → Split in Batches → Process → Loop (até terminar)
```
**Use quando**: Processar grandes datasets em chunks

### Padrão de Tratador de Erro
```
Main Flow → [Caminho Success]
         └→ [Error Trigger → Error Handler]
```
**Use quando**: Precisar de tratamento de erro separado em workflow

---

## Armadilhas Comuns

### 1. Estrutura de Dados de Webhook
**Problema**: Não consigo acessar dados do payload do webhook

**Solução**: Dados estão aninhados sob `$json.body`
```javascript
❌ {{$json.email}}
✅ {{$json.body.email}}
```
Ver: skill n8n Expression Syntax

### 2. Múltiplos Itens de Entrada
**Problema**: Nó processa todos os itens de entrada, mas eu só quero um

**Solução**: Use modo "Execute Once" ou processe apenas o primeiro item
```javascript
{{$json[0].field}}  // Apenas o primeiro item
```

### 3. Problemas de Autenticação
**Problema**: Chamadas de API falhando com 401/403

**Solução**:
- Configurar credenciais corretamente
- Usar a seção "Credentials", não parâmetros
- Testar credenciais antes da ativação do workflow

### 4. Ordem de Execução de Nó
**Problema**: Nós executando em ordem inesperada

**Solução**: Verificar configurações de workflow → Execution Order
- v0: De cima para baixo (legado)
- v1: Baseado em conexão (recomendado)

### 5. Erros de Expression
**Problema**: Expressions aparecendo como texto literal

**Solução**: Use {{}} ao redor de expressions
- Ver skill n8n Expression Syntax para detalhes

---

## Integração com Outras Skills

Essas skills trabalham juntas com Padrões de Workflow:

**n8n MCP Tools Expert** - Use para:
- Encontrar nós para seu padrão (search_nodes)
- Entender operações de nó (get_node)
- Criar workflows (n8n_create_workflow)
- Implementar templates (n8n_deploy_template)
- Usar ai_agents_guide para guidance de padrão IA

**n8n Expression Syntax** - Use para:
- Escrever expressions em nós de transformação
- Acessar dados de webhook corretamente ({{$json.body.field}})
- Referenciar nós anteriores ({{$node["Node Name"].json.field}})

**n8n Node Configuration** - Use para:
- Configurar operações específicas para nós de padrão
- Entender requisitos específicos de nó

**n8n Validation Expert** - Use para:
- Validar estrutura de workflow
- Corrigir erros de validação
- Garantir correção de workflow antes da implementação

---

## Estatísticas de Padrão

Padrões de workflow comuns:

**Triggers Mais Comuns**:
1. Webhook - 35%
2. Schedule (tarefas periódicas) - 28%
3. Manual (teste/admin) - 22%
4. Service triggers (Slack, email, etc.) - 15%

**Transformações Mais Comuns**:
1. Set (mapeamento de campo) - 68%
2. Code (lógica customizada) - 42%
3. IF (roteamento condicional) - 38%
4. Switch (multi-condição) - 18%

**Saídas Mais Comuns**:
1. HTTP Request (APIs) - 45%
2. Slack - 32%
3. Escritas em banco de dados - 28%
4. Email - 24%

**Complexidade Média de Workflow**:
- Simples (3-5 nós): 42%
- Médio (6-10 nós): 38%
- Complexo (11+ nós): 20%

---

## Exemplos de Início Rápido

### Exemplo 1: Webhook Simples → Slack
```
1. Webhook (path: "form-submit", POST)
2. Set (mapear campos de formulário)
3. Slack (postar mensagem em #notifications)
```

### Exemplo 2: Relatório Agendado
```
1. Schedule (diariamente às 9 AM)
2. HTTP Request (buscar analytics)
3. Code (agregar dados)
4. Email (enviar relatório formatado)
5. Error Trigger → Slack (notificar em falha)
```

### Exemplo 3: Sincronização de Banco de Dados
```
1. Schedule (a cada 15 minutos)
2. Postgres (query registros novos)
3. IF (verificar se existem registros)
4. MySQL (inserir registros)
5. Postgres (atualizar timestamp de sincronização)
```

### Exemplo 4: Assistente de IA
```
1. Webhook (receber mensagem de chat)
2. AI Agent
   ├─ OpenAI Chat Model (ai_languageModel)
   ├─ HTTP Request Tool (ai_tool)
   ├─ Database Tool (ai_tool)
   └─ Window Buffer Memory (ai_memory)
3. Webhook Response (enviar resposta de IA)
```

### Exemplo 5: Integração de API
```
1. Manual Trigger (para teste)
2. HTTP Request (GET /api/users)
3. Split In Batches (processar 100 por vez)
4. Set (transformar dados de usuário)
5. Postgres (upsert usuários)
6. Loop (voltar ao passo 3 até terminar)
```

---

## Arquivos de Padrão Detalhado

Para guidance abrangente em cada padrão:

- **webhook_processing.md** - Padrões de webhook, estrutura de dados, tratamento de resposta
- **http_api_integration.md** - APIs REST, autenticação, paginação, retries
- **database_operations.md** - Queries, sincronização, transações, processamento em lote
- **ai_agent_workflow.md** - Agentes de IA, ferramentas, memória, nós langchain
- **scheduled_tasks.md** - Schedules Cron, relatórios, tarefas de manutenção

---

## Exemplos de Template Real

Da biblioteca de templates n8n:

**Template #2947**: Clima para Slack
- Padrão: Scheduled Task
- Nós: Schedule → HTTP Request (API de clima) → Set → Slack
- Complexidade: Simples (4 nós)

**Processamento de Webhook**: Padrão mais comum
- Mais comum: Envios de formulário, webhooks de pagamento, integrações de chat

**API HTTP**: Padrão comum
- Mais comum: Busca de dados, integrações de terceiros

**Operações de Banco de Dados**: Padrão comum
- Mais comum: ETL, sincronização de dados, workflows de backup

**Agentes de IA**: Crescente em uso
- Mais comum: Chatbots, geração de conteúdo, análise de dados

Use `search_templates` e `get_template` das ferramentas n8n-mcp para encontrar exemplos!

---

## Melhores Práticas

### ✅ Faça

- Comece com o padrão mais simples que resolve seu problema
- Planeje a estrutura do workflow antes de construir
- Use tratamento de erro em todos os workflows
- Teste com dados de amostra antes da ativação
- Siga o checklist de criação de workflow
- Use nomes descritivos para nós
- Documente workflows complexos (campo de notas)
- Monitore execuções de workflow após implementação

### ❌ Não Faça

- Construir workflows de uma só vez (itere! média de 56s entre edições)
- Pular validação antes da ativação
- Ignorar cenários de erro
- Usar padrões complexos quando simples funcionam
- Hardcodear credenciais em parâmetros
- Esquecer de tratar casos de dados vazios
- Misturar múltiplos padrões sem limites claros
- Implementar sem testar

---

## Resumo

**Pontos-Chave**:
1. **5 padrões principais** cobrem 90%+ dos casos de uso de workflow
2. **Processamento de webhook** é o padrão mais comum
3. Use o **checklist de criação de workflow** para cada workflow
4. **Planejar padrão** → **Selecionar nós** → **Construir** → **Validar** → **Implementar**
5. Integre com outras skills para desenvolvimento completo de workflow

**Próximos Passos**:
1. Identificar padrão de seu caso de uso
2. Ler o arquivo de padrão detalhado
3. Usar n8n MCP Tools Expert para encontrar nós
4. Seguir o checklist de criação de workflow
5. Usar n8n Validation Expert para validar

**Skills Relacionadas**:
- n8n MCP Tools Expert - Encontrar e configurar nós
- n8n Expression Syntax - Escrever expressions corretamente
- n8n Validation Expert - Validar e corrigir erros
- n8n Node Configuration - Configurar operações específicas