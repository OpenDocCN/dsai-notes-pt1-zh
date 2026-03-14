# P6：构建 Makemore 第五部分：构建一个 WaveNet 🧠

![](img/c9be2506944533fbac16b5c8db99ea71_1.png)

![](img/c9be2506944533fbac16b5c8db99ea71_3.png)

![](img/c9be2506944533fbac16b5c8db99ea71_5.png)

![](img/c9be2506944533fbac16b5c8db99ea71_7.png)

![](img/c9be2506944533fbac16b5c8db99ea71_9.png)

![](img/c9be2506944533fbac16b5c8db99ea71_11.png)

![](img/c9be2506944533fbac16b5c8db99ea71_13.png)

![](img/c9be2506944533fbac16b5c8db99ea71_15.png)

![](img/c9be2506944533fbac16b5c8db99ea71_17.png)

![](img/c9be2506944533fbac16b5c8db99ea71_19.png)

![](img/c9be2506944533fbac16b5c8db99ea71_21.png)

![](img/c9be2506944533fbac16b5c8db99ea71_23.png)

![](img/c9be2506944533fbac16b5c8db99ea71_25.png)

在本节课中，我们将继续完善我们最喜爱的字符级语言模型。我们将从之前构建的多层感知器（MLP）架构出发，将其复杂化，以处理更长的输入序列，并构建一个更深、能逐步融合信息的模型。最终，我们将实现一个类似于 **WaveNet** 的层次化架构。

![](img/c9be2506944533fbac16b5c8db99ea71_27.png)

![](img/c9be2506944533fbac16b5c8db99ea71_29.png)

![](img/c9be2506944533fbac16b5c8db99ea71_31.png)

![](img/c9be2506944533fbac16b5c8db99ea71_33.png)

![](img/c9be2506944533fbac16b5c8db99ea71_35.png)

![](img/c9be2506944533fbac16b5c8db99ea71_37.png)

![](img/c9be2506944533fbac16b5c8db99ea71_39.png)

## 概述

![](img/c9be2506944533fbac16b5c8db99ea71_41.png)

![](img/c9be2506944533fbac16b5c8db99ea71_43.png)

![](img/c9be2506944533fbac16b5c8db99ea71_45.png)

![](img/c9be2506944533fbac16b5c8db99ea71_47.png)

![](img/c9be2506944533fbac16b5c8db99ea71_49.png)

![](img/c9be2506944533fbac16b5c8db99ea71_51.png)

![](img/c9be2506944533fbac16b5c8db99ea71_53.png)

![](img/c9be2506944533fbac16b5c8db99ea71_55.png)

![](img/c9be2506944533fbac16b5c8db99ea71_57.png)

在之前的课程中，我们构建了一个基于三层感知器的字符级语言模型。它接收三个先前的字符，并尝试预测序列中的第四个字符。本节课的目标是扩展这个模型，使其能够处理更长的上下文（例如8个字符），并通过一个更深的、树状层次化的结构来逐步融合信息，而不是一次性将所有信息压缩到一个隐藏层中。这种架构灵感来源于2016年的 **WaveNet** 论文，它本质上也是一个自回归模型，用于预测序列中的下一个元素。

![](img/c9be2506944533fbac16b5c8db99ea71_59.png)

![](img/c9be2506944533fbac16b5c8db99ea71_61.png)

![](img/c9be2506944533fbac16b5c8db99ea71_63.png)

![](img/c9be2506944533fbac16b5c8db99ea71_65.png)

![](img/c9be2506944533fbac16b5c8db99ea71_67.png)

![](img/c9be2506944533fbac16b5c8db99ea71_69.png)

![](img/c9be2506944533fbac16b5c8db99ea71_71.png)

![](img/c9be2506944533fbac16b5c8db99ea71_73.png)

![](img/c9be2506944533fbac16b5c8db99ea71_75.png)

## 从现有代码库开始

![](img/c9be2506944533fbac16b5c8db99ea71_77.png)

![](img/c9be2506944533fbac16b5c8db99ea71_79.png)

![](img/c9be2506944533fbac16b5c8db99ea71_81.png)

![](img/c9be2506944533fbac16b5c8db99ea71_83.png)

![](img/c9be2506944533fbac16b5c8db99ea71_85.png)

![](img/c9be2506944533fbac16b5c8db99ea71_87.png)

我们本节课的起点代码与第三部分结束时的代码非常相似。我们有一个处理好的数据集，包含约18.2万个“给定三个字符预测第四个字符”的示例。我们的代码已经模块化，包含了 `Linear`、`BatchNorm` 等层，其API设计模仿了PyTorch的 `torch.nn` 模块。

![](img/c9be2506944533fbac16b5c8db99ea71_89.png)

![](img/c9be2506944533fbac16b5c8db99ea71_91.png)

![](img/c9be2506944533fbac16b5c8db99ea71_93.png)

![](img/c9be2506944533fbac16b5c8db99ea71_95.png)

![](img/c9be2506944533fbac16b5c8db99ea71_97.png)

首先，我们注意到损失曲线因为批次大小太小而波动剧烈。我们通过将损失列表重塑为二维张量并计算行平均来平滑可视化。

![](img/c9be2506944533fbac16b5c8db99ea71_99.png)

![](img/c9be2506944533fbac16b5c8db99ea71_101.png)

![](img/c9be2506944533fbac16b5c8db99ea71_103.png)

![](img/c9be2506944533fbac16b5c8db99ea71_105.png)

![](img/c9be2506944533fbac16b5c8db99ea71_107.png)

![](img/c9be2506944533fbac16b5c8db99ea71_109.png)

![](img/c9be2506944533fbac16b5c8db99ea71_111.png)

```python
# 将损失列表重塑为二维张量以平滑曲线
losses = torch.tensor(losses).view(-1, 1000).mean(1)
```

![](img/c9be2506944533fbac16b5c8db99ea71_113.png)

![](img/c9be2506944533fbac16b5c8db99ea71_115.png)

![](img/c9be2506944533fbac16b5c8db99ea71_117.png)

![](img/c9be2506944533fbac16b5c8db99ea71_119.png)

![](img/c9be2506944533fbac16b5c8db99ea71_121.png)

![](img/c9be2506944533fbac16b5c8db99ea71_123.png)

![](img/c9be2506944533fbac16b5c8db99ea71_125.png)

![](img/c9be2506944533fbac16b5c8db99ea71_127.png)

## 重构模型：引入更多模块

![](img/c9be2506944533fbac16b5c8db99ea71_129.png)

![](img/c9be2506944533fbac16b5c8db99ea71_131.png)

![](img/c9be2506944533fbac16b5c8db99ea71_133.png)

![](img/c9be2506944533fbac16b5c8db99ea71_135.png)

![](img/c9be2506944533fbac16b5c8db99ea71_137.png)

![](img/c9be2506944533fbac16b5c8db99ea71_139.png)

![](img/c9be2506944533fbac16b5c8db99ea71_141.png)

![](img/c9be2506944533fbac16b5c8db99ea71_143.png)

上一节我们处理了数据可视化问题。本节中，我们将重构模型的前向传播逻辑，使其更加模块化和简洁。

![](img/c9be2506944533fbac16b5c8db99ea71_145.png)

![](img/c9be2506944533fbac16b5c8db99ea71_147.png)

![](img/c9be2506944533fbac16b5c8db99ea71_149.png)

![](img/c9be2506944533fbac16b5c8db99ea71_151.png)

![](img/c9be2506944533fbac16b5c8db99ea71_153.png)

![](img/c9be2506944533fbac16b5c8db99ea71_155.png)

![](img/c9be2506944533fbac16b5c8db99ea71_157.png)

![](img/c9be2506944533fbac16b5c8db99ea71_159.png)

我们之前将嵌入表（`C`）和展平（`view`）操作放在了层列表之外。为了使结构更统一，我们创建了 `Embedding` 和 `Flatten` 模块，并将它们加入到层的序列中。

![](img/c9be2506944533fbac16b5c8db99ea71_161.png)

![](img/c9be2506944533fbac16b5c8db99ea71_163.png)

![](img/c9be2506944533fbac16b5c8db99ea71_165.png)

![](img/c9be2506944533fbac16b5c8db99ea71_167.png)

![](img/c9be2506944533fbac16b5c8db99ea71_169.png)

![](img/c9be2506944533fbac16b5c8db99ea71_171.png)

![](img/c9be2506944533fbac16b5c8db99ea71_173.png)

![](img/c9be2506944533fbac16b5c8db99ea71_175.png)

```python
class Embedding:
    def __init__(self, num_embeddings, embedding_dim):
        self.weight = torch.randn((num_embeddings, embedding_dim))
    def __call__(self, idx):
        return self.weight[idx]

![](img/c9be2506944533fbac16b5c8db99ea71_177.png)

![](img/c9be2506944533fbac16b5c8db99ea71_179.png)

![](img/c9be2506944533fbac16b5c8db99ea71_181.png)

![](img/c9be2506944533fbac16b5c8db99ea71_183.png)

![](img/c9be2506944533fbac16b5c8db99ea71_185.png)

![](img/c9be2506944533fbac16b5c8db99ea71_187.png)

![](img/c9be2506944533fbac16b5c8db99ea71_189.png)

![](img/c9be2506944533fbac16b5c8db99ea71_191.png)

![](img/c9be2506944533fbac16b5c8db99ea71_193.png)

class Flatten:
    def __call__(self, x):
        return x.view(x.shape[0], -1)
```

![](img/c9be2506944533fbac16b5c8db99ea71_195.png)

![](img/c9be2506944533fbac16b5c8db99ea71_197.png)

![](img/c9be2506944533fbac16b5c8db99ea71_199.png)

接着，我们引入 `Sequential` 容器来管理这些层，这进一步简化了代码。`Sequential` 接收一个层列表，并在前向传播中依次调用它们。

![](img/c9be2506944533fbac16b5c8db99ea71_201.png)

![](img/c9be2506944533fbac16b5c8db99ea71_203.png)

![](img/c9be2506944533fbac16b5c8db99ea71_205.png)

![](img/c9be2506944533fbac16b5c8db99ea71_207.png)

![](img/c9be2506944533fbac16b5c8db99ea71_209.png)

![](img/c9be2506944533fbac16b5c8db99ea71_211.png)

![](img/c9be2506944533fbac16b5c8db99ea71_213.png)

![](img/c9be2506944533fbac16b5c8db99ea71_215.png)

```python
class Sequential:
    def __init__(self, layers):
        self.layers = layers
    def __call__(self, x):
        for layer in self.layers:
            x = layer(x)
        return x
```

![](img/c9be2506944533fbac16b5c8db99ea71_217.png)

![](img/c9be2506944533fbac16b5c8db99ea71_219.png)

![](img/c9be2506944533fbac16b5c8db99ea71_221.png)

![](img/c9be2506944533fbac16b5c8db99ea71_223.png)

![](img/c9be2506944533fbac16b5c8db99ea71_225.png)

![](img/c9be2506944533fbac16b5c8db99ea71_227.png)

现在，我们的模型定义和前向传播变得非常清晰：

![](img/c9be2506944533fbac16b5c8db99ea71_229.png)

![](img/c9be2506944533fbac16b5c8db99ea71_231.png)

![](img/c9be2506944533fbac16b5c8db99ea71_233.png)

![](img/c9be2506944533fbac16b5c8db99ea71_235.png)

![](img/c9be2506944533fbac16b5c8db99ea71_237.png)

![](img/c9be2506944533fbac16b5c8db99ea71_239.png)

![](img/c9be2506944533fbac16b5c8db99ea71_241.png)

![](img/c9be2506944533fbac16b5c8db99ea71_243.png)

```python
model = Sequential([
    Embedding(vocab_size, n_embd),
    Flatten(),
    Linear(n_embd * block_size, n_hidden), BatchNorm(n_hidden), Tanh(),
    Linear(n_hidden, vocab_size)
])
logits = model(xb)
```

![](img/c9be2506944533fbac16b5c8db99ea71_245.png)

![](img/c9be2506944533fbac16b5c8db99ea71_247.png)

![](img/c9be2506944533fbac16b5c8db99ea71_249.png)

![](img/c9be2506944533fbac16b5c8db99ea71_251.png)

## 扩展上下文并引入层次化结构

![](img/c9be2506944533fbac16b5c8db99ea71_253.png)

![](img/c9be2506944533fbac16b5c8db99ea71_255.png)

上一节我们重构了模型代码。本节中，我们将扩展模型的上下文长度，并引入WaveNet的核心思想——层次化融合。

![](img/c9be2506944533fbac16b5c8db99ea71_257.png)

![](img/c9be2506944533fbac16b5c8db99ea71_259.png)

![](img/c9be2506944533fbac16b5c8db99ea71_261.png)

![](img/c9be2506944533fbac16b5c8db99ea71_263.png)

![](img/c9be2506944533fbac16b5c8db99ea71_265.png)

![](img/c9be2506944533fbac16b5c8db99ea71_267.png)

首先，我们将输入上下文长度从3增加到8。这立即带来了性能提升（验证损失从 ~2.10 降至 ~2.02），因为模型拥有了更多信息。

![](img/c9be2506944533fbac16b5c8db99ea71_269.png)

![](img/c9be2506944533fbac16b5c8db99ea71_271.png)

![](img/c9be2506944533fbac16b5c8db99ea71_273.png)

但是，简单地将8个字符的嵌入向量展平后送入一个线性层，意味着信息被过早地压缩了。我们想要的是像WaveNet那样，以树状结构逐步融合信息：先融合相邻的两个字符，再融合上一层的两个“组”，以此类推。

![](img/c9be2506944533fbac16b5c8db99ea71_275.png)

![](img/c9be2506944533fbac16b5c8db99ea71_277.png)

![](img/c9be2506944533fbac16b5c8db99ea71_279.png)

为了实现这一点，我们需要修改 `Flatten` 层。我们创建了一个新的 `FlattenConsecutive` 层，它可以将连续的 `n` 个元素拼接在一起，并增加一个“组”的维度。

![](img/c9be2506944533fbac16b5c8db99ea71_281.png)

![](img/c9be2506944533fbac16b5c8db99ea71_283.png)

![](img/c9be2506944533fbac16b5c8db99ea71_285.png)

![](img/c9be2506944533fbac16b5c8db99ea71_287.png)

![](img/c9be2506944533fbac16b5c8db99ea71_289.png)

![](img/c9be2506944533fbac16b5c8db99ea71_291.png)

![](img/c9be2506944533fbac16b5c8db99ea71_293.png)

![](img/c9be2506944533fbac16b5c8db99ea71_295.png)

```python
class FlattenConsecutive:
    def __init__(self, n):
        self.n = n
    def __call__(self, x):
        B, T, C = x.shape
        x = x.view(B, T // self.n, C * self.n)
        if x.shape[1] == 1:
            x = x.squeeze(1)
        return x
```

![](img/c9be2506944533fbac16b5c8db99ea71_297.png)

![](img/c9be2506944533fbac16b5c8db99ea71_299.png)

![](img/c9be2506944533fbac16b5c8db99ea71_301.png)

![](img/c9be2506944533fbac16b5c8db99ea71_303.png)

![](img/c9be2506944533fbac16b5c8db99ea71_305.png)

![](img/c9be2506944533fbac16b5c8db99ea71_307.png)

然后，我们重新设计模型架构。第一层 `FlattenConsecutive(2)` 将8个字符分成4组，每组2个字符的嵌入被拼接。随后的线性层只处理这“2个字符”的信息。之后，我们再次使用 `FlattenConsecutive(2)` 将4组合并为2组，以此类推，形成一个小型的层次化网络。

![](img/c9be2506944533fbac16b5c8db99ea71_309.png)

![](img/c9be2506944533fbac16b5c8db99ea71_311.png)

![](img/c9be2506944533fbac16b5c8db99ea71_313.png)

![](img/c9be2506944533fbac16b5c8db99ea71_315.png)

![](img/c9be2506944533fbac16b5c8db99ea71_317.png)

![](img/c9be2506944533fbac16b5c8db99ea71_319.png)

```python
model = Sequential([
    Embedding(vocab_size, n_embd),
    FlattenConsecutive(2), Linear(n_embd * 2, n_hidden), BatchNorm(n_hidden), Tanh(),
    FlattenConsecutive(2), Linear(n_hidden * 2, n_hidden), BatchNorm(n_hidden), Tanh(),
    FlattenConsecutive(2), Linear(n_hidden * 2, n_hidden), BatchNorm(n_hidden), Tanh(),
    Linear(n_hidden, vocab_size)
])
```

![](img/c9be2506944533fbac16b5c8db99ea71_321.png)

![](img/c9be2506944533fbac16b5c8db99ea71_323.png)

![](img/c9be2506944533fbac16b5c8db99ea71_325.png)

![](img/c9be2506944533fbac16b5c8db99ea71_327.png)

![](img/c9be2506944533fbac16b5c8db99ea71_329.png)

![](img/c9be2506944533fbac16b5c8db99ea71_331.png)

![](img/c9be2506944533fbac16b5c8db99ea71_333.png)

## 修复批归一化层

![](img/c9be2506944533fbac16b5c8db99ea71_335.png)

![](img/c9be2506944533fbac16b5c8db99ea71_337.png)

![](img/c9be2506944533fbac16b5c8db99ea71_339.png)

![](img/c9be2506944533fbac16b5c8db99ea71_341.png)

![](img/c9be2506944533fbac16b5c8db99ea71_343.png)

上一节我们构建了层次化模型。本节中，我们需要修复一个关键问题：`BatchNorm` 层对多维输入的处理。

![](img/c9be2506944533fbac16b5c8db99ea71_345.png)

![](img/c9be2506944533fbac16b5c8db99ea71_347.png)

![](img/c9be2506944533fbac16b5c8db99ea71_349.png)

我们原来的 `BatchNorm` 实现假设输入是二维的 `(batch_size, features)`。但在我们的新架构中，`FlattenConsecutive` 会产生三维输入 `(batch_size, groups, features)`。我们需要让 `BatchNorm` 在训练时，同时计算 `batch` 和 `groups` 维度上的均值和方差。

![](img/c9be2506944533fbac16b5c8db99ea71_351.png)

![](img/c9be2506944533fbac16b5c8db99ea71_353.png)

![](img/c9be2506944533fbac16b5c8db99ea71_355.png)

![](img/c9be2506944533fbac16b5c8db99ea71_357.png)

![](img/c9be2506944533fbac16b5c8db99ea71_359.png)

![](img/c9be2506944533fbac16b5c8db99ea71_361.png)

```python
class BatchNorm:
    def __call__(self, x):
        if self.training:
            dims = (0, 1) if x.ndim == 3 else (0)
            xmean = x.mean(dims, keepdim=True)
            xvar = x.var(dims, keepdim=True)
        # ... 后续标准化和更新运行统计量
```

![](img/c9be2506944533fbac16b5c8db99ea71_363.png)

![](img/c9be2506944533fbac16b5c8db99ea71_365.png)

![](img/c9be2506944533fbac16b5c8db99ea71_367.png)

![](img/c9be2506944533fbac16b5c8db99ea71_369.png)

![](img/c9be2506944533fbac16b5c8db99ea71_371.png)

![](img/c9be2506944533fbac16b5c8db99ea71_373.png)

![](img/c9be2506944533fbac16b5c8db99ea71_375.png)

修复这个Bug后，模型性能得到了小幅但稳定的提升。

![](img/c9be2506944533fbac16b5c8db99ea71_377.png)

![](img/c9be2506944533fbac16b5c8db99ea71_379.png)

![](img/c9be2506944533fbac16b5c8db99ea71_381.png)

![](img/c9be2506944533fbac16b5c8db99ea71_383.png)

![](img/c9be2506944533fbac16b5c8db99ea71_385.png)

![](img/c9be2506944533fbac16b5c8db99ea71_387.png)

![](img/c9be2506944533fbac16b5c8db99ea71_389.png)

![](img/c9be2506944533fbac16b5c8db99ea71_391.png)

## 实验结果与未来方向

![](img/c9be2506944533fbac16b5c8db99ea71_393.png)

![](img/c9be2506944533fbac16b5c8db99ea71_395.png)

通过增加模型容量（如嵌入维度和隐藏层大小），我们最终将验证损失降低到了 **1.993** 左右，成功跨过了2.0的界限。

![](img/c9be2506944533fbac16b5c8db99ea71_397.png)

![](img/c9be2506944533fbac16b5c8db99ea71_399.png)

![](img/c9be2506944533fbac16b5c8db99ea71_401.png)

![](img/c9be2506944533fbac16b5c8db99ea71_403.png)

![](img/c9be2506944533fbac16b5c8db99ea71_405.png)

本节课我们一起实现了一个简化的WaveNet风格架构。我们学习了如何：
1.  使用模块化构建块（如 `Sequential`）来组织复杂网络。
2.  通过 `FlattenConsecutive` 和线性层实现信息的层次化融合。
3.  调整 `BatchNorm` 以正确处理多维输入。

![](img/c9be2506944533fbac16b5c8db99ea71_407.png)

![](img/c9be2506944533fbac16b5c8db99ea71_409.png)

![](img/c9be2506944533fbac16b5c8db99ea71_411.png)

![](img/c9be2506944533fbac16b5c8db99ea71_413.png)

![](img/c9be2506944533fbac16b5c8db99ea71_415.png)

![](img/c9be2506944533fbac16b5c8db99ea71_417.png)

![](img/c9be2506944533fbac16b5c8db99ea71_419.png)

然而，我们实现的只是WaveNet思想的核心骨架。完整的WaveNet还包括**门控激活单元**、**残差连接**和**空洞因果卷积**（用于高效计算）。此外，我们缺乏一个系统的超参数搜索和实验框架，目前的优化更多是“猜测与检验”。

![](img/c9be2506944533fbac16b5c8db99ea71_421.png)

![](img/c9be2506944533fbac16b5c8db99ea71_423.png)

![](img/c9be2506944533fbac16b5c8db99ea71_425.png)

![](img/c9be2506944533fbac16b5c8db99ea71_427.png)

![](img/c9be2506944533fbac16b5c8db99ea71_429.png)

![](img/c9be2506944533fbac16b5c8db99ea71_431.png)

在未来的课程中，我们可以：
*   实现空洞卷积来高效地计算整个输入序列的输出。
*   添加残差连接以训练更深的网络。
*   建立实验管线，进行大规模的超参数优化。
*   探索循环神经网络（RNN/LSTM）和Transformer架构。

![](img/c9be2506944533fbac16b5c8db99ea71_433.png)

![](img/c9be2506944533fbac16b5c8db99ea71_435.png)

![](img/c9be2506944533fbac16b5c8db99ea71_437.png)

![](img/c9be2506944533fbac16b5c8db99ea71_439.png)

![](img/c9be2506944533fbac16b5c8db99ea71_441.png)

![](img/c9be2506944533fbac16b5c8db99ea71_443.png)

![](img/c9be2506944533fbac16b5c8db99ea71_445.png)

![](img/c9be2506944533fbac16b5c8db99ea71_447.png)

**挑战**：你可以尝试调整本课的模型（如各层通道数、嵌入维度），或者阅读WaveNet论文实现更复杂的层，看看能否击败 **1.993** 的验证损失记录。

![](img/c9be2506944533fbac16b5c8db99ea71_449.png)

![](img/c9be2506944533fbac16b5c8db99ea71_451.png)

![](img/c9be2506944533fbac16b5c8db99ea71_453.png)

![](img/c9be2506944533fbac16b5c8db99ea71_455.png)

![](img/c9be2506944533fbac16b5c8db99ea71_457.png)

![](img/c9be2506944533fbac16b5c8db99ea71_459.png)

![](img/c9be2506944533fbac16b5c8db99ea71_461.png)

![](img/c9be2506944533fbac16b5c8db99ea71_463.png)

![](img/c9be2506944533fbac16b5c8db99ea71_465.png)

![](img/c9be2506944533fbac16b5c8db99ea71_467.png)

---
**总结**：本节课中，我们从基础的MLP出发，逐步构建了一个层次化的、类似WaveNet的字符级语言模型。我们重构了代码使其更清晰，引入了层次化信息融合的概念，并修复了批归一化层的多维处理问题。虽然性能得到了提升，但这仅仅是探索现代深度神经网络架构的开始。