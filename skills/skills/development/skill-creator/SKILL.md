---
name: skill-creator
description: Criar novas skills, modificar e melhorar skills existentes, e medir desempenho de skills. Use quando os usuários querem criar uma skill do zero, editar ou otimizar uma skill existente, executar avaliações para testar uma skill, fazer benchmark de desempenho com análise de variância, ou otimizar a descrição de uma skill para melhor precisão no disparo.
---

# Skill Creator

Uma skill para criar novas skills e melhorá-las iterativamente.

Em alto nível, o processo de criar uma skill funciona assim:

- Decida o que você quer que a skill faça e aproximadamente como ela deve fazer isso
- Escreva um rascunho da skill
- Crie alguns prompts de teste e execute claude-com-acesso-à-skill neles
- Ajude o usuário a avaliar os resultados tanto qualitativamente quanto quantitativamente
  - Enquanto as execuções acontecem em background, elabore algumas avaliações quantitativas se não houver nenhuma (se houver algumas, você pode usá-las como estão ou modificá-las se achar que algo precisa mudar). Depois explique-as ao usuário (ou se já existirem, explique as que já existem)
  - Use o script `eval-viewer/generate_review.py` para mostrar os resultados ao usuário, e também permita que ele veja as métricas quantitativas
- Reescreva a skill com base no feedback da avaliação do usuário (e também se houver falhas óbvias que apareçam nos benchmarks quantitativos)
- Repita até estar satisfeito
- Expanda o conjunto de testes e tente novamente em maior escala

Seu trabalho ao usar esta skill é descobrir em que ponto do processo o usuário está e então ajudá-lo a avançar por essas etapas. Por exemplo, talvez ele diga "Quero criar uma skill para X". Você pode ajudar a esclarecer o que ele quer dizer, escrever um rascunho, criar os casos de teste, descobrir como ele quer avaliar, executar todos os prompts e repetir.

Por outro lado, talvez ele já tenha um rascunho da skill. Nesse caso você pode ir direto para a parte de avaliação/iteração do loop.

É claro que você deve sempre ser flexível e se o usuário disser "Não preciso executar um monte de avaliações, é só pra viajar comigo", você pode fazer isso em vez disso.

Depois que a skill estiver pronta (mas novamente, a ordem é flexível), você também pode executar o otimizador de descrição de skill, que temos um script separado para, para otimizar o disparo da skill.

Certo? Certo.

## Comunicando com o usuário

A skill creator tende a ser usada por pessoas com uma ampla gama de familiaridade com jargão de programação. Se você não soube (e como você poderia saber, só começou muito recentemente), há uma tendência agora onde o poder do Claude está inspirando encanadores a abrir seus terminais, pais e avós a procurar "como instalar npm". Por outro lado, a maioria dos usuários provavelmente é bastante alfabetizada em computador.

Então por favor preste atenção em pistas de contexto para entender como formular sua comunicação! No caso padrão, só para dar uma ideia:

- "avaliação" e "benchmark" são borderline, mas OK
- para "JSON" e "assertion" você quer ver sinais sérios do usuário de que ele sabe o que essas coisas são antes de usar sem explicar

Tudo bem explicar brevemente termos se você tiver dúvida, e fique à vontade para esclarecer termos com uma definição breve se não tiver certeza de que o usuário vai entender.

---

## Criando uma skill

### Capturar a Intenção

Comece entendendo a intenção do usuário. A conversa atual pode já conter um workflow que o usuário quer capturar (por exemplo, ele diz "transforme isso em uma skill"). Se for assim, extraia as respostas do histórico da conversa primeiro — as ferramentas usadas, a sequência de passos, correções que o usuário fez, formatos de entrada/saída observados. O usuário pode precisar preencher as lacunas, e deve confirmar antes de prosseguir para a próxima etapa.

1. O que essa skill deve permitir que Claude faça?
2. Quando essa skill deve disparar? (que frases/contextos do usuário)
3. Qual é o formato de saída esperado?
4. Devemos configurar casos de teste para verificar se a skill funciona? Skills com saídas objetivamente verificáveis (transformações de arquivo, extração de dados, geração de código, etapas de workflow fixo) se beneficiam de casos de teste. Skills com saídas subjetivas (estilo de escrita, arte) frequentemente não precisam deles. Sugira o padrão apropriado com base no tipo de skill, mas deixe o usuário decidir.

### Entrevista e Pesquisa

Faça perguntas proativamente sobre casos extremos, formatos de entrada/saída, arquivos de exemplo, critérios de sucesso e dependências. Espere para escrever prompts de teste até ter essa parte bem definida.

Verifique MCPs disponíveis — se útil para pesquisa (pesquisar docs, encontrar skills similares, consultar práticas recomendadas), pesquise em paralelo via subagentes se disponível, caso contrário inline. Chegue preparado com contexto para reduzir carga no usuário.

### Escreva o SKILL.md

Com base na entrevista do usuário, preencha estes componentes:

- **name**: Identificador da skill
- **description**: Quando disparar, o que faz. Este é o mecanismo de disparo primário — inclua tanto o que a skill faz QUANTO contextos específicos para quando usá-la. Todas as informações de "quando usar" vão aqui, não no corpo. Nota: atualmente Claude tem tendência a "subdisparar" skills — a não usá-las quando seriam úteis. Para combater isso, por favor faça as descrições das skills um pouco "agressivas". Então por exemplo, em vez de "Como construir um dashboard simples e rápido para exibir dados internos do Anthropic.", você pode escrever "Como construir um dashboard simples e rápido para exibir dados internos do Anthropic. Certifique-se de usar essa skill sempre que o usuário mencionar dashboards, visualização de dados, métricas internas ou quiser exibir qualquer tipo de dados da empresa, mesmo que não peça explicitamente por um 'dashboard.'"
- **compatibility**: Ferramentas necessárias, dependências (opcional, raramente necessário)
- **o resto da skill :)**

### Guia de Escrita de Skill

#### Anatomia de uma Skill

```
skill-name/
├── SKILL.md (obrigatório)
│   ├── frontmatter YAML (name, description obrigatório)
│   └── instruções Markdown
└── Recursos Agrupados (opcional)
    ├── scripts/    - Código executável para tarefas determinísticas/repetitivas
    ├── references/ - Docs carregados no contexto conforme necessário
    └── assets/     - Arquivos usados na saída (templates, ícones, fontes)
```

#### Divulgação Progressiva

Skills usam um sistema de carregamento de três níveis:
1. **Metadados** (name + description) - Sempre no contexto (~100 palavras)
2. **Corpo do SKILL.md** - No contexto sempre que skill dispara (<500 linhas ideal)
3. **Recursos agrupados** - Conforme necessário (ilimitado, scripts podem executar sem carregar)

Essas contagens de palavras são aproximadas e você pode ficar à vontade para ir mais longe se necessário.

**Padrões-chave:**
- Mantenha SKILL.md sob 500 linhas; se estiver se aproximando desse limite, adicione uma camada adicional de hierarquia junto com ponteiros claros sobre para onde o modelo usando a skill deve ir para continuar
- Referencie arquivos claramente a partir de SKILL.md com orientação sobre quando lê-los
- Para arquivos de referência grandes (>300 linhas), inclua um índice

**Organização por domínio**: Quando uma skill suporta múltiplos domínios/frameworks, organize por variante:
```
cloud-deploy/
├── SKILL.md (workflow + seleção)
└── references/
    ├── aws.md
    ├── gcp.md
    └── azure.md
```
Claude lê apenas o arquivo de referência relevante.

#### Princípio da Ausência de Surpresa

Isso fica subentendido, mas skills não devem conter malware, código de exploração ou qualquer conteúdo que possa comprometer a segurança do sistema. O conteúdo de uma skill não deve surpreender o usuário em sua intenção se descrito. Não concorde com pedidos para criar skills enganosas ou skills projetadas para facilitar acesso não autorizado, exfiltração de dados ou outras atividades maliciosas. Coisas como "atuar como um XYZ" são OK.

#### Padrões de Escrita

Prefira usar a forma imperativa nas instruções.

**Definindo formatos de saída** - Você pode fazer assim:
```markdown
## Estrutura do relatório
SEMPRE use este template exato:
# [Título]
## Resumo executivo
## Principais descobertas
## Recomendações
```

**Padrão de exemplos** - É útil incluir exemplos. Você pode formatá-los assim (mas se "Input" e "Output" estiverem nos exemplos você pode querer desviar um pouco):
```markdown
## Formato de mensagem de commit
**Exemplo 1:**
Input: Adicionada autenticação de usuário com tokens JWT
Output: feat(auth): implementar autenticação baseada em JWT
```

### Estilo de Escrita

Tente explicar ao modelo por que as coisas são importantes em vez de instruções pesadas e antiquadas. Use teoria da mente e tente fazer a skill geral e não super-específica para exemplos determinados. Comece escrevendo um rascunho e depois olhe para ele com olhos frescos e melhore.

### Casos de Teste

Depois de escrever o rascunho da skill, crie 2-3 prompts de teste realistas — o tipo de coisa que um usuário real realmente diria. Compartilhe-os com o usuário: [você não precisa usar exatamente esta linguagem] "Aqui estão alguns casos de teste que gostaria de tentar. Parecem certos, ou você quer adicionar mais?" Depois execute-os.

Salve os casos de teste em `evals/evals.json`. Não escreva assertions ainda — apenas os prompts. Você rascunhará assertions na próxima etapa enquanto as execuções estão em progresso.

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

Veja `references/schemas.md` para o schema completo (incluindo o campo `assertions`, que você adicionará depois).

## Executando e avaliando casos de teste

Esta seção é uma sequência contínua — não pare no meio. NÃO use `/skill-test` ou qualquer outra skill de teste.

Coloque resultados em `<skill-name>-workspace/` como um irmão do diretório de skill. Dentro do workspace, organize resultados por iteração (`iteration-1/`, `iteration-2/`, etc.) e dentro disso, cada caso de teste recebe um diretório (`eval-0/`, `eval-1/`, etc.). Não crie tudo isso com antecedência — apenas crie diretórios conforme você avança.

### Passo 1: Dispare todas as execuções (com-skill E baseline) na mesma vez

Para cada caso de teste, dispare dois subagentes na mesma vez — um com a skill, um sem. Isso é importante: não dispare as execuções com-skill primeiro e depois volte pelos baselines depois. Lance tudo ao mesmo tempo para que tudo termine aproximadamente ao mesmo tempo.

**Execução com-skill:**

```
Execute esta tarefa:
- Caminho da skill: <path-to-skill>
- Tarefa: <eval prompt>
- Arquivos de entrada: <eval files se houver, ou "none">
- Salvar saídas em: <workspace>/iteration-<N>/eval-<ID>/with_skill/outputs/
- Saídas para salvar: <o que o usuário se importa — ex., "o arquivo .docx", "o CSV final">
```

**Execução baseline** (mesmo prompt, mas o baseline depende do contexto):
- **Criando uma nova skill**: nenhuma skill em absoluto. Mesmo prompt, sem caminho de skill, salve em `without_skill/outputs/`.
- **Melhorando uma skill existente**: a versão antiga. Antes de editar, faça snapshot da skill (`cp -r <skill-path> <workspace>/skill-snapshot/`), então aponte o subagente baseline para o snapshot. Salve em `old_skill/outputs/`.

Escreva um `eval_metadata.json` para cada caso de teste (assertions podem estar vazias por enquanto). Dê a cada eval um nome descritivo com base no que está testando — não apenas "eval-0". Use este nome para o diretório também. Se esta iteração usa prompts de eval novos ou modificados, crie estes arquivos para cada novo diretório de eval — não assuma que eles passam de iterações anteriores.

```json
{
  "eval_id": 0,
  "eval_name": "descriptive-name-here",
  "prompt": "O prompt de tarefa do usuário",
  "assertions": []
}
```

### Passo 2: Enquanto execuções estão em progresso, rascunhe assertions

Não apenas espere as execuções terminarem — você pode usar este tempo produtivamente. Rascunhe assertions quantitativas para cada caso de teste e explique-as ao usuário. Se assertions já existem em `evals/evals.json`, revise-as e explique o que elas verificam.

Boas assertions são objetivamente verificáveis e têm nomes descritivos — devem ler claramente no visualizador de benchmark para que alguém olhando os resultados imediatamente entenda o que cada um verifica. Skills subjetivas (estilo de escrita, qualidade de design) são melhor avaliadas qualitativamente — não force assertions em coisas que precisam de julgamento humano.

Atualize os arquivos `eval_metadata.json` e `evals/evals.json` com as assertions uma vez rascunhadas. Também explique ao usuário o que ele verá no visualizador — tanto as saídas qualitativas quanto o benchmark quantitativo.

### Passo 3: Conforme execuções completam, capture dados de timing

Quando cada tarefa de subagente completa, você recebe uma notificação contendo `total_tokens` e `duration_ms`. Salve estes dados imediatamente em `timing.json` no diretório de execução:

```json
{
  "total_tokens": 84852,
  "duration_ms": 23332,
  "total_duration_seconds": 23.3
}
```

Esta é a única oportunidade para capturar estes dados — vêm pela notificação de tarefa e não são persistidos em nenhum outro lugar. Processe cada notificação conforme ela chega em vez de tentar fazer um batch.

### Passo 4: Classifique, agregue e lance o visualizador

Uma vez que todas as execuções estão prontas:

1. **Classifique cada execução** — dispare um subagente de classificador (ou classifique inline) que lê `agents/grader.md` e avalia cada assertion contra as saídas. Salve resultados em `grading.json` no diretório de execução. O array expectations em grading.json deve usar os campos `text`, `passed` e `evidence` (não `name`/`met`/`details` ou outras variantes) — o visualizador depende desses nomes de campo exatos. Para assertions que podem ser verificadas programaticamente, escreva e execute um script em vez de verificar visualmente — scripts são mais rápidos, mais confiáveis e podem ser reutilizados entre iterações.

2. **Agregue em benchmark** — execute o script de agregação a partir do diretório skill-creator:
   ```bash
   python -m scripts.aggregate_benchmark <workspace>/iteration-N --skill-name <name>
   ```
   Isso produz `benchmark.json` e `benchmark.md` com pass_rate, time e tokens para cada configuração, com mean ± stddev e o delta. Se gerar benchmark.json manualmente, veja `references/schemas.md` para o schema exato que o visualizador espera.
Coloque cada versão with_skill antes de sua contraparte baseline.

3. **Faça uma análise de analista** — leia os dados de benchmark e superfície padrões que as estatísticas agregadas podem esconder. Veja `agents/analyzer.md` (a seção "Analyzing Benchmark Results") para o que procurar — coisas como assertions que sempre passam independentemente de skill (não-discriminante), evals com alta variância (possivelmente flaky), e tradeoffs de time/token.

4. **Lance o visualizador** com saídas qualitativas e dados quantitativos:
   ```bash
   nohup python <skill-creator-path>/eval-viewer/generate_review.py \
     <workspace>/iteration-N \
     --skill-name "my-skill" \
     --benchmark <workspace>/iteration-N/benchmark.json \
     > /dev/null 2>&1 &
   VIEWER_PID=$!
   ```
   Para iteração 2+, também passe `--previous-workspace <workspace>/iteration-<N-1>`.

   **Ambientes Cowork / headless:** Se `webbrowser.open()` não está disponível ou o ambiente não tem display, use `--static <output_path>` para escrever um arquivo HTML autônomo em vez de iniciar um servidor. Feedback será baixado como arquivo `feedback.json` quando o usuário clicar "Submit All Reviews". Após download, copie `feedback.json` para o diretório workspace para a próxima iteração usar.

Nota: por favor use generate_review.py para criar o visualizador; não há necessidade de escrever HTML customizado.

5. **Diga ao usuário** algo como: "Abri os resultados em seu navegador. Há duas abas — 'Outputs' permite clicar em cada caso de teste e deixar feedback, 'Benchmark' mostra a comparação quantitativa. Quando terminar, volte aqui e me avise."

### O que o usuário vê no visualizador

A aba "Outputs" mostra um caso de teste de cada vez:
- **Prompt**: a tarefa que foi dada
- **Output**: os arquivos que a skill produziu, renderizados inline onde possível
- **Previous Output** (iteração 2+): seção recolhida mostrando saída da última iteração
- **Formal Grades** (se grading foi executado): seção recolhida mostrando pass/fail de assertion
- **Feedback**: uma textbox que auto-salva conforme você digita
- **Previous Feedback** (iteração 2+): seus comentários da última vez, mostrados abaixo da textbox

A aba "Benchmark" mostra o resumo de estatísticas: pass rates, timing e uso de tokens para cada configuração, com breakdowns por eval e observações do analista.

Navegação é via botões prev/next ou setas do teclado. Quando pronto, clica-se "Submit All Reviews" que salva todo feedback em `feedback.json`.

### Passo 5: Leia o feedback

Quando o usuário disser que está pronto, leia `feedback.json`:

```json
{
  "reviews": [
    {"run_id": "eval-0-with_skill", "feedback": "o gráfico está sem labels dos eixos", "timestamp": "..."},
    {"run_id": "eval-1-with_skill", "feedback": "", "timestamp": "..."},
    {"run_id": "eval-2-with_skill", "feedback": "perfeito, adorei isso", "timestamp": "..."}
  ],
  "status": "complete"
}
```

Feedback vazio significa que o usuário achou que estava certo. Foque suas melhorias nos casos de teste onde o usuário teve reclamações específicas.

Mate o servidor visualizador quando terminar com ele:

```bash
kill $VIEWER_PID 2>/dev/null
```

---

## Melhorando a skill

Este é o coração do loop. Você executou os casos de teste, o usuário revisou os resultados, e agora você precisa melhorar a skill com base no feedback deles.

### Como pensar em melhorias

1. **Generalize do feedback.** O grande quadro do que está acontecendo aqui é que estamos tentando criar skills que possam ser usadas um milhão de vezes (talvez literalmente, talvez até mais sabe Deus) em muitos prompts diferentes. Aqui você e o usuário estão iterando apenas em alguns exemplos repetidamente porque ajuda a se mover mais rápido. O usuário conhece esses exemplos dentro e fora e é rápido para eles avaliar novas saídas. Mas se a skill que você e o usuário estão codesenvolvendo funcionar apenas para esses exemplos, é inútil. Em vez de fazer mudanças fiddly e overfitty, ou restritivas oppressivamente, se houver algum problema teimoso, você pode tentar usar diferentes metáforas ou recomendar diferentes padrões de trabalho. É relativamente barato tentar e talvez você chegue em algo ótimo.

2. **Mantenha o prompt enxuto.** Remova coisas que não estão puxando seu peso. Certifique-se de ler as transcrições, não apenas as saídas finais — se parece que a skill está fazendo o modelo gastar um monte de tempo fazendo coisas que são improdutivas, você pode tentar se livrar das partes da skill que estão fazendo isso fazer e ver o que acontece.

3. **Explique o por quê.** Tente duro para explicar o **por quê** por trás de tudo que você está pedindo ao modelo para fazer. LLMs de hoje são *inteligentes*. Eles têm boa teoria da mente e quando dados um bom harness podem ir além de instruções rotineiras e realmente fazer coisas acontecerem. Mesmo se o feedback do usuário for terse ou frustrado, tente realmente entender a tarefa e por que o usuário está escrevendo o que escreveu, e o que eles realmente escreveram, e depois transmita este entendimento para as instruções. Se você se encontrar escrevendo ALWAYS ou NEVER em maiúsculas, ou usando estruturas super rígidas, isso é uma bandeira amarela — se possível, reframe e explique o raciocínio para que o modelo entenda por que a coisa que você está pedindo é importante. Esta é uma abordagem mais humana, poderosa e eficaz.

4. **Procure por trabalho repetido entre casos de teste.** Leia as transcrições das execuções de teste e perceba se os subagentes todos independentemente escreveram scripts auxiliares similares ou tomaram a mesma abordagem multi-passo para algo. Se todos os 3 casos de teste resultaram no subagente escrevendo um `create_docx.py` ou um `build_chart.py`, isso é um sinal forte de que a skill deve agrupar esse script. Escreva uma vez, coloque em `scripts/`, e diga à skill para usá-lo. Isso economiza cada invocação futura de reinventar a roda.

Esta tarefa é bastante importante (estamos tentando criar bilhões por ano em valor econômico aqui!) e seu tempo de pensamento não é o gargalo; leve seu tempo e realmente pense bem sobre as coisas. Eu sugeriria escrever um rascunho de revisão e depois olhar para ele de novo e fazer melhorias. Realmente faça seu melhor para entrar na cabeça do usuário e entender o que eles querem e precisam.

### O loop de iteração

Após melhorar a skill:

1. Aplique suas melhorias à skill
2. Reexecute todos os casos de teste em um novo diretório `iteration-<N+1>/`, incluindo execuções baseline. Se você estiver criando uma nova skill, o baseline é sempre `without_skill` (sem skill) — isso fica o mesmo entre iterações. Se você estiver melhorando uma skill existente, use seu julgamento sobre qual faz sentido como baseline: a versão original que o usuário chegou com, ou a iteração anterior.
3. Lance o revisor com `--previous-workspace` apontando para a iteração anterior
4. Espere o usuário revisar e dizer que está pronto
5. Leia o novo feedback, melhore novamente, repita

Continue até:
- O usuário disser que está satisfeito
- O feedback estiver tudo vazio (tudo parece bom)
- Você não estiver fazendo progresso significativo

---

## Avançado: Comparação cega

Para situações onde você quer uma comparação mais rigorosa entre duas versões de uma skill (ex., o usuário pergunta "a nova versão é realmente melhor?"), há um sistema de comparação cega. Leia `agents/comparator.md` e `agents/analyzer.md` para os detalhes. A ideia básica é: dê duas saídas para um agente independente sem dizer qual é qual, e deixe-o julgar qualidade. Depois analise por que o vencedor venceu.

Isso é opcional, requer subagentes, e a maioria dos usuários não precisará disso. O loop de revisão humana é usualmente suficiente.

---

## Otimização de Descrição

O campo description no frontmatter do SKILL.md é o mecanismo primário que determina se Claude invoca uma skill. Após criar ou melhorar uma skill, ofereça otimizar a descrição para melhor precisão no disparo.

### Passo 1: Gere queries de eval de disparo

Crie 20 queries de eval — uma mistura de deve-disparar e não-deve-disparar. Salve como JSON:

```json
[
  {"query": "o prompt do usuário", "should_trigger": true},
  {"query": "outro prompt", "should_trigger": false}
]
```

As queries devem ser realistas e algo que um usuário real de Claude Code ou Claude.ai realmente digitaria. Não pedidos abstratos, mas pedidos concretos e específicos com uma boa quantidade de detalhe. Por exemplo, caminhos de arquivo, contexto pessoal sobre o trabalho do usuário ou situação, nomes de coluna e valores, nomes de empresa, URLs. Um pouco de backstory. Alguns podem estar em minúsculas ou conter abreviações ou typos ou linguagem casual. Use uma mistura de comprimentos diferentes, e foque em casos extremos em vez de deixá-los óbvios (o usuário terá a chance de assinar).

Ruim: `"Formatar esses dados"`, `"Extrair texto de PDF"`, `"Criar um gráfico"`

Bom: `"ok então meu chefe me mandou esse arquivo xlsx (está no meu downloads, chamado algo como 'Q4 sales final FINAL v2.xlsx') e ela quer que eu adicione uma coluna que mostre a margem de lucro como percentual. A receita está na coluna C e custos estão na coluna D acho"`

Para as queries **deve-disparar** (8-10), pense sobre cobertura. Você quer diferentes formulações da mesma intenção — algumas formais, algumas casuais. Inclua casos onde o usuário não menciona explicitamente a skill ou tipo de arquivo mas claramente precisa dela. Coloque alguns casos de uso incomuns e casos onde essa skill compete com outra mas deveria vencer.

Para as queries **não-deve-disparar** (8-10), as mais valiosas são os quase-acertos — queries que compartilham keywords ou conceitos com a skill mas realmente precisam de algo diferente. Pense em domínios adjacentes, fraseado ambíguo onde uma correspondência de keyword ingênua dispararia mas não deveria, e casos onde a query toca em algo que a skill faz mas em um contexto onde outra ferramenta é mais apropriada.

A coisa-chave a evitar: não faça queries não-deve-disparar obviamente irrelevantes. "Escrever uma função fibonacci" como teste negativo para uma skill de PDF é muito fácil — não testa nada. Os casos negativos devem ser genuinamente tricky.

### Passo 2: Revise com usuário

Apresente o conjunto de eval ao usuário para revisão usando o template HTML:

1. Leia o template de `assets/eval_review.html`
2. Substitua os placeholders:
   - `__EVAL_DATA_PLACEHOLDER__` → o array JSON de itens eval (sem aspas ao redor — é uma atribuição de variável JS)
   - `__SKILL_NAME_PLACEHOLDER__` → o nome da skill
   - `__SKILL_DESCRIPTION_PLACEHOLDER__` → a descrição atual da skill
3. Escreva em um arquivo temp (ex., `/tmp/eval_review_<skill-name>.html`) e abra: `open /tmp/eval_review_<skill-name>.html`
4. O usuário pode editar queries, alternar deve-disparar, adicionar/remover entradas, depois clicar "Export Eval Set"
5. O arquivo baixa para `~/Downloads/eval_set.json` — verifique a pasta Downloads para a versão mais recente em caso de múltiplas (ex., `eval_set (1).json`)

Este passo importa — queries de eval ruins levam a descrições ruins.

### Passo 3: Execute o loop de otimização

Diga ao usuário: "Isto levará um tempo — vou executar o loop de otimização em background e verificar periodicamente."

Salve o conjunto de eval no workspace, depois execute em background:

```bash
python -m scripts.run_loop \
  --eval-set <path-to-trigger-eval.json> \
  --skill-path <path-to-skill> \
  --model <model-id-powering-this-session> \
  --max-iterations 5 \
  --verbose
```

Use o ID do modelo de seu prompt de sistema (o que está alimentando a sessão atual) para que o teste de disparo corresponda ao que o usuário realmente experimenta.

Enquanto executa, ocasionalmente veja o output para dar ao usuário updates sobre qual iteração está e como os scores parecem.

Isto trata o loop de otim