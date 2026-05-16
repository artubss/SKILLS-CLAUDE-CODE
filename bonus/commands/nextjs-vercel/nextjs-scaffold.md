---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [project-name] [--typescript] [--tailwind] [--app-router]
description: Crie uma nova aplicação Next.js com boas práticas e configuração otimizada
---

## Scaffolding de Aplicação Next.js

**Nome do Projeto**: $ARGUMENTS

## Análise de Ambiente

- Diretório atual: !`pwd`
- Versão Node.js: !`node --version`
- Versão npm: !`npm --version`
- package.json existente: @package.json (se existir)

## Requisitos de Scaffolding

### 1. Inicialização do Projeto
Com base nos argumentos fornecidos, determine as opções de configuração:
- **TypeScript**: Verifique a flag `--typescript` ou detecte configuração TS existente
- **Tailwind CSS**: Verifique a flag `--tailwind` ou detecte configuração existente
- **App Router**: Verifique a flag `--app-router` (padrão para novos projetos)
- **ESLint/Prettier**: Sempre incluir para qualidade de código

### 2. Configuração Next.js
Crie `next.config.js` otimizado com:
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    optimizePackageImports: ['lucide-react', '@heroicons/react'],
  },
  images: {
    formats: ['image/webp', 'image/avif'],
    deviceSizes: [640, 750, 828, 1080, 1200, 1920, 2048, 3840],
  },
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          {
            key: 'X-Frame-Options',
            value: 'DENY',
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
        ],
      },
    ];
  },
};
```

### 3. Dependências Essenciais
Instale dependências principais:
- **Produção**: `next`, `react`, `react-dom`
- **Desenvolvimento**: `eslint`, `eslint-config-next`, `typescript` (se TS), `@types/*` (se TS)
- **Opcional**: `tailwindcss`, `prettier`, `husky`, `lint-staged`

### 4. Estrutura de Projeto
Crie estrutura de diretórios otimizada:
```
project-name/
├── app/                    # App Router (Next.js 13+)
│   ├── globals.css
│   ├── layout.tsx
│   ├── page.tsx
│   └── api/
├── components/             # Componentes reutilizáveis
│   └── ui/                # Primitivos de UI
├── lib/                   # Utilitários e configurações
├── public/                # Ativos estáticos
├── types/                 # Definições de tipos TypeScript
├── .env.local             # Variáveis de ambiente
├── .env.example           # Template de ambiente
├── .gitignore
├── next.config.js
├── package.json
├── README.md
└── tsconfig.json          # Se TypeScript
```

### 5. Arquivos de Configuração

#### Configuração ESLint
```json
{
  "extends": ["next/core-web-vitals"],
  "rules": {
    "@next/next/no-img-element": "error",
    "@next/next/no-html-link-for-pages": "error"
  }
}
```

#### Configuração TypeScript (se aplicável)
```json
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

### 6. Componentes Iniciais

#### Layout Raiz
```typescript
import type { Metadata } from 'next';
import { Inter } from 'next/font/google';
import './globals.css';

const inter = Inter({ subsets: ['latin'] });

export const metadata: Metadata = {
  title: 'Nome do Projeto',
  description: 'Gerado com scaffolding Next.js Claude Code',
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="pt-BR">
      <body className={inter.className}>{children}</body>
    </html>
  );
}
```

#### Página Inicial
```typescript
export default function Home() {
  return (
    <main className="flex min-h-screen flex-col items-center justify-between p-24">
      <div className="z-10 max-w-5xl w-full items-center justify-between font-mono text-sm">
        <h1 className="text-4xl font-bold">Bem-vindo ao seu App Next.js</h1>
        <p className="mt-4 text-lg">
          Construído com scaffolding Claude Code
        </p>
      </div>
    </main>
  );
}
```

### 7. Scripts de Desenvolvimento
Atualize package.json com scripts otimizados:
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "type-check": "tsc --noEmit",
    "format": "prettier --write .",
    "format:check": "prettier --check ."
  }
}
```

### 8. Documentação
Crie README.md abrangente com:
- Visão geral do projeto e funcionalidades
- Instruções de instalação e configuração
- Fluxo de desenvolvimento
- Diretrizes de deploy
- Diretrizes de contribuição

## Etapas de Implementação

1. **Inicializar Projeto**: Crie diretório do projeto e estrutura básica
2. **Instalar Dependências**: Configure Next.js com opções escolhidas
3. **Configurar TypeScript**: Configure TypeScript se solicitado
4. **Configurar Tailwind**: Configure Tailwind CSS se solicitado
5. **Criar Componentes**: Gere componentes e layouts iniciais
6. **Configurar Ferramentas de Desenvolvimento**: Configure ESLint, Prettier e scripts
7. **Configuração de Ambiente**: Crie arquivos .env e exemplos
8. **Gerar Documentação**: Crie README e guias de configuração

## Checklist de Qualidade

- [ ] Next.js configurado com App Router
- [ ] Configuração TypeScript (se solicitado)
- [ ] Tailwind CSS configurado (se solicitado)
- [ ] ESLint e Prettier configurados
- [ ] Headers de segurança configurados
- [ ] Otimização de imagens habilitada
- [ ] Scripts de desenvolvimento funcionando
- [ ] Template de variáveis de ambiente criado
- [ ] Documentação README completa
- [ ] Projeto construindo com sucesso

## Tarefas Pós-Scaffolding

Após o scaffolding, execute estes comandos para verificar a configuração:
```bash
cd [project-name]
npm install
npm run build
npm run lint
npm run type-check  # Se TypeScript
```

Forneça próximos passos específicos com base nos requisitos do projeto e funcionalidades adicionais necessárias.