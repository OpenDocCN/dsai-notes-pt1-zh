# 复现GPT-2 (124M版本)：1：从零开始复现GPT-2

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_1.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_3.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_5.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_7.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_9.png)

在本节课中，我们将要学习如何从零开始复现OpenAI发布的GPT-2模型，具体是拥有1.24亿参数的版本。我们将从加载预训练模型开始，逐步实现自己的GPT-2架构，并最终训练一个性能达到甚至超越原版模型的版本。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_11.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_13.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_15.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_16.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_18.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_20.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_22.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_24.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_25.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_27.png)

## 概述与目标

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_29.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_31.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_33.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_35.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_37.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_39.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_41.png)

上一节我们介绍了GPT-2模型系列，本节中我们来看看如何具体复现124M参数版本。我们的最终目标是训练一个模型，使其在验证损失和下游任务（如HellaSwag评估）上的表现达到或超过OpenAI发布的原始模型。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_43.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_45.png)

## 加载预训练模型作为目标

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_47.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_49.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_51.png)

首先，我们需要一个明确的目标。让我们加载OpenAI发布的GPT-2 124M模型，并尝试用它生成一些文本。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_53.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_55.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_57.png)

由于原始代码库使用TensorFlow，而我们将使用PyTorch，因此我们借助Hugging Face Transformers库来加载模型。这简化了从TensorFlow到PyTorch权重的转换过程。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_59.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_61.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_63.png)

以下是加载模型并检查其参数的代码：

```python
from transformers import GPT2LMHeadModel

model_hf = GPT2LMHeadModel.from_pretrained("gpt2")
state_dict_hf = model_hf.state_dict()

for k, v in state_dict_hf.items():
    print(k, v.shape)
```

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_65.png)

通过上述代码，我们可以看到模型的所有参数及其形状。例如，词嵌入矩阵 `wte.weight` 的形状是 `(50257, 768)`，其中50257是GPT-2的词表大小，768是嵌入维度。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_67.png)

## 实现我们自己的GPT-2模型

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_69.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_71.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_73.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_75.png)

为了完全理解模型内部机制，我们将从头实现自己的GPT-2类。我们的第一个任务是确保能够将OpenAI的权重加载到我们实现的类中，这能验证我们实现的正确性。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_77.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_79.png)

### 模型骨架

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_81.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_82.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_84.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_86.png)

GPT-2是一个仅包含解码器的Transformer。与原始Transformer论文相比，主要区别在于层归一化的位置有所调整，并且在最终自注意力块后添加了一个额外的层归一化。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_88.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_90.png)

以下是GPT-2模型的主要骨架：

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_92.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_94.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_96.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_98.png)

```python
import torch
import torch.nn as nn

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_100.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_101.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_102.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_104.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_106.png)

class GPT(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.config = config

        self.transformer = nn.ModuleDict(dict(
            wte = nn.Embedding(config.vocab_size, config.n_embd),
            wpe = nn.Embedding(config.block_size, config.n_embd),
            h = nn.ModuleList([Block(config) for _ in range(config.n_layer)]),
            ln_f = nn.LayerNorm(config.n_embd),
        ))
        self.lm_head = nn.Linear(config.n_embd, config.vocab_size, bias=False)

    def forward(self, idx, targets=None):
        # 前向传播逻辑
        b, t = idx.size()
        pos = torch.arange(0, t, dtype=torch.long, device=idx.device).unsqueeze(0)
        tok_emb = self.transformer.wte(idx)
        pos_emb = self.transformer.wpe(pos)
        x = tok_emb + pos_emb
        for block in self.transformer.h:
            x = block(x)
        x = self.transformer.ln_f(x)
        logits = self.lm_head(x)

        loss = None
        if targets is not None:
            loss = F.cross_entropy(logits.view(-1, logits.size(-1)), targets.view(-1))
        return logits, loss
```

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_108.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_110.png)

### Transformer块

每个Transformer块包含一个多头自注意力机制和一个前馈网络（MLP），并采用了“预归一化”结构。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_112.png)

以下是块的实现：

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_114.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_115.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_117.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_119.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_120.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_122.png)

```python
class Block(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.ln_1 = nn.LayerNorm(config.n_embd)
        self.attn = CausalSelfAttention(config)
        self.ln_2 = nn.LayerNorm(config.n_embd)
        self.mlp = MLP(config)

    def forward(self, x):
        x = x + self.attn(self.ln_1(x))
        x = x + self.mlp(self.ln_2(x))
        return x
```

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_124.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_125.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_126.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_128.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_129.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_131.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_133.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_134.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_136.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_138.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_140.png)

### 多头自注意力

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_142.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_144.png)

我们实现了一个高效的多头自注意力模块，它使用矩阵操作来并行处理所有注意力头。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_146.png)

```python
class CausalSelfAttention(nn.Module):
    def __init__(self, config):
        super().__init__()
        assert config.n_embd % config.n_head == 0
        self.c_attn = nn.Linear(config.n_embd, 3 * config.n_embd)
        self.c_proj = nn.Linear(config.n_embd, config.n_embd)
        self.n_head = config.n_head
        self.n_embd = config.n_embd
        self.register_buffer("bias", torch.tril(torch.ones(config.block_size, config.block_size))
                                    .view(1, 1, config.block_size, config.block_size))

    def forward(self, x):
        B, T, C = x.size()
        qkv = self.c_attn(x)
        q, k, v = qkv.split(self.n_embd, dim=2)
        k = k.view(B, T, self.n_head, C // self.n_head).transpose(1, 2)
        q = q.view(B, T, self.n_head, C // self.n_head).transpose(1, 2)
        v = v.view(B, T, self.n_head, C // self.n_head).transpose(1, 2)

        att = (q @ k.transpose(-2, -1)) * (1.0 / math.sqrt(k.size(-1)))
        att = att.masked_fill(self.bias[:,:,:T,:T] == 0, float('-inf'))
        att = F.softmax(att, dim=-1)
        y = att @ v
        y = y.transpose(1, 2).contiguous().view(B, T, C)
        y = self.c_proj(y)
        return y
```

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_148.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_149.png)

### 前馈网络（MLP）

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_151.png)

MLP包含两个线性层，中间使用GELU激活函数。GPT-2使用的是GELU的近似版本。

```python
class MLP(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.c_fc = nn.Linear(config.n_embd, 4 * config.n_embd)
        self.c_proj = nn.Linear(4 * config.n_embd, config.n_embd)
        self.gelu = nn.GELU(approximate='tanh')

    def forward(self, x):
        x = self.c_fc(x)
        x = self.gelu(x)
        x = self.c_proj(x)
        return x
```

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_153.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_155.png)

## 权重绑定

GPT-2采用了权重绑定策略，即词嵌入层的权重与语言模型头部的线性层权重共享。这减少了参数量并引入了有益的归纳偏置。

在我们的实现中，可以通过以下方式实现权重绑定：

```python
model.transformer.wte.weight = model.lm_head.weight
```

## 模型初始化

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_157.png)

正确的初始化对训练稳定性至关重要。我们遵循GPT-2源代码中的初始化方案。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_159.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_161.png)

以下是初始化权重的函数：

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_163.png)

```python
def _init_weights(self, module):
    if isinstance(module, nn.Linear):
        std = 0.02
        if hasattr(module, 'GPT_SCALE_INIT'):
            std *= (2 * self.config.n_layer) ** -0.5
        torch.nn.init.normal_(module.weight, mean=0.0, std=std)
        if module.bias is not None:
            torch.nn.init.zeros_(module.bias)
    elif isinstance(module, nn.Embedding):
        torch.nn.init.normal_(module.weight, mean=0.0, std=0.02)
```

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_165.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_167.png)

## 数据加载与训练循环

为了训练模型，我们需要一个数据加载器。我们从简单的Tiny Shakespeare数据集开始，然后过渡到更大的FineWeb-Edu数据集。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_169.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_171.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_173.png)

以下是一个简单的数据加载器实现：

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_175.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_176.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_178.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_180.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_182.png)

```python
class DataLoaderLite:
    def __init__(self, B, T, split):
        self.B = B
        self.T = T
        with open('input.txt', 'r') as f:
            text = f.read()
        enc = tiktoken.get_encoding('gpt2')
        tokens = enc.encode(text)
        self.tokens = torch.tensor(tokens)
        self.current_position = 0

    def next_batch(self):
        B, T = self.B, self.T
        buf = self.tokens[self.current_position: self.current_position + B*T + 1]
        x = buf[:-1].view(B, T)
        y = buf[1:].view(B, T)
        self.current_position += B * T
        if self.current_position + (B*T + 1) > len(self.tokens):
            self.current_position = 0
        return x, y
```

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_184.png)

训练循环包括前向传播、损失计算、反向传播和优化器步骤。我们使用AdamW优化器，并采用梯度裁剪和学习率调度。

## 性能优化

为了充分利用硬件，我们实施了一系列性能优化：

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_186.png)

1.  **混合精度训练**：使用`torch.autocast`和BF16精度，以利用Tensor Cores并减少内存带宽。
2.  **Torch Compile**：使用`torch.compile`编译模型，通过内核融合和减少Python开销来加速。
3.  **Flash Attention**：用高效的Flash Attention实现替换标准的注意力计算，减少内存访问。
4.  **美化数字**：将词表大小等参数调整为2的幂次，以更好地匹配CUDA内核的块大小。
5.  **分布式数据并行**：使用`DistributedDataParallel`在多个GPU上并行训练。

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_188.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_190.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_192.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_194.png)

## 评估与结果

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_196.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_198.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_200.png)

我们定期在验证集和HellaSwag基准上评估模型。经过约100亿token的训练（约2小时），我们的模型在HellaSwag上的准确率超过了原始的GPT-2 124M模型。经过400亿token的训练（约8小时），我们接近了GPT-3 124M模型的性能。

## 总结

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_202.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_204.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_206.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_208.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_209.png)

![](img/c3f9a3f0d6940a35d7ed355ae2c72101_211.png)

本节课中我们一起学习了如何从零开始复现GPT-2 124M模型。我们涵盖了从加载预训练权重、实现模型架构、设置数据管道、配置优化器，到实施高级性能优化和分布式训练的完整流程。通过这个过程，我们不仅复现了原模型，还在更少的数据量上取得了可比的性能，这得益于更高质量的数据集和优化的训练设置。