# Guia de Início Rápido do Cloudflare Sandbox

Coloque seu Cloudflare Claude Code Sandbox rodando em menos de 5 minutos.

## Checklist de Pré-requisitos

- [ ] Conta Cloudflare (inscreva-se em https://dash.cloudflare.com/sign-up)
- [ ] Chave de API da Anthropic (obtenha em https://console.anthropic.com/)
- [ ] Node.js 16.17.0+ instalado
- [ ] Docker instalado e em execução (para desenvolvimento local)

## Opção 1: Deploy em Produção (Mais Rápido)

Perfeito se você quer pular testes locais e fazer deploy direto.

### Passo 1: Instalar Dependências
```bash
cd .claude/sandbox/cloudflare
npm install
```

### Passo 2: Configurar Chave de API
```bash
npx wrangler secret put ANTHROPIC_API_KEY
# Cole sua chave de API da Anthropic quando solicitado
```

### Passo 3: Fazer Deploy
```bash
npx wrangler deploy
```

### Passo 4: Aguardar Provisionamento do Container
```bash
# Aguarde 2-3 minutos, depois verifique:
npx wrangler containers list
# Você deverá ver: ✓ Container ready
```

### Passo 5: Testar Seu Deploy
```bash
# Obtenha a URL do seu worker do output do deploy, depois:
curl -X POST https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev/execute \
  -H "Content-Type: application/json" \
  -d '{"question": "What is the 10th Fibonacci number?"}'
```

Resposta esperada:
```json
{
  "success": true,
  "question": "What is the 10th Fibonacci number?",
  "code": "def fibonacci(n):\n    ...",
  "output": "55\n",
  "error": "",
  "executionTime": 1234
}
```

**Pronto!** Seu sandbox está ativo na borda da rede.

---

## Opção 2: Desenvolvimento Local Primeiro

Perfeito se você quer testar localmente antes de fazer deploy.

### Passo 1: Instalar Dependências
```bash
cd .claude/sandbox/cloudflare
npm install
```

### Passo 2: Criar Arquivo de Ambiente Local
```bash
cp .dev.vars.example .dev.vars
# Edite .dev.vars e adicione sua chave de API da Anthropic
```

### Passo 3: Iniciar Docker
```bash
# macOS: Abra Docker Desktop
# Linux: sudo systemctl start docker
# Windows: Inicie Docker Desktop

# Verifique se Docker está em execução:
docker ps
```

### Passo 4: Iniciar Servidor de Desenvolvimento
```bash
npm run dev
```

Aguarde por:
```
⛅️ wrangler 3.78.12
-------------------
⎔ Starting local server...
[wrangler:inf] Ready on http://localhost:8787
```

### Passo 5: Testar Localmente
```bash
# Em um novo terminal:
curl -X POST http://localhost:8787/execute \
  -H "Content-Type: application/json" \
  -d '{"question": "Calculate factorial of 5"}'
```

### Passo 6: Fazer Deploy Quando Estiver Pronto
```bash
# Pare o servidor de desenvolvimento (Ctrl+C)
npx wrangler secret put ANTHROPIC_API_KEY
npx wrangler deploy
```

**Pronto!** Você testou localmente e fez deploy.

---

## Opção 3: Usar as Ferramentas de CLI

Perfeito se você prefere interação por linha de comando.

### Passo 1: Configuração (mesmo que acima)
```bash
cd .claude/sandbox/cloudflare
npm install
npx wrangler secret put ANTHROPIC_API_KEY
npx wrangler deploy
```

### Passo 2: Usar o Launcher
```bash
# Executar um prompt
node launcher.ts "What is 2 to the power of 10?" \
  "" \
  your_anthropic_key \
  https://your-worker.workers.dev
```

### Passo 3: Usar o Monitor (para depuração)
```bash
# Obter métricas detalhadas de execução
node monitor.ts "Calculate factorial of 5" \
  your_anthropic_key \
  https://your-worker.workers.dev
```

**Pronto!** Você está usando as ferramentas de CLI.

---

## Problemas Comuns & Soluções Rápidas

### "Container not ready"
**Solução**: Aguarde 2-3 minutos após o primeiro deploy
```bash
npx wrangler containers list
```

### "Docker daemon is not running"
**Solução**: Inicie Docker Desktop ou o serviço Docker
```bash
docker ps  # Deverá listar containers
```

### "ANTHROPIC_API_KEY not configured"
**Solução**: Configure o secret
```bash
# Produção:
npx wrangler secret put ANTHROPIC_API_KEY

# Local (.dev.vars):
echo "ANTHROPIC_API_KEY=sk-ant-your-key" > .dev.vars
```

### "Worker not found"
**Solução**: Faça deploy do worker
```bash
npx wrangler deploy
```

### "Execution timeout"
**Solução**: Aumente o timeout na requisição
```json
{
  "question": "Sua pergunta",
  "timeout": 60000
}
```

---

## Próximos Passos

### 1. Personalizar Seu Worker
Edite `src/index.ts` para adicionar lógica customizada:
- Adicionar autenticação
- Implementar rate limiting
- Adicionar tratamento de erros customizado
- Criar endpoints especializados

### 2. Adicionar Monitoramento
```bash
# Assistir logs em tempo real
npx wrangler tail

# Usar a ferramenta monitor
node monitor.ts "seu prompt" sua_chave_api
```

### 3. Testar Diferentes Linguagens
```bash
# Python (padrão)
curl -X POST https://your-worker.workers.dev/execute \
  -d '{"question": "Fibonacci", "language": "python"}'

# JavaScript
curl -X POST https://your-worker.workers.dev/execute \
  -d '{"question": "Fibonacci", "language": "javascript"}'
```

### 4. Integrar com Sua Aplicação
```javascript
// Integração com frontend
const response = await fetch('https://your-worker.workers.dev/execute', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ question: 'Calculate factorial of 5' })
});

const result = await response.json();
console.log(result.output);
```

### 5. Ativar Recursos Avançados
Veja o arquivo principal [README.md](./README.md) para:
- Streaming de output
- Code Interpreter API
- Estratégias de cache
- Otimização de desempenho

---

## Recursos

- **Documentação**: [README.md](./README.md)
- **Depuração**: [SANDBOX_DEBUGGING.md](./SANDBOX_DEBUGGING.md)
- **Informações de Componentes**: [claude-code-sandbox.md](./claude-code-sandbox.md)
- **Documentação Cloudflare**: https://developers.cloudflare.com/sandbox/
- **Documentação Anthropic**: https://docs.anthropic.com/

---

## Obtendo Ajuda

1. **Verificar logs**: `npx wrangler tail`
2. **Usar monitor**: `node monitor.ts "teste" sua_chave`
3. **Ler guia de depuração**: [SANDBOX_DEBUGGING.md](./SANDBOX_DEBUGGING.md)
4. **Verificar status do container**: `npx wrangler containers list`
5. **Testar saúde**: `curl https://your-worker.workers.dev/health`

---

**Você está pronto! Comece a executar código com Claude AI na rede edge do Cloudflare.**