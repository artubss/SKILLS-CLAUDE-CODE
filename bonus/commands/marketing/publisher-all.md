---
allowed-tools: Read, Write, Bash, Glob, WebFetch
argument-hint: <input> [lang]
description: Gerar conteúdo para TODAS as plataformas simultaneamente (X, LinkedIn, Medium, Dev.to)
---

# Gerador de Conteúdo Multiplataforma

Gere conteúdo para X/Twitter, LinkedIn, Medium e Dev.to em um único comando.

**Uso:** `$ARGUMENTS`

**Exemplos:**
```bash
/publisher:all meu-post              # Todas as plataformas, inglês
/publisher:all meu-post ja           # Todas as plataformas, japonês
/publisher:all artigo.md             # A partir do caminho do arquivo
```

**O que faz:**
Executa todos os comandos de publicação sequencialmente:
1. `/publisher:x` - Thread X/Twitter (3 versões: thread, longo, curto)
2. `/publisher:linkedin` - Post LinkedIn com anexo de mídia
3. `/publisher:medium` - Artigo pronto para Medium em HTML
4. `/publisher:devto` - Feed RSS Dev.to (se ainda não foi gerado)

**Processo:**

Para cada plataforma:
1. Interpreta a mesma entrada (slug, arquivo ou URL)
2. Gera conteúdo otimizado para a plataforma
3. Cria previews em HTML
4. Abre todos os previews em abas do navegador

**Saída:**
- `x-thread-[LANG].html` - Thread X com 3 abas de formato
- Draft do post LinkedIn (via API) ou preview em HTML
- `medium-article-[LANG].html` - Conteúdo pronto para Medium
- `rss-devto.xml` - Feed RSS completo

**Economia de Tempo:**
Em vez de executar 4 comandos separados e gastar ~2 horas adaptando conteúdo manualmente, isso gera tudo de uma vez.

**Próximos Passos:**
1. As abas do navegador abrem automaticamente
2. Revise o conteúdo de cada plataforma
3. Copie e cole ou publique via API
4. Ajuste conforme necessário para sua audiência

**Observação**: O conteúdo de cada plataforma é otimizado de forma única — não é apenas copiar e colar entre canais.