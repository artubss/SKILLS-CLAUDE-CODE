[Abrir Diagrama da Equipe de Pesquisa Aberta](../../../images/research_team_diagram.html)

## Visão Geral do Agente da Equipe de Pesquisa Aberta

A Equipe de Pesquisa Aberta representa um sistema sofisticado de pesquisa multi-agentes projetado para conduzir pesquisas abrangentes, com qualidade acadêmica, sobre tópicos complexos. Esta equipe orquestra nove agentes especializados através de um workflow hierárquico que garante cobertura completa, análise rigorosa e output de alta qualidade.

---

### 1. Agente Orquestrador de Pesquisa

**Propósito:** Coordenador central que gerencia todo o workflow de pesquisa, desde a consulta inicial até a geração do relatório final, garantindo que todas as fases sejam executadas em sequência correta com controle de qualidade.

**Características Principais:**

- Gerenciamento master do workflow em todas as fases de pesquisa
- Roteamento inteligente de tarefas para agentes especializados apropriados
- Portais de qualidade e validação entre estágios do workflow
- Gerenciamento de estado e rastreamento de progresso em projetos de pesquisa complexos
- Capacidades de tratamento de erros e degradação elegante
- Integração TodoWrite para rastreamento transparente de progresso

**Exemplo de Prompt de Sistema:**

```
You are the Research Orchestrator, an elite coordinator responsible for managing comprehensive research projects using the Open Deep Research methodology. You excel at breaking down complex research queries into manageable phases and coordinating specialized agents to deliver thorough, high-quality research outputs.
```

---

### 2. Agente Esclarecedor de Consulta

**Propósito:** Analisa consultas de pesquisa recebidas quanto à clareza, especificidade e acionabilidade. Determina quando esclarecimento do usuário é necessário antes de começar a pesquisa para otimizar qualidade.

**Características Principais:**

- Análise sistemática de consulta para detecção de ambiguidade e vagueza
- Sistema de pontuação de confiança (0.0-1.0) para tomada de decisão
- Geração estruturada de perguntas de esclarecimento com opções múltiplas
- Identificação de área de foco e geração de consulta refinada
- Output estruturado em JSON para integração seamless do workflow
- Framework de decisão equilibrando minuciosidade com experiência do usuário

**Exemplo de Prompt de Sistema:**

```
You are the Query Clarifier, an expert in analyzing research queries to ensure they are clear, specific, and actionable before research begins. Your role is critical in optimizing research quality by identifying ambiguities early.
```

---

### 3. Agente Gerador de Briefing de Pesquisa

**Propósito:** Transforma consultas de pesquisa esclarecidas em planos de pesquisa estruturados e acionáveis com perguntas específicas, palavras-chave, preferências de fontes e critérios de sucesso.

**Características Principais:**

- Conversão de consultas amplas em perguntas de pesquisa específicas
- Identificação de fontes e planejamento de metodologia de pesquisa
- Definição de critérios de sucesso e estabelecimento de limites de escopo
- Extração de palavras-chave para busca direcionada
- Planejamento de cronograma de pesquisa e alocação de recursos
- Integração com agentes de pesquisa downstream para handoff seamless

**Exemplo de Prompt de Sistema:**

```
You are the Research Brief Generator, transforming user queries into comprehensive research frameworks that guide systematic investigation and ensure thorough coverage of all relevant aspects.
```

---

### 4. Agente Coordenador de Pesquisa

**Propósito:** Planeja e coordena estrategicamente tarefas de pesquisa complexas em múltiplos pesquisadores especialistas, analisando requisitos e alocando tarefas para cobertura abrangente.

**Características Principais:**

- Estratégia de alocação de tarefas entre pesquisadores especializados
- Coordenação de threads de pesquisa paralela e gerenciamento de dependências
- Otimização de recursos e balanceamento de carga de trabalho
- Checkpoints de controle de qualidade e rastreamento de marcos
- Facilitação de comunicação entre pesquisadores
- Definição de estratégia de iteração para cobertura abrangente

**Exemplo de Prompt de Sistema:**

```
You are the Research Coordinator, strategically planning and coordinating complex research tasks across multiple specialist researchers. You analyze research requirements, allocate tasks to appropriate specialists, and define iteration strategies for comprehensive coverage.
```

---

### 5. Agente Pesquisador Acadêmico

**Propósito:** Encontra, analisa e sintetiza fontes acadêmicas, papers de pesquisa e literatura acadêmica com ênfase em fontes revisadas por pares e formatação correta de citações.

**Características Principais:**

- Busca em bases de dados acadêmicas (ArXiv, PubMed, Google Scholar)
- Verificação de status de revisão por pares e avaliação de impacto de periódicos
- Análise de citações e identificação de trabalhos seminais
- Extração de metodologia de pesquisa e avaliação de qualidade
- Formatação bibliográfica apropriada e preservação de DOI
- Identificação de lacunas de pesquisa e análise de direções futuras

**Exemplo de Prompt de Sistema:**

```
You are the Academic Researcher, specializing in finding and analyzing scholarly sources, research papers, and academic literature. Your expertise includes searching academic databases, evaluating peer-reviewed papers, and maintaining academic rigor throughout the research process.
```

---

### 6. Agente Pesquisador Técnico

**Propósito:** Analisa repositórios de código, documentação técnica, detalhes de implementação e avalia soluções técnicas com foco em aspectos práticos de implementação.

**Características Principais:**

- Análise de repositório GitHub e avaliação de qualidade de código
- Revisão de documentação técnica e análise de API
- Identificação de padrões de implementação e avaliação de melhores práticas
- Rastreamento de histórico de versão e análise de stack de tecnologia
- Extração de exemplos de código e avaliação de viabilidade técnica
- Integração com ferramentas de desenvolvimento e recursos técnicos

**Exemplo de Prompt de Sistema:**

```
You are the Technical Researcher, specializing in analyzing code repositories, technical documentation, and implementation details. You evaluate technical solutions, review code quality, and assess the practical aspects of technology implementations.
```

---

### 7. Agente Analista de Dados

**Propósito:** Fornece análise quantitativa, insights estatísticos e pesquisa orientada por dados com foco em interpretação de dados numéricos e identificação de tendências.

**Características Principais:**

- Análise estatística e capacidades de identificação de tendências
- Sugestões de visualização de dados e interpretação de métricas
- Análise comparativa em diferentes datasets e períodos de tempo
- Análise de benchmark de desempenho e pesquisa quantitativa
- Consulta de banco de dados e avaliação de qualidade de dados
- Integração com ferramentas estatísticas e fontes de dados

**Exemplo de Prompt de Sistema:**

```
You are the Data Analyst, specializing in quantitative analysis, statistical insights, and data-driven research. You excel at finding and interpreting numerical data, identifying trends, creating comparisons, and suggesting data visualizations.
```

---

### 8. Agente Sintetizador de Pesquisa

**Propósito:** Consolida e sintetiza descobertas de múltiplas fontes de pesquisa em análise unificada e abrangente, preservando complexidade e identificando contradições.

**Características Principais:**

- Consolidação de descobertas multi-fonte e identificação de padrões
- Resolução de contradições e análise de viés
- Extração de temas e mapeamento de relacionamentos entre fontes diversas
- Preservação de nuances enquanto cria resumos acessíveis
- Avaliação de força de evidência e pontuação de confiança
- Geração de insights estruturados para preparação de relatório

**Exemplo de Prompt de Sistema:**

```
You are the Research Synthesizer, responsible for consolidating findings from multiple research sources into a unified, comprehensive analysis. You excel at merging diverse perspectives, identifying patterns, and creating structured insights while preserving complexity.
```

---

### 9. Agente Gerador de Relatório

**Propósito:** Transforma descobertas de pesquisa sintetizadas em relatórios finais abrangentes e bem estruturados com formatação apropriada, citações e fluxo narrativo.

**Características Principais:**

- Estruturação profissional de relatório e desenvolvimento de narrativa
- Formatação de citações e gerenciamento de bibliografia
- Criação de sumário executivo e destaque de insights-chave
- Formulação de recomendações baseadas em descobertas de pesquisa
- Suporte a múltiplos formatos de output (acadêmico, negócio, técnico)
- Garantia de qualidade e otimização de formatação final

**Exemplo de Prompt de Sistema:**

```
You are the Report Generator, transforming synthesized research findings into comprehensive, well-structured final reports. You create readable narratives from complex research data, organize content logically, and ensure proper citation formatting.
```

---

### Arquitetura de Workflow

**Fases Sequenciais:**

1. **Processamento de Consulta**: Orquestrador → Esclarecedor de Consulta → Gerador de Briefing de Pesquisa
2. **Planejamento**: Coordenador de Pesquisa desenvolve estratégia e aloca tarefas de especialista
3. **Pesquisa Paralela**: Analistas acadêmico, técnico e de dados trabalham simultaneamente
4. **Síntese**: Sintetizador de Pesquisa consolida todas as descobertas de especialista
5. **Output**: Gerador de Relatório cria relatório final abrangente

**Padrões Principais de Orquestração:**

- **Coordenação Hierárquica**: Orquestrador central gerencia todas as fases do workflow
- **Execução Paralela**: Pesquisadores especialistas trabalham simultaneamente para eficiência
- **Portais de Qualidade**: Checkpoints de validação entre cada fase principal
- **Gerenciamento de Estado**: Contexto persistente e preservação de descobertas ao longo do workflow
- **Recuperação de Erro**: Degradação elegante e mecanismos de retry

**Protocolo de Comunicação:**

Todos os agentes usam JSON estruturado para comunicação inter-agentes, mantendo:
- Rastreamento de status de fase e conclusão
- Preservação de dados acumulados e descobertas
- Métricas de qualidade e pontuação de confiança
- Planejamento de próxima ação e gerenciamento de dependência

---

### Notas Gerais de Configuração:

- Cada agente opera com permissões de ferramenta focadas apropriadas ao seu papel
- Agentes podem ser invocados individualmente ou como parte do workflow completo
- O orquestrador mantém gerenciamento de estado abrangente em todas as fases
- Controle de qualidade está integrado em cada ponto de transição de workflow
- O sistema suporta tanto projetos de pesquisa completos quanto consulta de agente individual
- Todas as descobertas mantêm rastreabilidade completa para fontes originais e metodologias

Esta equipe de pesquisa representa uma abordagem abrangente para pesquisa assistida por IA, combinando os pontos fortes de agentes especializados com gerenciamento coordenado de workflow para entregar resultados de pesquisa abrangentes e de alta qualidade em tópicos complexos.