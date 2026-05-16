# Writing Plans

## Visão Geral

Escreva planos de implementação abrangentes assumindo que o engenheiro tem zero contexto sobre nossa base de código e gosto questionável. Documente tudo que ele precisa saber: quais arquivos tocar para cada tarefa, código, testes, documentação que pode precisar verificar, como testar. Entregue o plano todo como tarefas pequenas. DRY. YAGNI. TDD. Commits frequentes.

Assuma que é um desenvolvedor habilidoso, mas sabe quase nada sobre nossa toolset ou domínio do problema. Assuma que não conhece muito bem design de testes.

**Anuncie no início:** "Estou usando a skill writing-plans para criar o plano de implementação."

**Contexto:** Deve ser executado em uma worktree dedicada (criada pela skill brainstorming).

**Salve planos em:** `docs/plans/YYYY-MM-DD-<feature-name>.md`

## Granularidade de Tarefas Pequenas

**Cada passo é uma ação (2-5 minutos):**
- "Escrever o teste que falha" - passo
- "Executar para garantir que falha" - passo
- "Implementar o código mínimo para fazer o teste passar" - passo
- "Executar os testes e garantir que passam" - passo
- "Commit" - passo

## Cabeçalho do Documento de Plano

**Cada plano DEVE começar com este cabeçalho:**

```markdown
# [Nome da Feature] - Plano de Implementação

> **Para Claude:** SUB-SKILL OBRIGATÓRIA: Use superpowers:executing-plans para implementar este plano tarefa por tarefa.

**Objetivo:** [Uma frase descrevendo o que isto constrói]

**Arquitetura:** [2-3 sentenças sobre a abordagem]

**Tech Stack:** [Tecnologias/bibliotecas principais]

---
```

## Estrutura de Tarefas

```markdown
### Tarefa N: [Nome do Componente]

**Arquivos:**
- Criar: `caminho/exato/arquivo.py`
- Modificar: `caminho/exato/existente.py:123-145`
- Teste: `tests/caminho/exato/teste.py`

**Passo 1: Escrever o teste que falha**

```python
def test_comportamento_especifico():
    resultado = funcao(entrada)
    assert resultado == esperado
```

**Passo 2: Executar teste para verificar que falha**

Executar: `pytest tests/caminho/teste.py::test_nome -v`
Esperado: FALHA com "funcao not defined"

**Passo 3: Escrever implementação mínima**

```python
def funcao(entrada):
    return esperado
```

**Passo 4: Executar teste para verificar que passa**

Executar: `pytest tests/caminho/teste.py::test_nome -v`
Esperado: PASSAR

**Passo 5: Commit**

```bash
git add tests/caminho/teste.py src/caminho/arquivo.py
git commit -m "feat: adicionar funcionalidade especifica"
```
```

## Lembre-se
- Caminhos de arquivo exatos sempre
- Código completo no plano (não "adicionar validação")
- Comandos exatos com saída esperada
- Referencie skills relevantes com sintaxe @
- DRY, YAGNI, TDD, commits frequentes

## Handoff de Execução

Após salvar o plano, ofereça opção de execução:

**"Plano completo e salvo em `docs/plans/<nome-arquivo>.md`. Duas opções de execução:**

**1. Controlado por Subagente (esta sessão)** - Dispacho subagente novo por tarefa, revisão entre tarefas, iteração rápida

**2. Sessão Paralela (separada)** - Abra nova sessão com executing-plans, execução em lote com checkpoints

**Qual abordagem?"**

**Se Controlado por Subagente escolhido:**
- **SUB-SKILL OBRIGATÓRIA:** Use superpowers:subagent-driven-development
- Permaneça nesta sessão
- Subagente novo por tarefa + code review

**Se Sessão Paralela escolhida:**
- Guie-os para abrir nova sessão na worktree
- **SUB-SKILL OBRIGATÓRIA:** Nova sessão usa superpowers:executing-plans