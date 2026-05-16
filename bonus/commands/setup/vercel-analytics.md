---
allowed-tools: Read, Write, Edit, Bash
argument-hint:
description: Configurar Vercel Analytics e Speed Insights para projetos React/Vite
---

# Configuração do Vercel Analytics

Configure automaticamente Vercel Analytics e Speed Insights para seu projeto React/Vite.

**Uso:** `/vercel-analytics` (nenhum argumento necessário)

**O que faz:**
- Instala os pacotes @vercel/analytics e @vercel/speed-insights
- Adiciona componentes ao seu app React
- Configura roteamento SPA para deploy no Vercel
- Corrige erros 404 para acesso direto a rotas

**Processo:**

1. **Instalar Pacotes Vercel**
   ```bash
   npm install @vercel/analytics @vercel/speed-insights
   ```

2. **Detectar Arquivo Principal da App**
   - Procura pelo ponto de entrada principal do React:
     - `src/App.tsx` ou `src/App.jsx`
     - `src/main.tsx` ou `src/main.jsx`
   - Lê o arquivo para determinar a estrutura atual

3. **Adicionar Componentes de Analytics**
   - Importar Analytics de '@vercel/analytics/react'
   - Importar SpeedInsights de '@vercel/speed-insights/react'
   - Adicionar ambos os componentes ao componente App principal
   - Usar imports `/react` (não `/next`)

4. **Criar Configuração vercel.json**
   - Criar `vercel.json` na raiz do projeto
   - Adicionar regras de rewrite para SPA:
   ```json
   {
     "rewrites": [
       { "source": "/(.*)", "destination": "/index.html" }
     ]
   }
   ```
   - Isso garante que todas as rotas sirvam index.html (corrige 404s)

5. **Verificar Configuração**
   - Confirmar que componentes estão importados corretamente
   - Verificar se vercel.json existe e é válido
   - Exibir mensagem de sucesso com próximos passos

**Resultado Esperado:**
- ✅ Rastreamento de Analytics ativo
- ✅ Monitoramento de Speed Insights configurado
- ✅ Roteamento SPA funciona corretamente no Vercel
- ✅ Sem erros 404 no acesso direto a rotas

**Próximos Passos:**
1. Deploy para Vercel: `vercel deploy`
2. Visualizar analytics em: https://vercel.com/dashboard/analytics
3. Verificar Speed Insights: https://vercel.com/dashboard/speed-insights

**Nota**: Funciona com React, Vite, Create React App e outros frameworks SPA.