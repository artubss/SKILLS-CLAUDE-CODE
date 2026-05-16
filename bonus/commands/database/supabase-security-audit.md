---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [audit-scope] | --rls | --permissions | --auth | --api-keys | --comprehensive
description: Realizar auditoria de segurança abrangente do Supabase com análise de RLS e avaliação de vulnerabilidades
---

# Auditoria de Segurança do Supabase

Realizar auditoria de segurança abrangente do Supabase com análise de políticas RLS e avaliação de vulnerabilidades: **$ARGUMENTS**

## Contexto de Segurança Atual

- Acesso Supabase: Integração MCP para análise de segurança e revisão de políticas
- Políticas RLS: Implementação atual de Row Level Security e efetividade das políticas
- Configuração de autenticação: !`find . -name "*auth*" -o -name "*supabase*" | grep -E "\\.(js|ts|json)$" | head -5` setup de autenticação
- Segurança de API: Implementação atual de gerenciamento de chaves de API e controle de acesso

## Tarefa

Executar auditoria de segurança abrangente com avaliação de vulnerabilidades e otimização de políticas:

**Escopo da Auditoria**: Use $ARGUMENTS para focar em políticas RLS, análise de permissões, segurança de autenticação, gerenciamento de chaves de API ou revisão de segurança abrangente

**Framework de Auditoria de Segurança**:
1. **Análise de Políticas RLS** - Revisar políticas de Row Level Security, testar efetividade das políticas, identificar lacunas, otimizar desempenho das políticas
2. **Avaliação de Permissões** - Analisar permissões de tabelas, revisar controle de acesso baseado em papéis, validar hierarquias de permissão, identificar acessos com privilégios excessivos
3. **Segurança de Autenticação** - Revisar configuração de autenticação, analisar segurança de JWT, validar gerenciamento de sessão, avaliar autenticação multifator
4. **Gerenciamento de Chaves de API** - Auditar uso de chaves de API, revisar políticas de rotação de chaves, validar escopo de chaves, avaliar riscos de exposição
5. **Proteção de Dados** - Analisar tratamento de dados sensíveis, revisar implementação de criptografia, validar mascaramento de dados, avaliar segurança de backups
6. **Varredura de Vulnerabilidades** - Identificar vulnerabilidades de segurança, avaliar riscos de injeção, revisar configuração CORS, validar rate limiting

**Recursos Avançados**: Testes de segurança automatizados, simulação de políticas, pontuação de vulnerabilidades, verificação de conformidade, configuração de monitoramento de segurança.

**Integração de Conformidade**: Verificação de conformidade com GDPR, validação de requisitos SOC2, aplicação de melhores práticas de segurança, análise de trilha de auditoria.

**Output**: Relatório abrangente de auditoria de segurança com avaliações de vulnerabilidades, recomendações de políticas, melhorias de segurança e validação de conformidade.