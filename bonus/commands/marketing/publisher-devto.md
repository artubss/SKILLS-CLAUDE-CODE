---
allowed-tools: Read, Write, Bash, Glob
argument-hint:
description: Gerar feed RSS Dev.to a partir de todos os posts do blog para sindicação automática
---

# Gerador de Feed RSS Dev.to

Gere um feed RSS completo a partir de todos os seus posts do blog para importação automática no Dev.to.

**Uso:** `/publisher:devto` (nenhum argumento necessário)

**O que faz:**
- Verifica todos os posts do blog no seu codebase
- Converte markdown para HTML
- Gera feed RSS 2.0 com codificação apropriada
- Cria arquivo `public/rss-devto.xml`
- Fornece instruções de configuração para Dev.to

**Processo:**

1. **Verificar Posts do Blog**
   - Procura por arquivos markdown no codebase
   - Padrões comuns:
     - `src/content/blog/**/*.md`
     - `content/blog/**/*.md`
     - `posts/**/*.md`
     - `blog/**/*.md`

2. **Analisar Posts do Blog**
   - Extrai frontmatter (título, data, descrição, tags)
   - Converte corpo markdown para HTML
   - Codifica HTML adequadamente para RSS (seções CDATA)
   - Extrai datas de publicação

3. **Gerar Feed RSS**
   - Cria estrutura XML RSS 2.0 válida
   - Inclui todos os posts como itens
   - Adiciona metadados adequados do canal
   - Codifica HTML para compatibilidade com Dev.to

4. **Salvar Arquivo do Feed**
   - Escreve em `public/rss-devto.xml`
   - Garante formatação XML apropriada
   - Valida estrutura RSS

5. **Exibir Instruções de Configuração**
   - Mostra como adicionar RSS ao Dev.to
   - Explica requisitos de deployment
   - Guia você pela configuração

**Configuração Única:**
1. Execute este comando para gerar o feed RSS
2. Faça deploy do seu site (deixe o RSS publicamente acessível)
3. Acesse https://dev.to/settings/extensions
4. Adicione sua URL RSS (ex: `https://seusite.com/rss-devto.xml`)
5. Dev.to importará automaticamente todos os posts futuros

**Benefícios:**
- Sindicação automática para Dev.to
- Todos os posts futuros sincronizam automaticamente
- Sem necessidade de cópia manual
- Mantém formatação original