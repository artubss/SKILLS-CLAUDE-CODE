---
allowed-tools: Bash(df:*), Bash(du:*), Bash(npm cache clean:*), Bash(brew cleanup:*), Bash(rm:*), Bash(find:*), Bash(docker system prune:*)
argument-hint: [--aggressive] | [--maximum]
description: Limpar caches do sistema (npm, Homebrew, Yarn, navegadores, Python/ML) para liberar espaço em disco
---

# Limpeza de Cache do Sistema

Limpar arquivos temporários e caches para liberar espaço em disco: $ARGUMENTS

## Uso Atual de Disco

- **Espaço em disco**: !`df -h / | tail -1`
- **Cache npm**: !`du -sh ~/.npm 2>/dev/null || echo "Não encontrado"`
- **Cache Yarn**: !`du -sh ~/Library/Caches/Yarn 2>/dev/null || echo "Não encontrado"`
- **Cache Homebrew**: !`brew cleanup -n 2>/dev/null | head -5 || echo "Homebrew não instalado"`

## Opções de Limpeza

Com base nos argumentos fornecidos, execute o nível apropriado de limpeza:

### Opção 1: Limpeza Conservadora (padrão)

Limpeza segura de caches de gerenciadores de pacotes que podem ser facilmente reconstruídos:

```bash
# Registrar espaço em disco inicial
echo "Iniciando limpeza..."
df -h / | tail -1 | awk '{print "Antes: " $4 " livres"}'

# Limpar cache npm
echo "Limpando cache npm..."
npm cache clean --force

# Limpar Homebrew
echo "Limpando Homebrew..."
brew cleanup

# Limpar cache Yarn
echo "Limpando cache Yarn..."
rm -rf ~/Library/Caches/Yarn

# Mostrar resultados
df -h / | tail -1 | awk '{print "Depois: " $4 " livres"}'
```

### Opção 2: Limpeza Agressiva (flag --aggressive)

Inclui toda a limpeza conservadora mais caches de navegadores e ferramentas de desenvolvimento:

```bash
# Executar limpeza conservadora primeiro (da Opção 1)
npm cache clean --force
brew cleanup
rm -rf ~/Library/Caches/Yarn

# Limpar caches de navegadores
echo "Limpando caches de navegadores..."
rm -rf ~/Library/Caches/Google
rm -rf ~/Library/Caches/com.operasoftware.Opera
rm -rf ~/Library/Caches/Firefox
rm -rf ~/Library/Caches/Mozilla
rm -rf ~/Library/Caches/zen
rm -rf ~/Library/Caches/Arc

# Limpar caches de ferramentas de desenvolvimento
echo "Limpando caches de desenvolvimento..."
rm -rf ~/Library/Caches/JetBrains
rm -rf ~/Library/Caches/pnpm
rm -rf ~/.cache/puppeteer
rm -rf ~/.cache/selenium

# Limpar caches Python/ML
echo "Limpando caches Python/ML..."
rm -rf ~/.cache/uv
rm -rf ~/.cache/huggingface
rm -rf ~/.cache/torch
rm -rf ~/.cache/whisper

# Mostrar resultados
df -h / | tail -1 | awk '{print "Depois da limpeza agressiva: " $4 " livres"}'
```

### Opção 3: Limpeza Máxima (flag --maximum)

Inclui toda a limpeza agressiva mais Docker e node_modules antigos:

```bash
# Executar limpeza agressiva primeiro (da Opção 2)
npm cache clean --force
brew cleanup
rm -rf ~/Library/Caches/Yarn
rm -rf ~/Library/Caches/{Google,com.operasoftware.Opera,Firefox,Mozilla,zen,Arc,JetBrains,pnpm}
rm -rf ~/.cache/{puppeteer,selenium,uv,huggingface,torch,whisper}

# Limpar Docker (se instalado)
echo "Limpando Docker..."
docker system prune -af --volumes 2>/dev/null || echo "Docker não está rodando ou não está instalado"

# Listar diretórios node_modules para revisão manual
echo "Procurando diretórios node_modules..."
echo "Nota: Não deletando automaticamente. Revise e delete manualmente se necessário."
find ~ -name "node_modules" -type d -prune 2>/dev/null | head -20

# Mostrar resultados
df -h / | tail -1 | awk '{print "Depois da limpeza máxima: " $4 " livres"}'
```

## Etapas de Execução

1. **Determinar Nível de Limpeza**
   - Sem argumentos ou vazio: Executar Limpeza Conservadora (Opção 1)
   - `--aggressive`: Executar Limpeza Agressiva (Opção 2)
   - `--maximum`: Executar Limpeza Máxima (Opção 3)

2. **Verificações de Segurança**
   - Verificar permissões suficientes
   - Garantir que aplicações críticas estejam fechadas (navegadores para Opção 2+)
   - Avisar sobre remoção de containers Docker (Opção 3)

3. **Executar Limpeza**
   - Executar comandos apropriados com base na opção selecionada
   - Mostrar progresso para cada etapa de limpeza
   - Tratar erros com elegância (diretórios ausentes, permissões)

4. **Relatar Resultados**
   - Exibir espaço em disco antes e depois
   - Mostrar quantidade de espaço recuperado
   - Listar o que foi limpo
   - Fornecer recomendações se mais espaço for necessário

## Notas Importantes

**Limpeza Conservadora** (padrão):
- ✅ Sempre seguro executar
- ✅ Caches se reconstroem automaticamente quando necessário
- ✅ Sem impacto em aplicações

**Limpeza Agressiva** (--aggressive):
- ⚠️ Fechar navegadores antes de executar
- ⚠️ Caches de navegadores se reconstruirão no próximo uso
- ⚠️ Modelos de ML serão re-baixados se necessário

**Limpeza Máxima** (--maximum):
- ⚠️ Para e remove todos os containers/imagens Docker
- ⚠️ Apenas deleta node_modules após revisão manual
- ⚠️ Mais impactante mas recupera mais espaço

## Recuperação

Todos os caches limpos são temporários e se reconstruirão automaticamente:

- **npm/Yarn**: Se reconstrói no próximo `npm install`
- **Homebrew**: Baixado no próximo `brew install`
- **Navegadores**: Se reconstrói na próxima sessão de navegação
- **Python/ML**: Re-baixa modelos no próximo uso
- **Docker**: Puxar imagens novamente com `docker pull`

## Exemplo de Uso

```bash
# Limpeza conservadora (padrão)
/cleanup-cache

# Limpeza agressiva
/cleanup-cache --aggressive

# Limpeza máxima
/cleanup-cache --maximum
```

Após a limpeza, verifique os resultados e informe ao usuário:
1. Espaço liberado
2. Espaço livre atual
3. O que foi limpo
4. Se limpeza adicional é recomendada