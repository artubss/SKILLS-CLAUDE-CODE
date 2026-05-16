---
name: "playwright"
description: "Use when the task requires automating a real browser from the terminal (navigation, form filling, snapshots, screenshots, data extraction, UI-flow debugging) via `playwright-cli` or the bundled wrapper script."
author: openai
---


# Habilidade Playwright CLI

Automatize um navegador real a partir do terminal usando `playwright-cli`. Prefira o script wrapper agrupado para que o CLI funcione mesmo quando não estiver instalado globalmente.
Trate essa habilidade como automação baseada em CLI. Não pivote para `@playwright/test` a menos que o usuário solicite explicitamente arquivos de teste.

## Verificação de pré-requisitos (obrigatória)

Antes de propor comandos, verifique se `npx` está disponível (o wrapper depende disso):

```bash
command -v npx >/dev/null 2>&1
```

Se não estiver disponível, pause e peça ao usuário para instalar Node.js/npm (que fornece `npx`). Forneça estas etapas textualmente:

```bash
# Verify Node/npm are installed
node --version
npm --version

# If missing, install Node.js/npm, then:
npm install -g @playwright/cli@latest
playwright-cli --help
```

Assim que `npx` estiver presente, prossiga com o script wrapper. Uma instalação global de `playwright-cli` é opcional.

## Caminho da habilidade (definido uma vez)

```bash
export CODEX_HOME="${CODEX_HOME:-$HOME/.codex}"
export PWCLI="$CODEX_HOME/skills/playwright/scripts/playwright_cli.sh"
```

Skills com escopo de usuário são instaladas em `$CODEX_HOME/skills` (padrão: `~/.codex/skills`).

## Início rápido

Use o script wrapper:

```bash
"$PWCLI" open https://playwright.dev --headed
"$PWCLI" snapshot
"$PWCLI" click e15
"$PWCLI" type "Playwright"
"$PWCLI" press Enter
"$PWCLI" screenshot
```

Se o usuário preferir uma instalação global, isso também é válido:

```bash
npm install -g @playwright/cli@latest
playwright-cli --help
```

## Fluxo de trabalho principal

1. Abra a página.
2. Capture snapshot para obter referências estáveis de elementos.
3. Interaja usando referências do snapshot mais recente.
4. Re-capture snapshot após navegação ou mudanças significativas no DOM.
5. Capture artefatos (screenshot, pdf, traces) quando útil.

Loop mínimo:

```bash
"$PWCLI" open https://example.com
"$PWCLI" snapshot
"$PWCLI" click e3
"$PWCLI" snapshot
```

## Quando re-capturar snapshot

Capture snapshot novamente após:

- navegação
- clicar em elementos que mudam substancialmente a UI
- abrir/fechar modais ou menus
- mudanças de abas

As referências podem ficar obsoletas. Quando um comando falhar por uma referência ausente, capture snapshot novamente.

## Padrões recomendados

### Preenchimento de formulário e envio

```bash
"$PWCLI" open https://example.com/form
"$PWCLI" snapshot
"$PWCLI" fill e1 "user@example.com"
"$PWCLI" fill e2 "password123"
"$PWCLI" click e3
"$PWCLI" snapshot
```

### Debugar fluxo de UI com traces

```bash
"$PWCLI" open https://example.com --headed
"$PWCLI" tracing-start
# ...interactions...
"$PWCLI" tracing-stop
```

### Trabalho com múltiplas abas

```bash
"$PWCLI" tab-new https://example.com
"$PWCLI" tab-list
"$PWCLI" tab-select 0
"$PWCLI" snapshot
```

## Script wrapper

O script wrapper usa `npx --package @playwright/cli playwright-cli` para que o CLI funcione sem uma instalação global:

```bash
"$PWCLI" --help
```

Prefira o wrapper a menos que o repositório já padronize uma instalação global.

## Referências

Abra apenas o que você precisa:

- Referência de comandos CLI: `references/cli.md`
- Fluxos de trabalho práticos e solução de problemas: `references/workflows.md`

## Diretrizes de segurança

- Sempre capture snapshot antes de referenciar ids de elementos como `e12`.
- Re-capture snapshot quando as referências parecerem obsoletas.
- Prefira comandos explícitos sobre `eval` e `run-code` a menos que necessário.
- Quando você não tiver um snapshot recente, use referências placeholder como `eX` e explique por que; não contorne referências com `run-code`.
- Use `--headed` quando uma verificação visual ajudar.
- Ao capturar artefatos neste repositório, use `output/playwright/` e evite introduzir novas pastas de artefatos no nível superior.
- Padrão para comandos CLI e fluxos de trabalho, não specs de teste do Playwright.