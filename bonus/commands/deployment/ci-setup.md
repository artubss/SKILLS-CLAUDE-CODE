---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [platform] | --github-actions | --gitlab-ci | --jenkins | --full-setup
description: Configurar pipeline CI/CD abrangente com testes automatizados, build e deployment
---

# Configuração de Pipeline CI/CD

Configurar pipeline de integração contínua: $ARGUMENTS

## Análise do Projeto Atual

- Tipo de projeto: @package.json or @setup.py or @go.mod or @pom.xml (detectar linguagem/framework)
- Workflows existentes: !`find .github/workflows -name "*.yml" 2>/dev/null | head -3`
- Branches Git: !`git branch -r | head -5`
- Dependências: @package-lock.json or @requirements.txt or @go.sum (se existir)
- Scripts de build: Verificar comandos de build em package.json ou Makefile

## Tarefa

Implementar CI/CD abrangente seguindo melhores práticas: $ARGUMENTS

1. **Análise do Projeto**
   - Identificar stack de tecnologia e requisitos de deployment
   - Revisar processos de build e testes existentes
   - Entender ambientes de deployment (dev, staging, prod)
   - Avaliar estratégia de controle de versão e branching atual

2. **Seleção da Plataforma CI/CD**
   - Escolher plataforma CI/CD apropriada conforme requisitos:
     - **GitHub Actions**: Integração nativa com GitHub, marketplace extenso
     - **GitLab CI**: Integrado ao GitLab, plataforma DevOps abrangente
     - **Jenkins**: Auto-hospedado, altamente customizável, plugins extensos
     - **CircleCI**: Em nuvem, otimizado para velocidade
     - **Azure DevOps**: Integração com ecossistema Microsoft
     - **AWS CodePipeline**: Solução nativa AWS

3. **Configuração do Repositório**
   - Garantir configuração apropriada de `.gitignore`
   - Configurar regras de proteção de branch
   - Definir requisitos de merge e reviews
   - Estabelecer estratégia de versionamento semântico

4. **Configuração do Pipeline de Build**
   
   **Exemplo GitHub Actions:**
   ```yaml
   name: CI/CD Pipeline
   
   on:
     push:
       branches: [ main, develop ]
     pull_request:
       branches: [ main ]
   
   jobs:
     test:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - name: Setup Node.js
           uses: actions/setup-node@v3
           with:
             node-version: '18'
             cache: 'npm'
         - run: npm ci
         - run: npm run test
         - run: npm run build
   ```

   **Exemplo GitLab CI:**
   ```yaml
   stages:
     - test
     - build
     - deploy
   
   test:
     stage: test
     script:
       - npm ci
       - npm run test
     cache:
       paths:
         - node_modules/
   ```

5. **Configuração de Ambientes**
   - Configurar variáveis de ambiente e secrets
   - Configurar diferentes ambientes (dev, staging, prod)
   - Implementar configurações específicas por ambiente
   - Configurar gerenciamento seguro de secrets

6. **Integração de Testes Automatizados**
   - Configurar execução de testes unitários
   - Configurar execução de testes de integração
   - Implementar execução de testes E2E
   - Configurar relatórios de testes e cobertura

   **Testes Multi-estágio:**
   ```yaml
   test:
     strategy:
       matrix:
         node-version: [16, 18, 20]
     runs-on: ubuntu-latest
     steps:
       - uses: actions/checkout@v3
       - uses: actions/setup-node@v3
         with:
           node-version: ${{ matrix.node-version }}
       - run: npm ci
       - run: npm test
   ```

7. **Gates de Qualidade de Código**
   - Integrar verificações de linting e formatação
   - Configurar análise estática de código (SonarQube, CodeClimate)
   - Configurar scanning de vulnerabilidades de segurança
   - Implementar limites de cobertura de testes

8. **Otimização de Build**
   - Configurar estratégias de cache de build
   - Implementar execução paralela de jobs
   - Otimizar builds de imagens Docker
   - Configurar gerenciamento de artifacts

   **Exemplo de Caching:**
   ```yaml
   - name: Cache node modules
     uses: actions/cache@v3
     with:
       path: ~/.npm
       key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
       restore-keys: |
         ${{ runner.os }}-node-
   ```

9. **Integração Docker**
   - Criar Dockerfiles otimizados
   - Configurar builds multi-stage
   - Configurar integração com registro de containers
   - Implementar scanning de segurança para imagens

   **Dockerfile Multi-stage:**
   ```dockerfile
   FROM node:18-alpine AS builder
   WORKDIR /app
   COPY package*.json ./
   RUN npm ci --only=production
   
   FROM node:18-alpine AS runtime
   WORKDIR /app
   COPY --from=builder /app/node_modules ./node_modules
   COPY . .
   EXPOSE 3000
   CMD ["npm", "start"]
   ```

10. **Estratégias de Deployment**
    - Implementar deployment blue-green
    - Configurar canary releases
    - Configurar atualizações rolling
    - Implementar integração com feature flags

11. **Infrastructure as Code**
    - Usar Terraform, CloudFormation ou ferramentas similares
    - Manter definições de infraestrutura em controle de versão
    - Implementar testes de infraestrutura
    - Configurar provisionamento automatizado de infraestrutura

12. **Monitoramento e Observabilidade**
    - Configurar monitoramento de performance de aplicação
    - Configurar agregação e análise de logs
    - Implementar health checks e alertas
    - Configurar notificações de deployment

13. **Integração de Segurança**
    - Implementar scanning de vulnerabilidades de dependências
    - Configurar scanning de segurança de containers
    - Configurar SAST (Static Application Security Testing)
    - Implementar scanning de secrets

   **Exemplo de Security Scanning:**
   ```yaml
   security:
     runs-on: ubuntu-latest
     steps:
       - uses: actions/checkout@v3
       - name: Run Snyk to check for vulnerabilities
         uses: snyk/actions/node@master
         env:
           SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
   ```

14. **Tratamento de Migrações de Banco de Dados**
    - Automatizar migrações de schema de banco de dados
    - Implementar estratégias de rollback
    - Configurar seeding de banco de dados para testes
    - Configurar procedimentos de backup e recuperação

15. **Integração de Testes de Performance**
    - Configurar load testing no pipeline
    - Configurar benchmarks de performance
    - Implementar detecção de regressão de performance
    - Configurar monitoramento de performance

16. **Deployment Multi-Ambiente**
    - Configurar deployment em ambiente de staging
    - Configurar deployment em produção com aprovações
    - Implementar workflow de promoção de ambiente
    - Configurar configurações específicas por ambiente

   **Environment Deployment:**
   ```yaml
   deploy-staging:
     needs: test
     if: github.ref == 'refs/heads/develop'
     runs-on: ubuntu-latest
     steps:
       - name: Deploy to staging
         run: |
           # Deploy para ambiente de staging
   
   deploy-production:
     needs: test
     if: github.ref == 'refs/heads/main'
     runs-on: ubuntu-latest
     environment: production
     steps:
       - name: Deploy to production
         run: |
           # Deploy para ambiente de produção
   ```

17. **Rollback e Recuperação**
    - Implementar procedimentos de rollback automatizado
    - Configurar testes de verificação de deployment
    - Configurar detecção de falhas e alertas
    - Documentar procedimentos de recuperação manual

18. **Notificação e Relatórios**
    - Configurar integração com Slack/Teams para notificações
    - Configurar alertas por email para falhas
    - Implementar relatórios de status de deployment
    - Configurar dashboards de métricas

19. **Conformidade e Auditoria**
    - Implementar rastreamento de auditoria de deployment
    - Configurar verificações de conformidade (SOC 2, HIPAA, etc.)
    - Configurar workflows de aprovação para deployments sensíveis
    - Documentar processos de gerenciamento de mudanças

20. **Otimização de Pipeline**
    - Monitorar performance e custos do pipeline
    - Implementar paralelização de pipeline
    - Otimizar alocação de recursos
    - Configurar analytics e relatórios de pipeline

**Melhores Práticas:**

1. **Falhar Rápido**: Implementar detecção de falhas antecipada
2. **Execução Paralela**: Executar jobs independentes em paralelo
3. **Caching**: Cache de dependências e artifacts de build
4. **Segurança**: Nunca expor secrets em logs
5. **Documentação**: Documentar processos e procedimentos do pipeline
6. **Monitoramento**: Monitorar saúde e performance do pipeline
7. **Testes**: Testar mudanças de pipeline em branches de feature
8. **Rollback**: Sempre ter uma estratégia de rollback

**Pipeline Completo Exemplo:**
```yaml
name: Full CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run test:coverage
      - run: npm run build

  security-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Security scan
        run: npm audit --audit-level=high

  deploy-staging:
    needs: [lint-and-test, security-scan]
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to staging
        run: echo "Deploying to staging"

  deploy-production:
    needs: [lint-and-test, security-scan]
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to production
        run: echo "Deploying to production"
```

Comece com CI básico e adicione gradualmente recursos mais sofisticados conforme seu time e projeto amadurecem.