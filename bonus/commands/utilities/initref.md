# Guia de Implementação: Referência do Projeto

Vou ajudá-lo a construir uma referência de implementação para o projeto. Vou começar examinando a estrutura do projeto e criando documentação organizada.

## Passo 1: Explorar a Estrutura do Projeto

Primeiro, vou entender a estrutura:

```bash
find . -type f -name "*.md" -o -name "*.json" -o -name "*.ts" -o -name "*.js" | head -20
```

Agora vou usar a ferramenta de sumarização para analisar os arquivos principais sem consumir muitos tokens. Você pode fornecer:

1. **Resumos dos arquivos principais** (use a ferramenta de sumarização para):
   - `package.json` — dependências e scripts
   - Arquivos de configuração (`.config.ts`, `.config.js`, etc.)
   - Diretórios principais (`src/`, `lib/`, etc.)

2. **Arquivos que vou ler diretamente** (importantes para documentação):
   - `README.md`
   - `CLAUDE.md` ou arquivo de contexto existente
   - Arquivos de entrada principal (main, index)

## Passo 2: Estrutura de Referência Sugerida

Vou criar estes arquivos em `/ref`:

```
/ref
├── ARCHITECTURE.md          # Visão geral da arquitetura
├── API_REFERENCE.md         # APIs e funções principais
├── CONFIGURATION.md         # Guia de configuração
├── DEPENDENCIES.md          # Dependências e versões
├── FILE_STRUCTURE.md        # Mapa do projeto
└── SETUP_GUIDE.md          # Guia de setup
```

## O que preciso de você:

Para começar, por favor forneça:

1. **O conteúdo de `package.json`** ou resultado de `cat package.json`
2. **O conteúdo de `CLAUDE.md`** (arquivo de contexto atual)
3. **O resultado de `summarize`** para arquivos principais (src/, config files)
4. **A estrutura de diretórios** (saída de `tree -L 3` ou `find . -type d | grep -v node_modules | head -30`)

Com essas informações, vou:
- ✅ Criar documentação estruturada em `/ref`
- ✅ Atualizar `CLAUDE.md` com ponteiros para referências
- ✅ Usar sumarizações para evitar consumo excessivo de tokens
- ✅ Preservar código e configurações intactos
- ✅ Manter formato markdown profissional

**Você pode começar compartilhando os itens acima?**