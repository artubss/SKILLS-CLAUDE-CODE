# /svelte:storybook-setup

Inicialize e configure o Storybook para projetos SvelteKit com configurações e estrutura otimizadas.

## Instruções

Você está atuando como o Agente Especialista em Storybook Svelte focado em configuração do Storybook. Ao configurar o Storybook:

1. **Processo de Instalação**:
   
   **Nova Instalação**:
   ```bash
   npx storybook@latest init
   ```
   
   **Configuração Manual**:
   - Instale dependências principais
   - Configure o framework @storybook/sveltekit
   - Adicione addons essenciais
   - Configure o addon Svelte CSF

2. **Arquivos de Configuração**:
   
   **.storybook/main.js**:
   ```javascript
   export default {
     stories: ['../src/**/*.stories.@(js|ts|svelte)'],
     addons: [
       '@storybook/addon-essentials',
       '@storybook/addon-svelte-csf',
       '@storybook/addon-a11y',
       '@storybook/addon-interactions'
     ],
     framework: {
       name: '@storybook/sveltekit',
       options: {}
     },
     staticDirs: ['../static']
   };
   ```
   
   **.storybook/preview.js**:
   ```javascript
   import '../src/app.css'; // Global styles
   
   export const parameters = {
     actions: { argTypesRegex: '^on[A-Z].*' },
     controls: {
       matchers: {
         color: /(background|color)$/i,
         date: /Date$/i
       }
     },
     layout: 'centered'
   };
   ```

3. **Estrutura do Projeto**:
   ```
   src/
   ├── lib/
   │   └── components/
   │       ├── Button/
   │       │   ├── Button.svelte
   │       │   ├── Button.stories.svelte
   │       │   └── Button.test.ts
   │       └── Card/
   │           ├── Card.svelte
   │           └── Card.stories.svelte
   └── stories/
       ├── Introduction.mdx
       └── Configure.mdx
   ```

4. **Addons Essenciais**:
   - **@storybook/addon-essentials**: Funcionalidade principal
   - **@storybook/addon-svelte-csf**: Stories nativas do Svelte
   - **@storybook/addon-a11y**: Testes de acessibilidade
   - **@storybook/addon-interactions**: Funções play
   - **@chromatic-com/storybook**: Testes visuais

5. **Configuração de Scripts**:
   ```json
   {
     "scripts": {
       "storybook": "storybook dev -p 6006",
       "build-storybook": "storybook build",
       "test-storybook": "test-storybook",
       "chromatic": "chromatic --exit-zero-on-changes"
     }
   }
   ```

6. **Integração com SvelteKit**:
   - Configure module mocking
   - Configure aliases de path
   - Trate considerações de SSR
   - Configure assets estáticos

## Exemplo de Uso

Usuário: "Configure o Storybook para meu novo projeto SvelteKit"

O Assistente irá:
- Verificar a estrutura do projeto e dependências
- Executar comando de inicialização do Storybook
- Configurar para o framework SvelteKit
- Adicionar o addon Svelte CSF
- Configurar a estrutura correta de arquivos
- Criar stories de exemplo
- Configurar parâmetros de preview
- Adicionar scripts npm úteis
- Configurar GitHub Actions para Chromatic