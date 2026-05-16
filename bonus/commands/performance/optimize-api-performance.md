---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [api-type] | --rest | --graphql | --grpc
description: Otimização abrangente de performance de API com redução de tempo de resposta, melhoria de throughput e aprimoramentos de escalabilidade
---

# Otimizar Performance de API

Analise e otimize a performance de API para tempos de resposta mais rápidos, maior throughput e melhor escalabilidade: **$ARGUMENTS**

## Instruções

1. **Análise de Performance de API**
   - Analise métricas de tempo de resposta e throughput atuais da API
   - Identifique endpoints mais lentos e padrões de gargalo
   - Profile o ciclo de vida da requisição/resposta da API e tempo de processamento
   - Documente métricas de performance baseline em diferentes cenários de carga
   - Mapeie cadeias de dependência de API e chamadas de serviços externos

2. **Otimização de Requisição/Resposta**
   - Otimize a lógica de parsing e validação de requisição
   - Implemente serialização de resposta eficiente e compressão
   - Minimize tamanhos de payload através de inclusão seletiva de campos
   - Configure headers HTTP apropriados e diretivas de cache
   - Otimize roteamento de requisição e processamento de middleware

3. **Otimização de Query de Banco de Dados**
   - Identifique e otimize queries de banco de dados lentas
   - Implemente estratégias de cache de resultados de query
   - Adicione índices apropriados de banco de dados para queries de API
   - Otimize pooling e gerenciamento de conexão de banco de dados
   - Implemente batching de query e agregação quando aplicável

4. **Implementação de Estratégia de Cache**
   - Implemente cache multi-nível (in-memory, Redis, CDN)
   - Configure estratégias de invalidação de cache
   - Configure cache de resposta de API com valores de TTL apropriados
   - Implemente estratégias de aquecimento e pré-carregamento de cache
   - Monitore razões de acerto de cache e efetividade

5. **Rate Limiting e Throttling**
   - Implemente rate limiting inteligente baseado em padrões de uso
   - Configure throttling adaptativo para diferentes tiers de usuário
   - Configure gerenciamento de fila para lidar com picos de tráfego
   - Implemente padrões de circuit breaker para serviços externos
   - Monitore e ajuste rate limits baseado em métricas de performance

6. **Concorrência e Paralelização**
   - Implemente padrões apropriados de async/await para operações de I/O
   - Otimize configuração e gerenciamento de thread pool
   - Implemente processamento paralelo para operações independentes
   - Configure pooling de conexão para concorrência ótima
   - Use streaming para transferências de dados grandes

7. **Gateway de API e Load Balancing**
   - Configure API gateway para roteamento ótimo e distribuição de carga
   - Implemente health checks e failover automático
   - Configure algoritmos de load balancing para distribuição equilibrada de tráfego
   - Configure transformação de requisição/resposta no nível de gateway
   - Implemente versionamento de API e traffic splitting

8. **Monitoramento e Observabilidade**
   - Configure monitoramento abrangente de performance de API
   - Implemente distributed tracing para visibilidade do ciclo de vida de requisição
   - Configure coleta de métricas de performance e alerting
   - Monitore taxas de erro de API e percentis de tempo de resposta
   - Configure dashboards de performance em tempo real

9. **Otimização de Performance de Segurança**
   - Otimize processos de autenticação e autorização
   - Implemente validação eficiente de JWT e cache
   - Configure terminação de SSL/TLS para performance ótima
   - Otimize validação de API key e rate limiting
   - Implemente tuning de performance de middleware de segurança

10. **Otimização de Entrega de Conteúdo**
    - Configure CDN para respostas de API estáticas e assets
    - Implemente load balancing geográfico e edge caching
    - Otimize distribuição geográfica de endpoints de API
    - Configure compressão de conteúdo e otimização
    - Configure headers de cache para performance ótima de CDN

11. **Otimização de Design de API**
    - Revise e otimize padrões de design de endpoint de API
    - Implemente estratégias eficientes de paginação e filtragem
    - Otimize versionamento de API e compatibilidade retroativa
    - Projete APIs para cache ótimo no lado do cliente
    - Implemente otimização de query GraphQL (se aplicável)

12. **Load Testing e Validação de Performance**
    - Implemente cenários de load testing abrangentes
    - Configure performance regression testing em CI/CD
    - Configure testes de chaos engineering para validação de resilência
    - Monitore performance de API sob várias condições de carga
    - Valide otimizações de performance com dados de teste realistas

13. **Planejamento de Escalabilidade**
    - Projete arquitetura de API para escalabilidade horizontal
    - Implemente políticas de auto-scaling baseadas em métricas de performance
    - Configure estratégias de escalabilidade de banco de dados (read replicas, sharding)
    - Planeje crescimento de tráfego e requisitos de capacidade
    - Implemente estratégias de degradação graciosa

14. **Otimização de Serviço de Terceiro**
    - Otimize chamadas de API externa e integrações
    - Implemente políticas de retry e exponential backoff
    - Configure timeouts para serviços externos
    - Configure mecanismos de fallback para indisponibilidade de serviço
    - Monitore impacto de performance de serviço de terceiro

15. **Automação de Performance Testing**
    - Configure pipelines de performance testing automatizados
    - Configure benchmarking de performance e comparação
    - Implemente detecção de regressão de performance
    - Configure load testing em ambientes de staging
    - Crie estratégias de gerenciamento de dados de teste de performance

Foque em otimizações que proporcionem o maior impacto em tempos de resposta e throughput. Priorize mudanças que melhorem a experiência do usuário e escalabilidade do sistema mantendo confiabilidade.