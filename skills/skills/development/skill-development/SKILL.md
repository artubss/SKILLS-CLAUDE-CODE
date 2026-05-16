---
name: skill-creator
description: Crie novas skills, modifique e melhore skills existentes, e meça o desempenho delas. Use quando os usuários querem criar uma skill do zero, editar ou otimizar uma skill existente, executar avaliações para testar uma skill, fazer benchmark de desempenho com análise de variância, ou otimizar a descrição de uma skill para maior precisão no acionamento.
---

# Skill Creator

Uma skill para criar novas skills e melhorá-las iterativamente.

Em alto nível, o processo de criação de uma skill funciona assim:

- Decida o que você quer que a skill faça e, grosso modo, como ela deve fazer isso
- Escreva um rascunho da skill
- Crie alguns prompts de teste e execute claude-com-acesso-à-skill neles
- Ajude o usuário a avaliar os resultados qualitativa e quantitativamente
  - Enquanto as execuções acontecem em background, rascunhe algumas avaliações quantitativas caso não existam (se existirem, você pode usar como estão ou modificar se achar que algo precisa mudar). Depois explique-as para o usuário (ou, se já existiam, explique as que já existem)
  - Use o script `eval-viewer/generate_review.py` para mostrar os resultados ao usuário e permitir que ele veja as métricas quantitativas
- Reescreva a skill com base no feedback da avaliação dos resultados pelo usuário (e também se houver falhas óbvias reveladas pelos benchmarks quantitativos)
- Repita até ficar satisfeito
- Expanda o conjunto de testes e tente novamente em escala maior

Seu trabalho ao usar esta skill é descobrir onde o usuário está neste processo e então ajudá-lo a progredir através destes estágios. Por exemplo, talvez ele diga "Quero fazer uma skill para X". Você pode ajudar a detalhar o que ele quer dizer, escrever um rascunho, escrever os casos de teste, descobrir como ele quer avaliar, executar todos os prompts e repetir.

Por outro lado, talvez ele já tenha um rascunho da skill. Neste caso você pode ir direto para a parte de avaliação/iteração do loop.

Claro, você sempre deve ser flexível e se o usuário disser "Não preciso rodar um monte de avaliações, apenas conversa comigo", você pode fazer isso em vez disso.

Então, depois que a skill estiver pronta (mas novamente, a ordem é flexível), você também pode executar o otimizador de descrição de skill, que temos um script separado para, para otimizar o acionamento da skill.

Legal? Legal.

## Comunicando com o usuário

A skill creator pode ser usada por pessoas com uma ampla gama de familiaridade com jargão técnico. Se você ainda não ouviu falar (e como poderia, começou só muito recentemente), existe uma tendência agora em que o poder do Claude está inspirando encanadores a abrir seus terminais, pais e avós a fazer buscas no Google por "como instalar npm". Por outro lado, a maioria dos usuários provavelmente é bastante alfabetizada em computação.

Então por favor preste atenção em pistas de contexto para entender como formular sua comunicação! No caso padrão, só para dar uma ideia:

- "avaliação" e "benchmark" estão na linha limite, mas OK
- para "JSON" e "assertion" você quer ver sinais sérios do usuário de que ele sabe o que essas coisas são antes de usar sem explicar

Está OK explicar brevemente termos se você tiver dúvida, e sinta-se livre para esclarecer termos com uma definição curta se não tiver certeza se o usuário vai entender.

---

## Criando uma skill

### Capturar Intenção

Comece entendendo a intenção do usuário. A conversa atual pode já conter um workflow que o usuário quer capturar (por exemplo, ele diz "transforme isto em uma skill"). Se for assim, extraia as respostas do histórico de conversa primeiro — as ferramentas usadas, a sequência de passos, as correções que o usuário fez, os formatos de entrada/saída observados. O usuário pode precisar preencher as lacunas e deve confirmar antes de prosseguir para o próximo passo.

1. O que esta skill deve permitir que Claude faça?
2. Quando esta skill deve ser acionada? (quais frases/contextos do usuário)
3. Qual é o formato de saída esperado?
4. Devemos configurar casos de teste para verificar se a skill funciona? Skills com saídas objetivamente verificáveis (transformação de arquivos, extração de dados, geração de código, passos de workflow fixos) se beneficiam de casos de teste. Skills com saídas subjetivas (estilo de escrita, arte) frequentemente não precisam deles. Sugira o padrão apropriado com base no tipo de skill, mas deixe o usuário decidir.

### Entrevista e Pesquisa

Faça perguntas proativas sobre casos extremos, formatos de entrada/saída, exemplos de arquivos, critérios de sucesso e dependências. Espere para escrever prompts de teste até ter esta parte resolvida.

Verifique MCPs disponíveis — se úteis para pesquisa (buscar em docs, encontrar skills similares, procurar boas práticas), pesquise em paralelo via subagentos se disponível, senão inline. Chegue preparado com contexto para reduzir o fardo no usuário.

### Escrever o SKILL.md

Com base na entrevista do usuário, preencha estes componentes:

- **name**: Identificador da skill
- **description**: Quando acionar, o que faz. Este é o mecanismo de acionamento principal - inclua tanto o que a skill faz QUANTO contextos específicos para quando usá-la. Toda informação de "quando usar" vai aqui, não no corpo. Nota: atualmente Claude tende a "sub-acionar" skills -- a não usá-las quando seriam úteis. Para combater isso, por favor faça as descrições de skills um pouco "assertivas". Por exemplo, em vez de "Como construir um dashboard simples e rápido para exibir dados internos do Anthropic.", você pode escrever "Como construir um dashboard simples e rápido para exibir dados internos do Anthropic. Certifique-se de usar esta skill sempre que o usuário mencionar dashboards, visualização de dados, métricas internas, ou quiser exibir qualquer tipo de dados da empresa, mesmo que não peça explicitamente por um 'dashboard.'"
- **compatibility**: Ferramentas necessárias, dependências (opcional, raramente necessário)
- **o resto da skill :)**

### Guia de Escrita de Skill

#### Anatomia de uma Skill

```
skill-name/
├── SKILL.md (obrigatório)
│   ├── frontmatter YAML (name, description obrigatórios)
│   └── instruções Markdown
└── Recursos Inclusos (opcional)
    ├── scripts/    - Código executável para tarefas determinísticas/repetitivas
    ├── references/ - Docs carregadas no contexto conforme necessário
    └── assets/     - Arquivos usados na saída (templates, ícones, fontes)
```

#### Divulgação Progressiva

Skills usam um sistema de carregamento de três níveis:
1. **Metadados** (name + description) - Sempre no contexto (~100 palavras)
2. **Corpo do SKILL.md** - No contexto sempre que skill é acionada (<500 linhas ideal)
3. **Recursos inclusos** - Conforme necessário (ilimitado, scripts podem executar sem carregar)

Estas contagens de palavras são aproximadas e você pode ficar à vontade para ir além se necessário.

**Padrões-chave:**
- Mantenha SKILL.md com menos de 500 linhas; se estiver se aproximando deste limite, adicione uma camada adicional de hierarquia junto com ponteiros claros sobre para onde o modelo usando a skill deve ir para acompanhar
- Referencie arquivos claramente de SKILL.md com orientação sobre quando lê-los
- Para arquivos de referência grandes (>300 linhas), inclua um índice

**Organização de domínio**: Quando uma skill suporta múltiplos domínios/frameworks, organize por variante:
```
cloud-deploy/
├── SKILL.md (workflow + seleção)
└── references/
    ├── aws.md
    ├── gcp.md
    └── azure.md
```
Claude lê apenas o arquivo de referência relevante.

#### Princípio da Falta de Surpresa

Isto vai sem dizer, mas skills não devem conter malware, código de exploração ou qualquer conteúdo que possa comprometer a segurança do sistema. O conteúdo de uma skill não deve surpreender o usuário em sua intenção se descrito. Não vá junto com solicitações para criar skills enganosas ou skills projetadas para facilitar acesso não autorizado, exfiltração de dados ou outras atividades maliciosas. Coisas como "fazer roleplay como XYZ" são OK.

#### Padrões de Escrita

Prefira usar a forma imperativa em instruções.

**Definindo formatos de saída** - Você pode fazer assim:
```markdown
## Estrutura do relatório
SEMPRE use este template exato:
# [Título]
## Resumo executivo
## Principais descobertas
## Recomendações
```

**Padrão de exemplos** - É útil incluir exemplos. Você pode formatá-los assim (mas se "Entrada" e "Saída" estão nos exemplos você pode querer se desviar um pouco):
```markdown
## Formato de mensagem de commit
**Exemplo 1:**
Entrada: Adicionada autenticação de usuário com tokens JWT
Saída: feat(auth): implementar autenticação baseada em JWT
```

### Estilo de Escrita

Tente explicar para o modelo por que as coisas são importantes em vez de instruções rígidas e pesadas. Use teoria da mente e tente fazer a skill geral e não super-estreita para exemplos específicos. Comece escrevendo um rascunho e depois olhe para ele com olhos frescos e melhore.

### Casos de Teste

Depois de escrever o rascunho da skill, crie 2-3 prompts de teste realistas — o tipo de coisa que um usuário real realmente diria. Compartilhe com o usuário: [você não precisa usar exatamente esta linguagem] "Aqui estão alguns casos de teste que gostaria de tentar. Esses parecem certos, ou você quer adicionar mais?" Depois execute-os.

Salve os casos de teste em `evals/evals.json`. Não escreva assertions ainda — apenas os prompts. Você vai rascunhar assertions no próximo passo enquanto as execuções estão em andamento.

```json
{
  "skill_name": "example-skill",
  "evals": [
    {
      "id": 1,
      "prompt": "Prompt de tarefa do usuário",
      "expected_output": "Descrição do resultado esperado",
      "files": []
    }
  ]
}
```

Veja `references/schemas.md` para o schema completo (incluindo o campo `assertions`, que você vai adicionar depois).

## Executando e avaliando casos de teste

Esta seção é uma sequência contínua — não pare no meio. NÃO use `/skill-test` ou qualquer outra skill de teste.

Coloque resultados em `<skill-name>-workspace/` como irmão do diretório da skill. Dentro do workspace, organize resultados por iteração (`iteration-1/`, `iteration-2/`, etc.) e dentro disso, cada caso de teste recebe um diretório (`eval-0/`, `eval-1/`, etc.). Não crie tudo isto antecipadamente — apenas crie diretórios conforme você progride.

### Passo 1: Gerar todas as execuções (com-skill E baseline) na mesma volta

Para cada caso de teste, gere dois subagentos na mesma volta — um com a skill, um sem. Isto é importante: não gere as execuções com-skill primeiro e depois volte para baselines depois. Lance tudo de uma vez para que tudo termine próximo ao mesmo tempo.

**Execução com-skill:**

```
Execute esta tarefa:
- Caminho da skill: <path-to-skill>
- Tarefa: <eval prompt>
- Arquivos de entrada: <eval files if any, or "none">
- Salvar saídas em: <workspace>/iteration-<N>/eval-<ID>/with_skill/outputs/
- Saídas para salvar: <what the user cares about — e.g., "the .docx file", "the final CSV">
```

**Execução baseline** (mesmo prompt, mas o baseline depende do contexto):
- **Criando uma nova skill**: nenhuma skill. Mesmo prompt, nenhum caminho de skill, salve em `without_skill/outputs/`.
- **Melhorando uma skill existente**: a versão antiga. Antes de editar, fotografe a skill (`cp -r <skill-path> <workspace>/skill-snapshot/`), depois aponte o subagentos baseline para o snapshot. Salve em `old_skill/outputs/`.

Escreva um `eval_metadata.json` para cada caso de teste (assertions podem estar vazias por enquanto). Dê a cada eval um nome descritivo com base no que está testando — não apenas "eval-0". Use este nome para o diretório também. Se esta iteração usa prompts de eval novos ou modificados, crie estes arquivos para cada novo diretório de eval — não assuma que eles passam de iterações anteriores.

```json
{
  "eval_id": 0,
  "eval_name": "descriptive-name-here",
  "prompt": "Prompt de tarefa do usuário",
  "assertions": []
}
```

### Passo 2: Enquanto as execuções estão em andamento, rascunhe assertions

Não apenas espere as execuções terminarem — você pode usar este tempo produtivamente. Rascunhe assertions quantitativas para cada caso de teste e explique-as para o usuário. Se assertions já existem em `evals/evals.json`, revise-as e explique o que elas verificam.

Boas assertions são objetivamente verificáveis e têm nomes descritivos — elas devem ler claramente no viewer de benchmark para que alguém vendo rapidamente os resultados entenda imediatamente o que cada uma verifica. Skills subjetivas (estilo de escrita, qualidade de design) são melhor avaliadas qualitativamente — não force assertions em coisas que precisam de julgamento humano.

Atualize os arquivos `eval_metadata.json` e `evals/evals.json` com as assertions uma vez rascunhadas. Também explique para o usuário o que ele verá no viewer — tanto os outputs qualitativos quanto o benchmark quantitativo.

### Passo 3: Conforme as execuções são concluídas, capture dados de timing

Quando cada tarefa de subagentos é concluída, você recebe uma notificação contendo `total_tokens` e `duration_ms`. Salve estes dados imediatamente em `timing.json` no diretório de execução:

```json
{
  "total_tokens": 84852,
  "duration_ms": 23332,
  "total_duration_seconds": 23.3
}
```

Esta é a única oportunidade para capturar estes dados — ela vem através da notificação de tarefa e não é persistida em outro lugar. Processe cada notificação conforme chegar em vez de tentar agrupar.

### Passo 4: Classificar, agregar e lançar o viewer

Uma vez que todas as execuções estejam concluídas:

1. **Classifique cada execução** — gere um subagentos classificador (ou classifique inline) que leia `agents/grader.md` e avalie cada assertion contra os outputs. Salve os resultados em `grading.json` em cada diretório de execução. O array expectations em grading.json deve usar os campos `text`, `passed` e `evidence` (não `name`/`met`/`details` ou outras variantes) — o viewer depende destes nomes de campo exatos. Para assertions que podem ser verificadas programaticamente, escreva e execute um script em vez de avaliar visualmente — scripts são mais rápidos, mais confiáveis e podem ser reutilizados entre iterações.

2. **Agregue em benchmark** — execute o script de agregação do diretório skill-creator:
   ```bash
   python -m scripts.aggregate_benchmark <workspace>/iteration-N --skill-name <name>
   ```
   Isto produz `benchmark.json` e `benchmark.md` com pass_rate, time e tokens para cada configuração, com média ± desvio padrão e o delta. Se gerar benchmark.json manualmente, veja `references/schemas.md` para o schema exato que o viewer espera.
Coloque cada versão with_skill antes de sua contraparte baseline.

3. **Faça uma passagem de analista** — leia os dados de benchmark e resgate padrões que as estatísticas agregadas podem esconder. Veja `agents/analyzer.md` (seção "Analyzing Benchmark Results") para o que procurar — coisas como assertions que sempre passam independentemente da skill (não discriminantes), avaliações com alta variância (possivelmente instáveis) e trade-offs de tempo/token.

4. **Lance o viewer** com tanto outputs qualitativos quanto dados quantitativos:
   ```bash
   nohup python <skill-creator-path>/eval-viewer/generate_review.py \
     <workspace>/iteration-N \
     --skill-name "my-skill" \
     --benchmark <workspace>/iteration-N/benchmark.json \
     > /dev/null 2>&1 &
   VIEWER_PID=$!
   ```
   Para iteração 2+, também passe `--previous-workspace <workspace>/iteration-<N-1>`.

   **Ambientes cowork / headless:** Se `webbrowser.open()` não está disponível ou o ambiente não tem display, use `--static <output_path>` para escrever um arquivo HTML independente em vez de iniciar um servidor. O feedback será baixado como um arquivo `feedback.json` quando o usuário clicar em "Submit All Reviews". Após download, copie `feedback.json` para o diretório workspace para a próxima iteração pegar.

Nota: por favor use generate_review.py para criar o viewer; não há necessidade de escrever HTML customizado.

5. **Diga para o usuário** algo como: "Abri os resultados no seu navegador. Há duas abas — 'Outputs' permite que você clique em cada caso de teste e deixe feedback, 'Benchmark' mostra a comparação quantitativa. Quando terminar, volte aqui e me avise."

### O que o usuário vê no viewer

A aba "Outputs" mostra um caso de teste por vez:
- **Prompt**: a tarefa que foi dada
- **Output**: os arquivos que a skill produziu, renderizados inline onde possível
- **Previous Output** (iteração 2+): seção recolhida mostrando o output da última iteração
- **Formal Grades** (se grading foi executado): seção recolhida mostrando assertion pass/fail
- **Feedback**: uma caixa de texto que auto-salva conforme você digita
- **Previous Feedback** (iteração 2+): seus comentários da última vez, mostrados abaixo da caixa de texto

Navegação é via botões prev/next ou setas de teclado. Quando concluído, eles clicam em "Submit All Reviews" que salva todo feedback em `feedback.json`.

### Passo 5: Leia o feedback

Quando o usuário disser que terminou, leia `feedback.json`:

```json
{
  "reviews": [
    {"run_id": "eval-0-with_skill", "feedback": "o gráfico está faltando rótulos de eixo", "timestamp": "..."},
    {"run_id": "eval-1-with_skill", "feedback": "", "timestamp": "..."},
    {"run_id": "eval-2-with_skill", "feedback": "perfeito, adorei isto", "timestamp": "..."}
  ],
  "status": "complete"
}
```

Feedback vazio significa que o usuário achou que estava OK. Foque suas melhorias nos casos de teste onde o usuário teve reclamações específicas.

Termine o servidor viewer quando terminar com ele:

```bash
kill $VIEWER_PID 2>/dev/null
```

---

## Melhorando a skill

Este é o coração do loop. Você executou os casos de teste, o usuário revisou os resultados, e agora você precisa melhorar a skill com base no feedback dele.

### Como pensar em melhorias

1. **Generalize a partir do feedback.** A coisa grande que está acontecendo aqui é que estamos tentando criar skills que podem ser usadas um milhão de vezes (talvez literalmente, talvez até mais quem sabe) em muitos prompts diferentes. Aqui você e o usuário estão iterando em apenas alguns exemplos várias vezes porque ajuda a se mover mais rápido. O usuário conhece estes exemplos de dentro para fora e é rápido para ele avaliar novos outputs. Mas se a skill que você e o usuário estão codesenvolvendo funciona apenas para estes exemplos, é inútil. Em vez de fazer mudanças fiddly e overfitty, ou MUSTs opressivamente constritivos, se há algum problema persistente, você pode tentar usar metáforas diferentes ou recomendar padrões diferentes de trabalho. É relativamente barato tentar e talvez você chegue a algo ótimo.

2. **Mantenha o prompt enxuto.** Remova coisas que não estão puxando seu peso. Certifique-se de ler as transcrições, não apenas os outputs finais — se parece que a skill está fazendo o modelo desperdiçar muito tempo fazendo coisas que são improdutivas, você pode tentar se livrar das partes da skill que estão fazendo isso e ver o que acontece.

3. **Explique o porquê.** Tente muito explicar o **porquê** por trás de tudo que você está pedindo ao modelo para fazer. Os LLMs de hoje são *inteligentes*. Eles têm boa teoria da mente e quando dado um bom harness podem ir além de instruções rote e realmente fazer coisas acontecerem. Mesmo se o feedback do usuário for conciso ou frustrado, tente realmente entender a tarefa e por que o usuário escreveu o que escreveu, e o que ele realmente escreveu, e depois transmita este entendimento para as instruções. Se você se vê escrevendo ALWAYS ou NEVER em caps, ou usando estruturas super rígidas, isso é uma bandeira amarela — se possível, reformule e explique o raciocínio para que o modelo entenda por que a coisa que você está pedindo é importante. Esta é uma abordagem mais humana, poderosa e eficaz.

4. **Procure por trabalho repetido entre casos de teste.** Leia as transcrições das execuções de teste e note se os subagentos todos escreveram independentemente scripts helper similares ou tomaram a mesma abordagem multi-passo para algo. Se todos os 3 casos de teste resultaram no subagentos escrevendo um `create_docx.py` ou um `build_chart.py`, este é um sinal forte de que a skill deve agrupar este script. Escreva-o uma vez, coloque em `scripts/`, e diga à skill para usá-lo. Isto economiza cada invocação futura de reinventar a roda.

Esta tarefa é bem importante (estamos tentando criar bilhões por ano em valor econômico aqui!) e seu tempo de reflexão não é o bloqueador; leve seu tempo e realmente pense nas coisas. Sugiro escrever um rascunho de revisão e depois olhar para ele com olhos novos e fazer melhorias. Realmente tente entrar na cabeça do usuário e entender o que ele quer e precisa.

### O loop de iteração

Depois de melhorar a skill:

1. Aplique suas melhorias à skill
2. Reexecute todos os casos de teste em um novo diretório `iteration-<N+1>/`, incluindo execuções baseline. Se você está criando uma nova skill, o baseline é sempre `without_skill` (nenhuma skill) — esse permanece igual entre iterações. Se você está melhorando uma skill existente, use seu julgamento sobre o que faz sentido como baseline: a versão original com a qual o usuário veio, ou a iteração anterior.
3. Lance o reviewer com `--previous-workspace` apontando para a iteração anterior
4. Espere o usuário revisar e dizer que terminou
5. Leia o novo feedback, melhore novamente, repita

Continue até:
- O usuário dizer que está feliz
- O feedback estar tudo vazio (tudo parece bom)
- Você não estar fazendo progresso significativo

---

## Avançado: Comparação cega

Para situações onde você quer uma comparação mais rigorosa entre duas versões de uma skill (por exemplo, o usuário pergunta "a nova versão é realmente melhor?"), há um sistema de comparação cega. Leia `agents/comparator.md` e `agents/analyzer.md` para os detalhes. A ideia básica é: dê dois outputs para um agente independente sem dizer qual é qual, e deixe-o julgar qualidade. Depois analise por que o vencedor venceu.

Isto é opcional, requer subagentos, e a maioria dos usuários não vai precisar. O loop de revisão humana é geralmente suficiente.

---

## Otimização de Descrição

O campo description no frontmatter SKILL.md é o mecanismo primário que determina se Claude invoca uma skill. Depois de criar ou melhorar uma skill, ofereça otimizar a descrição para melhor precisão de acionamento.

### Passo 1: Gerar queries de eval de acionamento

Crie 20 queries de eval — uma mistura de should-trigger e should-not-trigger. Salve como JSON:

```json
[
  {"query": "o prompt do usuário", "should_trigger": true},
  {"query": "outro prompt", "should_trigger": false}
]
```

As queries devem ser realistas e algo que um usuário Claude Code ou Claude.ai realmente digitaria. Não pedidos abstratos, mas pedidos que são concretos e específicos e têm uma boa quantidade de detalhe. Por exemplo, caminhos de arquivo, contexto pessoal sobre o trabalho do usuário ou situação, nomes e valores de coluna, nomes de empresa, URLs. Um pouco de backstory. Alguns podem estar em minúsculas ou conter abreviações ou typos ou fala casual. Use uma mistura de diferentes comprimentos e foque em casos extremos em vez de torná-los óbvios (o usuário terá uma chance de assinar).

Ruim: `"Formate estes dados"`, `"Extraia texto de PDF"`, `"Crie um gráfico"`

Bom: `"ok então meu chefe acabou de me enviar este arquivo xlsx (está em meus downloads, chamado algo como 'Q4 sales final FINAL v2.xlsx') e ela quer que eu adicione uma coluna que mostra a margem de lucro como percentagem. A receita está na coluna C e custos estão na coluna D acho"`

Para as queries **should-trigger** (8-10), pense em cobertura. Você quer diferentes formas de dizer a mesma intenção — alguns formais, alguns casual. Inclua casos onde o usuário não explicitamente nomeie a skill ou tipo de arquivo mas claramente precise dela. Jogue em alguns casos de uso incomum e casos onde esta skill compete com outra mas deveria ganhar.

Para as queries **should-not-trigger** (8-10), as mais valiosas são as quase-erros — queries que compartilham palavras-chave ou conceitos com a skill mas realmente precisam de algo diferente. Pense em domínios adjacentes, fraseado ambíguo onde um match de palavra-chave ingênuo dispararia mas não deveria, e casos onde a query toca algo que a skill faz mas em um contexto onde outra ferramenta é mais apropriada.

A coisa-chave a evitar: não faça queries should-not-trigger obviamente irrelevantes. "Escreva uma função fibonacci" como um teste negativo para uma skill de PDF é muito fácil — não testa nada. Os casos negativos devem ser genuinamente tricky.

### Passo 2: Revisar com usuário

Apresente o conjunto de eval para o usuário usando o template HTML:

1. Leia o template de `assets/eval_review.html`
2. Substitua os placeholders:
   - `__EVAL_DATA_PLACEHOLDER__` → o array JSON de items de eval (sem aspas em volta — é uma atribuição de variável JS)
   - `__SKILL_NAME_PLACEHOLDER__` → o nome da skill
   - `__SKILL_DESCRIPTION_PLACEHOLDER__` → a descrição atual da skill
3. Escreva para um arquivo temp (por exemplo, `/tmp/eval_review_<skill-name>.html`) e abra: `open /tmp/eval_review_<skill-name>.html`
4. O usuário pode editar queries, alternar should-trigger, adicionar/remover entries, depois clicar em "Export Eval Set"
5. O arquivo baixa para `~/Downloads/eval_set.json` — verifique a pasta Downloads para a versão mais recente em caso de múltiplas (por exemplo, `eval_set (1).json`)

Este passo importa — bad eval queries levam a bad descriptions.

### Passo 3: Executar o loop de otimização

Diga para o usuário: "Isto vai levar algum tempo — vou executar o loop de otimização em background e verificar periodicamente."

Salve o conjunto de eval no workspace, depois execute em background:

```bash
python -m scripts.run_loop \
  --eval-set <path-to-trigger-eval.json> \
  --skill-path <path-to-skill> \
  --model <model-id-powering-this-session> \
  --max-iterations 5 \
  --verbose
```

Use o ID do modelo do seu prompt de sistema (o que está potencializando a sessão atual) para que o teste de acionamento corresponda ao que o usuário realmente experimenta.

Enquanto executa, tail periodicamente a saída para dar ao usuário atualizações sobre em qual iteração está e como os scores parecem.

Isto trata o loop de otimização completo automaticamente. Ele divide o conjunto de eval em 60% treino e 40% teste retido, avalia a descrição atual (executando cada query 