---
allowed-tools: Read, Write, Edit, Bash, Grep, Glob
argument-hint: [--pages-to-app] [--js-to-ts] [--class-to-hooks] [--analyze]
description: Assistente abrangente de migração Next.js para Pages Router para App Router, JavaScript para TypeScript e padrões modernos
---

## Assistente de Migração Next.js

**Tipo de Migração**: $ARGUMENTS

## Análise do Projeto Atual

### Análise da Estrutura do Projeto
- Versão Next.js: !`grep '"next"' package.json | head -1`
- Router atual: !`ls -la pages/ 2>/dev/null && echo "Pages Router detectado" || echo "Diretório pages/ não encontrado"`
- App router: !`ls -la app/ 2>/dev/null && echo "App Router detectado" || echo "Diretório app/ não encontrado"`
- TypeScript: @tsconfig.json (se existir)

### Visão Geral da Estrutura de Arquivos
- Diretório Pages: @pages/ (se existir)
- Diretório App: @app/ (se existir)  
- Componentes: @components/ (se existir)
- Rotas API: @pages/api/ ou @app/api/
- Estilos: @styles/ (se existir)

## Estratégias de Migração

### 1. Migração de Pages Router para App Router

#### Análise Pré-Migração
```typescript
// Ferramenta de análise de migração
interface MigrationAnalysis {
  currentStructure: 'pages' | 'app' | 'hybrid';
  pagesCount: number;
  apiRoutesCount: number;
  customApp: boolean;
  customDocument: boolean;
  customError: boolean;
  middlewareExists: boolean;
  complexityScore: number;
}

const analyzeMigrationComplexity = (): MigrationAnalysis => {
  return {
    currentStructure: 'pages', // Detectado na estrutura de arquivos
    pagesCount: 0, // Contagem de arquivos .js/.tsx em pages/
    apiRoutesCount: 0, // Contagem de arquivos em pages/api/
    customApp: false, // Verifica pages/_app
    customDocument: false, // Verifica pages/_document
    customError: false, // Verifica pages/_error ou 404
    middlewareExists: false, // Verifica middleware.ts
    complexityScore: 0, // Escala de 1-10
  };
};
```

#### Etapas de Migração

##### Etapa 1: Criar Estrutura de Diretórios do App Router
```bash
#!/bin/bash
# Criar estrutura de diretórios do App Router

echo "🚀 Criando estrutura de diretórios do App Router..."

# Criar diretório base do app
mkdir -p app
mkdir -p app/globals
mkdir -p app/api

# Criar arquivos de layout
echo "📁 Criando estrutura de layout..."

# Layout raiz
cat > app/layout.tsx << 'EOF'
import type { Metadata } from 'next'
import { Inter } from 'next/font/google'
import './globals.css'

const inter = Inter({ subsets: ['latin'] })

export const metadata: Metadata = {
  title: 'Seu App',
  description: 'Migrado para App Router',
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="pt-BR">
      <body className={inter.className}>{children}</body>
    </html>
  )
}
EOF

# CSS global
cat > app/globals.css << 'EOF'
/* Estilos globais para App Router */
:root {
  --max-width: 1100px;
  --border-radius: 12px;
  --font-mono: ui-monospace, Menlo, Monaco, 'Cascadia Code', 'Segoe UI Mono',
    'Roboto Mono', 'Oxygen Mono', 'Ubuntu Monospace', 'Source Code Pro',
    'Fira Code', 'Droid Sans Mono', 'Courier New', monospace;
}

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

html,
body {
  max-width: 100vw;
  overflow-x: hidden;
}

body {
  color: rgb(var(--foreground-rgb));
  background: linear-gradient(
      to bottom,
      transparent,
      rgb(var(--background-end-rgb))
    )
    rgb(var(--background-start-rgb));
}

a {
  color: inherit;
  text-decoration: none;
}

@media (prefers-color-scheme: dark) {
  html {
    color-scheme: dark;
  }
}
EOF

echo "✅ Estrutura do App Router criada"
```

##### Etapa 2: Migrar Páginas para App Router
```typescript
// Utilitário de migração de páginas
interface PageMigration {
  source: string;
  destination: string;
  type: 'page' | 'api' | 'dynamic' | 'nested';
  hasGetServerSideProps: boolean;
  hasGetStaticProps: boolean;
  hasGetStaticPaths: boolean;
}

const migratePage = async (pagePath: string): Promise<string> => {
  const pageContent = readFileSync(pagePath, 'utf-8');
  
  // Extrair componente de página
  const componentMatch = pageContent.match(/export default function (\w+)/);
  const componentName = componentMatch?.[1] || 'Page';
  
  // Verificar métodos de busca de dados
  const hasGetServerSideProps = pageContent.includes('getServerSideProps');
  const hasGetStaticProps = pageContent.includes('getStaticProps');
  const hasGetStaticPaths = pageContent.includes('getStaticPaths');
  
  // Converter para formato App Router
  let appRouterCode = '';
  
  // Adicionar metadata se página tem componente Head
  if (pageContent.includes('from \'next/head\'')) {
    appRouterCode += `import type { Metadata } from 'next'\n\n`;
    appRouterCode += generateMetadata(pageContent);
  }
  
  // Converter busca de dados
  if (hasGetServerSideProps) {
    appRouterCode += convertGetServerSideProps(pageContent);
  } else if (hasGetStaticProps) {
    appRouterCode += convertGetStaticProps(pageContent);
  }
  
  // Converter componente
  appRouterCode += convertPageComponent(pageContent);
  
  return appRouterCode;
};

const convertGetServerSideProps = (content: string): string => {
  // Extrair lógica de getServerSideProps e converter para Server Component
  const gsspMatch = content.match(/export async function getServerSideProps[\s\S]*?(?=export|$)/);
  
  if (!gsspMatch) return '';
  
  return `
// Server Component com busca de dados direta
async function fetchData(context: any) {
  // Convertido de getServerSideProps
  // Adicione sua lógica de busca de dados aqui
  return { data: null };
}
`;
};

const generateMetadata = (content: string): string => {
  // Extrair conteúdo do componente Head e converter para metadata
  return `
export const metadata: Metadata = {
  title: 'Título da Página',
  description: 'Descrição da página',
}

`;
};

const convertPageComponent = (content: string): string => {
  // Converter componente de página para formato App Router
  return content
    .replace(/import Head from \'next\/head\'/g, '')
    .replace(/<Head>[\s\S]*?<\/Head>/g, '')
    .replace(/export async function getServerSideProps[\s\S]*?(?=export)/g, '')
    .replace(/export async function getStaticProps[\s\S]*?(?=export)/g, '')
    .replace(/export async function getStaticPaths[\s\S]*?(?=export)/g, '');
};
```

##### Etapa 3: Migrar Rotas API
```typescript
// Migração de rotas API
const migrateApiRoute = (apiPath: string): string => {
  const apiContent = readFileSync(apiPath, 'utf-8');
  
  // Converter para formato App Router API
  let newApiContent = `import { NextRequest, NextResponse } from 'next/server'\n\n`;
  
  // Extrair funções de handler
  const methods = ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'];
  
  methods.forEach(method => {
    const handlerRegex = new RegExp(`if.*req\\.method.*===.*['"]${method}['"]`, 'i');
    
    if (apiContent.match(handlerRegex)) {
      newApiContent += `
export async function ${method}(
  request: NextRequest,
  { params }: { params: { [key: string]: string } }
) {
  try {
    // Handler ${method} migrado
    // Adicione sua lógica aqui
    
    return NextResponse.json({ message: '${method} sucesso' })
  } catch (error) {
    console.error('${method} erro:', error)
    return NextResponse.json(
      { error: 'Erro interno do servidor' },
      { status: 500 }
    )
  }
}
`;
    }
  });
  
  return newApiContent;
};
```

### 2. Migração de JavaScript para TypeScript

#### Configuração de TypeScript
```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "es6"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

#### Processo de Conversão de Arquivos
```bash
#!/bin/bash
# Converter arquivos JavaScript para TypeScript

echo "🔄 Convertendo arquivos JavaScript para TypeScript..."

# Encontrar todos os arquivos .js e .jsx
find . -name "*.js" -o -name "*.jsx" | grep -v node_modules | grep -v .next | while read file; do
  # Pular se versão TypeScript já existe
  ts_file="${file%.*}.ts"
  tsx_file="${file%.*}.tsx"
  
  if [[ -f "$ts_file" ]] || [[ -f "$tsx_file" ]]; then
    echo "⏭️  Pulando $file (versão TypeScript já existe)"
    continue
  fi
  
  # Determinar se arquivo contém JSX
  if grep -q "jsx\|<.*>" "$file"; then
    new_file="${file%.*}.tsx"
  else
    new_file="${file%.*}.ts"
  fi
  
  echo "📝 Convertendo $file -> $new_file"
  
  # Copiar arquivo com nova extensão
  cp "$file" "$new_file"
  
  # Adicionar anotações de tipo básicas
  sed -i.bak '
    # Adicionar importação React para arquivos TSX
    /^import.*React/!{
      /\.tsx$/s/^/import React from '\''react'\''\n/
    }
    
    # Adicionar tipos básicos de props
    s/function \([A-Z][a-zA-Z]*\)(\([^)]*\))/function \1(\2: any)/g
    
    # Adicionar anotações de tipo de retorno para funções simples
    s/const \([a-zA-Z][a-zA-Z0-9]*\) = (/const \1 = (/g
  ' "$new_file"
  
  # Remover arquivo de backup
  rm "${new_file}.bak" 2>/dev/null || true
  
  echo "✅ $file convertido"
done

echo "🎉 Conversão de JavaScript para TypeScript concluída"
echo "⚠️  Revise e adicione anotações de tipo apropriadas"
```

### 3. Migração de Class Components para Function Components

#### Análise e Conversão de Componentes
```typescript
// Conversor de class para function components
const convertClassComponent = (componentCode: string): string => {
  // Extrair partes do class component
  const classMatch = componentCode.match(/class (\w+) extends (?:React\.)?Component/);
  const componentName = classMatch?.[1] || 'Component';
  
  // Extrair state
  const stateMatch = componentCode.match(/state\s*=\s*{([^}]+)}/);
  const initialState = stateMatch?.[1] || '';
  
  // Extrair métodos de ciclo de vida
  const lifecycleMethods = extractLifecycleMethods(componentCode);
  
  // Extrair método render
  const renderMatch = componentCode.match(/render\(\)\s*{([\s\S]*?)(?=^\s*})/m);
  const renderContent = renderMatch?.[1] || '';
  
  // Gerar function component
  let functionComponent = `import React, { useState, useEffect } from 'react';\n\n`;
  
  // Adicionar tipos de props se existirem
  const propsMatch = componentCode.match(/(\w+)Props/);
  if (propsMatch) {
    functionComponent += `interface ${propsMatch[1]}Props {\n  // Adicione definições de props aqui\n}\n\n`;
  }
  
  functionComponent += `const ${componentName}: React.FC<${componentName}Props> = (props) => {\n`;
  
  // Converter state
  if (initialState) {
    const stateVars = parseState(initialState);
    stateVars.forEach(({ name, value }) => {
      functionComponent += `  const [${name}, set${capitalize(name)}] = useState(${value});\n`;
    });
  }
  
  // Converter métodos de ciclo de vida para hooks
  if (lifecycleMethods.componentDidMount) {
    functionComponent += `\n  useEffect(() => {\n`;
    functionComponent += `    ${lifecycleMethods.componentDidMount}\n`;
    functionComponent += `  }, []);\n`;
  }
  
  if (lifecycleMethods.componentDidUpdate) {
    functionComponent += `\n  useEffect(() => {\n`;
    functionComponent += `    ${lifecycleMethods.componentDidUpdate}\n`;
    functionComponent += `  });\n`;
  }
  
  if (lifecycleMethods.componentWillUnmount) {
    functionComponent += `\n  useEffect(() => {\n`;
    functionComponent += `    return () => {\n`;
    functionComponent += `      ${lifecycleMethods.componentWillUnmount}\n`;
    functionComponent += `    };\n`;
    functionComponent += `  }, []);\n`;
  }
  
  // Adicionar retorno do render
  functionComponent += `\n  return (\n`;
  functionComponent += renderContent.replace(/this\.state\./g, '').replace(/this\.props\./g, 'props.');
  functionComponent += `  );\n`;
  functionComponent += `};\n\n`;
  functionComponent += `export default ${componentName};`;
  
  return functionComponent;
};

const extractLifecycleMethods = (code: string) => {
  return {
    componentDidMount: extractMethod(code, 'componentDidMount'),
    componentDidUpdate: extractMethod(code, 'componentDidUpdate'),
    componentWillUnmount: extractMethod(code, 'componentWillUnmount'),
  };
};

const extractMethod = (code: string, methodName: string): string | null => {
  const regex = new RegExp(`${methodName}\\(\\)\\s*{([\\s\\S]*?)(?=^\\s*})`);
  const match = code.match(regex);
  return match?.[1] || null;
};

const parseState = (stateString: string) => {
  // Parser de state simples - seria necessária implementação mais robusta
  return [
    { name: 'example', value: 'null' }
  ];
};

const capitalize = (str: string) => str.charAt(0).toUpperCase() + str.slice(1);
```

### 4. Migração para Padrões React Modernos

#### Padrões de Conversão de Hooks
```typescript
// Converter padrões comuns para hooks modernos

// Gerenciamento de state
const convertStateManagement = `
// ❌ State antigo em class component
class MyComponent extends Component {
  state = { count: 0, name: '' };
  
  updateCount = () => {
    this.setState({ count: this.state.count + 1 });
  };
}

// ✅ State moderno com hooks
const MyComponent = () => {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');
  
  const updateCount = () => {
    setCount(prev => prev + 1);
  };
};
`;

// Gerenciamento de efeitos
const convertEffects = `
// ❌ Métodos de ciclo de vida antigos
componentDidMount() {
  this.fetchData();
}

componentDidUpdate(prevProps) {
  if (prevProps.id !== this.props.id) {
    this.fetchData();
  }
}

componentWillUnmount() {
  clearInterval(this.timer);
}

// ✅ useEffect moderno
useEffect(() => {
  fetchData();
}, []); // componentDidMount

useEffect(() => {
  fetchData();
}, [id]); // componentDidUpdate com dependência

useEffect(() => {
  return () => {
    clearInterval(timer);
  };
}, []); // componentWillUnmount
`;

// Uso de Context
const convertContext = `
// ❌ Context antigo
import { ThemeContext } from './context';

class MyComponent extends Component {
  static contextType = ThemeContext;
  
  render() {
    const theme = this.context;
    return <div style={{ color: theme.color }}>Conteúdo</div>;
  }
}

// ✅ Context moderno com hooks
import { useContext } from 'react';
import { ThemeContext } from './context';

const MyComponent = () => {
  const theme = useContext(ThemeContext);
  
  return <div style={{ color: theme.color }}>Conteúdo</div>;
};
`;
```

## Processo Abrangente de Migração

### 1. Checklist Pré-Migração
```bash
#!/bin/bash
# Validação pré-migração

echo "🔍 Executando verificações pré-migração..."

# Verificar versão do Next.js
NEXT_VERSION=$(grep '"next"' package.json | grep -o '[0-9.]*')
echo "📦 Versão Next.js: $NEXT_VERSION"

# Verificar possíveis bloqueadores
BLOCKERS=0

# Verificar servidor customizado
if [ -f "server.js" ] || [ -f "server.ts" ]; then
  echo "⚠️  Servidor customizado detectado - pode necessitar tratamento especial"
  ((BLOCKERS++))
fi

# Verificar pages/_document com lógica customizada
if [ -f "pages/_document.js" ] || [ -f "pages/_document.tsx" ]; then
  if grep -q "getInitialProps" pages/_document.*; then
    echo "⚠️  _document customizado com getInitialProps - necessita migração manual"
    ((BLOCKERS++))
  fi
fi

# Verificar pages/_error
if [ -f "pages/_error.js" ] || [ -f "pages/_error.tsx" ]; then
  echo "ℹ️  Página de erro customizada encontrada - será necessário migrar para error.tsx"
fi

# Verificar middleware
if [ -f "middleware.ts" ] || [ -f "middleware.js" ]; then
  echo "✅ Middleware já existe"
else
  echo "ℹ️  Nenhum middleware encontrado"
fi

echo ""
if [ $BLOCKERS -eq 0 ]; then
  echo "✅ Pronto para migração!"
else
  echo "⚠️  Encontrados $BLOCKERS possíveis bloqueadores - revise antes de prosseguir"
fi
```

### 2. Execução da Migração
```bash
#!/bin/bash
# Executar migração

echo "🚀 Iniciando processo de migração Next.js..."

# Etapa 1: Backup do projeto atual
echo "📦 Criando backup..."
tar -czf "project-backup-$(date +%Y%m%d_%H%M%S).tar.gz" \
  --exclude=node_modules \
  --exclude=.next \
  --exclude=.git \
  .

# Etapa 2: Instalar dependências
echo "📥 Instalando dependências necessárias..."
npm install --save-dev @types/react @types/react-dom @types/node
npm install --save-dev typescript

# Etapa 3: Criar config TypeScript
if [ ! -f "tsconfig.json" ]; then
  echo "⚙️  Criando configuração TypeScript..."
  npx tsc --init --jsx preserve --esModuleInterop --allowJs --strict
fi

# Etapa 4: Criar estrutura App Router
echo "🏗️  Criando estrutura App Router..."
mkdir -p app
# ... (lógica de criação das etapas anteriores)

# Etapa 5: Migrar páginas
echo "📄 Migrando páginas..."
# ... (lógica de migração)

# Etapa 6: Migrar rotas API
echo "🔌 Migrando rotas API..."
# ... (lógica de migração de API)

# Etapa 7: Atualizar configurações
echo "⚙️  Atualizando configurações..."
# Atualizar next.config.js, scripts package.json, etc.

echo "✅ Migração concluída!"
echo "⚠️  Revise o código migrado e teste completamente"
```

### 3. Validação Pós-Migração
```bash
#!/bin/bash
# Validação pós-migração

echo "🔍 Executando validação pós-migração..."

# Verificar se o projeto compila
echo "🏗️  Testando build..."
npm run build

if [ $? -eq 0 ]; then
  echo "✅ Build bem-sucedido"
else
  echo "❌ Build falhou - verifique erros acima"
  exit 1
fi

# Verificar compilação TypeScript
echo "🔍 Verificando TypeScript..."
npx tsc --noEmit

if [ $? -eq 0 ]; then
  echo "✅ Validação TypeScript passou"
else
  echo "⚠️  Erros TypeScript encontrados - revise e corrija"
fi

# Executar testes se existirem
if [ -f "package.json" ] && grep -q '"test"' package.json; then
  echo "🧪 Executando testes..."
  npm test
fi

# Verificar arquivos não utilizados
echo "🧹 Verificando arquivos não utilizados..."
if [ -d "pages" ]; then
  echo "ℹ️  Diretório original pages/ ainda existe"
  echo "💡 Revise e remova após confirmar que a migração foi bem-sucedida"
fi

echo "✅ Validação pós-migração concluída"
```

## Documentação e Guias de Migração

### 1. Geração de Relatório de Migração
```typescript
// Gerar relatório abrangente de migração
interface MigrationReport {
  summary: {
    totalFiles: number;
    migratedFiles: number;
    skippedFiles: number;
    errorFiles: number;
  };
  details: {
    pages: MigratedFile[];
    components: MigratedFile[];
    apiRoutes: MigratedFile[];
  };
  issues: Issue[];
  recommendations: string[];
}

interface MigratedFile {
  original: string;
  migrated: string;
  status: 'success' | 'warning' | 'error';
  notes: string[];
}

interface Issue {
  file: string;
  type: 'error' | 'warning';
  message: string;
  solution?: string;
}

const generateMigrationReport = (): MigrationReport => {
  // Implementação para gerar relatório abrangente de migração
  return {
    summary: {
      totalFiles: 0,
      migratedFiles: 0,
      skippedFiles: 0,
      errorFiles: 0,
    },
    details: {
      pages: [],
      components: [],
      apiRoutes: [],
    },
    issues: [],
    recommendations: [
      'Teste toda funcionalidade completamente',
      'Atualize qualquer importação codificada',
      'Revise e otimize divisão de bundle',
      'Atualize documentação e README',
    ],
  };
};
```

### 2. Guia de Melhores Práticas
```markdown
# Melhores Práticas de Migração

## Antes da Migração
- [ ] Atualize para a versão mais recente do Next.js
- [ ] Execute suite completa de testes
- [ ] Crie backup abrangente
- [ ] Revise configurações customizadas

## Durante a Migração
- [ ] Migre incrementalmente (páginas primeiro, depois componentes)
- [ ] Teste cada etapa de migração
- [ ] Mantenha notas detalhadas de alterações
- [ ] Resolva erros TypeScript imediatamente

## Após Migração
- [ ] Atualize todas as importações e referências
- [ ] Teste toda funcionalidade
- [ ] Atualize documentação
- [ ] Monitore métricas de performance
- [ ] Limpe arquivos antigos após validação

## Armadilhas Comuns
- Mudanças na sintaxe de importações dinâmicas
- Atualizações na configuração de middleware
- Manipulação de variáveis de ambiente
- Ajustes de CSS e estilos
```

Forneça assistência abrangente de migração com ferramentas automatizadas, etapas de validação e documentação detalhada para modernização bem-sucedida do Next.js.