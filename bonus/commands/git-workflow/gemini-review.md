---
allowed-tools: Bash(gh:*), Read, Grep, TodoWrite, Edit, MultiEdit
argument-hint: [número-pr] | --analyze-only | --preview | --priority high|medium|low
description: Transformar análises de PR do Gemini Code Assist em TodoLists priorizadas com execução automatizada
model: claude-sonnet-4-5-20250929
---

# Automação de Análise de PR com Gemini

## Por Que Este Comando Existe

**O Problema**: Gemini Code Assist fornece análises automáticas e gratuitas de PRs no GitHub. Mas revisões geradas por IA frequentemente são ignoradas porque carecem da urgência do feedback humano.

**O Ponto Crítico**: Pedir manualmente ao Claude Code para:
1. "Analisar a análise do Gemini do PR #42"
2. "Priorizar os problemas"
3. "Criar uma TodoList"
4. "Começar a trabalhar neles"

...fica tedioso rapidamente.

**A Solução**: Um único comando que automaticamente busca a análise do Gemini, avalia severidade, cria TodoLists priorizadas e opcionalmente inicia execução.

## O Que Torna Isto Diferente

| | Análise de Código | Melhoria de Código | Análise do Gemini |
|---|---|---|---|
| **Gatilho** | Quando você quer análise | Quando você quer melhorias | **Quando o Gemini já analisou** |
| **Entrada** | Base de código local | Base de código local | **Comentários do Gemini no PR do GitHub** |
| **Propósito** | Análise geral | Melhorias gerais | **Converter feedback de IA → TODOs acionáveis** |
| **Saída** | Relatório de análise | Melhorias aplicadas | **TodoList + Prioridade + Execução** |

## Gatilhos
- PR tem comentários de análise do Gemini Code Assist aguardando serem endereçados
- Necessário converter feedback de IA em itens de ação estruturados
- Deseja processar sistematicamente feedback de análise automatizada
- Reduzir mudanças manuais de contexto entre GitHub e desenvolvimento

## Uso
```bash
/gemini-review [número-pr] [--analyze-only] [--preview] [--priority high|medium|low]
```

## Fluxo de Comportamento
1. **Buscar**: Recuperar detalhes do PR e comentários de análise do Gemini usando GitHub CLI
2. **Analisar**: Analisar e categorizar comentários de análise por tipo e severidade
3. **Priorizar**: Avaliar cada comentário quanto à necessidade de refatoração e impacto
4. **TodoList**: Gerar TodoList estruturada com ordenação por prioridade
5. **Executar**: (Opcional) Começar a trabalhar em itens de alta prioridade com confirmação do usuário

Comportamentos-chave:
- Categorização inteligente de comentários (crítico, melhoria, sugestão, estilo)
- Avaliação de impacto para cada item de análise com estimativa de esforço
- Criação automática de TodoList com matriz de prioridade (imprescindível, importante, complementar)
- Mapeamento de localização de código e análise de dependências
- Estratégia de implementação com abordagem em fases

## Coordenação de Ferramentas
- **Bash**: Operações de GitHub CLI para busca de dados de PR e análise
- **Pensamento Sequencial**: Raciocínio multi-etapa para decisões complexas de refatoração
- **Grep**: Análise de padrões de código e identificação de localização de problemas
- **Read**: Inspeção de código-fonte para compreensão de contexto
- **TodoWrite**: Geração automática de TodoList com prioridades
- **Edit/MultiEdit**: Modificações de código ao executar correções

## Padrões-Chave
- **Análise de Comentários**: Comentários do Gemini → dados de análise estruturados
- **Classificação de Severidade**: Tipo de comentário → atribuição de nível de prioridade (Imprescindível/Importante/Complementar/Pular)
- **Geração de TodoList**: Resultados de análise → TodoWrite com itens priorizados
- **Análise de Impacto**: Mudanças de código → avaliação de efeito colateral
- **Planejamento de Execução**: Estratégia → etapas de implementação acionáveis

## Exemplos

### Analisar PR do Ramo Atual
```bash
/gemini-review
# Detecta automaticamente PR do ramo atual
# Gera TodoList priorizada a partir da análise do Gemini
# Pronta para executar após confirmação do usuário
```

### Analisar PR Específico
```bash
/gemini-review 42
# Analisa comentários de análise do Gemini no PR #42
# Cria TodoList priorizada com estimativas de esforço
```

### Modo Visualização (Execução Segura)
```bash
/gemini-review --preview
# Mostra o que seria corrigido sem aplicar alterações
# Cria TodoList para execução manual
# Permite revisão antes da implementação
```

## Exemplo de Fluxo Real

**Antes (Manual, Tedioso)**:
```bash
1. Abrir página do PR no GitHub
2. Ler análise do Gemini (frequentemente ignorada porque "gerada por IA")
3. Dizer ao Claude: "Analise a análise do Gemini do PR #42"
4. Dizer ao Claude: "Priorize esses problemas"
5. Dizer ao Claude: "Crie uma TodoList"
6. Dizer ao Claude: "Comece a trabalhar neles"
```

**Depois (Automatizado)**:
```bash
/gemini-review 42
# → TodoList criada automaticamente
# → Prioridades definidas com base em severidade
# → Pronta para execução imediata
```

## Estrutura de Saída de Análise

### 1. Resumo de Análise
- Contagem total de comentários por severidade
- Distribuição de severidade (crítico/melhoria/sugestão/estilo)
- Temas e padrões comuns identificados
- Sentimento geral da análise e áreas-chave de foco
- Esforço total estimado necessário

### 2. Análise Categorizada
Para cada comentário de análise:
- **Categoria**: Crítico | Melhoria | Sugestão | Estilo
- **Localização**: Caminho do arquivo e números de linha com contexto
- **Problema**: Descrição do problema da análise do Gemini
- **Impacto**: Consequências potenciais se não endereçado
- **Decisão**: Imprescindível | Importante | Complementar | Pular
- **Raciocínio**: Por que essa prioridade foi atribuída
- **Esforço**: Tempo de implementação estimado (Pequeno/Médio/Grande)

### 3. Geração de TodoList

**Cria automaticamente TodoList com confirmação do usuário antes da execução**

```
Alta Prioridade (Imprescindível):
✓ Corrigir injeção SQL em auth.js:45 (15 min)
✓ Remover chave de API exposta em config.js:12 (5 min)

Prioridade Média (Importante):
○ Refatorar complexidade do UserService (45 min)
○ Adicionar tratamento de erro ao fluxo de pagamento (30 min)

Baixa Prioridade (Complementar):
○ Atualizar comentários JSDoc (20 min)
○ Renomear variável para clareza (5 min)

Pulado:
- Sugestão de estilo conflita com padrões do projeto
- Já endereçado em abordagem diferente
```

*Nota: Usuário revisa e confirma a TodoList antes de qualquer modificação de código*

### 4. Plano de Execução
- **Fase 1 - Correções Críticas**: Problemas de segurança e falhas críticas (imediato)
- **Fase 2 - Melhorias Importantes**: Manutenibilidade e performance (mesmo PR)
- **Fase 3 - Melhorias Opcionais**: Estilo e documentação (PR futuro)
- **Dependências**: Ordem de implementação com base em dependências de código
- **Estratégia de Teste**: Atualizações de teste necessárias para cada fase

### 5. Registro de Decisão
- **Mudanças Aceitas**: O que será implementado e por quê
- **Mudanças Adiadas**: O que será endereçado em iterações futuras
- **Mudanças Rejeitadas**: O que não será implementado e raciocínio
- **Compensações**: Custos vs. benefícios analisados para cada decisão

## Limites

**Fará:**
- Buscar e analisar comentários de análise do Gemini Code Assist de PRs do GitHub
- Categorizar e priorizar sistematicamente feedback de análise
- Gerar TodoLists com ordenação de prioridade e estimativas de esforço
- Fornecer raciocínio de decisão e análise de compensações
- Mapear comentários de análise para localizações específicas de código
- Executar correções com confirmação do usuário em modo visualização

**Não Fará:**
- Implementar alterações automaticamente sem revisão do usuário (a menos que explicitamente solicitado)
- Descartar sugestões do Gemini sem análise e documentação
- Tomar decisões arquiteturais sem considerar contexto do projeto
- Modificar código fora do escopo de comentários de análise
- Trabalhar com sistemas de análise não-Gemini (GitHub Copilot, CodeRabbit, etc.)

## Critérios de Decisão

### Imprescindível (Crítico) - Alta Prioridade
- Vulnerabilidades de segurança e exposição de dados
- Problemas de integridade de dados e possível corrupção
- Mudanças críticas ou erros em tempo de execução
- Problemas de performance críticos (>100ms de atraso, vazamento de memória)
- Violações de princípios de arquitetura central

### Importante (Melhoria) - Prioridade Média
- Problemas de manutenibilidade de código e débito técnico
- Melhorias de performance moderada (ganhos de 10-100ms)
- Violações importantes de melhores práticas
- Lacunas significativas de legibilidade e documentação
- Melhorias de tratamento de erro e resiliência

### Complementar (Sugestão) - Baixa Prioridade
- Melhorias de estilo de código e formatação
- Otimizações menores (<10ms de ganho)
- Oportunidades opcionais de refatoração
- Mensagens de erro aprimoradas e logging
- Comentários adicionais de código e documentação

### Pular (Não Aplicável)
- Conflita com padrões estabelecidos do projeto
- Fora do escopo da iteração atual
- ROI baixo (esforço alto, impacto baixo)
- Sugestões excessivamente opinadas sem benefício claro
- Já endereçado por outros meios ou abordagem diferente

## Integração com Fluxo de Git

### Fluxo Recomendado
```bash
1. Criar PR → Gemini analisa automaticamente
2. Executar /gemini-review para gerar TodoList
3. Revisar prioridades da TodoList e ajustar se necessário
4. Executar correções sistematicamente (Fase 1 → Fase 2 → Fase 3)
5. Fazer commit das alterações com mensagens de commit convencionais
6. Atualizar PR e re-solicitar análise do Gemini se necessário
```

### Estratégia de Commit
- Agrupar alterações de refatoração relacionadas por categoria
- Usar mensagens de commit convencionais referenciando itens de análise
  - `fix(auth): resolver vulnerabilidade de injeção SQL (Gemini PR#42)`
  - `refactor(services): reduzir complexidade do UserService (Gemini PR#42)`
  - `docs: atualizar comentários JSDoc (Gemini PR#42)`
- Criar commits separados para mudanças críticas vs. melhorias
- Documentar raciocínio de decisão em mensagens de commit

## Uso Avançado

### Modo Interativo (Recomendado para Análises Complexas)
```
/gemini-review --interactive
# Percorrer cada comentário de análise com prompts de decisão
# Permite ajuste manual de prioridade
# Mostra contexto de código para cada problema
```

### Exportar Análise
```
/gemini-review --export gemini-analysis.md
# Exportar análise abrangente para arquivo markdown
# Útil para revisão em equipe e documentação
# Inclui todas as decisões e raciocínio
```

### Execução Seca (Sem Criar TodoList)
```
/gemini-review --dry-run
# Mostra análise e prioridades sem criar TodoList
# Útil para entender escopo antes de se comprometer
# Sem alterações no estado do fluxo
```

## Requisitos de Ferramenta
- **GitHub CLI** (`gh`) instalado e autenticado
- **Repositório** deve ter Gemini Code Assist configurado como revisor de PR
- **Ramo atual** deve ter PR associado ou fornecer número de PR explicitamente

## Configurar Gemini Code Assist

Se você ainda não configurou Gemini Code Assist:

1. Visite [App do GitHub do Gemini Code Assist](https://developers.google.com/gemini-code-assist/docs/set-up-code-assist-github)
2. Instale o app na sua organização/conta
3. Selecione repositórios para integração
4. Gemini analisará automaticamente PRs com tag `/gemini` ou auto-análise

**Por Que Gemini?**
- **Gratuito**: Sem custo para análises automáticas de PR
- **Abrangente**: Cobre segurança, performance, melhores práticas
- **Nativo do GitHub**: Integrado diretamente no fluxo de PR
- **Automatizado**: Sem necessidade de solicitações manuais de análise

## Limitações

- Suporta apenas análises do Gemini Code Assist (não GitHub Copilot, CodeRabbit, etc.)
- Requer acesso a GitHub CLI e autenticação
- Qualidade de análise depende da qualidade da análise do Gemini
- Não é possível modificar análises ou re-disparar análise do Gemini