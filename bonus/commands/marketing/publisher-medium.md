---
allowed-tools: Read, Write, Bash, Glob, WebFetch
argument-hint: <input> [lang]
description: Converte posts de blog para formato pronto para Medium com marcadores de upload de imagens
---

# Conversor de Artigos para Medium

Converte posts de blog para formato pronto para Medium com estrutura HTML apropriada e manipulação de imagens.

**Uso:** `$ARGUMENTS`

**Exemplos:**
```bash
/publisher:medium meu-post           # Inglês padrão
/publisher:medium meu-post ja        # Japonês
/publisher:medium artigo.md          # A partir do caminho do arquivo
/publisher:medium https://blog.com/post  # A partir de URL
```

**Processo:**

1. **Analisar Entrada e Detectar Fonte**
   - Caminho do arquivo, URL ou slug de post
   - Parâmetro de idioma opcional (en/ja)

2. **Detecção Universal de Entrada**
   - **Arquivo**: Lê markdown, PDF, HTML ou texto
   - **URL**: WebFetch para recuperar conteúdo
   - **Slug**: Busca post no repositório de código

3. **Converter para Formato Medium**
   - Analisa markdown e extrai frontmatter
   - Converte para HTML limpo adequado para Medium
   - Preserva headers, listas, blocos de código, citações
   - Adiciona marcadores de upload para diagramas
   - Inclui caminhos de imagens para fácil referência de upload

4. **Criar Arquivo de Visualização HTML**
   - Gera preview `medium-article-[LANG].html`
   - Inclui botão de cópia com um clique
   - Adiciona instruções de upload de imagens com caminhos de arquivo
   - Usa formatação e cores estilo Medium

5. **Abrir no Navegador**
   - Abre arquivo de preview HTML
   - Abre editor do Medium (https://medium.com/new-story)
   - Usuário copia HTML e cola no Medium
   - Segue marcadores de imagem para fazer upload de diagramas

**Saída:**
- Arquivo de preview HTML com botão de cópia
- Marcadores de upload de imagem claros
- Caminhos de arquivo mostrados para cada imagem
- Pronto para colar no editor do Medium

**Nota**: Funciona universalmente - nenhuma dependência necessária, apenas ferramentas Read, Write e Bash.