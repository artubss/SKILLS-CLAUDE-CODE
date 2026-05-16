---
name: docx
description: "Use this skill sempre que o usuário quiser criar, ler, editar ou manipular documentos do Word (.docx). Os gatilhos incluem: qualquer menção de 'documento Word', 'arquivo .docx', ou solicitações para produzir documentos profissionais com formatação como tabelas de conteúdo, títulos, números de página ou timbrados. Também use ao extrair ou reorganizar conteúdo de arquivos .docx, inserir ou substituir imagens em documentos, realizar localizar-substituir em arquivos Word, trabalhar com controle de alterações ou comentários, ou converter conteúdo em um documento Word polido. Se o usuário pedir um 'relatório', 'memorando', 'carta', 'template' ou entrega similar como arquivo Word ou .docx, use esta skill. NÃO use para PDFs, planilhas, Google Docs ou tarefas de codificação geral não relacionadas à geração de documentos."
license: Proprietary. LICENSE.txt has complete terms
---

# Criação, edição e análise de DOCX

## Visão geral

Um arquivo .docx é um arquivo ZIP contendo arquivos XML.

## Referência rápida

| Tarefa | Abordagem |
|--------|-----------|
| Ler/analisar conteúdo | `pandoc` ou descompactar para XML bruto |
| Criar novo documento | Use `docx-js` - veja Criando Novos Documentos abaixo |
| Editar documento existente | Descompactar → editar XML → recompactar - veja Editando Documentos Existentes abaixo |

### Convertendo .doc para .docx

Arquivos `.doc` legados devem ser convertidos antes de serem editados:

```bash
python scripts/office/soffice.py --headless --convert-to docx document.doc
```

### Lendo Conteúdo

```bash
# Extração de texto com controle de alterações
pandoc --track-changes=all document.docx -o output.md

# Acesso ao XML bruto
python scripts/office/unpack.py document.docx unpacked/
```

### Convertendo para Imagens

```bash
python scripts/office/soffice.py --headless --convert-to pdf document.docx
pdftoppm -jpeg -r 150 document.pdf page
```

### Aceitando Controle de Alterações

Para produzir um documento limpo com todas as alterações rastreadas aceitas (requer LibreOffice):

```bash
python scripts/accept_changes.py input.docx output.docx
```

---

## Criando Novos Documentos

Gere arquivos .docx com JavaScript, depois valide. Instale: `npm install -g docx`

### Setup
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
// CRÍTICO: docx-js padrão é A4, não US Letter
// Sempre defina o tamanho da página explicitamente para resultados consistentes
sections: [{
  properties: {
    page: {
      size: {
        width: 12240,   // 8,5 polegadas em DXA
        height: 15840   // 11 polegadas em DXA
      },
      margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 } // margens de 1 polegada
    }
  },
  children: [/* content */]
}]
```

**Tamanhos de página comuns (unidades DXA, 1440 DXA = 1 polegada):**

| Papel | Largura | Altura | Largura de Conteúdo (margens de 1") |
|-------|---------|--------|--------------------------------------|
| US Letter | 12.240 | 15.840 | 9.360 |
| A4 (padrão) | 11.906 | 16.838 | 9.026 |

**Orientação paisagem:** docx-js inverte largura/altura internamente, então passe as dimensões em retrato e deixe que ele processe a inversão:
```javascript
size: {
  width: 12240,   // Passe a borda CURTA como largura
  height: 15840,  // Passe a borda LONGA como altura
  orientation: PageOrientation.LANDSCAPE  // docx-js as inverte no XML
},
// Largura de conteúdo = 15840 - margem esquerda - margem direita (usa a borda longa)
```

### Estilos (Sobrescrever Títulos Incorporados)

Use Arial como fonte padrão (universalmente suportada). Mantenha títulos em preto para legibilidade.

```javascript
const doc = new Document({
  styles: {
    default: { document: { run: { font: "Arial", size: 24 } } }, // 12pt padrão
    paragraphStyles: [
      // IMPORTANTE: Use IDs exatos para sobrescrever estilos incorporados
      { id: "Heading1", name: "Heading 1", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 32, bold: true, font: "Arial" },
        paragraph: { spacing: { before: 240, after: 240 }, outlineLevel: 0 } }, // outlineLevel necessário para TOC
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
// ❌ ERRADO - nunca insira manualmente caracteres de bullet
new Paragraph({ children: [new TextRun("• Item")] })  // ERRADO
new Paragraph({ children: [new TextRun("\u2022 Item")] })  // ERRADO

// ✅ CORRETO - use configuração de numeração com LevelFormat.BULLET
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

**CRÍTICO: Tabelas precisam de larguras duplas** - defina tanto `columnWidths` na tabela quanto `width` em cada célula. Sem ambas, as tabelas renderizam incorretamente em algumas plataformas.

```javascript
// CRÍTICO: Sempre defina a largura da tabela para renderização consistente
// CRÍTICO: Use ShadingType.CLEAR (não SOLID) para evitar fundos pretos
const border = { style: BorderStyle.SINGLE, size: 1, color: "CCCCCC" };
const borders = { top: border, bottom: border, left: border, right: border };

new Table({
  width: { size: 9360, type: WidthType.DXA }, // Sempre use DXA (percentagens quebram no Google Docs)
  columnWidths: [4680, 4680], // Devem somar à largura da tabela (DXA: 1440 = 1 polegada)
  rows: [
    new TableRow({
      children: [
        new TableCell({
          borders,
          width: { size: 4680, type: WidthType.DXA }, // Também defina em cada célula
          shading: { fill: "D5E8F0", type: ShadingType.CLEAR }, // CLEAR não SOLID
          margins: { top: 80, bottom: 80, left: 120, right: 120 }, // Preenchimento da célula (interno, não adicionado à largura)
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
// US Letter com margens de 1": 12240 - 2880 = 9360 DXA
width: { size: 9360, type: WidthType.DXA },
columnWidths: [7000, 2360]  // Devem somar à largura da tabela
```

**Regras de largura:**
- **Sempre use `WidthType.DXA`** — nunca `WidthType.PERCENTAGE` (incompatível com Google Docs)
- A largura da tabela deve ser igual à soma de `columnWidths`
- A largura `width` da célula deve corresponder ao `columnWidth` correspondente
- As margens `margins` da célula são preenchimento interno - reduzem a área de conteúdo, não adicionam à largura da célula
- Para tabelas em largura total: use largura de conteúdo (largura da página menos margens esquerda e direita)

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

### Hyperlinks

```javascript
// Link externo
new Paragraph({
  children: [new ExternalHyperlink({
    children: [new TextRun({ text: "Click here", style: "Hyperlink" })],
    link: "https://example.com",
  })]
})

// Link interno (bookmark + referência)
// 1. Crie bookmark no destino
new Paragraph({ heading: HeadingLevel.HEADING_1, children: [
  new Bookmark({ id: "chapter1", children: [new TextRun("Chapter 1")] }),
]})
// 2. Link para ele
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

### Tabulações

```javascript
// Alinhar texto à direita na mesma linha (ex: data oposta a um título)
new Paragraph({
  children: [
    new TextRun("Company Name"),
    new TextRun("\tJanuary 2025"),
  ],
  tabStops: [{ type: TabStopType.RIGHT, position: TabStopPosition.MAX }],
})

// Líder com pontos (ex: estilo TOC)
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

Force uma quebra de coluna com uma nova seção usando `type: SectionType.NEXT_COLUMN`.

### Tabela de Conteúdos

```javascript
// CRÍTICO: Títulos devem usar HeadingLevel APENAS - sem estilos customizados
new TableOfContents("Table of Contents", { hyperlink: true, headingStyleRange: "1-3" })
```

### Headers/Footers

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

- **Defina o tamanho da página explicitamente** - docx-js padrão é A4; use US Letter (12240 x 15840 DXA) para documentos dos EUA
- **Paisagem: passe dimensões em retrato** - docx-js inverte largura/altura internamente; passe a borda curta como `width`, borda longa como `height`, e defina `orientation: PageOrientation.LANDSCAPE`
- **Nunca use `\n`** - use elementos Paragraph separados
- **Nunca use bullets unicode** - use `LevelFormat.BULLET` com configuração de numeração
- **PageBreak deve estar em Paragraph** - standalone cria XML inválido
- **ImageRun requer `type`** - sempre especifique png/jpg/etc
- **Sempre defina tabela `width` com DXA** - nunca use `WidthType.PERCENTAGE` (quebra no Google Docs)
- **Tabelas precisam de larguras duplas** - array `columnWidths` E `width` em célula, ambos devem corresponder
- **Largura da tabela = soma de columnWidths** - para DXA, certifique-se de que somam exatamente
- **Sempre adicione margens de célula** - use `margins: { top: 80, bottom: 80, left: 120, right: 120 }` para preenchimento legível
- **Use `ShadingType.CLEAR`** - nunca SOLID para sombreamento de tabela
- **Nunca use tabelas como divisores/regras** - células têm altura mínima e renderizam como caixas vazias (inclusive em headers/footers); use `border: { bottom: { style: BorderStyle.SINGLE, size: 6, color: "2E75B6", space: 1 } }` em um Paragraph em vez disso. Para footers de duas colunas, use tabulações (veja seção Tabulações), não tabelas
- **TOC requer HeadingLevel apenas** - sem estilos customizados em parágrafos de título
- **Sobrescreva estilos incorporados** - use IDs exatos: "Heading1", "Heading2", etc.
- **Inclua `outlineLevel`** - necessário para TOC (0 para H1, 1 para H2, etc.)

---

## Editando Documentos Existentes

**Siga todos os 3 passos em ordem.**

### Passo 1: Descompactar
```bash
python scripts/office/unpack.py document.docx unpacked/
```
Extrai XML, formata, mescla execuções adjacentes, e converte aspas inteligentes para entidades XML (`&#x201C;` etc.) para que sobrevivam à edição. Use `--merge-runs false` para pular mesclagem de execução.

### Passo 2: Editar XML

Edite arquivos em `unpacked/word/`. Veja Referência XML abaixo para padrões.

**Use "Claude" como autor** para alterações rastreadas e comentários, a menos que o usuário peça explicitamente o uso de um nome diferente.

**Use a ferramenta Edit diretamente para substituição de string. Não escreva scripts Python.** Scripts introduzem complexidade desnecessária. A ferramenta Edit mostra exatamente o que está sendo substituído.

**CRÍTICO: Use aspas inteligentes para conteúdo novo.** Ao adicionar texto com apóstrofos ou aspas, use entidades XML para produzir aspas inteligentes:
```xml
<!-- Use essas entidades para tipografia profissional -->
<w:t>Here&#x2019;s a quote: &#x201C;Hello&#x201D;</w:t>
```
| Entidade | Caractere |
|----------|-----------|
| `&#x2018;` | ' (esquerda simples) |
| `&#x2019;` | ' (direita simples / apóstrofo) |
| `&#x201C;` | " (esquerda dupla) |
| `&#x201D;` | " (direita dupla) |

**Adicionando comentários:** Use `comment.py` para lidar com boilerplate em vários arquivos XML (texto deve ser pré-escapado em XML):
```bash
python scripts/comment.py unpacked/ 0 "Comment text with &amp; and &#x2019;"
python scripts/comment.py unpacked/ 1 "Reply text" --parent 0  # responder ao comentário 0
python scripts/comment.py unpacked/ 0 "Text" --author "Custom Author"  # nome de autor customizado
```
Depois adicione marcadores a document.xml (veja Comentários em Referência XML).

### Passo 3: Recompactar
```bash
python scripts/office/pack.py unpacked/ output.docx --original document.docx
```
Valida com auto-repair, condensa XML, e cria DOCX. Use `--validate false` para pular.

**Auto-repair corrigirá:**
- `durableId` >= 0x7FFFFFFF (regenera ID válido)
- Falta de `xml:space="preserve"` em `<w:t>` com espaço em branco

**Auto-repair não corrigirá:**
- XML malformado, aninhamento de elementos inválido, relacionamentos faltantes, violações de schema

### Armadilhas Comuns

- **Substitua elementos `<w:r>` inteiros**: Ao adicionar alterações rastreadas, substitua todo o bloco `<w:r>...</w:r>` com `<w:del>...<w:ins>...` como irmãos. Não injete tags de alteração rastreada dentro de uma execução.
- **Preserve formatação `<w:rPr>`**: Copie o bloco `<w:rPr>` da execução original para suas execuções de alteração rastreada para manter negrito, tamanho de fonte, etc.

---

## Referência XML

### Conformidade de Schema

- **Ordem de elementos em `<w:pPr>`**: `<w:pStyle>`, `<w:numPr>`, `<w:spacing>`, `<w:ind>`, `<w:jc>`, `<w:rPr>` por último
- **Espaço em branco**: Adicione `xml:space="preserve"` a `<w:t>` com espaços iniciais/finais
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
<!-- Mude "30 days" para "60 days" -->
<w:r><w:t>The term is </w:t></w:r>
<w:del w:id="1" w:author="Claude" w:date="...">
  <w:r><w:delText>30</w:delText></w:r>
</w:del>
<w:ins w:id="2" w:author="Claude" w:date="...">
  <w:r><w:t>60</w:t></w:r>
</w:ins>
<w:r><w:t> days.</w:t></w:r>
```

**Excluindo parágrafos/itens de lista inteiros** - ao remover TODO o conteúdo de um parágrafo, também marque a marca de parágrafo como excluída para que se mescle com o próximo parágrafo. Adicione `<w:del/>` dentro de `<w:pPr><w:rPr>`:
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
Sem o `<w:del/>` em `<w:pPr><w:rPr>`, aceitar alterações deixa um parágrafo/item de lista vazio.

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
4. Reference em document.xml:
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
- **LibreOffice**: Conversão de PDF (auto-configurado para ambientes sandboxed via `scripts/office/soffice.py`)
- **Poppler**: `pdftoppm` para imagens