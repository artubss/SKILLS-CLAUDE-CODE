# Otimizar Comando de Build

Otimize processos de build e aumente a velocidade

## Instruções

Siga esta abordagem sistemática para otimizar o desempenho de build: **$ARGUMENTS**

1. **Análise do Sistema de Build**
   - Identifique o sistema de build em uso (Webpack, Vite, Rollup, Gradle, Maven, Cargo, etc.)
   - Revise arquivos de configuração e definições de build
   - Analise tempos de build atuais e tamanhos de saída
   - Mapeie o pipeline completo de build e dependências

2. **Baseline de Desempenho**
   - Meça tempos de build atuais para diferentes cenários:
     - Build limpo (do zero)
     - Build incremental (com cache)
     - Builds de desenvolvimento vs produção
   - Documente tamanhos de bundle e assets
   - Identifique as partes mais lentas do processo de build

3. **Otimização de Dependências**
   - Analise dependências de build e seu impacto
   - Remova dependências não utilizadas do processo de build
   - Atualize ferramentas de build para versões estáveis mais recentes
   - Considere ferramentas de build alternativas mais rápidas

4. **Estratégia de Cache**
   - Ative e otimize o cache de build
   - Configure cache persistente para CI/CD
   - Configure cache compartilhado para desenvolvimento em equipe
   - Implemente compilação incremental quando possível

5. **Análise de Bundle**
   - Analise composição e tamanhos de bundle
   - Identifique dependências grandes e duplicatas
   - Use analisadores de bundle específicos para sua ferramenta de build
   - Procure oportunidades para dividir bundles

6. **Code Splitting e Carregamento Lazy**
   - Implemente imports dinâmicos e code splitting
   - Configure splitting baseado em rotas para SPAs
   - Configure separação de chunk de vendor
   - Otimize tamanhos de chunks e estratégias de carregamento

7. **Otimização de Assets**
   - Otimize imagens (compressão, conversão de formato, lazy loading)
   - Minifique CSS e JavaScript
   - Configure tree shaking para remover código morto
   - Implemente compressão de assets (gzip, brotli)

8. **Otimização de Build de Desenvolvimento**
   - Ative fast refresh/hot reloading
   - Use otimizações específicas de desenvolvimento
   - Configure source maps para melhor debugging
   - Otimize configurações do servidor de desenvolvimento

9. **Otimização de Build de Produção**
   - Ative todas as otimizações de produção
   - Configure eliminação de código morto
   - Configure minificação e compressão adequadas
   - Otimize para tamanhos menores de bundle

10. **Processamento Paralelo**
    - Ative processamento paralelo onde suportado
    - Configure threads de trabalho para tarefas de build
    - Otimize para sistemas multi-núcleo
    - Use compilação paralela para TypeScript/Babel

11. **Otimização do Sistema de Arquivos**
    - Otimize monitoramento e polling de arquivos
    - Configure padrões adequados de include/exclude
    - Use loaders e processadores eficientes de arquivo
    - Minimize operações de I/O de arquivo

12. **Otimização de Build em CI/CD**
    - Otimize ambientes e recursos de build em CI
    - Implemente estratégias adequadas de cache para CI
    - Use matrizes de build eficientemente
    - Configure jobs paralelos em CI quando benéfico

13. **Otimização de Uso de Memória**
    - Monitore e otimize uso de memória durante builds
    - Configure tamanhos de heap para ferramentas de build
    - Identifique e corrija vazamentos de memória no processo de build
    - Use opções de compilação eficientes em memória

14. **Otimização de Saída**
    - Configure compressão e encoding
    - Otimize estratégias de nomenclatura e hash de arquivo
    - Configure manifestos de assets adequados
    - Implemente entrega de assets eficiente

15. **Monitoramento e Profiling**
    - Configure monitoramento de tempo de build
    - Use ferramentas de profiling de build para identificar gargalos
    - Rastreie mudanças de tamanho de bundle ao longo do tempo
    - Monitore regressões de desempenho de build

16. **Otimizações Específicas da Ferramenta**
    
    **Para Webpack:**
    - Configure optimization.splitChunks
    - Use thread-loader para processamento paralelo
    - Ative optimization.usedExports para tree shaking
    - Configure resolve.modules e resolve.extensions

    **Para Vite:**
    - Configure build.rollupOptions
    - Use esbuild para transpiração mais rápida
    - Otimize pré-bundling de dependências
    - Configure build.chunkSizeWarningLimit

    **Para TypeScript:**
    - Use compilação incremental
    - Configure referências de projeto
    - Otimize configurações de tsconfig.json
    - Use skipLibCheck quando apropriado

17. **Configuração Específica de Ambiente**
    - Separe configurações de desenvolvimento e produção
    - Use variáveis de ambiente para otimização de build
    - Configure feature flags para builds condicionais
    - Otimize para ambientes alvo

18. **Testando Otimizações de Build**
    - Teste saídas de build para correção
    - Verifique que todas as otimizações funcionam em ambientes alvo
    - Procure por mudanças quebradas resultantes de otimizações
    - Meça e documente melhorias de desempenho

19. **Documentação e Manutenção**
    - Documente todas as mudanças de otimização e seu impacto
    - Crie dashboard de monitoramento de desempenho de build
    - Configure alertas para regressões de desempenho de build
    - Revise e atualize regularmente a configuração de build

Foque nas otimizações que proporcionam maior impacto para seu projeto e fluxo de trabalho em equipe específico. Sempre meça antes e depois para quantificar melhorias.