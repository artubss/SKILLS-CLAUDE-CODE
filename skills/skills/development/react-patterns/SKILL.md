---
name: react-patterns
description: Padrões e princípios modernos de React. Hooks, composição, performance, boas práticas com TypeScript.
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Padrões React

> Princípios para construir aplicações React prontas para produção.

---

## 1. Princípios de Design de Componentes

### Tipos de Componentes

| Tipo | Uso | Estado |
|------|-----|-------|
| **Server** | Busca de dados, estático | Nenhum |
| **Client** | Interatividade | useState, effects |
| **Presentational** | Exibição de UI | Apenas props |
| **Container** | Lógica/estado | Estado complexo |

### Regras de Design

- Uma responsabilidade por componente
- Props para baixo, eventos para cima
- Composição sobre herança
- Prefira componentes pequenos e focados

---

## 2. Padrões de Hooks

### Quando Extrair Hooks

| Padrão | Extrair Quando |
|--------|----------------|
| **useLocalStorage** | Mesma lógica de armazenamento necessária |
| **useDebounce** | Múltiplos valores com debounce |
| **useFetch** | Padrões de fetch repetidos |
| **useForm** | Estado de formulário complexo |

### Regras de Hooks

- Hooks apenas no nível superior
- Mesma ordem a cada renderização
- Custom hooks começam com "use"
- Limpe efeitos ao desmontar

---

## 3. Seleção de Gerenciamento de Estado

| Complexidade | Solução |
|--------------|---------|
| Simples | useState, useReducer |
| Compartilhado localmente | Context |
| Estado do servidor | React Query, SWR |
| Global complexo | Zustand, Redux Toolkit |

### Posicionamento de Estado

| Escopo | Onde |
|--------|------|
| Componente único | useState |
| Pai-filho | Eleve o estado |
| Subárvore | Context |
| Em toda a app | Global store |

---

## 4. Padrões React 19

### Novos Hooks

| Hook | Propósito |
|------|-----------|
| **useActionState** | Estado de submissão de formulário |
| **useOptimistic** | Atualizações otimistas de UI |
| **use** | Leia recursos na renderização |

### Benefícios do Compilador

- Memoização automática
- Menos useMemo/useCallback manual
- Foque em componentes puros

---

## 5. Padrões de Composição

### Componentes Compostos

- Pai fornece context
- Filhos consomem context
- Composição flexível baseada em slots
- Exemplo: Tabs, Accordion, Dropdown

### Render Props vs Hooks

| Caso de Uso | Prefira |
|-------------|---------|
| Lógica reutilizável | Custom hook |
| Flexibilidade de renderização | Render props |
| Transversal | Higher-order component |

---

## 6. Princípios de Performance

### Quando Otimizar

| Sinal | Ação |
|-------|------|
| Renderizações lentas | Perfil primeiro |
| Listas grandes | Virtualize |
| Cálculo caro | useMemo |
| Callbacks estáveis | useCallback |

### Ordem de Otimização

1. Verifique se realmente é lento
2. Perfil com DevTools
3. Identifique o gargalo
4. Aplique correção direcionada

---

## 7. Tratamento de Erros

### Uso de Error Boundary

| Escopo | Posicionamento |
|--------|-----------------|
| Em toda a app | Nível raiz |
| Funcionalidade | Nível de rota/feature |
| Componente | Ao redor do componente arriscado |

### Recuperação de Erro

- Mostre UI alternativa
- Registre o erro
- Ofereça opção de nova tentativa
- Preserve dados do usuário

---

## 8. Padrões TypeScript

### Tipagem de Props

| Padrão | Uso |
|--------|-----|
| Interface | Props de componente |
| Type | Unions, complexo |
| Generic | Componentes reutilizáveis |

### Tipos Comuns

| Necessidade | Tipo |
|------------|------|
| Children | ReactNode |
| Event handler | MouseEventHandler |
| Ref | RefObject<Element> |

---

## 9. Princípios de Testes

| Nível | Foco |
|-------|------|
| Unitário | Funções puras, hooks |
| Integração | Comportamento do componente |
| E2E | Fluxos do usuário |

### Prioridades de Teste

- Comportamento visível ao usuário
- Casos extremos
- Estados de erro
- Acessibilidade

---

## 10. Anti-padrões

| ❌ Não faça | ✅ Faça |
|-------------|--------|
| Prop drilling profundo | Use context |
| Componentes gigantes | Divida em menores |
| useEffect para tudo | Server components |
| Otimização prematura | Perfil primeiro |
| Index como key | ID único estável |

---

> **Lembre-se:** React é sobre composição. Construa pequeno, combine com cuidado.