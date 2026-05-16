---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [área-de-foco] | --full
description: Realizar avaliação abrangente de segurança e análise de vulnerabilidades
---

# Auditoria de Segurança

Realizar avaliação abrangente de segurança: $ARGUMENTS

## Ambiente Atual

- Scan de dependências: !`npm audit --audit-level=moderate 2>/dev/null || pip check 2>/dev/null || echo "No package manager detected"`
- Arquivos de ambiente: @.env* (se existir)
- Configuração de segurança: @.github/workflows/security.yml ou @security/ (se existir)
- Commits recentes: !`git log --oneline --grep="security\|fix" -10`

## Tarefa

Realizar auditoria de segurança sistemática seguindo estas etapas:

1. **Configuração do Ambiente**
   - Identificar o stack de tecnologia e framework
   - Verificar ferramentas e configurações de segurança existentes
   - Revisar setup de deployment e infraestrutura

2. **Segurança de Dependências**
   - Scan de todas as dependências para vulnerabilidades conhecidas
   - Verificar pacotes desatualizados com problemas de segurança
   - Revisar fontes de dependências e integridade
   - Usar ferramentas apropriadas: `npm audit`, `pip check`, `cargo audit`, etc.

3. **Autenticação & Autorização**
   - Revisar mecanismos de autenticação e implementação
   - Verificar gerenciamento de sessão apropriado
   - Validar controles de autorização e restrições de acesso
   - Examinar políticas de senha e armazenamento

4. **Validação & Sanitização de Entrada**
   - Verificar validação e sanitização de toda entrada de usuário
   - Identificar vulnerabilidades de SQL Injection
   - Localizar problemas potenciais de XSS (Cross-Site Scripting)
   - Revisar segurança e validação de upload de arquivos

5. **Proteção de Dados**
   - Identificar práticas de manipulação de dados sensíveis
   - Verificar implementação de criptografia para dados em repouso e em trânsito
   - Revisar práticas de mascaramento e anonimização de dados
   - Validar protocolos de comunicação segura (HTTPS, TLS)

6. **Gestão de Segredos**
   - Scan de segredos hardcoded, chaves de API e senhas
   - Verificar práticas apropriadas de gestão de segredos
   - Revisar segurança de variáveis de ambiente
   - Identificar arquivos de configuração expostos

7. **Tratamento de Erros & Logging**
   - Revisar mensagens de erro para divulgação de informações
   - Verificar práticas de logging para eventos de segurança
   - Validar que dados sensíveis não são registrados em logs
   - Avaliar robustez do tratamento de erros

8. **Segurança da Infraestrutura**
   - Revisar segurança de containerização (Docker, etc.)
   - Verificar segurança do pipeline CI/CD
   - Examinar configuração de nuvem e permissões
   - Avaliar configurações de segurança de rede

9. **Headers de Segurança & CORS**
   - Verificar implementação de headers de segurança
   - Revisar configuração de CORS
   - Validar configurações de CSP (Content Security Policy)
   - Examinar atributos de segurança de cookies

10. **Relatório**
    - Documentar todas as descobertas com níveis de severidade (Crítico, Alto, Médio, Baixo)
    - Fornecer passos específicos de remediação para cada problema
    - Incluir exemplos de código e referências de arquivos
    - Criar um resumo executivo com principais recomendações

Use ferramentas automatizadas de scan de segurança quando disponíveis e forneça revisão manual para padrões de segurança complexos.