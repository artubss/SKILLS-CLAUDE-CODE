---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [auth-method] | --oauth | --jwt | --mfa | --passwordless
description: Implementar sistema seguro de autenticação de usuários com método escolhido e melhores práticas de segurança
---

# Adicionar Sistema de Autenticação

Implementar sistema seguro de autenticação de usuários: **$ARGUMENTS**

## Estado Atual da Aplicação

- Detecção de framework: @package.json ou @requirements.txt ou @Cargo.toml
- Autenticação existente: !`grep -r "auth\|login\|jwt\|session" src/ --include="*.js" --include="*.py" --include="*.rs" | wc -l`
- Configuração de segurança: @.env* (verificar variáveis relacionadas a autenticação)
- Setup do banco de dados: Verificar modelos de usuário ou tabelas de autenticação

## Tarefa

Implementar sistema abrangente de autenticação com melhores práticas de segurança:

**Métodos de Autenticação**: Escolher entre username/password, OAuth 2.0, JWT, SAML, MFA ou passwordless baseado em $ARGUMENTS

**Áreas de Implementação**:
1. **Gerenciamento de Usuários** - Registro, perfis, políticas de senha, verificação de conta
2. **Fluxo de Autenticação** - Login/logout, gerenciamento de sessão, manipulação de tokens, middleware
3. **Sistema de Autorização** - RBAC, permissões, proteção de rotas, segurança de API
4. **Endurecimento de Segurança** - Hash de senha, rate limiting, proteção CSRF, cookies seguros
5. **Integração** - Componentes frontend, endpoints de API, modelos de banco de dados, middleware

**Padrões de Segurança**: Implementar diretrizes de autenticação OWASP, gerenciamento seguro de sessão e tratamento apropriado de erros.

**Output**: Sistema de autenticação pronto para produção com controles de segurança abrangentes e interface amigável ao usuário.