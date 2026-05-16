---
allowed-tools: Ler, Escrever, Editar, Bash
argument-hint: [tipo-versão] | --patch | --minor | --major | --prerelease
description: Preparar e executar lançamento do projeto com gerenciamento de versão e atualização de changelog
---

# Lançamento do Projeto

Atualize CHANGELOG.md com as mudanças desde o último aumento de versão. Verifique nosso README.md para qualquer mudança necessária. Verifique o escopo das mudanças desde o último lançamento e aumente nosso número de versão conforme apropriado: **$ARGUMENTS**

## Estado Atual do Projeto

- Status do Git: !`git status --porcelain`
- Versão atual: !`git describe --tags --abbrev=0 2>/dev/null || echo "Sem tags anteriores"`
- Commits recentes: !`git log --oneline --since="1 month ago" | head -10`
- Informações do pacote: @package.json ou @setup.py ou @Cargo.toml (se existir)

## Tarefa

Prepare um lançamento do projeto seguindo estas etapas:

1. **Analise as Mudanças**: Revise o histórico do git desde o último lançamento para determinar o incremento de versão apropriado
2. **Atualize a Versão**: Atualize a versão em package.json, setup.py ou outros arquivos de versão conforme versionamento semântico
3. **Atualize o Changelog**: Adicione novas entradas em CHANGELOG.md com categorização apropriada (Adicionado, Alterado, Corrigido, etc.)
4. **Atualize a Documentação**: Revise e atualize README.md se necessário para novos recursos ou mudanças
5. **Crie o Lançamento**: Adicione tag ao lançamento e prepare as notas de lançamento

Se o tipo de versão for especificado em $ARGUMENTS, use esse incremento. Caso contrário, analise as mudanças e sugira versionamento apropriado.

Concentre-se em manter versionamento semântico apropriado e documentação clara do changelog.