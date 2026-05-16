---
name: pptx
description: "Use this skill sempre que um arquivo .pptx estiver envolvido de qualquer forma — como entrada, saída ou ambos. Isso inclui: criar decks de slides, pitch decks ou apresentações; ler, analisar ou extrair texto de qualquer arquivo .pptx (mesmo que o conteúdo extraído seja usado em outro lugar, como em um email ou resumo); editar, modificar ou atualizar apresentações existentes; combinar ou dividir arquivos de slides; trabalhar com templates, layouts, anotações do apresentador ou comentários. Acione sempre que o usuário mencionar \"deck\", \"slides\", \"apresentação\" ou referenciar um nome de arquivo .pptx, independentemente do que planejam fazer com o conteúdo depois. Se um arquivo .pptx precisa ser aberto, criado ou tocado, use esta skill."
license: Proprietary. LICENSE.txt has complete terms
---

# Skill PPTX

## Referência Rápida

| Tarefa | Guia |
|--------|------|
| Ler/analisar conteúdo | `python -m markitdown presentation.pptx` |
| Editar ou criar a partir de template | Leia [editing.md](editing.md) |
| Criar do zero | Leia [pptxgenjs.md](pptxgenjs.md) |

---

## Lendo Conteúdo

```bash
# Extração de texto
python -m markitdown presentation.pptx

# Visão geral visual
python scripts/thumbnail.py presentation.pptx

# XML bruto
python scripts/office/unpack.py presentation.pptx unpacked/
```

---

## Workflow de Edição

**Leia [editing.md](editing.md) para detalhes completos.**

1. Analise o template com `thumbnail.py`
2. Descompacte → manipule slides → edite conteúdo → limpe → compacte

---

## Criando do Zero

**Leia [pptxgenjs.md](pptxgenjs.md) para detalhes completos.**

Use quando não houver template ou apresentação de referência disponível.

---

## Ideias de Design

**Não crie slides chatos.** Apenas bullets em fundo branco não impressionam ninguém. Considere ideias desta lista para cada slide.

### Antes de Começar

- **Escolha uma paleta de cores ousada e informada pelo conteúdo**: A paleta deve parecer projetada para ESTE tópico. Se você conseguisse trocar suas cores para uma apresentação completamente diferente e ainda funcionasse, você não fez escolhas específicas o suficiente.
- **Dominância sobre igualdade**: Uma cor deve dominar (60-70% do peso visual), com 1-2 tons de suporte e um acento aguçado. Nunca dê a todas as cores peso igual.
- **Contraste escuro/claro**: Fundos escuros para slides de título + conclusão, claro para conteúdo (estrutura "sanduíche"). Ou comprometa-se com escuro em toda a apresentação para uma sensação premium.
- **Comprometa-se com um motivo visual**: Escolha UM elemento distintivo e repita-o — molduras de imagem arredondadas, ícones em círculos coloridos, bordas espessas em um lado. Repita-o em cada slide.

### Paletas de Cores

Escolha cores que correspondam ao seu tópico — não use o azul genérico padrão. Use estas paletas como inspiração:

| Tema | Primária | Secundária | Acento |
|------|----------|-----------|--------|
| **Executivo Meia-Noite** | `1E2761` (azul marinho) | `CADCFC` (azul-gelo) | `FFFFFF` (branco) |
| **Floresta e Musgo** | `2C5F2D` (floresta) | `97BC62` (musgo) | `F5F5F5` (creme) |
| **Energia Coral** | `F96167` (coral) | `F9E795` (ouro) | `2F3C7E` (azul marinho) |
| **Terracota Quente** | `B85042` (terracota) | `E7E8D1` (areia) | `A7BEAE` (sálvia) |
| **Gradiente Oceano** | `065A82` (azul profundo) | `1C7293` (azul-petróleo) | `21295C` (meia-noite) |
| **Carvão Minimalista** | `36454F` (carvão) | `F2F2F2` (branco-off) | `212121` (preto) |
| **Confiança Azul-petróleo** | `028090` (azul-petróleo) | `00A896` (água do mar) | `02C39A` (hortelã) |
| **Baga e Creme** | `6D2E46` (baga) | `A26769` (rosa-poeirento) | `ECE2D0` (creme) |
| **Sálvia Calma** | `84B59F` (sálvia) | `69A297` (eucalipto) | `50808E` (ardósia) |
| **Cereja Ousada** | `990011` (cereja) | `FCF6F5` (branco-off) | `2F3C7E` (azul marinho) |

### Para Cada Slide

**Cada slide precisa de um elemento visual** — imagem, gráfico, ícone ou forma. Slides apenas com texto são esquecíveis.

**Opções de layout:**
- Duas colunas (texto à esquerda, ilustração à direita)
- Linhas de ícone + texto (ícone em círculo colorido, header em negrito, descrição abaixo)
- Grade 2x2 ou 2x3 (imagem em um lado, grade de blocos de conteúdo no outro)
- Imagem com bleed parcial (lado esquerdo ou direito completo) com conteúdo sobreposto

**Exibição de dados:**
- Callouts de grandes estatísticas (números grandes 60-72pt com rótulos pequenos abaixo)
- Colunas de comparação (antes/depois, prós/contras, opções lado a lado)
- Timeline ou fluxo de processo (passos numerados, setas)

**Polish visual:**
- Ícones em pequenos círculos coloridos ao lado de headers de seção
- Texto em itálico para destaque com estatísticas-chave ou taglines

### Tipografia

**Escolha um emparelhamento de fonte interessante** — não use Arial como padrão. Escolha uma fonte de header com personalidade e emparelhe-a com uma fonte de corpo limpa.

| Fonte de Header | Fonte de Corpo |
|-----------------|----------------|
| Georgia | Calibri |
| Arial Black | Arial |
| Calibri | Calibri Light |
| Cambria | Calibri |
| Trebuchet MS | Calibri |
| Impact | Arial |
| Palatino | Garamond |
| Consolas | Calibri |

| Elemento | Tamanho |
|----------|--------|
| Título do slide | 36-44pt negrito |
| Header de seção | 20-24pt negrito |
| Texto de corpo | 14-16pt |
| Legendas | 10-12pt atenuado |

### Espaçamento

- 0,5" de margem mínima
- 0,3-0,5" entre blocos de conteúdo
- Deixe espaço para respirar — não preencha cada polegada

### Evite (Erros Comuns)

- **Não repita o mesmo layout** — varie colunas, cards e callouts entre slides
- **Não centralize texto de corpo** — alinhe parágrafos e listas à esquerda; centralize apenas títulos
- **Não economize no contraste de tamanho** — títulos precisam de 36pt+ para se destacar de 14-16pt de corpo
- **Não use azul como padrão** — escolha cores que reflitam o tópico específico
- **Não misture espaçamento aleatoriamente** — escolha gaps de 0,3" ou 0,5" e use consistentemente
- **Não estilize um slide e deixe o restante simples** — comprometa-se totalmente ou mantenha simplicidade em toda parte
- **Não crie slides apenas com texto** — adicione imagens, ícones, gráficos ou elementos visuais; evite apenas título + bullets
- **Não esqueça de padding em caixas de texto** — ao alinhar linhas ou formas com bordas de texto, defina `margin: 0` na caixa de texto ou desloque a forma para compensar o padding
- **Não use elementos de baixo contraste** — ícones E texto precisam de contraste forte contra o fundo; evite texto claro em fundos claros ou texto escuro em fundos escuros
- **NUNCA use linhas de acento sob títulos** — estas são marca registrada de slides gerados por IA; use espaço em branco ou cor de fundo em vez disso

---

## QA (Obrigatório)

**Assuma que há problemas. Seu trabalho é encontrá-los.**

Sua primeira renderização quase nunca está correta. Aborde o QA como uma caça a bugs, não um passo de confirmação. Se você não encontrou zero problemas na primeira inspeção, você não estava procurando com cuidado o suficiente.

### QA de Conteúdo

```bash
python -m markitdown output.pptx
```

Verifique conteúdo faltante, erros de digitação, ordem incorreta.

**Ao usar templates, verifique se há texto de placeholder restante:**

```bash
python -m markitdown output.pptx | grep -iE "xxxx|lorem|ipsum|this.*(page|slide).*layout"
```

Se grep retornar resultados, corrija-os antes de declarar sucesso.

### QA Visual

**⚠️ USE SUBAGENTES** — mesmo para 2-3 slides. Você esteve olhando para o código e verá o que espera, não o que está lá. Subagentes têm olhos frescos.

Converta slides em imagens (veja [Convertendo para Imagens](#convertendo-para-imagens)), depois use este prompt:

```
Inspecione visualmente estes slides. Assuma que há problemas — encontre-os.

Procure por:
- Elementos sobrepostos (texto através de formas, linhas através de palavras, elementos empilhados)
- Texto transbordando ou cortado nas bordas/limites de caixa
- Linhas decorativas posicionadas para texto de uma linha mas título envolvido para duas linhas
- Citações de fonte ou rodapés colidindo com conteúdo acima
- Elementos muito próximos (< 0,3" de gaps) ou cards/seções quase tocando
- Gaps desiguais (grande área vazia em um lugar, apertado em outro)
- Margem insuficiente das bordas do slide (< 0,5")
- Colunas ou elementos similares não alinhados consistentemente
- Texto de baixo contraste (ex: texto cinza claro em fundo cor creme)
- Ícones de baixo contraste (ex: ícones escuros em fundos escuros sem círculo contrastante)
- Caixas de texto muito estreitas causando envolvimento excessivo
- Conteúdo de placeholder deixado para trás

Para cada slide, liste problemas ou áreas de preocupação, mesmo que menores.

Leia e analise estas imagens:
1. /path/to/slide-01.jpg (Esperado: [breve descrição])
2. /path/to/slide-02.jpg (Esperado: [breve descrição])

Relate TODOS os problemas encontrados, incluindo menores.
```

### Loop de Verificação

1. Gere slides → Converta para imagens → Inspecione
2. **Liste problemas encontrados** (se nenhum encontrado, procure novamente mais criticamente)
3. Corrija problemas
4. **Re-verifique slides afetados** — uma correção frequentemente cria outro problema
5. Repita até que uma passagem completa não revele novos problemas

**Não declare sucesso até ter completado pelo menos um ciclo de correção e verificação.**

---

## Convertendo para Imagens

Converta apresentações para imagens de slide individual para inspeção visual:

```bash
python scripts/office/soffice.py --headless --convert-to pdf output.pptx
pdftoppm -jpeg -r 150 output.pdf slide
```

Isso cria `slide-01.jpg`, `slide-02.jpg`, etc.

Para re-renderizar slides específicos após correções:

```bash
pdftoppm -jpeg -r 150 -f N -l N output.pdf slide-fixed
```

---

## Dependências

- `pip install "markitdown[pptx]"` - extração de texto
- `pip install Pillow` - grades de miniatura
- `npm install -g pptxgenjs` - criando do zero
- LibreOffice (`soffice`) - conversão PDF (auto-configurado para ambientes sandboxed via `scripts/office/soffice.py`)
- Poppler (`pdftoppm`) - PDF para imagens