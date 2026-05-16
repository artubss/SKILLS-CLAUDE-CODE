---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [build-tool] | --webpack | --vite | --rollup
description: Reduzir e otimizar tamanhos de bundle através de análise, configuração e estratégias de code splitting
---

# Otimizar Tamanho do Bundle

Reduza e otimize tamanhos de bundle: **$ARGUMENTS**

## Instruções

1. **Análise e Avaliação do Bundle**
   - Analise o tamanho atual e a composição do bundle usando webpack-bundle-analyzer ou ferramentas similares
   - Identifique dependências grandes e código não utilizado em todos os bundles
   - Avalie a configuração de build atual e as configurações de otimização
   - Crie medições de linha de base para rastreamento de otimização
   - Documente métricas de desempenho e tempos de carregamento atuais

2. **Configuração da Ferramenta de Build**
   - Configure as definições de otimização da ferramenta de build para builds em produção
   - Ative recursos de code splitting e otimização de chunks
   - Configure tree shaking e eliminação de código morto
   - Defina analisadores de bundle e ferramentas de visualização
   - Otimize o desempenho do build e os tamanhos de saída

3. **Code Splitting e Carregamento Dinâmico**
   - Implemente code splitting baseado em rotas para aplicações single-page
   - Configure imports dinâmicos para componentes e módulos
   - Configure carregamento dinâmico para recursos não críticos
   - Otimize os tamanhos dos chunks e estratégias de carregamento
   - Implemente padrões de carregamento progressivo

4. **Tree Shaking e Eliminação de Código Morto**
   - Configure ferramentas de build para tree shaking otimizado
   - Marque pacotes como livres de side-effects quando apropriado
   - Otimize instruções de import para melhor tree shaking
   - Use módulos ES6 e evite CommonJS quando possível
   - Implemente plugins Babel para otimização automática de imports

5. **Otimização de Dependências**
   - Analise e audite dependências de pacotes quanto ao impacto de tamanho
   - Substitua bibliotecas grandes por alternativas menores
   - Use imports específicos em vez de importar bibliotecas inteiras
   - Implemente estratégias de deduplicação de dependências
   - Configure dependências externas e uso de CDN

6. **Otimização de Assets**
   - Otimize imagens através de compressão e conversão de formato
   - Implemente estratégias de carregamento de imagens responsivas
   - Configure minificação e compressão de assets
   - Defina loaders e processadores de arquivo eficientes
   - Otimize carregamento e subsetting de fontes

7. **Module Federation e Micro-frontends**
   - Implemente module federation para aplicações grandes
   - Configure dependências compartilhadas e otimização em tempo de execução
   - Defina arquitetura de micro-frontend para compartilhamento de código
   - Otimize carregamento e caching de módulos remotos
   - Implemente monitoramento de desempenho de federation

8. **Monitoramento e Medição de Desempenho**
   - Defina monitoramento e rastreamento de tamanho do bundle
   - Configure análise automática de bundle em CI/CD
   - Monitore mudanças de tamanho do bundle ao longo do tempo
   - Defina budgets de desempenho e alertas
   - Rastreie métricas de desempenho de carregamento

9. **Estratégias de Carregamento Progressivo**
   - Implemente dicas de recurso (preload, prefetch, dns-prefetch)
   - Configure service workers para estratégias de cache
   - Defina intersection observer para carregamento dinâmico
   - Otimize prioridades de carregamento de recursos críticos
   - Implemente carregamento adaptativo baseado em velocidade de conexão

10. **Validação e Monitoramento Contínuo**
    - Defina validação automática de tamanho do bundle em CI/CD
    - Configure limites de tamanho do bundle e alertas
    - Implemente testes de regressão de tamanho do bundle
    - Monitore desempenho de carregamento no mundo real
    - Defina recomendações automáticas de otimização

Foque em otimizações que forneçam as reduções mais significativas de tamanho do bundle enquanto mantêm a funcionalidade da aplicação. Sempre meça o impacto das mudanças tanto no tamanho do bundle quanto no desempenho em tempo de execução.