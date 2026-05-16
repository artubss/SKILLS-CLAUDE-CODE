---
allowed-tools: Read, Edit, Bash
argument-hint: [--build] [--analyze] [--report]
description: Analise e otimize o tamanho do bundle do Next.js com recomendações detalhadas
---

## Analisador de Bundle do Next.js

**Modo de Análise**: $ARGUMENTS

## Análise do Projeto Atual

### Configuração de Build
- Configuração Next.js: @next.config.js
- Package.json: @package.json
- Configuração TypeScript: @tsconfig.json (se existir)
- Output de build: !`ls -la .next/ 2>/dev/null || echo "No build found"`

### Análise de Dependências
- Dependências de produção: !`npm list --prod --depth=0 2>/dev/null || echo "Run npm install first"`
- Dependências de desenvolvimento: !`npm list --dev --depth=0 2>/dev/null || echo "Run npm install first"`
- Vulnerabilidades do pacote: !`npm audit --audit-level=moderate 2>/dev/null || echo "No audit available"`

## Configuração de Análise de Bundle

### 1. Instalar Analisador de Bundle
```bash
# Instalar webpack-bundle-analyzer
npm install --save-dev @next/bundle-analyzer

# Ou usar o analisador integrado do Next.js
npm install --save-dev cross-env
```

### 2. Configurar Analisador de Bundle do Next.js
```javascript
// next.config.js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
});

/** @type {import('next').NextConfig} */
const nextConfig = {
  // Sua configuração existente
  experimental: {
    optimizePackageImports: [
      'lucide-react',
      '@heroicons/react',
      'date-fns',
      'lodash',
    ],
  },
  webpack: (config, { buildId, dev, isServer, defaultLoaders, webpack }) => {
    // Otimizações de análise de bundle
    if (!dev && !isServer) {
      config.optimization.splitChunks = {
        chunks: 'all',
        cacheGroups: {
          default: false,
          vendors: false,
          // Chunk de vendor para bibliotecas comuns
          vendor: {
            name: 'vendors',
            chunks: 'all',
            test: /node_modules/,
            priority: 20,
          },
          // Chunk comum para código compartilhado
          common: {
            name: 'commons',
            minChunks: 2,
            chunks: 'all',
            priority: 10,
            reuseExistingChunk: true,
            enforce: true,
          },
          // Chunk de bibliotecas de UI
          ui: {
            name: 'ui-libs',
            chunks: 'all',
            test: /node_modules\/(react|react-dom|@radix-ui|@headlessui)/,
            priority: 15,
          },
          // Chunk de bibliotecas utilitárias
          utils: {
            name: 'utils',
            chunks: 'all',
            test: /node_modules\/(lodash|date-fns|clsx|classnames)/,
            priority: 15,
          },
        },
      };
    }

    return config;
  },
};

module.exports = withBundleAnalyzer(nextConfig);
```

### 3. Scripts do Package.json
```json
{
  "scripts": {
    "analyze": "cross-env ANALYZE=true next build",
    "analyze:server": "cross-env BUNDLE_ANALYZE=server next build",
    "analyze:browser": "cross-env BUNDLE_ANALYZE=browser next build",
    "build:analyze": "npm run build && npm run analyze"
  }
}
```

## Execução da Análise de Bundle

### 1. Gerar Relatório de Análise
```bash
# Análise completa de bundle
ANALYZE=true npm run build

# Análise de bundle do lado do servidor
BUNDLE_ANALYZE=server npm run build

# Análise de bundle do lado do cliente
BUNDLE_ANALYZE=browser npm run build

# Build de produção com análise
npm run analyze
```

### 2. Verificação de Tamanho de Bundle
```bash
# Verificar tamanho atual do bundle
ls -lah .next/static/chunks/ | head -20

# Verificar tamanhos de bundle com detalhes
find .next/static/chunks -name "*.js" -exec ls -lah {} \; | sort -k5 -hr

# Análise de tamanho comprimido
find .next/static/chunks -name "*.js" -exec gzip -c {} \; | wc -c
```

## Resultados da Análise de Bundle

### 1. Detalhamento de Tamanho de Bundle
Analise o relatório gerado pelo webpack-bundle-analyzer para:

#### Bundles de Cliente
- **Bundle principal**: Código da aplicação principal
- **Bundle de framework**: Runtime do Next.js e React
- **Bundles de vendor**: Bibliotecas de terceiros
- **Bundles de página**: Chunks individuais de páginas
- **Bundles compartilhados**: Código comum entre páginas

#### Bundles de Servidor
- **API routes**: Manipuladores de API do lado do servidor
- **Middleware**: Middleware de edge e servidor
- **Componentes de servidor**: Bundles de RSC

### 2. Limites de Tamanho e Recomendações
```javascript
// Limites de tamanho de bundle
const bundleThresholds = {
  // First Load JS (crítico)
  firstLoadJS: {
    warning: 200 * 1024, // 200KB
    error: 300 * 1024,   // 300KB
  },
  // Chunks individuais
  chunk: {
    warning: 150 * 1024, // 150KB
    error: 250 * 1024,   // 250KB
  },
  // Tamanho total do bundle
  total: {
    warning: 1024 * 1024, // 1MB
    error: 2048 * 1024,   // 2MB
  }
};
```

## Estratégias de Otimização de Bundle

### 1. Otimização de Code Splitting
```typescript
// Imports dinâmicos para componentes grandes
import dynamic from 'next/dynamic';

const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
  loading: () => <p>Carregando...</p>,
  ssr: false, // Desabilitar SSR para componentes apenas de cliente
});

// Code splitting baseado em rota
const AdminDashboard = dynamic(() => import('./AdminDashboard'), {
  loading: () => <DashboardSkeleton />,
});

// Carregamento condicional
const ChartComponent = dynamic(
  () => import('./ChartComponent'),
  { 
    ssr: false,
    loading: () => <ChartSkeleton />
  }
);
```

### 2. Otimização de Bibliotecas
```javascript
// Otimizar imports de lodash
// ❌ Importa toda a biblioteca lodash
import _ from 'lodash';

// ✅ Importar apenas as funções necessárias
import { debounce, throttle } from 'lodash';

// ✅ Ainda melhor - usar alternativas com tree-shaking
import debounce from 'lodash/debounce';
import throttle from 'lodash/throttle';
```

```javascript
// Otimização de biblioteca de data
// ❌ Moment.js (bundle grande)
import moment from 'moment';

// ✅ date-fns (tree-shakable)
import { format, parseISO } from 'date-fns';

// ✅ Day.js (alternativa menor)
import dayjs from 'dayjs';
```

### 3. Otimizações Específicas do Next.js
```javascript
// Otimizações do next.config.js
const nextConfig = {
  // Otimizar imports de pacotes
  experimental: {
    optimizePackageImports: [
      'react-icons',
      '@heroicons/react',
      'lucide-react',
      'date-fns',
      'lodash',
    ],
  },
  
  // Tree shaking para CSS
  experimental: {
    optimizeCss: true,
  },
  
  // Minimizar JavaScript do lado do cliente
  compiler: {
    removeConsole: process.env.NODE_ENV === 'production',
  },
  
  // Otimizações de webpack
  webpack: (config, { dev, isServer }) => {
    if (!dev && !isServer) {
      // Analisar tamanho do bundle
      config.optimization.concatenateModules = true;
      
      // Habilitar compressão
      config.plugins.push(
        new (require('compression-webpack-plugin'))({
          algorithm: 'gzip',
          test: /\.(js|css|html|svg)$/,
          threshold: 8192,
          minRatio: 0.8,
        })
      );
    }
    return config;
  },
};
```

### 4. Otimização de Imagem
```typescript
// Componente Image do Next.js com otimização
import Image from 'next/image';

// Otimizar imagens com dimensionamento adequado
<Image
  src="/hero-image.jpg"
  alt="Hero"
  width={1200}
  height={600}
  priority={isAboveFold}
  placeholder="blur"
  blurDataURL="data:image/jpeg;base64,..."
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
/>
```

## Análise de Impacto de Desempenho

### 1. Impacto nas Core Web Vitals
Analise o impacto do tamanho do bundle em:
- **Largest Contentful Paint (LCP)**: Bundles grandes atrasam a renderização de conteúdo
- **First Input Delay (FID)**: JavaScript bloqueando a thread principal
- **Cumulative Layout Shift (CLS)**: Imports dinâmicos causando mudanças de layout

### 2. Desempenho de Rede
```javascript
// Simular condições de rede para testes
const networkConditions = {
  'Fast 3G': { downloadThroughput: 1500, uploadThroughput: 750, latency: 562.5 },
  'Slow 3G': { downloadThroughput: 500, uploadThroughput: 500, latency: 2000 },
  'Offline': { downloadThroughput: 0, uploadThroughput: 0, latency: 0 }
};
```

### 3. Estratégias de Carregamento de Bundle
```typescript
// Preload de chunks críticos
useEffect(() => {
  // Preload da próxima página provável
  router.prefetch('/dashboard');
  
  // Preload de componentes críticos
  import('./CriticalComponent');
}, []);

// Lazy load de recursos não críticos
const LazyFeature = lazy(() => 
  import('./LazyFeature').then(module => ({
    default: module.LazyFeature
  }))
);
```

## Recomendações de Otimização

### 1. Ações Imediatas
- **Remover dependências não utilizadas**: Auditar e remover pacotes não usados
- **Otimizar imports**: Usar padrões de importação com tree-shaking
- **Habilitar compressão**: Configurar compressão gzip/brotli
- **Minimizar polyfills**: Usar recursos JavaScript moderno com polyfills direcionados

### 2. Melhorias de Médio Prazo
- **Estratégia de code splitting**: Implementar splitting baseado em rota e componente
- **Substituição de bibliotecas**: Substituir bibliotecas grandes por alternativas menores
- **Cache de bundle**: Implementar estratégias de cache de longo prazo
- **Monitoramento de desempenho**: Configurar monitoramento de tamanho de bundle em CI/CD

### 3. Otimizações de Longo Prazo
- **Micro-frontends**: Considerar mudanças de arquitetura para aplicações grandes
- **Edge computing**: Mover computação mais perto dos usuários
- **Progressive enhancement**: Implementar estratégias de carregamento progressivo
- **Performance budgets**: Estabelecer e enforçar orçamentos de tamanho de bundle

## Monitoramento e Manutenção

### 1. Monitoramento Automático de Bundle
```yaml
# GitHub Action para monitoramento de bundle
name: Bundle Size Check
on: [pull_request]

jobs:
  bundle-analysis:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run build
      - uses: nextjs-bundle-analysis/bundle-analyzer@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
```

### 2. Performance Budgets
```javascript
// webpack.config.js performance budgets
module.exports = {
  performance: {
    maxAssetSize: 250000, // 250KB
    maxEntrypointSize: 350000, // 350KB
    hints: 'error',
  },
};
```

### 3. Agenda de Auditoria Regular
- **Semanal**: Atualizações de dependência e auditoria de segurança
- **Mensal**: Análise completa de bundle e revisão de otimizações
- **Trimestral**: Revisão de arquitetura e otimizações principais

## Geração de Relatório de Análise

Gere um relatório abrangente incluindo:
1. **Tamanhos de Bundle Atuais**: Detalhamento por tipo de chunk
2. **Oportunidades de Otimização**: Recomendações específicas com impacto de tamanho
3. **Métricas de Desempenho**: Análise de impacto nas Core Web Vitals
4. **Roadmap de Implementação**: Tarefas de otimização priorizadas
5. **Configuração de Monitoramento**: Ferramentas e processos para monitoramento contínuo

Forneça recomendações específicas e práticas para otimização imediata e de longo prazo do bundle.