---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [nome-da-feature] | [tipo-da-feature] [nome]
description: Cria scaffolding de nova feature com código boilerplate, testes e documentação
---

# Criar Feature

Cria scaffolding de nova feature: $ARGUMENTS

## Contexto do Projeto Atual

- Estrutura do projeto: !`find . -maxdepth 2 -type d -name src -o -name components -o -name features | head -5`
- Branch atual: !`git branch --show-current`
- Informações do pacote: @package.json or @Cargo.toml or @requirements.txt (se existe)
- Docs de arquitetura: @docs/architecture.md or @README.md (se existe)

## Tarefa

Siga esta abordagem sistemática para criar uma nova feature: $ARGUMENTS

1. **Planejamento da Feature**
   - Defina os requisitos da feature e critérios de aceição
   - Divida a feature em tarefas menores e gerenciáveis
   - Identifique componentes afetados e áreas de impacto potencial
   - Planeje o design da API/interface antes da implementação

2. **Pesquisa e Análise**
   - Estude padrões e convenções da base de código existente
   - Identifique features similares para manter consistência
   - Pesquise dependências externas ou bibliotecas necessárias
   - Revise qualquer documentação ou especificação relevante

3. **Design da Arquitetura**
   - Projete a arquitetura da feature e fluxo de dados
   - Planeje mudanças no schema do banco de dados, se necessário
   - Defina endpoints e contratos de API
   - Considere implicações de escalabilidade e performance

4. **Configuração do Ambiente**
   - Crie uma nova branch de feature: `git checkout -b feature/$ARGUMENTS`
   - Garanta que o ambiente de desenvolvimento está atualizado
   - Instale novas dependências necessárias
   - Configure feature flags, se aplicável

5. **Estratégia de Implementação**
   - Comece com funcionalidade core e construa incrementalmente
   - Siga padrões e normas de código do projeto
   - Implemente tratamento de erros e validação adequada
   - Use injeção de dependência e mantenha baixo acoplamento

6. **Mudanças no Banco de Dados (se aplicável)**
   - Crie scripts de migração para mudanças de schema
   - Garanta compatibilidade retroativa
   - Planeje cenários de rollback
   - Teste migrações em dados de amostra

7. **Desenvolvimento da API**
   - Implemente endpoints com status HTTP apropriados
   - Adicione validação de requisição/resposta
   - Implemente autenticação e autorização adequadas
   - Documente contratos de API e exemplos

8. **Implementação Frontend (se aplicável)**
   - Crie componentes reutilizáveis seguindo padrões do projeto
   - Implemente design responsivo e acessibilidade
   - Adicione gerenciamento de estado adequado
   - Trate estados de carregamento e erro

9. **Implementação de Testes**
   - Escreva testes unitários para lógica de negócio core
   - Crie testes de integração para endpoints de API
   - Adicione testes end-to-end para workflows do usuário
   - Teste cenários de erro e casos extremos

10. **Considerações de Segurança**
    - Implemente validação e sanitização de entrada adequada
    - Adicione verificações de autorização para operações sensíveis
    - Revise por vulnerabilidades comuns de segurança
    - Garanta proteção de dados e conformidade com privacidade

11. **Otimização de Performance**
    - Otimize queries e índices do banco de dados
    - Implemente cache onde apropriado
    - Monitore uso de memória e otimize algoritmos
    - Considere lazy loading e paginação

12. **Documentação**
    - Adicione documentação inline e comentários de código
    - Atualize documentação de API
    - Crie documentação do usuário, se necessário
    - Atualize o README do projeto, se aplicável

13. **Preparação para Code Review**
    - Execute todos os testes e garanta que passam
    - Execute linting e ferramentas de formatação
    - Verifique cobertura de código e métricas de qualidade
    - Faça auto-review das mudanças

14. **Testes de Integração**
    - Teste integração da feature com funcionalidade existente
    - Verifique que feature flags funcionam corretamente
    - Teste procedimentos de deployment e rollback
    - Valide monitoramento e logging

15. **Commit e Push**
    - Crie commits atômicos com mensagens descritivas
    - Siga formato de conventional commit, se o projeto usar
    - Faça push da branch de feature: `git push origin feature/$ARGUMENTS`

16. **Criação de Pull Request**
    - Crie PR com descrição abrangente
    - Inclua screenshots ou demos, se aplicável
    - Adicione labels e revisores apropriados
    - Link para issues ou especificações relacionadas

17. **Garantia de Qualidade**
    - Coordene com time de QA para testes
    - Resolva bugs ou problemas encontrados
    - Verifique requisitos de acessibilidade e usabilidade
    - Teste em diferentes ambientes e navegadores

18. **Planejamento de Deployment**
    - Planeje estratégia de rollout da feature
    - Configure monitoramento e alertas
    - Prepare procedimentos de rollback
    - Agende deployment e comunicação

Lembre-se de manter qualidade de código, seguir convenções do projeto e priorizar experiência do usuário durante todo o processo de desenvolvimento.