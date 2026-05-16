---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [migration-type] | framework | database | cloud | architecture | --version-upgrade
description: Crie guias de migração abrangentes com procedimentos passo a passo, validação e estratégias de reversão
---

# Gerador de Guia de Migração

Crie guia de migração abrangente: $ARGUMENTS

## Análise do Sistema Atual

- Versões atuais: @package.json ou @requirements.txt ou detectar de arquivos de lock
- Histórico de migração: !`find . -name "*migration*" -o -name "*upgrade*" | head -5`
- Schema do banco de dados: !`find . -name "*schema*" -o -name "*.sql" | head -3`
- Dependências: !`grep -c "dependency\|require\|import" package.json requirements.txt 2>/dev/null || echo "0"`
- Infraestrutura: @docker-compose.yml ou @k8s/ ou @terraform/ (se existir)

## Tarefa

Gere guia de migração sistemático com medidas de segurança abrangentes: $ARGUMENTS

1. **Análise do Escopo da Migração**
   - Identifique o que está sendo migrado (framework, biblioteca, arquitetura, etc.)
   - Determine as versões ou tecnologias de origem e destino
   - Avalie a escala e complexidade da migração
   - Identifique os sistemas e componentes afetados

2. **Avaliação de Impacto**
   - Analise mudanças significativas entre versões
   - Identifique recursos e APIs deprecados
   - Revise novos recursos e capacidades
   - Avalie requisitos e restrições de compatibilidade
   - Avalie implicações de desempenho e segurança

3. **Pré-requisitos e Requisitos**
   - Documente requisitos do sistema para a versão alvo
   - Liste ferramentas necessárias e dependências
   - Especifique versões mínimas e requisitos de compatibilidade
   - Identifique habilidades necessárias e preparação do time
   - Descreva necessidades de infraestrutura e ambiente

4. **Preparação Pré-Migração**
   - Crie estratégias abrangentes de backup
   - Configure ambientes de desenvolvimento e teste
   - Documente o estado atual do sistema e configurações
   - Estabeleça procedimentos de reversão e planos de contingência
   - Crie timeline de migração e marcos

5. **Processo de Migração Passo a Passo**
   
   **Exemplo para Atualização de Framework:**
   ```markdown
   ## Etapa 1: Configuração do Ambiente
   1. Atualize o ambiente de desenvolvimento
   2. Instale a nova versão do framework
   3. Atualize ferramentas de build e dependências
   4. Configure IDE e ferramentas
   
   ## Etapa 2: Atualização de Dependências
   1. Atualize package.json/requirements.txt
   2. Resolva conflitos de dependência
   3. Atualize bibliotecas relacionadas
   4. Teste compatibilidade
   
   ## Etapa 3: Migração de Código
   1. Atualize instruções de import
   2. Substitua APIs deprecadas
   3. Atualize arquivos de configuração
   4. Modifique scripts de build
   ```

6. **Documentação de Mudanças Significativas**
   - Liste todas as mudanças significativas com exemplos
   - Forneça comparações de código antes/depois
   - Explique a razão por trás das mudanças
   - Oferça abordagens alternativas para recursos removidos

   **Exemplo de Mudança Significativa:**
   ```markdown
   ### Removido: `oldMethod()`
   **Antes:**
   ```javascript
   const result = library.oldMethod(param1, param2);
   ```
   
   **Depois:**
   ```javascript
   const result = library.newMethod({ 
     param1: param1, 
     param2: param2 
   });
   ```
   
   **Justificativa:** Melhor segurança de tipos e extensibilidade
   ```

7. **Mudanças de Configuração**
   - Documente atualizações de arquivos de configuração
   - Explique novas opções de configuração
   - Forneça scripts de migração de configuração
   - Mostre configurações específicas de ambiente

8. **Migração de Banco de Dados (se aplicável)**
   - Crie scripts de migração de schema do banco
   - Documente requisitos de transformação de dados
   - Forneça procedimentos de backup e restauração
   - Teste migração com dados de amostra
   - Planeje migrações sem tempo de inatividade

9. **Estratégia de Testes**
   - Atualize testes existentes para novas APIs
   - Crie casos de teste específicos da migração
   - Implemente testes de integração e E2E
   - Configure testes de desempenho e carga
   - Documente cenários de teste e resultados esperados

10. **Considerações de Desempenho**
    - Documente mudanças de desempenho e otimizações
    - Forneça diretrizes de benchmarking
    - Identifique possíveis regressões de desempenho
    - Sugira atualizações de monitoramento e alertas
    - Inclua mudanças de uso de memória e recursos

11. **Atualizações de Segurança**
    - Documente melhorias e mudanças de segurança
    - Atualize código de autenticação e autorização
    - Revise e atualize configurações de segurança
    - Atualize varredura de segurança de dependências
    - Documente novas práticas recomendadas de segurança

12. **Estratégia de Deployment**
    - Planeje abordagem de rollout em fases
    - Crie scripts de deployment e automação
    - Configure monitoramento e health checks
    - Planeje deployments azul-verde ou canário
    - Documente procedimentos de reversão

13. **Problemas Comuns e Resolução de Problemas**
    
    ```markdown
    ## Problemas Comuns de Migração
    
    ### Problema: Erros de Resolução de Import/Módulo
    **Sintomas:** Não é possível resolver o módulo 'old-package'
    **Solução:** 
    1. Atualize instruções de import para novos nomes de pacotes
    2. Verifique package.json para dependências corretas
    3. Limpe node_modules e reinstale
    
    ### Problema: Método de API Não Encontrado
    **Sintomas:** TypeError: oldMethod is not a function
    **Solução:** Substitua pela nova API conforme documentado na etapa 3
    ```

14. **Comunicação e Treinamento do Time**
    - Crie materiais de treinamento para o time
    - Agende sessões de compartilhamento de conhecimento
    - Documente novos fluxos de trabalho de desenvolvimento
    - Atualize padrões de codificação e diretrizes
    - Crie guias de referência rápida

15. **Ferramentas e Automação**
    - Forneça scripts e utilitários de migração
    - Crie ferramentas de transformação de código (codemods)
    - Configure verificações de compatibilidade automatizadas
    - Implemente atualizações de pipeline CI/CD
    - Crie ferramentas de validação e verificação

16. **Timeline e Marcos**
    
    ```markdown
    ## Timeline de Migração
    
    ### Fase 1: Preparação (Semana 1-2)
    - [ ] Configuração do ambiente
    - [ ] Treinamento do time
    - [ ] Migração do ambiente de desenvolvimento
    
    ### Fase 2: Desenvolvimento (Semana 3-6)
    - [ ] Migração da aplicação principal
    - [ ] Testes e validação
    - [ ] Otimização de desempenho
    
    ### Fase 3: Deployment (Semana 7-8)
    - [ ] Deployment em staging
    - [ ] Deployment em produção
    - [ ] Monitoramento e suporte
    ```

17. **Mitigação de Riscos**
    - Identifique riscos potenciais de migração
    - Crie planos de contingência para cada risco
    - Documente procedimentos de escalação
    - Planeje cenários de timeline estendida
    - Prepare comunicação para stakeholders

18. **Tarefas Pós-Migração**
    - Limpe código e configurações deprecados
    - Atualize documentação e arquivos README
    - Revise e otimize a nova implementação
    - Conduza retrospectiva pós-migração
    - Planeje manutenção e atualizações futuras

19. **Validação e Testes**
    - Crie planos de teste abrangentes
    - Documente critérios de aceitação
    - Configure testes de regressão automatizados
    - Planeje testes de aceitação do usuário
    - Implemente monitoramento e alertas

20. **Atualizações de Documentação**
    - Atualize documentação de API
    - Revise guias de desenvolvimento
    - Atualize documentação de deployment
    - Crie guias de resolução de problemas
    - Atualize materiais de onboarding do time

**Tipos de Migração e Considerações Específicas:**

**Migração de Framework (React 17 → 18):**
- Atualize imports do React e ReactDOM
- Substitua métodos de ciclo de vida deprecados
- Atualize métodos da testing library
- Lide com recursos concurrent e Suspense

**Migração de Banco de Dados (MySQL → PostgreSQL):**
- Converta diferenças de sintaxe SQL
- Atualize tipos de dados e restrições
- Migre stored procedures para funções
- Atualize configurações de ORM

**Migração em Cloud (On-premise → AWS):**
- Containerize aplicações
- Atualize pipelines CI/CD
- Configure serviços em cloud
- Implemente infraestrutura como código

**Migração de Arquitetura (Monolito → Microserviços):**
- Identifique limites de serviços
- Implemente comunicação entre serviços
- Configure service discovery
- Planeje estratégias de consistência de dados

Lembre-se de:
- Teste minuciosamente em ambientes fora de produção primeiro
- Comunique progresso e problemas regularmente
- Documente lições aprendidas para futuras migrações
- Mantenha o guia de migração atualizado com base em experiências reais