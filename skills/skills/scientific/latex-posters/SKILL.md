---
name: latex-posters
description: "Crie pôsteres de pesquisa profissionais em LaTeX usando beamerposter, tikzposter ou baposter. Suporte para apresentações em conferências, pôsteres acadêmicos e comunicação científica. Inclui design de layout, esquemas de cores, formatos multi-coluna, integração de figuras e boas práticas específicas para comunicação visual."
allowed-tools: [Read, Write, Edit, Bash]
---

# Pôsteres de Pesquisa em LaTeX

## Visão Geral

Pôsteres de pesquisa são um meio crítico para comunicação científica em conferências, simpósios e eventos acadêmicos. Esta habilidade fornece orientação abrangente para criar pôsteres de pesquisa profissionais e visualmente atraentes usando pacotes LaTeX. Gere pôsteres de qualidade de publicação com layout adequado, tipografia, esquemas de cores e hierarquia visual.

## Quando Usar Esta Habilidade

Esta habilidade deve ser usada quando:
- Criando pôsteres de pesquisa para conferências, simpósios ou sessões de pôsteres
- Projetando pôsteres acadêmicos para eventos universitários ou defesas de tese
- Preparando resumos visuais de pesquisa para engajamento público
- Convertendo artigos científicos em formato de pôster
- Criando modelos de pôster para grupos de pesquisa ou departamentos
- Projetando pôsteres que atendem a requisitos específicos de tamanho de conferência (A0, A1, 36×48", etc.)
- Construindo pôsteres com layouts multi-coluna complexos
- Integrando figuras, tabelas, equações e citações em formato de pôster

## Aprimoramento Visual com Esquemas Científicos

**⚠️ OBRIGATÓRIO: Cada pôster de pesquisa DEVE incluir pelo menos 2-3 figuras geradas por IA usando a habilidade scientific-schematics.**

Isto não é opcional. Pôsteres são principalmente mídia visual – pôsteres textuais falham em comunicar efetivamente. Antes de finalizar qualquer pôster:
1. Gere no mínimo DUAS esquemáticas ou diagramas
2. Alvo de 3-4 figuras para pôsteres abrangentes (fluxograma de metodologia, visualização de resultados principais, estrutura conceitual)
3. Figuras devem ocupar 40-50% da área do pôster

**Como gerar figuras:**
- Use a habilidade **scientific-schematics** para gerar diagramas de qualidade de publicação com IA
- Simplesmente descreva seu diagrama desejado em linguagem natural
- O Nano Banana Pro gerará automaticamente, revisará e refinará a esquemática

**Como gerar esquemáticas:**
```bash
python scripts/generate_schematic.py "descrição do seu diagrama" -o figures/output.png
```

A IA irá automaticamente:
- Criar imagens de qualidade de publicação com formatação adequada
- Revisar e refinar por meio de múltiplas iterações
- Garantir acessibilidade (amigável para daltonismo, alto contraste)
- Salvar outputs no diretório figures/

**Quando adicionar esquemáticas:**
- Fluxogramas de metodologia de pesquisa para conteúdo do pôster
- Diagramas de estrutura conceitual
- Visualizações de design experimental
- Diagramas de pipeline de análise de dados
- Diagramas de arquitetura de sistema
- Ilustrações de vias biológicas
- Qualquer conceito complexo que se beneficie de visualização

Para orientação detalhada sobre como criar esquemáticas, consulte a documentação da habilidade scientific-schematics.

---

## Capacidades Principais

### 1. Pacotes LaTeX para Pôsteres

Suporte para três pacotes LaTeX principais, cada um com vantagens distintas. Para comparação detalhada e orientação específica de pacotes, consulte `references/latex_poster_packages.md`.

**beamerposter**:
- Extensão da classe de apresentação Beamer
- Sintaxe familiar para usuários de Beamer
- Excelente suporte de temas e personalização
- Melhor para: Pôsteres acadêmicos tradicionais, identidade institucional

**tikzposter**:
- Design moderno e flexível com integração TikZ
- Temas de cores e modelos de layout integrados
- Extensiva personalização por meio de comandos TikZ
- Melhor para: Designs coloridos e modernos, gráficos personalizados

**baposter**:
- Sistema de layout baseado em caixas
- Espaçamento automático e posicionamento
- Estilos padrão com aparência profissional
- Melhor para: Layouts multi-coluna, espaçamento consistente

### 2. Layout e Estrutura de Pôster

Crie layouts de pôster efetivos seguindo princípios de comunicação visual. Para orientação abrangente sobre layout, consulte `references/poster_layout_design.md`.

**Seções Comuns de Pôster**:
- **Cabeçalho/Título**: Título, autores, afiliações, logos
- **Introdução/Contexto**: Contexto da pesquisa e motivação
- **Métodos/Abordagem**: Metodologia e design experimental
- **Resultados**: Principais descobertas com figuras e visualizações de dados
- **Conclusões**: Principais conclusões e implicações
- **Referências**: Citações principais (tipicamente abreviadas)
- **Agradecimentos**: Financiamento, colaboradores, instituições

**Estratégias de Layout**:
- **Layouts baseados em colunas**: Grades de 2, 3 ou 4 colunas
- **Layouts baseados em blocos**: Arranjo flexível de blocos de conteúdo
- **Fluxo padrão Z**: Guie leitores através do conteúdo logicamente
- **Hierarquia visual**: Use tamanho, cor e espaçamento para enfatizar pontos-chave

### 3. Princípios de Design para Pôsteres de Pesquisa

Aplique princípios de design baseados em evidências para máximo impacto. Para orientação detalhada sobre design, consulte `references/poster_design_principles.md`.

**Tipografia**:
- Título: 72-120pt para visibilidade de distância
- Cabeçalhos de seção: 48-72pt
- Texto do corpo: mínimo 24-36pt para legibilidade de 4-6 pés
- Use fontes sem serifa (Arial, Helvetica, Calibri) para clareza
- Limite a 2-3 famílias de fontes no máximo

**Cor e Contraste**:
- Use esquemas de cores de alto contraste para legibilidade
- Paletas de cores institucionais para identidade visual
- Paletas amigáveis para daltônico (evite combinações vermelho-verde)
- Espaço em branco é espaço ativo — não aglutine

**Elementos Visuais**:
- Figuras de alta resolução (mínimo 300 DPI para impressão)
- Rótulos grandes e claros em todas as figuras
- Estilo de figura consistente ao longo
- Uso estratégico de ícones e gráficos
- Equilíbrio entre texto e conteúdo visual (40-50% visual recomendado)

**Diretrizes de Conteúdo**:
- **Menos é mais**: 300-800 palavras totais recomendadas
- Pontos de marcação sobre parágrafos para capacidade de escaneamento
- Mensagens claras e concisas
- Figuras auto-explicativas com explicação textual mínima
- Códigos QR para materiais suplementares ou recursos online

### 4. Tamanhos Padrão de Pôster

Suporte para dimensões de pôster internacionais e específicas de conferências:

**Padrões Internacionais**:
- A0 (841 × 1189 mm / 33,1 × 46,8 polegadas) - Padrão europeu mais comum
- A1 (594 × 841 mm / 23,4 × 33,1 polegadas) - Formato menor
- A2 (420 × 594 mm / 16,5 × 23,4 polegadas) - Pôsteres compactos

**Padrões Norte-Americanos**:
- 36 × 48 polegadas (914 × 1219 mm) - Tamanho comum de conferência nos EUA
- 42 × 56 polegadas (1067 × 1422 mm) - Formato grande
- 48 × 72 polegadas (1219 × 1829 mm) - Extra grande

**Orientação**:
- Retrato (vertical) - Mais comum, tradicional
- Paisagem (horizontal) - Melhor para conteúdo amplo, linhas do tempo

### 5. Modelos Específicos de Pacote

Forneça modelos prontos para uso de cada pacote principal. Modelos disponíveis no diretório `assets/`.

**Modelos beamerposter**:
- `beamerposter_classic.tex` - Estilo acadêmico tradicional
- `beamerposter_modern.tex` - Design limpo e minimalista
- `beamerposter_colorful.tex` - Tema vibrante com blocos

**Modelos tikzposter**:
- `tikzposter_default.tex` - Layout padrão tikzposter
- `tikzposter_rays.tex` - Design moderno com tema ray
- `tikzposter_wave.tex` - Tema estilo onda profissional

**Modelos baposter**:
- `baposter_portrait.tex` - Layout clássico retrato
- `baposter_landscape.tex` - Paisagem multi-coluna
- `baposter_minimal.tex` - Design minimalista

### 6. Integração de Figuras e Imagens

Otimize conteúdo visual para apresentações de pôster:

**Melhores Práticas**:
- Use gráficos vetoriais (PDF, SVG) quando possível para escalabilidade
- Imagens raster: mínimo 300 DPI no tamanho final de impressão
- Estilo de imagem consistente (bordas, legendas, tamanhos)
- Agrupe figuras relacionadas
- Use subfiguras para comparações

**Comandos LaTeX para Figuras**:
```latex
% Inclua pacote de gráficos
\usepackage{graphicx}

% Figura simples
\includegraphics[width=0.8\linewidth]{figure.pdf}

% Figura com legenda em tikzposter
\block{Results}{
  \begin{tikzfigure}
    \includegraphics[width=0.9\linewidth]{results.png}
  \end{tikzfigure}
}

% Múltiplas subfiguras
\usepackage{subcaption}
\begin{figure}
  \begin{subfigure}{0.48\linewidth}
    \includegraphics[width=\linewidth]{fig1.pdf}
    \caption{Condição A}
  \end{subfigure}
  \begin{subfigure}{0.48\linewidth}
    \includegraphics[width=\linewidth]{fig2.pdf}
    \caption{Condição B}
  \end{subfigure}
\end{figure}
```

### 7. Esquemas de Cores e Temas

Forneça paletas de cores profissionais para diversos contextos:

**Cores de Instituição Acadêmica**:
- Combine com marca da universidade ou departamento
- Use códigos de cores oficiais (RGB, CMYK ou definições de cor LaTeX)

**Paletas de Cores Científicas** (amigáveis para daltônico):
- Viridis: Gradiente profissional de roxo a amarelo
- ColorBrewer: Paletas testadas em pesquisa para visualização de dados
- IBM Color Blind Safe: Paleta corporativa acessível

**Seleção de Tema Específico de Pacote**:

**beamerposter**:
```latex
\usetheme{Berlin}
\usecolortheme{beaver}
```

**tikzposter**:
```latex
\usetheme{Rays}
\usecolorstyle{Denmark}
```

**baposter**:
```latex
\begin{poster}{
  background=plain,
  bgColorOne=white,
  headerColorOne=blue!70,
  textborder=rounded
}
```

### 8. Tipografia e Formatação de Texto

Garanta legibilidade e apelo visual:

**Seleção de Fonte**:
```latex
% Fontes sem serifa recomendadas para pôsteres
\usepackage{helvet}      % Helvetica
\usepackage{avant}       % Avant Garde
\usepackage{sfmath}      % Fontes matemáticas sem serifa

% Defina padrão como sem serifa
\renewcommand{\familydefault}{\sfdefault}
```

**Dimensionamento de Texto**:
```latex
% Ajuste tamanhos de texto para visibilidade
\setbeamerfont{title}{size=\VeryHuge}
\setbeamerfont{author}{size=\Large}
\setbeamerfont{institute}{size=\normalsize}
```

**Ênfase e Destaque**:
- Use negrito para termos-chave: `\textbf{importante}`
- Destaques de cor com moderação: `\textcolor{blue}{destaque}`
- Caixas para informações críticas
- Evite itálico (mais difícil de ler de distância)

### 9. Códigos QR e Elementos Interativos

Aprimore a interatividade do pôster para conferências modernas:

**Integração de Código QR**:
```latex
\usepackage{qrcode}

% Link para artigo, repositório de código ou materiais suplementares
\qrcode[height=2cm]{https://github.com/username/project}

% Código QR com legenda
\begin{center}
  \qrcode[height=3cm]{https://doi.org/10.1234/paper}\\
  \small Digitalize para ver o artigo completo
\end{center}
```

**Aprimoramentos Digitais**:
- Link para repositórios GitHub com código
- Link para apresentações em vídeo ou demos
- Link para visualizações web interativas
- Link para dados suplementares ou apêndices

### 10. Compilação e Saída

Gere saída PDF de alta qualidade para impressão ou exibição digital:

**Comandos de Compilação**:
```bash
# Compilação básica
pdflatex poster.tex

# Com bibliografia
pdflatex poster.tex
bibtex poster
pdflatex poster.tex
pdflatex poster.tex

# Para pôsteres baseados em beamer
lualatex poster.tex  # Melhor suporte de fontes
xelatex poster.tex   # Fontes Unicode e modernas
```

**Garantindo Cobertura de Página Completa**:

Pôsteres devem usar toda a página sem margens excessivas. Configure pacotes corretamente:

**beamerposter - Configuração de Página Completa**:
```latex
\documentclass[final,t]{beamer}
\usepackage[size=a0,scale=1.4,orientation=portrait]{beamerposter}

% Remova margens padrão de beamer
\setbeamersize{text margin left=0mm, text margin right=0mm}

% Use geometry para controle preciso
\usepackage[margin=10mm]{geometry}  % 10mm margens em torno

% Remova símbolos de navegação
\setbeamertemplate{navigation symbols}{}

% Remova rodapé e cabeçalho se não necessário
\setbeamertemplate{footline}{}
\setbeamertemplate{headline}{}
```

**tikzposter - Configuração de Página Completa**:
```latex
\documentclass[
  25pt,                      % Escala de fonte
  a0paper,                   % Tamanho de papel
  portrait,                  % Orientação
  margin=10mm,               % Margens externas (mínimas)
  innermargin=15mm,          % Espaço dentro de blocos
  blockverticalspace=15mm,   % Espaço entre blocos
  colspace=15mm,             % Espaço entre colunas
  subcolspace=8mm            % Espaço entre subcolunas
]{tikzposter}

% Isto garante que conteúdo preencha a página
```

**baposter - Configuração de Página Completa**:
```latex
\documentclass[a0paper,portrait,fontscale=0.285]{baposter}

\begin{poster}{
  grid=false,
  columns=3,
  colspacing=1.5em,          % Espaço entre colunas
  eyecatcher=true,
  background=plain,
  bgColorOne=white,
  borderColor=blue!50,
  headerheight=0.12\textheight,  % 12% para cabeçalho
  textborder=roundedleft,
  headerborder=closed,
  boxheaderheight=2em        % Altura consistente de cabeçalho de caixa
}
% Conteúdo aqui
\end{poster}
```

**Problemas Comuns e Correções**:

**Problema**: Grandes margens brancas ao redor do pôster
```latex
% Correção para beamerposter
\setbeamersize{text margin left=5mm, text margin right=5mm}

% Correção para tikzposter
\documentclass[..., margin=5mm, innermargin=10mm]{tikzposter}

% Correção para baposter - ajuste na classe de documento
\documentclass[a0paper, margin=5mm]{baposter}
```

**Problema**: Conteúdo não preenche espaço vertical
```latex
% Use \vfill entre seções para distribuir espaço
\block{Introduction}{...}
\vfill
\block{Methods}{...}
\vfill
\block{Results}{...}

% Ou ajuste manualmente espaçamento de blocos
\vspace{1cm}  % Adicione espaço entre blocos específicos
```

**Problema**: Pôster estende além de limites de página
```latex
% Verifique cálculo de largura total
% Para 3 colunas com espaçamento:
% Total = 3×columnwidth + 2×colspace + 2×margins
% Garanta que isto seja igual a \paperwidth

% Debug adicionando limite de página visível
\usepackage{eso-pic}
\AddToShipoutPictureBG{
  \AtPageLowerLeft{
    \put(0,0){\framebox(\LenToUnit{\paperwidth},\LenToUnit{\paperheight}){}}
  }
}
```

**Preparação para Impressão**:
- Gere PDF/X-1a para impressão profissional
- Incorpore todas as fontes
- Converta cores para CMYK se necessário
- Verifique resolução de todas as imagens (mínimo 300 DPI)
- Adicione área de sangria se necessário (geralmente 3-5mm)
- Verifique tamanho de página corresponde aos requisitos exatamente

**Exibição Digital**:
- Espaço de cor RGB para exibição em tela
- Otimize tamanho de arquivo para email/web
- Teste legibilidade em diferentes telas

### 11. Revisão de PDF e Controle de Qualidade

**CRÍTICO**: Sempre revise o PDF gerado antes de imprimir ou apresentar. Use esta lista de verificação sistemática:

**Passo 1: Verificação de Tamanho de Página**
```bash
# Verifique dimensões de PDF (deve corresponder exatamente ao tamanho do pôster)
pdfinfo poster.pdf | grep "Page size"

# Saídas esperadas:
# A0: 2384 x 3370 points (841 x 1189 mm)
# 36x48": 2592 x 3456 points
# A1: 1684 x 2384 points (594 x 841 mm)
```

**Passo 2: Lista de Verificação de Inspeção Visual**

Abra PDF em zoom de 100% e verifique:

**Layout e Espaçamento**:
- [ ] Conteúdo preenche página inteira (sem grandes margens brancas)
- [ ] Espaçamento consistente entre colunas
- [ ] Espaçamento consistente entre blocos/seções
- [ ] Todos os elementos alinhados apropriadamente (use ferramenta de régua)
- [ ] Nenhum texto ou figura sobreposto
- [ ] Espaço em branco distribuído uniformemente

**Tipografia**:
- [ ] Título claramente visível e grande (72pt+)
- [ ] Cabeçalhos de seção legíveis (48-72pt)
- [ ] Texto do corpo legível em zoom de 100% (mínimo 24-36pt)
- [ ] Nenhum texto recortado ou saindo das bordas
- [ ] Uso de fonte consistente ao longo
- [ ] Todos os caracteres especiais renderizados corretamente (símbolos, letras gregas)

**Elementos Visuais**:
- [ ] Todas as figuras exibem corretamente
- [ ] Nenhuma imagem pixelizada ou desfocada
- [ ] Legendas de figura presentes e legíveis
- [ ] Cores renderizam como esperado (não desbotadas ou muito escuras)
- [ ] Logos exibem com clareza
- [ ] Códigos QR visíveis e escaneáveis

**Completude de Conteúdo**:
- [ ] Título e autores completos
- [ ] Todas as seções presentes (Intro, Métodos, Resultados, Conclusões)
- [ ] Referências incluídas
- [ ] Informações de contato visíveis
- [ ] Agradecimentos (se aplicável)
- [ ] Nenhum texto de placeholder restante (Lorem ipsum, TODO, etc.)

**Qualidade Técnica**:
- [ ] Nenhuma avisos de compilação LaTeX em áreas importantes
- [ ] Todas as citações resolvidas (sem marcas [?])
- [ ] Todas as referências cruzadas funcionando
- [ ] Limites de página corretos (nenhum conteúdo recortado)

**Passo 3: Teste de Impressão em Escala Reduzida**

**Teste Pré-Impressão Essencial**:
```bash
# Crie teste em tamanho reduzido (25% do tamanho final)
# Isto simula visualização de pôster completo de ~8-10 pés

# Para pôster A0, imprima em papel A4 (escala 24,7%)
# Para pôster 36x48", imprima em letter (escala ~25%)
```

**Lista de Verificação de Teste de Impressão**:
- [ ] Título legível de 6 pés de distância
- [ ] Cabeçalhos de seção legíveis de 4 pés de distância
- [ ] Texto do corpo legível de 2 pés de distância
- [ ] Figuras claras e compreensíveis
- [ ] Cores impressas com precisão
- [ ] Nenhum defeito óbvio de design

**Passo 4: Verificações de Qualidade Digital**

**Verificação de Incorporação de Fontes**:
```bash
# Verifique que todas as fontes estão incorporadas (obrigatório para impressão)
pdffonts poster.pdf

# Todas as fontes devem mostrar "yes" na coluna "emb"
# Se alguma mostrar "no", recompile com:
pdflatex -dEmbedAllFonts=true poster.tex
```

**Verificação de Resolução de Imagem**:
```bash
# Extraia informações de imagem
pdfimages -list poster.pdf

# Verifique que todas as imagens têm pelo menos 300 DPI
# Fórmula: DPI = pixels / (polegadas no pôster)
# Para largura A0 (33,1"): 300 DPI = 9930 pixels mínimo
```

**Otimização de Tamanho de Arquivo**:
```bash
# Para email/web, comprima se necessário (>50MB)
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 \
   -dPDFSETTINGS=/printer -dNOPAUSE -dQUIET -dBATCH \
   -sOutputFile=poster_compressed.pdf poster.pdf

# Para impressão, mantenha original (sem compressão)
```

**Passo 5: Verificação de Acessibilidade**

**Verificação de Contraste de Cor**:
- [ ] Contraste texto-fundo ≥ 4,5:1 (WCAG AA)
- [ ] Contraste de elementos importantes ≥ 7:1 (WCAG AAA)
- Teste online: https://webaim.org/resources/contrastchecker/

**Simulação de Daltonismo**:
- [ ] Visualize PDF por simulador de daltonismo
- [ ] Informação não perdida com simulação vermelho-verde
- [ ] Use Coblis (color-blindness.com) ou ferramenta similar

**Passo 6: Prova de Conteúdo**

**Revisão Sistemática**:
- [ ] Verificação ortográfica de todo o texto
- [ ] Verifique todos os nomes de autores e afiliações
- [ ] Verifique todos os números e estatísticas quanto à precisão
- [ ] Confirme que todas as citações estão corretas
- [ ] Revise rótulos e legendas de figuras
- [ ] Verifique erros de digitação em cabeçalhos e títulos

**Revisão por Pares**:
- [ ] Peça a colega para revisar pôster
- [ ] Teste de 30 segundos: Conseguem identificar mensagem principal?
- [ ] Revisão de 5 minutos: Entendem as conclusões?
- [ ] Anote quaisquer elementos confusos

**Passo 7: Validação Técnica**

**Revisão de Log de Compilação LaTeX**:
```bash
# Verifique avisos em arquivo .log
grep -i "warning\|error\|overfull\|underfull" poster.log

# Problemas comuns a corrigir:
# - Overfull hbox: Texto se estendendo além de margens
# - Underfull hbox: Espaçamento excessivo
# - Referências ausentes: Citações não resolvidas
# - Figuras ausentes: Arquivos de imagem não encontrados
```

**Corrija Avisos Comuns**:
```latex
% Overfull hbox (texto muito largo)
\usepackage{microtype}  % Melhor espaçamento
\sloppy  % Permita espaçamento ligeiramente mais solto
\hyphenation{palavra-longa}  % Hifenização manual

% Fontes ausentes
\usepackage[T1]{fontenc}  % Melhor codificação de fonte

% Imagem não encontrada
% Garanta que caminhos estejam corretos e arquivos existam
\graphicspath{{./figures/}{./images/}}
```

**Passo 8: Lista de Verificação Final Pré-Impressão**

**Antes de Enviar para Impressora**:
- [ ] Tamanho de PDF corresponde exatamente aos requisitos (verifique com pdfinfo)
- [ ] Todas as fontes incorporadas (verifique com pdffonts)
- [ ] Modo de cor correto (RGB para tela, CMYK para impressão se necessário)
- [ ] Área de sangria adicionada se necessário (geralmente 3-5mm)
- [ ] Marcas de corte visíveis se necessário
- [ ] Teste de impressão concluído e revisado
- [ ] Nomeação de arquivo clara: [SobreNome]_[Conferência]_Poster.pdf
- [ ] Cópia de backup salva

**Especificações de Impressão para Confirmar**:
- [ ] Tipo de papel (fosco vs. brilhante)
- [ ] Método de impressão (inkjet, grande formato, tecido)
- [ ] Perfil de cor (fornecido à impressora se necessário)
- [ ] Prazo de entrega e endereço de envio
- [ ] Preferência de embalagem (tubo ou plano)

**Lista de Verificação de Apresentação Digital**:
- [ ] Tamanho de PDF otimizado (<10MB para email)
- [ ] Testado em múltiplos visualizadores de PDF (Adobe, Preview, etc.)
- [ ] Exibe corretamente em diferentes telas
- [ ] Códigos QR testados e funcionais
- [ ] Formatos alternativos preparados (PNG para mídia social)

**Script de Revisão** (Disponível em `scripts/review_poster.sh`):
```bash
#!/bin/bash
# Script automatizado de revisão de PDF de pôster

echo "Verificação de Qualidade de PDF de Pôster"
echo "======================================="

# Verifique se arquivo existe
if [ ! -f "$1" ]; then
    echo "Erro: Arquivo não encontrado"
    exit 1
fi

echo "Arquivo: $1"
echo ""

# Verifique tamanho de página
echo "1. Dimensões de Página:"
pdfinfo "$1" | grep "Page size"
echo ""

# Verifique fontes
echo "2. Incorporação de Fontes:"
pdffonts "$1" | head -20
echo ""

# Verifique tamanho de arquivo
echo "3. Tamanho de Arquivo:"
ls -lh "$1" | awk '{print $5}'
echo ""

# Contagem de páginas (deve ser 1 para pôster)
echo "4. Contagem de Páginas:"
pdfinfo "$1" | grep "Pages"
echo ""

echo "Verificações manuais necessárias:"
echo "- Inspeção visual em zoom de 100%"
echo "- Teste