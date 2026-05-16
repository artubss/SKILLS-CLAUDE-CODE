---
description: Desenvolvimento orientado de features com compreensão de codebase e foco em arquitetura
argument-hint: Descrição opcional da feature
---

# Desenvolvimento de Features

Você está ajudando um desenvolvedor a implementar uma nova feature. Siga uma abordagem sistemática: compreenda o codebase profundamente, identifique e questione todos os detalhes não especificados, projete arquiteturas elegantes, implemente, teste minuciosamente e revise.

## Princípios Principais

- **Faça perguntas esclarecedoras**: Identifique todas as ambiguidades, casos extremos e comportamentos não especificados. Faça perguntas específicas e concretas em vez de fazer suposições. Aguarde as respostas do usuário antes de prosseguir com a implementação. Faça perguntas cedo (depois de compreender o codebase, antes de projetar a arquitetura).
- **Compreenda antes de agir**: Leia e entenda os padrões de código existentes primeiro
- **Leia arquivos identificados por agentes**: Ao ativar agentes, peça-lhes para retornar listas dos arquivos mais importantes a ler. Depois que os agentes terminarem, leia esses arquivos para construir contexto detalhado antes de prosseguir.
- **Simples e elegante**: Priorize código legível, mantível e arquitetonicamente sólido
- **Teste minuciosamente**: Garanta cobertura de testes apropriada para todo o código novo
- **Use TodoWrite**: Acompanhe todo o progresso ao longo do processo

---

## Fase 1: Descoberta

**Objetivo**: Entender o que precisa ser construído

Solicitação inicial: $ARGUMENTS

**Ações**:
1. Crie lista de tarefas com todas as fases
2. Se a feature não estiver clara, pergunte ao usuário:
   - Que problema vocês estão resolvendo?
   - O que a feature deve fazer?
   - Há restrições ou requisitos?
3. Resuma o entendimento e confirme com o usuário

---

## Fase 2: Exploração do Codebase

**Objetivo**: Compreender código existente relevante e padrões em altos e baixos níveis

**Ações**:
1. Ative 2-3 agentes code-explorer em paralelo. Cada agente deve:
   - Rastrear o código de forma abrangente e focar em obter compreensão completa de abstrações, arquitetura e fluxo de controle
   - Focar em um aspecto diferente do codebase (ex: features similares, compreensão de alto nível, compreensão arquitetônica, experiência do usuário, etc)
   - Incluir uma lista de 5-10 arquivos-chave a ler

   **Exemplos de prompts para agentes**:
   - "Encontre features similares a [feature] e rastreie sua implementação de forma abrangente"
   - "Mapeie a arquitetura e abstrações para [área de feature], rastreando o código de forma abrangente"
   - "Analise a implementação atual de [feature/área existente], rastreando o código de forma abrangente"
   - "Identifique padrões de UI, abordagens de testes ou pontos de extensão relevantes para [feature]"

2. Assim que os agentes retornarem, leia todos os arquivos identificados pelos agentes para construir compreensão profunda
3. Apresente resumo abrangente dos achados e padrões descobertos

---

## Fase 3: Perguntas Esclarecedoras

**Objetivo**: Preencher lacunas e resolver todas as ambiguidades antes de projetar

**CRÍTICO**: Esta é uma das fases mais importantes. NÃO PULE.

**Ações**:
1. Revise os achados do codebase e a solicitação original da feature
2. Identifique aspectos não especificados: casos extremos, tratamento de erros, pontos de integração, limites de escopo, preferências de design, compatibilidade retroativa, necessidades de performance
3. **Apresente todas as perguntas ao usuário em uma lista clara e organizada**
4. **Aguarde as respostas antes de prosseguir para o design de arquitetura**

Se o usuário disser "o que você achar melhor", forneça sua recomendação e obtenha confirmação explícita.

---

## Fase 4: Design de Arquitetura

**Objetivo**: Projetar múltiplas abordagens de implementação com diferentes trade-offs

**Ações**:
1. Ative 2-3 agentes code-architect em paralelo com diferentes focos: mudanças mínimas (menor mudança, máxima reutilização), arquitetura limpa (manutenibilidade, abstrações elegantes), ou equilíbrio pragmático (velocidade + qualidade)
2. Revise todas as abordagens e forme sua opinião sobre qual se encaixa melhor para essa tarefa específica (considere: pequeno ajuste vs feature grande, urgência, complexidade, contexto da equipe)
3. Apresente ao usuário: resumo breve de cada abordagem, comparação de trade-offs, **sua recomendação com justificativa**, diferenças concretas de implementação
4. **Pergunte ao usuário qual abordagem ele prefere**

---

## Fase 5: Implementação

**Objetivo**: Construir a feature

**NÃO COMECE SEM APROVAÇÃO EXPLÍCITA DO USUÁRIO**

**Ações**:
1. Aguarde aprovação explícita do usuário
2. Leia todos os arquivos relevantes identificados nas fases anteriores
3. Implemente seguindo a arquitetura escolhida
4. Siga as convenções do codebase estritamente
5. Escreva código limpo e bem documentado
6. Atualize as tarefas conforme progride

---

## Fase 6: Testes Automatizados

**Objetivo**: Garantir cobertura de testes abrangente e que todos os testes passem

**Ações**:
1. **Gerar Testes**: Ative 2 agentes test-generator em paralelo com diferentes focos:
   - Testes unitários: Foco em funções individuais, casos extremos, tratamento de erros
   - Testes de integração: Foco em interações entre componentes, fluxo de dados, contratos de API

   Cada agente deve analisar o código novo e fornecer:
   - Casos de teste com código de implementação completo
   - Classificação de prioridade (crítico/importante/nice-to-have)
   - Mocks e fixtures necessários

2. **Revisar Testes Gerados**:
   - Consolide recomendações de testes de ambos os agentes
   - Priorize testes críticos que devem ser implementados
   - Apresente plano de testes ao usuário para aprovação

3. **Implementar Testes**:
   - Escreva os casos de teste aprovados seguindo as convenções do projeto
   - Configure mocks e fixtures de teste necessários
   - Garanta que os testes sejam bem organizados e mantíveis

4. **Executar Testes**: Ative agente test-runner para:
   - Executar a suite de testes completa (ou subset relevante)
   - Analisar qualquer falha com diagnóstico de causa raiz
   - Fornecer correções específicas para testes falhando

5. **Corrigir e Iterar**:
   - Se testes falharem por bugs na implementação, corrija a implementação
   - Se testes falharem por problemas nos testes, corrija os testes
   - Re-execute testes até que todos passem
   - **Não prossiga para Revisão de Qualidade até que todos os testes passem**

6. **Relatar Cobertura**: Resuma a cobertura de testes alcançada e quaisquer lacunas

---

## Fase 7: Revisão de Qualidade

**Objetivo**: Garantir que o código seja simples, DRY, elegante, fácil de ler e funcionalmente correto

**Ações**:
1. Ative 3 agentes code-reviewer em paralelo com diferentes focos: simplicidade/DRY/elegância, bugs/correção funcional, convenções do projeto/abstrações
2. Consolide achados e identifique problemas de maior severidade que você recomenda corrigir
3. **Apresente achados ao usuário e pergunte o que ele quer fazer** (corrigir agora, corrigir depois, ou prosseguir como está)
4. Aborde problemas com base na decisão do usuário
5. Se mudanças significativas forem feitas, re-execute testes usando agente test-runner para garantir que nada quebrou

---

## Fase 8: Resumo

**Objetivo**: Documentar o que foi realizado

**Ações**:
1. Marque todas as tarefas como completas
2. Resuma:
   - O que foi construído
   - Principais decisões tomadas
   - Arquivos modificados
   - Cobertura de testes alcançada
   - Próximos passos sugeridos

---