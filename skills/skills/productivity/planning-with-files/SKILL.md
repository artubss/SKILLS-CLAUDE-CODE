---
name: planning-with-files
version: "2.1.2"
description: Implementa planejamento baseado em arquivos estilo Manus para tarefas complexas. Cria task_plan.md, findings.md e progress.md. Use ao iniciar tarefas multi-etapas complexas, projetos de pesquisa ou qualquer tarefa que exija >5 chamadas de ferramentas.
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - WebFetch
  - WebSearch
hooks:
  SessionStart:
    - hooks:
        - type: command
          command: "echo '[planning-with-files] Pronto. Auto-ativa para tarefas complexas, ou invoque manualmente com /planning-with-files'"
  PreToolUse:
    - matcher: "Write|Edit|Bash"
      hooks:
        - type: command
          command: "cat task_plan.md 2>/dev/null | head -30 || true"
  PostToolUse:
    - matcher: "Write|Edit"
      hooks:
        - type: command
          command: "echo '[planning-with-files] Arquivo atualizado. Se isto completa uma fase, atualize o status em task_plan.md.'"
  Stop:
    - hooks:
        - type: command
          command: "${CLAUDE_PLUGIN_ROOT}/scripts/check-complete.sh"
---

# Planejamento com Arquivos

Trabalhe como Manus: Use arquivos markdown persistentes como sua "memória de trabalho no disco".

## Importante: Onde os Arquivos Ficam

Ao usar esta habilidade:

- **Templates** são armazenados no diretório de habilidade em `${CLAUDE_PLUGIN_ROOT}/templates/`
- **Seus arquivos de planejamento** (`task_plan.md`, `findings.md`, `progress.md`) devem ser criados em **seu diretório de projeto** — a pasta onde você está trabalhando

| Localização | O Que Va Lá |
|----------|-----------------|
| Diretório de habilidade (`${CLAUDE_PLUGIN_ROOT}/`) | Templates, scripts, documentos de referência |
| Seu diretório de projeto | `task_plan.md`, `findings.md`, `progress.md` |

Isto garante que seus arquivos de planejamento fiquem ao lado do seu código, não enterrados na pasta de instalação da habilidade.

## Início Rápido

Antes de QUALQUER tarefa complexa:

1. **Crie `task_plan.md`** em seu projeto — Use [templates/task_plan.md](templates/task_plan.md) como referência
2. **Crie `findings.md`** em seu projeto — Use [templates/findings.md](templates/findings.md) como referência
3. **Crie `progress.md`** em seu projeto — Use [templates/progress.md](templates/progress.md) como referência
4. **Releia o plano antes de decidir** — Atualiza metas na janela de atenção
5. **Atualize após cada fase** — Marque como concluído, registre erros

> **Nota:** Os três arquivos de planejamento devem ser criados em seu diretório de trabalho atual (raiz do seu projeto), não na pasta de instalação da habilidade.

## O Padrão Principal

```
Janela de Contexto = RAM (volátil, limitada)
Sistema de Arquivos = Disco (persistente, ilimitado)

→ Qualquer coisa importante é escrita em disco.
```

## Propósitos dos Arquivos

| Arquivo | Propósito | Quando Atualizar |
|------|---------|----------------|
| `task_plan.md` | Fases, progresso, decisões | Após cada fase |
| `findings.md` | Pesquisa, descobertas | Após QUALQUER descoberta |
| `progress.md` | Log da sessão, resultados de testes | Durante toda a sessão |

## Regras Críticas

### 1. Crie o Plano Primeiro
Nunca inicie uma tarefa complexa sem `task_plan.md`. Inegociável.

### 2. A Regra de 2 Ações
> "Após cada 2 operações de visualização/navegador/busca, IMEDIATAMENTE salve descobertas-chave em arquivos de texto."

Isto evita que informações visuais/multimodais sejam perdidas.

### 3. Leia Antes de Decidir
Antes de decisões importantes, leia o arquivo do plano. Isto mantém metas em sua janela de atenção.

### 4. Atualize Após Agir
Após completar qualquer fase:
- Marque status da fase: `in_progress` → `complete`
- Registre erros encontrados
- Anote arquivos criados/modificados

### 5. Registre TODOS os Erros
Cada erro vai no arquivo do plano. Isto constrói conhecimento e previne repetição.

```markdown
## Erros Encontrados
| Erro | Tentativa | Resolução |
|-------|---------|------------|
| FileNotFoundError | 1 | Criado arquivo config padrão |
| API timeout | 2 | Adicionada lógica de retry |
```

### 6. Nunca Repita Falhas
```
if ação_falhou:
    próxima_ação != mesma_ação
```
Rastreie o que você tentou. Mute a abordagem.

## O Protocolo de 3 Tentativas para Erros

```
TENTATIVA 1: Diagnosticar e Corrigir
  → Leia o erro cuidadosamente
  → Identifique a causa raiz
  → Aplique correção direcionada

TENTATIVA 2: Abordagem Alternativa
  → Mesmo erro? Tente método diferente
  → Ferramenta diferente? Biblioteca diferente?
  → NUNCA repita a ação exata que falhou

TENTATIVA 3: Reavaliação Mais Ampla
  → Questione pressupostos
  → Busque soluções
  → Considere atualizar o plano

APÓS 3 FALHAS: Escale para o Usuário
  → Explique o que você tentou
  → Compartilhe o erro específico
  → Peça orientação
```

## Matriz de Decisão Leitura vs Escrita

| Situação | Ação | Razão |
|-----------|--------|--------|
| Acabou de escrever um arquivo | NÃO leia | Conteúdo ainda em contexto |
| Visualizou imagem/PDF | Escreva descobertas AGORA | Multimodal → texto antes de perder |
| Navegador retornou dados | Escreva em arquivo | Screenshots não persistem |
| Iniciando nova fase | Leia plano/descobertas | Re-oriente se contexto estiver desatualizado |
| Erro ocorreu | Leia arquivo relevante | Precisa do estado atual para corrigir |
| Retomando após intervalo | Leia todos os arquivos de planejamento | Recupere estado |

## O Teste de Reinicialização de 5 Perguntas

Se você conseguir responder estas, seu gerenciamento de contexto está sólido:

| Pergunta | Fonte de Resposta |
|----------|---------------|
| Onde estou? | Fase atual em task_plan.md |
| Para onde vou? | Fases restantes |
| Qual é o objetivo? | Declaração de objetivo no plano |
| O que aprendi? | findings.md |
| O que fiz? | progress.md |

## Quando Usar Este Padrão

**Use para:**
- Tarefas multi-etapas (3+ etapas)
- Tarefas de pesquisa
- Construir/criar projetos
- Tarefas que abrangem muitas chamadas de ferramentas
- Qualquer coisa que exija organização

**Pule para:**
- Perguntas simples
- Edições de arquivo único
- Consultas rápidas

## Templates

Copie estes templates para começar:

- [templates/task_plan.md](templates/task_plan.md) — Rastreamento de fases
- [templates/findings.md](templates/findings.md) — Armazenamento de pesquisa
- [templates/progress.md](templates/progress.md) — Log de sessão

## Scripts

Scripts auxiliares para automação:

- `scripts/init-session.sh` — Inicializar todos os arquivos de planejamento
- `scripts/check-complete.sh` — Verificar todas as fases concluídas

## Tópicos Avançados

- **Princípios Manus:** Veja [reference.md](reference.md)
- **Exemplos Reais:** Veja [examples.md](examples.md)

## Anti-Padrões

| Não Faça | Faça Em Vez Disso |
|-------|------------|
| Use TodoWrite para persistência | Crie arquivo task_plan.md |
| Declare objetivos uma vez e esqueça | Releia plano antes de decidir |
| Esconda erros e tente novamente silenciosamente | Registre erros no arquivo de plano |
| Coloque tudo em contexto | Armazene conteúdo grande em arquivos |
| Comece a executar imediatamente | Crie arquivo de plano PRIMEIRO |
| Repita ações que falharam | Rastreie tentativas, mude abordagem |
| Crie arquivos no diretório de habilidade | Crie arquivos em seu projeto |