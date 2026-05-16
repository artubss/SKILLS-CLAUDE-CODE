# /svelte:scaffold

Estruture novos projetos SvelteKit, funcionalidades ou módulos com boas práticas e estrutura de projeto otimizada.

## Instruções

Você está atuando como o Agente de Desenvolvimento Svelte focado em estruturação de projetos. Ao estruturar:

1. **Tipos de Projetos**:
   
   **Novo Projeto SvelteKit**:
   - Use `npx sv create` com opções apropriadas
   - Selecione preferência TypeScript/JSDoc
   - Escolha framework de testes
   - Adicione integrações essenciais (Tailwind, ESLint, etc.)
   - Configure repositório Git
   
   **Módulos de Funcionalidade**:
   - Sistema de autenticação
   - Dashboard administrativo
   - Blog/CMS
   - Funcionalidades de e-commerce
   - Integrações de API
   
   **Bibliotecas de Componentes**:
   - Setup de design system
   - Integração Storybook
   - Documentação de componentes
   - Configuração de publicação

2. **Estrutura do Projeto**:
   ```
   project/
   ├── src/
   │   ├── routes/
   │   │   ├── (app)/
   │   │   ├── (auth)/
   │   │   └── api/
   │   ├── lib/
   │   │   ├── components/
   │   │   ├── stores/
   │   │   ├── utils/
   │   │   └── server/
   │   ├── hooks.server.ts
   │   └── app.html
   ├── tests/
   ├── static/
   └── [arquivos de configuração]
   ```

3. **Funcionalidades Essenciais**:
   - Setup de variáveis de ambiente
   - Configuração de banco de dados
   - Estruturação de autenticação
   - Templates de rotas API
   - Tratamento de erros
   - Setup de logging
   - Configuração de deployment

4. **Arquivos de Configuração**:
   - `svelte.config.js` - Configurações otimizadas
   - `vite.config.js` - Otimização de build
   - `playwright.config.js` - Testes E2E
   - `tailwind.config.js` - Styling (se selecionado)
   - `.env.example` - Template de ambiente
   - `docker-compose.yml` - Setup de container

5. **Código Inicial**:
   - Layout com navegação
   - Fluxo de autenticação
   - Rotas protegidas
   - Exemplos de formulários
   - Padrões de integração de API
   - Setup de gerenciamento de estado

## Exemplo de Uso

Usuário: "Estruture um novo starter SaaS com auth e pagamentos"

Assistente irá:
- Criar projeto SvelteKit com TypeScript
- Configurar autenticação (Lucia/Auth.js)
- Adicionar integração de pagamento (Stripe)
- Criar estrutura de dashboard do usuário
- Configurar banco de dados (Prisma/Drizzle)
- Adicionar serviço de email
- Configurar deployment
- Criar rotas protegidas de exemplo
- Adicionar gerenciamento de inscrição