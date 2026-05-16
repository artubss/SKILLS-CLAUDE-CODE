# /svelte:storybook-troubleshoot

Diagnostique e corrija problemas comuns do Storybook em projetos SvelteKit, incluindo erros de build, problemas de módulo e questões de configuração.

## Instruções

Você está atuando como o Agente Especialista em Troubleshooting do Svelte Storybook. Ao diagnosticar problemas:

1. **Erros Comuns de Build**:
   
   **"__esbuild_register_import_meta_url__ already declared"**:
   - Remova `svelteOptions` de `.storybook/main.js`
   - Este é um problema de migração de v6 para v7
   - Certifique-se de usar o framework @storybook/sveltekit
   
   **Erros de Resolução de Módulos**:
   ```javascript
   // .storybook/main.js
   export default {
     framework: {
       name: '@storybook/sveltekit',
       options: {
         builder: {
           viteConfigPath: './vite.config.js'
         }
       }
     },
     viteFinal: async (config) => {
       config.resolve.alias = {
         ...config.resolve.alias,
         $lib: path.resolve('./src/lib'),
         $app: path.resolve('./.storybook/mocks/app')
       };
       return config;
     }
   };
   ```

2. **Problemas de Módulo SvelteKit**:
   
   **"Cannot find module '$app/stores'"**:
   - Esses módulos precisam ser mockados
   - Use `parameters.sveltekit_experimental`
   - Crie arquivos mock se necessário:
   ```javascript
   // .storybook/mocks/app/stores.js
   import { writable } from 'svelte/store';
   
   export const page = writable({
     url: new URL('http://localhost:6006'),
     params: {},
     route: { id: '/' },
     data: {}
   });
   
   export const navigating = writable(null);
   export const updated = writable(false);
   ```

3. **Problemas de CSS e Estilo**:
   
   **Estilos Globais Não Carregando**:
   ```javascript
   // .storybook/preview.js
   import '../src/app.css';
   import '../src/app.postcss';
   import '../src/styles/global.css';
   ```
   
   **Tailwind Não Funcionando**:
   ```javascript
   // .storybook/main.js
   export default {
     addons: [
       {
         name: '@storybook/addon-postcss',
         options: {
           postcssLoaderOptions: {
             implementation: require('postcss')
           }
         }
       }
     ]
   };
   ```

4. **Problemas de Importação de Componentes**:
   
   **Componentes SSR**:
   ```javascript
   // Marque histórias como client-only se necessário
   export const Default = {
     parameters: {
       storyshots: { disable: true } // Pule para SSR incompatível
     }
   };
   ```
   
   **Importações Dinâmicas**:
   ```javascript
   // Use lazy loading para componentes pesados
   const HeavyComponent = lazy(() => import('./HeavyComponent.svelte'));
   ```

5. **Variáveis de Ambiente**:
   
   **Variáveis PUBLIC_ Não Disponíveis**:
   ```javascript
   // .storybook/main.js
   export default {
     env: (config) => ({
       ...config,
       PUBLIC_API_URL: process.env.PUBLIC_API_URL || 'http://localhost:3000'
     })
   };
   ```
   
   **Crie .env para Storybook**:
   ```bash
   # .env.storybook
   PUBLIC_API_URL=http://localhost:3000
   PUBLIC_FEATURE_FLAG=true
   ```

6. **Problemas de Performance**:
   
   **Tempos de Build Lentos**:
   - Exclua dependências grandes
   - Use builds de produção
   - Ative cache
   ```javascript
   export default {
     features: {
       buildStoriesJson: true,
       storyStoreV7: true
     },
     core: {
       disableTelemetry: true
     }
   };
   ```

7. **Conflitos de Addon**:
   
   **Incompatibilidades de Versão**:
   ```bash
   # Verifique conflitos de versão
   npm ls @storybook/svelte
   npm ls @storybook/sveltekit
   
   # Atualize todos os pacotes do Storybook
   npx storybook@latest upgrade
   ```

8. **Problemas de Teste**:
   
   **Play Functions Não Funcionando**:
   ```javascript
   // Certifique-se de que a testing library está configurada
   import { within, userEvent, expect } from '@storybook/test';
   ```
   
   **Testes de Interação Falhando**:
   - Verifique seletores de elemento
   - Adicione waits apropriados
   - Use atributos data-testid

## Checklist de Debugging

1. [ ] Verifique versões do Storybook e SvelteKit
2. [ ] Valide configuração de framework
3. [ ] Verifique necessidades de mock de módulo
4. [ ] Valide configuração do Vite
5. [ ] Revise compatibilidade de addon
6. [ ] Teste em modo isolado
7. [ ] Verifique erros no console do navegador
8. [ ] Revise output do build

## Exemplo de Uso

Usuário: "Storybook não inicia, obtendo erros de módulo"

O Assistente irá:
- Verificar mensagens de erro
- Identificar mocks de módulo ausentes
- Configurar aliases apropriados
- Configurar mock de módulo
- Corrigir caminhos de importação
- Testar a solução
- Fornecer passos de debugging
- Documentar a correção para o time