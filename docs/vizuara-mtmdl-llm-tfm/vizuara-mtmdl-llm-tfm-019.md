#  019：理解因果注意力机制

在本节课中，我们将学习因果注意力机制，这是GPT等模型中实际使用的注意力机制。我们将了解其工作原理，特别是它与标准自注意力的区别。虽然我们将在下一讲中实现多头注意力，但今天我们将专注于理解掩码自注意力（也称为因果注意力）的核心概念。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_1.png)

## 自注意力机制回顾

![](img/edbe8ac55858e949bcfeafd4a78bbd19_2.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_4.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_6.png)

在深入探讨因果注意力机制的细节之前，我们先快速回顾一下注意力机制的基本思想。

注意力机制的目标是为每一个输入嵌入向量生成一个对应的上下文向量。输入嵌入向量是每个词元（token）的直接向量表示，而上下文向量则包含了该词元周围其他词元的信息，因此信息更加丰富。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_8.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_9.png)

例如，对于句子“The cat sat on the mat”，如果简单地将词元“cat”转换为输入嵌入向量，它可能只代表“猫”这个词本身。然而，在句子中，“cat”与“sat”、“on”、“the”、“mat”等词元相关联。上下文向量就是为了将这种关联信息编码到“cat”的表示中。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_11.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_13.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_14.png)

如果句子有6个词元，那么输出就需要6个上下文向量。这些上下文向量比原始的输入嵌入向量包含更丰富的信息。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_16.png)

### 简化的自注意力

在两讲之前，我们实现了一个简化版本的自注意力。其核心思想是计算两个向量之间的点积，作为注意力分数的代理。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_18.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_19.png)

如果两个向量在语义空间中指向相似的方向，它们的点积就大，意味着它们应该更多地关注彼此。反之，如果向量正交（点积为0），则意味着它们之间关联不大。

然而，这种方法存在一个问题。考虑句子“The dog chased the ball, but it could not catch it.”中的最后一个词元“it”。“it”应该更多地指代“ball”而不是“dog”。如果直接使用输入嵌入向量的点积，可能会发现“it”与“ball”和“dog”的点积相似，无法有效区分。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_21.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_22.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_23.png)

### 引入可学习的变换

![](img/edbe8ac55858e949bcfeafd4a78bbd19_25.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_27.png)

为了解决上述问题，我们在上一讲中引入了可学习的变换矩阵。我们不再直接使用输入嵌入向量计算点积，而是先将它们变换到查询（Query）、键（Key）、值（Value）空间。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_29.png)

以下是引入的三个变换矩阵：
*   **查询矩阵 `W_Q`**：用于将当前关注的词元（如“it”）变换到查询空间。
*   **键矩阵 `W_K`**：用于将所有词元（如“dog”、“ball”）变换到键空间。
*   **值矩阵 `W_V`**：用于将所有词元变换到值空间，用于最终生成上下文向量。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_31.png)

通过使用`W_Q`和`W_K`矩阵进行变换，再计算变换后向量的点积，模型可以学习到更复杂、更准确的注意力模式。例如，经过训练，变换后的“it”与变换后的“ball”的点积可以大于与变换后的“dog”的点积，从而正确捕捉“it”指代“ball”的语义关系。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_33.png)

## 因果注意力机制

上一节我们回顾了标准自注意力如何通过变换来捕捉词元间的关联。本节中，我们来看看因果注意力机制，它在此基础上增加了一个关键约束。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_35.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_36.png)

因果注意力，也称为掩码自注意力，是GPT等自回归语言模型的核心。它的核心思想是：在生成或处理一个词元时，模型只能“看到”或“关注”该词元之前（左侧）的词元，而不能“看到”未来的词元。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_38.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_40.png)

这模拟了人类阅读或生成文本的过程：我们写下一个词时，只知道前面已经写下的内容，而不知道后面将要写什么。对于语言模型来说，这确保了它在预测下一个词时，只基于已生成的上下文，从而保持生成过程的自回归特性。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_42.png)

### 工作原理

![](img/edbe8ac55858e949bcfeafd4a78bbd19_44.png)

假设我们有一个包含5个词元的序列。在标准自注意力中，每个词元都可以关注序列中的所有其他词元（包括自身和未来的词元）。计算出的注意力分数矩阵是一个完整的矩阵。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_46.png)

在因果注意力中，我们需要对这个过程进行修改，以确保位置`i`的词元只能关注位置`0`到`i`的词元（包括自身）。这通过应用一个**因果掩码**来实现。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_48.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_50.png)

因果掩码是一个上三角矩阵（主对角线及以上为1，主对角线以下为负无穷大或一个非常大的负数）。在计算注意力分数后，将这个掩码加到分数矩阵上。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_52.png)

以下是应用因果掩码的关键步骤：
1.  **计算注意力分数**：像标准自注意力一样，通过查询向量`Q`和键向量`K`的点积计算原始注意力分数矩阵`S`。`S`的维度为`[序列长度, 序列长度]`。
2.  **应用因果掩码**：创建一个掩码矩阵`M`，其中`M[i, j] = 0` 如果 `j <= i`（允许关注），否则 `M[i, j] = -inf`（禁止关注）。将`M`加到`S`上。
    *   公式表示为：`S_masked = S + M`
3.  **计算注意力权重**：对掩码后的分数矩阵`S_masked`的每一行应用softmax函数。由于被掩码的位置分数为负无穷，经过softmax后其权重变为0。
    *   公式表示为：`Attention Weights = softmax(S_masked, dim=-1)`
4.  **计算上下文向量**：使用得到的注意力权重对值向量`V`进行加权求和，得到最终的上下文向量。

通过这种方式，模型在生成每个词时，其上下文向量仅由它之前（包括自身）的词元信息聚合而成。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_54.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_55.png)

### 代码示例

![](img/edbe8ac55858e949bcfeafd4a78bbd19_57.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_59.png)

以下是一个简化的伪代码，展示因果掩码的应用：

![](img/edbe8ac55858e949bcfeafd4a78bbd19_61.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_63.png)

```python
import torch
import torch.nn.functional as F

# 假设序列长度 seq_len = 5
seq_len = 5
# 1. 计算原始注意力分数 (S)
# S shape: [seq_len, seq_len]
S = ... # 通过 Q @ K.T 计算得出

![](img/edbe8ac55858e949bcfeafd4a78bbd19_65.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_66.png)

# 2. 创建因果掩码 (M)
# 使用 torch.triu 生成上三角矩阵，对角线偏移为1，即主对角线为0，上三角部分为1
mask = torch.triu(torch.ones(seq_len, seq_len), diagonal=1).bool()
# 将 mask 中为 True（即上三角）的位置设置为一个非常大的负数（如 -1e9）
causal_mask = torch.where(mask, torch.tensor(-1e9), torch.tensor(0.0))

![](img/edbe8ac55858e949bcfeafd4a78bbd19_68.png)

# 3. 应用掩码
S_masked = S + causal_mask

# 4. 计算注意力权重
attention_weights = F.softmax(S_masked, dim=-1)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_70.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_71.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_72.png)

# 5. 计算输出
context_vectors = attention_weights @ V
```

## 总结

![](img/edbe8ac55858e949bcfeafd4a78bbd19_74.png)

本节课中，我们一起学习了因果注意力机制。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_76.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_77.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_78.png)

首先，我们回顾了自注意力的核心目标：为每个输入词元生成富含上下文信息的向量。我们指出了直接使用输入嵌入点积的局限性，并回顾了通过可学习的查询、键、值矩阵进行变换以解决该问题的方法。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_80.png)

接着，我们深入探讨了因果注意力。我们了解到，因果注意力是标准自注意力的一个变体，它通过应用一个上三角掩码矩阵，强制模型在生成每个词元时只能关注它之前的词元。这种机制是GPT等自回归语言模型能够进行连贯文本生成的关键。

![](img/edbe8ac55858e949bcfeafd4a78bbd19_82.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_83.png)

![](img/edbe8ac55858e949bcfeafd4a78bbd19_84.png)

下一讲，我们将在此基础上，实现更强大的多头注意力机制。