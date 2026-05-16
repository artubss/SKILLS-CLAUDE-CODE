---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [focus-area] | --headers | --auth | --encryption | --infrastructure
description: Fortalecer configuração de segurança da aplicação com controles de segurança abrangentes
---

# Endurecimento de Segurança

Fortalecer configuração de segurança da aplicação e controles: **$ARGUMENTS**

## Postura de Segurança Atual

- Framework: @package.json ou @requirements.txt ou @Cargo.toml (detectar framework)
- Headers de segurança: !`curl -I http://localhost:3000 2>/dev/null | grep -i 'x-\|content-security\|strict-transport' || echo "No server running"`
- Configuração de ambiente: @.env* (verificar variáveis relacionadas a segurança)
- Dependências: !`npm audit --audit-level=moderate 2>/dev/null || echo "Run dependency audit first"`

## Tarefa

Implementar endurecimento de segurança abrangente com base em melhores práticas de segurança:

**Foco do Endurecimento**: Use $ARGUMENTS para direcionar áreas específicas ou aplicar endurecimento abrangente

**Controles de Segurança**:
1. **Autenticação & Autorização** - MFA, RBAC, segurança de sessão, políticas de senha
2. **Validação de Entrada** - Prevenção de XSS, proteção contra injeção SQL, tokens CSRF
3. **Comunicação Segura** - HTTPS/TLS, HSTS, gestão de certificados
4. **Proteção de Dados** - Criptografia em repouso/trânsito, gestão de chaves, armazenamento seguro
5. **Headers de Segurança** - CSP, CORS, headers de resposta de segurança
6. **Segurança de Infraestrutura** - Endurecimento de containers, segmentação de rede, monitoramento

**Saída**: Aplicação endurecida com controles de segurança abrangentes, configuração apropriada e capacidades de monitoramento.