---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [migration-strategy] | --gradual | --complete | --strict | --incremental
description: Migrar projeto JavaScript para TypeScript com tipagem adequada e configuração de ferramentas
---

# Migrar para TypeScript

Migrar projeto JavaScript para TypeScript com segurança de tipo abrangente: **$ARGUMENTS**

## Estado Atual do JavaScript

- Estrutura do projeto: @package.json (analisar mistura JS/TS e dependências)
- Arquivos JavaScript: !`find . -name "*.js" -not -path "./node_modules/*" | wc -l`
- TypeScript existente: !`find . -name "*.ts" -not -path "./node_modules/*" | wc -l`
- Sistema de build: @webpack.config.js ou @vite.config.js ou @rollup.config.js

## Tarefa

Migrar sistematicamente a base de código JavaScript para TypeScript com tipagem adequada e ferramentas:

**Estratégia de Migração**: Use $ARGUMENTS para especificar migração gradual, conversão completa, modo strict, ou abordagem incremental

**Processo de Migração**:
1. **Configuração do Ambiente** - Instalação do TypeScript, configuração de tsconfig.json, integração com ferramenta de build
2. **Definições de Tipo** - Instalar pacotes @types, criar declarações de tipo customizadas, definir interfaces
3. **Migração de Arquivos** - Renomear .js para .ts/.tsx, adicionar anotações de tipo, resolver erros do compilador
4. **Transformação de Código** - Converter classes, funções e módulos com tipagem adequada
5. **Resolução de Erros** - Corrigir incompatibilidades de tipo, tratamento de null/undefined, problemas de modo strict
6. **Teste e Validação** - Atualizar arquivos de teste, configurar verificação de tipo, validar cobertura de tipos

**Recursos Avançados**: Tipos genéricos, tipos mapeados, tipos condicionais, augmentação de módulo e configurações rigorosas do compilador.

**Experiência do Desenvolvedor**: Configurar integração com IDE, debugging, regras de linting e onboarding da equipe.

**Saída**: Base de código TypeScript totalmente tipada com verificação de tipo rigorosa, IntelliSense abrangente e produtividade melhorada do desenvolvedor.