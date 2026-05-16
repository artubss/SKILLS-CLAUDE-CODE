# Explorador de Cenários de Arquitetura

Explore decisões arquiteturais através de análise sistemática de cenários com avaliação de trade-offs e validação de futuro-proofing.

## Instruções

Você está sendo designado para explorar sistematicamente decisões arquiteturais através de modelagem abrangente de cenários e otimizar escolhas de design de sistemas. Siga esta abordagem: **$ARGUMENTS**

### 1. Avaliação de Pré-requisitos

**Validação Crítica do Contexto Arquitetural:**

- **Escopo do Sistema**: Qual arquitetura de sistema ou componente você está projetando?
- **Requisitos de Escala**: Quais são os padrões de uso esperados e projeções de crescimento?
- **Restrições**: Quais restrições técnicas, comerciais ou de recursos se aplicam?
- **Cronograma**: Qual é o cronograma de implementação e o roadmap de evolução?
- **Critérios de Sucesso**: Como você medirá o sucesso arquitetural?

**Se o contexto estiver pouco claro, conduza sistematicamente:**

```
Escopo do Sistema Ausente:
"Qual arquitetura de sistema específica precisa ser explorada?
- Novo Design de Sistema: Arquitetura de aplicação greenfield ou serviço
- Migração de Sistema: Migrar de arquitetura legada para moderna
- Arquitetura de Escalabilidade: Expandir capacidades do sistema existente
- Arquitetura de Integração: Conectar múltiplos sistemas e serviços
- Arquitetura de Plataforma: Construir infraestrutura fundamental

Por favor, especifique os limites do sistema, componentes-chave e funções primárias."

Requisitos de Escala Ausentes:
"Quais são o escopo esperado do sistema e padrões de uso?
- Escala de Usuários: Número de usuários concorrentes e totais
- Escala de Dados: Volume, velocidade e variedade de dados processados
- Escala de Transações: Requisições por segundo, padrões de carga máxima
- Escala Geográfica: Distribuição em região única, multi-região ou global
- Projeções de Crescimento: Cronograma e magnitude esperados de escalabilidade"
```

### 2. Geração de Opções de Arquitetura

**Identifique sistematicamente abordagens arquiteturais:**

#### Matriz de Padrões de Arquitetura
```
Framework de Abordagem Arquitetural:

Padrões Monolíticos:
- Arquitetura em Camadas: Separação tradicional n-tier com delimitação clara
- Monólito Modular: Módulos bem-definidos dentro de um único deployment
- Arquitetura Plugin: Sistema core com ecossistema de plugins extensível
- Monólito Orientado a Serviços: Limites de serviço internos com deployment único

Padrões Distribuídos:
- Microserviços: Serviços independentes com alinhamento a capacidades comerciais
- Service Mesh: Microserviços com comunicação em nível de infraestrutura
- Event-Driven: Comunicação assíncrona com event sourcing
- CQRS/Event Sourcing: Separação comando-query com armazenamento de eventos

Padrões Híbridos:
- Microserviços Modulares: Serviços agrupados por domínio comercial
- Micro-Frontend: Decomposição frontend alinhada aos serviços backend
- Strangler Fig: Migração gradual de monólito para distribuído
- API Gateway: Ponto de entrada centralizado com roteamento de serviços backend

Padrões Cloud-Native:
- Serverless: Baseado em funções com infraestrutura de provedor cloud
- Container-Native: Primeiro Kubernetes com serviços cloud-native
- Multi-Cloud: Agnóstico a cloud com infraestrutura portável
- Edge-First: Computação distribuída com otimização de localização de edge
```

#### Especificação de Variações de Arquitetura
```
Para cada opção arquitetural:

Características Estruturais:
- Organização de Componentes: [como partes do sistema são estruturadas e relacionadas]
- Padrões de Comunicação: [síncrono vs assíncrono, protocolos, messaging]
- Gerenciamento de Dados: [estratégia de banco de dados, modelo de consistência, padrões de armazenamento]
- Modelo de Deployment: [empacotamento, distribuição, escalabilidade, abordagem operacional]

Atributos de Qualidade:
- Perfil de Escalabilidade: [escalabilidade horizontal vs vertical, análise de gargalos]
- Características de Confiabilidade: [modos de falha, recuperação, tolerância a faltas]
- Expectativas de Desempenho: [latência, throughput, eficiência de recursos]
- Modelo de Segurança: [autenticação, autorização, proteção de dados, superfície de ataque]

Considerações de Implementação:
- Stack Tecnológico: [linguagens, frameworks, bancos de dados, infraestrutura]
- Adequação da Estrutura de Time: [implicações da Lei de Conway, capacidades do time]
- Processo de Desenvolvimento: [workflows de build, teste, deploy, monitoramento]
- Estratégia de Evolução: [como a arquitetura pode crescer e mudar ao longo do tempo]
```

### 3. Desenvolvimento do Framework de Cenários

**Crie cenários abrangentes de teste arquitetural:**

#### Matriz de Cenários de Uso
```
Framework de Cenários Multi-Dimensional:

Cenários de Carga:
- Operação Normal: Padrões típicos de uso diário e tráfego
- Carga Máxima: Máximo esperado de uso concorrente e volume de transações
- Teste de Estresse: Além da capacidade normal para identificar pontos críticos
- Teste de Picos: Aumentos súbitos de tráfego e manipulação de burst

Cenários de Crescimento:
- Crescimento Linear: Aumentos constantes de volume de usuários e dados ao longo do tempo
- Crescimento Exponencial: Requisitos de escalabilidade rápida e adoção viral
- Expansão Geográfica: Deployment multi-região e escalabilidade global
- Expansão de Funcionalidades: Novas capacidades e adições de serviço

Cenários de Falha:
- Falhas de Componentes: Falhas de serviços individuais ou bancos de dados
- Falhas de Infraestrutura: Disrupções de rede, armazenamento ou computação
- Falhas em Cascata: Propagação de falha e impactos em todo o sistema
- Recuperação de Desastres: Recuperação de grande interrupção e continuidade comercial

Cenários de Evolução:
- Migração Tecnológica: Mudanças de framework, linguagem ou plataforma
- Mudanças no Modelo de Negócio: Novas fontes de receita ou ofertas de serviço
- Mudanças Regulatórias: Requisitos de conformidade e proteção de dados
- Resposta Competitiva: Pressões de mercado e requisitos de funcionalidades
```

#### Modelagem de Impacto de Cenários
- Impacto de desempenho sob cada tipo de cenário
- Implicações de custo para infraestrutura e operações
- Efeitos na velocidade de desenvolvimento e produtividade do time
- Avaliação de risco e requisitos de mitigação

### 4. Framework de Análise de Trade-offs

**Avaliação sistemática de trade-offs arquiteturais:**

#### Matriz de Trade-offs de Atributos de Qualidade
```
Avaliação de Qualidade de Arquitetura:

Trade-offs de Desempenho:
- Latência vs Throughput: Tempo de resposta vs processamento concorrente máximo
- Memória vs CPU: Estratégias de otimização de utilização de recursos
- Consistência vs Disponibilidade: Implicações do teorema CAP e escolhas
- Cache vs Atualização: Obsolescência de dados vs velocidade de resposta

Trade-offs de Escalabilidade:
- Horizontal vs Vertical: Abordagem de escalabilidade de infraestrutura e economia
- Stateless vs Stateful: Gerenciamento de sessão e implicações de desempenho
- Síncrono vs Assíncrono: Complexidade de comunicação vs desempenho
- Acoplamento vs Autonomia: Independência de serviço vs overhead operacional

Trade-offs de Desenvolvimento:
- Velocidade de Desenvolvimento vs Desempenho em Runtime: Investimento de tempo de otimização
- Type Safety vs Flexibilidade: Tratamento de erro em tempo de compilação vs runtime
- Reuso de Código vs Independência de Serviço: Bibliotecas compartilhadas vs duplicação
- Complexidade de Teste vs Confiabilidade do Sistema: Investimento em teste vs qualidade

Trade-offs Operacionais:
- Complexidade vs Controle: Serviços gerenciados vs infraestrutura auto-gerenciada
- Monitoramento vs Privacidade: Observabilidade vs proteção de dados
- Automação vs Flexibilidade: Padronização vs customização
- Custo vs Desempenho: Gasto de infraestrutura vs tempos de resposta
```

#### Construção de Matriz de Decisão
- Atribuição de peso para diferentes atributos de qualidade com base em prioridades comerciais
- Metodologia de pontuação para cada opção de arquitetura através de dimensões de qualidade
- Análise de sensibilidade para variações de peso e pontuação
- Identificação de fronteira de Pareto para soluções não-dominadas

### 5. Avaliação de Future-Proofing

**Avalie adaptabilidade arquitetural e potencial de evolução:**

#### Cenários de Evolução Tecnológica
```
Framework de Análise de Future-Proofing:

Integração de Tendências Tecnológicas:
- Integração de IA/ML: Embedding e escalabilidade de capacidades de machine learning
- Edge Computing: Processamento distribuído e requisitos de baixa latência
- Quantum Computing: Criptografia pós-quantum e impactos computacionais
- Blockchain/DLT: Integração de ledger distribuído e mecanismos de confiança

Preparação para Evolução de Mercado:
- Flexibilidade de Modelo de Negócio: Pivôs de assinatura, marketplace, plataforma
- Expansão Global: Conformidade multi-tenant, multi-região e multi-regulatória
- Evolução de Expectativa de Cliente: Experiências real-time, personalizadas, omnichannel
- Mudanças na Paisagem Competitiva: Paridade de funcionalidades e requisitos de diferenciação

Future-Proofing Regulatório:
- Regulamentação de Privacidade: Evolução de GDPR, CCPA e requisitos globais de privacidade
- Padrões de Segurança: Evolução de zero-trust e frameworks de conformidade
- Soberania de Dados: Residência de dados geográfica e restrições de cross-border
- Requisitos de Acessibilidade: Design inclusivo e suporte a tecnologia assistiva
```

#### Pontuação de Adaptabilidade
- Flexibilidade de arquitetura para mudanças de requisitos
- Viabilidade e custo de migração tecnológica
- Evolução de skill de time e gerenciamento de curva de aprendizado
- Proteção de investimento e gerenciamento de débito técnico

### 6. Engine de Simulação de Arquitetura

**Modele comportamento arquitetural em diferentes cenários:**

#### Framework de Simulação de Desempenho
```
Simulação de Arquitetura em Múltiplas Camadas:

Simulação em Nível de Componente:
- Características de desempenho e uso de recursos de serviço individual
- Desempenho de query de banco de dados e oportunidades de otimização
- Taxas de hit de cache e estratégias de invalidação
- Padrões de throughput e latência de fila de mensagens

Simulação em Nível de Integração:
- Overhead de comunicação serviço-a-serviço e otimização
- Desempenho de API gateway e eficiência de roteamento
- Distribuição de load balancer e health checking
- Efetividade de circuit breaker e mecanismos de retry

Simulação em Nível de Sistema:
- Fluxo de requisição end-to-end e experiência de usuário
- Distribuição de carga máxima e alocação de recursos
- Padrões de propagação de falha e recuperação
- Efetividade de sistema de monitoramento e alerting

Simulação em Nível de Infraestrutura:
- Utilização de recurso cloud e comportamento de auto-scaling
- Otimização de bandwidth de rede e latência
- Desempenho de armazenamento e padrões de consistência de dados
- Impacto de aplicação de política de segurança no desempenho
```

#### Integração de Modelagem de Custo
- Estimativa de custo de infraestrutura através de diferentes cenários
- Projeção de custo de desenvolvimento e operacional
- Análise de total cost of ownership em timeline multi-ano
- Análise de oportunidades de otimização de custo e trade-offs

### 7. Avaliação de Risco e Mitigação

**Avaliação abrangente de risco arquitetural:**

#### Framework de Risco Técnico
```
Avaliação de Risco de Arquitetura:

Riscos de Implementação:
- Maturidade Tecnológica: Riscos de adoção de tecnologia nova vs comprovada
- Gerenciamento de Complexidade: Desafios de compreensão e debug do sistema
- Desafios de Integração: Dependências de serviço de terceiros e compatibilidade
- Incerteza de Desempenho: Requisitos de escalabilidade e otimização não testados

Riscos Operacionais:
- Complexidade de Deployment: Gerenciamento de release e capacidades de rollback
- Lacunas de Monitoramento: Limitações de observabilidade e troubleshooting
- Desafios de Escalabilidade: Confiabilidade de auto-scaling e controle de custo
- Planejamento de Recuperação de Desastres: Backup, recuperação e planejamento de continuidade

Riscos Estratégicos:
- Lock-in Tecnológico: Dependência de vendor e flexibilidade de migração
- Dependências de Skill: Requisitos de expertise de time e lacunas de conhecimento
- Restrições de Evolução: Modificação e extensão de arquitetura
- Desvantagem Competitiva: Time-to-market e velocidade de desenvolvimento de features
```

#### Desenvolvimento de Estratégia de Mitigação de Risco
- Abordagens específicas de mitigação para riscos identificados
- Planejamento de contingência e opções de arquitetura alternativa
- Indicadores de alerta antecipado e estratégias de monitoramento
- Critérios de aceitação de risco e comunicação com stakeholders

### 8. Framework de Decisão e Recomendações

**Gere orientação arquitetural sistemática:**

#### Formato de Architecture Decision Record (ADR)
```
## Decisão de Arquitetura: [Nome do Sistema] - [Tópico de Decisão]

### Contexto e Declaração do Problema
- Requisitos de Negócio: [requisitos funcionais e não-funcionais principais]
- Restrições Atuais: [limitações técnicas, de recurso e cronograma]
- Drivers de Decisão: [fatores influenciando a escolha arquitetural]

### Opções de Arquitetura Consideradas

#### Opção 1: [Nome da Arquitetura]
- Descrição: [abordagem arquitetural e características principais]
- Prós: [vantagens e benefícios]
- Contras: [desvantagens e riscos]
- Trade-offs: [impactos específicos de atributo de qualidade]

[Repetir para cada opção]

### Resultado da Decisão
- Arquitetura Selecionada: [abordagem escolhida com fundamentação]
- Fundamentação da Decisão: [por que esta opção foi selecionada]
- Benefícios Esperados: [vantagens antecipadas e métricas de sucesso]
- Trade-offs Aceitos: [compromissos e estratégias de mitigação]

### Estratégia de Implementação
- Fase 1 (Imediata): [passos iniciais de implementação e validação]
- Fase 2 (Curto Prazo): [desenvolvimento do sistema core e integração]
- Fase 3 (Médio Prazo): [implementação de otimização e escalabilidade]
- Fase 4 (Longo Prazo): [roadmap de evolução e melhorias]

### Validação e Critérios de Sucesso
- Métricas de Desempenho: [KPIs específicos e intervalos aceitáveis]
- Portais de Qualidade: [conformidade arquitetural e checkpoints de validação]
- Cronograma de Revisão: [quando reavaliar decisões arquiteturais]
- Triggers de Adaptação: [condições que requerem modificação arquitetural]

### Riscos e Mitigação
- Riscos de Alta Prioridade: [principais preocupações e respostas]
- Estratégia de Monitoramento: [sistemas de alerta antecipado e health checks]
- Planos de Contingência: [abordagens alternativas se problemas surgirem]
- Aprendizado e Adaptação: [como incorporar feedback e melhorar]
```

### 9. Evolução Contínua de Arquitetura

**Estabeleça avaliação contínua de arquitetura e melhoria:**

#### Monitoramento de Saúde de Arquitetura
- Rastreamento de métrica de desempenho contra previsões arquiteturais
- Planejamento de acúmulo de débito técnico e remediação
- Medição de produtividade do time e velocidade de desenvolvimento
- Correlação de satisfação do usuário e resultados comerciais

#### Práticas de Arquitetura Evolutiva
- Revisão regular de arquitetura e avaliação de fitness function
- Identificação e implementação de melhorias incrementais
- Avaliação de tendência tecnológica e planejamento de adoção
- Compartilhamento de conhecimento de arquitetura entre times

## Exemplos de Uso

```bash
# Planejamento de migração de microserviços
/dev:architecture-scenario-explorer Avalie migração de monólito para microserviços para plataforma de e-commerce com 1M+ usuários

# Design de arquitetura de novo sistema
/dev:architecture-scenario-explorer Projete arquitetura para plataforma de análise em tempo real processando 100k eventos/segundo

# Avaliação de arquitetura de escalabilidade
/dev:architecture-scenario-explorer Analise opções de arquitetura para escalabilidade de plataforma de mídia social de 10k para 1M usuários ativos diários

# Planejamento de modernização tecnológica
/dev:architecture-scenario-explorer Compare arquiteturas serverless vs container-native para modernização de pipeline de processamento de dados
```

## Indicadores de Qualidade

- **Verde**: Múltiplas arquiteturas analisadas, cobertura abrangente de cenários, trade-offs validados
- **Amarelo**: Algumas opções arquiteturais consideradas, cobertura básica de cenários, trade-offs estimados
- **Vermelho**: Foco em arquitetura única, análise limitada de cenários, suposições não validadas

## Armadilhas Comuns a Evitar

- Architecture astronauting: Over-engineering para requisitos teóricos e não reais
- Cargo cult architecture: Cópia de padrões bem-sucedidos sem entender o contexto
- Viés tecnológico: Escolher arquitetura com base em preferências tecnológicas e não requisitos
- Otimização prematura: Resolver problemas de desempenho que ainda não existem
- Obsessão por escalabilidade: Over-otimizar para escala que pode nunca se materializar
- Cegueira a evolução: Não planejar para mudança e crescimento arquitetural

Transforme decisões arquiteturais de debates baseados em opinião em escolhas sistemáticas, orientadas por evidência, através de exploração abrangente de cenários e análise de trade-offs.