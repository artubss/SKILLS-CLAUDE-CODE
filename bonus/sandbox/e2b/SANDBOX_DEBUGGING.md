# Guia de Debugging do E2B Sandbox

## 🔍 Ferramentas de Monitoramento Disponíveis

### 1. Launcher Principal com Logging Melhorado
**Arquivo**: `e2b-launcher.py`
- Logging detalhado de cada etapa
- Verificação de instalação do Claude Code
- Monitoramento de permissões e ambiente
- Timeouts estendidos para operações longas
- Download automático de arquivos gerados

### 2. Monitor de Sandbox em Tempo Real
**Arquivo**: `e2b-monitor.py`  
- Monitoramento de recursos do sistema
- Rastreamento do file system em tempo real
- Análise de performance e uso de memória
- Logging com timestamps detalhados

### 3. Simulador Demo
Para testes sem API keys válidas, crie um arquivo demo que simule o fluxo completo.

## 🚨 Troubleshooting Comum

### Problema: "Sandbox timeout"
**Sintomas**:
```
❌ Error: The sandbox was not found: This error is likely due to sandbox timeout
```

**Soluções**:
1. **Aumentar timeout do sandbox**:
   ```python
   sbx = Sandbox.create(timeout=600)  # 10 minutos
   sbx.set_timeout(900)  # Estender para 15 minutos
   ```

2. **Usar o monitor para ver o que consome tempo**:
   ```bash
   python e2b-monitor.py "Your prompt here" "" your_e2b_key your_anthropic_key
   ```

### Problema: "Claude not found"
**Sintomas**:
```
❌ Claude not found, checking PATH...
```

**Passos de Debugging**:
1. **Verificar template correto**:
   ```python
   template="anthropic-claude-code"  # Deve ser exatamente este
   ```

2. **Verificar instalação no sandbox**:
   ```bash
   # O launcher executa automaticamente:
   which claude
   claude --version
   echo $PATH
   ```

### Problema: "Permission denied"
**Sintomas**:
```
❌ Write permission issue
```

**Soluções**:
1. **Verificar diretório de trabalho**:
   ```bash
   pwd
   whoami
   ls -la
   ```

2. **Mudar para diretório com permissões**:
   ```python
   sbx.commands.run("cd /home/user && mkdir workspace && cd workspace")
   ```

### Problema: API Key Issues
**Sintomas**:
```
❌ Error: 401: Invalid API key
```

**Debugging**:
1. **Verificar formato de API key**:
   - E2B keys: formato específico de E2B
   - Anthropic keys: começam com "sk-ant-"

2. **Verificar permissões**:
   - Verificar que a key tenha permissões de sandbox
   - Verificar quota/limites da conta

## 📊 Usando o Monitor para Debugging

### Comando Básico:
```bash
python e2b-monitor.py "Create a React app" "" your_e2b_key your_anthropic_key
```

### Output do Monitor:
```
[14:32:15] INFO: 🚀 Starting enhanced E2B sandbox with monitoring
[14:32:16] INFO: ✅ Sandbox created: abc123xyz
[14:32:17] INFO: 🔍 System resources check
[14:32:17] INFO: Memory usage:
[14:32:17] INFO:                total        used        free
[14:32:17] INFO:   Mem:           2.0Gi       512Mi       1.5Gi
[14:32:18] INFO: 📁 Initial file system state
[14:32:18] INFO: Current directory: /home/user
[14:32:19] INFO: 🤖 Executing Claude Code with monitoring
[14:32:19] INFO: Starting monitored execution: echo 'Create a React app'...
[14:32:22] INFO: Command completed in 3.45 seconds
[14:32:22] INFO: Exit code: 0
[14:32:22] INFO: STDOUT length: 2847 characters
```

## 🎯 Casos de Uso Específicos

### 1. **Debugging Timeouts**
```bash
# Usar o monitor para ver exatamente onde trava
python e2b-monitor.py "Complex prompt that times out"
```

### 2. **Verificar Geração de Arquivos**
O launcher baixa automaticamente arquivos gerados:
```
💾 DOWNLOADING FILES TO LOCAL MACHINE:
✅ Downloaded: ./index.html → ./e2b-output/index.html
✅ Downloaded: ./styles.css → ./e2b-output/styles.css

📁 All files downloaded to: /path/to/project/e2b-output
```

### 3. **Monitoramento de Performance**
```
[14:33:20] INFO: Top processes:
[14:33:20] INFO:   USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
[14:33:20] INFO:   user      1234  5.2  2.1  98765 43210 pts/0    S+   14:32   0:01 claude
```

## 🛠 Configuração Avançada

### Variáveis de Ambiente Úteis:
```bash
export E2B_DEBUG=1                    # Debug mode
export ANTHROPIC_API_KEY=your_key     # Claude API key  
export E2B_API_KEY=your_key          # E2B API key
```

### Configuração de Timeout Personalizada:
```python
# Para operações muito longas (ex: compilação completa)
sbx = Sandbox.create(timeout=1800)  # 30 minutos
sbx.set_timeout(3600)               # 1 hora máximo
```

## 📋 Checklist de Debugging

### Antes de Reportar um Issue:
- [ ] API keys válidas e com permissões corretas
- [ ] Template correto: "anthropic-claude-code"
- [ ] Timeout suficiente para a operação
- [ ] Executar com o monitor para logs detalhados
- [ ] Verificar que Claude Code está instalado no sandbox
- [ ] Revisar permissões de escrita no diretório
- [ ] Comprovar memória/recursos disponíveis

### Informações a Incluir em Reports:
- Output completo do launcher ou monitor
- Sandbox ID se estiver disponível
- Prompt exato que causa o problema
- Componentes instalados (se aplicável)
- Tempo de execução antes da falha

## 🚀 Funcionalidades do Sistema

### Download Automático de Arquivos
O launcher baixa automaticamente todos os arquivos gerados:
- HTML, CSS, JS, TS, TSX, Python, JSON, Markdown
- Salvos no diretório local `./e2b-output/`
- Exclui arquivos internos do Claude Code
- Preserva nomes de arquivo originais

### Logging Detalhado
- Verificação de instalação do Claude Code
- Monitoramento de permissões e ambiente do sandbox
- Rastreamento de exit codes e tamanho de output
- Timestamps para análise de performance

### Timeouts Inteligentes
- 10 minutos timeout inicial para criação
- 15 minutos total estendido automaticamente
- 5 minutos timeout para execução do Claude Code
- Timeouts curtos para verificações (5-10 segundos)

---

**Com essas ferramentas você pode monitorar exatamente o que está acontecendo dentro do sandbox E2B e debugar qualquer problema que surgir.**