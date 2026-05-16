---
description: Análise profunda multidisciplinar: expõe suposições ocultas, gera soluções concorrentes, testa cada uma com raciocínio adversarial e entrega recomendações calibradas por confiança
argument-hint: [problema ou pergunta a analisar]
---

# Modo de Análise Profunda e Resolução de Problemas

Modo de análise profunda e resolução de problemas

## Instruções

Analise o problema ou pergunta fornecida: **$ARGUMENTS**

Antes de prosseguir, identifique: o desafio central, as principais restrições, as suposições implícitas e quem é afetado pelo resultado.

**Antes de começar a análise**, verifique se $ARGUMENTS fornece contexto suficiente:
- Se o problema é específico e o domínio é claro, prossiga imediatamente para a análise.
- Se contexto crítico está faltando (por exemplo, o domínio, as restrições ou os objetivos de quem toma a decisão), faça até três perguntas focadas antes de prosseguir. Não faça perguntas desnecessárias.

## Elementos de Análise Obrigatórios

Sua análise deve abordar todos os itens a seguir. A ordem e profundidade são suas para determinar com base no problema:

- **Enquadramento do problema**: O que está realmente sendo perguntado? Quais suposições estão embutidas na pergunta?
- **Soluções concorrentes**: Pelo menos 3 abordagens significativamente diferentes, não variações da mesma ideia.
- **Avaliação multidimensional**: Avalie cada solução através das lentes mais relevantes para este problema (técnica, econômica, humana, sistêmica, temporal — selecione e justifique quais se aplicam).
- **Teste adversarial**: Para cada solução principal, argumente contra ela. O que teria que ser verdade para que falhasse miseravelmente? Use inversão — pergunta o que você faria para garantir o fracasso e, em seguida, garanta que a recomendação evite esses caminhos.
- **Insight transdisciplinar**: Extraia pelo menos um paralelo não óbvio de um campo ou disciplina diferente.
- **Efeitos de segunda ordem**: O que cada abordagem torna mais ou menos provável que aconteça em 6 meses, 2 anos, 10 anos?
- **Síntese**: Qual abordagem ou combinação é recomendada? Por quê, dados os trade-offs específicos?
- **Calibração de confiança**: Para cada afirmação-chave, observe onde a incerteza é alta e o que mudaria a recomendação.

## Template de Saída Estruturada

Apresente os achados usando esta estrutura:

```
## Análise do Problema
- Desafio central
- Principais restrições
- Fatores críticos de sucesso

## Opções de Solução
### Opção 1: [Nome]
- Descrição
- Vantagens/Desvantagens
- Abordagem de implementação
- Avaliação de risco

### Opção 2: [Nome]
[Estrutura similar]

## Recomendação
- Abordagem recomendada
- Justificativa
- Roteiro de implementação
- Métricas de sucesso
- Plano de mitigação de riscos

## Perspectivas Alternativas
- Visão contrária
- Considerações futuras
- Áreas para pesquisa adicional
```

## Expectativas de Saída

- Cada opção de solução é avaliada em seus próprios méritos, não apenas comparada relativamente.
- Cadeias de raciocínio são explícitas — conclusões fazem referência à evidência ou lógica que as produziram.
- A incerteza é exposta, não ocultada. Se os dados são insuficientes, diga isso e especifique o que a resolveria.
- A seção de recomendação é acionável: os próximos passos são específicos o suficiente para começar imediatamente.
- O comprimento corresponde à complexidade do problema. Evite preenchimento.

## Exemplos de Uso

```bash
# Decisão arquitetônica
/ultra-think Devemos migrar para microsserviços ou melhorar nosso monolito?

# Resolução de problema complexo
/ultra-think Como escalamos nosso sistema para lidar com 10x de tráfego enquanto reduzimos custos?

# Planejamento estratégico
/ultra-think Qual stack de tecnologia devemos escolher para nossa plataforma de próxima geração?

# Desafio de design
/ultra-think Como podemos melhorar nossa API para ser mais amigável aos desenvolvedores mantendo compatibilidade retroativa?
```

> **Dica**: Para as decisões mais difíceis, habilite pensamento estendido nas configurações do Claude Code. Este comando de análise estruturada se combina com as capacidades nativas de raciocínio do Claude para resultados mais profundos.