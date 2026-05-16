---
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, AskUserQuestion
argument-hint: <detalhes da nota fiscal ou subcomando>
description: Emitir, cancelar e buscar notas fiscais húngaras via API do Agent szamlazz.hu — com busca de contribuinte NAV e cache automático de parceiros
---

# /szamlazz — Emissão de Notas Fiscais Húngaras via szamlazz.hu

Emita, cancele (storno) e baixe notas fiscais húngaras do Claude Code usando a [API do Agent szamlazz.hu](https://www.szamlazz.hu/): $ARGUMENTS

Parte do plugin [socialpro-szamlazz](https://github.com/socialproKGCMG/socialpro-szamlazz) by [SocialPro](https://www.socialpro.hu) — agência húngara de automação com IA e marketing digital.

## Objetivo

Emissão de notas fiscais húngaras (számlázás) via szamlazz.hu é uma das maiores perdas de tempo para PMEs húngaras em SaaS. A API do Agent é apenas XML e não documentada em inglês. Este comando transforma emissão de notas em um prompt de linguagem natural:

> "állíts ki egy 150 000 Ft-os számlát Példa Kft.-nek webfejlesztésről"

## Recursos

- **Tipos de nota**: regular, proforma (díjbekérő) e storno (cancelamento)
- **Busca de contribuinte NAV**: digite um número de imposto húngaro, obtenha nome e endereço da Autoridade Tributária Nacional
- **Cache de parceiros**: clientes memorizados por ID fiscal para reuso instantâneo
- **IVA húngaro**: 27% / 18% / 5% / 0% / AAM, com suporte KATA (pequeno contribuinte)
- **Multiplataforma**: macOS, Linux, Windows — Python 3.9+ e PyYAML apenas
- **Setup interativo**: primeira execução se configura em 30 segundos via 3 perguntas
- **Seguro**: chave de API armazenada no credential store do SO

## Uso

```bash
# Instale o plugin
/plugin marketplace add socialproKGCMG/socialpro-plugins
/plugin install szamlazz@socialpro-plugins

# Emita uma nota fiscal
/szamlazz állíts ki egy számlát Példa Kft.-nek 150 000 Ft-ról webfejlesztésről

# Cancele uma nota fiscal
/szamlazz sztornózd a SOC-2026-0042 számlát

# Proforma / díjbekérő
/szamlazz díjbekérő Acme Ltd-nek 500 EUR-ról konzultációért

# Baixe PDF
/szamlazz töltsd le a SOC-2026-0042 PDF-jét

# Busca de contribuinte NAV
/szamlazz ki ez a cég: 12345678-2-42
```

## Implementação

O comando ativa em palavras-chave húngaras E inglesas (számla, invoice, sztornó, storno, díjbekérő, proforma, etc.). Na primeira execução, detecta configuração faltante e guia por um setup interativo:

1. **Chave de API do Agent** — das configurações de szamlazz.hu
2. **Número de imposto do vendedor** — busca automática de dados da empresa via NAV
3. **Conta bancária** — detecção automática de nome do banco pelo prefixo giro

Após setup, notas fiscais seguem um fluxo rigoroso:
1. Carregue configuração do vendedor de `seller.yaml`
2. Resolva cliente (cache de parceiros → busca NAV → entrada manual)
3. Colete itens de linha com alíquota de IVA
4. Exiba resumo de confirmação (obrigatório — notas fiscais são documentos legais)
5. Envie XML para a API do Agent szamlazz.hu
6. Salve PDF localmente, atualize cache de parceiros

Todos os valores usam `Decimal` com `ROUND_HALF_UP` para 2 casas decimais — a API szamlazz.hu rejeita desacordos de cálculo.

## Tratamento de Erros

Os 7 códigos de erro szamlazz.hu mais comuns são traduzidos para o húngaro com recuperação acionável:

| Código | Significado | Recuperação |
|---:|---|---|
| 3 | Autenticação falhou | Regenere chave de Agent |
| 54, 55 | Cert e-Számla | Tente novamente com eszamla=false |
| 57, 259-264 | Desacordo de cálculo | Recalcule com Decimal |
| 136 | Saldo não pago | Pague inscrição szamlazz.hu |

## Requisitos

- Python 3.9+
- PyYAML (`pip install pyyaml`)
- Conta szamlazz.hu com chave de API do Agent

## Links

- **Repositório do plugin**: [github.com/socialproKGCMG/socialpro-szamlazz](https://github.com/socialproKGCMG/socialpro-szamlazz)
- **Página inicial do plugin**: [socialpro.hu/claude-code-plugins/szamlazz](https://www.socialpro.hu/claude-code-plugins/szamlazz)
- **Docs da API szamlazz.hu**: [szamlazz.hu](https://www.szamlazz.hu/)
- **Autor**: [SocialPro — Agência húngara de automação com IA](https://www.socialpro.hu)