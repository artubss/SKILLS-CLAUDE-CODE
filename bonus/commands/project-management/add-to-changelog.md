---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [version] [change-type] [message] | --added | --changed | --fixed
description: Adiciona entrada ao changelog do projeto seguindo o formato Keep a Changelog
---

# Atualizar Changelog

Adicione uma nova entrada ao arquivo CHANGELOG.md do projeto: **$ARGUMENTS**

## Exemplos de Uso
- `/add-to-changelog 1.1.0 added "Novo recurso de conversão markdown para BlockDoc"`
- `/add-to-changelog 1.0.2 fixed "Bug no renderizador HTML causando saída incorreta"`

## Estado Atual do Changelog

- Changelog existente: @CHANGELOG.md (se existir)
- Arquivos de versão do projeto: @package.json ou @setup.py (se existirem)

## Tarefa

Adicione a entrada de mudança especificada ao CHANGELOG.md:

**Argumentos**: 
- Versão: Primeiro argumento (ex: "1.1.0")
- Tipo de Mudança: Segundo argumento (added/changed/deprecated/removed/fixed/security)  
- Mensagem: Terceiro argumento (descrição da mudança)

**Requisitos**:
1. Criar CHANGELOG.md com cabeçalho padrão se não existir
2. Encontrar ou criar seção de versão com a data atual
3. Adicionar entrada sob a seção de tipo de mudança apropriada
4. Seguir o formato Keep a Changelog e Versionamento Semântico
5. Atualizar arquivos de versão do projeto se esta for uma nova versão

O changelog deve seguir o formato [Keep a Changelog](https://keepachangelog.com/).