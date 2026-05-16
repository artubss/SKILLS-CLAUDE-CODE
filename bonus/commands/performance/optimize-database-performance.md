---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [tipo-banco-dados] | --postgresql | --mysql | --mongodb
description: Otimize consultas de banco de dados, indexação e desempenho para melhorar tempos de resposta e escalabilidade
---

# Otimizar Desempenho do Banco de Dados

Otimize consultas e desempenho do banco de dados: **$ARGUMENTS**

## Instruções

1. **Análise de Desempenho do Banco de Dados**
   - Analise o desempenho atual do banco de dados e identifique gargalos
   - Revise logs de queries lentas e planos de execução
   - Avalie o design do schema do banco de dados e normalização
   - Avalie a estratégia de indexação e padrões de query
   - Monitore utilização de recursos do banco de dados (CPU, memória, I/O)

2. **Otimização de Queries**
   - Identifique e otimize queries com baixo desempenho
   - Analise planos de execução de queries e estratégias de otimização
   - Reescreva queries para melhor desempenho e eficiência
   - Implemente hints de query e diretivas de otimização
   - Configure timeout de query e limites de recursos

3. **Otimização de Estratégia de Índices**
   - Analise índices existentes e seus padrões de uso
   - Projete estratégia ótima de indexação para padrões de query
   - Crie índices compostos para queries de múltiplas colunas
   - Implemente índices que cobrem para evitar buscas em tabelas
   - Remova índices não utilizados e redundantes

4. **Otimização de Design de Schema**
   - Otimize estrutura de tabelas e tipos de dados
   - Implemente estratégias de desnormalização para workloads de leitura intensiva
   - Projete estratégias de particionamento para tabelas grandes
   - Crie views materializadas para agregações complexas
   - Otimize relacionamentos de chaves estrangeiras e constraints

5. **Otimização de Pool de Conexões**
   - Configure configurações ótimas de connection pooling do banco de dados
   - Ajuste tamanho do pool de conexão e configurações de timeout
   - Implemente monitoramento de conexão e health checks
   - Otimize ciclo de vida de conexões e procedimentos de limpeza
   - Configure segurança de conexão e configurações SSL

6. **Cache de Resultados de Queries**
   - Implemente cache inteligente de resultados de banco de dados
   - Projete estratégias de invalidação de cache para consistência de dados
   - Configure cache em nível de query e result-set
   - Configure políticas de expiração e refresh de cache
   - Monitore efetividade do cache e taxas de acerto

7. **Monitoramento e Profiling do Banco de Dados**
   - Configure monitoramento abrangente de desempenho do banco de dados
   - Monitore desempenho de queries e uso de recursos
   - Rastreie conexões e atividade de sessões do banco de dados
   - Implemente alertas para degradação de desempenho
   - Configure relatórios automatizados de desempenho

8. **Read Replicas e Balanceamento de Carga**
   - Configure read replicas para distribuição de queries
   - Implemente roteamento inteligente de queries de leitura/escrita
   - Configure balanceamento de carga entre instâncias de banco de dados
   - Monitore lag de replicação e consistência
   - Configure procedimentos de failover e recuperação de desastres

9. **Vacuum e Manutenção do Banco de Dados**
   - Implemente procedimentos automatizados de manutenção do banco de dados
   - Configure operações de vacuum e analyze para desempenho ótimo
   - Configure agendas de reconstrução e manutenção de índices
   - Monitore inchaço de tabelas e fragmentação
   - Implemente estratégias automatizadas de limpeza e arquivamento

10. **Testes de Desempenho e Benchmarking**
    - Configure frameworks de testes de desempenho do banco de dados
    - Implemente cenários de testes de carga para workloads realistas
    - Faça benchmark de desempenho de queries sob diferentes condições
    - Teste escalabilidade e limites de capacidade do banco de dados
    - Monitore regressão e melhorias de desempenho

Foque em otimizações do banco de dados que proporcionem as melhorias de desempenho mais significativas para seus padrões específicos de workload. Sempre meça desempenho antes e depois das mudanças para validar as otimizações.