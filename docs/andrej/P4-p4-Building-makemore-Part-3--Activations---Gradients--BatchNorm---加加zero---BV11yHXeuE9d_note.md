# P4：构建Makemore第三部分：激活值与梯度，BatchNorm 🧠📈

![](img/7e929a646a3b3c13dd787fc42b498be7_1.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_3.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_5.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_7.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_9.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_11.png)

在本节课中，我们将继续构建Makemore项目，深入探讨神经网络训练中的两个核心概念：**激活值**与**梯度**。我们将分析它们在训练过程中的表现，并学习如何使用**批量归一化（Batch Normalization）** 这一关键技术来稳定深度神经网络的训练。理解这些内容对于后续学习更复杂的模型（如循环神经网络）至关重要。

![](img/7e929a646a3b3c13dd787fc42b498be7_13.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_15.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_17.png)

---

![](img/7e929a646a3b3c13dd787fc42b498be7_19.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_21.png)

## 概述：为什么需要关注激活与梯度？🤔

![](img/7e929a646a3b3c13dd787fc42b498be7_23.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_25.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_27.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_28.png)

上一节我们实现了一个基于多层感知机（MLP）的字符级语言模型。在向更复杂的架构（如RNN、LSTM）迈进之前，我们必须先深入理解神经网络在训练期间内部发生了什么。核心在于观察**激活值（Activations）** 和反向传播的**梯度（Gradients）** 的行为。

![](img/7e929a646a3b3c13dd787fc42b498be7_30.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_32.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_34.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_36.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_37.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_39.png)

如果激活值分布不当（例如，过大或过小），或者梯度流动不畅（例如，消失或爆炸），网络将难以有效学习。这对于深度网络尤为关键。本节课，我们将首先诊断并修复初始化问题，然后引入批量归一化层来系统性地控制激活统计量。

![](img/7e929a646a3b3c13dd787fc42b498be7_41.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_43.png)

---

![](img/7e929a646a3b3c13dd787fc42b498be7_45.png)

## 1. 诊断与修复初始化问题 🔍

![](img/7e929a646a3b3c13dd787fc42b498be7_47.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_49.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_51.png)

神经网络训练的第一步是参数的初始化。糟糕的初始化会导致训练初期出现异常，例如损失值异常高或激活值饱和。

![](img/7e929a646a3b3c13dd787fc42b498be7_53.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_55.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_57.png)

### 1.1 输出层Logits的初始化

![](img/7e929a646a3b3c13dd787fc42b498be7_59.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_61.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_63.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_65.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_67.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_69.png)

在初始化时，我们希望网络对每个输出字符没有先验偏好，即预测概率应大致均匀。对于27个字符的分类问题，期望的初始损失应为 `-log(1/27) ≈ 3.29`。

![](img/7e929a646a3b3c13dd787fc42b498be7_70.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_72.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_74.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_76.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_78.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_80.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_81.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_83.png)

然而，在我们的初始代码中，第一次迭代的损失高达27。这是因为输出层的logits值过于极端，导致softmax输出对错误答案“过度自信”。

![](img/7e929a646a3b3c13dd787fc42b498be7_85.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_86.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_88.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_90.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_91.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_93.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_95.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_97.png)

**问题根源**：输出logits由公式 `logits = h @ w2 + b2` 计算得出。初始的 `w2` 和 `b2` 值过大。

![](img/7e929a646a3b3c13dd787fc42b498be7_99.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_101.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_103.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_105.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_107.png)

**解决方案**：将输出层的权重 `w2` 和偏置 `b2` 初始化为较小的值（例如，乘以0.01），使logits接近零，从而让softmax输出接近均匀分布。

![](img/7e929a646a3b3c13dd787fc42b498be7_109.png)

```python
# 修复输出层初始化
w2 = torch.randn(n_hidden, vocab_size) * 0.01 # 缩小权重
b2 = torch.randn(vocab_size) * 0 # 偏置置零
```

![](img/7e929a646a3b3c13dd787fc42b498be7_111.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_113.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_115.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_117.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_119.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_121.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_123.png)

修复后，初始损失从27降至接近预期的3.29，训练曲线也不再出现初期陡降的“曲棍球棒”形状，这意味着优化过程更有效率。

![](img/7e929a646a3b3c13dd787fc42b498be7_125.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_127.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_129.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_131.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_133.png)

### 1.2 隐藏层激活的初始化

![](img/7e929a646a3b3c13dd787fc42b498be7_135.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_137.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_139.png)

接下来，我们检查隐藏层的激活值 `h`，它由 `tanh` 函数产生。理想情况下，我们希望 `tanh` 的输入（预激活值）不要过大，否则 `tanh` 会进入饱和区（输出接近±1），导致梯度消失。

![](img/7e929a646a3b3c13dd787fc42b498be7_141.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_143.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_144.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_146.png)

**问题诊断**：绘制隐藏层预激活值 `hpreact` 的直方图，发现其值域很宽（例如-15到15）。这使得 `tanh` 的输出大量集中在±1附近，处于函数的平坦区域。在反向传播时，梯度会乘以 `1 - tanh(x)^2`，当输出接近±1时，这个因子接近零，梯度因此“消失”。

![](img/7e929a646a3b3c13dd787fc42b498be7_148.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_150.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_151.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_153.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_155.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_157.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_159.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_161.png)

**解决方案**：同样，通过缩小第一层权重 `w1` 和偏置 `b1` 的初始值，来控制预激活值的尺度。

![](img/7e929a646a3b3c13dd787fc42b498be7_163.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_165.png)

```python
# 修复隐藏层初始化
w1 = torch.randn(block_size * n_embd, n_hidden) * 0.2 # 例如，缩放因子0.2
b1 = torch.randn(n_hidden) * 0.01
```

![](img/7e929a646a3b3c13dd787fc42b498be7_167.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_169.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_171.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_173.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_175.png)

调整后，`tanh` 的输入被控制在合理范围（例如-1.5到1.5），输出直方图显示饱和神经元数量大大减少，梯度得以更有效地流动。

![](img/7e929a646a3b3c13dd787fc42b498be7_177.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_179.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_181.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_183.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_185.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_187.png)

---

![](img/7e929a646a3b3c13dd787fc42b498be7_189.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_191.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_193.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_195.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_197.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_199.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_201.png)

## 2. 权重初始化的原则性方法 ⚖️

![](img/7e929a646a3b3c13dd787fc42b498be7_203.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_205.png)

上一节我们通过试错法找到了缩放因子（如0.2）。但对于更深的网络，我们需要一个系统性的初始化方法。

![](img/7e929a646a3b3c13dd787fc42b498be7_207.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_209.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_211.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_213.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_215.png)

核心思想是：我们希望每一层线性变换的输出（在通过非线性函数之前）保持大致相同的分布，通常希望是均值为0、标准差为1的高斯分布。

![](img/7e929a646a3b3c13dd787fc42b498be7_217.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_219.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_221.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_223.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_225.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_227.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_229.png)

对于一个线性层 `y = x @ w`，假设输入 `x` 是标准高斯分布（均值为0，标准差为1）。那么，为了使输出 `y` 的标准差也保持为1，权重 `w` 的标准差应设置为 `1 / sqrt(fan_in)`，其中 `fan_in` 是该层的输入维度。

![](img/7e929a646a3b3c13dd787fc42b498be7_231.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_233.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_235.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_237.png)

然而，当层与层之间插入非线性激活函数（如 `tanh` 或 `ReLU`）时，情况会发生变化。这些函数会压缩或改变输入的分布。因此，我们需要引入一个**增益（gain）** 因子来补偿。

![](img/7e929a646a3b3c13dd787fc42b498be7_239.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_241.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_243.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_245.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_247.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_248.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_250.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_252.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_254.png)

例如，对于 `tanh` 激活函数，研究发现增益约为 `5/3`。因此，初始化权重时，我们使用：
`std = gain / sqrt(fan_in)`

![](img/7e929a646a3b3c13dd787fc42b498be7_256.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_258.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_260.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_262.png)

PyTorch的 `kaiming_normal_` 初始化方法就内置了针对不同非线性函数的增益计算。

![](img/7e929a646a3b3c13dd787fc42b498be7_264.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_266.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_268.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_270.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_272.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_274.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_276.png)

```python
# 使用PyTorch的Kaiming初始化（针对tanh）
torch.nn.init.kaiming_normal_(w1, nonlinearity='tanh')
```

![](img/7e929a646a3b3c13dd787fc42b498be7_278.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_280.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_282.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_284.png)

通过这种有原则的初始化，我们可以确保深层网络中各层的激活值分布更加稳定，而无需手动调整每个缩放因子。

![](img/7e929a646a3b3c13dd787fc42b498be7_286.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_288.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_289.png)

---

![](img/7e929a646a3b3c13dd787fc42b498be7_291.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_293.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_295.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_297.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_299.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_300.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_302.png)

## 3. 批量归一化（Batch Normalization）🚀

![](img/7e929a646a3b3c13dd787fc42b498be7_304.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_306.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_308.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_310.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_312.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_314.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_316.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_318.png)

尽管精心设计的初始化有所帮助，但在训练非常深的神经网络时，维持稳定的激活分布仍然极具挑战。批量归一化（BatchNorm）应运而生，它通过一个可微操作直接对激活值进行标准化，极大地稳定了深度网络的训练。

![](img/7e929a646a3b3c13dd787fc42b498be7_320.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_322.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_324.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_326.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_328.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_330.png)

### 3.1 BatchNorm的核心思想

![](img/7e929a646a3b3c13dd787fc42b498be7_332.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_334.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_336.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_338.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_340.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_342.png)

BatchNorm层的操作发生在线性层之后、非线性激活函数之前。对于一个批次（Batch）的输入 `x`（形状为 `[batch_size, features]`），它执行以下步骤：

![](img/7e929a646a3b3c13dd787fc42b498be7_343.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_345.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_347.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_349.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_351.png)

1.  **计算批次统计量**：计算该批次数据在每个特征维度上的均值（`mean`）和方差（`var`）。
2.  **标准化**：使用计算出的均值和方差对批次数据进行标准化，使其均值为0，方差为1。
    `xhat = (x - mean) / sqrt(var + eps)`
    （`eps` 是一个很小的数，防止除以零）
3.  **缩放与偏移**：引入两个**可学习参数** `gamma`（增益）和 `beta`（偏置），对标准化后的数据进行变换。
    `out = gamma * xhat + beta`

![](img/7e929a646a3b3c13dd787fc42b498be7_353.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_355.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_356.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_358.png)

**为什么需要 `gamma` 和 `beta`？**
如果只做标准化，网络每一层的表达能力会受到限制（强制为0均值1方差）。`gamma` 和 `beta` 允许网络学习最适合当前任务的数据分布形态。

![](img/7e929a646a3b3c13dd787fc42b498be7_360.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_362.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_364.png)

### 3.2 BatchNorm的训练与推理模式

![](img/7e929a646a3b3c13dd787fc42b498be7_366.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_368.png)

*   **训练模式**：使用**当前批次**的统计量（`mean`, `var`）进行标准化。同时，它会以指数移动平均（EMA）的方式更新两个**缓冲区（buffer）**：`running_mean` 和 `running_var`，用于估计整个训练集的全局统计量。
*   **推理/评估模式**：不再使用当前批次的统计量，而是使用训练阶段估算好的 `running_mean` 和 `running_var` 进行标准化。这使得网络可以对单个样本进行前向传播。

![](img/7e929a646a3b3c13dd787fc42b498be7_370.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_372.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_374.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_376.png)

```python
# 一个简化的BatchNorm1d实现示例
class BatchNorm1d:
    def __init__(self, dim, eps=1e-5, momentum=0.1):
        self.eps = eps
        self.momentum = momentum
        self.gamma = torch.ones(dim)   # 可学习增益，初始为1
        self.beta = torch.zeros(dim)   # 可学习偏置，初始为0
        self.running_mean = torch.zeros(dim)
        self.running_var = torch.ones(dim)

    def __call__(self, x):
        if self.training:
            # 训练模式：使用当前批次统计
            xmean = x.mean(0, keepdim=True)
            xvar = x.var(0, keepdim=True)
        else:
            # 推理模式：使用运行统计
            xmean = self.running_mean
            xvar = self.running_var

        xhat = (x - xmean) / torch.sqrt(xvar + self.eps)
        out = self.gamma * xhat + self.beta

        if self.training:
            # 更新运行统计
            with torch.no_grad():
                self.running_mean = (1 - self.momentum) * self.running_mean + self.momentum * xmean
                self.running_var = (1 - self.momentum) * self.running_var + self.momentum * xvar
        return out
```

![](img/7e929a646a3b3c13dd787fc42b498be7_378.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_380.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_382.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_384.png)

### 3.3 使用BatchNorm的注意事项

![](img/7e929a646a3b3c13dd787fc42b498be7_386.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_388.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_390.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_392.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_394.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_395.png)

1.  **与偏置项共存**：在线性层（`Linear`）或卷积层（`Conv2d`）后接BatchNorm时，通常应将线性层的 `bias` 参数设为 `False`。因为BatchNorm中的 `beta` 已经起到了偏置的作用，原线性层的偏置会在标准化时被减去，变得无用。
2.  **正则化效果**：由于标准化时使用的均值和方差来自当前批次，这为每个样本的激活引入了一些噪声（因为批次是随机采样的）。这种噪声起到了轻微的正则化作用，有助于防止过拟合。
3.  **批次大小**：BatchNorm的效果依赖于足够大的批次大小（batch size）来获得有意义的统计量估计。批次过小可能导致运行统计量估计不准确，训练不稳定。

![](img/7e929a646a3b3c13dd787fc42b498be7_397.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_399.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_400.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_402.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_404.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_406.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_408.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_410.png)

---

![](img/7e929a646a3b3c13dd787fc42b498be7_412.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_414.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_416.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_418.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_420.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_422.png)

## 4. 训练诊断工具 📊

![](img/7e929a646a3b3c13dd787fc42b498be7_424.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_426.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_428.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_430.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_431.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_433.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_435.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_437.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_439.png)

为了确保神经网络训练健康，我们需要监控一些关键指标。以下是一些有用的诊断工具：

![](img/7e929a646a3b3c13dd787fc42b498be7_441.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_443.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_445.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_447.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_448.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_450.png)

### 4.1 激活与梯度统计

![](img/7e929a646a3b3c13dd787fc42b498be7_452.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_454.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_456.png)

*   **前向传播激活直方图**：绘制每一层激活值（特别是非线性层如 `tanh` 的输出）的直方图。检查是否过度饱和（大量值集中在±1）或过度不活跃（大量值接近0）。
*   **梯度直方图**：绘制流经每一层的梯度的直方图。检查梯度是否消失（值非常小）或爆炸（值非常大）。

![](img/7e929a646a3b3c13dd787fc42b498be7_458.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_460.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_462.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_464.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_466.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_468.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_470.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_472.png)

### 4.2 参数更新比率

![](img/7e929a646a3b3c13dd787fc42b498be7_474.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_476.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_478.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_480.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_482.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_483.png)

更重要的指标是**参数更新与其数值本身的比率**。在一次优化步骤中，参数的更新量为 `update = -learning_rate * gradient`。我们关心 `update / data` 的尺度。

![](img/7e929a646a3b3c13dd787fc42b498be7_485.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_487.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_488.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_490.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_492.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_494.png)

通常，我们希望这个比率在对数尺度上（`log10(|update| / |data|)`）大约在 **-3** 左右（即更新量大约是参数值的千分之一）。这表示训练以稳定、适中的速度进行。

![](img/7e929a646a3b3c13dd787fc42b498be7_496.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_498.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_500.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_501.png)

*   比率远大于-3（如-1）：更新过大，可能导致训练不稳定。
*   比率远小于-3（如-5）：更新过小，训练可能过于缓慢。

![](img/7e929a646a3b3c13dd787fc42b498be7_503.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_505.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_507.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_509.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_511.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_512.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_514.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_516.png)

通过绘制不同网络层参数的这个比率随时间变化的曲线，可以直观判断学习率设置是否合适，以及网络各层是否在协调学习。

![](img/7e929a646a3b3c13dd787fc42b498be7_518.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_520.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_522.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_524.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_526.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_528.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_529.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_531.png)

---

![](img/7e929a646a3b3c13dd787fc42b498be7_533.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_535.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_537.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_539.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_541.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_543.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_545.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_547.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_549.png)

## 总结 🎯

![](img/7e929a646a3b3c13dd787fc42b498be7_551.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_553.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_555.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_556.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_558.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_560.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_562.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_564.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_566.png)

本节课我们一起深入探讨了神经网络训练的核心动态：

![](img/7e929a646a3b3c13dd787fc42b498be7_568.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_570.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_572.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_574.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_576.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_578.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_580.png)

1.  **初始化的重要性**：不恰当的初始化会导致输出层过度自信、隐藏层激活饱和，从而损害梯度流动和训练效率。我们学习了如何通过缩放权重来获得合理的初始激活分布。
2.  **原则性初始化**：介绍了基于输入维度（`fan_in`）和非线性函数增益（如 `tanh` 的 `5/3`）的初始化方法（如Kaiming初始化），这为构建更深网络提供了指导。
3.  **批量归一化（BatchNorm）**：作为稳定深度网络训练的关键技术，BatchNorm通过标准化激活、引入可学习的缩放偏移参数、以及区分的训练/推理模式，极大地缓解了梯度消失/爆炸问题，并带来了正则化益处。
4.  **诊断与监控**：我们学习了一套实用的诊断工具，包括检查激活/梯度直方图以及监控参数更新比率，这些工具能帮助我们判断网络训练是否健康，并指导超参数（如学习率）的调整。

![](img/7e929a646a3b3c13dd787fc42b498be7_582.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_584.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_586.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_588.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_590.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_592.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_594.png)

![](img/7e929a646a3b3c13dd787fc42b498be7_596.png)

通过理解激活、梯度和BatchNorm，我们为接下来探索更强大但也更难以训练的循环神经网络（RNN）架构奠定了坚实的基础。在下一课中，我们将开始构建RNN，并观察这些动态在其中扮演的关键角色。