---
allowed-tools: Read, Edit, Bash
argument-hint: [--lighthouse] [--bundle] [--runtime] [--all]
description: Auditoria abrangente de desempenho Next.js com recomendações de otimização acionáveis
---

## Auditoria de Desempenho Next.js

**Tipo de Auditoria**: $ARGUMENTS

## Análise da Aplicação Atual

### Estado da Aplicação
- Status de build: !`ls -la .next/ 2>/dev/null || echo "No build found - run 'npm run build' first"`
- Aplicação em execução: !`curl -s http://localhost:3000 > /dev/null && echo "App is running" || echo "App not running - start with 'npm run dev'"`
- Análise de bundle: !`ls -la .next/analyze/ 2>/dev/null || echo "No bundle analysis found"`

### Configuração do Projeto
- Configuração Next.js: @next.config.js
- Package.json: @package.json
- Configuração TypeScript: @tsconfig.json (if exists)
- Configuração Vercel: @vercel.json (if exists)

### Configuração de Monitoramento de Desempenho
- Web Vitals: Verifique @next/web-vitals ou similar
- Analytics: Verifique Vercel Analytics ou Google Analytics
- Ferramentas de monitoramento: Verifique Sentry, DataDog ou outras ferramentas APM

## Framework de Auditoria de Desempenho

### 1. Auditoria Lighthouse
```bash
# Install Lighthouse CLI if not available
npm install -g lighthouse

# Run Lighthouse audit
lighthouse http://localhost:3000 \
  --output=json \
  --output=html \
  --output-path=./performance-audit \
  --chrome-flags="--headless" \
  --preset=perf

# Mobile performance audit
lighthouse http://localhost:3000 \
  --output=json \
  --output-path=./performance-audit-mobile \
  --preset=perf \
  --form-factor=mobile \
  --throttling-method=devtools \
  --chrome-flags="--headless"

# Generate detailed report
lighthouse http://localhost:3000 \
  --output=html \
  --output-path=./lighthouse-report.html \
  --view
```

### 2. Análise de Bundle
```javascript
// next.config.js - Enable bundle analysis
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
});

module.exports = withBundleAnalyzer({
  // ... your config
  webpack: (config, { buildId, dev, isServer, defaultLoaders, webpack }) => {
    // Bundle analysis optimizations
    if (!dev && !isServer) {
      config.optimization.splitChunks = {
        chunks: 'all',
        cacheGroups: {
          vendor: {
            test: /[\\/]node_modules[\\/]/,
            name: 'vendors',
            chunks: 'all',
          },
        },
      };
    }
    return config;
  },
});
```

### 3. Análise de Desempenho em Runtime
```bash
# Build and analyze bundle
ANALYZE=true npm run build

# Check bundle sizes
ls -lah .next/static/chunks/ | grep -E "\\.js$" | sort -k5 -hr | head -10

# Analyze dependencies
npm ls --depth=0 --prod | grep -v "deduped"

# Check for duplicate dependencies
npm ls --depth=0 | grep -E "UNMET|invalid"
```

## Coleta de Métricas de Desempenho

### 1. Implementação de Core Web Vitals
```typescript
// lib/analytics.ts
export function reportWebVitals({ id, name, label, value }: any) {
  // Send to analytics service
  if (typeof window !== 'undefined') {
    // Client-side reporting
    fetch('/api/analytics/web-vitals', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        id,
        name,
        label,
        value,
        url: window.location.href,
        timestamp: Date.now(),
      }),
    }).catch(console.error);
  }
}

// Track specific metrics
export function trackMetric(name: string, value: number, labels?: Record<string, string>) {
  reportWebVitals({
    id: `${name}-${Date.now()}`,
    name,
    label: 'custom',
    value,
    ...labels,
  });
}

// Performance observer for custom metrics
export function initPerformanceObserver() {
  if (typeof window === 'undefined') return;
  
  // Largest Contentful Paint
  new PerformanceObserver((entryList) => {
    for (const entry of entryList.getEntries()) {
      trackMetric('LCP', entry.startTime);
    }
  }).observe({ entryTypes: ['largest-contentful-paint'] });
  
  // First Input Delay
  new PerformanceObserver((entryList) => {
    for (const entry of entryList.getEntries()) {
      trackMetric('FID', entry.processingStart - entry.startTime);
    }
  }).observe({ entryTypes: ['first-input'] });
  
  // Cumulative Layout Shift
  new PerformanceObserver((entryList) => {
    let clsValue = 0;
    for (const entry of entryList.getEntries()) {
      if (!entry.hadRecentInput) {
        clsValue += entry.value;
      }
    }
    trackMetric('CLS', clsValue);
  }).observe({ entryTypes: ['layout-shift'] });
}
```

### 2. Monitoramento de Desempenho no Lado do Servidor
```typescript
// middleware.ts - Performance monitoring
import { NextRequest, NextResponse } from 'next/server';

export function middleware(request: NextRequest) {
  const start = Date.now();
  
  const response = NextResponse.next();
  
  // Add performance headers
  response.headers.set('X-Response-Time', `${Date.now() - start}ms`);
  response.headers.set('X-Timestamp', new Date().toISOString());
  
  return response;
}
```

## Áreas de Análise de Desempenho

### 1. Desempenho de Carregamento
```typescript
// Analyze loading performance
const loadingPerformanceAudit = {
  // First Contentful Paint (FCP)
  fcp: {
    target: '< 1.8s',
    current: '?', // From Lighthouse
    optimizations: [
      'Otimize o caminho crítico de renderização',
      'Incorpore CSS crítico',
      'Pré-carregue recursos-chave',
      'Minimize recursos que bloqueiam renderização',
    ],
  },
  
  // Largest Contentful Paint (LCP)
  lcp: {
    target: '< 2.5s',
    current: '?', // From Lighthouse
    optimizations: [
      'Otimize imagens (componente Next.js Image)',
      'Pré-carregue elemento LCP',
      'Otimize tempo de resposta do servidor',
      'Use CDN para ativos estáticos',
    ],
  },
  
  // Time to Interactive (TTI)
  tti: {
    target: '< 3.8s',
    current: '?', // From Lighthouse
    optimizations: [
      'Reduza tamanho do bundle JavaScript',
      'Code splitting',
      'Remova código não utilizado',
      'Otimize scripts de terceiros',
    ],
  },
  
  // Speed Index
  speedIndex: {
    target: '< 3.4s',
    current: '?', // From Lighthouse
    optimizations: [
      'Otimize conteúdo acima da dobra',
      'Carregamento progressivo de imagens',
      'Priorização de recursos críticos',
    ],
  },
};
```

### 2. Desempenho em Runtime
```typescript
// Runtime performance analysis
const runtimePerformanceAudit = {
  // First Input Delay (FID)
  fid: {
    target: '< 100ms',
    current: '?',
    optimizations: [
      'Reduza tempo de execução JavaScript',
      'Divida tarefas longas',
      'Use web workers para computação pesada',
      'Otimize manipuladores de eventos',
    ],
  },
  
  // Cumulative Layout Shift (CLS)
  cls: {
    target: '< 0.1',
    current: '?',
    optimizations: [
      'Defina dimensões para elementos de mídia',
      'Reserve espaço para anúncios/embeds',
      'Evite inserção dinâmica de conteúdo',
      'Use CSS transforms para animações',
    ],
  },
  
  // Total Blocking Time (TBT)
  tbt: {
    target: '< 200ms',
    current: '?',
    optimizations: [
      'Code splitting',
      'Remova polyfills não utilizados',
      'Otimize código de terceiros',
      'Use setTimeout para operações pesadas',
    ],
  },
};
```

### 3. Desempenho de Bundle
```javascript
// Bundle analysis report
const bundleAnalysis = {
  totalSize: '?', // From webpack-bundle-analyzer
  firstLoadJS: '?', // Critical for performance
  chunks: {
    main: '?',
    framework: '?',
    vendor: '?',
    pages: '?',
  },
  
  recommendations: [
    // Dynamic imports for code splitting
    {
      type: 'Dynamic Import',
      description: 'Use dynamic imports for non-critical components',
      example: `
        const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
          loading: () => <Loading />,
          ssr: false
        });
      `,
    },
    
    // Tree shaking optimization
    {
      type: 'Tree Shaking',
      description: 'Import only needed functions from libraries',
      example: `
        // ❌ Imports entire library
        import * as _ from 'lodash';
        
        // ✅ Import only needed functions
        import { debounce, throttle } from 'lodash';
      `,
    },
    
    // Bundle splitting
    {
      type: 'Bundle Splitting',
      description: 'Optimize webpack chunk splitting',
      example: `
        module.exports = {
          webpack: (config, { isServer }) => {
            if (!isServer) {
              config.optimization.splitChunks.cacheGroups = {
                vendor: {
                  test: /[\\/]node_modules[\\/]/,
                  name: 'vendors',
                  chunks: 'all',
                },
              };
            }
            return config;
          },
        };
      `,
    },
  ],
};
```

## Recomendações de Otimização

### 1. Otimização de Imagens
```typescript
// Image optimization analysis
const imageOptimization = {
  // Next.js Image component usage
  nextImageUsage: 'Analyze usage of next/image vs <img>',
  
  recommendations: [
    {
      priority: 'High',
      description: 'Substitua tags <img> pelo componente Next.js Image',
      implementation: `
        import Image from 'next/image';
        
        // ❌ Regular img tag
        <img src="/hero.jpg" alt="Hero" />
        
        // ✅ Next.js Image component
        <Image
          src="/hero.jpg"
          alt="Hero"
          width={1200}
          height={600}
          priority={true} // For above-the-fold images
          placeholder="blur"
          blurDataURL="data:image/jpeg;base64,..."
        />
      `,
    },
    {
      priority: 'Medium',
      description: 'Implemente imagens responsivas com propriedade sizes',
      implementation: `
        <Image
          src="/hero.jpg"
          alt="Hero"
          width={1200}
          height={600}
          sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
        />
      `,
    },
    {
      priority: 'Low',
      description: 'Configure loader de imagem personalizado para CDN',
      implementation: `
        // next.config.js
        module.exports = {
          images: {
            loader: 'cloudinary',
            path: 'https://res.cloudinary.com/demo/image/fetch/',
          },
        };
      `,
    },
  ],
};
```

### 2. Otimização de CSS
```css
/* Critical CSS analysis */
.critical-css-audit {
  /* Above-the-fold styles that should be inlined */
}

/* Non-critical CSS that can be loaded asynchronously */
.non-critical-css {
  /* Styles for below-the-fold content */
}
```

```typescript
// CSS optimization recommendations
const cssOptimization = {
  recommendations: [
    {
      type: 'Critical CSS',
      description: 'Incorpore CSS crítico para renderização inicial mais rápida',
      implementation: 'Use styled-jsx or CSS-in-JS for critical styles',
    },
    {
      type: 'CSS Modules',
      description: 'Use CSS Modules para evitar poluição do namespace global',
      implementation: 'Import styles as modules: import styles from "./Component.module.css"',
    },
    {
      type: 'Tailwind Purging',
      description: 'Garanta que classes Tailwind não utilizadas sejam removidas',
      implementation: 'Configure purge in tailwind.config.js',
    },
  ],
};
```

### 3. Otimização de JavaScript
```typescript
// JavaScript optimization analysis
const jsOptimization = {
  recommendations: [
    {
      priority: 'High',
      type: 'Code Splitting',
      description: 'Implemente code splitting baseado em rotas e componentes',
      example: `
        // Route-based splitting (automatic with Next.js pages)
        
        // Component-based splitting
        const LazyComponent = dynamic(() => import('./LazyComponent'));
        
        // Conditional loading
        const AdminPanel = dynamic(() => import('./AdminPanel'), {
          ssr: false,
          loading: () => <AdminSkeleton />,
        });
      `,
    },
    {
      priority: 'Medium',
      type: 'Tree Shaking',
      description: 'Garanta que código não utilizado seja eliminado',
      example: `
        // ❌ Imports entire library
        import moment from 'moment';
        
        // ✅ Use tree-shakable alternative
        import { format } from 'date-fns';
        
        // ✅ Or import specific functions
        import debounce from 'lodash/debounce';
      `,
    },
    {
      priority: 'Medium',
      type: 'Polyfill Optimization',
      description: 'Reduza tamanho de polyfill direcionando navegadores modernos',
      example: `
        // next.config.js
        module.exports = {
          experimental: {
            browsersListForSwc: true,
          },
        };
      `,
    },
  ],
};
```

## Configuração de Monitoramento de Desempenho

### 1. Real User Monitoring (RUM)
```typescript
// pages/_app.tsx
import { reportWebVitals } from '../lib/analytics';

export { reportWebVitals };

export default function MyApp({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      {process.env.NODE_ENV === 'production' && (
        <script
          dangerouslySetInnerHTML={{
            __html: `
              // Custom RUM implementation
              window.addEventListener('load', () => {
                // Track page load time
                const loadTime = performance.timing.loadEventEnd - performance.timing.navigationStart;
                fetch('/api/analytics/performance', {
                  method: 'POST',
                  headers: { 'Content-Type': 'application/json' },
                  body: JSON.stringify({
                    metric: 'page_load_time',
                    value: loadTime,
                    url: window.location.href,
                  }),
                });
              });
            `,
          }}
        />
      )}
    </>
  );
}
```

### 2. Performance Budget
```javascript
// webpack.config.js - Performance budgets
module.exports = {
  performance: {
    maxAssetSize: 250000, // 250KB
    maxEntrypointSize: 400000, // 400KB
    hints: process.env.NODE_ENV === 'production' ? 'error' : 'warning',
  },
};
```

### 3. Monitoramento Contínuo de Desempenho
```yaml
# .github/workflows/performance.yml
name: Performance Audit
on: 
  pull_request:
    branches: [main]

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build Next.js
        run: npm run build
      
      - name: Start Next.js
        run: npm start &
        
      - name: Wait for server
        run: npx wait-on http://localhost:3000
      
      - name: Run Lighthouse CI
        run: |
          npm install -g @lhci/cli@0.12.x
          lhci autorun
        env:
          LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}
```

## Geração de Relatório de Desempenho

### 1. Relatório de Auditoria Abrangente
Gere relatório detalhado de desempenho incluindo:

#### Resumo Executivo
- Pontuação geral de desempenho (0-100)
- Status de Core Web Vitals
- Problemas-chave de desempenho
- Impacto na experiência do usuário

#### Análise Detalhada
- Análise detalhada de desempenho de carregamento
- Métricas de desempenho em runtime
- Análise de bundle e recomendações
- Oportunidades de otimização de imagens
- Otimizações de CSS e JavaScript

#### Plano de Ação
- Correções de alta prioridade (impacto imediato)
- Melhorias de prioridade média (impacto moderado)
- Estratégia de otimização de longo prazo
- Configuração de monitoramento de desempenho

#### Roadmap de Implementação
1. **Semana 1**: Correções críticas de desempenho
2. **Semana 2-3**: Otimização de imagens e ativos
3. **Semana 4**: Otimização de bundle e code splitting
4. **Contínuo**: Monitoramento de desempenho e prevenção de regressão

### 2. Dashboard de Rastreamento de Desempenho
```typescript
// Create performance dashboard component
const PerformanceDashboard = () => {
  return (
    <div className="performance-dashboard">
      <h2>Métricas de Desempenho</h2>
      
      {/* Core Web Vitals */}
      <section>
        <h3>Core Web Vitals</h3>
        <div className="metrics-grid">
          <MetricCard title="LCP" value="2.1s" target="< 2.5s" status="good" />
          <MetricCard title="FID" value="89ms" target="< 100ms" status="good" />
          <MetricCard title="CLS" value="0.08" target="< 0.1" status="good" />
        </div>
      </section>
      
      {/* Bundle Analysis */}
      <section>
        <h3>Análise de Bundle</h3>
        <BundleChart data={bundleData} />
      </section>
      
      {/* Performance Trends */}
      <section>
        <h3>Tendências de Desempenho</h3>
        <TrendChart metrics={performanceHistory} />
      </section>
    </div>
  );
};
```

Forneça auditoria abrangente de desempenho com recomendações específicas e mensuráveis, além de orientações de implementação para otimizações imediatas e de longo prazo.