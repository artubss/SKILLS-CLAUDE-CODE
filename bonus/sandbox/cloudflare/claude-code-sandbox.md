# Cloudflare Claude Code Sandbox

Execute Claude Code em um ambiente isolado de sandbox do Cloudflare Workers com execução de código alimentada por IA.

## Descrição

Este componente configura a integração do Cloudflare Sandbox SDK para executar Claude Code em um ambiente cloud seguro e isolado. Construído sobre sandboxes baseados em container do Cloudflare com Durable Objects para execução persistente.

## Funcionalidades

- **Execução Isolada**: Execute Claude Code em sandboxes seguros do Cloudflare Workers
- **Executor de Código IA**: Transforme linguagem natural em código Python/Node.js executável
- **Streaming em Tempo Real**: Transmita a saída da execução conforme acontece
- **Armazenamento Persistente**: Use Durable Objects para sessões de sandbox com estado
- **Distribuição Global**: Aproveite a rede de borda do Cloudflare para baixa latência
- **Instalação de Componentes**: Instale automaticamente agentes e comandos no sandbox

## Requisitos

- Conta Cloudflare (plano pago do Workers para Durable Objects)
- Chave de API Anthropic
- Node.js 16.17.0+
- Docker (para desenvolvimento local)
- Wrangler CLI

## Arquitetura

```
┌─────────────────────────────────────────────────────┐
│  Requisição do Usuário (Linguagem Natural)          │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│  Cloudflare Worker (Endpoint de API)                │
│  • POST /execute                                    │
│  • Recebe pergunta/prompt                           │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│  Claude IA (via SDK Anthropic)                      │
│  • Gera código Python/TypeScript                    │
│  • Retorna implementação executável                 │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│  Sandbox Cloudflare (Durable Object)                │
│  • Execução em container isolado                    │
│  • Runtime Python/Node.js                          │
│  • Acesso ao sistema de arquivos                    │
│  • Streaming de saída em tempo real                 │
└──────────────────┬──────────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────────┐
│  Resultados                                         │
│  • Código gerado                                    │
│  • Saída da execução                                │
│  • Mensagens de erro (se houver)                    │
└─────────────────────────────────────────────────────┘
```

## Uso

```bash
# Execute um prompt em sandbox do Cloudflare
npx claude-code-templates@latest --sandbox cloudflare --prompt "Calcule o 10º número de Fibonacci"

# Passe chaves de API diretamente
npx claude-code-templates@latest --sandbox cloudflare \
  --anthropic-api-key sua_chave_anthropic \
  --prompt "Crie um web scraper"

# Instale componentes e execute
npx claude-code-templates@latest --sandbox cloudflare \
  --agent frontend-developer \
  --command setup-react \
  --anthropic-api-key sua_chave_anthropic \
  --prompt "Crie um aplicativo de lista de tarefas moderno"

# Implante seu próprio sandbox do Cloudflare Worker
cd .claude/sandbox/cloudflare
npm install
npx wrangler secret put ANTHROPIC_API_KEY
npx wrangler deploy
```

## Configuração de Ambiente

O componente cria:
- `.claude/sandbox/cloudflare/src/index.ts` - Worker com lógica de sandbox
- `.claude/sandbox/cloudflare/wrangler.toml` - Configuração do Cloudflare
- `.claude/sandbox/cloudflare/package.json` - Dependências Node.js
- `.claude/sandbox/cloudflare/launcher.ts` - Script launcher TypeScript
- `.claude/sandbox/cloudflare/monitor.ts` - Ferramenta de monitoramento em tempo real

## Configuração de Chave de API

### Opção 1: Parâmetros CLI (Recomendado)
```bash
npx claude-code-templates@latest --sandbox cloudflare \
  --anthropic-api-key sua_chave_api_anthropic \
  --prompt "Seu prompt aqui"
```

### Opção 2: Segredos Wrangler
```bash
cd .claude/sandbox/cloudflare
npx wrangler secret put ANTHROPIC_API_KEY
# Cole sua chave de API quando solicitado
```

### Opção 3: Variáveis de Ambiente
```bash
export ANTHROPIC_API_KEY=sua_chave_api_anthropic_aqui

# Ou crie arquivo .dev.vars:
ANTHROPIC_API_KEY=sua_chave_api_anthropic_aqui
```

**Nota**: Segredos Wrangler são necessários para implantação em produção. Parâmetros CLI funcionam apenas para execução local.

## Como Funciona

1. Usuário envia requisição em linguagem natural (ex: "Qual é o fatorial de 5?")
2. Cloudflare Worker recebe requisição via POST /execute
3. Claude gera código Python/TypeScript executável via API Anthropic
4. Código é escrito no sistema de arquivos do sandbox
5. Sandbox executa código em container isolado
6. Resultados retornam em tempo real via streaming
7. Worker retorna código e saída da execução

## Implantação

### Desenvolvimento Local
```bash
cd .claude/sandbox/cloudflare
npm install
npm run dev

# Teste localmente
curl -X POST http://localhost:8787/execute \
  -H "Content-Type: application/json" \
  -d '{"question": "O que é 2^10?"}'
```

### Implantação em Produção
```bash
# Defina segredo da chave de API
npx wrangler secret put ANTHROPIC_API_KEY

# Implante no Cloudflare Workers
npx wrangler deploy

# Aguarde 2-3 minutos para provisionamento do container
npx wrangler containers list

# Teste a implantação
curl -X POST https://seu-worker.seu-subdominio.workers.dev/execute \
  -H "Content-Type: application/json" \
  -d '{"question": "Calcule o fatorial de 5"}'
```

## Benefícios de Segurança

- **Isolamento de Container**: Cada execução é executada em container Cloudflare isolado
- **Sem Acesso Local**: Sandboxes não têm acesso ao seu sistema local
- **Limites de Recurso**: Restrições automáticas de tempo de CPU e memória
- **Execução Temporária**: Containers destruídos após execução
- **Segurança de Borda**: Infraestrutura de segurança do Cloudflare integrada

## Funcionalidades Avançadas

### API do Interpretador de Código
```typescript
// Use interpretador de código integrado em vez de exec
import { getCodeInterpreter } from '@cloudflare/sandbox';

const interpreter = getCodeInterpreter(env.Sandbox, 'user-id');
const result = await interpreter.notebook.execCell('print(2**10)');
```

### Streaming de Saída
```typescript
// Transmita resultados de execução em tempo real
return new Response(
  new ReadableStream({
    async start(controller) {
      const result = await sandbox.exec('python script.py', {
        onStdout: (data) => controller.enqueue(data),
        onStderr: (data) => controller.enqueue(data)
      });
      controller.close();
    }
  })
);
```

### Sessões Persistentes
```typescript
// Mantenha estado do sandbox entre requisições
const sandbox = getSandbox(env.Sandbox, userId);
await sandbox.writeFile('/data/state.json', JSON.stringify(state));
// Depois...
const state = await sandbox.readFile('/data/state.json');
```

## Exemplos

```bash
# Computação matemática
npx claude-code-templates@latest --sandbox cloudflare \
  --prompt "Calcule o 100º número de Fibonacci"

# Análise de dados
npx claude-code-templates@latest --sandbox cloudflare \
  --prompt "Qual é a média de [10, 20, 30, 40, 50]?"

# Manipulação de string
npx claude-code-templates@latest --sandbox cloudflare \
  --prompt "Inverta a string 'Hello World'"

# Desenvolvimento web
npx claude-code-templates@latest --sandbox cloudflare \
  --agent frontend-developer \
  --prompt "Crie uma barra de navegação responsiva"
```

## Comparação com E2B

| Funcionalidade | Sandbox Cloudflare | E2B Sandbox |
|---|---|---|
| **Provedor** | Cloudflare Workers | E2B.dev |
| **Infraestrutura** | Rede de Borda Cloudflare | VMs em Cloud |
| **Preço** | R$ 25/mês aprox (Workers Pago) | Baseado em uso |
| **Cold Start** | ~100ms | ~2-3 segundos |
| **Duração Máxima** | 30 segundos (Workers) | Até horas |
| **Linguagens** | Python, Node.js | Ambiente Linux completo |
| **Global** | Sim (rede de borda) | Região única |
| **Melhor Para** | Tarefas rápidas e leves | Operações de longa duração |

## Resolução de Problemas

### Container Não Pronto
```bash
# Após primeira implantação, aguarde 2-3 minutos
npx wrangler containers list

# Verifique status do container
npx wrangler tail
```

### Problemas de Chave de API
```bash
# Verifique se segredo está definido
npx wrangler secret list

# Atualize segredo
npx wrangler secret put ANTHROPIC_API_KEY
```

### Problemas de Desenvolvimento Local
```bash
# Garanta que Docker está rodando
docker ps

# Limpe cache do wrangler
rm -rf .wrangler

# Reinstale dependências
rm -rf node_modules package-lock.json
npm install
```

## Dicas de Desempenho

1. **Use Code Interpreter API** para melhor desempenho com Python
2. **Implemente caching** para padrões de código frequentemente usados
3. **Transmita saída** para operações de longa duração
4. **Use Durable Objects** para persistência de sessão
5. **Implante em múltiplas regiões** (automático com Workers)

## Informações do Template

- **Provedor**: Cloudflare Workers + Sandbox SDK
- **Runtime**: V8 isolates com sandboxes baseados em container
- **Linguagens**: Python 3.x, Node.js
- **Timeout**: 30 segundos (Workers), configurável para Durable Objects
- **Memória**: 128MB padrão
- **Armazenamento**: Efêmero (use Durable Objects para persistência)

## Recursos

- [Documentação Cloudflare Sandbox SDK](https://developers.cloudflare.com/sandbox/)
- [Documentação Workers](https://developers.cloudflare.com/workers/)
- [Guia Durable Objects](https://developers.cloudflare.com/durable-objects/)
- [Referência Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/)
- [Documentação Anthropic API](https://docs.anthropic.com/)

## Próximos Passos

Após a instalação:
1. Configure conta Cloudflare e obtenha credenciais de API
2. Instale Wrangler CLI: `npm install -g wrangler`
3. Configure segredos: `npx wrangler secret put ANTHROPIC_API_KEY`
4. Implante seu worker: `npx wrangler deploy`
5. Teste com requisições de exemplo
6. Customize configuração de sandbox para seu caso de uso

## Licença

Usa Cloudflare Sandbox SDK (código aberto) e requer plano pago do Cloudflare Workers (R$ 25/mês aprox) para suporte a Durable Objects.