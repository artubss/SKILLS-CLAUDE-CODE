---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [cdn-provider] | --cloudflare | --aws | --fastly
description: Configure CDN para entrega de conteúdo otimizada, cache e otimização de desempenho global
---

# Otimização de CDN

Configure CDN para entrega otimizada: **$ARGUMENTS**

## Instruções

1. **Estratégia de CDN e Seleção de Provider**
   - Analise padrões de tráfego da aplicação e distribuição de usuários globais
   - Avalie providers de CDN com base em desempenho, custo e funcionalidades
   - Avalie tipos de conteúdo e requisitos específicos de cache
   - Planeje arquitetura de CDN e estratégia de localização de edge nodes
   - Defina metas de otimização de desempenho e custo

2. **Configuração e Instalação de CDN**
   - Configure CDN com configurações otimizadas para seus tipos de conteúdo
   - Configure servidores de origem e failover
   - Configure certificados SSL/TLS e configurações de segurança
   - Implemente domínio customizado e configuração de DNS
   - Configure rastreamento de monitoramento e analytics

3. **Otimização de Ativos Estáticos**
   - Otimize processo de build para entrega via CDN
   - Configure estratégias de content hashing e versionamento
   - Configure bundling de ativos e code splitting para CDN
   - Implemente entrega e otimização de imagens responsivas
   - Configure estratégias de carregamento e otimização de fontes

4. **Compressão e Otimização**
   - Configure configurações de compressão Gzip e Brotli
   - Configure compressão em tempo de build para ativos estáticos
   - Implemente compressão dinâmica para respostas de API
   - Configure minificação e otimização de ativos
   - Configure formatos de imagem progressiva (WebP, AVIF)

5. **Headers e Políticas de Cache**
   - Projete estratégias inteligentes de cache para diferentes tipos de conteúdo
   - Configure headers de cache control e valores de TTL
   - Implemente ETags e processamento de requisições condicionais
   - Configure hierarquia de cache e cache em múltiplas camadas
   - Configure estratégias de cache warming e preloading

6. **Otimização e Entrega de Imagens**
   - Implemente entrega de imagens responsivas com múltiplos formatos
   - Configure compressão automática e otimização de imagens
   - Configure lazy loading e carregamento progressivo de imagens
   - Implemente redimensionamento de imagens e conversão de formato
   - Configure suporte a formatos WebP e AVIF com fallbacks

7. **Purge de CDN e Invalidação de Cache**
   - Implemente estratégias inteligentes de invalidação de cache
   - Configure purge automatizado para pipelines de deployment
   - Configure purge seletivo por tags ou padrões
   - Implemente invalidação de cache em tempo real para conteúdo dinâmico
   - Configure monitoramento e alertas de invalidação de cache

8. **Monitoramento de Desempenho e Analytics**
   - Configure monitoramento de desempenho de CDN e rastreamento de métricas
   - Monitore cache hit ratios e uso de bandwidth
   - Rastreie tempos de resposta e taxas de erro entre regiões
   - Implemente monitoramento de usuários reais para desempenho de CDN
   - Configure alertas para degradação de desempenho

9. **Segurança e Controle de Acesso**
   - Configure headers de segurança de CDN e políticas
   - Implemente proteção contra hotlink e validação de referrer
   - Configure proteção contra DDoS e rate limiting
   - Configure geo-blocking e restrições de acesso
   - Implemente autenticação por token seguro para conteúdo protegido

10. **Otimização de Custo e Monitoramento**
    - Monitore uso de CDN e custos entre diferentes tiers
    - Implemente estratégias de otimização de custo para uso de bandwidth
    - Configure alertas automatizados de custo e monitoramento de budget
    - Analise padrões de uso para otimização de tier
    - Configure políticas de cache custo-eficazes

Foque em otimizações de CDN que proporcionem as melhorias de desempenho mais significativas para seus tipos de conteúdo e base de usuários específicos. Sempre meça o impacto de desempenho do CDN e ajuste as configurações com base em padrões de uso no mundo real.