---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [testing-type] | --capacity | --stress | --spike | --endurance | --volume
description: Configure testes de carga abrangentes com métricas de desempenho e identificação de gargalos
---

# Configurar Testes de Carga

Configure testes de carga abrangentes com análise de desempenho e identificação de gargalos: **$ARGUMENTS**

## Contexto de Desempenho Atual

- Tipo de aplicação: !`find . -name "server.js" -o -name "app.py" -o -name "main.go" | head -1 && echo "Aplicação servidor" || echo "Detectar tipo de app"`
- Endpoints de API: !`grep -r "app\\.get\\|app\\.post\\|@RequestMapping" . 2>/dev/null | wc -l` endpoints detectados
- Banco de dados: !`find . -name "*.sql" -o -name "database.js" | head -1 && echo "Banco de dados detectado" || echo "Nenhum arquivo de banco de dados"`
- Monitoramento atual: !`find . -name "prometheus.yml" -o -name "newrelic.js" | head -1 || echo "Nenhum monitoramento detectado"`

## Tarefa

Implemente testes de carga abrangentes com otimização de desempenho e análise de gargalos:

**Tipo de Teste**: Use $ARGUMENTS para focar em planejamento de capacidade, testes de estresse, testes de pico, testes de resistência ou testes de volume

**Framework de Testes de Carga**:

1. **Estratégia & Requisitos** - Analisar arquitetura da aplicação, definir objetivos de teste, determinar cenários, identificar métricas de desempenho
2. **Seleção & Setup de Ferramentas** - Escolher ferramentas apropriadas (k6, Artillery, JMeter, Gatling), instalar dependências, configurar ambientes
3. **Design de Cenários de Teste** - Criar cenários realistas de usuário, implementar scripts de teste de API, configurar geração de dados, projetar padrões de carga
4. **Métricas de Desempenho** - Configurar monitoramento de tempo de resposta, medição de throughput, rastreamento de taxa de erro, monitoramento de utilização de recursos
5. **Setup de Infraestrutura** - Configurar ambientes de teste, setup de dashboards de monitoramento, implementar coleta de resultados, otimizar execução de testes
6. **Análise & Otimização** - Identificar gargalos de desempenho, analisar restrições de recursos, recomendar otimizações, rastrear melhorias

**Recursos Avançados**: Geração de carga distribuída, monitoramento em tempo real, detecção automatizada de regressão de desempenho, integração com CI/CD, engenharia do caos.

**Garantia de Qualidade**: Confiabilidade de teste, precisão de resultados, consistência do ambiente, completude de monitoramento.

**Output**: Setup completo de testes de carga com cenários configurados, monitoramento de desempenho, análise de gargalos e recomendações de otimização.