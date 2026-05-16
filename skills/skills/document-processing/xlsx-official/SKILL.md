---
name: xlsx
description: "Use this skill sempre que um arquivo de planilha for a entrada ou saída principal. Isso significa qualquer tarefa em que o usuário queira: abrir, ler, editar ou corrigir um arquivo existente .xlsx, .xlsm, .csv ou .tsv (por exemplo, adicionar colunas, calcular fórmulas, formatar, criar gráficos, limpar dados desorganizados); criar uma nova planilha do zero ou a partir de outras fontes de dados; ou converter entre formatos de arquivo tabular. Dispare especialmente quando o usuário referencia um arquivo de planilha pelo nome ou caminho — até casualmente (como \"a xlsx nos meus downloads\") — e quer algo feito com ele ou produzido a partir dele. Também dispare para limpeza ou reestruturação de arquivos de dados tabulares desorganizados (linhas malformadas, cabeçalhos fora de lugar, dados inúteis) em planilhas adequadas. O entregável deve ser um arquivo de planilha. NÃO dispare quando o entregável principal for um documento Word, relatório HTML, script Python independente, pipeline de banco de dados ou integração Google Sheets API, mesmo que dados tabulares estejam envolvidos."
license: Proprietary. LICENSE.txt contém os termos completos
---

# Requisitos para Outputs

## Todos os arquivos Excel

### Fonte Profissional
- Use uma fonte consistente e profissional (por exemplo, Arial, Times New Roman) para todos os entregáveis, a menos que o usuário instrua o contrário

### Zero Erros de Fórmula
- Todo modelo Excel DEVE ser entregue com ZERO erros de fórmula (#REF!, #DIV/0!, #VALUE!, #N/A, #NAME?)

### Preservar Templates Existentes (ao atualizar templates)
- Estude e IGUALE EXATAMENTE o formato, estilo e convenções existentes ao modificar arquivos
- Nunca imponha formatação padronizada em arquivos com padrões estabelecidos
- As convenções do template existente SEMPRE sobrepõem estas diretrizes

## Modelos financeiros

### Padrões de Codificação por Cores
A menos que o usuário ou template existente especifique o contrário

#### Convenções de Cores Padrão da Indústria
- **Texto azul (RGB: 0,0,255)**: Inputs hardcoded e números que usuários mudarão para cenários
- **Texto preto (RGB: 0,0,0)**: TODAS as fórmulas e cálculos
- **Texto verde (RGB: 0,128,0)**: Links puxando de outras worksheets na mesma workbook
- **Texto vermelho (RGB: 255,0,0)**: Links externos para outros arquivos
- **Fundo amarelo (RGB: 255,255,0)**: Premissas-chave que precisam atenção ou células que precisam ser atualizadas

### Padrões de Formatação de Números

#### Regras de Formato Obrigatórias
- **Anos**: Formatar como strings de texto (por exemplo, "2024" não "2.024")
- **Moeda**: Usar formato $#,##0; SEMPRE especificar unidades nos cabeçalhos ("Receita ($mm)")
- **Zeros**: Usar formatação de números para deixar todos os zeros como "-", incluindo percentuais (por exemplo, "$#,##0;($#,##0);-")
- **Percentuais**: Padrão 0,0% (uma casa decimal)
- **Múltiplos**: Formatar como 0,0x para múltiplos de avaliação (EV/EBITDA, P/E)
- **Números negativos**: Usar parênteses (123) não menos -123

### Regras de Construção de Fórmulas

#### Posicionamento de Premissas
- Colocar TODAS as premissas (taxas de crescimento, margens, múltiplos, etc.) em células de premissas separadas
- Usar referências de célula em vez de valores hardcoded em fórmulas
- Exemplo: Use =B5*(1+$B$6) em vez de =B5*1,05

#### Prevenção de Erros de Fórmula
- Verificar se todas as referências de célula estão corretas
- Verificar erros de off-by-one em ranges
- Garantir fórmulas consistentes em todos os períodos de projeção
- Testar com casos extremos (valores zero, números negativos)
- Verificar se não há referências circulares não intencionais

#### Requisitos de Documentação para Hardcodes
- Comentário ou em células ao lado (se final da tabela). Formato: "Fonte: [Sistema/Documento], [Data], [Referência Específica], [URL se aplicável]"
- Exemplos:
  - "Fonte: 10-K da Empresa, FY2024, Página 45, Nota de Receita, [URL SEC EDGAR]"
  - "Fonte: 10-Q da Empresa, Q2 2025, Exhibit 99.1, [URL SEC EDGAR]"
  - "Fonte: Bloomberg Terminal, 15/08/2025, AAPL US Equity"
  - "Fonte: FactSet, 20/08/2025, Consensus Estimates Screen"

# Criação, edição e análise de XLSX

## Visão Geral

Um usuário pode pedir para criar, editar ou analisar o conteúdo de um arquivo .xlsx. Você tem diferentes ferramentas e workflows disponíveis para tarefas diferentes.

## Requisitos Importantes

**LibreOffice Obrigatório para Recalcular Fórmulas**: Você pode assumir que LibreOffice está instalado para recalcular valores de fórmulas usando o script `scripts/recalc.py`. O script configura automaticamente LibreOffice na primeira execução, incluindo em ambientes sandboxed onde unix sockets são restritos (tratado por `scripts/office/soffice.py`)

## Leitura e análise de dados

### Análise de dados com pandas
Para análise de dados, visualização e operações básicas, use **pandas** que fornece capacidades poderosas de manipulação de dados:

```python
import pandas as pd

# Ler Excel
df = pd.read_excel('file.xlsx')  # Padrão: primeira sheet
all_sheets = pd.read_excel('file.xlsx', sheet_name=None)  # Todas as sheets como dict

# Analisar
df.head()      # Visualizar dados
df.info()      # Info de colunas
df.describe()  # Estatísticas

# Escrever Excel
df.to_excel('output.xlsx', index=False)
```

## Workflows de Arquivo Excel

## CRÍTICO: Use Fórmulas, Não Valores Hardcoded

**Sempre use fórmulas Excel em vez de calcular valores em Python e hardcoding deles.** Isso garante que a planilha permaneça dinâmica e atualizável.

### ❌ ERRADO - Hardcoding de Valores Calculados
```python
# Ruim: Calculando em Python e hardcoding resultado
total = df['Sales'].sum()
sheet['B10'] = total  # Hardcodes 5000

# Ruim: Calculando taxa de crescimento em Python
growth = (df.iloc[-1]['Revenue'] - df.iloc[0]['Revenue']) / df.iloc[0]['Revenue']
sheet['C5'] = growth  # Hardcodes 0.15

# Ruim: Cálculo Python para média
avg = sum(values) / len(values)
sheet['D20'] = avg  # Hardcodes 42.5
```

### ✅ CORRETO - Usando Fórmulas Excel
```python
# Bom: Deixe Excel calcular a soma
sheet['B10'] = '=SUM(B2:B9)'

# Bom: Taxa de crescimento como fórmula Excel
sheet['C5'] = '=(C4-C2)/C2'

# Bom: Média usando função Excel
sheet['D20'] = '=AVERAGE(D2:D19)'
```

Isso se aplica a TODOS os cálculos — totais, percentuais, razões, diferenças, etc. A planilha deve ser capaz de recalcular quando os dados de origem mudam.

## Workflow Comum
1. **Escolha a ferramenta**: pandas para dados, openpyxl para fórmulas/formatação
2. **Criar/Carregar**: Criar nova workbook ou carregar arquivo existente
3. **Modificar**: Adicionar/editar dados, fórmulas e formatação
4. **Salvar**: Escrever em arquivo
5. **Recalcular fórmulas (OBRIGATÓRIO SE USAR FÓRMULAS)**: Use o script scripts/recalc.py
   ```bash
   python scripts/recalc.py output.xlsx
   ```
6. **Verificar e corrigir quaisquer erros**: 
   - O script retorna JSON com detalhes de erro
   - Se `status` é `errors_found`, verifique `error_summary` para tipos de erro e locais específicos
   - Corrija os erros identificados e recalcule novamente
   - Erros comuns a corrigir:
     - `#REF!`: Referências de célula inválidas
     - `#DIV/0!`: Divisão por zero
     - `#VALUE!`: Tipo de dado errado em fórmula
     - `#NAME?`: Nome de fórmula não reconhecido

### Criar novos arquivos Excel

```python
# Usando openpyxl para fórmulas e formatação
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment

wb = Workbook()
sheet = wb.active

# Adicionar dados
sheet['A1'] = 'Hello'
sheet['B1'] = 'World'
sheet.append(['Row', 'of', 'data'])

# Adicionar fórmula
sheet['B2'] = '=SUM(A1:A10)'

# Formatação
sheet['A1'].font = Font(bold=True, color='FF0000')
sheet['A1'].fill = PatternFill('solid', start_color='FFFF00')
sheet['A1'].alignment = Alignment(horizontal='center')

# Largura da coluna
sheet.column_dimensions['A'].width = 20

wb.save('output.xlsx')
```

### Editar arquivos Excel existentes

```python
# Usando openpyxl para preservar fórmulas e formatação
from openpyxl import load_workbook

# Carregar arquivo existente
wb = load_workbook('existing.xlsx')
sheet = wb.active  # ou wb['SheetName'] para sheet específica

# Trabalhar com múltiplas sheets
for sheet_name in wb.sheetnames:
    sheet = wb[sheet_name]
    print(f"Sheet: {sheet_name}")

# Modificar células
sheet['A1'] = 'New Value'
sheet.insert_rows(2)  # Inserir linha na posição 2
sheet.delete_cols(3)  # Deletar coluna 3

# Adicionar nova sheet
new_sheet = wb.create_sheet('NewSheet')
new_sheet['A1'] = 'Data'

wb.save('modified.xlsx')
```

## Recalcular fórmulas

Arquivos Excel criados ou modificados por openpyxl contêm fórmulas como strings mas não valores calculados. Use o script `scripts/recalc.py` fornecido para recalcular fórmulas:

```bash
python scripts/recalc.py <excel_file> [timeout_seconds]
```

Exemplo:
```bash
python scripts/recalc.py output.xlsx 30
```

O script:
- Configura automaticamente macro LibreOffice na primeira execução
- Recalcula todas as fórmulas em todas as sheets
- Escaneia TODAS as células procurando erros Excel (#REF!, #DIV/0!, etc.)
- Retorna JSON com locais e contagens detalhadas de erros
- Funciona em Linux e macOS

## Checklist de Verificação de Fórmula

Verificações rápidas para garantir que as fórmulas funcionem corretamente:

### Verificação Essencial
- [ ] **Testar 2-3 referências de amostra**: Verificar que puxam valores corretos antes de construir modelo completo
- [ ] **Mapeamento de coluna**: Confirmar que colunas Excel correspondem (por exemplo, coluna 64 = BL, não BK)
- [ ] **Offset de linha**: Lembrar que linhas Excel são 1-indexed (linha DataFrame 5 = linha Excel 6)

### Pitfalls Comuns
- [ ] **Tratamento de NaN**: Verificar valores nulos com `pd.notna()`
- [ ] **Colunas extremas-direita**: Dados FY geralmente em colunas 50+
- [ ] **Múltiplas correspondências**: Pesquisar todas as ocorrências, não apenas a primeira
- [ ] **Divisão por zero**: Verificar denominadores antes de usar `/` em fórmulas (#DIV/0!)
- [ ] **Referências erradas**: Verificar que todas as referências de célula apontam para células pretendidas (#REF!)
- [ ] **Referências entre sheets**: Usar formato correto (Sheet1!A1) para linkar sheets

### Estratégia de Teste de Fórmula
- [ ] **Começar pequeno**: Testar fórmulas em 2-3 células antes de aplicar amplamente
- [ ] **Verificar dependências**: Verificar que todas as células referenciadas em fórmulas existem
- [ ] **Testar casos extremos**: Incluir valores zero, negativos e muito grandes

### Interpretando Output de scripts/recalc.py
O script retorna JSON com detalhes de erro:
```json
{
  "status": "success",           // ou "errors_found"
  "total_errors": 0,              // Contagem total de erros
  "total_formulas": 42,           // Número de fórmulas no arquivo
  "error_summary": {              // Apenas presente se erros encontrados
    "#REF!": {
      "count": 2,
      "locations": ["Sheet1!B5", "Sheet1!C10"]
    }
  }
}
```

## Melhores Práticas

### Seleção de Biblioteca
- **pandas**: Melhor para análise de dados, operações em massa e exportação de dados simples
- **openpyxl**: Melhor para formatação complexa, fórmulas e recursos específicos do Excel

### Trabalhando com openpyxl
- Índices de célula são 1-based (row=1, column=1 se refere à célula A1)
- Use `data_only=True` para ler valores calculados: `load_workbook('file.xlsx', data_only=True)`
- **Aviso**: Se aberto com `data_only=True` e salvo, fórmulas são substituídas por valores e permanentemente perdidas
- Para arquivos grandes: Use `read_only=True` para leitura ou `write_only=True` para escrita
- Fórmulas são preservadas mas não avaliadas — use scripts/recalc.py para atualizar valores

### Trabalhando com pandas
- Especificar tipos de dados para evitar problemas de inferência: `pd.read_excel('file.xlsx', dtype={'id': str})`
- Para arquivos grandes, ler colunas específicas: `pd.read_excel('file.xlsx', usecols=['A', 'C', 'E'])`
- Manipular datas corretamente: `pd.read_excel('file.xlsx', parse_dates=['date_column'])`

## Diretrizes de Estilo de Código
**IMPORTANTE**: Ao gerar código Python para operações Excel:
- Escrever código Python mínimo e conciso sem comentários desnecessários
- Evitar nomes de variáveis verbosos e operações redundantes
- Evitar print statements desnecessários

**Para arquivos Excel em si**:
- Adicionar comentários a células com fórmulas complexas ou premissas importantes
- Documentar fontes de dados para valores hardcoded
- Incluir notas para cálculos-chave e seções de modelo