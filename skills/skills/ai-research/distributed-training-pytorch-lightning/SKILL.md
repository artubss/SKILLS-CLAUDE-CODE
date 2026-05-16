---
name: pytorch-lightning
description: Framework PyTorch de alto nível com classe Trainer, treinamento distribuído automático (DDP/FSDP/DeepSpeed), sistema de callbacks e boilerplate mínimo. Escala de laptop para supercomputador com o mesmo código. Use quando quiser loops de treinamento limpos com boas práticas integradas.
version: 1.0.0
author: Orchestra Research
license: MIT
tags: [PyTorch Lightning, Training Framework, Distributed Training, DDP, FSDP, DeepSpeed, High-Level API, Callbacks, Best Practices, Scalable]
dependencies: [lightning, torch, transformers]
---

# PyTorch Lightning - Framework de Treinamento de Alto Nível

## Início rápido

PyTorch Lightning organiza código PyTorch para eliminar boilerplate mantendo flexibilidade.

**Instalação**:
```bash
pip install lightning
```

**Converter PyTorch para Lightning** (3 passos):

```python
import lightning as L
import torch
from torch import nn
from torch.utils.data import DataLoader, Dataset

# Passo 1: Defina LightningModule (organize seu código PyTorch)
class LitModel(L.LightningModule):
    def __init__(self, hidden_size=128):
        super().__init__()
        self.model = nn.Sequential(
            nn.Linear(28 * 28, hidden_size),
            nn.ReLU(),
            nn.Linear(hidden_size, 10)
        )

    def training_step(self, batch, batch_idx):
        x, y = batch
        y_hat = self.model(x)
        loss = nn.functional.cross_entropy(y_hat, y)
        self.log('train_loss', loss)  # Auto-logged para TensorBoard
        return loss

    def configure_optimizers(self):
        return torch.optim.Adam(self.parameters(), lr=1e-3)

# Passo 2: Crie dados
train_loader = DataLoader(train_dataset, batch_size=32)

# Passo 3: Treine com Trainer (cuida de tudo mais!)
trainer = L.Trainer(max_epochs=10, accelerator='gpu', devices=2)
model = LitModel()
trainer.fit(model, train_loader)
```

**Pronto!** Trainer cuida de:
- Switching GPU/TPU/CPU
- Treinamento distribuído (DDP, FSDP, DeepSpeed)
- Precisão mista (FP16, BF16)
- Acumulação de gradientes
- Checkpointing
- Logging
- Barras de progresso

## Fluxos de trabalho comuns

### Fluxo 1: De PyTorch para Lightning

**Código PyTorch original**:
```python
model = MyModel()
optimizer = torch.optim.Adam(model.parameters())
model.to('cuda')

for epoch in range(max_epochs):
    for batch in train_loader:
        batch = batch.to('cuda')
        optimizer.zero_grad()
        loss = model(batch)
        loss.backward()
        optimizer.step()
```

**Versão Lightning**:
```python
class LitModel(L.LightningModule):
    def __init__(self):
        super().__init__()
        self.model = MyModel()

    def training_step(self, batch, batch_idx):
        loss = self.model(batch)  # Sem necessidade de .to('cuda')!
        return loss

    def configure_optimizers(self):
        return torch.optim.Adam(self.parameters())

# Treine
trainer = L.Trainer(max_epochs=10, accelerator='gpu')
trainer.fit(LitModel(), train_loader)
```

**Benefícios**: 40+ linhas → 15 linhas, sem gerenciamento de dispositivo, distribuído automático

### Fluxo 2: Validação e teste

```python
class LitModel(L.LightningModule):
    def __init__(self):
        super().__init__()
        self.model = MyModel()

    def training_step(self, batch, batch_idx):
        x, y = batch
        y_hat = self.model(x)
        loss = nn.functional.cross_entropy(y_hat, y)
        self.log('train_loss', loss)
        return loss

    def validation_step(self, batch, batch_idx):
        x, y = batch
        y_hat = self.model(x)
        val_loss = nn.functional.cross_entropy(y_hat, y)
        acc = (y_hat.argmax(dim=1) == y).float().mean()
        self.log('val_loss', val_loss)
        self.log('val_acc', acc)

    def test_step(self, batch, batch_idx):
        x, y = batch
        y_hat = self.model(x)
        test_loss = nn.functional.cross_entropy(y_hat, y)
        self.log('test_loss', test_loss)

    def configure_optimizers(self):
        return torch.optim.Adam(self.parameters(), lr=1e-3)

# Treine com validação
trainer = L.Trainer(max_epochs=10)
trainer.fit(model, train_loader, val_loader)

# Teste
trainer.test(model, test_loader)
```

**Funcionalidades automáticas**:
- Validação executa a cada epoch por padrão
- Métricas logadas para TensorBoard
- Checkpointing do melhor modelo baseado em val_loss

### Fluxo 3: Treinamento distribuído (DDP)

```python
# Mesmo código que single GPU!
model = LitModel()

# 8 GPUs com DDP (automático!)
trainer = L.Trainer(
    accelerator='gpu',
    devices=8,
    strategy='ddp'  # Ou 'fsdp', 'deepspeed'
)

trainer.fit(model, train_loader)
```

**Launch**:
```bash
# Comando único, Lightning cuida do resto
python train.py
```

**Sem mudanças necessárias**:
- Distribuição automática de dados
- Sincronização de gradientes
- Suporte multi-node (apenas defina `num_nodes=2`)

### Fluxo 4: Callbacks para monitoramento

```python
from lightning.pytorch.callbacks import ModelCheckpoint, EarlyStopping, LearningRateMonitor

# Crie callbacks
checkpoint = ModelCheckpoint(
    monitor='val_loss',
    mode='min',
    save_top_k=3,
    filename='model-{epoch:02d}-{val_loss:.2f}'
)

early_stop = EarlyStopping(
    monitor='val_loss',
    patience=5,
    mode='min'
)

lr_monitor = LearningRateMonitor(logging_interval='epoch')

# Adicione ao Trainer
trainer = L.Trainer(
    max_epochs=100,
    callbacks=[checkpoint, early_stop, lr_monitor]
)

trainer.fit(model, train_loader, val_loader)
```

**Resultado**:
- Salva automaticamente os melhores 3 modelos
- Para cedo se nenhuma melhoria por 5 epochs
- Loga taxa de aprendizado para TensorBoard

### Fluxo 5: Agendamento de taxa de aprendizado

```python
class LitModel(L.LightningModule):
    # ... (training_step, etc.)

    def configure_optimizers(self):
        optimizer = torch.optim.Adam(self.parameters(), lr=1e-3)

        # Cosine annealing
        scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(
            optimizer,
            T_max=100,
            eta_min=1e-5
        )

        return {
            'optimizer': optimizer,
            'lr_scheduler': {
                'scheduler': scheduler,
                'interval': 'epoch',  # Atualizar por epoch
                'frequency': 1
            }
        }

# Taxa de aprendizado auto-logada!
trainer = L.Trainer(max_epochs=100)
trainer.fit(model, train_loader)
```

## Quando usar vs alternativas

**Use PyTorch Lightning quando**:
- Quiser código limpo e organizado
- Precisar de loops de treinamento prontos para produção
- Alternar entre single GPU, multi-GPU, TPU
- Quiser callbacks e logging integrados
- Colaboração em equipe (estrutura padronizada)

**Principais vantagens**:
- **Organizado**: Separa código de pesquisa da engenharia
- **Automático**: DDP, FSDP, DeepSpeed com 1 linha
- **Callbacks**: Extensões de treinamento modulares
- **Reproduzível**: Menos boilerplate = menos bugs
- **Testado**: 1M+ downloads/mês, battle-tested

**Use alternativas em vez disso**:
- **Accelerate**: Mudanças mínimas em código existente, mais flexibilidade
- **Ray Train**: Orquestração multi-node, tuning de hiperparâmetros
- **PyTorch bruto**: Controle máximo, fins educacionais
- **Keras**: Ecossistema TensorFlow

## Problemas comuns

**Problema: Loss não diminui**

Verifique setup de dados e modelo:
```python
# Adicione ao training_step
def training_step(self, batch, batch_idx):
    if batch_idx == 0:
        print(f"Forma do batch: {batch[0].shape}")
        print(f"Labels: {batch[1]}")
    loss = ...
    return loss
```

**Problema: Memória insuficiente**

Reduza tamanho do batch ou use acumulação de gradientes:
```python
trainer = L.Trainer(
    accumulate_grad_batches=4,  # Batch efetivo = batch_size × 4
    precision='bf16'  # Ou 'fp16', reduz memória 50%
)
```

**Problema: Validação não está sendo executada**

Garanta que passa val_loader:
```python
# ERRADO
trainer.fit(model, train_loader)

# CORRETO
trainer.fit(model, train_loader, val_loader)
```

**Problema: DDP gera múltiplos processos inesperadamente**

Lightning detecta GPUs automaticamente. Defina dispositivos explicitamente:
```python
# Teste em CPU primeiro
trainer = L.Trainer(accelerator='cpu', devices=1)

# Depois GPU
trainer = L.Trainer(accelerator='gpu', devices=1)
```

## Tópicos avançados

**Callbacks**: Veja [references/callbacks.md](references/callbacks.md) para EarlyStopping, ModelCheckpoint, callbacks customizados e callback hooks.

**Estratégias distribuídas**: Veja [references/distributed.md](references/distributed.md) para DDP, FSDP, integração DeepSpeed ZeRO, setup multi-node.

**Tuning de hiperparâmetros**: Veja [references/hyperparameter-tuning.md](references/hyperparameter-tuning.md) para integração com Optuna, Ray Tune e WandB sweeps.

## Requisitos de hardware

- **CPU**: Funciona (bom para debugging)
- **Single GPU**: Funciona
- **Multi-GPU**: DDP (padrão), FSDP ou DeepSpeed
- **Multi-node**: DDP, FSDP, DeepSpeed
- **TPU**: Suportado (8 núcleos)
- **Apple MPS**: Suportado

**Opções de precisão**:
- FP32 (padrão)
- FP16 (V100, GPUs antigas)
- BF16 (A100/H100, recomendado)
- FP8 (H100)

## Recursos

- Docs: https://lightning.ai/docs/pytorch/stable/
- GitHub: https://github.com/Lightning-AI/pytorch-lightning ⭐ 29,000+
- Versão: 2.5.5+
- Exemplos: https://github.com/Lightning-AI/pytorch-lightning/tree/master/examples
- Discord: https://discord.gg/lightning-ai
- Usado por: Kaggle winners, research labs, production teams