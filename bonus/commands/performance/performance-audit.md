---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [área-alvo] | --frontend | --backend | --full
description: Auditoria de desempenho abrangente com métricas, identificação de gargalos e recomendações de otimização
---

# Auditoria de Desempenho

Conduza auditoria de desempenho abrangente: $ARGUMENTS

## Contexto de Desempenho Atual

- Análise de bundle: !`npm run build -- --analyze 2>/dev/null || echo "No build analyzer"`
- Dependências: !`npm list --depth=0 --prod 2>/dev/null | head -10`
- Tempo de build: !`time npm run build >/dev/null 2>&1 || echo "No build script"`
- Configuração de desempenho: @webpack.config.js ou @vite.config.js ou @next.config.js (se existir)

## Tarefa

Conduza auditoria de desempenho abrangente seguindo estas etapas:

1. **Análise de Stack de Tecnologia**
   - Identifique a linguagem primária, framework e ambiente de runtime
   - Revise ferramentas de build e configurações de otimização
   - Verifique ferramentas de monitoramento de desempenho já implementadas

2. **Análise de Desempenho de Código**
   - Identifique algoritmos ineficientes e estruturas de dados
   - Procure por loops aninhados e operações O(n²)
   - Verifique computações desnecessárias e operações redundantes
   - Revise padrões de alocação de memória e vazamentos potenciais

3. **Desempenho de Banco de Dados**
   - Analise queries de banco de dados quanto à eficiência
   - Verifique índices ausentes e queries lentas
   - Revise connection pooling e configuração de banco de dados
   - Identifique problemas N+1 e chamadas excessivas ao banco

4. **Desempenho de Frontend (se aplicável)**
   - Analise tamanho de bundle e otimização de chunks
   - Verifique código não utilizado e dependências
   - Revise otimização de imagens e lazy loading
   - Examine desempenho de renderização e ciclos de re-renderização
   - Verifique vazamentos de memória em componentes UI

5. **Desempenho de Rede**
   - Revise padrões de chamadas de API e estratégias de cache
   - Verifique requisições de rede desnecessárias
   - Analise tamanhos de payload e compressão
   - Examine uso de CDN e otimização de assets estáticos

6. **Operações Assíncronas**
   - Revise uso de async/await e manipulação de promises
   - Verifique operações bloqueantes e race conditions
   - Analise fila de tarefas e processamento em background
   - Identifique oportunidades para execução paralela

7. **Uso de Memória**
   - Verifique vazamentos de memória e consumo excessivo
   - Revise padrões de garbage collection
   - Analise ciclo de vida de objetos e limpeza
   - Identifique objetos grandes e retenção de dados desnecessária

8. **Desempenho de Build e Deployment**
   - Analise tempos de build e oportunidades de otimização
   - Revise bundling de dependências e tree shaking
   - Verifique otimizações de desenvolvimento vs produção
   - Examine eficiência do pipeline de deployment

9. **Monitoramento de Desempenho**
   - Verifique métricas de desempenho existentes e monitoramento
   - Identifique indicadores-chave de desempenho (KPIs) a rastrear
   - Revise alertas e limites de desempenho
   - Sugira estratégias de testes de desempenho

10. **Benchmarking e Profiling**
    - Execute ferramentas de profiling de desempenho apropriadas para o stack
    - Crie benchmarks para caminhos críticos de código
    - Meça impacto de otimizações antes e depois
    - Documente baselines de desempenho

11. **Recomendações de Otimização**
    - Priorize otimizações por impacto e esforço
    - Forneça exemplos de código específicos e alternativas
    - Sugira melhorias arquiteturais para escalabilidade
    - Recomende ferramentas e bibliotecas de desempenho apropriadas

Inclua caminhos de arquivo específicos, números de linha e métricas mensuráveis sempre que possível. Concentre-se em otimizações de alto impacto e baixo esforço em primeiro lugar.