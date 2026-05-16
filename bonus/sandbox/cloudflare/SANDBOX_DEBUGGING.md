# Guia de Debugging de Sandbox do Cloudflare

## 🔍 Ferramentas de Monitoramento Disponíveis

### 1. Launcher com Log Aprimorado
**Arquivo**: `launcher.ts`
- Log detalhado de cada etapa de execução
- Verificações de disponibilidade do worker
- Monitoramento de geração de código
- Fallback para execução direta se o worker indisponível
- Saída colorida no terminal para melhor legibilidade

### 2. Monitor em Tempo Real
**Arquivo**: `monitor.ts`
- Rastreamento de métricas de desempenho em tempo real
- Monitoramento de saúde do worker
- Análise de tempo de geração de código
- Monitoramento de execução no sandbox
- Rastreamento de uso de memória
- Relatório abrangente de erros

### 3. Ferramentas Wrangler CLI
**Ferramentas integradas do Cloudflare para debugging**:
- `npx wrangler tail` - Streaming de logs em tempo real
- `npx wrangler containers list` - Status dos containers
- `npx wrangler deployments list` - Histórico de deployments
- `npx wrangler dev` - Servidor de desenvolvimento local

## 🚨 Troubleshooting Comum

### Problema: "Container not ready"
**Sintomas**:
```
Error: Container not ready. Please wait 2-3 minutes after deployment.
```

**Soluções**:
1. **Aguarde o provisionamento**:
   ```bash
   # Verifique o status do container
   npx wrangler containers list

   # Saída esperada após provisionamento:
   # ✓ Container ready for sandbox execution
   ```

2. **Verifique o deployment**:
   ```bash
   npx wrangler deployments list
   # Verifique o status e timestamp do deployment
   ```

3. **Verifique os logs do worker**:
   ```bash
   npx wrangler tail
   # Procure por erros de inicialização
   ```

### Problema: "Worker not responding"
**Sintomas**:
```
❌ Worker health check failed: fetch failed
```

**Etapas de Debugging**:
1. **Verifique se o worker foi deployado**:
   ```bash
   npx wrangler deploy
   # Deve retornar a URL do worker
   ```

2. **Teste o endpoint do worker**:
   ```bash
   curl https://your-worker.your-subdomain.workers.dev
   # Deve retornar instruções de uso
   ```

3. **Verifique o desenvolvimento local**:
   ```bash
   # Para testes locais
   npm run dev

   # Teste o endpoint local
   curl http://localhost:8787
   ```

### Problema: "Anthropic API key not set"
**Sintomas**:
```
Error: ANTHROPIC_API_KEY is required
```

**Soluções**:
1. **Defina como secret do Wrangler (Produção)**:
   ```bash
   npx wrangler secret put ANTHROPIC_API_KEY
   # Cole sua chave quando solicitado
   ```

2. **Defina em .dev.vars (Desenvolvimento Local)**:
   ```bash
   # Crie arquivo .dev.vars:
   echo "ANTHROPIC_API_KEY=sk-ant-your-key-here" > .dev.vars
   ```

3. **Verifique se o secret foi definido**:
   ```bash
   npx wrangler secret list
   # Deve mostrar ANTHROPIC_API_KEY
   ```

### Problema: "Sandbox execution timeout"
**Sintomas**:
```
Error: Sandbox execution exceeded 30 second timeout
```

**Soluções**:
1. **Use Durable Objects para operações mais longas**:
   ```typescript
   // Em wrangler.toml, certifique-se que Durable Objects estão configurados
   [[durable_objects.bindings]]
   name = "Sandbox"
   class_name = "Sandbox"
   ```

2. **Otimize a geração de código**:
   ```typescript
   // Solicite código mais conciso
   const prompt = `Generate SIMPLE Python code...`;
   ```

3. **Divida em tarefas menores**:
   ```bash
   # Em vez de operações complexas, divida em etapas
   npx claude-code-templates --sandbox cloudflare \
     --prompt "Step 1: Create data structure"
   ```

### Problema: "Docker not running" (Desenvolvimento Local)
**Sintomas**:
```
Error: Docker daemon is not running
```

**Soluções**:
1. **Inicie o Docker Desktop**:
   - macOS: Abra a aplicação Docker Desktop
   - Linux: `sudo systemctl start docker`
   - Windows: Inicie o Docker Desktop

2. **Verifique se o Docker está rodando**:
   ```bash
   docker ps
   # Deve listar containers em execução
   ```

3. **Alternativa: Deploy direto no Cloudflare**:
   ```bash
   # Pule testes locais, faça deploy direto
   npx wrangler deploy
   ```

## 📊 Usando o Monitor para Debugging

### Comando Básico de Monitoramento:
```bash
# Monitore uma operação simples
node monitor.ts "Calculate factorial of 5" your_api_key

# Monitore com URL de worker customizada
node monitor.ts "Fibonacci 10" your_api_key https://your-worker.workers.dev
```

### Exemplo de Saída do Monitor:
```
[14:32:15] ℹ 🚀 Starting enhanced Cloudflare sandbox monitoring
============================================================
🖥️  SYSTEM INFORMATION
============================================================

Node.js Version: v20.11.0
Platform: darwin
Architecture: arm64
Memory Usage: 45MB / 128MB

============================================================

[14:32:16] ℹ 🔍 Checking Cloudflare Worker health...
[14:32:16] ✓ Worker is responding
[14:32:16] ℹ    Status: 200 OK

[14:32:17] ℹ 🤖 Starting code generation with Claude...
[14:32:19] ✓ Code generated in 2147ms
[14:32:19] ℹ    Model: claude-sonnet-4-5-20250929
[14:32:19] ℹ    Tokens used: 156 in, 89 out
[14:32:19] ℹ    Code length: 234 characters

[14:32:19] ℹ ⚙️  Executing in Cloudflare Sandbox...
[14:32:21] ✓ Sandbox execution completed in 1856ms
[14:32:21] ℹ    Exit code: 0 (success)
[14:32:21] ℹ    Output length: 3 characters

============================================================
📊 PERFORMANCE METRICS
============================================================

Total Execution Time: 4123ms
  ├─ Code Generation: 2147ms
  └─ Sandbox Execution: 1856ms
Memory Usage: 48MB

Status: Success ✓
============================================================
```

## 🎯 Debugging de Cenários Específicos

### 1. Problemas de Geração de Código
```bash
# Use o monitor para ver a interação exata com a API Claude
node monitor.ts "Complex prompt that might fail"

# Procure por:
# - Uso de tokens (pode atingir limites)
# - Preview do código gerado
# - Modelo utilizado (deve ser claude-sonnet-4-5)
```

### 2. Problemas de Execução no Sandbox
```bash
# Verifique os logs do worker durante testes
npx wrangler tail &
node launcher.ts "Test prompt"

# Procure por:
# - Erros de criação do sandbox
# - Falhas de escrita de arquivo
# - Erros de execução do Python
```

### 3. Problemas de Desempenho
```bash
# Use o monitor para identificar gargalos
node monitor.ts "Your prompt"

# Compare métricas:
# - Tempo de Geração de Código (API Claude)
# - Tempo de Execução no Sandbox (Cloudflare)
# - Tempo Total de Round Trip
```

### 4. Problemas de Rede/Deployment
```bash
# Verifique deployments
npx wrangler deployments list

# Veja logs recentes
npx wrangler tail --format=pretty

# Teste a saúde do worker
curl -v https://your-worker.workers.dev
```

## 🛠 Configuração Avançada

### Ative o Modo Debug:
```bash
# Em wrangler.toml
[env.development]
vars = { DEBUG = "true" }

# Ou em .dev.vars para desenvolvimento local
DEBUG=true
ANTHROPIC_API_KEY=your_key
```

### Timeouts Customizados:
```typescript
// Em src/index.ts
const result = await sandbox.exec('python /tmp/code.py', {
  timeout: 60000, // 60 seconds
});
```

### Log Verboso:
```bash
# Defina o nível de log
export WRANGLER_LOG=debug

# Execute com saída verbosa
npx wrangler deploy --verbose
```

## 📋 Checklist de Debugging

### Antes de Reportar um Problema:
- [ ] Conta Cloudflare Workers ativa (Plano pago se usar Durable Objects)
- [ ] Chave da API Anthropic válida e com créditos
- [ ] Worker deployado com sucesso (`npx wrangler deploy`)
- [ ] Aguardou 2-3 minutos após primeiro deployment
- [ ] Containers provisionados (`npx wrangler containers list`)
- [ ] Secrets configurados (`npx wrangler secret list`)
- [ ] Docker rodando (para desenvolvimento local)
- [ ] Usou a ferramenta monitor para métricas detalhadas
- [ ] Verificou logs do worker (`npx wrangler tail`)
- [ ] Testou com prompt simples primeiro

### Informações a Incluir em Relatórios de Bug:
- Saída completa do monitor mostrando timestamps e métricas
- URL do worker ou ambiente de desenvolvimento local
- Prompt exato que causou o problema
- Componentes instalados (se aplicável)
- Logs do worker de `npx wrangler tail`
- Status do container de `npx wrangler containers list`
- Mensagens de erro com stack traces completos
- Versões de Node.js e Wrangler

## 🚀 Dicas de Otimização de Desempenho

### 1. Minimize o Tempo de Geração de Código
```typescript
// Seja específico para reduzir o tempo de processamento do Claude
const prompt = `Generate a single Python function to calculate factorial.
Use recursion. Include only the function, no tests.`;
```

### 2. Use Code Interpreter API
```typescript
// Mais rápido que exec para Python
import { getCodeInterpreter } from '@cloudflare/sandbox';
const interpreter = getCodeInterpreter(env.Sandbox, userId);
const result = await interpreter.notebook.execCell(pythonCode);
```

### 3. Implemente Cache
```typescript
// Cache de código gerado para prompts comuns
const cacheKey = `code:${hashPrompt(prompt)}`;
let code = await env.CACHE.get(cacheKey);
if (!code) {
  code = await generateCode(prompt);
  await env.CACHE.put(cacheKey, code, { expirationTtl: 3600 });
}
```

### 4. Stream de Respostas
```typescript
// Stream de saída para melhor desempenho percebido
return new Response(
  new ReadableStream({
    async start(controller) {
      const result = await sandbox.exec(command, {
        onStdout: (data) => controller.enqueue(encoder.encode(data)),
      });
      controller.close();
    },
  })
);
```

## 🔗 Referência de Comandos Úteis

### Deployment e Gerenciamento
```bash
# Deploy do worker
npx wrangler deploy

# Deploy em ambiente específico
npx wrangler deploy --env production

# Reverter deployment
npx wrangler rollback

# Deletar deployment
npx wrangler delete
```

### Gerenciamento de Secrets
```bash
# Adicionar secret
npx wrangler secret put SECRET_NAME

# Listar secrets
npx wrangler secret list

# Deletar secret
npx wrangler secret delete SECRET_NAME
```

### Desenvolvimento Local
```bash
# Inicie o servidor dev
npm run dev

# Inicie em porta específica
npx wrangler dev --port 3000

# Inicie com Durable Objects remotos
npx wrangler dev --remote
```

### Monitoramento e Logs
```bash
# Tail de logs em tempo real
npx wrangler tail

# Tail com formatação bonita
npx wrangler tail --format=pretty

# Tail de deployment específico
npx wrangler tail --deployment-id <id>

# Filtrar logs
npx wrangler tail --status error
```

### Gerenciamento de Containers
```bash
# Listar containers
npx wrangler containers list

# Obter detalhes do container
npx wrangler containers describe <container-id>
```

## 💡 Dicas e Boas Práticas

1. **Sempre teste localmente primeiro**: Use `npm run dev` antes de fazer deploy
2. **Monitore métricas**: Use a ferramenta monitor para rastrear desempenho
3. **Verifique logs regularmente**: Configure `npx wrangler tail` durante testes
4. **Use configs específicas por ambiente**: Separe dev/prod em wrangler.toml
5. **Implemente tratamento de erros**: Capture e registre todos os erros adequadamente
6. **Defina timeouts apropriados**: Equilibre entre experiência do usuário e uso de recursos
7. **Use Durable Objects com sabedoria**: Apenas para operações com estado
8. **Cache agressivamente**: Reduza chamadas de API com cache inteligente
9. **Stream quando possível**: Melhor UX para operações longas
10. **Versione seus deployments**: Marque releases para rollback fácil

---

**Com essas ferramentas e técnicas, você pode debugar e otimizar efetivamente sua implementação de sandbox do Cloudflare.**