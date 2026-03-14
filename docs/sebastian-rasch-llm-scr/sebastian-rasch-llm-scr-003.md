# 3：编码注意力机制 🧠

在本章中，我们将学习注意力机制，这是大语言模型（LLM）的核心“引擎”。我们将从零开始，一步步实现一个完整的自注意力机制，并最终将其扩展为更强大的多头注意力。通过本章的学习，你将深入理解LLM如何通过注意力机制来处理和生成文本。

## 3.3：通过自注意力关注输入的不同部分

![](img/bee02a5463ba446d9eed4413792631b4_1.png)

![](img/bee02a5463ba446d9eed4413792631b4_3.png)

自注意力是一种解析输入数据的机制。它允许模型在处理序列中的任何一个元素时，都能参考整个输入序列，并根据重要性为不同部分分配不同的权重。

### 3.3.1：简化的自注意力机制（无训练权重）

为了循序渐进地理解自注意力，我们首先实现一个简化版本。这个版本不包含可训练的参数，旨在清晰地展示其核心计算逻辑。

#### 核心概念与目标

假设我们有一个输入句子“your journey starts with one step”。经过分词和嵌入后，每个词被表示为一个向量。我们的目标是：对于序列中的每一个词（例如“journey”），计算一个**上下文向量**。这个上下文向量包含了整个输入序列的信息，但会根据当前词（称为**查询**）与其他词的相关性进行加权。

**核心公式**：上下文向量 `z_i` 是输入向量 `x_j` 的加权和，权重 `α_ij` 表示 `x_j` 对于生成 `z_i` 的重要性。
`z_i = Σ_j (α_ij * x_j)`

#### 实现步骤

![](img/bee02a5463ba446d9eed4413792631b4_5.png)

![](img/bee02a5463ba446d9eed4413792631b4_6.png)

以下是计算第二个词（`x_2`）的上下文向量的步骤。

**1. 准备输入数据**
我们首先创建一个模拟的输入张量，其中每一行代表一个词的嵌入向量。

```python
import torch
torch.manual_seed(123)
inputs = torch.tensor([
    [0.43, 0.15, 0.89], # your
    [0.55, 0.87, 0.66], # journey
    [0.57, 0.85, 0.64], # starts
    [0.22, 0.58, 0.33], # with
    [0.77, 0.25, 0.10], # one
    [0.05, 0.80, 0.55]  # step
])
```

**2. 计算注意力分数**
注意力分数衡量查询向量（`x_2`）与序列中每个向量（包括自身）的相似度。我们使用点积作为相似度度量。

```python
query = inputs[1] # 第二个词 “journey” 作为查询
attention_scores = torch.zeros(inputs.shape[0])
for i, x_i in enumerate(inputs):
    attention_scores[i] = torch.dot(query, x_i)
# 更高效的方法：使用矩阵乘法
attention_scores_alt = torch.matmul(inputs, query)
```

**3. 计算注意力权重**
原始的注意力分数可能数值范围很大。我们使用Softmax函数将其归一化，使得所有权重之和为1，这更利于模型优化。

```python
attention_weights = torch.softmax(attention_scores, dim=0)
```

**4. 计算上下文向量**
最后，我们将输入序列中的每个向量乘以其对应的注意力权重，然后求和，得到最终的上下文向量 `z_2`。

![](img/bee02a5463ba446d9eed4413792631b4_8.png)

```python
context_vector_2 = torch.zeros(query.shape)
for i, x_i in enumerate(inputs):
    context_vector_2 += attention_weights[i] * x_i
# 更高效的方法：使用矩阵乘法
context_vector_2_alt = torch.matmul(attention_weights, inputs)
```

至此，我们得到了针对第二个输入词的上下文向量。它融合了所有输入词的信息，但“journey”本身的权重最高，这符合直觉。

### 3.3.2：推广到所有输入

上一节我们只为单个查询词计算了上下文向量。在实际的自注意力中，我们需要为序列中的**每一个**词都计算其对应的上下文向量。这意味着我们需要一个注意力权重矩阵，其中每个元素 `α_ij` 都表示词 `j` 对于词 `i` 的重要性。

**实现方法**
我们可以通过双重循环或更高效的矩阵运算来计算整个注意力分数矩阵。

```python
# 方法1：双重循环（清晰但低效）
attention_scores_matrix = torch.zeros((inputs.shape[0], inputs.shape[0]))
for i, x_i in enumerate(inputs):
    for j, x_j in enumerate(inputs):
        attention_scores_matrix[i, j] = torch.dot(x_i, x_j)

# 方法2：矩阵乘法（高效）
attention_scores_matrix_alt = torch.matmul(inputs, inputs.T)

# 计算注意力权重矩阵（对每一行进行Softmax）
attention_weights_matrix = torch.softmax(attention_scores_matrix_alt, dim=1)

# 一次性计算所有上下文向量
all_context_vectors = torch.matmul(attention_weights_matrix, inputs)
```

![](img/bee02a5463ba446d9eed4413792631b4_10.png)

![](img/bee02a5463ba446d9eed4413792631b4_11.png)

现在，`all_context_vectors` 矩阵的每一行就对应一个输入词的上下文向量。自注意力机制的核心思想——每个输出位置都能关注所有输入位置——在此得以完整实现。

![](img/bee02a5463ba446d9eed4413792631b4_13.png)

![](img/bee02a5463ba446d9eed4413792631b4_15.png)

## 3.4：引入可训练权重的真实自注意力

![](img/bee02a5463ba446d9eed4413792631b4_17.png)

简化的自注意力直接使用原始嵌入向量计算相似度。而真实的自注意力机制（如Transformer中所用）引入了三组可训练的权重矩阵：**查询（Query）**、**键（Key）** 和 **值（Value）**。这使得模型能够学习如何更有效地提取和组合信息。

![](img/bee02a5463ba446d9eed4413792631b4_19.png)

![](img/bee02a5463ba446d9eed4413792631b4_20.png)

![](img/bee02a5463ba446d9eed4413792631b4_22.png)

### 3.4.1：单查询的键值查询（KQV）注意力

我们首先关注如何为单个查询词（如 `x_2`）计算上下文向量，但这次使用可训练的权重。

#### 计算流程

1.  **线性变换**：使用三个不同的权重矩阵 `W_Q`, `W_K`, `W_V` 将每个输入向量 `x_i` 分别投影到查询、键、值空间。
    *   `q_i = x_i W_Q`
    *   `k_i = x_i W_K`
    *   `v_i = x_i W_V`
2.  **计算注意力分数**：计算查询向量 `q_2` 与所有键向量 `k_i` 的点积，作为相似度分数。
    *   `score_2i = q_2 · k_i`
3.  **缩放与归一化**：将分数除以键向量维度的平方根进行缩放（防止点积结果过大），然后应用Softmax得到注意力权重。
    *   `attention_weights_2i = softmax(score_2i / sqrt(d_k))`
4.  **计算上下文向量**：将注意力权重应用于所有的值向量 `v_i`，进行加权求和。
    *   `context_2 = Σ_i (attention_weights_2i * v_i)`

**代码实现**

![](img/bee02a5463ba446d9eed4413792631b4_24.png)

![](img/bee02a5463ba446d9eed4413792631b4_25.png)

![](img/bee02a5463ba446d9eed4413792631b4_27.png)

![](img/bee02a5463ba446d9eed4413792631b4_29.png)

```python
torch.manual_seed(123)
d_in = inputs.shape[1] # 输入维度，例如3
d_out = 2 # 输出/上下文向量维度，可自由设定

![](img/bee02a5463ba446d9eed4413792631b4_31.png)

# 初始化可训练的权重矩阵
W_query = torch.nn.Parameter(torch.rand(d_in, d_out))
W_key = torch.nn.Parameter(torch.rand(d_in, d_out))
W_value = torch.nn.Parameter(torch.rand(d_in, d_out))

![](img/bee02a5463ba446d9eed4413792631b4_33.png)

![](img/bee02a5463ba446d9eed4413792631b4_34.png)

# 1. 线性变换
query_2 = torch.matmul(inputs[1], W_query) # q_2
keys = torch.matmul(inputs, W_key) # 所有 k_i
values = torch.matmul(inputs, W_value) # 所有 v_i

![](img/bee02a5463ba446d9eed4413792631b4_36.png)

![](img/bee02a5463ba446d9eed4413792631b4_38.png)

![](img/bee02a5463ba446d9eed4413792631b4_39.png)

![](img/bee02a5463ba446d9eed4413792631b4_41.png)

![](img/bee02a5463ba446d9eed4413792631b4_43.png)

![](img/bee02a5463ba446d9eed4413792631b4_45.png)

![](img/bee02a5463ba446d9eed4413792631b4_47.png)

# 2. 计算注意力分数（针对查询2）
attention_scores_2 = torch.matmul(query_2, keys.T) # q_2 与所有 k_i 点积

# 3. 缩放与归一化
d_k = keys.shape[-1]
attention_weights_2 = torch.softmax(attention_scores_2 / d_k**0.5, dim=-1)

# 4. 计算上下文向量
context_vector_2 = torch.matmul(attention_weights_2, values)
```

通过引入可训练的 `W_Q`, `W_K`, `W_V`，模型可以在训练过程中学习如何为特定任务（如语言建模）生成更有意义的查询、键和值表示，从而计算出更有效的注意力权重和上下文向量。

### 3.4.2：推广到所有输入并封装为类

现在，我们将上一节针对单个查询的计算推广到为所有输入词并行计算上下文向量，并将其封装成一个整洁的PyTorch模块。

**实现要点**
*   使用矩阵运算一次性为所有输入生成查询、键、值矩阵。
*   计算所有查询-键对的注意力分数矩阵。
*   对分数矩阵的每一行进行缩放和Softmax，得到注意力权重矩阵。
*   使用注意力权重矩阵对值矩阵进行加权求和，得到所有上下文向量。

```python
import torch.nn as nn

![](img/bee02a5463ba446d9eed4413792631b4_49.png)

![](img/bee02a5463ba446d9eed4413792631b4_51.png)

class SelfAttentionV1(nn.Module):
    def __init__(self, d_in, d_out):
        super().__init__()
        self.W_query = nn.Linear(d_in, d_out, bias=False)
        self.W_key = nn.Linear(d_in, d_out, bias=False)
        self.W_value = nn.Linear(d_in, d_out, bias=False)

    def forward(self, x):
        queries = self.W_query(x)
        keys = self.W_key(x)
        values = self.W_value(x)

        # 计算注意力分数，缩放，并应用Softmax
        attn_scores = torch.matmul(queries, keys.T)
        attn_weights = torch.softmax(attn_scores / keys.shape[-1]**0.5, dim=-1)

        # 计算上下文向量
        context_vec = torch.matmul(attn_weights, values)
        return context_vec

# 使用示例
sa_v1 = SelfAttentionV1(d_in=3, d_out=2)
context_vectors = sa_v1(inputs)
```

![](img/bee02a5463ba446d9eed4413792631b4_53.png)

![](img/bee02a5463ba446d9eed4413792631b4_54.png)

这个 `SelfAttentionV1` 类已经是一个功能完整的自注意力模块。它接收一个输入序列，并输出一个融合了全局信息的上下文向量序列。

## 3.5：因果注意力与掩码

在语言建模任务中，模型在预测下一个词时，不应该“看到”未来的词。标准的自注意力机制会关注整个序列，包括未来词，这会导致训练时信息泄露。为了解决这个问题，我们引入**因果注意力掩码**。

![](img/bee02a5463ba446d9eed4413792631b4_56.png)

![](img/bee02a5463ba446d9eed4413792631b4_58.png)

### 3.5.1：因果注意力掩码

因果掩码的目标是：在计算注意力权重时，对于序列中的位置 `i`，只允许它关注位置 `0` 到 `i` 的词（即过去和现在的词），而将位置 `i+1` 及之后的词（未来词）的注意力权重强制设为0。

**实现方法**
我们创建一个下三角矩阵（主对角线及以下为1，以上为0），将其与注意力分数矩阵相乘（或相加一个极大的负值），使得未来位置的分数在Softmax后趋近于0。

```python
context_len = inputs.shape[0]
mask = torch.tril(torch.ones(context_len, context_len)) # 下三角矩阵
print(mask)
# tensor([[1., 0., 0., 0., 0., 0.],
#         [1., 1., 0., 0., 0., 0.],
#         [1., 1., 1., 0., 0., 0.],
#         [1., 1., 1., 1., 0., 0.],
#         [1., 1., 1., 1., 1., 0.],
#         [1., 1., 1., 1., 1., 1.]])

# 在Softmax之前应用掩码
masked_attn_scores = attn_scores.masked_fill(mask == 0, -torch.inf)
masked_attn_weights = torch.softmax(masked_attn_scores / keys.shape[-1]**0.5, dim=-1)
print(masked_attn_weights)
```
应用掩码后，注意力权重矩阵的上三角部分（代表对未来词的关注）都变成了0。

![](img/bee02a5463ba446d9eed4413792631b4_60.png)

![](img/bee02a5463ba446d9eed4413792631b4_61.png)

![](img/bee02a5463ba446d9eed4413792631b4_63.png)

### 3.5.2：Dropout掩码

![](img/bee02a5463ba446d9eed4413792631b4_64.png)

![](img/bee02a5463ba446d9eed4413792631b4_65.png)

![](img/bee02a5463ba446d9eed4413792631b4_66.png)

![](img/bee02a5463ba446d9eed4413792631b4_68.png)

![](img/bee02a5463ba446d9eed4413792631b4_69.png)

除了因果掩码，有时还会应用**Dropout掩码**。这是一种正则化技术，在训练过程中随机“丢弃”（置零）一部分注意力权重，迫使模型不过度依赖某些特定的注意力连接，从而减轻过拟合。

![](img/bee02a5463ba446d9eed4413792631b4_71.png)

```python
dropout = nn.Dropout(0.5) # 以50%的概率丢弃
masked_attn_weights_dropout = dropout(masked_attn_weights)
```
需要注意的是，现代大型LLM训练中，注意力层的Dropout使用已不常见，但为了完整性我们在此提及。

### 3.5.3：实现因果注意力类

![](img/bee02a5463ba446d9eed4413792631b4_73.png)

![](img/bee02a5463ba446d9eed4413792631b4_75.png)

现在，我们将因果掩码和Dropout整合到之前的自注意力类中，创建一个完整的**因果自注意力**类。

```python
class CausalAttention(nn.Module):
    def __init__(self, d_in, d_out, context_length, dropout_rate=0.0):
        super().__init__()
        self.d_out = d_out
        self.W_query = nn.Linear(d_in, d_out, bias=False)
        self.W_key = nn.Linear(d_in, d_out, bias=False)
        self.W_value = nn.Linear(d_in, d_out, bias=False)
        self.dropout = nn.Dropout(dropout_rate)
        # 注册一个不可训练的缓冲区，用于存储因果掩码
        self.register_buffer(
            'mask',
            torch.triu(torch.ones(context_length, context_length), diagonal=1)
        )

    def forward(self, x):
        b, num_tokens, d_in = x.shape
        queries = self.W_query(x)
        keys = self.W_key(x)
        values = self.W_value(x)

        attn_scores = torch.matmul(queries, keys.transpose(1, 2))
        attn_scores = attn_scores / keys.shape[-1]**0.5

        # 应用因果掩码（将未来位置设为负无穷）
        attn_scores = attn_scores.masked_fill(
            self.mask.bool()[:num_tokens, :num_tokens], -torch.inf
        )

        attn_weights = torch.softmax(attn_scores, dim=-1)
        attn_weights = self.dropout(attn_weights)

        context_vec = torch.matmul(attn_weights, values)
        return context_vec

![](img/bee02a5463ba446d9eed4413792631b4_77.png)

![](img/bee02a5463ba446d9eed4413792631b4_78.png)

# 使用示例（考虑批量输入）
batch = torch.stack([inputs, inputs], dim=0) # 形状: (2, 6, 3)
ca = CausalAttention(d_in=3, d_out=2, context_length=6, dropout_rate=0.0)
context_vectors_causal = ca(batch)
```

![](img/bee02a5463ba446d9eed4413792631b4_80.png)

![](img/bee02a5463ba446d9eed4413792631b4_81.png)

![](img/bee02a5463ba446d9eed4413792631b4_82.png)

这个 `CausalAttention` 类已经具备了LLM解码器所需自注意力机制的核心特征。

## 3.6：多头注意力 🚀

单个注意力头可能只捕获到输入序列中特定类型的依赖关系（例如语法结构、指代关系等）。为了增强模型的表达能力，Transformer采用了**多头注意力**机制。它并行运行多个独立的注意力头，每个头都有自己的 `W_Q`, `W_K`, `W_V` 权重矩阵，从而可以从不同角度对输入进行解读。

### 3.6.1：多头注意力的概念与实现

**核心思想**：
1.  将输入分别投影到 `H` 个（头数）不同的查询、键、值子空间。
2.  在每个子空间中独立计算缩放点积注意力。
3.  将 `H` 个头输出的上下文向量拼接起来。
4.  （可选）通过一个线性投影层 `W_O` 将拼接后的向量映射回目标维度。

**简单实现（顺序执行）**
一种直观的实现方式是创建多个 `CausalAttention` 实例，然后依次计算并将结果拼接。

```python
class MultiHeadAttentionWrapper(nn.Module):
    def __init__(self, d_in, d_out, context_length, num_heads, dropout_rate=0.0):
        super().__init__()
        self.heads = nn.ModuleList([
            CausalAttention(d_in, d_out, context_length, dropout_rate)
            for _ in range(num_heads)
        ])

    def forward(self, x):
        # 依次计算每个头的输出并拼接
        head_outputs = [head(x) for head in self.heads]
        context_vec = torch.cat(head_outputs, dim=-1)
        return context_vec
```

### 3.6.2：高效的多头注意力实现

![](img/bee02a5463ba446d9eed4413792631b4_84.png)

![](img/bee02a5463ba446d9eed4413792631b4_86.png)

![](img/bee02a5463ba446d9eed4413792631b4_88.png)

上述实现使用Python循环顺序调用各个头，效率较低。我们可以利用矩阵运算的并行性，通过一次大的矩阵乘法完成所有头的线性变换，然后通过重塑（reshape）和转置操作来分离各个头。

![](img/bee02a5463ba446d9eed4413792631b4_90.png)

![](img/bee02a5463ba446d9eed4413792631b4_91.png)

![](img/bee02a5463ba446d9eed4413792631b4_92.png)

![](img/bee02a5463ba446d9eed4413792631b4_94.png)

![](img/bee02a5463ba446d9eed4413792631b4_95.png)

![](img/bee02a5463ba446d9eed4413792631b4_96.png)

![](img/bee02a5463ba446d9eed4413792631b4_98.png)

```python
class MultiHeadAttention(nn.Module):
    def __init__(self, d_in, d_out, context_length, num_heads, dropout_rate=0.0):
        super().__init__()
        assert d_out % num_heads == 0, “d_out must be divisible by num_heads”
        self.d_out = d_out
        self.num_heads = num_heads
        self.head_dim = d_out // num_heads

        self.W_query = nn.Linear(d_in, d_out, bias=False)
        self.W_key = nn.Linear(d_in, d_out, bias=False)
        self.W_value = nn.Linear(d_in, d_out, bias=False)
        self.dropout = nn.Dropout(dropout_rate)
        self.register_buffer(
            'mask',
            torch.triu(torch.ones(context_length, context_length), diagonal=1)
        )

    def forward(self, x):
        b, num_tokens, d_in = x.shape

        # 1. 计算Q, K, V （一次性计算所有头）
        queries = self.W_query(x) # 形状: (b, num_tokens, d_out)
        keys = self.W_key(x)
        values = self.W_value(x)

        # 2. 重塑以分离出头维度
        # 目标形状: (b, num_tokens, num_heads, head_dim)
        queries = queries.view(b, num_tokens, self.num_heads, self.head_dim)
        keys = keys.view(b, num_tokens, self.num_heads, self.head_dim)
        values = values.view(b, num_tokens, self.num_heads, self.head_dim)

        # 3. 转置以便矩阵乘法在头维度上并行
        # 目标形状: (b, num_heads, num_tokens, head_dim)
        queries