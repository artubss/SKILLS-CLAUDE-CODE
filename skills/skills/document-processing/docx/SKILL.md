---
name: docx
description: "Use esta habilidade sempre que o usuário quiser criar, ler, editar ou manipular documentos Word (.docx). Os gatilhos incluem: qualquer menção de 'documento Word', 'word', '.docx', ou solicitações para produzir documentos profissionais com formatação como sumários, títulos, números de página ou timbrados. Também use ao extrair ou reorganizar conteúdo de arquivos .docx, inserir ou substituir imagens em documentos, fazer encontrar-e-substituir em arquivos Word, trabalhar com alterações rastreadas ou comentários, ou converter conteúdo em um documento Word polido. Se o usuário pedir um 'relatório', 'memorando', 'carta', 'modelo' ou entrega similar como arquivo Word ou .docx, use esta habilidade. NÃO use para PDFs, planilhas, Google Docs ou tarefas de programação geral não relacionadas à geração de documentos."
license: Proprietary. LICENSE.txt has complete terms
---

# Criação, edição e análise de DOCX

## Visão Geral

Um arquivo .docx é um arquivo ZIP contendo arquivos XML.

## Referência Rápida

| Tarefa | Abordagem |
|--------|-----------|
| Ler/analisar conteúdo | `pandoc` ou descompactar para XML bruto |
| Criar novo documento | Use `docx-js` - veja Criando Novos Documentos abaixo |
| Editar documento existente | Descompactar → editar XML → recompactar - veja Editando Documentos Existentes abaixo |

### Convertendo .doc para .docx

Arquivos legados `.doc` devem ser convertidos antes da edição:

```bash
python scripts/office/soffice.py --headless --convert-to docx document.doc
```

### Lendo Conteúdo

```bash
# Extração de texto com alterações rastreadas
pandoc --track-changes=all document.docx -o output.md

# Acesso XML bruto
python scripts/office/unpack.py document.docx unpacked/
```

### Convertendo para Imagens

```bash
python scripts/office/soffice.py --headless --convert-to pdf document.docx
pdftoppm -jpeg -r 150 document.pdf page
```

### Aceitando Alterações Rastreadas

Para produzir um documento limpo com todas as alterações rastreadas aceitas (requer LibreOffice):

```bash
python scripts/accept_changes.py input.docx output.docx
```

---

## Criando Novos Documentos

Gere arquivos .docx com JavaScript, depois valide. Instale: `npm install -g docx`

### Configuração
```javascript
const { Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell, ImageRun,
        Header, Footer, AlignmentType, PageOrientation, LevelFormat, ExternalHyperlink,
        InternalHyperlink, Bookmark, FootnoteReferenceRun, PositionalTab,
        PositionalTabAlignment, PositionalTabRelativeTo, PositionalTabLeader,
        TabStopType, TabStopPosition, Column, SectionType,
        TableOfContents, HeadingLevel, BorderStyle, WidthType, ShadingType,
        VerticalAlign, PageNumber, PageBreak } = require('docx');

const doc = new Document({ sections: [{ children: [/* content */] }] });
Packer.toBuffer(doc).then(buffer => fs.writeFileSync("doc.docx", buffer));
```

### Validação
Após criar o arquivo, valide-o. Se a validação falhar, descompacte, corrija o XML e recompacte.
```bash
python scripts/office/validate.py doc.docx
```

### Tamanho de Página

```javascript
// CRÍTICO: docx-js padrão é A4, não Carta dos EUA
// Sempre defina o tamanho de página explicitamente para resultados consistentes
sections: [{
  properties: {
    page: {
      size: {
        width: 12240,   // 8,5 polegadas em DXA
        height: 15840   // 11 polegadas em DXA
      },
      margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 } // Margens de 1 polegada
    }
  },
  children: [/* content */]
}]
```

**Tamanhos de página comuns (unidades DXA, 1440 DXA = 1 polegada):**

| Papel | Largura | Altura | Largura do Conteúdo (margens de 1") |
|-------|---------|--------|-------------------------------------|
| Carta EUA | 12.240 | 15.840 | 9.360 |
| A4 (padrão) | 11.906 | 16.838 | 9.026 |

**Orientação paisagem:** docx-js troca largura/altura internamente, então passe dimensões retrato e deixe lidar com a troca:
```javascript
size: {
  width: 12240,   // Passe ARESTA CURTA como largura
  height: 15840,  // Passe ARESTA LONGA como altura
  orientation: PageOrientation.LANDSCAPE  // docx-js troca no XML
},
// Largura do conteúdo = 15840 - margem esquerda - margem direita (usa a aresta longa)
```

### Estilos (Sobrescrever Títulos Built-in)

Use Arial como fonte padrão (universalmente suportada). Mantenha títulos pretos para legibilidade.

```javascript
const doc = new Document({
  styles: {
    default: { document: { run: { font: "Arial", size: 24 } } }, // 12pt padrão
    paragraphStyles: [
      // IMPORTANTE: Use IDs exatos para sobrescrever estilos built-in
      { id: "Heading1", name: "Heading 1", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 32, bold: true, font: "Arial" },
        paragraph: { spacing: { before: 240, after: 240 }, outlineLevel: 0 } }, // outlineLevel obrigatório para TOC
      { id: "Heading2", name: "Heading 2", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 28, bold: true, font: "Arial" },
        paragraph: { spacing: { before: 180, after: 180 }, outlineLevel: 1 } },
    ]
  },
  sections: [{
    children: [
      new Paragraph({ heading: HeadingLevel.HEADING_1, children: [new TextRun("Title")] }),
    ]
  }]
});
```

### Listas (NUNCA use bullets unicode)

```javascript
// ❌ ERRADO - nunca insira caracteres de bullet manualmente
new Paragraph({ children: [new TextRun("• Item")] })  // RUIM
new Paragraph({ children: [new TextRun("\u2022 Item")] })  // RUIM

// ✅ CORRETO - use config de numeração com LevelFormat.BULLET
const doc = new Document({
  numbering: {
    config: [
      { reference: "bullets",
        levels: [{ level: 0, format: LevelFormat.BULLET, text: "•", alignment: AlignmentType.LEFT,
          style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] },
      { reference: "numbers",
        levels: [{ level: 0, format: LevelFormat.DECIMAL, text: "%1.", alignment: AlignmentType.LEFT,
          style: { paragraph: { indent: { left: 720, hanging: 360 } } } }] },
    ]
  },
  sections: [{
    children: [
      new Paragraph({ numbering: { reference: "bullets", level: 0 },
        children: [new TextRun("Bullet item")] }),
      new Paragraph({ numbering: { reference: "numbers", level: 0 },
        children: [new TextRun("Numbered item")] }),
    ]
  }]
});

// ⚠️ Cada referência cria numeração INDEPENDENTE
// Mesma referência = continua (1,2,3 depois 4,5,6)
// Referência diferente = reinicia (1,2,3 depois 1,2,3)
```

### Tabelas

**CRÍTICO: Tabelas precisam de larguras duplas** - defina `columnWidths` na tabela E `width` em cada célula. Sem ambas, tabelas renderizam incorretamente em algumas plataformas.

```javascript
// CRÍTICO: Sempre defina largura de tabela para renderização consistente
// CRÍTICO: Use ShadingType.CLEAR (não SOLID) para evitar fundos pretos
const border = { style: BorderStyle.SINGLE, size: 1, color: "CCCCCC" };
const borders = { top: border, bottom: border, left: border, right: border };

new Table({
  width: { size: 9360, type: WidthType.DXA }, // Sempre use DXA (percentuais quebram no Google Docs)
  columnWidths: [4680, 4680], // Devem somar à largura da tabela (DXA: 1440 = 1 polegada)
  rows: [
    new TableRow({
      children: [
        new TableCell({
          borders,
          width: { size: 4680, type: WidthType.DXA }, // Também defina em cada célula
          shading: { fill: "D5E8F0", type: ShadingType.CLEAR }, // CLEAR não SOLID
          margins: { top: 80, bottom: 80, left: 120, right: 120 }, // Padding de célula (interno, não adicionado à largura)
          children: [new Paragraph({ children: [new TextRun("Cell")] })]
        })
      ]
    })
  ]
})
```

**Cálculo de largura de tabela:**

Sempre use `WidthType.DXA` — `WidthType.PERCENTAGE` quebra no Google Docs.

```javascript
// Largura da tabela = soma de columnWidths = largura do conteúdo
// Carta EUA com margens de 1": 12240 - 2880 = 9360 DXA
width: { size: 9360, type: WidthType.DXA },
columnWidths: [7000, 2360]  // Devem somar à largura da tabela
```

**Regras de largura:**
- **Sempre use `WidthType.DXA`** — nunca `WidthType.PERCENTAGE` (incompatível com Google Docs)
- Largura da tabela deve ser igual à soma de `columnWidths`
- `width` de célula deve corresponder a `columnWidth`
- `margins` de célula são padding interno - reduzem área de conteúdo, não adicionam à largura da célula
- Para tabelas de largura total: use largura de conteúdo (largura de página menos margens esquerda e direita)

### Imagens

```javascript
// CRÍTICO: parâmetro type é OBRIGATÓRIO
new Paragraph({
  children: [new ImageRun({
    type: "png", // Obrigatório: png, jpg, jpeg, gif, bmp, svg
    data: fs.readFileSync("image.png"),
    transformation: { width: 200, height: 150 },
    altText: { title: "Title", description: "Desc", name: "Name" } // Todos os três obrigatórios
  })]
})
```

### Quebras de Página

```javascript
// CRÍTICO: PageBreak deve estar dentro de um Paragraph
new Paragraph({ children: [new PageBreak()] })

// Ou use pageBreakBefore
new Paragraph({ pageBreakBefore: true, children: [new TextRun("New page")] })
```

### Hiperlinks

```javascript
// Link externo
new Paragraph({
  children: [new ExternalHyperlink({
    children: [new TextRun({ text: "Click here", style: "Hyperlink" })],
    link: "https://example.com",
  })]
})

// Link interno (bookmark + referência)
// 1. Criar bookmark no destino
new Paragraph({ heading: HeadingLevel.HEADING_1, children: [
  new Bookmark({ id: "chapter1", children: [new TextRun("Chapter 1")] }),
]})
// 2. Vincular a ele
new Paragraph({ children: [new InternalHyperlink({
  children: [new TextRun({ text: "See Chapter 1", style: "Hyperlink" })],
  anchor: "chapter1",
})]})
```

### Notas de Rodapé

```javascript
const doc = new Document({
  footnotes: {
    1: { children: [new Paragraph("Source: Annual Report 2024")] },
    2: { children: [new Paragraph("See appendix for methodology")] },
  },
  sections: [{
    children: [new Paragraph({
      children: [
        new TextRun("Revenue grew 15%"),
        new FootnoteReferenceRun(1),
        new TextRun(" using adjusted metrics"),
        new FootnoteReferenceRun(2),
      ],
    })]
  }]
});
```

### Paradas de Tabulação

```javascript
// Alinhar texto à direita na mesma linha (ex: data oposta a um título)
new Paragraph({
  children: [
    new TextRun("Company Name"),
    new TextRun("\tJanuary 2025"),
  ],
  tabStops: [{ type: TabStopType.RIGHT, position: TabStopPosition.MAX }],
})

// Líder de ponto (ex: estilo TOC)
new Paragraph({
  children: [
    new TextRun("Introduction"),
    new TextRun({ children: [
      new PositionalTab({
        alignment: PositionalTabAlignment.RIGHT,
        relativeTo: PositionalTabRelativeTo.MARGIN,
        leader: PositionalTabLeader.DOT,
      }),
      "3",
    ]}),
  ],
})
```

### Layouts Multi-Coluna

```javascript
// Colunas de largura igual
sections: [{
  properties: {
    column: {
      count: 2,          // número de colunas
      space: 720,        // espaço entre colunas em DXA (720 = 0,5 polegada)
      equalWidth: true,
      separate: true,    // linha vertical entre colunas
    },
  },
  children: [/* conteúdo flui naturalmente pelas colunas */]
}]

// Colunas de largura customizada (equalWidth deve ser false)
sections: [{
  properties: {
    column: {
      equalWidth: false,
      children: [
        new Column({ width: 5400, space: 720 }),
        new Column({ width: 3240 }),
      ],
    },
  },
  children: [/* conteúdo */]
}]
```

Force uma quebra de coluna com nova seção usando `type: SectionType.NEXT_COLUMN`.

### Sumário

```javascript
// CRÍTICO: Títulos devem usar HeadingLevel APENAS - sem estilos customizados
new TableOfContents("Table of Contents", { hyperlink: true, headingStyleRange: "1-3" })
```

### Cabeçalhos/Rodapés

```javascript
sections: [{
  properties: {
    page: { margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 } } // 1440 = 1 polegada
  },
  headers: {
    default: new Header({ children: [new Paragraph({ children: [new TextRun("Header")] })] })
  },
  footers: {
    default: new Footer({ children: [new Paragraph({
      children: [new TextRun("Page "), new TextRun({ children: [PageNumber.CURRENT] })]
    })] })
  },
  children: [/* conteúdo */]
}]
```

### Regras Críticas para docx-js

- **Defina tamanho de página explicitamente** - docx-js padrão é A4; use Carta (12240 x 15840 DXA) para documentos brasileiros
- **Paisagem: passe dimensões retrato** - docx-js troca largura/altura internamente; passe aresta curta como `width`, aresta longa como `height`, e defina `orientation: PageOrientation.LANDSCAPE`
- **Nunca use `\n`** - use elementos Paragraph separados
- **Nunca use bullets unicode** - use `LevelFormat.BULLET` com config de numeração
- **PageBreak deve estar em Paragraph** - standalone cria XML inválido
- **ImageRun requer `type`** - sempre especifique png/jpg/etc
- **Sempre defina tabela `width` com DXA** - nunca use `WidthType.PERCENTAGE` (quebra no Google Docs)
- **Tabelas precisam de larguras duplas** - array `columnWidths` E `width` de célula, ambos devem corresponder
- **Largura da tabela = soma de columnWidths** - para DXA, certifique-se que somem exatamente
- **Sempre adicione margins de célula** - use `margins: { top: 80, bottom: 80, left: 120, right: 120 }` para padding legível
- **Use `ShadingType.CLEAR`** - nunca SOLID para sombreamento de tabela
- **Nunca use tabelas como divisores/regras** - células têm altura mínima e renderizam como caixas vazias (incluindo em cabeçalhos/rodapés); use `border: { bottom: { style: BorderStyle.SINGLE, size: 6, color: "2E75B6", space: 1 } }` em um Paragraph em vez disso. Para rodapés em duas colunas, use tab stops (veja seção Tab Stops), não tabelas
- **TOC requer HeadingLevel apenas** - sem estilos customizados em parágrafos com título
- **Sobrescreva estilos built-in** - use IDs exatos: "Heading1", "Heading2", etc.
- **Inclua `outlineLevel`** - obrigatório para TOC (0 para H1, 1 para H2, etc.)

---

## Editando Documentos Existentes

**Siga todos os 3 passos em ordem.**

### Passo 1: Descompactar
```bash
python scripts/office/unpack.py document.docx unpacked/
```
Extrai XML, formata, mescla runs adjacentes e converte smart quotes para entidades XML (`&#x201C;` etc.) para que sobrevivam à edição. Use `--merge-runs false` para pular mesclagem de runs.

### Passo 2: Editar XML

Edite arquivos em `unpacked/word/`. Veja Referência XML abaixo para padrões.

**Use "Claude" como o autor** para alterações rastreadas e comentários, a menos que o usuário solicite explicitamente o uso de um nome diferente.

**Use a ferramenta Edit diretamente para substituição de string. Não escreva scripts Python.** Scripts introduzem complexidade desnecessária. A ferramenta Edit mostra exatamente o que está sendo substituído.

**CRÍTICO: Use smart quotes para novo conteúdo.** Ao adicionar texto com apóstrofos ou aspas, use entidades XML para produzir smart quotes:
```xml
<!-- Use estas entidades para tipografia profissional -->
<w:t>Here&#x2019;s a quote: &#x201C;Hello&#x201D;</w:t>
```
| Entidade | Caractere |
|----------|-----------|
| `&#x2018;` | ' (single esquerda) |
| `&#x2019;` | ' (single direita / apóstrofo) |
| `&#x201C;` | " (dupla esquerda) |
| `&#x201D;` | " (dupla direita) |

**Adicionando comentários:** Use `comment.py` para lidar com boilerplate em vários arquivos XML (texto deve ser XML pré-escapado):
```bash
python scripts/comment.py unpacked/ 0 "Comment text with &amp; and &#x2019;"
python scripts/comment.py unpacked/ 1 "Reply text" --parent 0  # resposta ao comentário 0
python scripts/comment.py unpacked/ 0 "Text" --author "Custom Author"  # nome de autor customizado
```
Depois adicione marcadores a document.xml (veja Comentários em Referência XML).

### Passo 3: Recompactar
```bash
python scripts/office/pack.py unpacked/ output.docx --original document.docx
```
Valida com auto-repair, condensa XML e cria DOCX. Use `--validate false` para pular.

**Auto-repair vai consertar:**
- `durableId` >= 0x7FFFFFFF (regenera ID válido)
- `xml:space="preserve"` ausente em `<w:t>` com espaço em branco

**Auto-repair não vai consertar:**
- XML malformado, aninhamento inválido de elementos, relacionamentos ausentes, violações de schema

### Armadilhas Comuns

- **Substitua elementos inteiros `<w:r>`**: Ao adicionar alterações rastreadas, substitua o bloco inteiro `<w:r>...</w:r>` com `<w:del>...<w:ins>...` como irmãos. Não injete tags de alteração rastreada dentro de um run.
- **Preserve formatação `<w:rPr>`**: Copie o bloco `<w:rPr>` do run original para seus runs de alteração rastreada para manter negrito, tamanho de fonte, etc.

---

## Referência XML

### Conformidade de Schema

- **Ordem de elemento em `<w:pPr>`**: `<w:pStyle>`, `<w:numPr>`, `<w:spacing>`, `<w:ind>`, `<w:jc>`, `<w:rPr>` por último
- **Espaço em branco**: Adicione `xml:space="preserve"` a `<w:t>` com espaços à esquerda/direita
- **RSIDs**: Devem ser hex de 8 dígitos (ex: `00AB1234`)

### Alterações Rastreadas

**Inserção:**
```xml
<w:ins w:id="1" w:author="Claude" w:date="2025-01-01T00:00:00Z">
  <w:r><w:t>inserted text</w:t></w:r>
</w:ins>
```

**Exclusão:**
```xml
<w:del w:id="2" w:author="Claude" w:date="2025-01-01T00:00:00Z">
  <w:r><w:delText>deleted text</w:delText></w:r>
</w:del>
```

**Dentro de `<w:del>`**: Use `<w:delText>` em vez de `<w:t>`, e `<w:delInstrText>` em vez de `<w:instrText>`.

**Edições mínimas** - marque apenas o que muda:
```xml
<!-- Mudar "30 dias" para "60 dias" -->
<w:r><w:t>The term is </w:t></w:r>
<w:del w:id="1" w:author="Claude" w:date="...">
  <w:r><w:delText>30</w:delText></w:r>
</w:del>
<w:ins w:id="2" w:author="Claude" w:date="...">
  <w:r><w:t>60</w:t></w:r>
</w:ins>
<w:r><w:t> days.</w:t></w:r>
```

**Deletando parágrafos/itens de lista inteiros** - ao remover TODO conteúdo de um parágrafo, também marque a marca de parágrafo como deletada para que ela se mescle com o próximo parágrafo. Adicione `<w:del/>` dentro de `<w:pPr><w:rPr>`:
```xml
<w:p>
  <w:pPr>
    <w:numPr>...</w:numPr>  <!-- numeração de lista se presente -->
    <w:rPr>
      <w:del w:id="1" w:author="Claude" w:date="2025-01-01T00:00:00Z"/>
    </w:rPr>
  </w:pPr>
  <w:del w:id="2" w:author="Claude" w:date="2025-01-01T00:00:00Z">
    <w:r><w:delText>Entire paragraph content being deleted...</w:delText></w:r>
  </w:del>
</w:p>
```
Sem `<w:del/>` em `<w:pPr><w:rPr>`, aceitar alterações deixa um parágrafo/item de lista vazio.

**Rejeitando inserção de outro autor** - aninhe exclusão dentro de sua inserção:
```xml
<w:ins w:author="Jane" w:id="5">
  <w:del w:author="Claude" w:id="10">
    <w:r><w:delText>their inserted text</w:delText></w:r>
  </w:del>
</w:ins>
```

**Restaurando exclusão de outro autor** - adicione inserção depois (não modifique sua exclusão):
```xml
<w:del w:author="Jane" w:id="5">
  <w:r><w:delText>deleted text</w:delText></w:r>
</w:del>
<w:ins w:author="Claude" w:id="10">
  <w:r><w:t>deleted text</w:t></w:r>
</w:ins>
```

### Comentários

Após executar `comment.py` (veja Passo 2), adicione marcadores a document.xml. Para respostas, use flag `--parent` e aninhe marcadores dentro do pai.

**CRÍTICO: `<w:commentRangeStart>` e `<w:commentRangeEnd>` são irmãos de `<w:r>`, nunca dentro de `<w:r>`.**

```xml
<!-- Marcadores de comentário são filhos diretos de w:p, nunca dentro de w:r -->
<w:commentRangeStart w:id="0"/>
<w:del w:id="1" w:author="Claude" w:date="2025-01-01T00:00:00Z">
  <w:r><w:delText>deleted</w:delText></w:r>
</w:del>
<w:r><w:t> more text</w:t></w:r>
<w:commentRangeEnd w:id="0"/>
<w:r><w:rPr><w:rStyle w:val="CommentReference"/></w:rPr><w:commentReference w:id="0"/></w:r>

<!-- Comentário 0 com resposta 1 aninhada dentro -->
<w:commentRangeStart w:id="0"/>
  <w:commentRangeStart w:id="1"/>
  <w:r><w:t>text</w:t></w:r>
  <w:commentRangeEnd w:id="1"/>
<w:commentRangeEnd w:id="0"/>
<w:r><w:rPr><w:rStyle w:val="CommentReference"/></w:rPr><w:commentReference w:id="0"/></w:r>
<w:r><w:rPr><w:rStyle w:val="CommentReference"/></w:rPr><w:commentReference w:id="1"/></w:r>
```

### Imagens

1. Adicione arquivo de imagem a `word/media/`
2. Adicione relacionamento a `word/_rels/document.xml.rels`:
```xml
<Relationship Id="rId5" Type=".../image" Target="media/image1.png"/>
```
3. Adicione tipo de conteúdo a `[Content_Types].xml`:
```xml
<Default Extension="png" ContentType="image/png"/>
```
4. Referencie em document.xml:
```xml
<w:drawing>
  <wp:inline>
    <wp:extent cx="914400" cy="914400"/>  <!-- EMUs: 914400 = 1 polegada -->
    <a:graphic>
      <a:graphicData uri=".../picture">
        <pic:pic>
          <pic:blipFill><a:blip r:embed="rId5"/></pic:blipFill>
        </pic:pic>
      </a:graphicData>
    </a:graphic>
  </wp:inline>
</w:drawing>
```

---

## Dependências

- **pandoc**: Extração de texto
- **docx**: `npm install -g docx` (novos documentos)
- **LibreOffice**: Conversão para PDF (auto-configurado para ambientes sandbox via `scripts/office/soffice.py`)
- **Poppler**: `pdftoppm` para imagens