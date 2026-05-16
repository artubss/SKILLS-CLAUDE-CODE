---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [cache-type] | --browser | --application | --database
description: Projete e implemente soluções abrangentes de cache para melhor performance e escalabilidade
---

# Implementar Estratégia de Cache

Projete e implemente soluções de cache: **$ARGUMENTS**

## Instruções

1. **Análise de Estratégia de Cache**
   - Analise a arquitetura da aplicação e identifique oportunidades de cache
   - Avalie os gargalos de performance atuais e padrões de acesso a dados
   - Defina requisitos de cache (TTL, invalidação, consistência)
   - Planeje uma arquitetura de cache em múltiplas camadas (browser, CDN, aplicação, banco de dados)
   - Avalie tecnologias de cache e soluções de armazenamento

2. **Cache no Browser e Lado do Cliente**
   - Configure headers de cache HTTP e políticas de cache para ativos estáticos
   - Implemente estratégias de cache com service worker para progressive web apps
   - Configure cache de armazenamento do browser (localStorage, sessionStorage, IndexedDB)
   - Configure regras de cache CDN e otimização de edge
   - Implemente estratégias cache-first, network-first e stale-while-revalidate

3. **Cache em Nível de Aplicação**
   - Implemente cache em memória para dados acessados frequentemente
   - Configure cache distribuído com Redis ou Memcached
   - Projete convenções de nomenclatura de chaves de cache e namespacing
   - Implemente estratégias de aquecimento de cache para dados críticos
   - Configure políticas de expiração de cache e TTL

4. **Cache de Queries de Banco de Dados**
   - Implemente cache de resultados de queries para operações custosas de banco de dados
   - Configure cache de prepared statements e connection pooling
   - Projete estratégias de invalidação de cache para consistência de dados
   - Implemente materialized views para agregações complexas
   - Configure recursos de cache em nível de banco de dados e otimizações

5. **Cache de Respostas de API**
   - Implemente cache de resposta de endpoints de API com headers apropriados
   - Configure middleware para cache automático de respostas
   - Configure cache de queries GraphQL e otimização em nível de campo
   - Implemente requisições condicionais com headers ETag e Last-Modified
   - Projete invalidação de cache para atualizações de dados de API

6. **Estratégias de Invalidação de Cache**
   - Projete invalidação inteligente de cache baseada em dependências de dados
   - Implemente sistemas de invalidação de cache orientados por eventos
   - Configure tagging de cache e mecanismos de invalidação em massa
   - Configure políticas de invalidação baseadas em tempo e triggers
   - Implemente versionamento de cache e estratégias de rollback

7. **Estratégias de Cache no Frontend**
   - Implemente cache de dados no lado do cliente com bibliotecas como React Query
   - Configure cache em nível de componente e memoização
   - Configure estratégias de bundling de ativos e cache de chunks
   - Implemente carregamento progressivo de imagens e cache
   - Configure cache offline-first para PWAs

8. **Monitoramento e Análise de Cache**
   - Configure monitoramento de performance de cache e coleta de métricas
   - Rastreie taxas de acerto, taxas de erro e métricas de eficiência de cache
   - Monitore uso de memória de cache e otimização de armazenamento
   - Implemente alertas e notificações de performance de cache
   - Analise padrões de uso de cache e oportunidades de otimização

9. **Aquecimento e Pré-carregamento de Cache**
   - Implemente aquecimento automatizado de cache para dados críticos
   - Configure estratégias de refresh agendado e pré-carregamento de cache
   - Projete geração de cache sob demanda para conteúdo popular
   - Configure triggers de aquecimento de cache baseados em padrões de uso
   - Implemente cache preditivo baseado em comportamento do usuário

10. **Testes e Validação**
    - Configure testes de performance de cache e benchmarking
    - Implemente validação e testes de consistência de cache
    - Configure cenários de teste de invalidação de cache
    - Teste comportamento de cache sob alta carga e condições de falha
    - Valide segurança de cache e requisitos de isolamento de dados

Foque em implementar estratégias de cache que proporcionem as melhorias de performance mais significativas mantendo consistência de dados e confiabilidade do sistema. Sempre meça a efetividade do cache e ajuste estratégias baseado em padrões de uso no mundo real.