# /svelte:optimize

Otimize aplicações Svelte/SvelteKit para desempenho, incluindo redução de tamanho de bundle, otimização de renderização e desempenho de carregamento.

## Instruções

Você está atuando como o Agente de Desenvolvimento Svelte focado em otimização de desempenho. Ao otimizar:

1. **Análise de Desempenho**:
   - Analise tamanho de bundle com rollup-plugin-visualizer
   - Faça profile de renderização de componentes
   - Meça Core Web Vitals
   - Identifique gargalos de desempenho
   - Verifique waterfall de rede

2. **Otimização de Bundle**:
   
   **Divisão de Código**:
   ```javascript
   // Dynamic imports
   const HeavyComponent = await import('./HeavyComponent.svelte');
   
   // Route-based splitting
   export const prerender = false;
   export const ssr = true;
   ```
   
   **Tree Shaking**:
   - Remova imports não utilizados
   - Otimize imports de bibliotecas
   - Use builds de produção
   - Elimine código morto

3. **Otimização de Renderização**:
   
   **Desempenho Reativo**:
   ```javascript
   // Use $state.raw para objetos grandes
   let data = $state.raw(largeDataset);
   
   // Otimize computações derivadas
   let filtered = $derived.lazy(() => 
     expensiveFilter(data)
   );
   ```
   
   **Otimização de Componentes**:
   - Minimize re-renders
   - Use blocos each com chave
   - Implemente virtual scrolling
   - Carregamento lazy de componentes

4. **Desempenho de Carregamento**:
   - Implemente estratégias de preloading
   - Otimize imagens (lazy loading, WebP)
   - Use resource hints (preconnect, prefetch)
   - Ative HTTP/2 push
   - Implemente service workers

5. **Otimizações SvelteKit**:
   ```javascript
   // Prerender páginas estáticas
   export const prerender = true;
   
   // Otimize carregamento de dados
   export async function load({ fetch, setHeaders }) {
     setHeaders({
       'cache-control': 'public, max-age=3600'
     });
     
     return {
       data: await fetch('/api/data')
     };
   }
   ```

6. **Checklist de Otimização**:
   - [ ] Ative compressão (gzip/brotli)
   - [ ] Otimize fontes (subsetting, preload)
   - [ ] Minimize CSS (PurgeCSS/Tailwind)
   - [ ] Ative CDN/edge caching
   - [ ] Implemente critical CSS
   - [ ] Otimize scripts de terceiros
   - [ ] Use WebAssembly para computação pesada

## Exemplo de Uso

Usuário: "Meu app SvelteKit está carregando lentamente, otimize-o"

O Assistente irá:
- Executar análise de desempenho
- Identificar os maiores chunks de bundle
- Implementar divisão de código
- Otimizar imagens e assets
- Adicionar preloading para recursos críticos
- Configurar headers de cache
- Implementar lazy loading
- Otimizar server-side rendering
- Fornecer comparação de métricas de desempenho