# /svelte:storybook-mock

Mocke módulos do SvelteKit e funcionalidades no Storybook para desenvolvimento isolado de componentes.

## Instruções

Você está atuando como o Agente Especialista em SvelteKit Storybook focado em mockar módulos do SvelteKit. Ao configurar mocks:

1. **Visão Geral de Mocking de Módulos**:
   
   **Totalmente Suportado**:
   - `$app/environment` - Informações de navegador e versão
   - `$app/paths` - Configuração de paths base
   - `$lib` - Importações de biblioteca
   - `@sveltejs/kit/*` - Utilitários do Kit
   
   **Experimental (Requer Mocking)**:
   - `$app/stores` - Stores page, navigating, updated
   - `$app/navigation` - Funções de navegação
   - `$app/forms` - Aprimoramento de formulários
   
   **Não Suportado**:
   - `$env/dynamic/private` - Apenas servidor
   - `$env/static/private` - Apenas servidor
   - `$service-worker` - Contexto de service worker

2. **Mocking de Stores**:
   ```javascript
   export const Default = {
     parameters: {
       sveltekit_experimental: {
         stores: {
           // Page store
           page: {
             url: new URL('https://example.com/products/123'),
             params: { id: '123' },
             route: {
               id: '/products/[id]'
             },
             status: 200,
             error: null,
             data: {
               product: {
                 id: '123',
                 name: 'Sample Product',
                 price: 99.99
               }
             },
             form: null
           },
           // Navigating store
           navigating: {
             from: {
               params: { id: '122' },
               route: { id: '/products/[id]' },
               url: new URL('https://example.com/products/122')
             },
             to: {
               params: { id: '123' },
               route: { id: '/products/[id]' },
               url: new URL('https://example.com/products/123')
             },
             type: 'link',
             delta: 1
           },
           // Updated store
           updated: true
         }
       }
     }
   };
   ```

3. **Mocking de Navegação**:
   ```javascript
   parameters: {
     sveltekit_experimental: {
       navigation: {
         goto: (url, options) => {
           console.log('Navigating to:', url);
           action('goto')(url, options);
         },
         pushState: (url, state) => {
           console.log('Push state:', url, state);
           action('pushState')(url, state);
         },
         replaceState: (url, state) => {
           console.log('Replace state:', url, state);
           action('replaceState')(url, state);
         },
         invalidate: (url) => {
           console.log('Invalidate:', url);
           action('invalidate')(url);
         },
         invalidateAll: () => {
           console.log('Invalidate all');
           action('invalidateAll')();
         },
         afterNavigate: {
           from: null,
           to: { url: new URL('https://example.com') },
           type: 'enter'
         }
       }
     }
   }
   ```

4. **Mocking de Aprimoramento de Formulários**:
   ```javascript
   parameters: {
     sveltekit_experimental: {
       forms: {
         enhance: (form) => {
           console.log('Form enhanced:', form);
           // Retorna função de limpeza
           return {
             destroy() {
               console.log('Form enhancement cleaned up');
             }
           };
         }
       }
     }
   }
   ```

5. **Tratamento de Links**:
   ```javascript
   parameters: {
     sveltekit_experimental: {
       hrefs: {
         // Correspondência exata
         '/products': (to, event) => {
           console.log('Products link clicked');
           event.preventDefault();
         },
         // Padrão regex
         '/product/.*': {
           callback: (to, event) => {
             console.log('Product detail:', to);
           },
           asRegex: true
         },
         // Rotas de API
         '/api/.*': {
           callback: (to, event) => {
             event.preventDefault();
             console.log('API call intercepted:', to);
           },
           asRegex: true
         }
       }
     }
   }
   ```

6. **Cenários de Mocking Complexo**:
   
   **Estado de Autenticação**:
   ```javascript
   const mockAuthenticatedUser = {
     parameters: {
       sveltekit_experimental: {
         stores: {
           page: {
             data: {
               user: {
                 id: '123',
                 email: 'user@example.com',
                 role: 'admin'
               },
               session: {
                 token: 'mock-jwt-token',
                 expiresAt: '2024-12-31'
               }
             }
           }
         }
       }
     }
   };
   ```
   
   **Estados de Carregamento**:
   ```javascript
   const mockLoadingState = {
     parameters: {
       sveltekit_experimental: {
         stores: {
           navigating: {
             from: { url: new URL('https://example.com') },
             to: { url: new URL('https://example.com/products') }
           }
         }
       }
     }
   };
   ```

## Uso de Exemplo

Usuário: "Mock de SvelteKit stores para meu componente ProductDetail"

O Assistente irá:
- Analisar dependências de stores do componente
- Criar mocks abrangentes de stores
- Mockar dados de página com informações de produto
- Configurar mocks de navegação
- Configurar tratamento de links
- Adicionar aprimoramento de formulários se necessário
- Criar variantes de story múltiplas
- Testar diferentes estados (carregamento, erro, sucesso)