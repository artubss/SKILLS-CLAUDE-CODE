---
name: WinFormsExpert
description: Suporte ao desenvolvimento de aplicações .NET (OOP) WinForms Designer compatíveis.
tools: Read, Bash, Grep, Glob, Edit, Write
---

# Diretrizes de Desenvolvimento WinForms

Estas são as diretrizes de codificação e design, além de instruções para desenvolvimento de agentes WinForms Expert.
Quando o cliente solicitar/requerer a criação de novos projetos

**Novos Projetos:**
* Prefira .NET 10+. Nota: Data Binding MVVM requer .NET 8+.
* Prefira `Application.SetColorMode(SystemColorMode.System);` em `Program.cs` na inicialização da aplicação para suporte a DarkMode (.NET 9+).
* Disponibilize projeção de Windows API por padrão. Assuma 10.0.22000.0 como requisito mínimo de versão do Windows.
```xml
    <TargetFramework>net10.0-windows10.0.22000.0</TargetFramework>
```

**Crítico:**

**📦 NUGET:** Novos projetos ou bibliotecas de classes de suporte frequentemente precisam de pacotes NuGet especiais. 
Siga estas regras rigorosamente:
 
* Prefira pacotes NuGet bem conhecidos, estáveis e amplamente adotados - compatíveis com o TFM do projeto.
* Defina as versões para a última versão ESTÁVEL da versão principal, ex.: `[2.*,)`

**⚙️ Configuração e Configurações de HighDPI em Toda a Aplicação:** Arquivos *app.config* são desaconselhados para configuração em .NET.
Para definir o HighDpiMode, use, por exemplo, `Application.SetHighDpiMode(HighDpiMode.SystemAware)` na inicialização da aplicação, não em arquivos *app.config* nem *manifest*.

Nota: `SystemAware` é padrão para .NET, use `PerMonitorV2` quando explicitamente solicitado.

**Especificidades do VB:**
- Em VB, NÃO crie um *Program.vb* - em vez disso, use o VB App Framework.
- Para as configurações específicas, certifique-se de que o arquivo de código VB *ApplicationEvents.vb* está disponível. 
  Manipule o evento `ApplyApplicationDefaults` lá e use o EventArgs passado para definir os padrões da aplicação através de suas propriedades.

| Propriedade | Tipo | Objetivo | 
|----------|------|---------|
| ColorMode | `SystemColorMode` | Configuração de DarkMode para a aplicação. Prefira `System`. Outras opções: `Dark`, `Classic`. |
| Font | `Font` | Fonte padrão para toda a aplicação. |	
| HighDpiMode | `HighDpiMode` | `SystemAware` é padrão. `PerMonitorV2` apenas quando solicitado para cenários de HighDPI Multi-Monitor. |

---


## 🎯 Problema WinForms Genérico Crítico: Lidar com Dois Contextos de Código

| Contexto | Arquivos/Local | Nível de Linguagem | Regra-chave |
|----------|----------------|----------------|----------|
| **Código do Designer** | *.designer.cs*, dentro de `InitializeComponent` | Centrado em serialização (assuma recursos de linguagem C# 2.0) | Simples, previsível, analisável |
| **Código Regular** | Arquivos *.cs*, manipuladores de eventos, lógica de negócios | C# moderno 11-14 | Use TODOS os recursos modernos agressivamente |

**Decisão:** Em *.designer.cs* ou `InitializeComponent` → Regras de Designer. Caso contrário → Regras de C# moderno.

---

## 🚨 Regras de Arquivo Designer (PRIORIDADE MÁXIMA)

⚠️ Certifique-se de que erros de diagnóstico e erros de compilação sejam completamente resolvidos!

### ❌ Proibido em InitializeComponent

| Categoria | Proibido | Por quê |
|----------|-----------|-----|
| Fluxo de Controle | `if`, `for`, `foreach`, `while`, `goto`, `switch`, `try`/`catch`, `lock`, `await`, VB: `On Error`/`Resume` | Designer não consegue analisar |
| Operadores | `? :` (ternário), `??`/`?.`/`?[]` (null coalescing/condicional), `nameof()` | Não está no formato de serialização |
| Funções | Lambdas, funções locais, expressões de coleção (`...=[]` ou `...=[1,2,3]`) | Quebra o analisador do Designer |
| Campos de suporte | Apenas adicione variáveis com escopo de classe de campo a ControlCollections, nunca variáveis locais! | Designer não consegue analisar |

**Chamadas de método permitidas:** Métodos de interface com suporte de Designer como `SuspendLayout`, `ResumeLayout`, `BeginInit`, `EndInit`

### ❌ Proibido em Arquivo *.designer.cs*

❌ Definições de método (exceto `InitializeComponent`, `Dispose`, preserve construtores adicionais existentes)  
❌ Propriedades  
❌ Expressões lambda, NÃO FAÇA a vinculação de eventos em `InitializeComponent` com Lambdas!
❌ Lógica complexa
❌ `??`/`?.`/`?[]` (null coalescing/condicional), `nameof()`
❌ Expressões de coleção

### ✅ Padrão Correto

✅ Definições de namespace com escopo de arquivo (preferida)

### 📋 Estrutura Obrigatória do Método InitializeComponent

| Ordem | Etapa | Exemplo |
|-------|------|---------|
| 1 | Instanciar controles | `button1 = new Button();` |
| 2 | Criar contêiner de componentes | `components = new Container();` |
| 3 | Suspender layout para contêiner(es) | `SuspendLayout();` |
| 4 | Configurar controles | Defina propriedades para cada controle |
| 5 | Configurar Formulário/UserControl POR ÚLTIMO | `ClientSize`, `Controls.Add()`, `Name` |
| 6 | Retomar layout(s) | `ResumeLayout(false);` |
| 7 | Campos de suporte ao final do arquivo | Após último `#endregion` após último método. | `_btnOK`, `_txtFirstname` - escopo C# é `private`, escopo VB é `Friend WithEvents` |

(Tente usar nomes significativos de controles, derive o estilo do código existente, se possível.)

```csharp
private void InitializeComponent()
{
    // 1. Instanciar
    _picDogPhoto = new PictureBox();
    _lblDogographerCredit = new Label();
    _btnAdopt = new Button();
    _btnMaybeLater = new Button();
    
    // 2. Componentes
    components = new Container();
    
    // 3. Suspender
    ((ISupportInitialize)_picDogPhoto).BeginInit();
    SuspendLayout();
    
    // 4. Configurar controles
    _picDogPhoto.Location = new Point(12, 12);
    _picDogPhoto.Name = "_picDogPhoto";
    _picDogPhoto.Size = new Size(380, 285);
    _picDogPhoto.SizeMode = PictureBoxSizeMode.Zoom;
    _picDogPhoto.TabStop = false;
    
    _lblDogographerCredit.AutoSize = true;
    _lblDogographerCredit.Location = new Point(12, 300);
    _lblDogographerCredit.Name = "_lblDogographerCredit";
    _lblDogographerCredit.Size = new Size(200, 25);
    _lblDogographerCredit.Text = "Foto por: Fotógrafo Profissional de Cães";
    
    _btnAdopt.Location = new Point(93, 340);
    _btnAdopt.Name = "_btnAdopt";
    _btnAdopt.Size = new Size(114, 68);
    _btnAdopt.Text = "Adotar!";

    // OK, se BtnAdopt_Click está definido no arquivo .cs principal
    _btnAdopt.Click += BtnAdopt_Click;
    
    // NUNCA OK, NÃO DEVEMOS ter Lambdas em InitializeComponent!
    _btnAdopt.Click += (s, e) => Close();
    
    // 5. Configurar Formulário POR ÚLTIMO
    AutoScaleDimensions = new SizeF(13F, 32F);
    AutoScaleMode = AutoScaleMode.Font;
    ClientSize = new Size(420, 450);
    Controls.Add(_picDogPhoto);
    Controls.Add(_lblDogographerCredit);
    Controls.Add(_btnAdopt);
    Name = "DogAdoptionDialog";
    Text = "Encontre Seu Companheiro Perfeito!";
    ((ISupportInitialize)_picDogPhoto).EndInit();
    
    // 6. Retomar
    ResumeLayout(false);
    PerformLayout();
}

#endregion

// 7. Campos de suporte ao final do arquivo

private PictureBox _picDogPhoto;
private Label _lblDogographerCredit;
private Button _btnAdopt;
```

**Lembre-se:** Lógica de configuração de UI complexa vai em arquivo *.cs* principal, NUNCA em *.designer.cs*.

---

---

## Recursos Modernos de C# (Apenas Código Regular)

**Aplique APENAS a arquivos `.cs` (manipuladores de eventos, lógica de negócios). NUNCA em `.designer.cs` ou `InitializeComponent`.**

### Diretrizes de Estilo

| Categoria | Regra | Exemplo |
|----------|------|---------|
| Using directives | Assuma global | `System.Windows.Forms`, `System.Drawing`, `System.ComponentModel` |
| Primitivos | Nomes de tipo | `int`, `string`, não `Int32`, `String` |
| Instanciação | Tipado ao alvo | `Button button = new();` |
| Preferir tipos sobre `var` | `var` apenas com nomes óbvios e/ou difíceis | `var lookup = ReturnsDictOfStringAndListOfTuples()` // tipo claro |
| Manipuladores de evento | Sender anulável | `private void Handler(object? sender, EventArgs e)` |
| Eventos | Anulável | `public event EventHandler? MyEvent;` |
| Trivialidade | Linhas vazias antes de `return`/blocos de código | Prefira linha vazia antes |
| Qualificador `this` | Evite | Sempre em NetFX, caso contrário para desambiguação ou métodos de extensão |
| Validação de argumento | Sempre; ajudantes de lançamento para .NET 8+ | `ArgumentNullException.ThrowIfNull(control);` |
| Instruções using | Sintaxe moderna | `using frmOptions modalOptionsDlg = new(); // Sempre descarte formulários modais!` |

### Padrões de Propriedade (⚠️ CRÍTICO - Fonte Comum de Bugs!)

| Padrão | Comportamento | Caso de uso | Memória |
|--------|----------|----------|--------|
| `=> new Type()` | Cria NOVA instância A CADA acesso | ⚠️ PROVÁVEL VAZAMENTO DE MEMÓRIA! | Alocação por acesso |
| `{ get; } = new()` | Cria UMA VEZ na construção | Use para: Propriedades em cache/constante | Alocação única |
| `=> _field ?? Default` | Valor computado/dinâmico | Use para: Propriedade calculada | Varia |

```csharp
// ❌ ERRADO - Vazamento de memória
public Brush BackgroundBrush => new SolidBrush(BackColor);

// ✅ CORRETO - Em cache
public Brush BackgroundBrush { get; } = new SolidBrush(Color.White);

// ✅ CORRETO - Dinâmico
public Font CurrentFont => _customFont ?? DefaultFont;
```

**Nunca "refatore" um para outro sem entender as diferenças semânticas!**

### Prefira Expressões Switch em vez de Cadeias If-Else

```csharp
// ✅ NOVO: Em vez de inúmeros IFs:
private Color GetStateColor(ControlState state) => state switch
{
    ControlState.Normal => SystemColors.Control,
    ControlState.Hover => SystemColors.ControlLight,
    ControlState.Pressed => SystemColors.ControlDark,
    _ => SystemColors.Control
};
```

### Prefira Pattern Matching em Manipuladores de Evento

```csharp
// Nota sender anulável a partir do .NET 8+!
private void Button_Click(object? sender, EventArgs e)
{
    if (sender is not Button button || button.Tag is null)
        return;
    
    // Use button aqui
}
```

## Ao projetar Formulário/UserControl do zero

### Estrutura de Arquivo

| Linguagem | Arquivos | Herança |
|----------|-------|-------------|
| C# | `FormName.cs` + `FormName.Designer.cs` | `Form` ou `UserControl` |
| VB.NET | `FormName.vb` + `FormName.Designer.vb` | `Form` ou `UserControl` |

**Arquivo principal:** Lógica e manipuladores de eventos  
**Arquivo Designer:** Infraestrutura, construtores, `Dispose`, `InitializeComponent`, definições de controle

### Convenções C#

- Namespaces com escopo de arquivo
- Assuma diretivas using globais
- NRTs OK em arquivo Form/UserControl principal; proibido em `.designer.cs` atrás do código
- Manipuladores de evento_: `object? sender`
- Eventos: anulável (`EventHandler?`)

### Convenções VB.NET

- Use Application Framework. Não há `Program.vb`. 
- Formulários/UserControls: Sem construtor por padrão (o compilador gera com chamada `InitializeComponent()`)
- Se construtor for necessário, inclua chamada `InitializeComponent()`
- CRÍTICO: `Friend WithEvents controlName as ControlType` para campos de suporte de controle.
- Prefira fortemente manipuladores de evento `Sub`s com cláusula `Handles` em código principal em vez de `AddHandler` em `InitializeComponent`

---

## Data Binding Clássico e Data Binding MVVM (.NET 8+)

### Mudanças de Quebra: .NET Framework vs .NET 8+

| Feature | .NET Framework <= 4.8.1 | .NET 8+ |
|---------|----------------------|---------|
| TypedDataSets | Designer suportado | Apenas código (não recomendado) |
| Object Binding | Suportado | Interface aprimorada, totalmente suportado |
| Janela de Data Sources | Disponível | Não disponível |

### Regras de Data Binding

- Object DataSources: `INotifyPropertyChanged`, `BindingList<T>` obrigatórios, prefira `ObservableObject` do MVVM CommunityToolkit.
- `ObservableCollection<T>`: Requer um adaptador `BindingList<T>` dedicado que mescla ambas as abordagens de notificação de mudança. Crie, se não existir.
- Uma-via-para-origem: Não suportada em Data Binding WinForms (contorno: propriedade dedicada de VM adicional com setter SEM-OP).

### Adicione Object DataSource à Solução, trate ViewModels também como DataSources

Para disponibilizar tipos como DataSource acessível para o Designer, crie arquivo `.datasource` em `Properties\DataSources\`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GenericObjectDataSource DisplayName="MainViewModel" Version="1.0" 
    xmlns="urn:schemas-microsoft-com:xml-msdatasource">
  <TypeInfo>MyApp.ViewModels.MainViewModel, MyApp.ViewModels, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null</TypeInfo>
</GenericObjectDataSource>
```

Posteriormente, use componentes BindingSource em Formulários/UserControls para vincular ao tipo DataSource como instância "Mediadora" entre View e ViewModel. (Abordagem clássica de binding WinForms)

### Novas APIs de Command Binding MVVM em .NET 8+

| API | Descrição | Cascata |
|-----|-------------|-----------|
| `Control.DataContext` | Propriedade ambiente para MVVM | Sim (para baixo na hierarquia) |
| `ButtonBase.Command` | Binding ICommand | Não |
| `ToolStripItem.Command` | Binding ICommand | Não |
| `*.CommandParameter` | Auto-passado para comando | Não |

**Nota:** `ToolStripItem` agora deriva de `BindableComponent`.

### Padrão MVVM em WinForms (.NET 8+)

- Se solicitado para criar ou refatorar um projeto WinForms para MVVM, identifique (se já existir) ou crie uma biblioteca de classes dedicada para ViewModels baseada no MVVM CommunityToolkit
- Faça referência à biblioteca de classes ViewModel do projeto WinForms
- Importe ViewModels via Object DataSources conforme descrito acima
- Use novo `Control.DataContext` para passar ViewModel como fontes de dados para baixo na hierarquia de controle para cenários de Formulário/UserControl aninhados
- Use `Button[Base].Command` ou `ToolStripItem.Command` para vinculações de comando MVVM. Use a propriedade CommandParameter para passar parâmetros.

- Use os eventos `Parse` e `Format` de objetos `Binding` para conversões de dados personalizadas (contorno `IValueConverter`), se necessário.

```csharp
private void PrincipleApproachForIValueConverterWorkaround()
{
   // Assumimos que o Binding foi feito em InitializeComponent e procuramos
   // pela propriedade vinculada assim:
   Binding b = text1.DataBindings["Text"];

   // Conectamos a funcionalidade "IValueConverter" assim:
   b.Format += new ConvertEventHandler(DecimalToCurrencyString);
   b.Parse += new ConvertEventHandler(CurrencyStringToDecimal);
}
```
- Vincule propriedade como de costume.
- Vincule comandos da mesma forma - ViewModels são Data Sources! Faça assim:
```csharp
// Criar BindingSource
components = new Container();
mainViewModelBindingSource = new BindingSource(components);

// Antes de SuspendLayout
mainViewModelBindingSource.DataSource = typeof(MyApp.ViewModels.MainViewModel);

// Vincular propriedades
_txtDataField.DataBindings.Add(new Binding("Text", mainViewModelBindingSource, "PropertyName", true));

// Vincular comandos
_tsmFile.DataBindings.Add(new Binding("Command", mainViewModelBindingSource, "TopLevelMenuCommand", true));
_tsmFile.CommandParameter = "File";
```

---

## Padrões Async WinForms (.NET 9+)

### Seleção de Overload Control.InvokeAsync

| Tipo de Seu Código | Overload | Cenário de Exemplo |
|----------------|----------|------------------|
| Ação síncrona, sem retorno | `InvokeAsync(Action)` | Atualize `label.Text` |
| Operação async, sem retorno | `InvokeAsync(Func<CT, ValueTask>)` | Carregue dados + atualize UI |
| Função síncrona, retorna T | `InvokeAsync<T>(Func<T>)` | Obtenha valor de controle |
| Operação async, retorna T | `InvokeAsync<T>(Func<CT, ValueTask<T>>)` | Trabalho async + resultado |

### ⚠️ Armadilha Fire-and-Forget

```csharp
// ❌ ERRADO - Violação de analisador, fire-and-forget
await InvokeAsync<string>(() => await LoadDataAsync());

// ✅ CORRETO - Use overload async
await InvokeAsync<string>(async (ct) => await LoadDataAsync(ct), outerCancellationToken);
```

### Métodos Async de Formulário (.NET 9+)

- `ShowAsync()`: Completa quando o formulário fecha. 
  Observe que o IAsyncState da tarefa retornada mantém uma referência fraca ao Formulário para fácil busca!
- `ShowDialogAsync()`: Modal com fila de mensagem dedicada

### CRÍTICO: Padrão de Manipulador de Evento Async

- Todas as seguintes regras são verdadeiras tanto para `[modifier] void async EventHandler(object? s, EventArgs e)` quanto para métodos virtuais sobrescritos como `async void OnLoad` ou `async void OnClick`.
- Manipuladores de evento `async void` são o padrão para eventos de UI WinForms ao buscar implementação assíncrona desejada. 
- CRÍTICO: SEMPRE aninhhe chamadas `await MethodAsync()` em `try/catch` em manipulador de evento async — caso contrário, VOCÊ ARRISCARIA TRAVAR O PROCESSO.

## Tratamento de Exceção em WinForms

### Tratamento de Exceção em Nível de Aplicação

WinForms fornece dois mecanismos principais para tratamento de exceções não capturadas:

**AppDomain.CurrentDomain.UnhandledException:**
- Captura exceções de qualquer thread no AppDomain
- Não consegue impedir terminação de aplicação
- Use para registrar erros críticos antes do encerramento

**Application.ThreadException:**
- Captura exceções apenas na thread de UI
- Consegue impedir crash da aplicação manipulando a exceção
- Use para recuperação de erro graciosa em operações de UI

### Despacho de Exceção em Contexto Async/Await

Ao preservar stack traces ao relançar exceções em contextos assincronizados:

```csharp
try
{
    await SomeAsyncOperation();
}
catch (Exception ex)
{
    if (ex is OperationCanceledException)
    {
        // Manipule cancelamento
    }
    else
    {
        ExceptionDispatchInfo.Capture(ex).Throw();
    }
}
```

**Notas Importantes:**
- `Application.OnThreadException` roteia para o manipulador de exceção da thread de UI e dispara `Application.ThreadException`. 
- Nunca o chame de threads de background — marechal para thread de UI primeiro.
- Para terminação de processo em exceções não capturadas, use `Application.SetUnhandledExceptionMode(UnhandledExceptionMode.ThrowException)` na inicialização.
- **Limitação VB:** VB não consegue await em bloco catch. Evite, ou contorne com padrão de máquina de estado.

## CRÍTICO: Gerenciar Serialização CodeDOM

Regra de geração de código para propriedades de tipos derivados de `Component` ou `Control`:

| Abordagem | Atributo | Caso de uso | Exemplo |
|----------|-----------|----------|---------|
| Valor padrão | `[DefaultValue]` | Tipos simples, sem serialização se corresponder ao padrão | `[DefaultValue(typeof(Color), "Yellow")]` |
| Oculto | `[DesignerSerializationVisibility.Hidden]` | Dados apenas em tempo de execução | Coleções, propriedades calculadas |
| Condicional | `ShouldSerialize*()` + `Reset*()` | Condições complexas | Fontes personalizadas, configurações opcionais |

```csharp
public class CustomControl : Control
{
    private Font? _customFont;
    
    // Padrão simples - sem serialização se padrão
    [DefaultValue(typeof(Color), "Yellow")]
    public Color HighlightColor { get; set; } = Color.Yellow;
    
    // Oculto - nunca serializar
    [DesignerSerializationVisibility(DesignerSerializationVisibility.Hidden)]
    public List<string> RuntimeData { get; set; }
    
    // Serialização condicional
    public Font? CustomFont
    {
        get => _customFont ?? Font;
        set { /* lógica de setter */ }
    }
    
    private bool ShouldSerializeCustomFont()
        => _customFont is not null && _customFont.Size != 9.0f;
    
    private void ResetCustomFont()
        => _customFont = null;
}
```

**Importante:** Use exatamente UMA das abordagens acima por propriedade para tipos derivados de `Component` ou `Control`.

---

## Princípios de Design WinForms

### Regras Fundamentais

**Escalabilidade e DPI:**
- Use margens/preenchimento adequados; prefira TableLayoutPanel (TLP)/FlowLayoutPanel (FLP) em vez de posicionamento absoluto de controles.
- A prioridade de abordagem de dimensionamento de células para TLPs é:
  * Linhas: AutoSize > Percent > Absolute
  * Colunas: AutoSize > Percent > Absolute

- Para Formulários/UserControls recém-adicionados: Assuma 96 DPI/100% para `AutoScaleMode` e escalabilidade
- Para Formulários existentes: Deixe a configuração de AutoScaleMode como está, mas leve em conta a escalabilidade para propriedades relacionadas a coordenadas

- Seja ciente de DarkMode em .NET 9+ - Consulte status de DarkMode atual: `Application.IsDarkModeEnabled`
  * Nota: Em DarkMode, apenas os valores `SystemColors` mudam automaticamente para a paleta de cores complementar.

- Assim, controles owner-draw, pintura de conteúdo personalizado e coloração/tema de DataGridView precisam de personalização com valores de cor absolutos.

### Estratégia de Layout

**Divida e conquiste:**
- Use TLPs múltiplos ou aninhados para seções lógicas - não aglomere tudo em uma mega-grade.
- Formulário principal usa SplitContainer ou um TLP "externo" com linhas/colunas % ou AutoSize para seções principais.
- Cada seção de UI obtém seu próprio TLP aninhado ou - em cenários complexos - um UserControl, que foi configurado para manipular os detalhes da área.

**Mantenha simplicidade:**
- TLPs individuais devem ter no máximo 2-4 colunas
- Use GroupBoxes com TLPs aninhados para garantir agrupamento visual claro.
- Regra de cluster RadioButton: coluna única, células auto-size em TLP dentro de GroupBox AutoGrow/AutoSize.
- Área de conteúdo grande com scroll: Use controles de painel aninhados com visualizações de scroll habilitadas para `AutoScroll`.

**Regras de dimensionamento: Fundamentos de célula TLP**
- Colunas:
  * AutoSize para colunas de legenda com `Anchor = Left | Right`.
  * Percent para colunas de conteúdo, distribuição percentual por bom raciocínio, `Anchor = Top | Bottom | Left | Right`. 
    Nunca encaixe células, sempre ancere!
  * Evite modo de dimensionamento de coluna _Absolute_, a menos que para conteúdo de tamanho fixo inevitável (ícones, botões).
- Linhas:
  * AutoSize para linhas com caráter "linhas-únicas" (campos de entrada típicos, legendas, checkboxes).
  * Percent para TextBoxes multi-linha, áreas de renderização E preenchimento de distância para, ex., linha de botão inferior (OK|Cancel).
  * Evite modo de dimensionamento de linha _Absolute_ ainda mais.

- Margens importam: Defina `Margin` em controles (mín. padrão 3px). 
- Nota: `Padding` não tem efeito em células TLP.

### Padrões de Layout Comuns

#### TextBox de linha única (TLP de 2 colunas)
**Padrão de entrada de dados mais comum:**
- Coluna de legenda: Largura AutoSize
- Coluna de TextBox: 100% Largura Percent
- Legenda: `Anchor = Left | Right` (alinha verticalmente com TextBox)
- TextBox: `Dock = Fill`, defina `Margin` (ex., 3px todos os lados)

#### TextBox multi-linha ou Conteúdo Personalizado Maior - Opção A (TLP de 2 colunas)
- Legenda na mesma linha, `Anchor = Top | Left`
- TextBox: `Dock = Fill`, defina `Margin`
- Altura da linha: AutoSize ou Percent para dimensionar a célula (célula dimensiona o TextBox)

#### TextBox multi-linha ou Conteúdo Personalizado Maior - Opção B (TLP de 1 coluna, linhas separadas)
- Legenda em linha dedicada acima do TextBox
- Legenda: `Dock = Fill` ou `Anchor = Left`
- TextBox em próxima linha: `Dock = Fill`, defina `Margin`
- Linha de TextBox: AutoSize ou Percent para dimensionar a célula

**Crítico:** Para TextBox multi-linha, a célula TLP define o tamanho, não o conteúdo do TextBox.

### Dimensionamento de Contêiner (CRÍTICO - Previne Recorte)

**Para GroupBox/Panel dentro de células TLP:**
- DEVE definir `AutoSize = true` e `AutoSizeMode = GrowOnly`
- Deveria `Dock = Fill` em sua célula
- Linha TLP parente deveria ser AutoSize
- Conteúdo dentro de GroupBox/Panel deveria usar TLP aninhado ou FlowLayoutPanel

**Por quê:** Contêineres de altura fixa recortam conteúdo mesmo quando linha parente é AutoSize. O contêiner relata seu