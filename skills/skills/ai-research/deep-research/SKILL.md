---
name: deep-research
description: "Execute tarefas de pesquisa autônoma que planejam, buscam, leem e sintetizam informações em relatórios abrangentes."
risk: safe
source: "https://github.com/sanjay3290/ai-skills/tree/main/skills/deep-research"
date_added: "2026-02-27"
---

# Skill de Pesquisa Profunda Gemini

Execute tarefas de pesquisa autônoma que planejam, buscam, leem e sintetizam informações em relatórios abrangentes.

## Quando Usar Esta Skill

Use esta skill quando:
- Realizar análise de mercado
- Conduzir mapeamento competitivo
- Criar revisões de literatura
- Fazer pesquisa técnica
- Realizar due diligence
- Precisar de relatórios de pesquisa detalhados e citados

## Requisitos

- Python 3.8+
- httpx: `pip install -r requirements.txt`
- Variável de ambiente GEMINI_API_KEY

## Configuração

1. Obtenha uma chave de API Gemini em [Google AI Studio](https://aistudio.google.com/)
2. Configure a variável de ambiente:
   ```bash
   export GEMINI_API_KEY=your-api-key-here
   ```
   Ou crie um arquivo `.env` no diretório da skill.

## Uso

### Iniciar uma tarefa de pesquisa
```bash
python3 scripts/research.py --query "Research the history of Kubernetes"
```

### Com formato de saída estruturado
```bash
python3 scripts/research.py --query "Compare Python web frameworks" \
  --format "1. Executive Summary\n2. Comparison Table\n3. Recommendations"
```

### Transmitir progresso em tempo real
```bash
python3 scripts/research.py --query "Analyze EV battery market" --stream
```

### Iniciar sem esperar
```bash
python3 scripts/research.py --query "Research topic" --no-wait
```

### Verificar status da pesquisa em execução
```bash
python3 scripts/research.py --status <interaction_id>
```

### Aguardar conclusão
```bash
python3 scripts/research.py --wait <interaction_id>
```

### Continuar de uma pesquisa anterior
```bash
python3 scripts/research.py --query "Elaborate on point 2" --continue <interaction_id>
```

### Listar pesquisas recentes
```bash
python3 scripts/research.py --list
```

## Formatos de Saída

- **Padrão**: Relatório markdown legível
- **JSON** (`--json`): Dados estruturados para uso programático
- **Raw** (`--raw`): Resposta bruta da API

## Custo e Tempo

| Métrica | Valor |
|---------|-------|
| Tempo | 2-10 minutos por tarefa |
| Custo | R$ 10-25 por tarefa (varia conforme complexidade) |
| Uso de tokens | ~250k-900k entrada, ~60k-80k saída |

## Melhores Casos de Uso

- Análise de mercado e mapeamento competitivo
- Revisões de literatura técnica
- Pesquisa de due diligence
- Pesquisa histórica e cronogramas
- Análise comparativa (frameworks, produtos, tecnologias)

## Fluxo de Trabalho

1. Usuário solicita pesquisa → Execute `--query "..."`
2. Informe ao usuário o tempo estimado (2-10 minutos)
3. Monitore com `--stream` ou consulte com `--status`
4. Retorne resultados formatados
5. Use `--continue` para perguntas de acompanhamento

## Códigos de Saída

- **0**: Sucesso
- **1**: Erro (erro de API, problema de configuração, timeout)
- **130**: Cancelado pelo usuário (Ctrl+C)