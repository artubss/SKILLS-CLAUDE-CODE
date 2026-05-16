---
allowed-tools: Read, Write, Edit
argument-hint: [tipo-middleware] [--auth] [--rate-limit] [--redirect] [--rewrite]
description: Criar middleware Next.js otimizado com autenticação, rate limiting e lógica de roteamento
---

## Criador de Middleware Next.js

**Tipo de Middleware**: $ARGUMENTS

## Análise do Projeto Atual

### Estrutura do Projeto
- Configuração Next.js: @next.config.js
- Middleware existente: @middleware.ts ou @middleware.js (se existir)
- Diretório app: @app/ (se usar App Router)
- Configuração de autenticação: @auth.config.ts ou @lib/auth/ (se existir)

### Detecção de Framework
- Package.json: @package.json
- Configuração TypeScript: @tsconfig.json (se existir)
- Bibliotecas de autenticação: Detectar NextAuth.js, Auth0 ou autenticação customizada

## Estratégia de Implementação de Middleware

### 1. Estrutura de Arquivo de Middleware
Criar middleware abrangente na raiz do projeto:
```
middleware.ts                 # Arquivo principal de middleware
lib/middleware/              # Utilitários de middleware
├── auth.ts                  # Middleware de autenticação
├── rateLimit.ts            # Lógica de rate limiting
├── redirects.ts            # Regras de redirecionamento
├── rewrites.ts             # Reescrita de URL
├── cors.ts                 # Tratamento de CORS
├── security.ts             # Headers de segurança
└── types.ts               # Tipos TypeScript
```

### 2. Template de Middleware Base
```typescript
// middleware.ts
import { NextRequest, NextResponse } from 'next/server';
import { authMiddleware } from './lib/middleware/auth';
import { rateLimitMiddleware } from './lib/middleware/rateLimit';
import { securityMiddleware } from './lib/middleware/security';
import { redirectMiddleware } from './lib/middleware/redirects';

export async function middleware(request: NextRequest) {
  const { pathname } = request.nextUrl;
  
  // Aplicar headers de segurança primeiro
  let response = await securityMiddleware(request);
  
  // Aplicar rate limiting
  const rateLimitResult = await rateLimitMiddleware(request);
  if (rateLimitResult) return rateLimitResult;
  
  // Tratar autenticação para rotas protegidas
  if (isProtectedRoute(pathname)) {
    const authResult = await authMiddleware(request);
    if (authResult) return authResult;
  }
  
  // Tratar redirecionamentos
  const redirectResult = await redirectMiddleware(request);
  if (redirectResult) return redirectResult;
  
  // Aplicar headers adicionais à resposta
  if (response) {
    return response;
  }
  
  return NextResponse.next();
}

function isProtectedRoute(pathname: string): boolean {
  const protectedPaths = ['/dashboard', '/admin', '/api/protected'];
  return protectedPaths.some(path => pathname.startsWith(path));
}

export const config = {
  matcher: [
    // Fazer match com todos os paths exceto arquivos estáticos e imagens
    '/((?!_next/static|_next/image|favicon.ico|.*\\.(?:svg|png|jpg|jpeg|gif|webp)$).*)',
  ],
};
```

## Componentes de Middleware

### 1. Middleware de Autenticação
```typescript
// lib/middleware/auth.ts
import { NextRequest, NextResponse } from 'next/server';
import { jwtVerify } from 'jose';

const JWT_SECRET = new TextEncoder().encode(
  process.env.JWT_SECRET || 'your-secret-key'
);

export async function authMiddleware(request: NextRequest) {
  try {
    // Obter token de cookies ou header Authorization
    const token = request.cookies.get('auth-token')?.value ||
      request.headers.get('authorization')?.replace('Bearer ', '');

    if (!token) {
      return redirectToLogin(request);
    }

    // Verificar token JWT
    const { payload } = await jwtVerify(token, JWT_SECRET);
    
    // Adicionar informações de usuário aos headers para uso posterior
    const response = NextResponse.next();
    response.headers.set('x-user-id', payload.sub as string);
    response.headers.set('x-user-role', payload.role as string);
    
    return response;
    
  } catch (error) {
    console.error('Erro no middleware de autenticação:', error);
    return redirectToLogin(request);
  }
}

function redirectToLogin(request: NextRequest) {
  const loginUrl = new URL('/login', request.url);
  loginUrl.searchParams.set('callbackUrl', request.url);
  return NextResponse.redirect(loginUrl);
}

// Controle de acesso baseado em papel
export function requireRole(allowedRoles: string[]) {
  return async function roleMiddleware(request: NextRequest) {
    const userRole = request.headers.get('x-user-role');
    
    if (!userRole || !allowedRoles.includes(userRole)) {
      return new NextResponse('Acesso negado', { status: 403 });
    }
    
    return NextResponse.next();
  };
}
```

### 2. Middleware de Rate Limiting
```typescript
// lib/middleware/rateLimit.ts
import { NextRequest, NextResponse } from 'next/server';

// Armazenamento simples em memória (usar Redis em produção)
const requestCounts = new Map<string, { count: number; resetTime: number }>();

interface RateLimitConfig {
  windowMs: number; // Janela de tempo em milissegundos
  maxRequests: number; // Máximo de requisições por janela
  keyGenerator?: (request: NextRequest) => string;
}

const defaultConfig: RateLimitConfig = {
  windowMs: 15 * 60 * 1000, // 15 minutos
  maxRequests: 100, // 100 requisições por 15 minutos
};

export async function rateLimitMiddleware(
  request: NextRequest,
  config: RateLimitConfig = defaultConfig
) {
  const key = config.keyGenerator 
    ? config.keyGenerator(request)
    : getClientIP(request);
  
  const now = Date.now();
  const clientData = requestCounts.get(key);
  
  // Resetar janela se expirada
  if (!clientData || now > clientData.resetTime) {
    requestCounts.set(key, {
      count: 1,
      resetTime: now + config.windowMs
    });
    return null; // Permitir requisição
  }
  
  // Incrementar contador
  clientData.count++;
  
  // Verificar se limite foi excedido
  if (clientData.count > config.maxRequests) {
    const resetTime = Math.ceil((clientData.resetTime - now) / 1000);
    
    return new NextResponse('Limite de taxa excedido', {
      status: 429,
      headers: {
        'X-RateLimit-Limit': config.maxRequests.toString(),
        'X-RateLimit-Remaining': '0',
        'X-RateLimit-Reset': resetTime.toString(),
        'Retry-After': resetTime.toString(),
      },
    });
  }
  
  return null; // Permitir requisição
}

function getClientIP(request: NextRequest): string {
  return request.headers.get('x-forwarded-for') ||
    request.headers.get('x-real-ip') ||
    request.ip ||
    'unknown';
}

// Rate limiting específico para API
export const apiRateLimit = (request: NextRequest) =>
  rateLimitMiddleware(request, {
    windowMs: 60 * 1000, // 1 minuto
    maxRequests: 60, // 60 requisições por minuto
    keyGenerator: (req) => `api:${getClientIP(req)}`,
  });
```

### 3. Middleware de Headers de Segurança
```typescript
// lib/middleware/security.ts
import { NextRequest, NextResponse } from 'next/server';

export async function securityMiddleware(request: NextRequest) {
  const response = NextResponse.next();
  
  // Headers de segurança
  const securityHeaders = {
    // Proteção XSS
    'X-XSS-Protection': '1; mode=block',
    
    // Opções de tipo de conteúdo
    'X-Content-Type-Options': 'nosniff',
    
    // Opções de frame
    'X-Frame-Options': 'DENY',
    
    // HSTS
    'Strict-Transport-Security': 'max-age=31536000; includeSubDomains',
    
    // Política de referência
    'Referrer-Policy': 'strict-origin-when-cross-origin',
    
    // Política de permissões
    'Permissions-Policy': 'camera=(), microphone=(), geolocation=()',
    
    // Política de segurança de conteúdo
    'Content-Security-Policy': generateCSP(),
  };
  
  // Aplicar headers de segurança
  Object.entries(securityHeaders).forEach(([key, value]) => {
    response.headers.set(key, value);
  });
  
  return response;
}

function generateCSP(): string {
  const csp = [
    "default-src 'self'",
    "script-src 'self' 'unsafe-eval' 'unsafe-inline'",
    "style-src 'self' 'unsafe-inline'",
    "img-src 'self' data: https:",
    "font-src 'self' data:",
    "connect-src 'self'",
    "frame-ancestors 'none'",
  ];
  
  return csp.join('; ');
}
```

### 4. Middleware de CORS
```typescript
// lib/middleware/cors.ts
import { NextRequest, NextResponse } from 'next/server';

interface CorsOptions {
  origin: string | string[] | boolean;
  methods: string[];
  allowedHeaders: string[];
  credentials: boolean;
}

const defaultCorsOptions: CorsOptions = {
  origin: true, // Permitir todas as origens em desenvolvimento
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
  allowedHeaders: ['Content-Type', 'Authorization', 'X-Requested-With'],
  credentials: true,
};

export function corsMiddleware(options: Partial<CorsOptions> = {}) {
  const config = { ...defaultCorsOptions, ...options };
  
  return function cors(request: NextRequest) {
    const response = NextResponse.next();
    const origin = request.headers.get('origin');
    
    // Tratar requisições preflight
    if (request.method === 'OPTIONS') {
      return handlePreflight(request, config);
    }
    
    // Definir headers de CORS
    if (shouldAllowOrigin(origin, config.origin)) {
      response.headers.set('Access-Control-Allow-Origin', origin || '*');
    }
    
    if (config.credentials) {
      response.headers.set('Access-Control-Allow-Credentials', 'true');
    }
    
    response.headers.set(
      'Access-Control-Allow-Methods',
      config.methods.join(', ')
    );
    
    response.headers.set(
      'Access-Control-Allow-Headers',
      config.allowedHeaders.join(', ')
    );
    
    return response;
  };
}

function handlePreflight(request: NextRequest, config: CorsOptions) {
  const headers = new Headers();
  const origin = request.headers.get('origin');
  
  if (shouldAllowOrigin(origin, config.origin)) {
    headers.set('Access-Control-Allow-Origin', origin || '*');
  }
  
  if (config.credentials) {
    headers.set('Access-Control-Allow-Credentials', 'true');
  }
  
  headers.set('Access-Control-Allow-Methods', config.methods.join(', '));
  headers.set('Access-Control-Allow-Headers', config.allowedHeaders.join(', '));
  headers.set('Access-Control-Max-Age', '86400'); // 24 horas
  
  return new NextResponse(null, { status: 200, headers });
}

function shouldAllowOrigin(
  origin: string | null,
  allowedOrigin: string | string[] | boolean
): boolean {
  if (allowedOrigin === true) return true;
  if (allowedOrigin === false) return false;
  if (typeof allowedOrigin === 'string') return origin === allowedOrigin;
  if (Array.isArray(allowedOrigin)) return allowedOrigin.includes(origin || '');
  return false;
}
```

### 5. Middleware de Redirecionamento e Reescrita
```typescript
// lib/middleware/redirects.ts
import { NextRequest, NextResponse } from 'next/server';

interface RedirectRule {
  source: string | RegExp;
  destination: string;
  permanent?: boolean;
  conditions?: (request: NextRequest) => boolean;
}

const redirectRules: RedirectRule[] = [
  // Redirecionamento de URLs legadas
  {
    source: '/old-page',
    destination: '/new-page',
    permanent: true,
  },
  
  // Redirecionamentos dinâmicos
  {
    source: /^\/user\/(.+)$/,
    destination: '/profile/$1',
    permanent: false,
  },
  
  // Redirecionamentos condicionais
  {
    source: '/admin',
    destination: '/admin/dashboard',
    conditions: (request) => {
      const userRole = request.headers.get('x-user-role');
      return userRole === 'admin';
    },
  },
  
  // Modo de manutenção
  {
    source: /.*/,
    destination: '/maintenance',
    conditions: (request) => {
      return process.env.MAINTENANCE_MODE === 'true' &&
        !request.nextUrl.pathname.startsWith('/maintenance');
    },
  },
];

export async function redirectMiddleware(request: NextRequest) {
  const { pathname } = request.nextUrl;
  
  for (const rule of redirectRules) {
    if (shouldApplyRule(rule, pathname, request)) {
      const destination = resolveDestination(rule.destination, pathname);
      const url = new URL(destination, request.url);
      
      return NextResponse.redirect(url, {
        status: rule.permanent ? 301 : 302,
      });
    }
  }
  
  return null; // Sem redirecionamento necessário
}

function shouldApplyRule(
  rule: RedirectRule,
  pathname: string,
  request: NextRequest
): boolean {
  // Verificar correspondência de padrão
  const matches = typeof rule.source === 'string'
    ? pathname === rule.source
    : rule.source.test(pathname);
  
  if (!matches) return false;
  
  // Verificar condições adicionais
  if (rule.conditions) {
    return rule.conditions(request);
  }
  
  return true;
}

function resolveDestination(destination: string, pathname: string): string {
  // Tratar substituições dinâmicas
  return destination.replace(/\$(\d+)/g, (match, num) => {
    // Extrair de correspondências de regex
    return pathname; // Simplificado - precisaria de correspondência real de regex
  });
}
```

### 6. Middleware de Teste A/B
```typescript
// lib/middleware/abTest.ts
import { NextRequest, NextResponse } from 'next/server';

interface ABTest {
  name: string;
  variants: string[];
  traffic: number; // Porcentagem do tráfego a incluir (0-100)
  condition?: (request: NextRequest) => boolean;
}

const activeTests: ABTest[] = [
  {
    name: 'homepage-design',
    variants: ['control', 'variant-a', 'variant-b'],
    traffic: 50,
  },
  {
    name: 'checkout-flow',
    variants: ['old-checkout', 'new-checkout'],
    traffic: 100,
    condition: (req) => req.nextUrl.pathname.startsWith('/checkout'),
  },
];

export function abTestMiddleware(request: NextRequest) {
  const response = NextResponse.next();
  
  for (const test of activeTests) {
    // Verificar se usuário deve ser incluído no teste
    if (test.condition && !test.condition(request)) continue;
    
    // Verificar alocação de tráfego
    const userId = getUserId(request);
    const hash = hashString(userId + test.name);
    const bucket = hash % 100;
    
    if (bucket >= test.traffic) continue;
    
    // Atribuir variante
    const variantIndex = hash % test.variants.length;
    const variant = test.variants[variantIndex];
    
    // Definir cookie para experiência consistente
    response.cookies.set(`ab_${test.name}`, variant, {
      maxAge: 30 * 24 * 60 * 60, // 30 dias
      httpOnly: false, // Permitir acesso no lado do cliente
    });
    
    // Definir header para uso no lado do servidor
    response.headers.set(`x-ab-${test.name}`, variant);
  }
  
  return response;
}

function getUserId(request: NextRequest): string {
  // Obter ID de usuário do cookie ou gerar ID anônimo
  return request.cookies.get('user-id')?.value ||
    request.headers.get('x-forwarded-for') ||
    'anonymous';
}

function hashString(str: string): number {
  let hash = 0;
  for (let i = 0; i < str.length; i++) {
    const char = str.charCodeAt(i);
    hash = ((hash << 5) - hash) + char;
    hash = hash & hash; // Converter para inteiro de 32 bits
  }
  return Math.abs(hash);
}
```

## Padrões Avançados de Middleware

### 1. Composição de Middleware
```typescript
// lib/middleware/compose.ts
import { NextRequest, NextResponse } from 'next/server';

type MiddlewareFunction = (
  request: NextRequest,
  response?: NextResponse
) => NextResponse | Promise<NextResponse> | null;

export function composeMiddleware(...middlewares: MiddlewareFunction[]) {
  return async function composedMiddleware(request: NextRequest) {
    let response: NextResponse | null = null;
    
    for (const middleware of middlewares) {
      const result = await middleware(request, response || undefined);
      
      if (result && result.status >= 300 && result.status < 400) {
        // Tratar redirecionamentos imediatamente
        return result;
      }
      
      if (result && result.status >= 400) {
        // Tratar erros imediatamente  
        return result;
      }
      
      if (result) {
        response = result;
      }
    }
    
    return response || NextResponse.next();
  };
}
```

### 2. Middleware de Feature Flags
```typescript
// lib/middleware/featureFlags.ts
import { NextRequest, NextResponse } from 'next/server';

interface FeatureFlag {
  name: string;
  enabled: boolean;
  percentage?: number;
  userGroups?: string[];
  geoRegions?: string[];
}

const featureFlags: FeatureFlag[] = [
  {
    name: 'new-dashboard',
    enabled: true,
    percentage: 25,
  },
  {
    name: 'premium-features',
    enabled: true,
    userGroups: ['premium', 'admin'],
  },
];

export function featureFlagMiddleware(request: NextRequest) {
  const response = NextResponse.next();
  const activeFlags: Record<string, boolean> = {};
  
  for (const flag of featureFlags) {
    if (!flag.enabled) {
      activeFlags[flag.name] = false;
      continue;
    }
    
    // Verificar rollout percentual
    if (flag.percentage) {
      const userId = getUserId(request);
      const hash = hashString(userId + flag.name) % 100;
      if (hash >= flag.percentage) {
        activeFlags[flag.name] = false;
        continue;
      }
    }
    
    // Verificar grupos de usuário
    if (flag.userGroups) {
      const userRole = request.headers.get('x-user-role');
      if (!userRole || !flag.userGroups.includes(userRole)) {
        activeFlags[flag.name] = false;
        continue;
      }
    }
    
    activeFlags[flag.name] = true;
  }
  
  // Definir feature flags nos headers
  response.headers.set('x-feature-flags', JSON.stringify(activeFlags));
  
  return response;
}
```

## Testes de Middleware

### 1. Testes Unitários
```typescript
// __tests__/middleware.test.ts
import { NextRequest } from 'next/server';
import { middleware } from '../middleware';

describe('Middleware', () => {
  it('deve adicionar headers de segurança', async () => {
    const request = new NextRequest('http://localhost:3000/');
    const response = await middleware(request);
    
    expect(response.headers.get('X-Frame-Options')).toBe('DENY');
    expect(response.headers.get('X-Content-Type-Options')).toBe('nosniff');
  });

  it('deve redirecionar usuários não autenticados de rotas protegidas', async () => {
    const request = new NextRequest('http://localhost:3000/dashboard');
    const response = await middleware(request);
    
    expect(response.status).toBe(302);
    expect(response.headers.get('location')).toContain('/login');
  });
});
```

### 2. Testes de Integração
```typescript
// __tests__/middleware.integration.test.ts
describe('Integração de Middleware', () => {
  it('deve tratar fluxo de autenticação completo', async () => {
    // Testar fluxo login -> dashboard -> logout
  });
  
  it('deve respeitar rate limiting', async () => {
    // Testar múltiplas requisições atingindo limite de taxa
  });
});
```

## Deploy e Monitoramento

### 1. Monitoramento de Performance
```typescript
// lib/middleware/monitoring.ts
export function monitoringMiddleware(request: NextRequest) {
  const start = Date.now();
  
  return new Response(JSON.stringify({}), {
    status: 200,
    headers: {
      'x-response-time': `${Date.now() - start}ms`,
    },
  });
}
```

### 2. Tratamento de Erros
```typescript
// lib/middleware/errorHandler.ts
export function errorHandlerMiddleware(
  error: Error,
  request: NextRequest
): NextResponse {
  console.error('Erro no middleware:', error);
  
  // Registrar em serviço de monitoramento
  // logError(error, request);
  
  return new NextResponse('Erro interno do servidor', { status: 500 });
}
```

Gerar implementação de middleware abrangente com todos os recursos solicitados, tipos TypeScript apropriados e padrões prontos para produção.