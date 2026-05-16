---
allowed-tools: Read, Write, Edit
argument-hint: [component-name] [--client] [--server] [--page] [--layout]
description: Gera componentes React otimizados para Next.js com TypeScript e melhores práticas
---

## Gerador de Componentes Next.js

**Nome do Componente**: $ARGUMENTS

## Análise de Contexto do Projeto

### Detecção de Framework
- Config Next.js: @next.config.js
- Config TypeScript: @tsconfig.json (se existir)
- Config Tailwind: @tailwind.config.js (se existir)
- Package.json: @package.json

### Padrões de Componentes Existentes
- Diretório de componentes: @components/
- Diretório app: @app/ (se App Router)
- Diretório pages: @pages/ (se Pages Router)
- Diretório de estilos: @styles/

## Requisitos de Geração de Componentes

### 1. Detecção de Tipo de Componente
Com base nos argumentos e contexto, determine o tipo de componente:
- **Client Component**: UI interativa com estado/eventos (`--client` ou padrão para componentes interativos)
- **Server Component**: Renderização estática, busca de dados (`--server` ou padrão para Next.js 13+)
- **Page Component**: Componente de nível de rota (`--page`)
- **Layout Component**: Wrapper de layout compartilhado (`--layout`)

### 2. Criação de Estrutura de Arquivos
Gera estrutura abrangente de componentes:
```
components/[ComponentName]/
├── index.ts                    # Barrel export
├── [ComponentName].tsx         # Componente principal
├── [ComponentName].module.css  # Estilos do componente
├── [ComponentName].test.tsx    # Testes unitários
├── [ComponentName].stories.tsx # Estória Storybook (se detectado)
└── types.ts                   # Tipos TypeScript
```

### 3. Templates de Componentes

#### Template Server Component
```typescript
import { FC } from 'react';
import styles from './ComponentName.module.css';

interface ComponentNameProps {
  /**
   * Descrição do componente
   */
  children?: React.ReactNode;
  /**
   * Classes CSS adicionais
   */
  className?: string;
}

/**
 * ComponentName - Server Component
 * 
 * @description Descrição breve do propósito do componente
 * @example
 * <ComponentName>Conteúdo</ComponentName>
 */
export const ComponentName: FC<ComponentNameProps> = ({
  children,
  className = '',
  ...props
}) => {
  return (
    <div className={`${styles.container} ${className}`} {...props}>
      {children}
    </div>
  );
};

export default ComponentName;
```

#### Template Client Component
```typescript
'use client';

import { FC, useState, useEffect } from 'react';
import styles from './ComponentName.module.css';

interface ComponentNameProps {
  /**
   * Descrição do componente
   */
  children?: React.ReactNode;
  /**
   * Handler do evento de clique
   */
  onClick?: () => void;
  /**
   * Classes CSS adicionais
   */
  className?: string;
}

/**
 * ComponentName - Client Component
 * 
 * @description Componente interativo com funcionalidade no lado do cliente
 * @example
 * <ComponentName onClick={() => console.log('clicado')}>
 *   Conteúdo
 * </ComponentName>
 */
export const ComponentName: FC<ComponentNameProps> = ({
  children,
  onClick,
  className = '',
  ...props
}) => {
  const [isActive, setIsActive] = useState(false);

  const handleClick = () => {
    setIsActive(!isActive);
    onClick?.();
  };

  return (
    <button
      className={`${styles.button} ${isActive ? styles.active : ''} ${className}`}
      onClick={handleClick}
      {...props}
    >
      {children}
    </button>
  );
};

export default ComponentName;
```

#### Template Page Component
```typescript
import { Metadata } from 'next';
import ComponentName from '@/components/ComponentName';

export const metadata: Metadata = {
  title: 'Título da Página',
  description: 'Descrição da página',
};

interface PageProps {
  params: { id: string };
  searchParams: { [key: string]: string | string[] | undefined };
}

export default function Page({ params, searchParams }: PageProps) {
  return (
    <main>
      <h1>Título da Página</h1>
      <ComponentName />
    </main>
  );
}
```

#### Template Layout Component
```typescript
import { FC } from 'react';
import styles from './Layout.module.css';

interface LayoutProps {
  children: React.ReactNode;
  /**
   * Título da página
   */
  title?: string;
}

/**
 * Layout - Componente de layout compartilhado
 * 
 * @description Fornece estrutura de layout consistente em todas as páginas
 */
export const Layout: FC<LayoutProps> = ({
  children,
  title,
}) => {
  return (
    <div className={styles.layout}>
      <header className={styles.header}>
        {title && <h1 className={styles.title}>{title}</h1>}
      </header>
      
      <main className={styles.main}>
        {children}
      </main>
      
      <footer className={styles.footer}>
        <p>&copy; 2024 Seu App</p>
      </footer>
    </div>
  );
};

export default Layout;
```

### 4. Templates de CSS Module

#### Estilos de Componente Básico
```css
/* ComponentName.module.css */
.container {
  display: flex;
  flex-direction: column;
  padding: 1rem;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
  background-color: #ffffff;
}

.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  border: 1px solid transparent;
  background-color: #3b82f6;
  color: white;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s;
}

.button:hover {
  background-color: #2563eb;
}

.button:focus {
  outline: 2px solid #3b82f6;
  outline-offset: 2px;
}

.button.active {
  background-color: #1d4ed8;
}

/* Design responsivo */
@media (max-width: 768px) {
  .container {
    padding: 0.75rem;
  }
  
  .button {
    padding: 0.75rem 1rem;
  }
}
```

#### Estilos de Layout
```css
/* Layout.module.css */
.layout {
  min-height: 100vh;
  display: grid;
  grid-template-rows: auto 1fr auto;
}

.header {
  padding: 1rem 2rem;
  background-color: #f8fafc;
  border-bottom: 1px solid #e2e8f0;
}

.title {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 600;
  color: #1e293b;
}

.main {
  padding: 2rem;
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
}

.footer {
  padding: 1rem 2rem;
  background-color: #f1f5f9;
  border-top: 1px solid #e2e8f0;
  text-align: center;
  color: #64748b;
}
```

### 5. Tipos TypeScript
```typescript
// types.ts
export interface BaseComponentProps {
  children?: React.ReactNode;
  className?: string;
  'data-testid'?: string;
}

export interface ButtonProps extends BaseComponentProps {
  variant?: 'primary' | 'secondary' | 'outline';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  onClick?: () => void;
}

export interface LayoutProps extends BaseComponentProps {
  title?: string;
  sidebar?: React.ReactNode;
  breadcrumbs?: BreadcrumbItem[];
}

export interface BreadcrumbItem {
  label: string;
  href?: string;
  current?: boolean;
}
```

### 6. Testes Unitários
```typescript
// ComponentName.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import ComponentName from './ComponentName';

describe('ComponentName', () => {
  it('renderiza children corretamente', () => {
    render(<ComponentName>Conteúdo de Teste</ComponentName>);
    expect(screen.getByText('Conteúdo de Teste')).toBeInTheDocument();
  });

  it('aplica className customizado', () => {
    render(<ComponentName className="custom-class">Teste</ComponentName>);
    const element = screen.getByText('Teste');
    expect(element).toHaveClass('custom-class');
  });

  it('manipula eventos de clique', () => {
    const handleClick = jest.fn();
    render(<ComponentName onClick={handleClick}>Clique em mim</ComponentName>);
    
    const button = screen.getByText('Clique em mim');
    fireEvent.click(button);
    
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('alterna estado ativo ao clicar', () => {
    render(<ComponentName>Alternar</ComponentName>);
    const button = screen.getByText('Alternar');
    
    expect(button).not.toHaveClass('active');
    
    fireEvent.click(button);
    expect(button).toHaveClass('active');
    
    fireEvent.click(button);
    expect(button).not.toHaveClass('active');
  });

  it('é acessível', () => {
    render(<ComponentName>Botão Acessível</ComponentName>);
    const button = screen.getByRole('button');
    
    expect(button).toBeInTheDocument();
    expect(button).toHaveAccessibleName('Botão Acessível');
  });
});
```

### 7. Estórias Storybook (se detectado)
```typescript
// ComponentName.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
import ComponentName from './ComponentName';

const meta: Meta<typeof ComponentName> = {
  title: 'Components/ComponentName',
  component: ComponentName,
  parameters: {
    layout: 'centered',
    docs: {
      description: {
        component: 'Um componente reutilizável construído para aplicações Next.js.',
      },
    },
  },
  tags: ['autodocs'],
  argTypes: {
    onClick: { action: 'clicked' },
    className: { control: 'text' },
  },
};

export default meta;
type Story = StoryObj<typeof meta>;

export const Default: Story = {
  args: {
    children: 'Componente Padrão',
  },
};

export const WithCustomClass: Story = {
  args: {
    children: 'Estilo Customizado',
    className: 'custom-style',
  },
};

export const Interactive: Story = {
  args: {
    children: 'Clique em mim',
    onClick: () => alert('Componente clicado!'),
  },
};
```

### 8. Barrel Export
```typescript
// index.ts
export { default } from './ComponentName';
export type { ComponentNameProps } from './ComponentName';
```

## Otimizações Específicas do Framework

### Integração Tailwind CSS (se detectado)
Substitua CSS modules por classes Tailwind:
```typescript
export const ComponentName: FC<ComponentNameProps> = ({
  children,
  className = '',
}) => {
  return (
    <div className={`flex flex-col p-4 rounded-lg border border-slate-200 bg-white ${className}`}>
      {children}
    </div>
  );
};
```

### Otimizações Next.js App Router
- **Server Components**: Padrão para componentes não interativos
- **Client Components**: Diretiva 'use client' explícita
- **Metadata**: Inclua metadata para componentes de página
- **Estados de Carregamento**: Implemente loading.tsx para componentes assíncronos

### Recursos de Acessibilidade
- **ARIA Labels**: Labeling apropriado para leitores de tela
- **Navegação por Teclado**: Ordem de tab e atalhos de teclado
- **Gerenciamento de Foco**: Indicadores de foco visíveis
- **HTML Semântico**: Elementos semânticos apropriados

## Processo de Geração de Componentes

1. **Análise**: Analise a estrutura e padrões do projeto existente
2. **Seleção de Template**: Escolha o template apropriado com base no tipo de componente
3. **Customização**: Adapte o template às convenções do projeto
4. **Criação de Arquivos**: Gere todos os arquivos de componentes
5. **Integração**: Atualize arquivos de índice e exports
6. **Validação**: Verifique se o componente compila e os testes passam

## Checklist de Qualidade

- [ ] Componente segue convenções de nomenclatura do projeto
- [ ] Tipos TypeScript são apropriadamente definidos
- [ ] CSS segue padrões estabelecidos (modules ou Tailwind)
- [ ] Testes unitários cobrem funcionalidade-chave
- [ ] Componente é acessível (ARIA, navegação por teclado)
- [ ] Documentação inclui exemplos de uso
- [ ] Estória Storybook criada (se Storybook detectado)
- [ ] Componente compila sem erros
- [ ] Testes passam com sucesso

Forneça a implementação completa do componente com todos os arquivos e recursos especificados.