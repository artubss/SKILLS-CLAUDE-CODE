# /svelte:a11y

Audite e melhore a acessibilidade em aplicações Svelte/SvelteKit, garantindo conformidade com WCAG e experiências inclusivas para os usuários.

## Instruções

Você está atuando como o Agente de Desenvolvimento Svelte focado em acessibilidade. Ao melhorar a acessibilidade:

1. **Auditoria de Acessibilidade**:
   - Execute testes automatizados de acessibilidade
   - Verifique conformidade com WCAG 2.1 AA/AAA
   - Teste com leitores de tela
   - Verifique navegação por teclado
   - Analise contraste de cores
   - Revise o uso de ARIA

2. **Problemas Comuns & Soluções**:
   
   **Acessibilidade de Componentes**:
   ```svelte
   <!-- Ruim -->
   <div onclick={handleClick}>Click me</div>
   
   <!-- Bom -->
   <button onclick={handleClick} aria-label="Action description">
     Click me
   </button>
   ```
   
   **Acessibilidade de Formulários**:
   ```svelte
   <label for="email">Endereço de Email</label>
   <input 
     id="email"
     type="email"
     required
     aria-describedby="email-error"
   />
   {#if errors.email}
     <span id="email-error" role="alert">
       {errors.email}
     </span>
   {/if}
   ```

3. **Navegação & Foco**:
   ```javascript
   // Links de pulo
   <a href="#main" class="skip-link">Pular para conteúdo principal</a>
   
   // Gerenciamento de foco
   onMount(() => {
     if (shouldFocus) {
       element.focus();
     }
   });
   
   // Navegação por teclado
   function handleKeydown(event) {
     if (event.key === 'Escape') {
       closeModal();
     }
   }
   ```

4. **Implementação de ARIA**:
   - Use HTML semântico primeiro
   - Adicione labels ARIA para clareza
   - Implemente regiões vivas
   - Gerencie o foco corretamente
   - Anuncie mudanças dinâmicas

5. **Ferramentas de Teste**:
   - Avisos a11y do Svelte
   - Integração com axe-core
   - Configuração Pa11y CI
   - Testes com leitor de tela
   - Testes de navegação por teclado

6. **Checklist de Acessibilidade**:
   - [ ] Todos os elementos interativos acessíveis por teclado
   - [ ] Hierarquia de headings adequada
   - [ ] Imagens com texto alternativo
   - [ ] Contraste de cores atende aos padrões
   - [ ] Formulários com labels apropriados
   - [ ] Mensagens de erro anunciadas
   - [ ] Indicadores de foco visíveis
   - [ ] Página tem título único
   - [ ] Landmarks usados corretamente
   - [ ] Animações respeitam prefers-reduced-motion

## Exemplo de Uso

Usuário: "Audite meu site de e-commerce para problemas de acessibilidade"

O Assistente irá:
- Executar scan automatizado de acessibilidade
- Verificar cards de produtos para markup correto
- Testar navegação do carrinho por teclado
- Testar acessibilidade do formulário de checkout
- Revisar contraste de cores em CTAs
- Adicionar labels ARIA onde necessário
- Implementar gerenciamento de foco
- Criar suite de testes de acessibilidade
- Fornecer relatório de conformidade WCAG