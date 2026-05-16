---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [target] | --web-app | --api | --auth | --full-scan
description: Realizar teste de penetração e avaliação de vulnerabilidades em aplicação
---

# Teste de Penetração

Realizar teste de penetração e avaliação de vulnerabilidades: **$ARGUMENTS**

## Contexto da Aplicação

- Serviços em execução: !`netstat -tlnp 2>/dev/null | grep LISTEN | head -10 || lsof -i -P | grep LISTEN | head -10`
- Framework web: @package.json ou @requirements.txt (detectar framework e versão)
- Endpoints de API: !`grep -r "route\|endpoint\|@app\\.route\|@RequestMapping" src/ 2>/dev/null | wc -l`
- Autenticação: !`grep -r "auth\|login\|jwt\|session" src/ 2>/dev/null | wc -l`

## Tarefa

Conduzir teste de penetração sistemático seguindo metodologias de hacking ético:

**Alvo do Teste**: Usar $ARGUMENTS para focar em aplicação web, API, autenticação ou teste abrangente

**Fases de Teste**:
1. **Reconhecimento** - Descoberta de serviços, fingerprinting de tecnologias, mapeamento de superfície de ataque
2. **Avaliação de Vulnerabilidades** - OWASP Top 10, falhas de injeção, autenticação quebrada
3. **Teste de Exploração** - XSS, CSRF, SQL injection, tentativas de escalação de privilégio
4. **Teste de Autenticação** - Força bruta, gerenciamento de sessão, bypasses de autorização
5. **Teste de Segurança de API** - Validação de entrada, rate limiting, bypass de autenticação
6. **Teste de Infraestrutura** - Segurança de rede, segurança de container, problemas de configuração

**Metodologia de Teste**:
- Seguir OWASP Testing Guide e diretrizes NIST
- Usar técnicas automatizadas e testes manuais
- Documentar todos os achados com exemplos de proof-of-concept
- Fornecer recomendações de remediação para cada vulnerabilidade
- Manter limites éticos e evitar danos a dados

**Saída**: Relatório abrangente de teste de penetração com resumo executivo, achados detalhados, classificações de risco e roadmap de remediação.