---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [application-type] | --node | --python | --java | --go | --multi-stage
description: Containerizar aplicação com configuração Docker otimizada, segurança e builds em múltiplos estágios
---

# Containerização de Aplicação

Containerizar aplicação para deploy: $ARGUMENTS

## Análise Atual da Aplicação

- Tipo de aplicação: @package.json ou @setup.py ou @go.mod ou @pom.xml (detectar runtime)
- Docker existente: @Dockerfile ou @docker-compose.yml (se existir)
- Dependências: !`find . -name "*requirements*.txt" -o -name "package*.json" -o -name "go.mod" | head -3`
- Configuração de porta: !`grep -r "PORT\|listen\|bind" src/ 2>/dev/null | head -3 || echo "Port detection needed"`
- Ferramentas de build: @Makefile ou detecção de scripts de build

## Tarefa

Implementar estratégia de containerização pronta para produção:

1. **Análise de Aplicação e Estratégia de Containerização**
   - Analisar arquitetura da aplicação e requisitos de runtime
   - Identificar dependências da aplicação e serviços externos
   - Determinar image base ótima e ambiente de runtime
   - Planejar estratégia de build em múltiplos estágios para otimização
   - Avaliar requisitos de segurança e conformidade

2. **Criação e Otimização do Dockerfile**
   - Criar Dockerfile abrangente com builds em múltiplos estágios
   - Selecionar images base mínimas (Alpine, distroless ou variantes slim)
   - Configurar cache de camadas apropriado e otimização de build
   - Implementar melhores práticas de segurança (usuário não-root, superfície mínima de ataque)
   - Configurar permissões de arquivo e propriedade apropriadas

3. **Configuração do Processo de Build**
   - Configurar arquivo .dockerignore para excluir arquivos desnecessários
   - Configurar argumentos de build e variáveis de ambiente
   - Implementar instalação de dependências em tempo de build e limpeza
   - Configurar bundling de aplicação e otimização de assets
   - Configurar contexto de build apropriado e estrutura de arquivos

4. **Configuração de Runtime**
   - Configurar inicialização de aplicação e health checks
   - Configurar tratamento apropriado de sinais e shutdown gracioso
   - Configurar logging e redirecionamento de saída
   - Configurar gerenciamento de configuração específico por ambiente
   - Configurar limites de recursos e otimização de desempenho

5. **Endurecimento de Segurança**
   - Executar aplicação como usuário não-root com privilégios mínimos
   - Configurar scanning de segurança e avaliação de vulnerabilidades
   - Implementar gerenciamento de secrets e tratamento seguro de credenciais
   - Configurar segurança de rede e regras de firewall
   - Configurar políticas de segurança e controles de acesso

6. **Configuração do Docker Compose**
   - Criar docker-compose.yml para desenvolvimento local
   - Configurar dependências de serviço e networking
   - Configurar montagem de volumes e persistência de dados
   - Configurar variáveis de ambiente e secrets
   - Configurar ambientes desenvolvimento versus produção

7. **Preparação de Orquestração de Container**
   - Preparar configurações para deploy em Kubernetes
   - Criar manifestos de deployment e definições de service
   - Configurar ingress e load balancing
   - Configurar persistent volumes e storage classes
   - Configurar auto-scaling e gerenciamento de recursos

8. **Monitoramento e Observabilidade**
   - Configurar métricas de aplicação e endpoints de health
   - Configurar agregação de logs e logging centralizado
   - Configurar tracing distribuído e monitoramento
   - Configurar alertas e sistemas de notificação
   - Configurar monitoramento de desempenho e profiling

9. **Integração CI/CD**
   - Configurar build automatizado de imagem Docker
   - Configurar scanning de imagem e validação de segurança
   - Configurar registry de imagem e gerenciamento de artefatos
   - Configurar pipelines de deploy automatizado
   - Configurar estratégias de rollback e blue-green deployment

10. **Testes e Validação**
    - Testar builds de container e funcionalidade
    - Validar configurações de segurança e conformidade
    - Testar deploy em diferentes ambientes
    - Validar desempenho e utilização de recursos
    - Testar procedimentos de backup e recuperação de desastres
    - Criar documentação para deploy e gerenciamento de container