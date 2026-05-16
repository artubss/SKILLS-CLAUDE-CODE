---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [rate-limit-type] | --api | --authentication | --file-upload | --database
description: Implementar rate limiting abrangente de API com algoritmos avançados e políticas específicas do usuário
---

# Configurar Rate Limiting

Implementar sistema abrangente de rate limiting de API com mecanismos de controle avançados: **$ARGUMENTS**

## Estado Atual da API

- Detecção de framework: @package.json ou @requirements.txt (Express, FastAPI, Spring Boot, etc.)
- Rate limiting existente: !`grep -r "rate.limit\|throttle\|rateLimit" src/ 2>/dev/null | wc -l`
- Disponibilidade de Redis: !`redis-cli ping 2>/dev/null || echo "Redis not available"`
- Endpoints da API: !`grep -r "route\|endpoint\|@app\\.route" src/ 2>/dev/null | wc -l`

## Tarefa

Implementar sistema de rate limiting pronto para produção com algoritmos sofisticados e políticas de usuário:

**Tipo de Rate Limiting**: Use $ARGUMENTS para focar em rate limiting de API, limiting de autenticação, controles de upload de arquivo ou limiting de acesso a banco de dados

**Arquitetura de Rate Limiting**:
1. **Implementação de Algoritmo** - Token bucket, sliding window, fixed window, leaky bucket algorithms
2. **Políticas de Usuário** - Limites por tier, autenticado vs anônimo, quotas específicas do usuário, controles baseados em IP
3. **Backend de Armazenamento** - Integração com Redis, rate limiting distribuído, estratégias de persistência, mecanismos de failover
4. **Configuração de Endpoints** - Limites por rota, regras específicas por método, configuração dinâmica, A/B testing
5. **Monitoramento & Analytics** - Rastreamento de uso, detecção de abuso, métricas de desempenho, sistemas de alerta
6. **Mecanismos de Bypass** - Gerenciamento de whitelist, tratamento de requisições internas, overrides de emergência

**Recursos Avançados**: Rate limiting adaptativo, controles baseados em geo-localização, gerenciamento de chave de API, sistemas de quota, prevenção de abuso.

**Prontidão para Produção**: Alta disponibilidade, otimização de desempenho, controles de segurança, monitoramento abrangente.

**Output**: Sistema de rate limiting completo com políticas inteligentes, monitoramento abrangente e capacidades avançadas de prevenção de abuso.