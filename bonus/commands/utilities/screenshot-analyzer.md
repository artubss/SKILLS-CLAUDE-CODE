---
description: "Analise capturas de tela de produtos para extrair recursos e gerar listas de tarefas de desenvolvimento."
argument-hint: Caminho da captura de tela ou descrição
allowed-tools: Read, Write, Grep, Glob, TodoWrite, Task
---

# Comando de Análise de Captura de Tela

Este comando usa um pipeline multi-agente para análise abrangente de capturas de tela.

## Fase 1: Descoberta

**Objetivo**: Entender quais capturas de tela analisar

Solicitação inicial: $ARGUMENTS

**Ações**:
1. Se nenhum caminho de captura de tela foi fornecido, pergunte ao usuário:
   - Quais capturas de tela você quer que eu analise?
   - De qual produto/app são essas capturas?
   - Isso é para análise competitiva ou planejamento de produto interno?
2. Leia e verifique se os arquivos de captura de tela existem
3. Confirme o escopo com o usuário (tela única, app completo, recurso específico)

---

## Fase 2: Análise Multi-Agente

Para cada captura de tela, inicie TRÊS agentes de análise EM PARALELO usando a ferramenta Task:

### Chamadas de Agentes em Paralelo

Inicie todos os três em uma ÚNICA mensagem com múltiplas chamadas da ferramenta Task:

**Task 1: Análise de UI**
- subagent_type: "general-purpose"
- prompt: Inclua o caminho da captura de tela, solicite análise de componentes de UI, layout, padrões de design em formato JSON

**Task 2: Análise de Interação**
- subagent_type: "general-purpose"
- prompt: Inclua o caminho da captura de tela, solicite fluxos de usuário, elementos clicáveis, transições de estado em formato JSON

**Task 3: Análise de Negócio**
- subagent_type: "general-purpose"
- prompt: Inclua o caminho da captura de tela, solicite funções de negócio, entidades de dados, lógica de domínio em formato JSON

Cada agente deve:
1. Ler a captura de tela usando a ferramenta Read
2. Analisar de acordo com sua especialidade
3. Retornar análise estruturada em JSON

---

## Fase 3: Síntese

Após as análises paralelas serem concluídas, inicie o agente sintetizador:

**Task 4: Síntese**
- subagent_type: "general-purpose"
- prompt: Forneça todos os três resultados de análise, solicite combinar em lista de tarefas unificada

O sintetizador deve:
1. Fazer referência cruzada de todas as análises
2. Eliminar duplicatas de recursos
3. Gerar lista de tarefas hierárquica
4. Saída em formato markdown

---

## Fase 4: Revisão

Inicie o agente revisor para validar:

**Task 5: Revisão**
- subagent_type: "general-purpose"
- prompt: Forneça o caminho da captura de tela e a lista de tarefas sintetizada, solicite revisão de completude

O revisor deve:
1. Comparar com a captura de tela original
2. Verificar recursos faltantes
3. Validar qualidade das tarefas
4. Sugerir correções se necessário

---

## Fase 5: Saída

1. Escreva a lista de tarefas final em `docs/plans/YYYY-MM-DD-<product>-features.md`
2. Crie o diretório docs/plans se não existir
3. Apresente resumo ao usuário com:
   - Total de módulos identificados
   - Total de recursos extraídos
   - Total de tarefas geradas
   - Link para arquivo de saída

---

## Exemplo de Chamada de Análise Paralela

Quando você chegar à Fase 2, sua resposta deve incluir TRÊS chamadas da ferramenta Task como:

```
[Task 1: Análise de UI para screenshot.png]
[Task 2: Análise de Interação para screenshot.png]
[Task 3: Análise de Negócio para screenshot.png]
```

Todas iniciadas simultaneamente para máxima eficiência.