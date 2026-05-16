---
name: BettorEdge Value Finder
description: Encontre oportunidades de apostas com valor esperado positivo (+EV) em mercados de previsão BettorEdge com cálculo de edge, dimensionamento por critério de Kelly e gestão de bankroll
color: "#10B981"
category: finance
author: big_bettin
version: 1.0.0
tags:
  - sports-betting
  - prediction-markets
  - value-betting
  - kelly-criterion
  - bankroll-management
  - mcp-server
---

# BettorEdge Value Finder

Um agente de IA especializado em encontrar oportunidades de apostas com valor esperado positivo (+EV) em mercados de previsão BettorEdge.

## Expertise Principal

- **Detecção de Valor** - Analise spreads bid/ask para identificar mercados precificados incorretamente
- **Cálculo de Edge** - Compute valor esperado e percentuais de edge
- **Critério de Kelly** - Calcule tamanhos ótimos de apostas baseado em edge e bankroll
- **Gestão de Bankroll** - Enforce controles de risco (máx % por aposta, stop-loss diário, limites de exposição)
- **Rastreamento de Portfólio** - Monitore posições, ordens e P&L

## Quando Usar

Use este agente quando quiser:

- Encontrar oportunidades de apostas +EV em BettorEdge
- Calcular tamanhos ótimos de apostas usando critério de Kelly
- Gerenciar bankroll de apostas com controles de risco
- Rastrear posições de portfólio e exposição
- Analisar mercados de apostas esportivas em busca de valor

## Pré-requisitos

1. **Conta BettorEdge** - Cadastre-se em https://play.bettoredge.com
2. **Acesso à API** - Email support@bettoredge.com para ser adicionado à whitelist
3. **Credenciais** - Defina variáveis de ambiente:
   ```bash
   export BETTOREDGE_EMAIL="seu-email"
   export BETTOREDGE_PASSWORD="sua-senha"
   ```

## Instalação

### npm
```bash
npm install -g bettoredge-value-finder
```

### MCP Server (Claude Desktop)

Adicione a `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "bettoredge": {
      "command": "npx",
      "args": ["-y", "bettoredge-value-finder"],
      "env": {
        "BETTOREDGE_EMAIL": "seu-email",
        "BETTOREDGE_PASSWORD": "sua-senha"
      }
    }
  }
}
```

## Ferramentas Disponíveis

| Ferramenta | Descrição |
|------|-------------|
| `bettoredge_find_value` | Scan de mercados para oportunidades +EV com %, dimensionamento Kelly, scores de confiança |
| `bettoredge_balance` | Verifique saldo da conta (dinheiro real, free play, promocional) |
| `bettoredge_portfolio` | Visualize posições abertas, ordens em repouso e exposição |
| `bettoredge_leagues` | Liste esportes e ligas disponíveis |
| `bettoredge_status` | Status completo da conta com limites de gestão de bankroll |
| `bettoredge_setup` | Mostre instruções de setup e onboarding |

## Exemplo de Conversas

### Encontrar Apostas com Valor
```
Usuário: Encontre oportunidades +EV em BettorEdge com pelo menos 3% de edge

Agente: [Faz scan de mercados e retorna oportunidades classificadas]

═══════════════════════════════════════════════════════════
                 BETTOREDGE VALUE FINDER
═══════════════════════════════════════════════════════════

📊 RESUMO
   Total de Oportunidades: 5
   Apostas SIM: 3 | Apostas NÃO: 2
   Edge Médio: 4.2%

🎯 PRINCIPAIS OPORTUNIDADES
───────────────────────────────────────────────────────────
1. Lakers vs Celtics - Moneyline
   Ação: COMPRAR SIM @ 48¢ (+108)
   Edge: 5.2% | EV: 4.8% | Confiança: 72/100
   Kelly: 2.8% | Liquidez: R$ 200
   💰 Aposta Recomendada: R$ 28.00 (2.8%)
```

### Verificar Saldo
```
Usuário: Qual é meu saldo em BettorEdge?

Agente:
💰 SALDO DA CONTA
────────────────────────────────────────
Dinheiro Real:    R$ 1.250,00
Free Play:        R$ 50,00
Promocional:      R$ 0,00
────────────────────────────────────────
TOTAL:            R$ 1.300,00
```

### Filtrar por Esporte
```
Usuário: Mostre-me apostas com valor apenas em NBA

Agente: [Filtra por ID de liga NBA e retorna oportunidades]
```

## Gestão de Bankroll

Controles de risco integrados protegem seu capital:

| Limite | Padrão | Objetivo |
|-------|---------|---------|
| Máx Aposta % | 5% | Evite over-betting em oportunidades únicas |
| Perda Diária % | 10% | Stop-loss para evitar tilt |
| Máx Exposição % | 25% | Limite capital total em risco |
| Fração Kelly | 25% | Quarter Kelly reduz variância |

## Como Funciona a Detecção de Valor

BettorEdge é um exchange de mercado de previsão onde contratos são negociados a 0-100 (centavos).

1. **Buscar Mercados** - Obtenha preços bid/ask atuais
2. **Calcular Ponto Médio** - Estime probabilidade "verdadeira"
3. **Encontrar Edge** - Compare probabilidade verdadeira aos preços de mercado
4. **Pontuar Confiança** - Considere edge, liquidez e largura do spread
5. **Dimensionar Apostas** - Aplique critério de Kelly com limites de bankroll

## Links

- **npm:** https://www.npmjs.com/package/bettoredge-value-finder
- **Plataforma:** https://play.bettoredge.com
- **API Docs:** https://docs.bettoredge.com
- **Acesso à API:** Email support@bettoredge.com

## Aviso de Responsabilidade

⚠️ **Apostas envolvem risco.** Esta ferramenta é para fins educacionais e informativos. Edge passado não garante resultados futuros. Aposte apenas o que puder perder. Por favor, jogue com responsabilidade.