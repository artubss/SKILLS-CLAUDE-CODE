---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [environment-type] | --development | --production | --microservices | --compose
description: Configurar containerização Docker com builds multi-estágio e workflows de desenvolvimento
---

# Configurar Contêineres Docker

Configurar containerização Docker abrangente para desenvolvimento e produção: **$ARGUMENTS**

## Estado Atual do Projeto

- Tipo de aplicação: @package.json ou @requirements.txt (detectar Node.js, Python, etc.)
- Docker existente: @Dockerfile ou @docker-compose.yml (se existir)
- Dependências: !`find . -name "package-lock.json" -o -name "poetry.lock" -o -name "Pipfile.lock" | wc -l`
- Serviços necessários: Detecção de banco de dados, cache, fila de mensagens a partir de configs

## Tarefa

Implementar containerização Docker pronta para produção com builds otimizados e workflows de desenvolvimento:

**Tipo de Ambiente**: Use $ARGUMENTS para especificar desenvolvimento, produção, microservices ou configuração Docker Compose

**Estratégia de Containerização**:
1. **Criação do Dockerfile** - Builds multi-estágio, otimização de camadas, melhores práticas de segurança
2. **Workflow de Desenvolvimento** - Hot reloading, volume mounts, capacidades de debug
3. **Otimização para Produção** - Redução de tamanho de imagem, scanning de vulnerabilidades, health checks
4. **Configuração Multi-Serviço** - Docker Compose, service discovery, configuração de networking
5. **Integração CI/CD** - Automação de build, gerenciamento de registry, pipelines de deployment
6. **Monitoramento & Logs** - Observabilidade de contêiner, agregação de logs, monitoramento de recursos

**Recursos de Segurança**: Usuários não-root, imagens base mínimas, scanning de vulnerabilidades, gerenciamento de secrets.

**Otimização de Performance**: Cache de camadas, contextos de build, builds multi-plataforma e constraints de recursos.

**Output**: Configuração Docker completa com contêineres otimizados, workflows de desenvolvimento, deployment para produção e documentação abrangente.