# 课程 P10：从零开始重现 GPT-2 (124M) 🚀

![](img/fffe638cf84a8d1e020ce63d0efeee6e_1.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_3.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_5.png)

在本节课中，我们将学习如何从零开始构建并训练一个 GPT-2 (124M) 模型。我们将涵盖从加载预训练权重、理解模型架构、实现核心模块，到进行高效训练和评估的完整流程。通过本教程，你将能够亲手重现一个功能完整的语言模型。

## 概述

![](img/fffe638cf84a8d1e020ce63d0efeee6e_7.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_9.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_11.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_12.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_14.png)

GPT-2 是 OpenAI 在 2019 年发布的开创性语言模型。它属于一个包含多个尺寸的“迷你系列”，其中 124M 参数版本是一个理想的起点。本节课的目标是理解其架构，并用 PyTorch 从头实现它，最终训练一个性能接近甚至超越原版的开源模型。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_16.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_18.png)

---

![](img/fffe638cf84a8d1e020ce63d0efeee6e_20.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_22.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_24.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_26.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_28.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_30.png)

## 1. 目标：加载并理解预训练的 GPT-2 模型

![](img/fffe638cf84a8d1e020ce63d0efeee6e_32.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_34.png)

首先，让我们明确目标。我们将使用 Hugging Face `transformers` 库加载 OpenAI 发布的 GPT-2 (124M) 模型，并检查其权重结构。这为我们后续自己构建模型提供了参考基准。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_36.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_38.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_39.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_41.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_43.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_45.png)

以下是加载模型并检查其权重的步骤：

![](img/fffe638cf84a8d1e020ce63d0efeee6e_47.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_49.png)

1.  **导入模型**：从 `transformers` 库导入 `GPT2LMHeadModel`。
2.  **加载预训练权重**：使用 `from_pretrained(‘gpt2’)` 加载 124M 参数版本（`gpt2` 即对应此版本）。
3.  **获取状态字典**：模型的 `state_dict()` 包含了所有权重张量及其形状。
4.  **分析关键参数**：
    *   **标记嵌入** (`transformer.wte.weight`)：形状为 `(50257, 768)`。GPT-2 的词表大小为 50257，每个标记用 768 维向量表示。
    *   **位置嵌入** (`transformer.wpe.weight`)：形状为 `(1024, 768)`。模型支持的最大序列长度为 1024，每个位置有独立的嵌入向量。
    *   **Transformer 层权重**：包括注意力层、前馈网络层等的权重和偏置。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_51.png)

通过分析这些权重，我们可以确认模型结构，并为后续实现提供准确的参数形状。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_53.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_55.png)

---

![](img/fffe638cf84a8d1e020ce63d0efeee6e_57.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_59.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_61.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_62.png)

## 2. 构建 GPT-2 模型架构 🏗️

上一节我们查看了目标模型的结构，本节中我们来看看如何用 PyTorch 模块搭建我们自己的 GPT-2。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_64.png)

GPT-2 是一个仅解码器的 Transformer 模型。与原始 Transformer 相比，它移除了编码器部分，并调整了层归一化的位置（采用“预归一化”）。

### 2.1 模型整体框架

![](img/fffe638cf84a8d1e020ce63d0efeee6e_66.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_68.png)

我们首先定义 `GPT` 类作为模型容器。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_70.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_72.png)

```python
import torch
import torch.nn as nn
from torch.nn import functional as F

class GPT(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.config = config

        # 标记嵌入和位置嵌入
        self.transformer = nn.ModuleDict(dict(
            wte = nn.Embedding(config.vocab_size, config.n_embd),
            wpe = nn.Embedding(config.block_size, config.n_embd),
            h = nn.ModuleList([Block(config) for _ in range(config.n_layer)]),
            ln_f = nn.LayerNorm(config.n_embd),
        ))
        # 语言模型头（输出层），注意：通常与标记嵌入权重绑定
        self.lm_head = nn.Linear(config.n_embd, config.vocab_size, bias=False)

    def forward(self, idx, targets=None):
        # idx: (B, T) 批大小 x 序列长度
        B, T = idx.shape
        device = idx.device

        # 生成位置索引
        pos = torch.arange(0, T, dtype=torch.long, device=device).unsqueeze(0) # (1, T)

        # 前向传播
        tok_emb = self.transformer.wte(idx) # (B, T, n_embd)
        pos_emb = self.transformer.wpe(pos) # (1, T, n_embd)
        x = tok_emb + pos_emb # (B, T, n_embd)

        # 通过所有 Transformer 块
        for block in self.transformer.h:
            x = block(x)
        x = self.transformer.ln_f(x) # 最终的层归一化

        # 计算 logits
        logits = self.lm_head(x) # (B, T, vocab_size)

        # 计算损失（如果提供了目标）
        loss = None
        if targets is not None:
            B, T, C = logits.shape
            logits = logits.view(B*T, C)
            targets = targets.view(B*T)
            loss = F.cross_entropy(logits, targets)

        return logits, loss
```

![](img/fffe638cf84a8d1e020ce63d0efeee6e_74.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_76.png)

### 2.2 实现 Transformer 块

![](img/fffe638cf84a8d1e020ce63d0efeee6e_78.png)

每个 `Block` 包含一个带掩码的多头自注意力机制和一个前馈网络（MLP），均采用预归一化。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_80.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_82.png)

```python
class Block(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.ln_1 = nn.LayerNorm(config.n_embd)
        self.attn = CausalSelfAttention(config) # 我们将实现这个
        self.ln_2 = nn.LayerNorm(config.n_embd)
        self.mlp = MLP(config)

    def forward(self, x):
        # 预归一化 -> 注意力 -> 残差连接
        x = x + self.attn(self.ln_1(x))
        # 预归一化 -> MLP -> 残差连接
        x = x + self.mlp(self.ln_2(x))
        return x
```

![](img/fffe638cf84a8d1e020ce63d0efeee6e_84.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_86.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_88.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_90.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_92.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_94.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_96.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_98.png)

### 2.3 实现因果自注意力

![](img/fffe638cf84a8d1e020ce63d0efeee6e_100.png)

这是 Transformer 的核心，允许每个标记关注其左侧的所有标记。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_102.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_104.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_106.png)

```python
class CausalSelfAttention(nn.Module):
    def __init__(self, config):
        super().__init__()
        assert config.n_embd % config.n_head == 0
        # 键、查询、值投影
        self.c_attn = nn.Linear(config.n_embd, 3 * config.n_embd)
        # 输出投影
        self.c_proj = nn.Linear(config.n_embd, config.n_embd)
        # 正则化
        self.attn_dropout = nn.Dropout(config.dropout)
        self.resid_dropout = nn.Dropout(config.dropout)
        self.n_head = config.n_head
        self.n_embd = config.n_embd
        self.dropout = config.dropout
        # 使用 PyTorch 的优化注意力实现（Flash Attention）
        self.flash = hasattr(torch.nn.functional, ‘scaled_dot_product_attention’)

    def forward(self, x):
        B, T, C = x.size() # 批大小，序列长度，嵌入维度

        # 计算查询、键、值
        qkv = self.c_attn(x)
        q, k, v = qkv.split(self.n_embd, dim=2)
        # 重塑为多头格式
        k = k.view(B, T, self.n_head, C // self.n_head).transpose(1, 2) # (B, nh, T, hs)
        q = q.view(B, T, self.n_head, C // self.n_head).transpose(1, 2) # (B, nh, T, hs)
        v = v.view(B, T, self.n_head, C // self.n_head).transpose(1, 2) # (B, nh, T, hs)

        # 因果自注意力
        if self.flash:
            # 使用高效的 Flash Attention
            y = torch.nn.functional.scaled_dot_product_attention(q, k, v, attn_mask=None, dropout_p=self.dropout if self.training else 0, is_causal=True)
        else:
            # 手动实现（用于理解）
            att = (q @ k.transpose(-2, -1)) * (1.0 / math.sqrt(k.size(-1)))
            # 因果掩码：防止关注未来的标记
            mask = torch.tril(torch.ones(T, T, device=x.device)).view(1, 1, T, T)
            att = att.masked_fill(mask == 0, float(‘-inf’))
            att = F.softmax(att, dim=-1)
            att = self.attn_dropout(att)
            y = att @ v # (B, nh, T, hs)

        # 重新组合多头输出
        y = y.transpose(1, 2).contiguous().view(B, T, C)

        # 输出投影
        y = self.resid_dropout(self.c_proj(y))
        return y
```

![](img/fffe638cf84a8d1e020ce63d0efeee6e_108.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_110.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_112.png)

### 2.4 实现 MLP（前馈网络）

![](img/fffe638cf84a8d1e020ce63d0efeee6e_114.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_116.png)

MLP 包含两个线性层，中间使用 GELU 激活函数。GPT-2 使用了 GELU 的近似版本。

```python
class MLP(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.c_fc = nn.Linear(config.n_embd, 4 * config.n_embd)
        self.c_proj = nn.Linear(4 * config.n_embd, config.n_embd)
        self.dropout = nn.Dropout(config.dropout)

    def forward(self, x):
        x = self.c_fc(x)
        x = F.gelu(x) # GPT-2 使用近似的 gelu: 0.5 * x * (1 + torch.tanh(math.sqrt(2/math.pi) * (x + 0.044715 * torch.pow(x, 3))))
        x = self.c_proj(x)
        x = self.dropout(x)
        return x
```

![](img/fffe638cf84a8d1e020ce63d0efeee6e_118.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_120.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_122.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_124.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_126.png)

---

![](img/fffe638cf84a8d1e020ce63d0efeee6e_128.png)

## 3. 加载预训练权重并验证 ✅

![](img/fffe638cf84a8d1e020ce63d0efeee6e_130.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_132.png)

现在我们已经实现了自己的 GPT-2 类，接下来需要将 Hugging Face 模型中的权重加载到我们的结构中，并验证生成功能是否正常。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_134.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_136.png)

以下是关键步骤：

![](img/fffe638cf84a8d1e020ce63d0efeee6e_138.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_139.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_141.png)

1.  **创建配置对象**：确保超参数（层数、头数、嵌入维度等）与 GPT-2 (124M) 一致。
2.  **映射权重名称**：遍历 Hugging Face 模型的状态字典，将权重名称映射到我们模型中的对应参数。
3.  **处理权重绑定**：注意标记嵌入 (`wte`) 和语言模型头 (`lm_head`) 的权重是共享的。在我们的实现中，可以通过简单地将 `lm_head.weight` 指向 `wte.weight` 来实现。
4.  **验证生成**：使用相同的提示词进行文本生成，比较结果以确保模型行为一致。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_143.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_145.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_147.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_149.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_151.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_153.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_155.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_157.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_159.png)

完成这一步后，我们就有了一个功能上等同于开源 GPT-2 (124M) 的模型，这为我们从头开始训练提供了信心和基准。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_161.png)

---

![](img/fffe638cf84a8d1e020ce63d0efeee6e_163.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_165.png)

## 4. 准备数据与训练循环 🔄

![](img/fffe638cf84a8d1e020ce63d0efeee6e_167.png)

上一节我们成功加载了预训练模型，本节中我们来看看如何准备数据并构建训练循环。

### 4.1 数据加载器

我们需要一个能够将文本数据流式转换为模型可接受的批次张量的数据加载器。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_169.png)

```python
class DataLoaderLite:
    def __init__(self, B, T, process_rank, num_processes):
        self.B = B # 批大小
        self.T = T # 序列长度（块大小）
        self.process_rank = process_rank
        self.num_processes = num_processes
        # 假设我们有一个已标记化的 token 数组 `tokens`
        self.tokens = tokens # 一个一维的 numpy 数组或 torch 张量
        self.current_position = self.B * self.T * self.process_rank # 根据进程排名偏移起始位置

    def next_batch(self):
        B, T = self.B, self.T
        # 获取当前批次所需的 tokens (需要多一个 token 作为目标)
        buf = self.tokens[self.current_position : self.current_position + B*T + 1]
        # 重塑为输入 (x) 和目标 (y)
        x = torch.from_numpy(buf[:-1].reshape(B, T).astype(np.int64))
        y = torch.from_numpy(buf[1:].reshape(B, T).astype(np.int64))
        # 更新位置，循环回到开头如果到达末尾
        self.current_position += B * T * self.num_processes
        if self.current_position + (B * T * self.num_processes + 1) > len(self.tokens):
            self.current_position = self.B * self.T * self.process_rank
        return x, y
```

### 4.2 优化器与学习率调度

我们使用 AdamW 优化器，并遵循 GPT-3 论文中的设置：使用余弦衰减学习率调度，并带有预热阶段。

```python
def configure_optimizers(model, weight_decay, learning_rate, device_type):
    # 将参数分为需要权重衰减和不需要的两组
    decay_params = []
    no_decay_params = []
    for name, param in model.named_parameters():
        if param.requires_grad:
            if name.endswith(‘.bias’) or len(param.shape) == 1:
                no_decay_params.append(param)
            else:
                decay_params.append(param)

    optim_groups = [
        {‘params’: decay_params, ‘weight_decay’: weight_decay},
        {‘params’: no_decay_params, ‘weight_decay’: 0.0}
    ]
    # 使用融合的 AdamW 实现（如果可用）以获得更好性能
    fused_available = ‘fused’ in inspect.signature(torch.optim.AdamW).parameters
    use_fused = fused_available and device_type == ‘cuda’
    optimizer = torch.optim.AdamW(optim_groups, lr=learning_rate, betas=(0.9, 0.95), eps=1e-8, fused=use_fused)
    return optimizer

![](img/fffe638cf84a8d1e020ce63d0efeee6e_171.png)

def get_lr(it, warmup_iters, learning_rate, lr_decay_iters, min_lr):
    # 1) 线性预热
    if it < warmup_iters:
        return learning_rate * (it+1) / warmup_iters
    # 2) 如果超过衰减区间，则返回最小学习率
    if it > lr_decay_iters:
        return min_lr
    # 3) 在预热和衰减区间之间，使用余弦衰减
    decay_ratio = (it - warmup_iters) / (lr_decay_iters - warmup_iters)
    coeff = 0.5 * (1.0 + math.cos(math.pi * decay_ratio))
    return min_lr + coeff * (learning_rate - min_lr)
```

### 4.3 训练步骤

![](img/fffe638cf84a8d1e020ce63d0efeee6e_173.png)

训练循环的核心步骤包括前向传播、损失计算、反向传播和参数更新。

```python
model.train()
optimizer.zero_grad()
loss_accum = 0.0
# 梯度累积循环（用于模拟更大的批大小）
for micro_step in range(gradient_accumulation_steps):
    x, y = get_batch(‘train’)
    x, y = x.to(device), y.to(device)
    with torch.autocast(device_type=device_type, dtype=torch.bfloat16): # 混合精度训练
        logits, loss = model(x, y)
    # 缩放损失以进行梯度累积
    loss = loss / gradient_accumulation_steps
    loss_accum += loss.detach()
    loss.backward()
    # 可选：在最后一步进行梯度裁剪
    if (micro_step == gradient_accumulation_steps - 1):
        torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
        optimizer.step()
        optimizer.zero_grad()
```

![](img/fffe638cf84a8d1e020ce63d0efeee6e_175.png)

---

![](img/fffe638cf84a8d1e020ce63d0efeee6e_177.png)

## 5. 高级优化与多 GPU 训练 ⚡

为了充分利用现代硬件并加速训练，我们需要应用一系列优化技术。

### 5.1 性能优化技巧

![](img/fffe638cf84a8d1e020ce63d0efeee6e_179.png)

以下是提升训练速度的关键方法：

![](img/fffe638cf84a8d1e020ce63d0efeee6e_181.png)

*   **降低数值精度**：使用 `torch.autocast` 进行混合精度训练（BF16），这能显著减少内存占用并加速计算。
*   **使用 `torch.compile`**：PyTorch 2.0 引入的编译器可以融合操作，减少 Python 开销和 GPU 内存读写，带来显著的性能提升。
*   **使用 Flash Attention**：如果硬件支持，使用 PyTorch 内置的高效注意力实现，它通过算法优化避免了大型注意力矩阵的显式存储和计算。
*   **调整“丑陋”的数字**：将词表大小等参数调整为 2 的幂（例如从 50257 调整为 50304），可以使 CUDA 内核运行更高效，因为许多内核是为 2 的幂次方块大小优化的。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_183.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_185.png)

### 5.2 多 GPU 分布式训练

![](img/fffe638cf84a8d1e020ce63d0efeee6e_187.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_189.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_191.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_193.png)

为了在多个 GPU 上并行训练，我们使用 PyTorch 的分布式数据并行。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_195.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_197.png)

```python
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel as DDP

# 初始化进程组
dist.init_process_group(“nccl”)
# 每个进程获取自己的排名和世界大小（总进程数）
rank = dist.get_rank()
world_size = dist.get_world_size()
# 根据排名设置当前进程使用的 GPU
torch.cuda.set_device(rank)
device = f’cuda:{rank}‘

![](img/fffe638cf84a8d1e020ce63d0efeee6e_199.png)

# 包装模型
model = GPT(config)
model.to(device)
model = DDP(model, device_ids=[rank])

![](img/fffe638cf84a8d1e020ce63d0efeee6e_201.png)

# 在训练循环中，确保每个进程处理数据的不同部分
# 梯度会在反向传播时在所有进程间自动同步平均
```

![](img/fffe638cf84a8d1e020ce63d0efeee6e_203.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_205.png)

---

![](img/fffe638cf84a8d1e020ce63d0efeee6e_207.png)

## 6. 使用真实数据集训练与评估 📊

![](img/fffe638cf84a8d1e020ce63d0efeee6e_209.png)

我们使用 FineWeb-Edu 数据集（一个高质量的网络文本子集）进行训练，并使用 HellaSwag 基准进行评估。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_211.png)

### 6.1 数据集准备

![](img/fffe638cf84a8d1e020ce63d0efeee6e_213.png)

1.  下载并预处理 FineWeb-Edu 数据集，将其标记化并保存为分片文件。
2.  修改数据加载器以支持从多个分片中流式读取数据，并确保在多 GPU 训练中每个进程处理不同的数据块。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_215.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_217.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_219.png)

### 6.2 评估：HellaSwag

![](img/fffe638cf84a8d1e020ce63d0efeee6e_221.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_223.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_225.png)

HellaSwag 是一个句子补全任务，用于评估模型的常识推理能力。评估方法不是直接让模型选择 A/B/C/D，而是让模型为每个选项计算平均对数似然（或损失），然后选择可能性最高的选项。

![](img/fffe638cf84a8d1e020ce63d0efeee6e_227.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_229.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_231.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_233.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_235.png)

![](img/fffe638cf84a8d1e020ce63d0efeee6e_237.png)

```python
def evaluate_hellaswag(model, dataset, device):
    model.eval()