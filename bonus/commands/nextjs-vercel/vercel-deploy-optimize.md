---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [environment] [--analyze] [--preview]
description: Otimize e faça deploy da aplicação Next.js para Vercel com monitoramento de performance
---

## Otimização de Deployment no Vercel

**Ambiente Alvo**: $ARGUMENTS

## Estado Atual do Deployment

- Diretório do projeto: !`pwd`
- Status do Git: !`git status --porcelain`
- Branch atual: !`git branch --show-current`
- Status do projeto Vercel: !`vercel --version 2>/dev/null || echo "Vercel CLI not installed"`
- Output do build: !`ls -la .next/ 2>/dev/null || echo "No build found"`

## Análise de Configuração

### Configuração do Projeto
- Configuração Next.js: @next.config.js
- Configuração Vercel: @vercel.json (se existir)
- Package.json: @package.json
- Variáveis de ambiente: @.env.local (se existir)
- Exemplo de ambiente: @.env.example (se existir)

### Configuração do Vercel
Analise e otimize a configuração `vercel.json`:
```json
{
  "framework": "nextjs",
  "buildCommand": "npm run build",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "regions": ["iad1", "sfo1", "lhr1"],
  "functions": {
    "app/api/**/*.ts": {
      "runtime": "nodejs18.x",
      "maxDuration": 30,
      "memory": 1024
    }
  },
  "crons": [],
  "headers": [
    {
      "source": "/api/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "s-maxage=300, stale-while-revalidate=86400"
        }
      ]
    },
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "X-Frame-Options",
          "value": "DENY"
        },
        {
          "key": "X-Content-Type-Options",
          "value": "nosniff"
        },
        {
          "key": "Referrer-Policy",
          "value": "strict-origin-when-cross-origin"
        }
      ]
    }
  ],
  "redirects": [],
  "rewrites": []
}
```

## Otimização Pré-Deployment

### 1. Otimização de Build
Execute análise abrangente de build:
- **Análise de Bundle**: Gere relatório do bundle analyzer
- **Verificação de Performance**: Analise o output do build em busca de oportunidades de otimização
- **Verificação de Tipos**: Certifique-se de que a compilação TypeScript está livre de erros
- **Verificação de Lint**: Execute ESLint para qualidade de código

```bash
# Comandos de otimização de build
npm run build
npm run lint
npm run type-check  # Se for projeto TypeScript
```

### 2. Otimização de Performance

#### Verificação de Otimização de Imagens
```javascript
// Verifique a configuração de imagens do Next.js
const nextConfig = {
  images: {
    formats: ['image/webp', 'image/avif'],
    deviceSizes: [640, 750, 828, 1080, 1200, 1920, 2048, 3840],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384],
    minimumCacheTTL: 31536000,
    dangerouslyAllowSVG: false,
    contentSecurityPolicy: "default-src 'self'; script-src 'none'; sandbox;",
  },
};
```

#### Análise de Bundle
Gere e analise o webpack bundle:
```bash
ANALYZE=true npm run build
# ou
npm run build -- --analyze
```

### 3. Configuração de Ambiente

#### Configuração de Variáveis de Ambiente
Garanta a configuração apropriada de variáveis de ambiente:
- **Produção**: Verifique se todas as variáveis de ambiente necessárias estão configuradas no dashboard do Vercel
- **Preview**: Configure variáveis de ambiente de preview
- **Desenvolvimento**: Configuração de ambiente de desenvolvimento local

### 4. Otimização de Headers de Segurança
```javascript
// Headers de segurança aprimorados em next.config.js
const securityHeaders = [
  {
    key: 'X-DNS-Prefetch-Control',
    value: 'on'
  },
  {
    key: 'Strict-Transport-Security',
    value: 'max-age=63072000; includeSubDomains; preload'
  },
  {
    key: 'X-XSS-Protection',
    value: '1; mode=block'
  },
  {
    key: 'X-Frame-Options',
    value: 'SAMEORIGIN'
  },
  {
    key: 'Permissions-Policy',
    value: 'camera=(), microphone=(), geolocation=()'
  },
  {
    key: 'X-Content-Type-Options',
    value: 'nosniff'
  },
  {
    key: 'Referrer-Policy',
    value: 'origin-when-cross-origin'
  }
];
```

## Processo de Deployment

### 1. Checklist Pré-Deployment
- [ ] Build passa sem erros
- [ ] Todos os testes passam (se disponível)
- [ ] Variáveis de ambiente configuradas
- [ ] Headers de segurança implementados
- [ ] Baseline de métricas de performance estabelecido
- [ ] Migrações de banco de dados concluídas (se aplicável)

### 2. Comandos de Deployment

#### Deployment em Produção
```bash
# Faça deploy para produção
vercel --prod

# Deploy com variáveis de ambiente
vercel --prod --env-file .env.production

# Deploy de diretório específico
vercel --prod --cwd ./path/to/project
```

#### Deployment de Preview
```bash
# Faça deployment de preview a partir da branch atual
vercel

# Deploy com alias customizado
vercel --alias preview-branch-name.vercel.app
```

#### Deployment com Analytics
```bash
# Deploy com analytics de build
ANALYZE=true vercel --prod

# Deploy com monitoramento de performance
vercel --prod --meta performance=true
```

## Otimização Pós-Deployment

### 1. Configuração de Monitoramento de Performance

#### Rastreamento de Core Web Vitals
```typescript
// Adicione a _app.tsx ou layout.tsx
import { Analytics } from '@vercel/analytics/react';
import { SpeedInsights } from '@vercel/speed-insights/next';

export default function App({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      <Analytics />
      <SpeedInsights />
    </>
  );
}
```

#### Rastreamento de Performance Customizado
```typescript
// lib/analytics.ts
export function reportWebVitals({ id, name, label, value }) {
  fetch('/api/analytics', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      metric: name,
      value: value,
      label: label,
      timestamp: Date.now()
    })
  });
}
```

### 2. Validação de Deployment

#### Health Checks
- **Saúde da Aplicação**: Verifique se a aplicação carrega corretamente
- **Endpoints de API**: Teste rotas de API críticas
- **Conectividade de Banco de Dados**: Verifique conexões de banco de dados (se aplicável)
- **Serviços Externos**: Teste integrações com terceiros

#### Validação de Performance
- **Core Web Vitals**: Verifique os scores de LCP, FID, CLS
- **Score do Lighthouse**: Execute auditoria do Lighthouse
- **Teste de Carga**: Verifique performance da aplicação sob carga
- **Monitoramento de Erros**: Confirme se o rastreamento de erros está funcionando

### 3. Estratégia de Rollback
```bash
# Liste deployments recentes
vercel list

# Faça rollback para um deployment específico
vercel rollback <deployment-url>

# Gerenciamento de alias para rollback instantâneo
vercel alias set <previous-deployment-url> <production-domain>
```

## Otimizações Específicas do Ambiente

### Ambiente de Produção
- **Estratégia de Cache**: Implemente caching agressivo com ISR
- **Configuração de CDN**: Otimize entrega de assets
- **Otimização de Banco de Dados**: Connection pooling e otimização de queries
- **Monitoramento**: Rastreamento abrangente de erros e monitoramento de performance

### Ambiente de Preview
- **Teste de Features**: Ambiente seguro para validação de features
- **Revisão de Stakeholders**: URLs de preview compartilháveis
- **Teste de Integração**: Ambiente de teste end-to-end
- **Benchmark de Performance**: Compare contra métricas de produção

### Ambiente de Desenvolvimento
- **Hot Reloading**: Feedback rápido de desenvolvimento
- **Ferramentas de Debug**: Capacidades de debug aprimoradas
- **Dados de Teste**: Banco de dados de teste e serviços isolados
- **Analytics de Desenvolvimento**: Profiling de performance local

## Monitoramento e Manutenção

### 1. Métricas de Deployment
Rastreie métricas-chave de deployment:
- **Tempo de Build**: Monitore performance do build
- **Tempo de Deploy**: Rastreie duração do deployment
- **Taxa de Sucesso**: Monitore taxas de sucesso/falha do deployment
- **Frequência de Rollback**: Rastreie eventos de rollback

### 2. Monitoramento de Performance
- **Monitoramento de Usuário Real**: Rastreie performance real do usuário
- **Monitoramento Sintético**: Teste automatizado de performance
- **Rastreamento de Erros**: Monitore e alerte sobre erros
- **Monitoramento de Uptime**: Rastreie disponibilidade da aplicação

### 3. Otimização de Custos
- **Duração de Função**: Otimize tempo de execução de funções serverless
- **Uso de Bandwidth**: Monitore e otimize transferência de dados
- **Minutos de Build**: Otimize processos de build
- **Requisições de Edge**: Monitore uso de edge functions

## Resolução de Problemas Comuns

### Falhas de Build
- Verifique logs de build no dashboard do Vercel
- Verifique se todas as dependências estão em package.json
- Certifique-se de que as variáveis de ambiente estão configuradas corretamente
- Verifique se há erros TypeScript (se aplicável)

### Problemas de Performance
- Analise o tamanho do bundle e otimize imports
- Implemente code splitting apropriado
- Otimize imagens e assets estáticos
- Use features de otimização de performance do Next.js

### Problemas de Deployment
- Verifique a conexão do repositório Git
- Verifique regras de proteção de branch
- Certifique-se de ter permissões de acesso apropriadas
- Valide a configuração de deployment

## Critérios de Sucesso

O deployment é bem-sucedido quando:
- [ ] Aplicação constrói sem erros
- [ ] Todos os testes passam (se disponível)
- [ ] Scores de Core Web Vitals são ótimos (LCP < 2,5s, FID < 100ms, CLS < 0,1)
- [ ] Headers de segurança estão configurados corretamente
- [ ] Monitoramento de performance está ativo
- [ ] Rastreamento de erros está operacional
- [ ] Procedimentos de rollback são testados e documentados

Forneça recomendações pós-deployment e próximos passos para otimização e monitoramento contínuos.