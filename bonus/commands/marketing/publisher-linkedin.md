---
allowed-tools: Read, Write, Bash, Glob, WebFetch
argument-hint: <input> [idioma] [caminho-arquivo-customizado]
description: Gerar posts do LinkedIn a partir de conteúdo de blog com anexação automática de mídia via LinkedIn API
---

# Gerador de Posts do LinkedIn

Crie posts profissionais do LinkedIn a partir de qualquer fonte de conteúdo com anexação opcional de mídia.

**Uso:** `$ARGUMENTS`

**Exemplos:**
```bash
/publisher:linkedin meu-post                    # Auto-detecta e anexa diagramas do blog
/publisher:linkedin meu-post en                 # Inglês com diagramas
/publisher:linkedin meu-post en imagem.png      # Anexação de imagem customizada
/publisher:linkedin meu-post ja relatorio.pdf   # Japonês com PDF customizado
```

**Processo:**

1. **Analisar Argumentos de Entrada**
   - Entrada de conteúdo (slug, caminho de arquivo ou URL)
   - Parâmetro de idioma opcional (en/ja)
   - Caminho de arquivo opcional para anexação

2. **Detecção Universal de Entrada**
   - **Caminho de arquivo**: Ler e analisar (markdown, PDF, HTML, texto, JSON)
   - **URL**: Usar WebFetch para recuperar conteúdo
   - **Slug**: Buscar no codebase por post de blog correspondente

3. **Gerar Post Profissional do LinkedIn**
   - Usar tom de liderança de pensamento para inglês
   - Usar tom profissional de negócios (敬語) para japonês
   - Extrair insights principais do conteúdo real
   - Incluir hashtags relevantes (máximo 2-4)
   - Adicionar link para artigo completo

4. **Gerenciar Anexação de Mídia**
   - **Arquivo customizado**: Usar imagem/PDF especificado se fornecido
   - **Auto-detecção**: Encontrar diagramas do blog se disponíveis
   - Formatos suportados: PNG, JPG, JPEG, PDF

5. **Postar via LinkedIn API** (usando Bash + curl)
   - Verificar credenciais no arquivo .env
   - Gerenciar fluxo OAuth se necessário
   - **CRÍTICO**: Escapar caracteres reservados do LinkedIn Little Text Format: `| { } @ [ ] ( ) < > # * _ ~`
   - Fazer upload do arquivo de mídia e obter URN do asset
   - Criar rascunho de post com comentário e mídia
   - Abrir LinkedIn no navegador para revisão

**Autenticação da LinkedIn API:**
1. Criar app do LinkedIn em https://www.linkedin.com/developers/apps
2. Adicionar credenciais ao .env:
   ```
   LINKEDIN_CLIENT_ID=seu_client_id
   LINKEDIN_CLIENT_SECRET=seu_secret
   LINKEDIN_ACCESS_TOKEN=seu_token (gerado automaticamente no primeiro uso)
   ```

**Sem configuração de API**: Comando ainda gera o conteúdo do post para cópia manual.

**Nota**: Funciona em QUALQUER tipo de repositório (Python, Rust, Go, etc.) - usa apenas bash e curl, sem necessidade de Node.js.