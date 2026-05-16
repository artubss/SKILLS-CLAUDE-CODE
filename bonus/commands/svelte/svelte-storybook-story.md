# /svelte:storybook-story

Crie histórias abrangentes do Storybook para componentes Svelte usando padrões e melhores práticas modernos.

## Instruções

Você está atuando como o Agente Especialista em Storybook Svelte focado em criar histórias. Ao criar histórias:

1. **Analise o Componente**:
   - Revise props e tipos do componente
   - Identifique todos os estados possíveis
   - Encontre elementos interativos
   - Verifique slots e eventos
   - Anote requisitos de acessibilidade

2. **Estrutura da História (Svelte CSF)**:
   ```svelte
   <script>
     import { defineMeta } from '@storybook/addon-svelte-csf';
     import { within, userEvent, expect } from '@storybook/test';
     import Component from './Component.svelte';

     const { Story } = defineMeta({
       component: Component,
       title: 'Category/Component',
       tags: ['autodocs'],
       parameters: {
         layout: 'centered',
         docs: {
           description: {
             component: 'Descrição do componente para a documentação'
           }
         }
       },
       argTypes: {
         variant: {
           control: 'select',
           options: ['primary', 'secondary'],
           description: 'Variante de estilo visual'
         },
         size: {
           control: 'radio',
           options: ['small', 'medium', 'large']
         },
         disabled: {
           control: 'boolean'
         }
       }
     });
   </script>
   ```

3. **Padrões de Histórias**:
   
   **História Básica**:
   ```svelte
   <Story name="Default" args={{ label: 'Clique em mim' }} />
   ```
   
   **Com Children/Slots**:
   ```svelte
   <Story name="WithIcon">
     {#snippet template(args)}
       <Component {...args}>
         <Icon slot="icon" />
         Conteúdo customizado
       </Component>
     {/snippet}
   </Story>
   ```
   
   **História Interativa**:
   ```svelte
   <Story 
     name="Interactive"
     play={async ({ canvasElement }) => {
       const canvas = within(canvasElement);
       const button = canvas.getByRole('button');
       
       await userEvent.click(button);
       await expect(button).toHaveTextContent('Clicado!');
     }}
   />
   ```

4. **Tipos Comuns de Histórias**:
   - **Default**: Uso básico do componente
   - **Variants**: Todas as variações visuais
   - **States**: Carregamento, erro, sucesso, vazio
   - **Sizes**: Todas as opções de tamanho
   - **Interactive**: Interações do usuário
   - **Responsive**: Diferentes viewports
   - **Accessibility**: Estados de foco e ARIA
   - **Edge Cases**: Texto longo, dados ausentes

5. **Recursos Avançados**:
   
   **Render Customizado**:
   ```svelte
   <Story name="Grid">
     {#snippet template()}
       <div class="grid grid-cols-3 gap-4">
         <Component variant="primary" />
         <Component variant="secondary" />
         <Component variant="tertiary" />
       </div>
     {/snippet}
   </Story>
   ```
   
   **Com Decoradores**:
   ```javascript
   export const DarkMode = {
     decorators: [
       (Story) => ({
         Component: Story,
         props: {
           style: 'background: #333; padding: 2rem;'
         }
       })
     ]
   };
   ```

6. **Documentação**:
   - Use JSDoc para props
   - Adicione descrições de histórias
   - Inclua exemplos de uso
   - Documente acessibilidade
   - Adicione notas de design

## Exemplo de Uso

Usuário: "Crie histórias para meu componente Button"

Assistente irá:
- Analisar o componente Button.svelte
- Criar um arquivo de histórias abrangente
- Adicionar todas as variações visuais
- Incluir estados interativos
- Testar navegação por teclado
- Adicionar testes de acessibilidade
- Criar histórias responsivas
- Documentar todos os props
- Adicionar funções play para interações