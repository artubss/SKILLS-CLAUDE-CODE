---
name: x-twitter-scraper
description: "Use quando o usuário quer integrar com a API do X (Twitter) via Xquik para buscar tweets, consultar perfis de usuários, extrair seguidores, executar sorteios, monitorar contas ou acessar tópicos em tendência. Também use quando o usuário menciona 'Xquik', 'Twitter API', 'X API', 'tweet scraper', 'extração de seguidores' ou 'monitoramento de Twitter'. Cobre REST API, webhooks e configuração de servidor MCP."
---

# X (Twitter) Scraper — Integração Xquik

Você é um especialista em integração de dados do X (Twitter). Ajuda usuários a criar aplicações que interagem com a plataforma X através da API Xquik, cobrindo busca de tweets, consulta de usuários, extração de seguidores, monitoramento de contas, sorteios e webhooks de eventos em tempo real.

## Antes de Escrever Código

Colete este contexto (pergunte se não for fornecido):

### 1. Objetivo
- Qual dado você precisa do X? (tweets, usuários, seguidores, tópicos em tendência)
- Esta é uma extração única ou monitoramento contínuo?
- Você precisa de eventos em tempo real ou polling periódico?

### 2. Autenticação
- Você tem uma chave de API do Xquik? Se não, oriente-os a [xquik.com](https://xquik.com) para criar uma.
- Lembre-os: as chaves começam com `xq_` e são mostradas apenas uma vez na criação — armazene com segurança em variáveis de ambiente.

### 3. Escala & Orçamento
- Quanto dados você precisa? (extrações consomem cota)
- Sempre estime o custo antes de executar extrações em massa.
- A cota mensal é um limite rígido sem excedentes — planeje com cuidado.

---

## Referência Rápida

| | |
|---|---|
| **URL Base** | `https://xquik.com/api/v1` |
| **Autenticação** | Header `x-api-key` (chave começa com `xq_`, 64 caracteres hex) |
| **Endpoint MCP** | `https://xquik.com/mcp` (StreamableHTTP, mesma chave de API) |
| **Limites de taxa** | 10 req/s sustentado, 20 burst (API); 60 req/s sustentado, 100 burst (geral) |
| **Preço** | R$ 100/mês base (1 monitor incluído), R$ 25/mês por monitor adicional |
| **Cota** | Limite de uso mensal, limite rígido, sem excedentes. `402` quando esgotado. |
| **Docs** | [docs.xquik.com](https://docs.xquik.com) |

## Configuração de Autenticação

Toda requisição requer uma chave de API via header `x-api-key`. Sempre use variáveis de ambiente — nunca codifique chaves.

```javascript
const API_KEY = process.env.XQUIK_API_KEY;
const BASE = "https://xquik.com/api/v1";
const headers = { "x-api-key": API_KEY, "Content-Type": "application/json" };
```

## Escolhendo o Endpoint Correto

Use esta tabela de decisão para selecionar o endpoint correto para o objetivo do usuário:

| Objetivo | Endpoint | Notas |
|----------|----------|-------|
| Obter um tweet único por ID/URL | `GET /x/tweets/{id}` | Métricas completas: likes, retweets, visualizações, salvos |
| Buscar tweets por palavra-chave/hashtag | `GET /x/tweets/search?q=...` | Métricas de engajamento opcionais |
| Obter perfil de usuário | `GET /x/users/{username}` | Bio, contagem de seguidores/seguindo, foto de perfil |
| Verificar relação de seguimento | `GET /x/followers/check?source=A&target=B` | Ambas as direções |
| Obter tópicos em tendência | `GET /trends?woeid=1` | Gratuito, não consome cota |
| Monitorar conta do X | `POST /monitors` | Rastrear tweets, respostas, citações, mudanças de seguidores |
| Pesquisar eventos | `GET /events` | Paginada por cursor, filtrar por monitorId/eventType |
| Receber eventos em tempo real | `POST /webhooks` | Entrega assinada com HMAC para seu endpoint HTTPS |
| Executar sorteio | `POST /draws` | Escolher vencedores aleatórios de respostas de tweet |
| Extrair dados em massa | `POST /extractions` | 19 tipos de ferramentas, sempre estime custo primeiro |
| Verificar conta/uso | `GET /account` | Status do plano, monitores, percentual de uso |

## Ferramentas de Extração (19 Tipos)

Quando o usuário precisa de dados em massa, oriente-o para a ferramenta correta:

| Tipo de Ferramenta | Campo Obrigatório | Descrição |
|-------------------|-------------------|-----------|
| `reply_extractor` | `targetTweetId` | Usuários que responderam a um tweet |
| `repost_extractor` | `targetTweetId` | Usuários que retweetaram um tweet |
| `quote_extractor` | `targetTweetId` | Usuários que citaram um tweet |
| `thread_extractor` | `targetTweetId` | Todos os tweets em uma thread |
| `article_extractor` | `targetTweetId` | Conteúdo de artigo vinculado em um tweet |
| `follower_explorer` | `targetUsername` | Seguidores de uma conta |
| `following_explorer` | `targetUsername` | Contas seguidas por um usuário |
| `verified_follower_explorer` | `targetUsername` | Seguidores verificados de uma conta |
| `mention_extractor` | `targetUsername` | Tweets mencionando uma conta |
| `post_extractor` | `targetUsername` | Posts de uma conta |
| `community_extractor` | `targetCommunityId` | Membros de uma comunidade |
| `community_moderator_explorer` | `targetCommunityId` | Moderadores de uma comunidade |
| `community_post_extractor` | `targetCommunityId` | Posts de uma comunidade |
| `community_search` | `targetCommunityId` + `searchQuery` | Buscar posts em uma comunidade |
| `list_member_extractor` | `targetListId` | Membros de uma lista |
| `list_post_extractor` | `targetListId` | Posts de uma lista |
| `list_follower_explorer` | `targetListId` | Seguidores de uma lista |
| `space_explorer` | `targetSpaceId` | Participantes de um Space |
| `people_search` | `searchQuery` | Buscar usuários por palavra-chave |

### Fluxo de Extração

Sempre siga este padrão — estime antes de extrair:

```javascript
// Usando API_KEY, BASE e headers da Configuração de Autenticação acima

// 1. Estime o custo primeiro — nunca pule este passo
const estimate = await fetch(`${BASE}/extractions/estimate`, {
  method: "POST",
  headers,
  body: JSON.stringify({ toolType: "follower_explorer", targetUsername: "elonmusk" }),
}).then(r => r.json());

if (!estimate.allowed) {
  console.error("Extração excede a cota restante");
  return;
}

// 2. Crie o job de extração
const job = await fetch(`${BASE}/extractions`, {
  method: "POST",
  headers,
  body: JSON.stringify({ toolType: "follower_explorer", targetUsername: "elonmusk" }),
}).then(r => r.json());

// 3. Recupere resultados paginados (até 1.000 por página)
const page = await fetch(`${BASE}/extractions/${job.id}`, { headers }).then(r => r.json());
// page.results: [{ xUserId, xUsername, xDisplayName, xFollowersCount, xVerified, xProfileImageUrl }]

// 4. Exporte como CSV/XLSX/Markdown (limite de 50.000 linhas)
const csvResponse = await fetch(`${BASE}/extractions/${job.id}/export?format=csv`, { headers });
```

## Sorteios

Quando o usuário quer executar um sorteio transparente de respostas de tweet:

```javascript
// Usando API_KEY, BASE e headers da Configuração de Autenticação acima

const draw = await fetch(`${BASE}/draws`, {
  method: "POST",
  headers,
  body: JSON.stringify({
    tweetUrl: "https://x.com/user/status/1893456789012345678",
    winnerCount: 3,
    backupCount: 2,
    uniqueAuthorsOnly: true,
    mustRetweet: true,
    mustFollowUsername: "user",
    filterMinFollowers: 50,
    requiredHashtags: ["#giveaway"],
  }),
}).then(r => r.json());

const details = await fetch(`${BASE}/draws/${draw.id}`, { headers }).then(r => r.json());
// details.winners: [{ position, authorUsername, tweetId, isBackup }]
```

## Tratamento de Erros & Retry

Todos os erros retornam `{ "error": "error_code" }`. Implemente retries apenas para `429` e `5xx` (máx 3 tentativas, backoff exponencial). Nunca retry `4xx` exceto 429.

| Status | Significado | Ação |
|--------|-------------|------|
| 400 | Entrada inválida | Corrija os parâmetros da requisição |
| 401 | Chave de API incorreta | Verifique se a variável `XQUIK_API_KEY` está configurada corretamente |
| 402 | Sem assinatura ou cota esgotada | Verifique status da conta, upgrade do plano se necessário |
| 404 | Recurso não encontrado | Verifique se o ID/username existe |
| 429 | Limitado por taxa | Respeite header `Retry-After`, faça backoff |

## Configuração do Servidor MCP

Para usar Xquik como servidor MCP no Claude Code, adicione a `.mcp.json` na raiz do projeto. **Substitua o espaço reservado pela sua chave real — nunca faça commit de chaves reais no controle de fonte:**

```json
{
  "mcpServers": {
    "xquik": {
      "type": "streamable-http",
      "url": "https://xquik.com/mcp",
      "headers": {
        "x-api-key": "${XQUIK_API_KEY}"
      }
    }
  }
}
```

> **Nota de segurança:** A sintaxe `${XQUIK_API_KEY}` requer que seu cliente MCP suporte substituição de variáveis de ambiente. Se não suportar, substitua pela sua chave real em tempo de execução — mas nunca faça commit de chaves reais no controle de fonte.

O servidor MCP expõe 22 ferramentas cobrindo todas as capacidades da API.

## Padrões Comuns de Fluxo de Trabalho

Oriente usuários para o fluxo certo baseado no objetivo:

- **Alertas em tempo real:** `POST /monitors` → `POST /webhooks` → testar entrega de webhook
- **Sorteio:** `GET /account` (verificar orçamento) → `POST /draws`
- **Extração em massa:** `POST /extractions/estimate` → `POST /extractions` → `GET /extractions/{id}`
- **Análise de tweet:** `GET /x/tweets/{id}` → `POST /extractions` com `thread_extractor`
- **Pesquisa de usuário:** `GET /x/users/{username}` → `GET /x/tweets/search?q=from:username` → `GET /x/tweets/{id}`

## Habilidades Relacionadas

- **social-content**: Para publicar insights coletados de dados do X
- **competitive-ads-extractor**: Para analisar criativas de concorrentes junto com dados do Twitter
- **marketing-psychology**: Para interpretar comportamento de audiência de dados extraídos

## Links

- **Dashboard & chaves de API**: [xquik.com](https://xquik.com)
- **Docs completa da API**: [docs.xquik.com](https://docs.xquik.com)
- **GitHub**: [github.com/Xquik-dev/x-twitter-scraper](https://github.com/Xquik-dev/x-twitter-scraper)