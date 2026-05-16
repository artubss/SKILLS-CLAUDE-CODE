# Simulador de Comportamento de Sistema

Simule o desempenho do sistema sob várias cargas com planejamento de capacidade, identificação de gargalos e estratégias de otimização.

## Instruções

Você tem a tarefa de criar simulações abrangentes de comportamento do sistema para prever desempenho, identificar gargalos e otimizar o planejamento de capacidade. Siga esta abordagem: **$ARGUMENTS**

### 1. Avaliação de Pré-requisitos

**Validação Crítica do Contexto do Sistema:**

- **Arquitetura do Sistema**: Que tipo de sistema você está simulando?
- **Objetivos de Desempenho**: Quais são as métricas de desempenho alvo e SLAs?
- **Características de Carga**: Quais são os padrões de uso esperados e perfis de tráfego?
- **Restrições de Recursos**: Quais limitações de infraestrutura e orçamento se aplicam?
- **Objetivos de Otimização**: Quais aspectos de desempenho são mais críticos para otimizar?

**Se o contexto for pouco claro, guie sistematicamente:**

```
Arquitetura do Sistema Ausente:
"Que tipo de sistema precisa de simulação de comportamento?
- Aplicação Web: Aplicação voltada para usuários com padrões de tráfego HTTP
- Serviço API: Serviço backend com padrões de acesso programático
- Processamento de Dados: Requisitos de processamento em lote ou stream
- Sistema de Banco de Dados: Otimização de armazenamento e processamento de consultas
- Microsserviços: Sistema distribuído com comunicação entre serviços

Por favor, especifique componentes do sistema, stack de tecnologia e arquitetura de deployment."

Objetivos de Desempenho Ausentes:
"Quais objetivos de desempenho precisam ser atendidos?
- Tempo de Resposta: Latência alvo para requisições de usuários (p50, p95, p99)
- Throughput: Requisições por segundo ou transações por minuto
- Disponibilidade: Metas de uptime e requisitos de tolerância a falhas
- Escalabilidade: Crescimento de usuários e capacidades de manipulação de carga
- Eficiência de Recursos: Otimização de CPU, memória, armazenamento e rede"
```

### 2. Modelagem da Arquitetura do Sistema

**Mapeie sistematicamente componentes e interações do sistema:**

#### Framework de Arquitetura de Componentes
```
Mapeamento de Componentes do Sistema:

Camada de Aplicação:
- Componentes Frontend: Interfaces de usuário, aplicações single-page, apps móveis
- Serviços de Aplicação: Lógica de negócio, processamento de workflow, endpoints de API
- Serviços em Background: Jobs agendados, processamento de mensagens, operações em lote
- Serviços de Integração: Chamadas de API externa, tratamento de webhooks, sincronização de dados

Camada de Dados:
- Bancos de Dados Principais: Armazenamento de dados transacionais e processamento de consultas
- Sistemas de Cache: Redis, Memcached, CDN e cache em nível de aplicação
- Filas de Mensagens: Comunicação assíncrona e processamento de eventos
- Sistemas de Busca: Elasticsearch, Solr ou capacidades de busca de banco de dados

Camada de Infraestrutura:
- Load Balancers: Distribuição de tráfego e verificação de saúde
- Servidores Web: Tratamento de requisições HTTP e entrega de conteúdo estático
- Servidores de Aplicação: Geração de conteúdo dinâmico e lógica de negócio
- Componentes de Rede: Firewalls, VPNs e roteamento de tráfego
```

#### Modelagem de Padrões de Interação
```
Análise de Interação do Sistema:

Interações Síncronas:
- Requisição-Resposta: Chamadas de API diretas e consultas de banco de dados
- Service Mesh: Comunicação entre serviços com descoberta de serviço
- Transações de Banco de Dados: Conformidade ACID e mecanismos de bloqueio
- Chamadas de API Externa: Dependências de serviços terceirizados e timeouts

Interações Assíncronas:
- Filas de Mensagens: Padrões pub/sub e processamento orientado a eventos
- Streams de Eventos: Processamento de dados em tempo real e analytics
- Jobs em Background: Tarefas agendadas e processamento atrasado
- Webhooks: Notificações de sistemas externos e callbacks

Padrões de Fluxo de Dados:
- Padrões de Leitura: Otimização de consultas e estratégias de cache
- Padrões de Escrita: Ingestão de dados e gerenciamento de consistência
- Processamento em Lote: Operações ETL e processamento de pipeline de dados
- Processamento em Tempo Real: Stream processing e analytics ao vivo
```

### 3. Framework de Modelagem de Carga

**Crie simulações realistas de padrões de tráfego e uso:**

#### Análise de Padrões de Tráfego
```
Modelagem de Características de Carga:

Padrões de Comportamento de Usuário:
- Padrões Diários: Horários de pico, quedas de almoço, mínimos noturnos
- Padrões Semanais: Variações entre semana vs fim de semana
- Padrões Sazonais: Tráfego de feriados, flutuações do ciclo de negócios
- Spikes Orientados a Eventos: Campanhas de marketing, conteúdo viral, notícias

Distribuição de Requisições:
- Distribuição Geográfica: Tráfego multi-região e padrões de latência
- Distribuição de Dispositivos: Uso de móvel vs desktop vs API
- Distribuição de Features: Uso de features populares vs nicho
- Distribuição de Tipos de Usuário: Comportamentos de usuários novos vs retornantes vs power users

Escalabilidade de Volume de Carga:
- Usuários Concorrentes: Sessões ativas simultâneas e padrões de requisição
- Taxa de Requisições: Transações por segundo com capacidades de burst
- Volume de Dados: Tamanhos de payload e requisitos de transferência de dados
- Padrões de Conexão: Duração de sessão e connection pooling
```

#### Geração de Carga Sintética
```
Framework de Cenários de Teste de Carga:

Teste de Carga Base:
- Tráfego Normal: Padrões de uso típicos diários e volumes de requisição
- Carga Sustentada: Tráfego constante por períodos estendidos
- Rampa Gradual: Aumento lento de tráfego para identificar pontos de escala
- Estado Estável: Carga estável para estabelecimento de baseline de desempenho

Teste de Stress:
- Carga de Pico: Tráfego máximo esperado durante períodos ocupados
- Teste de Capacidade: Limites do sistema e identificação de ponto de ruptura
- Teste de Spike: Aumentos repentinos de tráfego e comportamento de recuperação
- Teste de Volume: Conjuntos de dados grandes e cenários de alto throughput

Teste de Resiliência:
- Cenários de Falha: Indisponibilidade de componentes e comportamento de serviço degradado
- Teste de Recuperação: Restauração do sistema e recuperação de desempenho
- Engenharia do Caos: Injeção aleatória de falhas e adaptação do sistema
- Simulação de Desastre: Cenários de indisponibilidade maior e continuidade de negócio
```

### 4. Motor de Modelagem de Desempenho

**Crie previsões abrangentes de desempenho do sistema:**

#### Framework de Métricas de Desempenho
```
Análise de Desempenho Multi-Dimensional:

Métricas de Tempo de Resposta:
- Latência de Requisição: Medição de tempo de resposta end-to-end
- Tempo de Processamento: Duração de execução de lógica de aplicação
- Tempo de Consulta de Banco de Dados: Desempenho de acesso e recuperação de dados
- Latência de Rede: Overhead de comunicação e utilização de largura de banda

Métricas de Throughput:
- Requisições por Segundo: Capacidade de tratamento de requisições HTTP
- Transações por Minuto: Taxa de conclusão de operações de negócio
- Taxa de Processamento de Dados: Throughput de jobs em lote e stream processing
- Capacidade de Usuários Concorrentes: Capacidade de tratamento de sessões simultâneas

Métricas de Utilização de Recursos:
- Uso de CPU: Consumo de poder de processamento e eficiência
- Uso de Memória: Alocação de RAM e impacto de coleta de lixo
- I/O de Armazenamento: Desempenho de leitura/escrita em disco e capacidade
- Largura de Banda de Rede: Taxas de transferência de dados e gerenciamento de congestionamento

Métricas de Qualidade:
- Taxas de Erro: Requisições falhadas e falhas de transação
- Disponibilidade: Uptime do sistema e confiabilidade do serviço
- Consistência: Integridade de dados e isolamento de transação
- Segurança: Overhead de autenticação, autorização e proteção de dados
```

#### Modelagem de Previsão de Desempenho
```
Framework de Previsão de Desempenho:

Modelos Analíticos:
- Teoria de Filas: Modelagem matemática de tempo de espera e taxa de serviço
- Lei de Little: Relação entre concorrência, throughput e latência
- Planejamento de Capacidade: Previsão de requisitos de recursos e otimização
- Análise de Gargalos: Identificação e resolução de restrições do sistema

Modelos de Simulação:
- Simulação de Eventos Discretos: Modelagem de comportamento do sistema com filas de eventos
- Simulação Monte Carlo: Análise probabilística de resultados de desempenho
- Dados de Teste de Carga: Extrapolação de padrões de desempenho históricos
- Machine Learning: Reconhecimento de padrões e analytics preditiva

Modelos Híbridos:
- Analítico + Empírico: Modelos matemáticos calibrados com dados reais
- Modelagem Multi-Camada: Modelos em nível de componente agregados para nível de sistema
- Adaptação Dinâmica: Modelos que se ajustam com base em desempenho em tempo real
- Baseado em Cenários: Diferentes modelos para diferentes padrões de carga e uso
```

### 5. Sistema de Identificação de Gargalos

**Identifique e analise sistematicamente restrições de desempenho:**

#### Framework de Detecção de Gargalos
```
Análise de Restrições de Desempenho:

Gargalos de CPU:
- Alta Utilização de CPU: Operações intensivas em processamento e algoritmos
- Contenção de Threads: Overhead de sincronização e locking
- Context Switching: Criação excessiva de threads e gerenciamento
- Algoritmos Ineficientes: Fraca complexidade de tempo e oportunidades de otimização

Gargalos de Memória:
- Memory Leaks: Consumo gradual de memória e pressão de coleta de lixo
- Alocação de Objetos Grandes: Operações intensivas em memória e estratégias de cache
- Fragmentação de Memória: Padrões de alocação e gerenciamento de pool de memória
- Cache Misses: Efetividade de cache de aplicação e banco de dados

Gargalos de I/O:
- Desempenho de Banco de Dados: Otimização de consultas e efetividade de índices
- I/O de Disco: Padrões de acesso de armazenamento e limites de desempenho de disco
- I/O de Rede: Limitações de largura de banda e otimização de latência
- Dependências Externas: Tempos de resposta de serviços terceirizados e confiabilidade

Gargalos de Aplicação:
- Operações de Bloqueio: Chamadas síncronas e esgotamento de thread pool
- Código Ineficiente: Algoritmos pobres e processamento desnecessário
- Contenção de Recursos: Acesso a recursos compartilhados e mecanismos de locking
- Problemas de Configuração: Configurações subótimas e ajuste de parâmetros
```

#### Análise de Causa Raiz
- Profiling e análise de rastreamento de desempenho
- Análise de correlação entre métricas e gargalos
- Reconhecimento de padrões históricos e análise de tendência
- Análise comparativa entre diferentes configurações de sistema

### 6. Geração de Estratégia de Otimização

**Crie abordagens sistemáticas de melhoria de desempenho:**

#### Framework de Otimização de Desempenho
```
Estratégias de Otimização Multi-Nível:

Otimizações em Nível de Código:
- Otimização de Algoritmo: Complexidade de tempo e espaço melhorada
- Otimização de Consultas de Banco de Dados: Uso de índices e melhoria de plano de consulta
- Estratégias de Cache: Cache de aplicação, banco de dados e CDN
- Processamento Assíncrono: Operações não-bloqueantes e paralelização

Otimizações em Nível de Arquitetura:
- Escalabilidade Horizontal: Distribuição de carga entre múltiplas instâncias
- Escalabilidade Vertical: Alocação de recursos e aumentos de capacidade
- Camadas de Cache: Cache multi-tier e estratégias de invalidação de cache
- Sharding de Banco de Dados: Particionamento de dados e armazenamento distribuído

Otimizações em Nível de Infraestrutura:
- Auto-Scaling: Alocação dinâmica de recursos com base em demanda
- Load Balancing: Distribuição de tráfego e otimização de verificação de saúde
- Implementação de CDN: Distribuição geográfica de conteúdo e cache de borda
- Otimização de Rede: Alocação de largura de banda e redução de latência

Otimizações em Nível de Sistema:
- Monitoramento e Alertas: Visibilidade de desempenho e detecção proativa de problemas
- Planejamento de Capacidade: Previsão de recursos e acomodação de crescimento
- Recuperação de Desastre: Estratégias de backup e mecanismos de failover
- Otimização de Segurança: Implementação de segurança ciente de desempenho
```

#### Análise de Custo-Benefício
- Quantificação e medição de melhoria de desempenho
- Implicações de custo de infraestrutura e otimização de orçamento
- Estimativa de esforço de desenvolvimento e alocação de recursos
- Cálculo de ROI para diferentes estratégias de otimização

### 7. Integração de Planejamento de Capacidade

**Conecte insights de desempenho ao planejamento de infraestrutura e recursos:**

#### Framework de Planejamento de Capacidade
```
Gerenciamento Sistemático de Capacidade:

Projeção de Crescimento:
- Crescimento de Usuários: Aquisição de clientes e evolução de padrão de uso
- Crescimento de Dados: Requisitos de armazenamento e aumentos de volume de processamento
- Crescimento de Features: Novos recursos e impactos de funcionalidade
- Crescimento Geográfico: Expansão multi-região e requisitos de latência

Previsão de Recursos:
- Recursos de Computação: Requisitos de CPU, memória e poder de processamento
- Recursos de Armazenamento: Capacidade de banco de dados, sistema de arquivos e backup
- Recursos de Rede: Largura de banda, conectividade e otimização de latência
- Recursos Humanos: Escalabilidade da equipe e desenvolvimento de expertise

Estratégia de Escalabilidade:
- Escalabilidade Horizontal: Multiplicação de instâncias e distribuição de carga
- Escalabilidade Vertical: Aprimoramento de recursos e aumentos de capacidade
- Auto-Scaling: Ajuste dinâmico com base em demanda em tempo real
- Escalabilidade Manual: Aumentos de capacidade planejados e janelas de manutenção

Otimização de Custo:
- Capacidade Reservada: Compromisso de longo prazo de recursos e economia de custo
- Instâncias Spot: Preço variável e capacidade temporária custo-efetiva
- Right-Sizing: Alocação ótima de recursos e eliminação de desperdício
- Multi-Cloud: Comparação de provedores e oportunidades de arbitragem de custo
```

### 8. Geração de Output e Recomendações

**Apresente insights de simulação em formato de otimização de desempenho acionável:**

```
## Simulação de Comportamento do Sistema: [Nome do Sistema]

### Resumo de Desempenho
- Capacidade Atual: [métricas de desempenho de baseline]
- Análise de Gargalos: [principais restrições de desempenho identificadas]
- Potencial de Otimização: [oportunidades de melhoria e ganhos esperados]
- Requisitos de Escalabilidade: [necessidades de recursos para acomodação de crescimento]

### Resultados de Teste de Carga

| Cenário | Throughput | Latência (p95) | Taxa de Erro | Uso de Recursos |
|----------|------------|----------------|--------------|-----------------|
| Carga Normal | 500 RPS | 200ms | 0,1% | 60% CPU |
| Carga de Pico | 1000 RPS | 800ms | 2,5% | 85% CPU |
| Teste de Stress | 1500 RPS | 2000ms | 15% | 95% CPU |

### Análise de Gargalos
- Gargalo Principal: [fator de desempenho mais limitante]
- Gargalos Secundários: [restrições adicionais afetando desempenho]
- Efeitos em Cascata: [como gargalos impactam outros componentes do sistema]
- Prioridade de Resolução: [ordem recomendada de resolução de gargalos]

### Recomendações de Otimização

#### Otimizações Imediatas (0-30 dias):
- Quick Wins: [melhorias de baixo esforço e alto impacto]
- Ajuste de Configuração: [ajustes de parâmetros e otimização de configurações]
- Otimização de Consulta: [melhorias de consulta de banco de dados e aplicação]
- Implementação de Cache: [adições estratégicas de camada de cache]

#### Otimizações Médio Prazo (1-6 meses):
- Mudanças de Arquitetura: [melhorias estruturais e estratégias de escala]
- Upgrades de Infraestrutura: [melhorias de hardware e plataforma]
- Refatoração de Código: [otimização de aplicação e melhoria de eficiência]
- Aprimoramento de Monitoramento: [melhorias de observabilidade e sistema de alertas]

#### Otimizações Longo Prazo (6+ meses):
- Migração de Tecnologia: [modernização de plataforma ou framework]
- Redesenho de Sistema: [melhorias fundamentais de arquitetura]
- Expansão de Capacidade: [escalabilidade de infraestrutura e distribuição geográfica]
- Integração de Inovação: [adoção de nova tecnologia e vantagem competitiva]

### Planejamento de Capacidade
- Capacidade Atual: [limites de sistema existentes e margem de segurança]
- Acomodação de Crescimento: [escalabilidade de recursos para demanda projetada]
- Implicações de Custo: [requisitos de orçamento para aumentos de capacidade]
- Requisitos de Timeline: [cronograma de implementação para melhorias de capacidade]

### Estratégia de Monitoramento e Alertas
- Indicadores-Chave de Desempenho: [métricas críticas para monitoramento contínuo]
- Limiares de Alerta: [níveis de aviso de degradação de desempenho]
- Procedimentos de Escalação: [protocolos de resposta para problemas de desempenho]
- Cronograma de Revisão Regular: [otimização contínua e avaliação de capacidade]
```

### 9. Aprendizado Contínuo de Desempenho

**Estabeleça refinamento contínuo de simulação e otimização de sistema:**

#### Validação de Desempenho
- Comparação de desempenho em tempo real com previsões de simulação
- Medição e validação da efetividade de otimização
- Correlação de experiência do usuário com métricas de desempenho do sistema
- Avaliação de impacto de negócio de melhorias de desempenho

#### Aprimoramento de Modelo
- Melhoria de precisão de simulação com base em comportamento real do sistema
- Refinamento de padrão de carga e modelagem de comportamento de usuário
- Aprimoramento de previsão de gargalos e sistemas de alerta antecipado
- Rastreamento e melhoria de efetividade de estratégia de otimização

## Exemplos de Uso

```bash
# Simulação de desempenho de aplicação web
/performance:system-behavior-simulator Simule o desempenho de plataforma de e-commerce sob tráfego de Black Friday com 10x a carga normal

# Análise de escalabilidade de serviço API
/performance:system-behavior-simulator Modele o desempenho de API REST para app móvel com 1M+ usuários ativos diários e distribuição geográfica

# Otimização de desempenho de banco de dados
/performance:system-behavior-simulator Simule o desempenho de banco de dados para carga de trabalho de analytics com requisitos de relatório em tempo real

# Planejamento de capacidade de microsserviços
/performance:system-behavior-simulator Modele o desempenho de mesh de microsserviços sob vários cenários de falha e condições de auto-scaling
```

## Indicadores de Qualidade

- **Verde**: Modelagem de carga abrangente, análise de gargalo validada, estratégias de otimização quantificadas
- **Amarelo**: Boa cobertura de carga, identificação básica de gargalo, benefícios de otimização estimados
- **Vermelho**: Cenários de carga limitados, gargalos não validados, sugestões de otimização apenas qualitativas

## Armadilhas Comuns a Evitar

- Irrealismo de carga: Teste com padrões artificiais que não correspondem ao uso real
- Visão de túnel de gargalo: Foco em restrições únicas ignorando outras
- Otimização prematura: Otimizar para problemas que ainda não existem
- Sub-planejamento de capacidade: Não considerar crescimento e spikes de tráfego
- Cegueira de monitoramento: Não estabelecer visibilidade contínua de desempenho
- Ignorância de custo: Otimizar desempenho sem considerar restrições de orçamento

Transforme o desempenho do sistema de combate a incêndios reativo em otimização proativa e orientada por dados através de simulação abrangente de comportamento e planejamento de capacidade.