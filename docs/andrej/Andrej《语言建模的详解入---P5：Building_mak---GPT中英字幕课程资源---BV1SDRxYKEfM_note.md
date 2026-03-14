# 语言建模详解入门：5：构建 WaveNet 🏗️

![](img/c2c4c553adf638a00076095ae2876499_1.png)

![](img/c2c4c553adf638a00076095ae2876499_3.png)

![](img/c2c4c553adf638a00076095ae2876499_4.png)

![](img/c2c4c553adf638a00076095ae2876499_5.png)

![](img/c2c4c553adf638a00076095ae2876499_7.png)

![](img/c2c4c553adf638a00076095ae2876499_8.png)

![](img/c2c4c553adf638a00076095ae2876499_10.png)

在本节课中，我们将继续实现 Makemore 字符级语言模型。我们将对之前的多层感知机架构进行复杂化，目标是构建一个更深层的模型，以渐进式地融合输入序列中的信息，最终形成一个类似于 **WaveNet** 的架构。

![](img/c2c4c553adf638a00076095ae2876499_11.png)

![](img/c2c4c553adf638a00076095ae2876499_12.png)

![](img/c2c4c553adf638a00076095ae2876499_14.png)

![](img/c2c4c553adf638a00076095ae2876499_16.png)

![](img/c2c4c553adf638a00076095ae2876499_18.png)

---

![](img/c2c4c553adf638a00076095ae2876499_20.png)

![](img/c2c4c553adf638a00076095ae2876499_21.png)

![](img/c2c4c553adf638a00076095ae2876499_22.png)

![](img/c2c4c553adf638a00076095ae2876499_24.png)

![](img/c2c4c553adf638a00076095ae2876499_26.png)

![](img/c2c4c553adf638a00076095ae2876499_27.png)

## 概述 📋

![](img/c2c4c553adf638a00076095ae2876499_29.png)

![](img/c2c4c553adf638a00076095ae2876499_31.png)

![](img/c2c4c553adf638a00076095ae2876499_32.png)

![](img/c2c4c553adf638a00076095ae2876499_34.png)

![](img/c2c4c553adf638a00076095ae2876499_35.png)

![](img/c2c4c553adf638a00076095ae2876499_37.png)

![](img/c2c4c553adf638a00076095ae2876499_38.png)

![](img/c2c4c553adf638a00076095ae2876499_39.png)

![](img/c2c4c553adf638a00076095ae2876499_40.png)

上一节我们实现了一个简单的多层感知机模型，它接收三个字符来预测第四个字符。本节我们将扩展这个模型，使其能够处理更长的上下文（例如8个字符），并通过分层结构更有效地融合信息，而不是将所有信息一次性压缩到一个隐藏层中。

![](img/c2c4c553adf638a00076095ae2876499_42.png)

![](img/c2c4c553adf638a00076095ae2876499_44.png)

![](img/c2c4c553adf638a00076095ae2876499_46.png)

![](img/c2c4c553adf638a00076095ae2876499_48.png)

![](img/c2c4c553adf638a00076095ae2876499_50.png)

![](img/c2c4c553adf638a00076095ae2876499_51.png)

![](img/c2c4c553adf638a00076095ae2876499_53.png)

---

![](img/c2c4c553adf638a00076095ae2876499_54.png)

![](img/c2c4c553adf638a00076095ae2876499_56.png)

![](img/c2c4c553adf638a00076095ae2876499_57.png)

![](img/c2c4c553adf638a00076095ae2876499_59.png)

![](img/c2c4c553adf638a00076095ae2876499_61.png)

![](img/c2c4c553adf638a00076095ae2876499_62.png)

![](img/c2c4c553adf638a00076095ae2876499_64.png)

![](img/c2c4c553adf638a00076095ae2876499_66.png)

![](img/c2c4c553adf638a00076095ae2876499_68.png)

## 从现有代码开始

我们的起始代码与第三部分结束时非常相似。我们导入了必要的库，读取了单词数据集，并将每个单词处理成多个“给定三个字符预测第四个字符”的示例。目前我们有大约 182,000 个这样的训练示例。

![](img/c2c4c553adf638a00076095ae2876499_70.png)

![](img/c2c4c553adf638a00076095ae2876499_72.png)

![](img/c2c4c553adf638a00076095ae2876499_74.png)

![](img/c2c4c553adf638a00076095ae2876499_75.png)

![](img/c2c4c553adf638a00076095ae2876499_77.png)

我们之前已经构建了一些模块化的层，例如 `Linear`、`BatchNorm1d` 和 `Tanh`，它们的 API 设计模仿了 PyTorch 的风格。这些层像乐高积木一样，可以堆叠起来构建神经网络。

![](img/c2c4c553adf638a00076095ae2876499_79.png)

![](img/c2c4c553adf638a00076095ae2876499_81.png)

![](img/c2c4c553adf638a00076095ae2876499_83.png)

![](img/c2c4c553adf638a00076095ae2876499_85.png)

![](img/c2c4c553adf638a00076095ae2876499_87.png)

![](img/c2c4c553adf638a00076095ae2876499_89.png)

### 现有架构的问题

![](img/c2c4c553adf638a00076095ae2876499_91.png)

![](img/c2c4c553adf638a00076095ae2876499_92.png)

![](img/c2c4c553adf638a00076095ae2876499_93.png)

![](img/c2c4c553adf638a00076095ae2876499_95.png)

![](img/c2c4c553adf638a00076095ae2876499_97.png)

![](img/c2c4c553adf638a00076095ae2876499_98.png)

![](img/c2c4c553adf638a00076095ae2876499_100.png)

![](img/c2c4c553adf638a00076095ae2876499_102.png)

当前的模型架构如下：
*   输入：3个字符的索引。
*   嵌入层：将每个索引转换为一个10维向量。
*   展平层：将所有向量拼接成一个长向量（例如，3*10=30维）。
*   线性层 + BatchNorm + Tanh：一个隐藏层。
*   输出线性层：预测下一个字符的概率分布。

![](img/c2c4c553adf638a00076095ae2876499_104.png)

![](img/c2c4c553adf638a00076095ae2876499_106.png)

![](img/c2c4c553adf638a00076095ae2876499_108.png)

![](img/c2c4c553adf638a00076095ae2876499_109.png)

![](img/c2c4c553adf638a00076095ae2876499_111.png)

![](img/c2c4c553adf638a00076095ae2876499_113.png)

![](img/c2c4c553adf638a00076095ae2876499_114.png)

![](img/c2c4c553adf638a00076095ae2876499_115.png)

这种架构的问题在于，它将所有输入字符的信息立即压缩到一个单一的隐藏层中，这可能不是最有效的信息利用方式。

![](img/c2c4c553adf638a00076095ae2876499_117.png)

![](img/c2c4c553adf638a00076095ae2876499_118.png)

![](img/c2c4c553adf638a00076095ae2876499_120.png)

![](img/c2c4c553adf638a00076095ae2876499_122.png)

![](img/c2c4c553adf638a00076095ae2876499_124.png)

![](img/c2c4c553adf638a00076095ae2876499_125.png)

![](img/c2c4c553adf638a00076095ae2876499_127.png)

---

![](img/c2c4c553adf638a00076095ae2876499_129.png)

![](img/c2c4c553adf638a00076095ae2876499_131.png)

![](img/c2c4c553adf638a00076095ae2876499_133.png)

![](img/c2c4c553adf638a00076095ae2876499_135.png)

![](img/c2c4c553adf638a00076095ae2876499_137.png)

## 改进代码结构

![](img/c2c4c553adf638a00076095ae2876499_139.png)

![](img/c2c4c553adf638a00076095ae2876499_140.png)

![](img/c2c4c553adf638a00076095ae2876499_142.png)

![](img/c2c4c553adf638a00076095ae2876499_144.png)

![](img/c2c4c553adf638a00076095ae2876499_146.png)

![](img/c2c4c553adf638a00076095ae2876499_148.png)

![](img/c2c4c553adf638a00076095ae2876499_149.png)

在深入新架构之前，我们先优化一下代码结构，使其更清晰、更接近 PyTorch 风格。

![](img/c2c4c553adf638a00076095ae2876499_151.png)

![](img/c2c4c553adf638a00076095ae2876499_152.png)

![](img/c2c4c553adf638a00076095ae2876499_153.png)

![](img/c2c4c553adf638a00076095ae2876499_154.png)

![](img/c2c4c553adf638a00076095ae2876499_155.png)

![](img/c2c4c553adf638a00076095ae2876499_157.png)

![](img/c2c4c553adf638a00076095ae2876499_158.png)

![](img/c2c4c553adf638a00076095ae2876499_160.png)

![](img/c2c4c553adf638a00076095ae2876499_162.png)

![](img/c2c4c553adf638a00076095ae2876499_164.png)

![](img/c2c4c553adf638a00076095ae2876499_165.png)

### 1. 创建 Embedding 和 Flatten 模块

![](img/c2c4c553adf638a00076095ae2876499_167.png)

![](img/c2c4c553adf638a00076095ae2876499_169.png)

我们将嵌入表查找和展平操作也封装成模块（`Embedding` 和 `Flatten`），并将它们加入到层的序列中，这样前向传播的逻辑会更加简洁。

![](img/c2c4c553adf638a00076095ae2876499_171.png)

**代码示例：Embedding 模块**
```python
class Embedding:
    def __init__(self, num_embeddings, embedding_dim):
        self.weight = torch.randn(num_embeddings, embedding_dim)

    def forward(self, idx):
        return self.weight[idx] # 索引查找
```

![](img/c2c4c553adf638a00076095ae2876499_173.png)

![](img/c2c4c553adf638a00076095ae2876499_174.png)

![](img/c2c4c553adf638a00076095ae2876499_176.png)

![](img/c2c4c553adf638a00076095ae2876499_178.png)

![](img/c2c4c553adf638a00076095ae2876499_179.png)

![](img/c2c4c553adf638a00076095ae2876499_181.png)

![](img/c2c4c553adf638a00076095ae2876499_182.png)

**代码示例：Flatten 模块**
```python
class Flatten:
    def forward(self, x):
        return x.view(x.shape[0], -1) # 展平除批次维度外的所有维度
```

![](img/c2c4c553adf638a00076095ae2876499_184.png)

![](img/c2c4c553adf638a00076095ae2876499_186.png)

![](img/c2c4c553adf638a00076095ae2876499_187.png)

![](img/c2c4c553adf638a00076095ae2876499_188.png)

![](img/c2c4c553adf638a00076095ae2876499_190.png)

![](img/c2c4c553adf638a00076095ae2876499_191.png)

### 2. 引入 Sequential 容器

![](img/c2c4c553adf638a00076095ae2876499_193.png)

![](img/c2c4c553adf638a00076095ae2876499_195.png)

![](img/c2c4c553adf638a00076095ae2876499_197.png)

![](img/c2c4c553adf638a00076095ae2876499_199.png)

![](img/c2c4c553adf638a00076095ae2876499_201.png)

![](img/c2c4c553adf638a00076095ae2876499_202.png)

我们创建一个 `Sequential` 容器来管理一系列层，这进一步简化了模型的定义和前向传播过程。

![](img/c2c4c553adf638a00076095ae2876499_204.png)

![](img/c2c4c553adf638a00076095ae2876499_206.png)

![](img/c2c4c553adf638a00076095ae2876499_208.png)

![](img/c2c4c553adf638a00076095ae2876499_210.png)

**代码示例：Sequential 容器**
```python
class Sequential:
    def __init__(self, layers):
        self.layers = layers

    def forward(self, x):
        for layer in self.layers:
            x = layer.forward(x)
        return x
```

![](img/c2c4c553adf638a00076095ae2876499_212.png)

![](img/c2c4c553adf638a00076095ae2876499_214.png)

![](img/c2c4c553adf638a00076095ae2876499_215.png)

经过这些重构，我们的模型定义和前向传播变得非常简洁：
```python
model = Sequential([
    Embedding(vocab_size, n_embd),
    Flatten(),
    Linear(n_embd * block_size, n_hidden), BatchNorm1d(n_hidden), Tanh(),
    Linear(n_hidden, vocab_size)
])

![](img/c2c4c553adf638a00076095ae2876499_217.png)

logits = model.forward(xb)
loss = F.cross_entropy(logits, yb)
```

![](img/c2c4c553adf638a00076095ae2876499_219.png)

---

![](img/c2c4c553adf638a00076095ae2876499_221.png)

![](img/c2c4c553adf638a00076095ae2876499_223.png)

![](img/c2c4c553adf638a00076095ae2876499_224.png)

![](img/c2c4c553adf638a00076095ae2876499_225.png)

![](img/c2c4c553adf638a00076095ae2876499_227.png)

![](img/c2c4c553adf638a00076095ae2876499_229.png)

## 扩展上下文长度

![](img/c2c4c553adf638a00076095ae2876499_231.png)

![](img/c2c4c553adf638a00076095ae2876499_233.png)

![](img/c2c4c553adf638a00076095ae2876499_234.png)

作为改进的第一步，我们将上下文长度从3增加到8。这意味着模型现在需要根据前8个字符来预测第9个字符。仅仅增加上下文长度（并相应调整第一层线性层的输入大小）就能带来性能提升，验证损失从约2.10降至2.02。

![](img/c2c4c553adf638a00076095ae2876499_236.png)

![](img/c2c4c553adf638a00076095ae2876499_238.png)

但这仍然是“一次性压缩”所有信息的架构。我们的目标是实现分层融合。

![](img/c2c4c553adf638a00076095ae2876499_240.png)

![](img/c2c4c553adf638a00076095ae2876499_242.png)

---

![](img/c2c4c553adf638a00076095ae2876499_244.png)

![](img/c2c4c553adf638a00076095ae2876499_246.png)

![](img/c2c4c553adf638a00076095ae2876499_248.png)

![](img/c2c4c553adf638a00076095ae2876499_250.png)

![](img/c2c4c553adf638a00076095ae2876499_252.png)

![](img/c2c4c553adf638a00076095ae2876499_254.png)

## 实现分层（树状）融合架构 🎄

![](img/c2c4c553adf638a00076095ae2876499_256.png)

![](img/c2c4c553adf638a00076095ae2876499_257.png)

![](img/c2c4c553adf638a00076095ae2876499_258.png)

![](img/c2c4c553adf638a00076095ae2876499_260.png)

![](img/c2c4c553adf638a00076095ae2876499_262.png)

![](img/c2c4c553adf638a00076095ae2876499_264.png)

![](img/c2c4c553adf638a00076095ae2876499_266.png)

![](img/c2c4c553adf638a00076095ae2876499_268.png)

![](img/c2c4c553adf638a00076095ae2876499_270.png)

WaveNet 的核心思想是渐进式融合：不是一次性处理所有字符，而是先融合相邻的字符对（形成双字符表示），然后融合这些双字符对（形成四字符表示），以此类推，形成一个树状结构。

![](img/c2c4c553adf638a00076095ae2876499_271.png)

![](img/c2c4c553adf638a00076095ae2876499_273.png)

![](img/c2c4c553adf638a00076095ae2876499_275.png)

![](img/c2c4c553adf638a00076095ae2876499_277.png)

### 1. 修改 Flatten 层

![](img/c2c4c553adf638a00076095ae2876499_279.png)

![](img/c2c4c553adf638a00076095ae2876499_281.png)

![](img/c2c4c553adf638a00076095ae2876499_282.png)

![](img/c2c4c553adf638a00076095ae2876499_283.png)

![](img/c2c4c553adf638a00076095ae2876499_285.png)

![](img/c2c4c553adf638a00076095ae2876499_286.png)

为了实现这一点，我们需要修改 `Flatten` 层。新的 `FlattenConsecutive` 层不是将所有字符的嵌入向量展平成一个长向量，而是将每 `n` 个连续的向量拼接起来，并增加一个“组”的维度。

![](img/c2c4c553adf638a00076095ae2876499_288.png)

![](img/c2c4c553adf638a00076095ae2876499_289.png)

![](img/c2c4c553adf638a00076095ae2876499_290.png)

![](img/c2c4c553adf638a00076095ae2876499_291.png)

![](img/c2c4c553adf638a00076095ae2876499_293.png)

**代码示例：FlattenConsecutive 层**
```python
class FlattenConsecutive:
    def __init__(self, n):
        self.n = n

    def forward(self, x):
        # x 形状: (batch_size, time_steps, channels)
        B, T, C = x.shape
        x = x.view(B, T // self.n, C * self.n)
        if x.shape[1] == 1: # 如果只剩一个组，则压缩掉该维度
            x = x.squeeze(1)
        return x
```
当 `n=2` 时，该层将形状为 `(B, 8, C)` 的输入转换为 `(B, 4, C*2)`。

![](img/c2c4c553adf638a00076095ae2876499_295.png)

![](img/c2c4c553adf638a00076095ae2876499_297.png)

### 2. 构建分层网络

![](img/c2c4c553adf638a00076095ae2876499_299.png)

![](img/c2c4c553adf638a00076095ae2876499_301.png)

![](img/c2c4c553adf638a00076095ae2876499_303.png)

![](img/c2c4c553adf638a00076095ae2876499_305.png)

利用新的 `FlattenConsecutive` 层，我们可以构建一个三层网络来实现初步的层次化融合：

![](img/c2c4c553adf638a00076095ae2876499_307.png)

![](img/c2c4c553adf638a00076095ae2876499_309.png)

![](img/c2c4c553adf638a00076095ae2876499_311.png)

1.  **第一层**：融合每两个连续的字符（`FlattenConsecutive(2)`），然后通过一个线性层处理这些“双字符”表示。
2.  **第二层**：再次融合（`FlattenConsecutive(2)`），将“双字符”表示融合成“四字符”表示，并通过另一个线性层。
3.  **第三层及输出**：继续处理并最终预测下一个字符。

![](img/c2c4c553adf638a00076095ae2876499_313.png)

![](img/c2c4c553adf638a00076095ae2876499_315.png)

![](img/c2c4c553adf638a00076095ae2876499_317.png)

![](img/c2c4c553adf638a00076095ae2876499_319.png)

![](img/c2c4c553adf638a00076095ae2876499_321.png)

以下是网络结构示例：
```python
model = Sequential([
    Embedding(vocab_size, n_embd),
    FlattenConsecutive(2), Linear(n_embd * 2, n_hidden), BatchNorm1d(n_hidden), Tanh(),
    FlattenConsecutive(2), Linear(n_hidden * 2, n_hidden), BatchNorm1d(n_hidden), Tanh(),
    FlattenConsecutive(2), Linear(n_hidden * 2, n_hidden), BatchNorm1d(n_hidden), Tanh(),
    Linear(n_hidden, vocab_size)
])
```

![](img/c2c4c553adf638a00076095ae2876499_323.png)

![](img/c2c4c553adf638a00076095ae2876499_325.png)

![](img/c2c4c553adf638a00076095ae2876499_326.png)

![](img/c2c4c553adf638a00076095ae2876499_327.png)

![](img/c2c4c553adf638a00076095ae2876499_328.png)

### 3. 修复 BatchNorm1d 以支持3D输入

![](img/c2c4c553adf638a00076095ae2876499_330.png)

![](img/c2c4c553adf638a00076095ae2876499_332.png)

我们的 `BatchNorm1d` 层最初是为2D输入 `(batch_size, features)` 设计的。现在，在分层网络中，输入可能变成3D `(batch_size, groups, features)`。我们需要修改它，使其在计算均值和方差时，将前两个维度都视为批次维度。

![](img/c2c4c553adf638a00076095ae2876499_334.png)

![](img/c2c4c553adf638a00076095ae2876499_335.png)

![](img/c2c4c553adf638a00076095ae2876499_336.png)

![](img/c2c4c553adf638a00076095ae2876499_337.png)

![](img/c2c4c553adf638a00076095ae2876499_338.png)

![](img/c2c4c553adf638a00076095ae2876499_339.png)

![](img/c2c4c553adf638a00076095ae2876499_341.png)

![](img/c2c4c553adf638a00076095ae2876499_342.png)

![](img/c2c4c553adf638a00076095ae2876499_343.png)

![](img/c2c4c553adf638a00076095ae2876499_345.png)

**代码示例：修复后的 BatchNorm1d**
```python
class BatchNorm1d:
    def forward(self, x):
        if self.training:
            dims = (0, 1) if x.ndim == 3 else (0) # 根据输入维度决定在哪些维度上求均值/方差
            xmean = x.mean(dims, keepdim=True)
            xvar = x.var(dims, keepdim=True)
            # ... 更新 running mean/var ...
        # ... 归一化 ...
```
这个修改确保了批归一化在多个“组”上也能正确工作，利用了更多数据来估计统计量，使训练更稳定。

![](img/c2c4c553adf638a00076095ae2876499_347.png)

![](img/c2c4c553adf638a00076095ae2876499_349.png)

![](img/c2c4c553adf638a00076095ae2876499_351.png)

![](img/c2c4c553adf638a00076095ae2876499_353.png)

---

![](img/c2c4c553adf638a00076095ae2876499_355.png)

![](img/c2c4c553adf638a00076095ae2876499_357.png)

## 性能与总结 📊

![](img/c2c4c553adf638a00076095ae2876499_359.png)

通过实现分层融合架构并修复 BatchNorm 的 bug，我们在控制参数数量大致相同的情况下，获得了轻微的验证损失提升（从 ~2.03 降至 ~2.02）。随后，通过增加嵌入维度和隐藏层大小来扩大模型规模，我们成功将验证损失降低到了 **1.993**，突破了 2.0 的大关。

![](img/c2c4c553adf638a00076095ae2876499_361.png)

![](img/c2c4c553adf638a00076095ae2876499_362.png)

![](img/c2c4c553adf638a00076095ae2876499_363.png)

![](img/c2c4c553adf638a00076095ae2876499_364.png)

本节课我们一起学习了以下内容：

![](img/c2c4c553adf638a00076095ae2876499_366.png)

![](img/c2c4c553adf638a00076095ae2876499_367.png)

![](img/c2c4c553adf638a00076095ae2876499_368.png)

![](img/c2c4c553adf638a00076095ae2876499_369.png)

![](img/c2c4c553adf638a00076095ae2876499_370.png)

![](img/c2c4c553adf638a00076095ae2876499_371.png)

![](img/c2c4c553adf638a00076095ae2876499_372.png)

![](img/c2c4c553adf638a00076095ae2876499_374.png)

![](img/c2c4c553adf638a00076095ae2876499_375.png)

![](img/c2c4c553adf638a00076095ae2876499_376.png)

![](img/c2c4c553adf638a00076095ae2876499_378.png)

1.  **代码重构**：我们将模型组件进一步模块化，引入了 `Embedding`、`Flatten`（及 `FlattenConsecutive`）模块和 `Sequential` 容器，使代码更清晰、更易于扩展。
2.  **分层融合思想**：我们理解了 WaveNet 架构的核心——通过树状结构渐进式地融合长序列上下文信息，并手动实现了这种结构的简化版本。
3.  **张量形状操作**：我们深入使用了 `.view()`、`.mean(dim=...)` 等操作来处理多维张量，这是构建复杂神经网络的基础技能。
4.  **调试与适配**：我们修复了 `BatchNorm1d` 层以兼容3D输入，这是一个典型的在调整网络结构时遇到的层间接口适配问题。

![](img/c2c4c553adf638a00076095ae2876499_380.png)

![](img/c2c4c553adf638a00076095ae2876499_381.png)

![](img/c2c4c553adf638a00076095ae2876499_382.png)

![](img/c2c4c553adf638a00076095ae2876499_384.png)

### 未来方向 🚀

![](img/c2c4c553adf638a00076095ae2876499_386.png)

![](img/c2c4c553adf638a00076095ae2876499_388.png)

![](img/c2c4c553adf638a00076095ae2876499_389.png)

![](img/c2c4c553adf638a00076095ae2876499_390.png)

![](img/c2c4c553adf638a00076095ae2876499_392.png)

![](img/c2c4c553adf638a00076095ae2876499_393.png)

![](img/c2c4c553adf638a00076095ae2876499_394.png)

我们当前的实现与真正的 WaveNet 还有距离，并且缺乏系统的实验流程。未来的课程可能会涵盖：
*   **卷积神经网络**：使用**扩张因果卷积**来高效实现我们当前的分层融合结构，避免冗余计算。
*   **残差连接与门控机制**：实现 WaveNet 论文中的残差块和门控线性单元，以提升训练深度网络的能力。
*   **实验框架**：建立超参数搜索和实验跟踪的体系，以科学地优化模型。
*   **其他架构**：探索循环神经网络（RNN、LSTM、GRU）以及如今占主导地位的 Transformer 架构。

![](img/c2c4c553adf638a00076095ae2876499_396.png)

![](img/c2c4c553adf638a00076095ae2876499_397.png)

![](img/c2c4c553adf638a00076095ae2876499_398.png)

![](img/c2c4c553adf638a00076095ae2876499_399.png)

![](img/c2c4c553adf638a00076095ae2876499_400.png)

![](img/c2c4c553adf638a00076095ae2876499_402.png)

![](img/c2c4c553adf638a00076095ae2876499_403.png)

![](img/c2c4c553adf638a00076095ae2876499_404.png)

你现在已经掌握了用模块化组件构建和调试深度神经网络的基本方法。尝试调整网络深度、宽度、融合因子（`n`）等超参数，看看能否进一步降低损失吧！