---
allowed-tools: Read, Write, Edit
argument-hint: [nome-da-funcao] [--auth] [--geo] [--transform] [--proxy]
description: Gere Edge Functions otimizadas do Vercel com geolocalização, autenticação e transformação de dados
---

## Gerador de Edge Functions do Vercel

**Nome da Função**: $ARGUMENTS

## Análise Atual do Projeto

### Estrutura do Projeto
- Configuração Vercel: @vercel.json (se existir)
- Configuração Next.js: @next.config.js
- Rotas de API: @app/api/ ou @pages/api/
- Middleware: @middleware.ts (se existir)

### Detecção de Framework
- Package.json: @package.json
- Configuração TypeScript: @tsconfig.json (se existir)
- Variáveis de ambiente: @.env.local (se existir)

## Estratégia de Implementação de Edge Functions

### 1. Criação de Estrutura de Arquivos
Gere estrutura abrangente de edge function:
```
api/edge/[nome-da-funcao]/
├── index.ts                    # Edge function principal
├── types.ts                   # Tipos TypeScript
├── utils.ts                   # Funções utilitárias
├── config.ts                  # Configuração
└── __tests__/
    └── [nome-da-funcao].test.ts # Testes unitários
```

### 2. Template Base de Edge Function
```typescript
// api/edge/[nome-da-funcao]/index.ts
import { NextRequest, NextResponse } from 'next/server';

export const runtime = 'edge';

export async function GET(request: NextRequest) {
  try {
    // Obter dados de geolocalização
    const country = request.geo?.country || 'Unknown';
    const city = request.geo?.city || 'Unknown';
    const region = request.geo?.region || 'Unknown';
    
    // Obter metadados da requisição
    const ip = request.headers.get('x-forwarded-for') || 'Unknown';
    const userAgent = request.headers.get('user-agent') || 'Unknown';
    const referer = request.headers.get('referer') || 'Unknown';
    
    // Processar requisição
    const result = await processRequest({
      geo: { country, city, region },
      ip,
      userAgent,
      referer,
      url: request.url,
    });
    
    return NextResponse.json(result, {
      status: 200,
      headers: {
        'Cache-Control': 'public, s-maxage=60, stale-while-revalidate=300',
        'Content-Type': 'application/json',
        'X-Edge-Location': region,
      },
    });
    
  } catch (error) {
    console.error('Erro de edge function:', error);
    
    return NextResponse.json(
      { error: 'Erro interno do servidor' },
      { status: 500 }
    );
  }
}

export async function POST(request: NextRequest) {
  try {
    const body = await request.json();
    
    // Validar corpo da requisição
    const validationResult = validateRequestBody(body);
    if (!validationResult.valid) {
      return NextResponse.json(
        { error: 'Corpo da requisição inválido', details: validationResult.errors },
        { status: 400 }
      );
    }
    
    // Processar requisição POST
    const result = await processPostRequest(body, request);
    
    return NextResponse.json(result, {
      status: 201,
      headers: {
        'Content-Type': 'application/json',
      },
    });
    
  } catch (error) {
    console.error('Erro de POST da edge function:', error);
    
    return NextResponse.json(
      { error: 'Erro interno do servidor' },
      { status: 500 }
    );
  }
}

async function processRequest(metadata: RequestMetadata): Promise<any> {
  // Implemente a lógica da sua edge function aqui
  return {
    message: 'Edge function executada com sucesso',
    metadata,
    timestamp: new Date().toISOString(),
  };
}

async function processPostRequest(body: any, request: NextRequest): Promise<any> {
  // Implemente a lógica de POST aqui
  return {
    message: 'POST processado com sucesso',
    data: body,
    timestamp: new Date().toISOString(),
  };
}

function validateRequestBody(body: any): ValidationResult {
  // Implemente a lógica de validação
  return { valid: true, errors: [] };
}

interface RequestMetadata {
  geo: {
    country: string;
    city: string;
    region: string;
  };
  ip: string;
  userAgent: string;
  referer: string;
  url: string;
}

interface ValidationResult {
  valid: boolean;
  errors: string[];
}
```

## Tipos Especializados de Edge Function

### 1. Entrega de Conteúdo Baseada em Geolocalização
```typescript
// api/edge/geo-content/index.ts
import { NextRequest, NextResponse } from 'next/server';

export const runtime = 'edge';

interface ContentConfig {
  [country: string]: {
    currency: string;
    language: string;
    content: string;
    pricing: number;
  };
}

const contentConfig: ContentConfig = {
  'US': {
    currency: 'USD',
    language: 'en-US',
    content: 'Bem-vindo à nossa loja dos EUA!',
    pricing: 99.99,
  },
  'BR': {
    currency: 'BRL',
    language: 'pt-BR',
    content: 'Bem-vindo à nossa loja brasileira!',
    pricing: 499.99,
  },
  'DE': {
    currency: 'EUR',
    language: 'de-DE',
    content: 'Willkommen in unserem deutschen Shop!',
    pricing: 89.99,
  },
};

export async function GET(request: NextRequest) {
  const country = request.geo?.country || 'US';
  const config = contentConfig[country] || contentConfig['US'];
  
  // Adicionar headers específicos da região
  const response = NextResponse.json({
    country,
    ...config,
    edgeLocation: request.geo?.region,
    timestamp: new Date().toISOString(),
  });
  
  response.headers.set('Cache-Control', 'public, s-maxage=3600, stale-while-revalidate=86400');
  response.headers.set('Vary', 'Accept-Language, CloudFront-Viewer-Country');
  response.headers.set('Content-Language', config.language);
  
  return response;
}
```

### 2. Edge Function de Autenticação
```typescript
// api/edge/auth-check/index.ts
import { NextRequest, NextResponse } from 'next/server';
import { jwtVerify } from 'jose';

export const runtime = 'edge';

const JWT_SECRET = new TextEncoder().encode(
  process.env.JWT_SECRET || 'sua-chave-secreta'
);

export async function GET(request: NextRequest) {
  try {
    // Extrair token do header ou cookie
    const authHeader = request.headers.get('authorization');
    const cookieToken = request.cookies.get('auth-token')?.value;
    
    const token = authHeader?.replace('Bearer ', '') || cookieToken;
    
    if (!token) {
      return NextResponse.json(
        { error: 'Nenhum token fornecido', authenticated: false },
        { status: 401 }
      );
    }
    
    // Verificar token JWT
    const { payload } = await jwtVerify(token, JWT_SECRET);
    
    // Retornar informações do usuário
    return NextResponse.json({
      authenticated: true,
      user: {
        id: payload.sub,
        email: payload.email,
        role: payload.role,
        exp: payload.exp,
      },
      location: {
        country: request.geo?.country,
        city: request.geo?.city,
      },
    });
    
  } catch (error) {
    console.error('Falha na verificação de autenticação:', error);
    
    return NextResponse.json(
      { error: 'Token inválido', authenticated: false },
      { status: 401 }
    );
  }
}

export async function POST(request: NextRequest) {
  try {
    const { username, password } = await request.json();
    
    // Validar credenciais (implemente sua lógica)
    const user = await validateCredentials(username, password);
    
    if (!user) {
      return NextResponse.json(
        { error: 'Credenciais inválidas' },
        { status: 401 }
      );
    }
    
    // Gerar token JWT
    const token = await generateJWT(user);
    
    const response = NextResponse.json({
      success: true,
      user: {
        id: user.id,
        email: user.email,
        role: user.role,
      },
    });
    
    // Definir cookie seguro
    response.cookies.set('auth-token', token, {
      httpOnly: true,
      secure: process.env.NODE_ENV === 'production',
      sameSite: 'strict',
      maxAge: 24 * 60 * 60, // 24 horas
    });
    
    return response;
    
  } catch (error) {
    console.error('Erro de autenticação:', error);
    
    return NextResponse.json(
      { error: 'Falha na autenticação' },
      { status: 500 }
    );
  }
}

async function validateCredentials(username: string, password: string) {
  // Implemente validação de credenciais
  // Normalmente envolveria lookup em banco de dados
  return null; // Placeholder
}

async function generateJWT(user: any): Promise<string> {
  // Implemente geração de JWT
  // Usaria uma biblioteca JWT apropriada
  return 'jwt-token'; // Placeholder
}
```

### 3. Edge Function de Transformação de Dados
```typescript
// api/edge/transform/index.ts
import { NextRequest, NextResponse } from 'next/server';

export const runtime = 'edge';

interface TransformConfig {
  format: 'json' | 'xml' | 'csv';
  fields?: string[];
  transforms?: Record<string, (value: any) => any>;
}

const transformers = {
  // Conversão de moeda
  currency: (value: number, targetCurrency: string = 'USD') => {
    const rates = { USD: 1, EUR: 0.85, GBP: 0.73, BRL: 5.0 };
    return value * (rates[targetCurrency as keyof typeof rates] || 1);
  },
  
  // Formatação de data
  date: (value: string, format: string = 'ISO') => {
    const date = new Date(value);
    if (format === 'ISO') return date.toISOString();
    if (format === 'BR') return date.toLocaleDateString('pt-BR');
    return date.toString();
  },
  
  // Formatação de texto
  text: (value: string, caseType: string = 'lower') => {
    if (caseType === 'upper') return value.toUpperCase();
    if (caseType === 'title') return value.replace(/\w\S*/g, txt => 
      txt.charAt(0).toUpperCase() + txt.substr(1).toLowerCase()
    );
    return value.toLowerCase();
  },
};

export async function POST(request: NextRequest) {
  try {
    const { data, config }: { data: any; config: TransformConfig } = await request.json();
    
    if (!data || !config) {
      return NextResponse.json(
        { error: 'Dados ou configuração ausentes' },
        { status: 400 }
      );
    }
    
    // Aplicar transformações
    const transformedData = await transformData(data, config, request);
    
    // Formatar saída baseado no formato solicitado
    const output = await formatOutput(transformedData, config.format);
    
    const response = new NextResponse(output, {
      status: 200,
      headers: {
        'Content-Type': getContentType(config.format),
        'Cache-Control': 'public, s-maxage=300',
      },
    });
    
    return response;
    
  } catch (error) {
    console.error('Erro de transformação:', error);
    
    return NextResponse.json(
      { error: 'Falha na transformação' },
      { status: 500 }
    );
  }
}

async function transformData(data: any, config: TransformConfig, request: NextRequest) {
  const country = request.geo?.country || 'US';
  
  // Aplicar filtragem de campos se especificado
  if (config.fields && Array.isArray(data)) {
    data = data.map(item => {
      const filtered: any = {};
      config.fields!.forEach(field => {
        if (item.hasOwnProperty(field)) {
          filtered[field] = item[field];
        }
      });
      return filtered;
    });
  }
  
  // Aplicar transformações customizadas
  if (config.transforms) {
    Object.entries(config.transforms).forEach(([field, transformFunc]) => {
      if (Array.isArray(data)) {
        data = data.map(item => ({
          ...item,
          [field]: transformFunc(item[field]),
        }));
      } else if (data.hasOwnProperty(field)) {
        data[field] = transformFunc(data[field]);
      }
    });
  }
  
  // Adicionar contexto geo
  return {
    ...data,
    _meta: {
      transformedAt: new Date().toISOString(),
      location: country,
      edgeRegion: request.geo?.region,
    },
  };
}

async function formatOutput(data: any, format: string): Promise<string> {
  switch (format) {
    case 'xml':
      return jsonToXml(data);
    case 'csv':
      return jsonToCsv(data);
    case 'json':
    default:
      return JSON.stringify(data, null, 2);
  }
}

function getContentType(format: string): string {
  switch (format) {
    case 'xml': return 'application/xml';
    case 'csv': return 'text/csv';
    case 'json':
    default: return 'application/json';
  }
}

function jsonToXml(data: any): string {
  // Conversão XML simples (implemente biblioteca XML apropriada para produção)
  return `<?xml version="1.0" encoding="UTF-8"?><root>${JSON.stringify(data)}</root>`;
}

function jsonToCsv(data: any): string {
  // Conversão CSV simples (implemente biblioteca CSV apropriada para produção)
  if (Array.isArray(data) && data.length > 0) {
    const headers = Object.keys(data[0]);
    const rows = data.map(row => headers.map(header => row[header] || '').join(','));
    return [headers.join(','), ...rows].join('\n');
  }
  return '';
}
```

### 4. Edge Function de Proxy e Cache
```typescript
// api/edge/proxy/index.ts
import { NextRequest, NextResponse } from 'next/server';

export const runtime = 'edge';

interface ProxyConfig {
  targetUrl: string;
  cacheTime: number;
  headers?: Record<string, string>;
  transformResponse?: boolean;
}

const proxyConfigs: Record<string, ProxyConfig> = {
  'api': {
    targetUrl: 'https://jsonplaceholder.typicode.com',
    cacheTime: 300, // 5 minutos
    headers: {
      'User-Agent': 'Vercel-Edge-Proxy/1.0',
    },
  },
  'cdn': {
    targetUrl: 'https://cdn.example.com',
    cacheTime: 3600, // 1 hora
    transformResponse: false,
  },
};

export async function GET(request: NextRequest) {
  try {
    const url = new URL(request.url);
    const proxyType = url.searchParams.get('type') || 'api';
    const targetPath = url.searchParams.get('path') || '';
    
    const config = proxyConfigs[proxyType];
    if (!config) {
      return NextResponse.json(
        { error: 'Tipo de proxy inválido' },
        { status: 400 }
      );
    }
    
    // Construir URL alvo
    const targetUrl = `${config.targetUrl}${targetPath}`;
    
    // Verificar cache primeiro (simplificado - usar cache apropriado em produção)
    const cacheKey = `proxy:${targetUrl}`;
    
    // Fazer requisição ao alvo
    const response = await fetch(targetUrl, {
      headers: {
        ...config.headers,
        'X-Forwarded-For': request.headers.get('x-forwarded-for') || '',
        'X-Real-IP': request.headers.get('x-real-ip') || '',
      },
    });
    
    if (!response.ok) {
      return NextResponse.json(
        { error: 'Erro do servidor upstream' },
        { status: response.status }
      );
    }
    
    let data;
    const contentType = response.headers.get('content-type') || '';
    
    if (contentType.includes('application/json')) {
      data = await response.json();
      
      // Transformar resposta se configurado
      if (config.transformResponse) {
        data = await transformProxyResponse(data, request);
      }
      
      return NextResponse.json(data, {
        status: 200,
        headers: {
          'Cache-Control': `public, s-maxage=${config.cacheTime}, stale-while-revalidate=${config.cacheTime * 2}`,
          'X-Proxy-Cache': 'MISS',
          'X-Edge-Location': request.geo?.region || 'unknown',
        },
      });
    } else {
      // Para respostas que não são JSON, passar adiante
      const blob = await response.blob();
      
      return new NextResponse(blob, {
        status: 200,
        headers: {
          'Content-Type': contentType,
          'Cache-Control': `public, s-maxage=${config.cacheTime}`,
        },
      });
    }
    
  } catch (error) {
    console.error('Erro de proxy:', error);
    
    return NextResponse.json(
      { error: 'Falha na requisição de proxy' },
      { status: 502 }
    );
  }
}

async function transformProxyResponse(data: any, request: NextRequest) {
  // Adicionar contexto geo aos dados proxied
  return {
    ...data,
    _proxy: {
      timestamp: new Date().toISOString(),
      location: request.geo?.country,
      region: request.geo?.region,
    },
  };
}
```

## Utilitários de Edge Function

### 1. Gerenciamento de Configuração
```typescript
// api/edge/[nome-da-funcao]/config.ts
export interface EdgeFunctionConfig {
  cacheTime: number;
  rateLimit: {
    requests: number;
    windowMs: number;
  };
  geo: {
    enabled: boolean;
    restrictedCountries?: string[];
  };
  security: {
    corsOrigins: string[];
    requireAuth: boolean;
  };
}

export const defaultConfig: EdgeFunctionConfig = {
  cacheTime: 300, // 5 minutos
  rateLimit: {
    requests: 100,
    windowMs: 60000, // 1 minuto
  },
  geo: {
    enabled: true,
  },
  security: {
    corsOrigins: ['*'],
    requireAuth: false,
  },
};
```

### 2. Funções Utilitárias
```typescript
// api/edge/[nome-da-funcao]/utils.ts
export function getClientIP(request: NextRequest): string {
  return request.headers.get('x-forwarded-for') ||
    request.headers.get('x-real-ip') ||
    request.ip ||
    'unknown';
}

export function generateCacheKey(request: NextRequest, suffix?: string): string {
  const url = new URL(request.url);
  const baseKey = `${url.pathname}${url.search}`;
  return suffix ? `${baseKey}:${suffix}` : baseKey;
}

export function createCorsResponse(
  data: any,
  origins: string[] = ['*']
): NextResponse {
  const response = NextResponse.json(data);
  
  response.headers.set('Access-Control-Allow-Origin', origins.join(', '));
  response.headers.set('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
  response.headers.set('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  
  return response;
}

export function validateGeoRestrictions(
  request: NextRequest,
  restrictedCountries: string[] = []
): boolean {
  const country = request.geo?.country;
  return !country || !restrictedCountries.includes(country);
}
```

### 3. Framework de Testes
```typescript
// api/edge/[nome-da-funcao]/__tests__/[nome-da-funcao].test.ts
import { NextRequest } from 'next/server';
import { GET, POST } from '../index';

// Mock de dados geo
const createMockRequest = (url: string, options: any = {}) => {
  const request = new NextRequest(url, options);
  
  // Propriedade geo simulada
  Object.defineProperty(request, 'geo', {
    value: {
      country: 'BR',
      city: 'São Paulo',
      region: 'sa-east-1',
    },
  });
  
  return request;
};

describe('Edge Function', () => {
  describe('Requisições GET', () => {
    it('deve retornar conteúdo baseado em geo', async () => {
      const request = createMockRequest('http://localhost:3000/api/edge/test');
      const response = await GET(request);
      const data = await response.json();
      
      expect(response.status).toBe(200);
      expect(data.metadata.geo.country).toBe('BR');
    });
    
    it('deve lidar com dados geo ausentes', async () => {
      const request = new NextRequest('http://localhost:3000/api/edge/test');
      const response = await GET(request);
      const data = await response.json();
      
      expect(response.status).toBe(200);
      expect(data.metadata.geo.country).toBe('Unknown');
    });
  });
  
  describe('Requisições POST', () => {
    it('deve validar corpo da requisição', async () => {
      const request = createMockRequest('http://localhost:3000/api/edge/test', {
        method: 'POST',
        body: JSON.stringify({ invalid: 'data' }),
        headers: { 'Content-Type': 'application/json' },
      });
      
      const response = await POST(request);
      const data = await response.json();
      
      expect(response.status).toBe(400);
      expect(data.error).toBe('Corpo da requisição inválido');
    });
  });
});
```

## Performance e Otimização

### 1. Otimização de Resposta
```typescript
// Otimizar respostas para performance de edge
export function optimizeResponse(data: any, request: NextRequest): NextResponse {
  const response = NextResponse.json(data);
  
  // Definir headers de cache apropriados
  const cacheTime = getCacheTime(request.url);
  response.headers.set(
    'Cache-Control',
    `public, s-maxage=${cacheTime}, stale-while-revalidate=${cacheTime * 2}`
  );
  
  // Adicionar dicas de compressão
  response.headers.set('Content-Encoding', 'gzip');
  
  // Adicionar headers de performance
  response.headers.set('X-Edge-Location', request.geo?.region || 'unknown');
  
  return response;
}

function getCacheTime(url: string): number {
  // Tempo de cache dinâmico baseado em padrões de URL
  if (url.includes('/static/')) return 3600; // 1 hora
  if (url.includes('/api/')) return 60; // 1 minuto
  return 300; // 5 minutos padrão
}
```

### 2. Tratamento de Erros
```typescript
export function createErrorResponse(
  error: unknown,
  request: NextRequest
): NextResponse {
  console.error('Erro de edge function:', error);
  
  // Registrar erro com contexto
  const errorContext = {
    url: request.url,
    method: request.method,
    country: request.geo?.country,
    timestamp: new Date().toISOString(),
  };
  
  // Retornar resposta de erro apropriada
  return NextResponse.json(
    {
      error: 'Erro interno do servidor',
      requestId: generateRequestId(),
    },
    {
      status: 500,
      headers: {
        'X-Error-Context': JSON.stringify(errorContext),
      },
    }
  );
}

function generateRequestId(): string {
  return Math.random().toString(36).substr(2, 9);
}
```

Gere implementação abrangente de edge function com os recursos solicitados, tipos TypeScript apropriados, tratamento de erros e padrões de otimização.