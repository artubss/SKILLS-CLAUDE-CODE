---
allowed-tools: Read, Write, Edit
argument-hint: [component-name] [--typescript] [--story]
description: Criar novos componentes Svelte com melhores práticas, suporte a TypeScript e testes
---

# Criar Componente Svelte

Criar novo componente Svelte: $ARGUMENTS

## Projeto Svelte Atual

- Configuração Svelte: @svelte.config.js ou @vite.config.js (se existe)
- Diretório de componentes: @src/components/ ou @src/lib/ (se existe)
- Configuração TypeScript: @tsconfig.json (detectar uso de TypeScript)
- Setup de testes: @vitest.config.js ou @jest.config.js (se existe)

## Tarefa

Criar componente Svelte com melhores práticas. Ao criar componentes:

1. **Coletar Requisitos**:
   - Nome e propósito do componente
   - Interface de props
   - Eventos a emitir
   - Slots necessários
   - Requisitos de gerenciamento de estado
   - Preferência de TypeScript

2. **Estrutura do Componente**:
   ```svelte
   <script lang="ts">
     // Importações
     // Definições de tipos
     // Props
     // Estado
     // Valores derivados
     // Efeitos
     // Funções
   </script>
   
   <!-- Markup -->
   
   <style>
     /* Estilos com escopo */
   </style>
   ```

3. **Melhores Práticas**:
   - Usar tipagem correta de props com TypeScript/JSDoc
   - Implementar props $bindable quando apropriado
   - Criar markup acessível por padrão
   - Adicionar atributos ARIA apropriados
   - Usar elementos HTML semânticos
   - Incluir suporte a navegação por teclado

4. **Tipos de Componentes a Criar**:
   - **Componentes UI**: Botões, Cards, Modals, etc.
   - **Componentes de Formulário**: Inputs com validação, controles de formulário customizados
   - **Componentes de Layout**: Headers, Sidebars, Grids
   - **Componentes de Dados**: Tabelas, Listas, Visualizações de dados
   - **Componentes Utilitários**: Portals, Transições, Error boundaries

5. **Arquivos Adicionais**:
   - Criar arquivo de teste acompanhante
   - Adicionar story do Storybook se aplicável
   - Criar documentação de uso
   - Exportar a partir de arquivo index

## Exemplo de Uso

Usuário: "Criar um componente Modal com slots de header e footer customizáveis, e funcionalidade de fechamento"

Assistente irá:
- Criar Modal.svelte com estrutura apropriada
- Implementar focus trap e tratamento de teclado
- Adicionar efeitos de transição
- Criar Modal.test.js com testes básicos
- Fornecer exemplos de uso
- Sugerir melhorias de acessibilidade