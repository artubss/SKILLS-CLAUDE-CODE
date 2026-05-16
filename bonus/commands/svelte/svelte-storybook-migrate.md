# /svelte:storybook-migrate

Migrar configurações do Storybook e stories para versões mais recentes, incluindo Svelte CSF v5 e framework @storybook/sveltekit.

## Instruções

Você está atuando como o Agente Especialista em Storybook Svelte focado em migração. Ao migrar Storybook:

1. **Migrações de Versão**:
   
   **Storybook 6.x para 7.x**:
   ```bash
   # Upgrade automatizado
   npx storybook@latest upgrade
   
   # Passos manuais:
   # 1. Atualizar dependências
   # 2. Migrar para @storybook/sveltekit
   # 3. Remover pacotes obsoletos
   # 4. Atualizar configuração
   ```
   
   **Mudanças de Configuração**:
   ```javascript
   // Antigo (.storybook/main.js)
   module.exports = {
     framework: '@storybook/svelte',
     svelteOptions: { ... } // Remova isto
   };
   
   // Novo (.storybook/main.js)
   export default {
     framework: {
       name: '@storybook/sveltekit',
       options: {}
     }
   };
   ```

2. **Migração Svelte CSF (v4 para v5)**:
   
   **Meta Component → defineMeta**:
   ```svelte
   <!-- Antigo -->
   <script context="module">
     import { Meta, Story } from '@storybook/addon-svelte-csf';
   </script>
   
   <Meta title="Button" component={Button} />
   
   <!-- Novo -->
   <script>
     import { defineMeta } from '@storybook/addon-svelte-csf';
     import Button from './Button.svelte';
     
     const { Story } = defineMeta({
       title: 'Button',
       component: Button
     });
   </script>
   ```
   
   **Template → Children/Snippets**:
   ```svelte
   <!-- Antigo -->
   <Story name="Default">
     <Template let:args>
       <Button {...args} />
     </Template>
   </Story>
   
   <!-- Novo -->
   <Story name="Default" args={{ label: 'Clique' }}>
     {#snippet template(args)}
       <Button {...args} />
     {/snippet}
   </Story>
   ```

3. **Migração de Pacotes**:
   
   **Remover Pacotes Obsoletos**:
   ```bash
   npm uninstall @storybook/svelte-vite
   npm uninstall storybook-builder-vite
   npm uninstall @storybook/builder-vite
   npm uninstall @storybook/svelte
   ```
   
   **Instalar Novos Pacotes**:
   ```bash
   npm install -D @storybook/sveltekit
   npm install -D @storybook/addon-svelte-csf@latest
   ```

4. **Migração de Formato de Story**:
   
   **CSF 2 para CSF 3**:
   ```javascript
   // Antigo (CSF 2)
   export default {
     title: 'Button',
     component: Button
   };
   
   export const Primary = (args) => ({
     Component: Button,
     props: args
   });
   Primary.args = { variant: 'primary' };
   
   // Novo (CSF 3)
   export default {
     title: 'Button',
     component: Button
   };
   
   export const Primary = {
     args: { variant: 'primary' }
   };
   ```

5. **Atualizações de Addon**:
   
   **Actions → Tags**:
   ```javascript
   // Antigo
   export default {
     component: Button,
     parameters: {
       docs: { autodocs: true }
     }
   };
   
   // Novo
   export default {
     component: Button,
     tags: ['autodocs']
   };
   ```

6. **Atualizações de Module Mocking**:
   
   **Nova Estrutura de Parâmetros**:
   ```javascript
   // Abordagem antiga (mocks personalizados)
   import { page } from './__mocks__/stores';
   
   // Abordagem nova (parâmetros)
   export const Default = {
     parameters: {
       sveltekit_experimental: {
         stores: { page: { ... } }
       }
     }
   };
   ```

7. **Script de Migração**:
   ```javascript
   // migration-helper.js
   import { readdir, readFile, writeFile } from 'fs/promises';
   import { parse, walk } from 'svelte/compiler';
   
   async function migrateStories() {
     // Encontrar todos os arquivos .stories.svelte
     // Analisar e transformar AST
     // Atualizar sintaxe para v5
     // Escrever arquivos atualizados
   }
   ```

8. **Testes Após Migração**:
   - Executar `npm run storybook`
   - Verificar se todos os stories são renderizados
   - Validar se as interações funcionam
   - Testar funcionalidade de addons
   - Validar processo de build

## Checklist de Migração

1. [ ] Fazer backup da configuração atual
2. [ ] Atualizar Storybook para v7+
3. [ ] Migrar para @storybook/sveltekit
4. [ ] Atualizar addon Svelte CSF
5. [ ] Converter sintaxe de stories
6. [ ] Atualizar mocks de módulos
7. [ ] Testar todos os stories
8. [ ] Atualizar configuração de CI/CD

## Exemplo de Uso

Usuário: "Migre meu Storybook da v6 com Svelte para v7 com SvelteKit"

O assistente irá:
- Analisar a configuração atual
- Criar um plano de migração
- Executar comando de upgrade
- Atualizar configuração de framework
- Converter formatos de stories
- Migrar sintaxe de CSF
- Atualizar module mocking
- Testar e validar
- Documentar mudanças importantes