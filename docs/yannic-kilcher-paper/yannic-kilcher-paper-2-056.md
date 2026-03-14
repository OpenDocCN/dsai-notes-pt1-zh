# 056：具有线性注意力的快速自回归变换器（论文解读）🚀

## 概述

在本节课中，我们将要学习一篇名为《Transformers are RNNs: Fast Autoregressive Transformers with Linear Attention》的论文。这篇论文由 Angelos Katharopoulos、Apoorv Vyas、Nikolaos Pappas 和 François Fleuret 共同撰写。论文的核心观点是，将 Transformer 中的注意力机制解释为一个核函数，从而推导出一种线性注意力机制。这种线性 Transformer 的计算速度比经典 Transformer 快数个数量级。论文还指出，在自回归 Transformer 的场景下，这种线性 Transformer 本质上等同于一种特殊的循环神经网络（RNN）。

---

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_1.png)

## 经典 Transformer 的问题

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_2.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_4.png)

上一节我们介绍了论文的核心贡献。本节中，我们来看看经典 Transformer 模型存在什么问题。我已经制作了许多关于 Transformer 的视频。如果你不了解 Transformer 是什么，可以参考我关于《Attention Is All You Need》那篇原始论文的解读视频。Transformer 模型，特别是其注意力机制，在序列处理任务中取得了巨大成功，例如 BERT、GPT 等模型。

本质上，Transformer 的核心是注意力机制。假设我们有一个输入序列，例如一段文本“An ice cream cone”。在语言建模任务中，模型的目标是根据给定的文本预测下一个词。这种根据已有内容预测后续内容的方式被称为“自回归”。

在 Transformer 的每一层中，每个输入词元（token）都会生成三个向量：**键（Key）**、**查询（Query）** 和 **值（Value）**。键可以看作附着在输入层，查询附着在输出层，而值则是实际被传递的信息。

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_6.png)

每个词元需要从序列中的所有其他词元那里聚合信息。例如，一个代词词元会非常关注句子中的命名实体，以确定其所指。信息聚合的方式如下：对于当前词元的查询向量，我们计算它与序列中所有词元的键向量的内积。然后，将这些内积结果通过一个 **softmax** 函数进行归一化，得到一个概率分布。这个分布决定了当前词元应该从其他每个词元那里获取多少信息。最后，用这个分布对所有的值向量进行加权求和，得到当前词元的输出。

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_7.png)

这个过程的问题在于其**二次复杂度**。如果序列长度为 `n`，那么计算所有查询和键之间的内积就需要 `n * n` 次操作，并且需要存储所有这些中间结果。这种高计算和内存成本很大程度上源于 softmax 函数的非线性特性。softmax 要求我们必须计算并存储所有词元对之间的得分，然后才能进行归一化。

---

## 线性 Transformer 的解决方案

上一节我们指出了经典 Transformer 的复杂度问题。本节中，我们来看看论文提出的线性 Transformer 如何解决这个问题。

论文从一个更宏观的视角开始，将 Transformer 层描述为一个包含残差连接的结构。但核心创新在于对注意力机制的重新表述。

论文的关键洞察是将注意力机制中的相似度计算（查询与键的内积后接 softmax）视为一个**核函数**。具体来说，经典注意力公式可以写为：

**公式：**
`Attention(Q, K, V) = softmax(Q * K^T) * V`

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_9.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_10.png)

其中，`Q` 是查询矩阵，`K` 是键矩阵，`V` 是值矩阵。

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_12.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_13.png)

论文提出，如果我们使用一个特征映射函数 `φ(·)` 将查询和键映射到另一个空间，并利用核函数的性质 `sim(x, y) = φ(x)^T φ(y)`，那么注意力计算可以重写为：

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_15.png)

**公式：**
`LinearAttention(Q, K, V) = (φ(Q) * (φ(K)^T * V)) / (φ(Q) * (φ(K)^T * 1))`

这里的除法是逐元素进行的。这个公式的巧妙之处在于，我们可以改变计算顺序。经典 Transformer 的计算顺序是 `(Q*K^T)*V`，复杂度为 O(n²d)，其中 `n` 是序列长度，`d` 是特征维度。而在线性注意力中，我们可以先计算 `φ(K)^T * V`，这是一个形状为 `(d', d_v)` 的矩阵（`d'` 是映射后的维度），然后再与 `φ(Q)` 相乘。这样，总复杂度就变成了 **O(n * d' * d_v)**。只要 `d'` 和 `d_v` 远小于 `n`，计算效率就会大幅提升。

更重要的是，**移除了 softmax** 使得整个计算过程变成了**线性**的。线性计算允许我们像 RNN 一样进行增量计算，这对于自回归生成任务至关重要。

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_17.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_18.png)

---

## 线性 Transformer 与 RNN 的等价性

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_20.png)

上一节我们介绍了线性注意力的高效计算公式。本节中，我们来看看这种线性特性如何使得 Transformer 在自回归模式下等价于 RNN。

在自回归任务中（如文本生成），模型逐个生成词元。在生成第 `t` 个词元时，模型只能看到前 `t-1` 个词元。对于经典 Transformer，这通常通过一个掩码（mask）来实现，阻止模型“看到”未来的信息，但其计算仍然需要为所有已生成的词元重新计算注意力，复杂度随着生成长度线性增长（O(t²)）。

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_22.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_23.png)

然而，对于线性 Transformer，由于其注意力计算是线性的，我们可以维护一个**状态向量**。这个状态向量聚合了所有历史键和值的信息。具体来说，我们可以定义两个累积状态：

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_25.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_26.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_27.png)

**公式：**
`S_t = Σ_{i=1}^{t} φ(k_i)^T * v_i`
`Z_t = Σ_{i=1}^{t} φ(k_i)^T`

其中，`k_i` 和 `v_i` 是第 `i` 个位置的键和值向量，`φ` 是特征映射函数。

那么，在生成第 `t+1` 个词元时，其注意力输出可以计算为：

**公式：**
`output_{t+1} = (φ(q_{t+1}) * S_t) / (φ(q_{t+1}) * Z_t)`

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_29.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_30.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_31.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_32.png)

然后，我们再用新生成的 `k_{t+1}` 和 `v_{t+1}` 更新状态：
`S_{t+1} = S_t + φ(k_{t+1})^T * v_{t+1`
`Z_{t+1} = Z_t + φ(k_{t+1})^T`

这个过程与**循环神经网络（RNN）** 的工作方式完全一致：
*   **隐藏状态**：`S_t` 和 `Z_t` 就是 RNN 的隐藏状态。
*   **输入**：当前时间步的查询 `q_{t+1}`、键 `k_{t+1}` 和值 `v_{t+1}`。
*   **输出**：`output_{t+1}`。
*   **状态更新**：根据新输入更新隐藏状态 `S` 和 `Z`。

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_34.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_35.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_36.png)

因此，线性自回归 Transformer 在推理时可以被视为一个特殊的 RNN，其**计算复杂度是常数 O(1)**，与已生成长度无关，只与模型维度有关。这解决了 Transformer 在长序列生成时推理速度慢的痛点。

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_38.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_39.png)

---

## 总结

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_41.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_42.png)

本节课中我们一起学习了《Transformers are RNNs》这篇论文的核心思想。

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_44.png)

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_46.png)

我们首先回顾了经典 Transformer 注意力机制的原理及其存在的**二次复杂度问题**。接着，论文通过将注意力解释为核函数，提出了**线性注意力**机制，通过改变计算顺序将复杂度降低为线性。最后，我们深入探讨了这种线性特性在自回归生成任务中的巨大优势：它使得 Transformer 能够以**循环神经网络（RNN）** 的方式进行增量推理，仅需维护一个固定大小的状态向量，从而实现了**常数时间**的每一步生成，极大地提升了长序列生成的效率。

![](img/245f91c13fdb65fdf9c4fb96c3e3e7b8_48.png)

这篇论文为高效序列建模提供了一个重要的思路，巧妙地在 Transformer 的强大表达能力和 RNN 的推理效率之间架起了桥梁。