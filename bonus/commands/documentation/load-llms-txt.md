---
allowed-tools: Bash, WebFetch
argument-hint: [data-source] | --xatu | --custom-url | --validate
description: Carregar e processar contexto de documentação externa de arquivos llms.txt ou fontes customizadas
---

# Carregador de Contexto de Documentação Externa

Carregar contexto de documentação externa: $ARGUMENTS

## Status de Contexto Atual

- Acesso à rede: !`curl -s --connect-timeout 5 https://httpbin.org/status/200 >/dev/null && echo "✅ Disponível" || echo "❌ Limitado"`
- Contexto existente: Verificar llms.txt local ou cache de documentação
- Tipo de projeto: @package.json ou @README.md (detectar necessidades de contexto do projeto)

## Tarefa

Carregar e processar contexto de documentação externa da fonte especificada.

### Ação Padrão (Dados Xatu)
Carregar o arquivo llms.txt do repositório de dados Xatu:
```bash
curl -s https://raw.githubusercontent.com/ethpandaops/xatu-data/refs/heads/master/llms.txt
```

### Carregamento de Fonte Customizada
Para URLs customizadas ou fontes de documentação alternativas:
- Validar acessibilidade da URL
- Baixar e armazenar conteúdo em cache
- Processar e estruturar informações
- Integração com contexto do projeto

### Opções de Processamento
- **Carregamento direto**: Recuperação de conteúdo direta
- **Validação**: Verificar formato e estrutura do conteúdo  
- **Integração**: Mesclar com documentação existente do projeto
- **Cache**: Armazenar localmente para acesso offline