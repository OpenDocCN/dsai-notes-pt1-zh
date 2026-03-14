# P5：p5 建造 makemore 第四部分：成为一个反向传播忍者 🥷

![](img/86f8499f4a173e8882bce59ebef07fb4_1.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_2.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_4.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_6.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_8.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_10.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_12.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_14.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_16.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_18.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_20.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_22.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_24.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_26.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_28.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_30.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_32.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_34.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_36.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_38.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_39.png)

在本节课中，我们将深入学习如何手动实现神经网络的反向传播。我们将从当前的多层感知器（MLP）架构出发，逐步替换 PyTorch 的自动微分功能，通过手动计算梯度来加深对神经网络内部工作原理的理解。

![](img/86f8499f4a173e8882bce59ebef07fb4_41.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_43.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_45.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_47.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_49.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_51.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_52.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_54.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_56.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_58.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_60.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_62.png)

## 概述

![](img/86f8499f4a173e8882bce59ebef07fb4_64.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_66.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_68.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_70.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_72.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_74.png)

到目前为止，我们已经实现并训练了一个包含批量归一化层的两层 MLP。我们的神经网络架构如下：

![](img/86f8499f4a173e8882bce59ebef07fb4_76.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_78.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_80.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_82.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_84.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_1.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_86.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_88.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_90.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_92.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_94.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_96.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_98.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_100.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_102.png)

我们取得了不错的损失，并对架构有了较好的理解。然而，当前的代码依赖于 PyTorch 的 `loss.backward()` 来自动计算梯度。本节课的目标是移除这个依赖，手动在张量级别实现反向传播。

![](img/86f8499f4a173e8882bce59ebef07fb4_104.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_106.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_108.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_109.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_111.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_113.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_114.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_116.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_118.png)

理解反向传播的内部机制至关重要，因为它是一个“漏水的抽象”。仅仅堆叠可微函数并期望反向传播自动工作是不够的。如果不理解其内部原理，在调试和优化网络时可能会遇到困难。

![](img/86f8499f4a173e8882bce59ebef07fb4_120.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_122.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_124.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_126.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_127.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_129.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_131.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_133.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_134.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_136.png)

历史上，手动编写反向传播曾是标准做法。例如，在 2010 年左右，使用 MATLAB 或 NumPy 手动计算梯度非常普遍。虽然如今自动微分已成为标准，但掌握手动反向传播能让你成为更强大的神经网络实践者和调试者。

![](img/86f8499f4a173e8882bce59ebef07fb4_138.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_140.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_142.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_144.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_146.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_148.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_149.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_151.png)

本节课的练习将分为四个部分：
1.  将整个计算图分解为原子操作，并逐一进行反向传播。
2.  对交叉熵损失进行数学推导，实现其高效的反向传播。
3.  对批量归一化层进行数学推导，实现其高效的反向传播。
4.  将所有手动反向传播代码整合，训练完整的 MLP。

![](img/86f8499f4a173e8882bce59ebef07fb4_153.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_155.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_157.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_159.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_161.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_163.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_165.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_167.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_169.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_171.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_173.png)

现在，让我们开始第一个练习。

![](img/86f8499f4a173e8882bce59ebef07fb4_175.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_177.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_179.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_181.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_183.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_185.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_187.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_188.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_190.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_192.png)

## 练习一：原子操作的反向传播

![](img/86f8499f4a173e8882bce59ebef07fb4_194.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_196.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_198.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_200.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_202.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_204.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_205.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_207.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_209.png)

在第一个练习中，我们将把前向传播过程分解为许多细小的原子操作（如加法、乘法、指数、对数等），并逐一为每个操作手动计算梯度。我们将从损失开始，逐步反向传播到网络的输入。

![](img/86f8499f4a173e8882bce59ebef07fb4_211.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_213.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_215.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_217.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_219.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_221.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_223.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_224.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_226.png)

以下是实现反向传播所需的关键步骤列表：

![](img/86f8499f4a173e8882bce59ebef07fb4_228.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_230.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_232.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_234.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_236.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_238.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_240.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_242.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_244.png)

*   **计算损失对 logprobs 的梯度 (`dlogprobs`)**：损失是正确标签对应 `logprobs` 的负平均值。因此，梯度在正确标签位置为 `-1/n`，其他位置为 0。
    ```python
    dlogprobs = torch.zeros_like(logprobs)
    dlogprobs[range(n), yb] = -1.0 / n
    ```
*   **计算损失对 probs 的梯度 (`dprobs`)**：`logprobs = log(probs)`。根据链式法则，`dprobs = dlogprobs * (1 / probs)`。
    ```python
    dprobs = (1.0 / probs) * dlogprobs
    ```
*   **计算损失对计数和计数的梯度**：`probs = counts / counts_sum`。这里涉及除法和广播，需要小心处理。对于 `dcounts_sum`，梯度需要沿被广播的维度求和。
    ```python
    dcounts_sum = (-counts / (counts_sum**2)) * dprobs
    dcounts_sum = dcounts_sum.sum(dim=0, keepdim=True)
    dcounts = (1.0 / counts_sum) * dprobs
    ```
*   **计算损失对归一化 logits 的梯度 (`dnormlogits`)**：`counts = exp(normlogits)`。因此，`dnormlogits = counts * dcounts`。
    ```python
    dnormlogits = counts * dcounts
    ```
*   **计算损失对 logits 和 logit maxes 的梯度**：`normlogits = logits - logitmaxes`。这里也有广播。
    ```python
    dlogits = dnormlogits.clone()
    dlogitmaxes = -dnormlogits.sum(dim=1, keepdim=True)
    ```
*   **计算损失对 logit maxes 的第二个分支梯度**：`logitmaxes` 来自 `logits` 的最大值。梯度需要路由到最大值出现的位置。
    ```python
    dlogits[range(n), logitmaxes_idx] += dlogitmaxes.squeeze()
    ```
*   **计算损失对隐藏层、权重和偏置的梯度（线性层）**：`logits = h @ w2 + b2`。通过维度匹配推导，`dh = dlogits @ w2.T`，`dw2 = h.T @ dlogits`，`db2 = dlogits.sum(dim=0)`。
    ```python
    dh = dlogits @ w2.T
    dw2 = h.T @ dlogits
    db2 = dlogits.sum(dim=0)
    ```
*   **计算损失对 tanh 激活前的梯度 (`dhpreact`)**：`h = tanh(hpreact)`。导数 `dhpreact = (1 - h**2) * dh`。
    ```python
    dhpreact = (1 - h**2) * dh
    ```
*   **计算损失对批量归一化参数和输出的梯度**：`hpreact = bngain * bnraw + bnbias`。需要处理广播。
    ```python
    dbngain = (bnraw * dhpreact).sum(dim=0, keepdim=True)
    dbnraw = bngain * dhpreact
    dbnbias = dhpreact.sum(dim=0, keepdim=True)
    ```
*   **继续反向传播通过批量归一化的各个原子步骤**：包括对方差、均值、差值等中间变量的梯度计算，过程较为繁琐，需仔细处理广播和求和。
*   **计算损失对第一个线性层参数和输入的梯度**：与第二个线性层类似。
    ```python
    demcat = dhprebn @ w1.T
    dw1 = emcat.T @ dhprebn
    db1 = dhprebn.sum(dim=0)
    ```
*   **计算损失对嵌入层参数的梯度 (`dC`)**：`em` 是通过索引从嵌入表 `C` 中查得的。梯度需要根据索引累加回 `dC`。
    ```python
    dC = torch.zeros_like(C)
    for k in range(Xb.shape[0]):
        for j in range(Xb.shape[1]):
            ix = Xb[k, j]
            dC[ix] += dem[k, j, :]
    ```

![](img/86f8499f4a173e8882bce59ebef07fb4_246.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_248.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_250.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_252.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_254.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_256.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_258.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_259.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_261.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_263.png)

通过逐步实现上述所有步骤，我们完成了对整个计算图的原子级反向传播。每一步的梯度都可以与 PyTorch 自动计算的梯度进行比较验证。

![](img/86f8499f4a173e8882bce59ebef07fb4_265.png)

上一节我们通过分解原子操作完成了反向传播，但这种方法效率较低。接下来，我们将看到如何通过数学推导来简化关键部分的反向传播。

![](img/86f8499f4a173e8882bce59ebef07fb4_267.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_269.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_271.png)

## 练习二：交叉熵损失的高效反向传播

![](img/86f8499f4a173e8882bce59ebef07fb4_273.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_275.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_277.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_279.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_281.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_283.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_284.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_286.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_288.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_290.png)

在第二个练习中，我们将交叉熵损失视为一个整体数学表达式，并通过解析方式直接计算损失对 logits 的梯度，而不是通过多个原子操作。

![](img/86f8499f4a173e8882bce59ebef07fb4_292.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_294.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_296.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_298.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_300.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_301.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_303.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_305.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_307.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_309.png)

交叉熵损失可以写为：
`loss = -log(softmax(logits)[label])` 对于单个样本，或对批次取平均。

![](img/86f8499f4a173e8882bce59ebef07fb4_311.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_313.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_315.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_317.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_319.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_321.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_323.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_325.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_327.png)

通过数学推导（应用微积分链式法则），我们可以得到损失对 logits 的梯度有一个非常简洁的形式：
对于批处理中的每个样本，`dlogits = softmax(logits) - one_hot(label)`。

![](img/86f8499f4a173e8882bce59ebef07fb4_329.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_331.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_333.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_335.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_337.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_338.png)

直观理解是：梯度会降低错误类别（根据网络当前概率）的 logits，并提高正确类别的 logits，推拉的力量是平衡的。

![](img/86f8499f4a173e8882bce59ebef07fb4_340.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_342.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_344.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_346.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_347.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_349.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_351.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_353.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_355.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_357.png)

以下是实现代码：
```python
probs = F.softmax(logits, dim=1)
dlogits = probs.clone()
dlogits[range(n), yb] -= 1
dlogits /= n  # 因为损失是平均值
```
这个简单的几行代码等价于练习一中从 `logits` 到 `loss` 之间所有原子操作的反向传播总和，但效率要高得多。

![](img/86f8499f4a173e8882bce59ebef07fb4_359.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_361.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_363.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_365.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_367.png)

## 练习三：批量归一化的高效反向传播

![](img/86f8499f4a173e8882bce59ebef07fb4_369.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_371.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_373.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_375.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_377.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_379.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_381.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_383.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_385.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_387.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_389.png)

类似地，对于批量归一化层，我们也可以推导出一个统一的反向传播公式。给定输入 `x`（即 `hprebn`）、输出 `y`（即 `hpreact`）、缩放参数 `gamma`（`bngain`）、偏移参数 `beta`（`bnbias`）、批次均值 `mu`、批次方差 `sigma2`（使用贝塞尔校正 `n-1`）和小常数 `eps`。

![](img/86f8499f4a173e8882bce59ebef07fb4_391.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_393.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_395.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_397.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_399.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_401.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_403.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_405.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_407.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_408.png)

前向传播为：
`xhat = (x - mu) / sqrt(sigma2 + eps)`
`y = gamma * xhat + beta`

![](img/86f8499f4a173e8882bce59ebef07fb4_410.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_412.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_414.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_415.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_416.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_418.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_419.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_421.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_423.png)

经过繁琐但直接的微积分推导（考虑 `mu` 和 `sigma2` 是 `x` 的函数，且 `xhat` 相互依赖），我们可以得到损失对输入 `x` 的梯度 `dx` 的解析表达式。

![](img/86f8499f4a173e8882bce59ebef07fb4_425.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_427.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_429.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_431.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_433.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_435.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_437.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_439.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_441.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_443.png)

以下是根据推导结果实现的向量化代码：
```python
# 前向传播中计算的中间变量
bnmean = bnraw.mean(dim=0, keepdim=True)
bndiff = bnraw - bnmean
bndiff2 = bndiff**2
bnvar = (bndiff2).sum(dim=0, keepdim=True) / (n - 1) # 贝塞尔校正
bnvar_inv = (bnvar + eps)**-0.5
bnrawhat = bndiff * bnvar_inv

![](img/86f8499f4a173e8882bce59ebef07fb4_444.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_446.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_447.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_449.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_450.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_452.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_454.png)

# 反向传播 (dhpreact 是损失对 y 的梯度)
dbnraw = (bngain * bnvar_inv / (n-1)) * ( (n-1) * dhpreact - dhpreact.sum(dim=0, keepdim=True) - bnrawhat * (dhpreact * bnrawhat).sum(dim=0, keepdim=True) )
```
这个公式直接计算了 `dhprebn`（即 `dx`），避免了通过所有中间变量反向传播的复杂性。

![](img/86f8499f4a173e8882bce59ebef07fb4_456.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_458.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_460.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_461.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_463.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_464.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_466.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_468.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_470.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_472.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_474.png)

## 练习四：整合与训练

![](img/86f8499f4a173e8882bce59ebef07fb4_476.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_478.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_480.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_482.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_484.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_486.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_488.png)

在最后的练习中，我们将把所有手动实现的反向传播代码片段整合到一起，形成一个完整的、不依赖 `loss.backward()` 的训练循环。

![](img/86f8499f4a173e8882bce59ebef07fb4_490.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_492.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_494.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_496.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_498.png)

我们将使用练习二和练习三推导出的高效反向传播公式，以及为线性层、激活函数和嵌入层手动编写的梯度计算代码。

![](img/86f8499f4a173e8882bce59ebef07fb4_500.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_502.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_504.png)

关键步骤包括：
1.  前向传播计算损失。
2.  手动反向传播计算所有参数的梯度。
3.  使用计算出的梯度（而非 `p.grad`）更新参数。
4.  关闭 PyTorch 的梯度跟踪以提升效率 (`torch.no_grad`)。

完成整合后，我们可以运行训练循环。经过足够迭代，损失会下降，并且从模型中进行采样可以生成看起来合理的“名字”，结果与使用自动微分时相同。这证明了我们手动计算梯度的正确性。

![](img/86f8499f4a173e8882bce59ebef07fb4_506.png)

## 总结

![](img/86f8499f4a173e8882bce59ebef07fb4_508.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_510.png)

本节课中，我们一起深入探讨了神经网络反向传播的内部机制。我们从最基础的原子操作开始，手动计算了每一个步骤的梯度。然后，我们通过数学推导，找到了交叉熵损失和批量归一化层更高效、更简洁的反向传播公式。最后，我们将所有手动代码整合，成功训练了一个多层感知器。

![](img/86f8499f4a173e8882bce59ebef07fb4_512.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_514.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_516.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_518.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_520.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_522.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_524.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_526.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_528.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_530.png)

通过这个过程，我们揭开了自动微分的神秘面纱，强化了对梯度如何在网络中流动的理解。这不仅能帮助我们在未来更好地调试神经网络，也让我们对所使用的工具有了更深刻的认识。现在，我们可以充满信心地称自己为“反向传播忍者”了！

![](img/86f8499f4a173e8882bce59ebef07fb4_532.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_534.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_536.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_538.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_539.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_541.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_543.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_545.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_547.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_549.png)

![](img/86f8499f4a173e8882bce59ebef07fb4_551.png)

在下一节课中，我们将探索更复杂的架构，如循环神经网络（RNN）及其变体（LSTM），以获取更好的模型性能。