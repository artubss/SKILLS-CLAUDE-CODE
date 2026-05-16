---
name: webapp-testing
description: Kit de ferramentas para interagir e testar aplicações web locais usando Playwright. Suporta verificação de funcionalidade frontend, depuração de comportamento de UI, captura de screenshots do navegador e visualização de logs do navegador.
license: Termos completos em LICENSE.txt
---

# Testes de Aplicação Web

Para testar aplicações web locais, escreva scripts nativos em Python com Playwright.

**Scripts auxiliares disponíveis**:
- `scripts/with_server.py` - Gerencia ciclo de vida do servidor (suporta múltiplos servidores)

**Sempre execute scripts com `--help` primeiro** para ver o uso. NÃO leia o código-fonte até tentar executar o script e descobrir que uma solução personalizada é absolutamente necessária. Esses scripts podem ser muito grandes e poluir sua janela de contexto. Existem para serem chamados diretamente como scripts black-box em vez de serem incorporados à sua janela de contexto.

## Árvore de Decisão: Escolhendo sua Abordagem

```
Tarefa do usuário → É HTML estático?
    ├─ Sim → Leia o arquivo HTML diretamente para identificar seletores
    │        ├─ Sucesso → Escreva script Playwright usando seletores
    │        └─ Falha/Incompleto → Trate como dinâmico (abaixo)
    │
    └─ Não (webapp dinâmico) → O servidor já está em execução?
        ├─ Não → Execute: python scripts/with_server.py --help
        │        Depois use o auxiliar + escreva script Playwright simplificado
        │
        └─ Sim → Reconhecimento-depois-ação:
            1. Navegue e aguarde networkidle
            2. Tire screenshot ou inspecione DOM
            3. Identifique seletores do estado renderizado
            4. Execute ações com seletores descobertos
```

## Exemplo: Usando with_server.py

Para iniciar um servidor, execute `--help` primeiro, depois use o auxiliar:

**Servidor único:**
```bash
python scripts/with_server.py --server "npm run dev" --port 5173 -- python your_automation.py
```

**Múltiplos servidores (ex: backend + frontend):**
```bash
python scripts/with_server.py \
  --server "cd backend && python server.py" --port 3000 \
  --server "cd frontend && npm run dev" --port 5173 \
  -- python your_automation.py
```

Para criar um script de automação, inclua apenas lógica Playwright (servidores são gerenciados automaticamente):
```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(headless=True) # Sempre inicie chromium em modo headless
    page = browser.new_page()
    page.goto('http://localhost:5173') # Servidor já em execução e pronto
    page.wait_for_load_state('networkidle') # CRÍTICO: Aguarde JS executar
    # ... sua lógica de automação
    browser.close()
```

## Padrão Reconhecimento-Depois-Ação

1. **Inspecione DOM renderizado**:
   ```python
   page.screenshot(path='/tmp/inspect.png', full_page=True)
   content = page.content()
   page.locator('button').all()
   ```

2. **Identifique seletores** dos resultados da inspeção

3. **Execute ações** usando seletores descobertos

## Armadilha Comum

❌ **Não** inspecione o DOM antes de aguardar `networkidle` em apps dinâmicos
✅ **Faça** aguardar `page.wait_for_load_state('networkidle')` antes da inspeção

## Melhores Práticas

- **Use scripts fornecidos como black boxes** - Para realizar uma tarefa, considere se um dos scripts disponíveis em `scripts/` pode ajudar. Esses scripts tratam workflows comuns e complexos de forma confiável sem poluir a janela de contexto. Use `--help` para ver o uso, depois invoque diretamente.
- Use `sync_playwright()` para scripts síncronos
- Sempre feche o navegador quando terminar
- Use seletores descritivos: `text=`, `role=`, seletores CSS ou IDs
- Adicione waits apropriados: `page.wait_for_selector()` ou `page.wait_for_timeout()`

## Arquivos de Referência

- **examples/** - Exemplos mostrando padrões comuns:
  - `element_discovery.py` - Descobrindo botões, links e inputs em uma página
  - `static_html_automation.py` - Usando URLs file:// para HTML local
  - `console_logging.py` - Capturando logs do console durante automação