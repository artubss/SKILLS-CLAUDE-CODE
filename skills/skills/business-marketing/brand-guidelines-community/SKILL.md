---
name: brand-guidelines
description: Aplica as cores de marca oficiais e tipografia da Anthropic a qualquer tipo de artefato que possa se beneficiar de um visual e sentimento do padrão Anthropic. Use quando diretrizes de cores de marca, guias de estilo, normas de design visual ou padrões de design corporativo se aplicarem.
license: Complete terms in LICENSE.txt
---

# Estilo de Marca Anthropic

## Visão Geral

Para acessar os recursos de identidade de marca oficial e padrões de estilo da Anthropic, use esta skill.

**Palavras-chave**: marca, identidade corporativa, identidade visual, pós-processamento, estilo, cores de marca, tipografia, marca Anthropic, formatação visual, design visual

## Diretrizes de Marca

### Cores

**Cores Principais:**

- Escuro: `#141413` - Texto primário e fundos escuros
- Claro: `#faf9f5` - Fundos claros e texto sobre escuro
- Cinza Médio: `#b0aea5` - Elementos secundários
- Cinza Claro: `#e8e6dc` - Fundos sutis

**Cores de Destaque:**

- Laranja: `#d97757` - Destaque primário
- Azul: `#6a9bcc` - Destaque secundário
- Verde: `#788c5d` - Destaque terciário

### Tipografia

- **Headings**: Poppins (com fallback para Arial)
- **Texto do Corpo**: Lora (com fallback para Georgia)
- **Nota**: As fontes devem estar pré-instaladas no seu ambiente para obter os melhores resultados

## Recursos

### Aplicação Inteligente de Fontes

- Aplica fonte Poppins a headings (24pt ou maior)
- Aplica fonte Lora ao texto do corpo
- Retorna automaticamente para Arial/Georgia se as fontes personalizadas não estiverem disponíveis
- Preserva legibilidade em todos os sistemas

### Estilo de Texto

- Headings (24pt+): fonte Poppins
- Texto do corpo: fonte Lora
- Seleção inteligente de cores com base no fundo
- Preserva hierarquia e formatação do texto

### Cores de Forma e Destaque

- Formas que não são texto usam cores de destaque
- Alterna entre acentos laranja, azul e verde
- Mantém interesse visual enquanto permanece alinhado com a marca

## Detalhes Técnicos

### Gestão de Fontes

- Usa fontes Poppins e Lora instaladas no sistema quando disponíveis
- Fornece fallback automático para Arial (headings) e Georgia (corpo)
- Nenhuma instalação de fonte necessária - funciona com fontes do sistema existentes
- Para melhores resultados, pré-instale as fontes Poppins e Lora no seu ambiente

### Aplicação de Cores

- Usa valores de cor RGB para correspondência precisa de marca
- Aplicada via classe RGBColor do python-pptx
- Mantém fidelidade de cor em diferentes sistemas