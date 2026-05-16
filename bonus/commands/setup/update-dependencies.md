---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [estratégia-atualização] | --patch | --minor | --major | --security-only
description: Atualizar e modernizar dependências do projeto com testes abrangentes e verificações de compatibilidade
---

# Atualizar Dependências

Atualizar e modernizar dependências do projeto com verificações de segurança: **$ARGUMENTS**

## Estado Atual das Dependências

- Gerenciador de pacotes: @package.json ou @requirements.txt ou @Cargo.toml (detectar gerenciador de pacotes)
- Pacotes desatualizados: !`npm outdated 2>/dev/null || pip list --outdated 2>/dev/null || echo "Verificação manual necessária"`
- Problemas de segurança: !`npm audit --audit-level=moderate 2>/dev/null || pip check 2>/dev/null || echo "Execute auditoria de segurança"`
- Arquivos de lock: @package-lock.json ou @poetry.lock ou @Cargo.lock

## Tarefa

Atualizar sistematicamente as dependências do projeto com validação abrangente de testes e compatibilidade:

**Estratégia de Atualização**: Use $ARGUMENTS para especificar atualizações de patch, atualizações menores, atualizações maiores ou apenas atualizações de segurança

**Processo de Atualização**:
1. **Análise de Dependências** - Auditar versões atuais, identificar pacotes desatualizados, avaliar vulnerabilidades de segurança
2. **Avaliação de Impacto** - Verificar changelogs, mudanças disruptivas, avisos de descontinuação, matriz de compatibilidade
3. **Atualizações Faseadas** - Aplicar atualizações de patch primeiro, depois menores, finalmente versões maiores com testes entre estágios
4. **Testes e Validação** - Executar suite de testes completa, verificação de build, testes de integração, verificações de desempenho
5. **Estratégia de Reversão** - Documentar mudanças, criar pontos de restauração, manter procedimentos de reversão
6. **Atualizações de Documentação** - Atualizar README, lista de dependências, guias de migração, notificações da equipe

**Recursos de Segurança**: Testes automatizados entre atualizações, resolução de conflitos de dependência, priorização de vulnerabilidades de segurança.

**Saída**: Manifest de dependências atualizado com resultados de testes abrangentes, relatório de auditoria de segurança e documentação de upgrade.