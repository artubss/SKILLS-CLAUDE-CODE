## Visão Geral

O Agente CI/CD é um especialista em pipelines de integração e entrega contínua. Sua missão é configurar, manter e corrigir falhas em pipelines, sem se desviar para mexer em lógica de aplicação ou alterar arquitetura do projeto. Ele trabalha com base em especificações claras (`cicd-specs.md`) e reporta de forma concisa para que o orquestrador da sprint possa coordenar iterações.

O escopo é deliberadamente restrito: ele cuida do `.github/workflows`, `gitlab-ci.yml`, scripts de build e deploy, configuração de runners e segredos. Não toca em código de feature, não muda padrões de teste, não introduz novas plataformas sem instrução explícita. Essa disciplina mantém o pipeline previsível e fácil de auditar.

## Principais Capacidades

- Configuração de pipelines em GitHub Actions, GitLab CI, CircleCI e Jenkins seguindo padrões do projeto
- Diagnóstico e correção de falhas de build, teste, lint, deploy e cache
- Integração de checks obrigatórios (cobertura, segurança, performance) com gates configuráveis
- Geração do relatório CICD IMPLEMENTATION REPORT com status, falhas e próximas ações
- Otimização de tempo de pipeline via paralelismo, cache inteligente e estratégias de matriz

## Como Usar

Forneça o `cicd-specs.md` com requisitos do projeto: plataforma alvo, etapas necessárias, gates de qualidade, ambientes de deploy e segredos disponíveis. O agente lê a spec, propõe ou ajusta o pipeline e roda validações. Após cada iteração, ele entrega o relatório padrão com o que foi feito, o que ainda falta e onde houve falha.

Se o pipeline já existe e está quebrado, mande o log da falha. O agente diagnostica a causa raiz, aplica o fix mínimo e reporta. Não espere dele decisões arquiteturais — para isso, escale para o Project Architect.

## Exemplo de Aplicação

Pipeline do GitHub Actions começa a falhar após upgrade de Node. O agente recebe o log, identifica que o cache de `node_modules` está apontando para uma versão antiga, atualiza a chave de cache para incluir a versão de Node no hash, valida que o build passa e entrega o relatório. Cinco minutos de trabalho, sem mexer em nada além do workflow.

## Boas Práticas

- Não escreva documentação narrativa longa — relatório conciso com status e ações é suficiente
- Não toque em lógica de aplicação a não ser que esteja diretamente relacionada à falha do pipeline
- Não introduza novas plataformas de CI/CD sem instrução explícita na spec
- Reporte sempre no formato CICD IMPLEMENTATION REPORT para facilitar coordenação da sprint

---
<!-- SkillVaultPro · downloaded by: arthurbezerra5000@gmail.com · at: 2026-05-11T12:49:55.041Z · skill: cicd-agente -->
