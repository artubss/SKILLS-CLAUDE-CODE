---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [escopo] | --api-keys | --passwords | --certificates | --fix
description: Verifica a base de código em busca de segredos, credenciais e informações sensíveis expostos
---

# Scanner de Segredos

Verifica a base de código em busca de segredos e informações sensíveis expostos: **$ARGUMENTS**

## Estado Atual do Repositório

- Status Git: !`git status --porcelain | wc -l` arquivos não commitados
- Tipos de arquivo: !`find . -name "*.js" -o -name "*.py" -o -name "*.env*" -o -name "*.yml" | wc -l` verificáveis
- Commits recentes: !`git log --oneline --grep="password\|key\|secret\|token" -5`
- Arquivos de ambiente: @.env* ou @config/* (se existirem)

## Tarefa

Execute detecção e remediação abrangente de segredos em toda a base de código:

**Escopo de Verificação**: Use $ARGUMENTS para focar em chaves API, senhas, certificados ou verificação completa

**Categorias de Detecção**:
1. **Chaves e Tokens de API** - GitHub, AWS, Google Cloud, Stripe, serviços de terceiros
2. **Credenciais de Banco de Dados** - Strings de conexão, nomes de usuário, senhas
3. **Certificados e Chaves** - Chaves privadas, chaves SSH, certificados SSL
4. **Segredos de Autenticação** - Segredos JWT, chaves de sessão, credenciais OAuth
5. **Vazamentos de Configuração** - URLs hardcoded, endpoints internos, configurações de debug

**Ações de Remediação**:
- Identifique segredos expostos com localizações de arquivo e números de linha
- Forneça alternativas seguras (variáveis de ambiente, gerenciamento de segredos)
- Gere entradas .gitignore para arquivos sensíveis
- Crie templates de configuração segura
- Implemente melhores práticas de gerenciamento de segredos

**Saída**: Relatório de segurança detalhado com níveis de risco, ações imediatas e melhorias de segurança de longo prazo.