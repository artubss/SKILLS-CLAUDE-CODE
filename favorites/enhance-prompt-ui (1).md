# Enhance Prompt — UI Design Thinking

## Quando ativar

Use este skill **antes de criar qualquer UI/componente/página** para transformar ideias vagas em especificações precisas que geram resultados 10x melhores.

---

## Pipeline de 4 Passos

### Passo 1 — Avaliar o que falta

Antes de codificar, pergunte (ou infira) sobre:

| Dimensão | Perguntas |
|---|---|
| **Plataforma** | Web desktop? Mobile? PWA? |
| **Tipo de página** | Landing? Dashboard? Auth? E-commerce? |
| **Visual style** | Dark/light? Minimal/rico? Corporativo/moderno? |
| **Cores** | Tem brand colors? Paleta definida? |
| **Componentes chave** | Quais elementos são indispensáveis? |
| **Tom/atmosfera** | Sério? Playful? Premium? Técnico? |

### Passo 2 — Verificar Design System existente

- **Se tem DESIGN.md / tokens / Tailwind config**: extraia e use como base
- **Se não tem**: defina antes de criar qualquer componente para garantir consistência

### Passo 3 — Aplicar Enhancements

**Substituir termos vagos por específicos:**

| ❌ Vago | ✅ Específico |
|---|---|
| "botão" | "primary CTA button com gradient indigo-violet" |
| "moderno" | "clean, minimal, generous whitespace, subtle shadows" |
| "menu" | "sticky navigation bar com glassmorphism e backdrop-blur" |
| "card" | "glassmorphism card, border white/5, bg white/3, hover glow" |
| "bonito" | "dark premium, indigo accent, gradient text, glow effects" |
| "input" | "rounded-xl, dark bg, indigo focus ring, placeholder zinc-500" |

**Amplificar a atmosfera:**
```
❌ "página escura com gradiente"
✅ "dark premium SaaS — bg #09090b, orbs de gradiente animados (indigo/violet/purple),
   grid pattern overlay 60px, noise texture opacity-4, hero text 8xl bold com
   gradient branco→zinc-400, headline accent em indigo→violet"
```

**Estruturar em seções numeradas:**
```
1. Navigation — sticky, glassmorphism, logo + nav links + CTA
2. Hero — centered, badge pulsante, H1 gradient, subtitle, 2 CTAs, scroll indicator
3. Stats — 4 contadores animados em grid, separator
4. Features — grid 3 colunas, glassmorphism cards, ícone + título + descrição
5. Pricing — 2 colunas, card destaque com gradient border + glow
6. FAQ — accordion minimal, border bottom
7. Footer — logo + links + copyright
```

### Passo 4 — Formato de Output

```
[DESCRIÇÃO EM UMA LINHA]
Ex: "Landing page premium dark para SaaS de skills para Claude Code"

[DESIGN SYSTEM]
- Plataforma: Web desktop + mobile responsive
- Tema: Dark premium (#09090b background)
- Cores: Indigo (#6366f1), Violet (#8b5cf6), Zinc scale
- Typography: Font-black para headlines, zinc-400 para body
- Componentes: Cards glassmorphism, Glow buttons, Badge pulsante
- Atmosfera: Premium, técnico, energético, confiável

[ESTRUTURA DA PÁGINA]
[seções numeradas com descrição de cada]
```

---

## Vocabulário UI/UX Essencial

### Efeitos visuais
- **Glassmorphism**: `bg-white/5 backdrop-blur-md border-white/10`
- **Glow button**: multi-layer blur + gradient + shadow on hover
- **Orb**: `rounded-full bg-purple-600/20 blur-[120px] animate-pulse`
- **Noise texture**: `background-image: url(svg filter feTurbulence)`
- **Grid overlay**: `background linear-gradient` 1px lines em opacity-3

### Animações
- **Fade + slide up**: `initial={{ opacity:0, y:30 }} animate={{ opacity:1, y:0 }}`
- **Count up**: contador de 0 ao valor real ao entrar no viewport
- **Ping badge**: `animate-ping` no indicador ao vivo
- **Parallax hero**: texto some ao scrollar com `useScroll + useTransform`
- **Stagger children**: delay progressivo em listas/grids

### Tokens de cor (dark premium)
- Background: `#09090b` (zinc-950)
- Cards: `bg-white/[0.03]` + `border-white/5`
- Text primary: `white`
- Text secondary: `zinc-400`
- Text muted: `zinc-600`
- Accent: `indigo-400` / `violet-400`

---

## Checklist antes de codificar

```
[ ] Design system definido (cores, tipografia, espaçamento)
[ ] Atmosfera/mood claramente descrita
[ ] Componentes-chave listados
[ ] Estados de hover/focus/active pensados
[ ] Mobile responsiveness planejado
[ ] Animações e transições definidas
[ ] Hierarquia visual clara (o que o usuário vê primeiro)
[ ] CTA principal óbvio e destacado
```


---
<!-- SkillVaultPro · downloaded by: arthurbezerra5000@gmail.com · at: 2026-05-11T12:55:02.498Z · skill: enhance-prompt-ui -->
