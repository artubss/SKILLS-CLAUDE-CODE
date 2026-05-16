---
name: brand-guidelines
description: Aplica as cores oficiais da marca Anthropic e tipografia a qualquer tipo de artefato que possa se beneficiar da identidade visual da Anthropic. Use quando diretrizes de cores da marca ou padrões de design, formatação visual ou padrões de design corporativo se aplicarem.
license: Complete terms in LICENSE.txt
---

# Estilo de Marca Anthropic

## Visão Geral

Para acessar recursos de identidade de marca e diretrizes de estilo oficiais da Anthropic, use esta habilidade.

**Palavras-chave**: marca, identidade corporativa, identidade visual, pós-processamento, estilo, cores da marca, tipografia, marca Anthropic, formatação visual, design visual

## Diretrizes de Marca

### Cores

**Cores Principais:**

- Escuro: `#141413` - Texto primário e fundos escuros
- Claro: `#faf9f5` - Fundos claros e texto em escuro
- Cinza Médio: `#b0aea5` - Elementos secundários
- Cinza Claro: `#e8e6dc` - Fundos sutis

**Cores de Destaque:**

- Laranja: `#d97757` - Destaque primário
- Azul: `#6a9bcc` - Destaque secundário
- Verde: `#788c5d` - Destaque terciário

### Tipografia

- **Títulos**: Poppins (com Arial como alternativa)
- **Corpo do Texto**: Lora (com Georgia como alternativa)
- **Nota**: As fontes devem estar pré-instaladas em seu ambiente para melhores resultados

## Recursos

### Aplicação Inteligente de Fontes

- Aplica fonte Poppins a títulos (24pt e maiores)
- Aplica fonte Lora ao corpo do texto
- Retrocede automaticamente para Arial/Georgia se as fontes personalizadas não estiverem disponíveis
- Preserva legibilidade em todos os sistemas

### Estilo de Texto

- Títulos (24pt+): fonte Poppins
- Corpo do texto: fonte Lora
- Seleção inteligente de cores baseada no fundo
- Preserva hierarquia de texto e formatação

### Formas e Cores de Destaque

- Formas sem texto usam cores de destaque
- Alterna entre destaques laranja, azul e verde
- Mantém interesse visual respeitando a identidade da marca

## Detalhes Técnicos

### Gerenciamento de Fontes

- Usa fontes Poppins e Lora instaladas no sistema quando disponíveis
- Fornece retrocesso automático para Arial (títulos) e Georgia (corpo)
- Não requer instalação de fontes - funciona com fontes já existentes no sistema
- Para melhores resultados, pré-instale as fontes Poppins e Lora em seu ambiente

### Aplicação de Cores

- Usa valores de cores RGB para correspondência precisa da marca
- Aplicadas através da classe RGBColor do python-pptx
- Mantém fidelidade de cores em diferentes sistemas