---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [route-path] [--method=GET] [--data='{}'] [--headers='{}']
description: Teste e valide rotas de API do Next.js com cenários de teste abrangentes
---

## Testador de Rota de API do Next.js

**Rota de API**: $ARGUMENTS

## Análise do Projeto Atual

### Detecção de Rotas de API
- API do App Router: @app/api/
- API do Pages Router: @pages/api/
- Configuração de API: @next.config.js
- Variáveis de ambiente: @.env.local

### Contexto do Projeto
- Versão do Next.js: !`grep '"next"' package.json | head -1`
- Configuração TypeScript: @tsconfig.json (se existe)
- Framework de testes: @jest.config.js ou @vitest.config.js (se existe)

## Análise da Rota de API

### Descoberta de Rota
Com base no caminho da rota fornecido, analise:
- **Arquivo de Rota**: Localize o arquivo de rota real
- **Métodos HTTP**: Métodos suportados (GET, POST, PUT, DELETE, PATCH)
- **Parâmetros de Rota**: Segmentos dinâmicos e parâmetros de query
- **Middleware**: Funções de middleware aplicadas
- **Autenticação**: Autenticação/autorização necessária

### Revisão da Implementação da Rota
- Implementação do manipulador de rota: @app/api/[route-path]/route.ts ou @pages/api/[route-path].ts
- Definições de tipo: @types/ ou tipos inline
- Esquemas de validação: @lib/validations/ ou validação inline
- Modelos de banco de dados: @lib/models/ ou @models/

## Estratégia de Geração de Testes

### 1. Testes de Funcionalidade Básica
```javascript
// Modelo de teste de rota de API
describe('Rota de API: /api/[route-path]', () => {
  describe('Requisições GET', () => {
    test('deve retornar 200 para requisição válida', async () => {
      const response = await fetch('/api/[route-path]');
      expect(response.status).toBe(200);
    });

    test('deve retornar resposta JSON válida', async () => {
      const response = await fetch('/api/[route-path]');
      const data = await response.json();
      expect(data).toBeDefined();
      expect(typeof data).toBe('object');
    });
  });

  describe('Requisições POST', () => {
    test('deve criar recurso com dados válidos', async () => {
      const testData = { name: 'Test', email: 'test@example.com' };
      const response = await fetch('/api/[route-path]', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(testData)
      });
      
      expect(response.status).toBe(201);
      const result = await response.json();
      expect(result.name).toBe(testData.name);
    });

    test('deve rejeitar dados inválidos', async () => {
      const invalidData = { invalid: 'field' };
      const response = await fetch('/api/[route-path]', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(invalidData)
      });
      
      expect(response.status).toBe(400);
    });
  });
});
```

### 2. Testes de Autenticação
```javascript
describe('Autenticação', () => {
  test('deve exigir autenticação para rotas protegidas', async () => {
    const response = await fetch('/api/protected-route');
    expect(response.status).toBe(401);
  });

  test('deve permitir requisições autenticadas', async () => {
    const token = 'valid-jwt-token';
    const response = await fetch('/api/protected-route', {
      headers: { 'Authorization': `Bearer ${token}` }
    });
    expect(response.status).not.toBe(401);
  });

  test('deve validar formato de token JWT', async () => {
    const invalidToken = 'invalid-token';
    const response = await fetch('/api/protected-route', {
      headers: { 'Authorization': `Bearer ${invalidToken}` }
    });
    expect(response.status).toBe(403);
  });
});
```

### 3. Testes de Validação de Entrada
```javascript
describe('Validação de Entrada', () => {
  const validationTests = [
    { field: 'email', invalid: 'not-an-email', valid: 'test@example.com' },
    { field: 'phone', invalid: '123', valid: '+1234567890' },
    { field: 'age', invalid: -1, valid: 25 },
    { field: 'name', invalid: '', valid: 'John Doe' }
  ];

  validationTests.forEach(({ field, invalid, valid }) => {
    test(`deve validar campo ${field}`, async () => {
      const invalidData = { [field]: invalid };
      const validData = { [field]: valid };

      // Testa dados inválidos
      const invalidResponse = await fetch('/api/[route-path]', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(invalidData)
      });
      expect(invalidResponse.status).toBe(400);

      // Testa dados válidos
      const validResponse = await fetch('/api/[route-path]', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(validData)
      });
      expect(validResponse.status).not.toBe(400);
    });
  });
});
```

### 4. Testes de Tratamento de Erros
```javascript
describe('Tratamento de Erros', () => {
  test('deve lidar com JSON malformado', async () => {
    const response = await fetch('/api/[route-path]', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: 'invalid-json'
    });
    expect(response.status).toBe(400);
  });

  test('deve lidar com cabeçalho Content-Type ausente', async () => {
    const response = await fetch('/api/[route-path]', {
      method: 'POST',
      body: JSON.stringify({ test: 'data' })
    });
    expect(response.status).toBe(400);
  });

  test('deve lidar com timeout de requisição', async () => {
    // Mock de endpoint lento
    jest.setTimeout(5000);
    const response = await fetch('/api/slow-endpoint');
    // Testa tratamento apropriado de timeout
  }, 5000);

  test('deve lidar com erros de conexão com banco de dados', async () => {
    // Mock de falha de banco de dados
    const mockDbError = jest.spyOn(db, 'connect').mockRejectedValue(new Error('DB Error'));
    
    const response = await fetch('/api/[route-path]');
    expect(response.status).toBe(500);
    
    mockDbError.mockRestore();
  });
});
```

### 5. Testes de Performance
```javascript
describe('Performance', () => {
  test('deve responder em tempo aceitável', async () => {
    const startTime = Date.now();
    const response = await fetch('/api/[route-path]');
    const endTime = Date.now();
    
    expect(response.status).toBe(200);
    expect(endTime - startTime).toBeLessThan(1000); // 1 segundo
  });

  test('deve lidar com requisições concorrentes', async () => {
    const promises = Array.from({ length: 10 }, () =>
      fetch('/api/[route-path]')
    );
    
    const responses = await Promise.all(promises);
    responses.forEach(response => {
      expect(response.status).toBe(200);
    });
  });

  test('deve implementar rate limiting', async () => {
    const requests = Array.from({ length: 100 }, () =>
      fetch('/api/[route-path]')
    );
    
    const responses = await Promise.all(requests);
    const rateLimitedResponses = responses.filter(r => r.status === 429);
    expect(rateLimitedResponses.length).toBeGreaterThan(0);
  });
});
```

## Comandos para Testes Manuais

### Geração de Comandos cURL
```bash
# Requisição GET
curl -X GET "http://localhost:3000/api/[route-path]" \
  -H "Accept: application/json"

# Requisição POST com dados
curl -X POST "http://localhost:3000/api/[route-path]" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"key": "value"}'

# Requisição autenticada
curl -X GET "http://localhost:3000/api/protected-route" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Accept: application/json"

# Upload de arquivo
curl -X POST "http://localhost:3000/api/upload" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -F "file=@path/to/file.jpg"
```

### Comandos HTTPie
```bash
# Requisição GET
http GET localhost:3000/api/[route-path]

# Requisição POST com JSON
http POST localhost:3000/api/[route-path] key=value

# Requisição autenticada
http GET localhost:3000/api/protected-route Authorization:"Bearer TOKEN"

# Cabeçalhos customizados
http GET localhost:3000/api/[route-path] X-Custom-Header:value
```

## Ferramentas de Teste Interativo

### Geração de Coleção Postman
```json
{
  "info": {
    "name": "Testes de API Next.js",
    "description": "Testes de API gerados para [route-path]"
  },
  "item": [
    {
      "name": "GET [route-path]",
      "request": {
        "method": "GET",
        "header": [],
        "url": {
          "raw": "{{baseUrl}}/api/[route-path]",
          "host": ["{{baseUrl}}"],
          "path": ["api", "[route-path]"]
        }
      }
    },
    {
      "name": "POST [route-path]",
      "request": {
        "method": "POST",
        "header": [
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "body": {
          "mode": "raw",
          "raw": "{\n  \"key\": \"value\"\n}"
        },
        "url": {
          "raw": "{{baseUrl}}/api/[route-path]",
          "host": ["{{baseUrl}}"],
          "path": ["api", "[route-path]"]
        }
      }
    }
  ]
}
```

### Coleção Thunder Client
```json
{
  "client": "Thunder Client",
  "collectionName": "Testes de API Next.js",
  "dateExported": "2024-01-01",
  "version": "1.1",
  "folders": [],
  "requests": [
    {
      "name": "Testar Rota de API",
      "url": "localhost:3000/api/[route-path]",
      "method": "GET",
      "headers": [
        {
          "name": "Accept",
          "value": "application/json"
        }
      ]
    }
  ]
}
```

## Gerenciamento de Dados de Teste

### Fixtures de Teste
```typescript
// test/fixtures/apiTestData.ts
export const validUserData = {
  name: 'John Doe',
  email: 'john@example.com',
  age: 30,
  role: 'user'
};

export const invalidUserData = {
  name: '',
  email: 'invalid-email',
  age: -1,
  role: 'invalid-role'
};

export const testHeaders = {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'User-Agent': 'API-Test-Suite/1.0'
};
```

### Geração de Dados Mock
```typescript
// test/utils/mockData.ts
export function generateMockUser() {
  return {
    id: Math.random().toString(36).substr(2, 9),
    name: `User ${Math.floor(Math.random() * 1000)}`,
    email: `user${Date.now()}@example.com`,
    createdAt: new Date().toISOString()
  };
}

export function generateBulkTestData(count: number) {
  return Array.from({ length: count }, generateMockUser);
}
```

## Configuração do Ambiente de Teste

### Configuração Jest
```javascript
// jest.config.js para testes de API
module.exports = {
  testEnvironment: 'node',
  setupFilesAfterEnv: ['<rootDir>/test/setup.js'],
  testMatch: ['**/__tests__/**/*.test.js', '**/?(*.)+(spec|test).js'],
  collectCoverageFrom: [
    'pages/api/**/*.{js,ts}',
    'app/api/**/*.{js,ts}',
    '!**/*.d.ts',
  ],
  coverageThreshold: {
    global: {
      branches: 70,
      functions: 70,
      lines: 70,
      statements: 70
    }
  }
};
```

### Setup de Teste
```javascript
// test/setup.js
import { createMocks } from 'node-mocks-http';
import { testDb } from './testDatabase';

// Setup global de testes
beforeAll(async () => {
  // Configurar banco de dados de testes
  await testDb.connect();
});

afterAll(async () => {
  // Limpeza do banco de dados de testes
  await testDb.disconnect();
});

beforeEach(async () => {
  // Resetar estado do banco de dados
  await testDb.reset();
});

// Função auxiliar para testes de API
global.createAPITest = (handler) => {
  return (method, url, options = {}) => {
    const { req, res } = createMocks({
      method,
      url,
      ...options
    });
    return handler(req, res);
  };
};
```

## Integração de Testes Automatizados

### Workflow do GitHub Actions
```yaml
name: Testes de API
on: [push, pull_request]

jobs:
  test-api:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run test:api
      - name: Enviar cobertura
        uses: codecov/codecov-action@v3
```

### Testes Contínuos
```bash
# Modo watch para desenvolvimento
npm run test:api -- --watch

# Relatório de cobertura
npm run test:api -- --coverage

# Teste de rota específica
npm run test:api -- --testNamePattern="api/users"
```

## Análise de Resultados de Teste

Gere relatório abrangente de testes incluindo:
1. **Cobertura de Testes**: Percentuais de cobertura por linha, branch e função
2. **Métricas de Performance**: Tempos de resposta, throughput
3. **Análise de Segurança**: Autenticação, autorização, validação de entrada
4. **Tratamento de Erros**: Cenários de exceção e respostas de erro
5. **Compatibilidade**: Resultados de testes entre ambientes

Forneça recomendações acionáveis para melhorar a confiabilidade, performance e segurança da API.