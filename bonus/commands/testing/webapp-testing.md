---
allowed-tools: Bash, Read
description: Toolkit para interagir com e testar aplicações web locais usando Playwright. Suporta verificação de funcionalidade frontend, depuração de comportamento de UI, captura de screenshots do navegador e visualização de logs do navegador.
---

# Teste de Aplicações Web

Esta skill permite testes e depuração abrangentes de aplicações web locais usando automação Playwright.

## Quando Usar Esta Skill

Use esta skill quando você precisar de:
- Testar funcionalidade frontend em um navegador real
- Verificar comportamento e interações de UI
- Depurar problemas em aplicações web
- Capturar screenshots para documentação ou depuração
- Inspecionar logs do console do navegador
- Validar envios de formulários e fluxos de usuário
- Verificar design responsivo em diferentes viewports

## Pré-requisitos

- Node.js instalado no sistema
- Uma aplicação web executando localmente (ou URL acessível)
- Playwright será instalado automaticamente se não estiver presente

## Capacidades Principais

### 1. Automação de Navegador
- Navegar para URLs
- Clicar botões e links
- Preencher campos de formulário
- Selecionar dropdowns
- Lidar com diálogos e alertas

### 2. Verificação
- Afirmar presença de elementos
- Verificar conteúdo de texto
- Verificar visibilidade de elementos
- Validar URLs
- Testar comportamento responsivo

### 3. Depuração
- Capturar screenshots
- Visualizar logs do console
- Inspecionar requisições de rede
- Depurar testes com falha

## Exemplos de Uso

### Exemplo 1: Teste Básico de Navegação
```javascript
// Navigate to a page and verify title
await page.goto('http://localhost:3000');
const title = await page.title();
console.log('Page title:', title);
```

### Exemplo 2: Interação com Formulário
```javascript
// Fill out and submit a form
await page.fill('#username', 'testuser');
await page.fill('#password', 'password123');
await page.click('button[type="submit"]');
await page.waitForURL('**/dashboard');
```

### Exemplo 3: Captura de Screenshot
```javascript
// Capture a screenshot for debugging
await page.screenshot({ path: 'debug.png', fullPage: true });
```

## Diretrizes

1. **Sempre verifique se a app está em execução** - Verifique se o servidor local está acessível antes de executar testes
2. **Use waits explícitos** - Aguarde elementos ou navegação serem concluídos antes de interagir
3. **Capture screenshots em caso de falha** - Tire screenshots para ajudar a depurar problemas
4. **Limpe recursos** - Sempre feche o navegador quando terminar
5. **Trate timeouts adequadamente** - Defina timeouts razoáveis para operações lentas
6. **Teste incrementalmente** - Comece com interações simples antes de fluxos complexos
7. **Use seletores com sabedoria** - Prefira seletores data-testid ou baseados em role em vez de classes CSS

## Padrões Comuns

### Padrão: Aguardar Elemento
```javascript
await page.waitForSelector('#element-id', { state: 'visible' });
```

### Padrão: Verificar se Elemento Existe
```javascript
const exists = await page.locator('#element-id').count() > 0;
```

### Padrão: Obter Logs do Console
```javascript
page.on('console', msg => console.log('Browser log:', msg.text()));
```

### Padrão: Tratar Erros
```javascript
try {
  await page.click('#button');
} catch (error) {
  await page.screenshot({ path: 'error.png' });
  throw error;
}
```

## Limitações

- Requer ambiente Node.js
- Não pode testar apps nativas mobile (use React Native Testing Library em seu lugar)
- Pode ter problemas com fluxos de autenticação complexos
- Alguns frameworks modernos podem exigir configuração específica