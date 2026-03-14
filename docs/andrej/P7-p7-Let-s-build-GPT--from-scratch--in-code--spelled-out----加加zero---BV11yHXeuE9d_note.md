# 课程 P7：从零构建 GPT 🧠

![](img/d489e9697e4ec525091637b9ac0b6163_1.png)

在本节课中，我们将学习如何从零开始构建一个类似 GPT 的 Transformer 语言模型。我们将使用一个简单的字符级数据集（Tiny Shakespeare），并逐步实现模型的核心组件，包括自注意力机制、多头注意力、前馈网络以及残差连接等。通过这个过程，你将深入理解现代大型语言模型（如 ChatGPT）背后的基本原理。

![](img/d489e9697e4ec525091637b9ac0b6163_3.png)

---

![](img/d489e9697e4ec525091637b9ac0b6163_5.png)

![](img/d489e9697e4ec525091637b9ac0b6163_7.png)

## 概述 📋

![](img/d489e9697e4ec525091637b9ac0b6163_9.png)

![](img/d489e9697e4ec525091637b9ac0b6163_11.png)

![](img/d489e9697e4ec525091637b9ac0b6163_13.png)

Transformer 架构是当今许多先进 AI 系统的核心，它最初在 2017 年的论文《Attention Is All You Need》中被提出。GPT（Generative Pre-trained Transformer）正是基于此架构构建的。在本教程中，我们将专注于构建一个仅解码器的 Transformer，用于字符级语言建模任务。虽然我们无法复现 ChatGPT 那样的复杂系统，但通过构建一个微型版本，我们可以清晰地理解其工作原理。

![](img/d489e9697e4ec525091637b9ac0b6163_15.png)

![](img/d489e9697e4ec525091637b9ac0b6163_17.png)

我们将从处理数据开始，逐步实现模型的关键部分，并在 Tiny Shakespeare 数据集上进行训练，最终生成莎士比亚风格的文本。

---

![](img/d489e9697e4ec525091637b9ac0b6163_19.png)

## 1. 数据准备与分词 📚

![](img/d489e9697e4ec525091637b9ac0b6163_21.png)

首先，我们需要准备数据并将其转换为模型可以处理的格式。我们将使用 Tiny Shakespeare 数据集，它包含了莎士比亚的所有作品。

### 1.1 读取数据
我们从指定 URL 下载数据集，并将其读取为一个长字符串。

```python
import torch
import requests

# 下载数据集
url = "https://raw.githubusercontent.com/karpathy/char-rnn/master/data/tinyshakespeare/input.txt"
text = requests.get(url).text
print(f"数据集长度（字符数）: {len(text)}")
print(text[:1000])  # 打印前1000个字符
```

### 1.2 创建词汇表
接下来，我们找出数据集中所有独特的字符，构建一个词汇表。每个字符将被映射到一个唯一的整数（标记）。

```python
# 获取所有独特字符并排序
chars = sorted(list(set(text)))
vocab_size = len(chars)
print(f"词汇表大小: {vocab_size}")
print(''.join(chars))  # 打印所有字符

# 创建编码器和解码器
stoi = {ch: i for i, ch in enumerate(chars)}  # 字符 -> 整数
itos = {i: ch for i, ch in enumerate(chars)}  # 整数 -> 字符

def encode(s):
    return [stoi[c] for c in s]  # 字符串 -> 整数列表

def decode(l):
    return ''.join([itos[i] for i in l])  # 整数列表 -> 字符串

# 测试编码解码
test_str = "hi there"
encoded = encode(test_str)
decoded = decode(encoded)
print(f"原始字符串: {test_str}")
print(f"编码后: {encoded}")
print(f"解码后: {decoded}")
```

### 1.3 划分数据集
我们将数据集分为训练集（90%）和验证集（10%）。验证集用于评估模型的泛化能力，防止过拟合。

```python
# 将整个文本编码为整数张量
data = torch.tensor(encode(text), dtype=torch.long)

# 划分训练集和验证集
n = int(0.9 * len(data))
train_data = data[:n]
val_data = data[n:]
```

---

![](img/d489e9697e4ec525091637b9ac0b6163_23.png)

## 2. 数据批处理 🔄

![](img/d489e9697e4ec525091637b9ac0b6163_25.png)

![](img/d489e9697e4ec525091637b9ac0b6163_27.png)

由于我们无法一次性将整个数据集输入模型，因此需要从数据中随机抽取小块（批次）进行训练。每个批次包含多个独立的序列，模型将并行处理它们。

以下是创建数据批次的函数：

![](img/d489e9697e4ec525091637b9ac0b6163_29.png)

```python
def get_batch(split):
    # 根据 split 选择训练集或验证集
    data = train_data if split == 'train' else val_data
    # 生成随机起始索引
    ix = torch.randint(len(data) - block_size, (batch_size,))
    # 构建输入 x 和目标 y
    x = torch.stack([data[i:i+block_size] for i in ix])
    y = torch.stack([data[i+1:i+block_size+1] for i in ix])
    return x, y

# 设置超参数
batch_size = 4
block_size = 8

# 获取一个批次
xb, yb = get_batch('train')
print('输入 xb 的形状:', xb.shape)
print('目标 yb 的形状:', yb.shape)
print('输入示例:\n', xb)
print('目标示例:\n', yb)
```

在这个批次中，`xb` 是模型的输入，`yb` 是每个位置对应的下一个字符的目标值。模型的任务是根据 `xb` 的上下文预测 `yb`。

---

## 3. 基础模型：Bigram 语言模型 🔤

在深入 Transformer 之前，我们先实现一个最简单的语言模型——Bigram 模型。它仅根据当前字符的身份来预测下一个字符，不考虑任何上下文信息。

### 3.1 模型定义
Bigram 模型本质上是一个查找表，其中每个字符都直接预测下一个字符的分布。

```python
import torch.nn as nn

![](img/d489e9697e4ec525091637b9ac0b6163_31.png)

class BigramLanguageModel(nn.Module):
    def __init__(self, vocab_size):
        super().__init__()
        # 每个标记直接映射到下一个标记的 logits
        self.token_embedding_table = nn.Embedding(vocab_size, vocab_size)

    def forward(self, idx, targets=None):
        # idx 和 targets 都是形状为 (B, T) 的整数张量
        logits = self.token_embedding_table(idx)  # (B, T, C)

        if targets is None:
            loss = None
        else:
            # 调整形状以匹配 PyTorch 的交叉熵损失期望
            B, T, C = logits.shape
            logits = logits.view(B*T, C)
            targets = targets.view(B*T)
            loss = nn.functional.cross_entropy(logits, targets)

        return logits, loss

    def generate(self, idx, max_new_tokens):
        # idx 是当前上下文，形状为 (B, T)
        for _ in range(max_new_tokens):
            # 获取预测
            logits, loss = self(idx)
            # 只关注最后一步
            logits = logits[:, -1, :]  # 变为 (B, C)
            # 应用 softmax 获取概率
            probs = nn.functional.softmax(logits, dim=-1)  # (B, C)
            # 从分布中采样下一个标记
            idx_next = torch.multinomial(probs, num_samples=1)  # (B, 1)
            # 将采样到的标记附加到序列上
            idx = torch.cat((idx, idx_next), dim=1)  # (B, T+1)
        return idx

# 实例化模型
model = BigramLanguageModel(vocab_size)
```

![](img/d489e9697e4ec525091637b9ac0b6163_33.png)

### 3.2 训练与生成
我们可以用简单的优化循环来训练这个模型，并观察其生成效果。

```python
optimizer = torch.optim.AdamW(model.parameters(), lr=1e-3)

for steps in range(10000):
    # 获取一个数据批次
    xb, yb = get_batch('train')
    # 前向传播，计算损失
    logits, loss = model(xb, yb)
    # 反向传播，更新参数
    optimizer.zero_grad(set_to_none=True)
    loss.backward()
    optimizer.step()

print(f"最终损失: {loss.item()}")

# 生成文本
context = torch.zeros((1, 1), dtype=torch.long)
print(decode(model.generate(context, max_new_tokens=500)[0].tolist()))
```

Bigram 模型的表现非常有限，因为它没有利用上下文信息。接下来，我们将引入自注意力机制，让字符之间能够进行交流。

---

## 4. 自注意力机制 🤝

自注意力是 Transformer 的核心组件，它允许序列中的每个元素（标记）根据其与序列中其他元素的关系来聚合信息。

![](img/d489e9697e4ec525091637b9ac0b6163_35.png)

### 4.1 数学原理
自注意力的关键思想是让每个标记生成三个向量：**查询（Query）**、**键（Key）** 和 **值（Value）**。
*   **查询（Q）**：表示“我正在寻找什么”。
*   **键（K）**：表示“我包含什么信息”。
*   **值（V）**：表示“如果被关注，我将传递什么信息”。

标记之间的亲和力（注意力权重）通过查询和键的点积计算：`affinity = Q @ K^T`。然后，我们使用这些权重对值进行加权求和，从而聚合信息。

![](img/d489e9697e4ec525091637b9ac0b6163_37.png)

![](img/d489e9697e4ec525091637b9ac0b6163_39.png)

为了实现语言建模中的因果性（即当前标记不能看到未来标记），我们使用一个下三角掩码矩阵，将未来位置的注意力权重设置为负无穷大，这样在 softmax 后它们的权重就变为 0。

### 4.2 实现单头自注意力
以下是单头自注意力的 PyTorch 实现：

![](img/d489e9697e4ec525091637b9ac0b6163_41.png)

![](img/d489e9697e4ec525091637b9ac0b6163_43.png)

```python
class Head(nn.Module):
    """ 单头自注意力 """

    def __init__(self, head_size):
        super().__init__()
        self.key = nn.Linear(n_embd, head_size, bias=False)
        self.query = nn.Linear(n_embd, head_size, bias=False)
        self.value = nn.Linear(n_embd, head_size, bias=False)
        # 下三角掩码，用于实现因果注意力
        self.register_buffer('tril', torch.tril(torch.ones(block_size, block_size)))
        self.dropout = nn.Dropout(dropout)

    def forward(self, x):
        B, T, C = x.shape
        k = self.key(x)   # (B, T, head_size)
        q = self.query(x) # (B, T, head_size)
        # 计算注意力分数（亲和力）
        wei = q @ k.transpose(-2, -1) * C**-0.5  # (B, T, T) 缩放点积
        # 应用因果掩码
        wei = wei.masked_fill(self.tril[:T, :T] == 0, float('-inf'))
        wei = nn.functional.softmax(wei, dim=-1)  # (B, T, T)
        wei = self.dropout(wei)
        # 加权聚合值
        v = self.value(x)  # (B, T, head_size)
        out = wei @ v  # (B, T, head_size)
        return out
```

![](img/d489e9697e4ec525091637b9ac0b6163_45.png)

![](img/d489e9697e4ec525091637b9ac0b6163_46.png)

![](img/d489e9697e4ec525091637b9ac0b6163_48.png)

在这个实现中：
1.  我们为键、查询和值定义了线性投影层。
2.  计算缩放点积注意力分数，并应用因果掩码。
3.  使用 softmax 将分数转换为概率分布（注意力权重）。
4.  使用这些权重对值向量进行加权求和，得到输出。

![](img/d489e9697e4ec525091637b9ac0b6163_50.png)

![](img/d489e9697e4ec525091637b9ac0b6163_52.png)

---

![](img/d489e9697e4ec525091637b9ac0b6163_54.png)

![](img/d489e9697e4ec525091637b9ac0b6163_56.png)

## 5. 多头注意力与 Transformer 块 🧩

![](img/d489e9697e4ec525091637b9ac0b6163_58.png)

![](img/d489e9697e4ec525091637b9ac0b6163_60.png)

单个注意力头可能只关注特定类型的关系。为了捕捉更丰富的信息，我们并行使用多个注意力头，这就是**多头注意力**。

![](img/d489e9697e4ec525091637b9ac0b6163_62.png)

![](img/d489e9697e4ec525091637b9ac0b6163_64.png)

### 5.1 实现多头注意力
我们将多个单头注意力的输出在通道维度上拼接起来。

```python
class MultiHeadAttention(nn.Module):
    """ 多头自注意力 """

    def __init__(self, num_heads, head_size):
        super().__init__()
        self.heads = nn.ModuleList([Head(head_size) for _ in range(num_heads)])
        self.proj = nn.Linear(n_embd, n_embd)  # 投影层
        self.dropout = nn.Dropout(dropout)

    def forward(self, x):
        # 并行运行所有注意力头并拼接结果
        out = torch.cat([h(x) for h in self.heads], dim=-1)
        out = self.dropout(self.proj(out))  # 投影回残差路径
        return out
```

![](img/d489e9697e4ec525091637b9ac0b6163_66.png)

![](img/d489e9697e4ec525091637b9ac0b6163_68.png)

### 5.2 前馈网络
在自注意力进行通信之后，每个标记需要独立处理收集到的信息。这是通过一个简单的前馈网络（FFN）实现的，通常是一个两层 MLP。

![](img/d489e9697e4ec525091637b9ac0b6163_70.png)

![](img/d489e9697e4ec525091637b9ac0b6163_72.png)

![](img/d489e9697e4ec525091637b9ac0b6163_74.png)

```python
class FeedForward(nn.Module):
    """ 简单的前馈网络 """

    def __init__(self, n_embd):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(n_embd, 4 * n_embd),  # 扩展维度
            nn.ReLU(),
            nn.Linear(4 * n_embd, n_embd),  # 投影回原始维度
            nn.Dropout(dropout),
        )

    def forward(self, x):
        return self.net(x)
```

![](img/d489e9697e4ec525091637b9ac0b6163_76.png)

![](img/d489e9697e4ec525091637b9ac0b6163_77.png)

![](img/d489e9697e4ec525091637b9ac0b6163_79.png)

### 5.3 构建 Transformer 块
现在，我们将多头注意力和前馈网络组合成一个 Transformer 块。为了优化深度网络，我们引入**残差连接**和**层归一化**。

![](img/d489e9697e4ec525091637b9ac0b6163_81.png)

*   **残差连接**：将块的输入直接加到其输出上。这创建了一条梯度高速公路，有助于缓解深度网络中的梯度消失问题。
*   **层归一化**：在块内对每个标记的特征进行归一化，稳定训练过程。

```python
class Block(nn.Module):
    """ Transformer 块：通信（注意力）后接计算（前馈） """

    def __init__(self, n_embd, n_head):
        super().__init__()
        head_size = n_embd // n_head
        self.sa = MultiHeadAttention(n_head, head_size)  # 多头自注意力
        self.ffwd = FeedForward(n_embd)                  # 前馈网络
        self.ln1 = nn.LayerNorm(n_embd)                  # 层归一化 1
        self.ln2 = nn.LayerNorm(n_embd)                  # 层归一化 2

    def forward(self, x):
        # 带残差连接和层归一化的自注意力
        x = x + self.sa(self.ln1(x))
        # 带残差连接和层归一化的前馈网络
        x = x + self.ffwd(self.ln2(x))
        return x
```

![](img/d489e9697e4ec525091637b9ac0b6163_83.png)

---

## 6. 构建完整 GPT 模型 🏗️

![](img/d489e9697e4ec525091637b9ac0b6163_85.png)

![](img/d489e9697e4ec525091637b9ac0b6163_87.png)

现在，我们可以将所有组件组合起来，构建完整的 GPT 模型。我们的模型将包括：
1.  **标记嵌入层**：将整数标记转换为向量。
2.  **位置嵌入层**：为序列中的每个位置提供位置信息。
3.  多个 **Transformer 块**（解码器块）。
4.  最终的**层归一化**和**线性投影层**，用于预测下一个标记。

![](img/d489e9697e4ec525091637b9ac0b6163_89.png)

![](img/d489e9697e4ec525091637b9ac0b6163_91.png)

![](img/d489e9697e4ec525091637b9ac0b6163_93.png)

```python
class GPTLanguageModel(nn.Module):

    def __init__(self):
        super().__init__()
        # 每个标记对应一个嵌入向量
        self.token_embedding_table = nn.Embedding(vocab_size, n_embd)
        # 每个位置对应一个嵌入向量
        self.position_embedding_table = nn.Embedding(block_size, n_embd)
        # 堆叠 Transformer 块
        self.blocks = nn.Sequential(*[Block(n_embd, n_head=n_head) for _ in range(n_layer)])
        # 最终的层归一化
        self.ln_f = nn.LayerNorm(n_embd)
        # 语言建模头，将特征投影回词汇表
        self.lm_head = nn.Linear(n_embd, vocab_size)

    def forward(self, idx, targets=None):
        B, T = idx.shape

        # 获取标记嵌入和位置嵌入
        tok_emb = self.token_embedding_table(idx)  # (B, T, n_embd)
        pos_emb = self.position_embedding_table(torch.arange(T, device=device))  # (T, n_embd)
        x = tok_emb + pos_emb  # (B, T, n_embd)
        # 通过 Transformer 块
        x = self.blocks(x)
        x = self.ln_f(x)
        logits = self.lm_head(x)  # (B, T, vocab_size)

        if targets is None:
            loss = None
        else:
            B, T, C = logits.shape
            logits = logits.view(B*T, C)
            targets = targets.view(B*T)
            loss = nn.functional.cross_entropy(logits, targets)

        return logits, loss

    def generate(self, idx, max_new_tokens):
        # idx 是当前上下文 (B, T)
        for _ in range(max_new_tokens):
            # 如果上下文过长，裁剪到块大小
            idx_cond = idx if idx.size(1) <= block_size else idx[:, -block_size:]
            # 获取预测
            logits, loss = self(idx_cond)
            # 关注最后一步
            logits = logits[:, -1, :]  # (B, C)
            # 应用 softmax 获取概率
            probs = nn.functional.softmax(logits, dim=-1)  # (B, C)
            # 从分布中采样下一个标记
            idx_next = torch.multinomial(probs, num_samples=1)  # (B, 1)
            # 将采样到的标记附加到序列上
            idx = torch.cat((idx, idx_next), dim=1)  # (B, T+1)
        return idx
```

![](img/d489e9697e4ec525091637b9ac0b6163_95.png)

![](img/d489e9697e4ec525091637b9ac0b6163_97.png)

---

![](img/d489e9697e4ec525091637b9ac0b6163_99.png)

## 7. 模型训练与评估 🚀

![](img/d489e9697e4ec525091637b9ac0b6163_101.png)

![](img/d489e9697e4ec525091637b9ac0b6163_103.png)

现在，我们可以使用更大的超参数来训练我们的 GPT 模型，并观察其性能。

![](img/d489e9697e4ec525091637b9ac0b6163_105.png)

![](img/d489e9697e4ec525091637b9ac0b6163_107.png)

### 7.1 设置超参数与设备
```python
# 超参数
batch_size = 64         # 每批处理的独立序列数
block_size = 256        # 最大上下文长度
max_iters = 5000        # 训练迭代次数
eval_interval = 500     # 每多少步评估一次
learning_rate = 3e-4    # 学习率
device = 'cuda' if torch.cuda.is_available() else 'cpu'  # 使用 GPU 如果可用
eval_iters = 200        # 评估时平均损失的批次数量
n_embd = 384            # 嵌入维度
n_head = 6              # 注意力头数量
n_layer = 6             # Transformer 块层数
dropout = 0.2           # Dropout 比率

![](img/d489e9697e4ec525091637b9ac0b6163_109.png)

# 实例化模型并移至设备
model = GPTLanguageModel()
m = model.to(device)
print(f"模型参数量: {sum(p.numel() for p in m.parameters())/1e6:.2f} M")

![](img/d489e9697e4ec525091637b9ac0b6163_111.png)

![](img/d489e9697e4ec525091637b9ac0b6163_113.png)

# 创建优化器
optimizer =