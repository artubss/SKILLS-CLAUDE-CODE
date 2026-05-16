---
allowed-tools: Read, Edit, Write, Bash
argument-hint: [version] | [entry-type] [description]
description: Gerar e manter changelog do projeto com formato Keep a Changelog
---

# Adicionar Entrada ao Changelog

Gerar e manter changelog do projeto: $ARGUMENTS

## Estado Atual

- Changelog existente: @CHANGELOG.md (se existe)
- Commits recentes: !`git log --oneline -10`
- Versão atual: !`git describe --tags --abbrev=0 2>/dev/null || echo "No tags found"`
- Versão do package: @package.json (se existe)

## Tarefa

1. **Formato do Changelog (Keep a Changelog)**
   ```markdown
   # Changelog
   
   All notable changes to this project will be documented in this file.
   
   The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
   and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
   
   ## [Unreleased]
   ### Added
   - New features
   
   ### Changed
   - Changes in existing functionality
   
   ### Deprecated
   - Soon-to-be removed features
   
   ### Removed
   - Removed features
   
   ### Fixed
   - Bug fixes
   
   ### Security
   - Security improvements
   ```

2. **Entradas de Versão**
   ```markdown
   ## [1.2.3] - 2024-01-15
   ### Added
   - User authentication system
   - Dark mode toggle
   - Export functionality for reports
   
   ### Fixed
   - Memory leak in background tasks
   - Timezone handling issues
   ```

3. **Ferramentas de Automação**
   ```bash
   # Gerar changelog a partir de commits git
   npm install -D conventional-changelog-cli
   npx conventional-changelog -p angular -i CHANGELOG.md -s
   
   # Auto-changelog
   npm install -D auto-changelog
   npx auto-changelog
   ```

4. **Convenção de Commits**
   ```bash
   # Conventional commits para auto-geração
   feat: add user authentication
   fix: resolve memory leak in tasks
   docs: update API documentation
   style: format code with prettier
   refactor: reorganize user service
   test: add unit tests for auth
   chore: update dependencies
   ```

5. **Integração com Releases**
   - Atualizar changelog antes de cada release
   - Incluir nas notas da release
   - Vincular a releases do GitHub
   - Marcar versões de forma consistente

Lembre-se de manter as entradas claras, categorizadas e focadas em mudanças visíveis ao usuário.