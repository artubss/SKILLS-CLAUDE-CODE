---
name: slack-gif-creator
description: Conhecimento e utilitários para criar GIFs animados otimizados para Slack. Fornece restrições, ferramentas de validação e conceitos de animação. Use quando usuários solicitarem GIFs animados para Slack como "crie um GIF de X fazendo Y para Slack".
license: Termos completos em LICENSE.txt
---

# Slack GIF Creator

Um toolkit que fornece utilitários e conhecimento para criar GIFs animados otimizados para Slack.

## Requisitos do Slack

**Dimensões:**
- GIFs de Emoji: 128x128 (recomendado)
- GIFs de Mensagem: 480x480

**Parâmetros:**
- FPS: 10-30 (quanto menor, menor o tamanho do arquivo)
- Cores: 48-128 (menos cores = arquivo menor)
- Duração: Mantenha menos de 3 segundos para GIFs de emoji

## Fluxo Principal

```python
from core.gif_builder import GIFBuilder
from PIL import Image, ImageDraw

# 1. Criar builder
builder = GIFBuilder(width=128, height=128, fps=10)

# 2. Gerar frames
for i in range(12):
    frame = Image.new('RGB', (128, 128), (240, 248, 255))
    draw = ImageDraw.Draw(frame)

    # Desenhar sua animação usando primitivas do PIL
    # (círculos, polígonos, linhas, etc.)

    builder.add_frame(frame)

# 3. Salvar com otimização
builder.save('output.gif', num_colors=48, optimize_for_emoji=True)
```

## Desenhando Gráficos

### Trabalhando com Imagens Enviadas pelo Usuário
Se um usuário enviar uma imagem, considere se ele quer:
- **Usá-la diretamente** (ex: "animar isto", "dividir em frames")
- **Usá-la como inspiração** (ex: "fazer algo parecido com isto")

Carregue e trabalhe com imagens usando PIL:
```python
from PIL import Image

uploaded = Image.open('file.png')
# Usar diretamente, ou apenas como referência de cores/estilo
```

### Desenhando do Zero
Ao desenhar gráficos do zero, use primitivas do PIL ImageDraw:

```python
from PIL import ImageDraw

draw = ImageDraw.Draw(frame)

# Círculos/óvalos
draw.ellipse([x1, y1, x2, y2], fill=(r, g, b), outline=(r, g, b), width=3)

# Estrelas, triângulos, qualquer polígono
points = [(x1, y1), (x2, y2), (x3, y3), ...]
draw.polygon(points, fill=(r, g, b), outline=(r, g, b), width=3)

# Linhas
draw.line([(x1, y1), (x2, y2)], fill=(r, g, b), width=5)

# Retângulos
draw.rectangle([x1, y1, x2, y2], fill=(r, g, b), outline=(r, g, b), width=3)
```

**Não use:** Fontes de emoji (confiabilidade inconsistente entre plataformas) ou presuma que gráficos pré-empacotados existem nesta skill.

### Deixando Gráficos com Boa Aparência

Gráficos devem parecer polidos e criativos, não básicos. Aqui está como:

**Use linhas mais espessas** - Sempre defina `width=2` ou superior para contornos e linhas. Linhas finas (width=1) parecem entrecortadas e amadoras.

**Adicione profundidade visual**:
- Use gradientes para fundos (`create_gradient_background`)
- Sobreponha múltiplas formas para complexidade (ex: uma estrela com uma estrela menor dentro)

**Torne as formas mais interessantes**:
- Não apenas desenhe um círculo simples - adicione destaques, anéis ou padrões
- Estrelas podem ter brilhos (desenhe versões maiores e semi-transparentes atrás)
- Combine múltiplas formas (estrelas + brilhos, círculos + anéis)

**Preste atenção às cores**:
- Use cores vibrantes e complementares
- Adicione contraste (contornos escuros em formas claras, contornos claros em formas escuras)
- Considere a composição geral

**Para formas complexas** (corações, flocos de neve, etc.):
- Use combinações de polígonos e elipses
- Calcule pontos cuidadosamente para simetria
- Adicione detalhes (um coração pode ter uma curva de destaque, flocos de neve têm galhos intricados)

Seja criativo e detalhista! Um bom GIF para Slack deve parecer polido, não como gráficos de espaço reservado.

## Utilitários Disponíveis

### GIFBuilder (`core.gif_builder`)
Monta frames e otimiza para Slack:
```python
builder = GIFBuilder(width=128, height=128, fps=10)
builder.add_frame(frame)  # Adicionar PIL Image
builder.add_frames(frames)  # Adicionar lista de frames
builder.save('out.gif', num_colors=48, optimize_for_emoji=True, remove_duplicates=True)
```

### Validadores (`core.validators`)
Verifique se o GIF atende aos requisitos do Slack:
```python
from core.validators import validate_gif, is_slack_ready

# Validação detalhada
passes, info = validate_gif('my.gif', is_emoji=True, verbose=True)

# Verificação rápida
if is_slack_ready('my.gif'):
    print("Pronto!")
```

### Funções de Easing (`core.easing`)
Movimento suave em vez de linear:
```python
from core.easing import interpolate

# Progresso de 0.0 a 1.0
t = i / (num_frames - 1)

# Aplicar easing
y = interpolate(start=0, end=400, t=t, easing='ease_out')

# Disponíveis: linear, ease_in, ease_out, ease_in_out,
#             bounce_out, elastic_out, back_out
```

### Auxiliares de Frame (`core.frame_composer`)
Funções de conveniência para necessidades comuns:
```python
from core.frame_composer import (
    create_blank_frame,         # Fundo com cor sólida
    create_gradient_background,  # Gradiente vertical
    draw_circle,                # Auxiliar para círculos
    draw_text,                  # Renderização simples de texto
    draw_star                   # Estrela de 5 pontas
)
```

## Conceitos de Animação

### Shake/Vibração
Deslocamento de posição do objeto com oscilação:
- Use `math.sin()` ou `math.cos()` com índice de frame
- Adicione pequenas variações aleatórias para sensação natural
- Aplique à posição x e/ou y

### Pulse/Batida do Coração
Dimensione o objeto ritmicamente:
- Use `math.sin(t * frequency * 2 * math.pi)` para pulso suave
- Para batida: dois pulsos rápidos e pausa (ajuste a onda seno)
- Dimensione entre 0.8 e 1.2 do tamanho base

### Bounce
Objeto cai e quica:
- Use `interpolate()` com `easing='bounce_out'` para pouso
- Use `easing='ease_in'` para queda (aceleração)
- Aplique gravidade aumentando a velocidade y a cada frame

### Spin/Rotação
Rotacione objeto ao redor do centro:
- PIL: `image.rotate(angle, resample=Image.BICUBIC)`
- Para bambolear: use onda seno para ângulo em vez de linear

### Fade In/Out
Apareça ou desapareça gradualmente:
- Crie imagem RGBA, ajuste canal alpha
- Ou use `Image.blend(image1, image2, alpha)`
- Fade in: alpha de 0 a 1
- Fade out: alpha de 1 a 0

### Slide
Mova objeto de fora da tela para posição:
- Posição inicial: fora dos limites do frame
- Posição final: localização alvo
- Use `interpolate()` com `easing='ease_out'` para parada suave
- Para ultrapassagem: use `easing='back_out'`

### Zoom
Dimensione e posicione para efeito de zoom:
- Zoom in: dimensione de 0.1 a 2.0, recorte centro
- Zoom out: dimensione de 2.0 a 1.0
- Pode adicionar motion blur para dramaticidade (filtro PIL)

### Explode/Rajada de Partículas
Crie partículas irradiando para fora:
- Gere partículas com ângulos e velocidades aleatórios
- Atualize cada partícula: `x += vx`, `y += vy`
- Adicione gravidade: `vy += gravity_constant`
- Diminua partículas com o tempo (reduza alpha)

## Estratégias de Otimização

Apenas quando solicitado a reduzir o tamanho do arquivo, implemente alguns dos seguintes métodos:

1. **Menos frames** - FPS menor (10 em vez de 20) ou duração menor
2. **Menos cores** - `num_colors=48` em vez de 128
3. **Dimensões menores** - 128x128 em vez de 480x480
4. **Remover duplicatas** - `remove_duplicates=True` em save()
5. **Modo emoji** - `optimize_for_emoji=True` otimiza automaticamente

```python
# Otimização máxima para emoji
builder.save(
    'emoji.gif',
    num_colors=48,
    optimize_for_emoji=True,
    remove_duplicates=True
)
```

## Filosofia

Esta skill fornece:
- **Conhecimento**: Requisitos do Slack e conceitos de animação
- **Utilitários**: GIFBuilder, validadores, funções de easing
- **Flexibilidade**: Crie a lógica de animação usando primitivas PIL

Ela NÃO fornece:
- Templates rígidos de animação ou funções pré-feitas
- Renderização de fonte de emoji (não confiável entre plataformas)
- Uma biblioteca de gráficos pré-empacotados integrados à skill

**Nota sobre uploads de usuários**: Esta skill não inclui gráficos pré-construídos, mas se um usuário carregar uma imagem, use PIL para carregá-la e trabalhar com ela - interprete baseado em sua solicitação se ele quer usá-la diretamente ou apenas como inspiração.

Seja criativo! Combine conceitos (quique + rotação, pulso + deslizamento, etc.) e use as capacidades completas do PIL.

## Dependências

```bash
pip install pillow imageio numpy
```