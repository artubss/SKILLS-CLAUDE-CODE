---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [plataforma] | --docusaurus | --gitbook | --notion | --storybook | --jupyter | --comprehensive
description: Use PROATIVAMENTE para criar plataformas de documentação interativa com exemplos ao vivo, playgrounds de código e recursos de engajamento do usuário
---

# Plataforma de Documentação Interativa

Crie documentação interativa com exemplos ao vivo: $ARGUMENTS

## Infraestrutura de Documentação Atual

- Geradores de site estático: !`find . -name "docusaurus.config.js" -o -name "gatsby-config.js" -o -name "_config.yml" | head -3`
- Framework de documentação: @docs/ ou @website/ (detecta configuração existente)
- Bibliotecas de componentes: !`find . -name "*.stories.*" | head -5` (detecção de Storybook)
- Exemplos interativos: !`find . -name "*.ipynb" -o -name "*playground*" | head -3`
- Configuração de hosting: @vercel.json ou @netlify.toml ou @.github/workflows/ (se existir)

## Tarefa

Construir plataforma abrangente de documentação interativa com exemplos de código ao vivo, recursos de engajamento do usuário e capacidades de integração multi-plataforma.

## Arquitetura de Documentação Interativa

### 1. Fundação e Configuração da Plataforma
- Seleção da plataforma de documentação e otimização de configuração
- Personalização de tema e configuração de marca
- Estrutura de navegação e organização de conteúdo
- Suporte a múltiplos idiomas e internacionalização
- Integração de busca com filtros avançados e indexação

### 2. Integração de Playground de Código ao Vivo
- Editor de código interativo com destaque de sintaxe
- Execução de código em tempo real e capacidades de preview
- Suporte a múltiplos idiomas e integração de frameworks
- Tratamento de erros e assistência de debugging
- Compartilhamento de código e recursos de colaboração

### 3. Documentação e Teste de API
- Exploração interativa de endpoints de API
- Capacidades de teste de requisição/resposta ao vivo
- Validação de parâmetros e geração de exemplos
- Integração de fluxo de autenticação
- Visualização e validação de schema de resposta

### 4. Sistema Interativo de Tutorial
- Experiências de aprendizado guiado passo a passo
- Rastreamento de progresso e validação de conclusão
- Exercícios práticos de codificação com feedback instantâneo
- Caminhos de aprendizado adaptativos baseados no progresso do usuário
- Elementos de gamificação e sistemas de conquistas

### 5. Integração de Documentação de Componentes
- Playground de componentes ao vivo com controles de propriedades
- Galeria visual de componentes com exemplos interativos
- Integração de design system e geração de style guide
- Teste de acessibilidade e validação de conformidade
- Teste de compatibilidade entre navegadores

### 6. Sistemas de Engajamento e Feedback do Usuário
- Mecanismos de coleta de avaliações e comentários
- Agregação e análise de feedback do usuário
- Integração de discussão em comunidade e Q&A
- Analytics de uso e rastreamento de comportamento
- Sistemas de personalização e recomendação

### 7. Gerenciamento e Publicação de Conteúdo
- Integração com controle de versão e publicação automatizada
- Fluxos de revisão e aprovação de conteúdo
- Colaboração multi-autor e edição
- Agendamento de conteúdo e atualizações automatizadas
- Otimização de SEO e gerenciamento de metadados

### 8. Recursos Interativos Avançados
- Busca avançada com filtros facetados e sugestões
- Ferramentas de diagramas interativos e visualização
- Conteúdo de vídeo incorporado e integração multimídia
- Design responsivo e capacidades offline
- Recursos de progressive web app e notificações

## Requisitos de Implementação

### Integração de Plataforma
- Suporte a múltiplos frameworks (React, Vue, Angular, vanilla JS)
- Integração com sistema de build e deployment automatizado
- Compatibilidade com sistema de gerenciamento de conteúdo
- Integração de serviços terceirizados (analytics, feedback, busca)
- Otimização de performance e code splitting

### Design de Experiência do Usuário
- Design responsivo em todos os tipos de dispositivo
- Conformidade com acessibilidade (padrões WCAG 2.1 AA)
- Aprimoramento progressivo para degradação de recursos
- Tempos de carregamento rápidos e Core Web Vitals otimizados
- Navegação intuitiva e descoberta de conteúdo

### Infraestrutura Técnica
- Configuração de hosting escalável e CDN
- Integração de banco de dados para dados de usuário e analytics
- Design de API para integrações externas
- Implementação de segurança e autenticação de usuário
- Sistemas de monitoramento e rastreamento de erros

## Entregáveis

1. **Arquitetura da Plataforma Interativa**
   - Configuração e setup completo da plataforma de documentação
   - Integração de playground de código ao vivo e teste de API
   - Sistema de tutorial interativo com rastreamento de progresso
   - Documentação de componentes com exemplos visuais

2. **Sistemas de Engajamento do Usuário**
   - Mecanismos de coleta e análise de feedback
   - Implementação de analytics de usuário e rastreamento de comportamento
   - Integração de recursos de comunidade e discussão
   - Motores de personalização e recomendação

3. **Framework de Gerenciamento de Conteúdo**
   - Pipelines de publicação e deployment automatizados
   - Fluxos de colaboração multi-autor e revisão
   - Integração com controle de versão e rastreamento de mudanças
   - Otimização de SEO e gerenciamento de metadados

4. **Performance e Otimização**
   - Design responsivo com capacidades offline
   - Implementação de monitoramento e otimização de performance
   - Frameworks de conformidade e teste de acessibilidade
   - Recursos de progressive web app e service workers

## Diretrizes de Integração

Implemente com plataformas modernas de documentação e fluxos de trabalho de desenvolvimento. Garanta escalabilidade para repositórios de conteúdo grande e colaboração em equipe, mantendo performance ideal e experiência do usuário em todos os dispositivos e plataformas.