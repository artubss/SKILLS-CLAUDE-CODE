---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [release-type] | --semantic | --conventional-commits | --github-actions | --full-automation
description: Configurar workflows de lançamento automatizado com versionamento semântico, commits convencionais e automação abrangente
---

# Sistema de Lançamento Automatizado

Configurar workflows de lançamento automatizado: $ARGUMENTS

## Análise Atual do Projeto

- Estrutura do projeto: @package.json ou @setup.py ou @go.mod (detectar tipo de projeto)
- Workflows existentes: !`find .github/workflows -name "*.yml" 2>/dev/null | head -3`
- Versionamento atual: @package.json version ou análise de tags git
- Padrões de commit: !`git log --oneline -20 | grep -E "^(feat|fix|docs|style|refactor|test|chore)" | wc -l || echo "0"` commits convencionais
- Histórico de lançamentos: !`git tag -l | wc -l || echo "0"` lançamentos existentes

## Tarefa

Implementar sistema abrangente de lançamento automatizado:

1. **Analisar Estrutura do Repositório**
   - Detectar tipo de projeto (Node.js, Python, Go, etc.)
   - Verificar workflows de CI/CD existentes
   - Identificar abordagem de versionamento atual
   - Revisar processos de lançamento existentes

2. **Criar Rastreamento de Versão**
   - Para Node.js: Usar campo version em package.json
   - Para Python: Usar __version__ em __init__.py ou pyproject.toml
   - Para Go: Usar versão em go.mod
   - Para outros: Criar arquivo version.txt
   - Garantir que a versão siga versionamento semântico (MAJOR.MINOR.PATCH)

3. **Configurar Commits Convencionais**
   - Criar CONTRIBUTING.md com convenções de commit:
     - `feat:` para novas funcionalidades (bumpa versão menor)
     - `fix:` para correções de bugs (bumpa versão patch)
     - `feat!:` ou `BREAKING CHANGE:` para mudanças incompatíveis (bumpa versão maior)
     - `docs:`, `chore:`, `style:`, `refactor:`, `test:` para mudanças sem lançamento
   - Incluir exemplos e diretrizes para cada tipo

4. **Criar Template de Pull Request**
   - Adicionar `.github/pull_request_template.md`
   - Incluir lembrete de commit convencional
   - Adicionar checklist para requisitos comuns
   - Referenciar diretrizes de contribuição

5. **Criar Workflow de Lançamento**
   - Adicionar `.github/workflows/release.yml`:
     - Disparar em push para branch main
     - Analisar commits desde o último lançamento
     - Determinar tipo de bump de versão
     - Atualizar versão no(s) arquivo(s) apropriado(s)
     - Gerar notas de lançamento a partir de commits
     - Atualizar CHANGELOG.md
     - Criar tag git
     - Criar GitHub Release
     - Anexar artefatos de distribuição
   - Incluir opção de disparo manual para lançamentos forçados

6. **Criar Workflow de Validação de PR**
   - Adicionar `.github/workflows/pr-check.yml`:
     - Validar que título do PR segue formato convencional
     - Verificar mensagens de commit
     - Fornecer feedback sobre impacto de versão
     - Executar testes e verificações de qualidade

7. **Configurar Notas de Lançamento do GitHub**
   - Criar `.github/release.yml`
   - Definir categorias para diferentes tipos de mudanças
   - Configurar exclusões de changelog
   - Configurar reconhecimento de contribuidores

8. **Atualizar Documentação**
   - Adicionar badges de lançamento ao README:
     - Badge de versão atual
     - Badge de último lançamento
     - Badge de status de build
   - Documentar processo de lançamento
   - Adicionar link para CONTRIBUTING.md
   - Explicar regras de bump de versão

9. **Configurar Gerenciamento de Changelog**
   - Garantir que CHANGELOG.md siga formato Keep a Changelog
   - Adicionar seção [Unreleased] para mudanças futuras
   - Configurar atualizações automáticas de changelog
   - Configurar categorias de changelog

10. **Configurar Proteção de Branch**
    - Recomendar regras de proteção de branch:
      - Exigir revisões de PR
      - Exigir verificações de status
      - Exigir títulos de PR convencionais
      - Descartar revisões antigas
    - Documentar configurações recomendadas

11. **Adicionar Scanning de Segurança**
    - Configurar Dependabot para atualizações de dependências
    - Configurar alertas de segurança
    - Adicionar política de segurança se necessário

12. **Testar o Sistema**
    - Criar exemplo de PR com título convencional
    - Verificar se verificações de PR funcionam corretamente
    - Testar disparo manual de lançamento
    - Validar geração de changelog

Argumentos: $ARGUMENTS

### Considerações Adicionais

**Para Monorepos:**
- Configurar versionamento independente por pacote
- Configurar changelog por pacote
- Usar escopos em commits convencionais

**Para Bibliotecas:**
- Incluir verificações de compatibilidade de API
- Gerar documentação de API
- Adicionar guias de atualização para mudanças incompatíveis

**Para Aplicações:**
- Incluir versionamento de imagem Docker
- Configurar triggers de deployment
- Adicionar procedimentos de rollback

**Melhores Práticas:**
- Sempre criar branches de lançamento para hotfixes
- Usar release candidates para versões principais
- Manter guias de atualização
- Manter lançamentos pequenos e frequentes
- Documentar procedimentos de rollback

Este sistema de lançamento automatizado fornece:
- ✅ Versionamento consistente
- ✅ Geração automática de changelog
- ✅ Diretrizes de contribuição claras
- ✅ Notas de lançamento profissionais
- ✅ Redução de trabalho manual
- ✅ Melhor manutenibilidade do projeto