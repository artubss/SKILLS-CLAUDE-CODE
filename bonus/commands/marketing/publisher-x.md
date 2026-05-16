---
allowed-tools: Read, Write, Bash, Glob, WebFetch
argument-hint: <input> [lang]
description: Gere threads do X/Twitter copiáveis a partir de posts de blog, artigos, PDFs ou URLs com 3 opções de formato
---

# Gerador de Thread X/Twitter

Gere uma thread do X copiável a partir de qualquer fonte de conteúdo - posts de blog, artigos, PDFs, URLs ou texto simples.

**Uso:** `$ARGUMENTS`

**Processo:**

1. **Analise Argumentos de Entrada**
   - Extraia a entrada de conteúdo e parâmetro de idioma opcional
   - Exemplos:
     - `2025-10-06-meu-post` (apenas slug, inglês padrão)
     - `2025-10-06-meu-post pt` (slug com português)
     - `caminho/para/artigo.md` (caminho do arquivo)
     - `https://meublog.com/post` (URL)
     - `docs/whitepaper.pdf pt` (PDF com idioma)

2. **Detecção Universal de Entrada**

   **Se a entrada parece um caminho de arquivo** (contém `/` ou extensão):
   - Use a ferramenta Read para verificar se o arquivo existe
   - Detecte o formato pela extensão:
     - `.md` / `.mdx` → Analise markdown com frontmatter (extraia título, descrição, corpo, metadados)
     - `.pdf` → Informe ao usuário que análise de PDF é limitada, sugira converter para markdown primeiro
     - `.docx` → Informe ao usuário que análise de DOCX é limitada, sugira converter para markdown primeiro
     - `.html` → Leia e extraia conteúdo principal, remova tags HTML
     - `.txt` → Leia como texto simples
     - `.json` → Analise JSON e extraia campos relevantes
   - Extraia: título, descrição, conteúdo do corpo, metadados

   **Se a entrada parece uma URL** (começa com `http://` ou `https://`):
   - Use a ferramenta WebFetch para recuperar a página
   - Prompt: "Extraia o conteúdo principal do artigo, título e descrição desta página"
   - Analise e limpe o texto

   **Se a entrada é um slug** (sem `/` e sem protocolo):
   - Pesquise a codebase usando Glob: `**/*${input}*.md`
   - Padrões comuns a verificar:
     - `src/content/blog/posts/{en,pt}/*${input}*.md`
     - `content/blog/*${input}*.md`
     - `posts/*${input}*.md`
     - `blog/*${input}*.md`
   - Se o idioma for especificado, priorize pasta de idioma correspondente
   - Use a ferramenta Read para analisar arquivo markdown com frontmatter

3. **Determine o Idioma** (padrão: Inglês):
   - Se o usuário especificar explicitamente "pt" → Português
   - Se o usuário especificar explicitamente "en" → Inglês
   - Se o caminho do arquivo contiver `/pt/` → Português
   - Se o conteúdo parecer estar em português → Português
   - Caso contrário → Inglês

4. **Gere TRÊS versões** no idioma alvo:

   **Versão 1: Thread (5-8 posts)**
   - Tom profissional e envolvente
   - Divida em posts digeríveis (máx 280 caracteres cada)

   **Versão 2: Única Longa (Premium)**
   - Formato estruturado com seções claras
   - **Para português**: Use 【colchetes】: 【O que é】【Para quem é】【Características principais】【Próximo passo】
   - **Para inglês**: Use headers: **What it is:** **Who it's for:** **Key features:** **What to do next:**

   **Versão 3: Única Curta (~280 caracteres)**
   - Anúncio conciso
   - 2-3 benefícios principais com emojis
   - Links e hashtags

5. **Exiba todas as versões** para o usuário no terminal:
   - Mostre posts de thread com contagem de caracteres
   - Mostre versão única longa
   - Mostre versão única curta
   - Formate para fácil cópia

6. **Crie arquivo de visualização HTML tri-formato** usando a ferramenta Write:
   - **IMPORTANTE**: Verifique primeiro se o arquivo existe: `ls -la x-thread-[LANG].html 2>&1`
   - Se o arquivo existir, use a ferramenta Read primeiro (mesmo que apenas 1 linha): `Read('x-thread-[LANG].html', limit=1)`
   - Depois use a ferramenta Write para criar/atualizar: `x-thread-[LANG].html` no diretório atual do usuário
   - **INCLUA TRÊS abas com interface seletora de abas**:
     - **Aba 1: Thread** - 5-8 posts com botões individuais "Copiar Post"
     - **Aba 2: Única Longa** - Formato estruturado com seções, um botão "Copiar"
     - **Aba 3: Única Curta** - Versão concisa (~280 caracteres), um botão "Copiar"
   - Use marca X (tema preto)
   - Seletor de abas no topo para navegação fácil
   - Use ferramenta Bash para abrir: `open x-thread-[LANG].html && open https://x.com/compose/post`

---

## Diretrizes de Thread X

### Estrutura da Thread (5-8 tweets):

1. **Tweet de Abertura** (Tweet 1/X)
   - Prenda atenção com uma declaração contrária, estatística ou afirmação audaciosa
   - Não revele tudo - crie curiosidade
   - SEM hashtags ou links no primeiro tweet (melhor alcance de algoritmo)
   - Máx 280 caracteres incluindo indicador de thread

2. **Tweets de Problema/Contexto** (Tweets 2-3/X)
   - Estabeleça o problema ou contexto
   - Use pontos de dados específicos do blog
   - Mantenha cada tweet com UMA ideia
   - Máx 280 caracteres cada

3. **Tweets de Insight** (Tweets 4-6/X)
   - Compartilhe 3-5 insights principais do blog
   - Use pontos de bala (•) ou listas numeradas
   - Inclua exemplos ou estatísticas específicas
   - Torne cada tweet autossuficiente
   - Máx 280 caracteres cada

4. **Tweet de CTA** (Tweet final)
   - Link para o artigo completo
   - CTA simples: "Leia o guia completo:" ou "Análise completa:"
   - Pode incluir 2-3 hashtags relevantes aqui
   - Estimule engajamento: "Qual é sua opinião?"

**Numeração da Thread:**
- Inclua numeração estilo "(1/6)" EM CADA tweet
- A contagem DEVE estar correta
- Coloque no final de cada tweet

**Limites de Caracteres:**
- Cada post: MÁXIM 280 caracteres (incluindo número da thread)
- Leve em conta encurtamento de URL: URLs = 23 caracteres no X
- Deixe buffer de 10-15 caracteres por segurança