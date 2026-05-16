---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [área-alvo] | --frontend | --backend | --database
description: Otimização abrangente de uso de memória com detecção de vazamentos, ajuste de coleta de lixo e profiling de memória
---

# Otimizar Uso de Memória

Analise e otimize padrões de uso de memória para evitar vazamentos e melhorar o desempenho da aplicação: **$ARGUMENTS**

## Instruções

1. **Análise e Profiling de Memória**
   - Faça profiling dos padrões atuais de uso de memória usando ferramentas apropriadas (Chrome DevTools, Node.js --inspect, Valgrind)
   - Identifique vazamentos de memória e hotspots de consumo excessivo
   - Analise padrões de coleta de lixo e seu impacto no desempenho
   - Crie medições de linha de base para rastreamento de otimizações
   - Documente hotspots de alocação de memória e padrões de crescimento ao longo do tempo

2. **Detecção de Vazamentos de Memória**
   - Configure detecção de vazamentos de memória para diferentes ambientes de runtime
   - Monitore snapshots de heap e compare em intervalos de tempo
   - Rastreie vazamentos de nós DOM em aplicações browser
   - Implemente limpeza e monitoramento de event listeners
   - Use ferramentas de profiling para identificar padrões de crescimento de memória

3. **Otimização de Coleta de Lixo**
   - Configure as definições de coleta de lixo para seu ambiente de runtime
   - Ajuste tamanhos de heap e flags de GC do Node.js para desempenho ideal
   - Monitore tempos de pausa e frequência de GC
   - Implemente monitoramento e alertas de desempenho de GC
   - Otimize ciclos de vida de objetos para reduzir pressão de GC

4. **Pool de Memória e Reutilização de Objetos**
   - Implemente object pooling para objetos alocados frequentemente
   - Crie pools de buffer para aplicações Node.js
   - Reutilize elementos DOM e componentes em aplicações frontend
   - Projete estruturas de dados eficientes em memória (buffers circulares, arrays esparsos)
   - Pré-aloque objetos para reduzir overhead de alocação em runtime

5. **Otimização de Strings e Texto**
   - Implemente string interning para strings frequentemente usadas
   - Otimize operações de concatenação e manipulação de strings
   - Use algoritmos eficientes de processamento de texto
   - Minimize duplicação de strings na aplicação
   - Considere compressão de strings para dados de texto grandes

6. **Otimização de Conexão com Banco de Dados**
   - Implemente proper connection pooling com limites apropriados
   - Configure timeouts de conexão e procedimentos de limpeza
   - Otimize cache de resultados de query e uso de memória
   - Monitore overhead de memória de conexão com banco de dados
   - Implemente detecção e prevenção de vazamento de conexões

7. **Otimização de Memória no Frontend**
   - Otimize lifecycle e limpeza de componentes
   - Implemente limpeza apropriada de event listeners
   - Use lazy loading para imagens e componentes
   - Minimize tamanho de bundle e implemente code splitting
   - Monitore e otimize padrões de uso de memória do browser

8. **Otimização de Memória no Backend**
   - Otimize manipulação e limpeza de requisições do servidor
   - Implemente streaming para processamento de dados grandes
   - Configure limites e monitoramento apropriados de memória
   - Otimize middleware e lifecycle de requisição
   - Use padrões eficientes de processamento de dados em memória

9. **Otimização de Container e Deploy**
   - Configure limites apropriados de memória de container
   - Otimize camadas de imagem Docker para eficiência de memória
   - Monitore uso de memória em ambientes de produção
   - Implemente políticas de auto-scaling baseadas em memória
   - Configure monitoramento e alertas de uso de memória

10. **Monitoramento e Alertas de Memória**
    - Configure dashboards de monitoramento de memória em tempo real
    - Configure alertas e limites de uso de memória
    - Implemente detecção de vazamento de memória em produção
    - Rastreie métricas de desempenho de memória ao longo do tempo
    - Crie testes automatizados de otimização de memória

11. **Gerenciamento de Memória em Produção**
    - Implemente tratamento gracioso de pressão de memória
    - Configure health checks baseados em memória
    - Configure trending e análise de uso de memória
    - Implemente procedimentos de limpeza emergencial de memória
    - Monitore e otimize padrões de uso de memória

Foque nas estratégias específicas de otimização de memória que trazem o maior impacto para seu ambiente alvo. Sempre meça o uso de memória antes e depois das otimizações para quantificar as melhorias.