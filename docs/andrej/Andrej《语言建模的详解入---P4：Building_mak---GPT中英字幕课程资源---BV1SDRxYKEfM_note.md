# 语言建模的详解入门：4：成为反向传播高手 🥷

![](img/a3d21168dc6b383673c85535a01b0c8f_1.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_2.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_4.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_6.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_7.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_9.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_11.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_13.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_15.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_17.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_18.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_19.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_20.png)

在本节课中，我们将学习如何手动实现神经网络的反向传播，摆脱对 PyTorch `loss.backward()` 的依赖。通过亲手计算张量级别的梯度，我们将深入理解反向传播的内部机制，这对于调试神经网络和优化模型至关重要。

![](img/a3d21168dc6b383673c85535a01b0c8f_22.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_24.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_26.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_27.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_29.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_30.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_31.png)

## 概述

到目前为止，我们已经实现了一个包含批归一化的两层多层感知机，并获得了不错的损失值。然而，我们一直依赖 PyTorch 的自动微分来计算梯度。本节课程的目标是移除 `loss.backward()` 的调用，手动在张量级别实现反向传播。

![](img/a3d21168dc6b383673c85535a01b0c8f_33.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_34.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_36.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_37.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_39.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_40.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_41.png)

理解反向传播的内部原理非常重要，因为它是一个“有漏洞的抽象”。仅仅堆叠各种函数模块并指望反向传播自动工作是不够的。如果不理解其内部机制，可能会遇到梯度消失、爆炸或死神经元等问题，并且难以进行有效的调试。

![](img/a3d21168dc6b383673c85535a01b0c8f_43.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_44.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_45.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_47.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_49.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_50.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_52.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_53.png)

## 历史背景与动机

![](img/a3d21168dc6b383673c85535a01b0c8f_55.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_56.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_57.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_59.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_61.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_63.png)

大约十年前，手动编写反向传播是深度学习的标准做法。如今，虽然自动微分已成为主流，但手动实现反向传播仍然是一项极具价值的练习。它能让你对神经网络的工作原理有更直观、更深刻的理解，使你成为更强大的神经网络调试者和构建者。

![](img/a3d21168dc6b383673c85535a01b0c8f_65.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_67.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_68.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_70.png)

## 练习设置

![](img/a3d21168dc6b383673c85535a01b0c8f_72.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_73.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_75.png)

我们将通过一系列练习来逐步实现完整的反向传播：

![](img/a3d21168dc6b383673c85535a01b0c8f_77.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_79.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_81.png)

1.  **逐原子操作反向传播**：将前向传播分解为最小的原子操作，并手动为每个中间变量计算梯度。
2.  **交叉熵损失的高效反向传播**：使用数学推导，直接计算损失函数对 logits 的梯度，避免繁琐的逐层回传。
3.  **批归一化的高效反向传播**：类似地，为批归一化层推导并实现一个高效、紧凑的梯度计算公式。
4.  **整合训练循环**：将手动计算的反向传播代码整合到完整的训练循环中，验证其效果。

![](img/a3d21168dc6b383673c85535a01b0c8f_83.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_84.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_86.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_88.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_90.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_92.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_93.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_94.png)

接下来，我们将开始第一个练习，手动反向传播整个计算图。

![](img/a3d21168dc6b383673c85535a01b0c8f_95.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_97.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_98.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_99.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_101.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_103.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_104.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_105.png)

---

![](img/a3d21168dc6b383673c85535a01b0c8f_107.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_109.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_111.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_112.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_114.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_116.png)

## 练习一：逐原子反向传播 🔬

![](img/a3d21168dc6b383673c85535a01b0c8f_118.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_119.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_121.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_123.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_125.png)

上一节我们概述了目标，本节我们将从计算图的末端开始，一步步手动计算每个中间变量的梯度。我们首先关注损失函数对 `logprobs` 的梯度。

![](img/a3d21168dc6b383673c85535a01b0c8f_127.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_129.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_131.png)

### 计算 Dlogprobs

![](img/a3d21168dc6b383673c85535a01b0c8f_133.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_135.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_137.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_139.png)

`logprobs` 是一个形状为 `(32, 27)` 的张量。损失函数是正确字符对数概率的负平均值。因此，只有被 `y_b` 索引到的那些 `logprobs` 元素对损失有贡献。

![](img/a3d21168dc6b383673c85535a01b0c8f_141.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_142.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_143.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_145.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_147.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_148.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_150.png)

**公式推导**：
对于被选中的元素，其梯度为 `-1/n`（其中 `n` 是批量大小，此处为 32）。对于其他未被选中的元素，梯度为 0。

![](img/a3d21168dc6b383673c85535a01b0c8f_152.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_153.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_155.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_157.png)

**代码实现**：
```python
dlogprobs = torch.zeros_like(logprobs)
dlogprobs[range(n), yb] = -1.0 / n
```
通过比较，我们验证了手动计算的 `dlogprobs` 与 PyTorch 自动计算的 `logprobs.grad` 完全一致。

![](img/a3d21168dc6b383673c85535a01b0c8f_159.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_161.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_162.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_164.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_166.png)

### 计算 Dprobs

![](img/a3d21168dc6b383673c85535a01b0c8f_168.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_170.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_172.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_174.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_176.png)

`logprobs` 由 `probs` 经过逐元素对数运算得到。根据微积分，`d(ln(x))/dx = 1/x`。

![](img/a3d21168dc6b383673c85535a01b0c8f_178.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_179.png)

**公式推导**：
`dprobs = (1 / probs) * dlogprobs`

![](img/a3d21168dc6b383673c85535a01b0c8f_181.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_183.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_185.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_187.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_189.png)

**代码实现**：
```python
dprobs = (1 / probs) * dlogprobs
```
验证通过。

![](img/a3d21168dc6b383673c85535a01b0c8f_191.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_193.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_195.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_196.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_197.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_199.png)

### 计算 Dcounts_sum_inv 和 Dcounts

![](img/a3d21168dc6b383673c85535a01b0c8f_201.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_203.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_205.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_207.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_209.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_211.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_213.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_215.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_217.png)

`probs` 由 `counts` 除以 `counts_sum`（并经过广播）得到。这里涉及乘法和广播操作的反向传播。

![](img/a3d21168dc6b383673c85535a01b0c8f_219.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_221.png)

**公式推导**：
*   对于乘法 `probs = counts * counts_sum_inv`，`counts_sum_inv` 的梯度需要从 `dprobs` 获取，并由于广播（重用）而在第0维（样本维）求和。
*   `counts` 的梯度来自两条路径：一条直接来自 `dprobs`，另一条通过 `counts_sum` 和 `counts_sum_inv` 间接传来。

![](img/a3d21168dc6b383673c85535a01b0c8f_223.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_224.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_226.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_228.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_230.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_232.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_234.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_236.png)

**代码实现**：
```python
# 梯度流向 counts_sum_inv (广播前的变量)
dcounts_sum_inv = (counts * dprobs).sum(0, keepdim=True)
# 梯度流向 counts (第一条路径)
dcounts = counts_sum_inv * dprobs
# 后续还会通过 counts_sum 分支补充梯度
```
验证了 `dcounts_sum_inv` 的正确性。

![](img/a3d21168dc6b383673c85535a01b0c8f_237.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_238.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_240.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_242.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_244.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_246.png)

### 计算 Dcounts_sum 和补充 Dcounts

![](img/a3d21168dc6b383673c85535a01b0c8f_248.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_250.png)

`counts_sum_inv` 是 `counts_sum` 的 `-1` 次幂。`counts_sum` 是 `counts` 在第1维（特征维）的和。

![](img/a3d21168dc6b383673c85535a01b0c8f_252.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_254.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_256.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_257.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_259.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_261.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_263.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_265.png)

**公式推导**：
*   `d(x^-1)/dx = -x^-2`，因此 `dcounts_sum = -counts_sum**-2 * dcounts_sum_inv`。
*   求和操作的反向传播是广播：`counts_sum` 的梯度被复制到 `counts` 的每个贡献元素上。

![](img/a3d21168dc6b383673c85535a01b0c8f_267.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_269.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_271.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_273.png)

**代码实现**：
```python
# 梯度流向 counts_sum
dcounts_sum = (-counts_sum**-2) * dcounts_sum_inv
# 梯度通过求和操作流向 counts (第二条路径)
dcounts += torch.ones_like(counts) * dcounts_sum
```
验证通过，并且补充第二条路径后，`dcounts` 也正确了。

![](img/a3d21168dc6b383673c85535a01b0c8f_275.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_276.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_278.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_280.png)

### 计算 Dnormlogits

![](img/a3d21168dc6b383673c85535a01b0c8f_282.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_284.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_285.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_287.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_289.png)

`counts` 是 `normlogits` 的逐元素指数。`d(e^x)/dx = e^x`。

![](img/a3d21168dc6b383673c85535a01b0c8f_291.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_293.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_294.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_296.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_298.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_299.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_301.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_303.png)

**公式推导**：
`dnormlogits = counts * dcounts`

![](img/a3d21168dc6b383673c85535a01b0c8f_305.png)

**代码实现**：
```python
dnormlogits = counts * dcounts
```
验证通过。

![](img/a3d21168dc6b383673c85535a01b0c8f_307.png)

### 计算 Dlogits 和 Dlogitmax

![](img/a3d21168dc6b383673c85535a01b0c8f_309.png)

`normlogits = logits - logitmax`，其中 `logitmax` 是 `logits` 每行的最大值，会进行广播。

**公式推导**：
*   `dlogits` 直接接收来自 `dnormlogits` 的梯度。
*   `dlogitmax` 接收来自 `dnormlogits` 的梯度，并由于广播而在第1维求和（带负号）。
*   `logitmax` 本身又由 `logits` 计算而来，因此 `logits` 还会从 `logitmax` 接收第二条路径的梯度。

![](img/a3d21168dc6b383673c85535a01b0c8f_311.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_313.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_315.png)

**代码实现**：
```python
# 第一条路径：来自减法的梯度
dlogits = dnormlogits.clone()
dlogitmax = (-dnormlogits).sum(1, keepdim=True)
# 第二条路径：来自最大值操作的梯度
# 使用 one-hot 编码将梯度散射回最大值出现的位置
dlogits += F.one_hot(logitmax_idx, logits.shape[1]) * dlogitmax
```
验证通过。值得注意的是，`logitmax` 的梯度理论上应为0（因为平移不影响 softmax），实际数值也极小（~1e-9），符合预期。

![](img/a3d21168dc6b383673c85535a01b0c8f_317.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_319.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_320.png)

### 计算 Dh, Dw2, Db2 (第一个线性层)

![](img/a3d21168dc6b383673c85535a01b0c8f_322.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_324.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_325.png)

`logits = h @ w2 + b2`。这是矩阵乘法和加法的反向传播。

![](img/a3d21168dc6b383673c85535a01b0c8f_327.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_329.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_331.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_333.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_334.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_335.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_336.png)

**公式推导（通过维度匹配法）**：
*   `dh` 的形状必须与 `h` (`32, 64`) 相同。`dlogits` 是 `(32, 27)`，`w2` 是 `(64, 27)`。唯一能使维度匹配的运算是：`dh = dlogits @ w2.T`。
*   `dw2` 的形状是 `(64, 27)`。`h` 是 `(32, 64)`。唯一匹配的运算是：`dw2 = h.T @ dlogits`。
*   `db2` 的形状是 `(27,)`。加法操作的梯度是求和，需要消除批量维：`db2 = dlogits.sum(0)`。

![](img/a3d21168dc6b383673c85535a01b0c8f_338.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_340.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_342.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_344.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_346.png)

**代码实现**：
```python
dh = dlogits @ w2.T
dw2 = h.T @ dlogits
db2 = dlogits.sum(0)
```
验证通过。

![](img/a3d21168dc6b383673c85535a01b0c8f_348.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_350.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_352.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_354.png)

### 计算 Dhpreact (Tanh 激活层)

![](img/a3d21168dc6b383673c85535a01b0c8f_356.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_357.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_358.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_360.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_362.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_364.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_365.png)

`h = tanh(hpreact)`。`d(tanh(x))/dx = 1 - tanh(x)^2`。

![](img/a3d21168dc6b383673c85535a01b0c8f_367.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_369.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_371.png)

**公式推导**：
`dhpreact = (1 - h**2) * dh`

![](img/a3d21168dc6b383673c85535a01b0c8f_373.png)

**代码实现**：
```python
dhpreact = (1 - h**2) * dh
```
验证通过。

![](img/a3d21168dc6b383673c85535a01b0c8f_375.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_377.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_379.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_381.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_383.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_385.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_387.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_389.png)

### 计算 Dbngain, Dbias, Dbnraw (批归一化缩放偏移)

![](img/a3d21168dc6b383673c85535a01b0c8f_391.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_392.png)

`hpreact = bnraw * bngain + bnbias`，涉及逐元素乘法和广播。

![](img/a3d21168dc6b383673c85535a01b0c8f_394.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_396.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_398.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_400.png)

**公式推导**：
*   `dbngain`：来自乘法，梯度为 `bnraw * dhpreact`，并由于 `bngain` 的广播而在第0维求和。
*   `dbnraw`：来自乘法，梯度为 `bngain * dhpreact`（广播自动处理）。
*   `dbnbias`：来自加法，梯度为 `dhpreact`，并由于广播而在第0维求和。

![](img/a3d21168dc6b383673c85535a01b0c8f_402.png)

**代码实现**：
```python
dbngain = (bnraw * dhpreact).sum(0, keepdim=True)
dbnraw = bngain * dhpreact
dbnbias = dhpreact.sum(0, keepdim=True)
```
验证通过。

![](img/a3d21168dc6b383673c85535a01b0c8f_404.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_406.png)

### 计算 Dbnvar_inv 和 Dbnvar

![](img/a3d21168dc6b383673c85535a01b0c8f_408.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_409.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_411.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_412.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_414.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_416.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_417.png)

`bnraw = bndiff * bnvar_inv`。`bnvar_inv = (bnvar + eps)**-0.5`。

![](img/a3d21168dc6b383673c85535a01b0c8f_419.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_421.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_423.png)

**公式推导**：
*   `dbndiff` (第一条路径)：`dbndiff = bnvar_inv * dbnraw`。
*   `dbnvar_inv`：`dbnvar_inv = bndiff * dbnraw`，并求和（因为 `bnvar_inv` 是标量广播而来？这里需要根据形状仔细处理，`bnvar_inv` 是 `(1,64)`，广播到 `(32,64)`，因此梯度需要沿第0维求和）。
*   `dbnvar`：`d(x^-0.5)/dx = -0.5 * x^-1.5`，所以 `dbnvar = -0.5 * (bnvar+eps)**-1.5 * dbnvar_inv`。

![](img/a3d21168dc6b383673c85535a01b0c8f_425.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_427.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_429.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_431.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_433.png)

**代码实现**：
```python
# 第一条路径流向 bndiff
dbndiff = bnvar_inv * dbnraw
# 流向 bnvar_inv
dbnvar_inv = (bndiff * dbnraw).sum(0, keepdim=True)
# 流向 bnvar
dbnvar = (-0.5 * (bnvar+eps)**-1.5) * dbnvar_inv
```
验证了 `dbnvar_inv` 和 `dbnvar` 的正确性。`dbndiff` 此时还不完整，因为 `bndiff` 还有另一条输入路径。

![](img/a3d21168dc6b383673c85535a01b0c8f_435.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_437.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_439.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_441.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_442.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_444.png)

### 计算 Dbndiff2 和补充 Dbndiff

![](img/a3d21168dc6b383673c85535a01b0c8f_446.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_448.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_450.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_452.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_454.png)

`bnvar = (bndiff2).sum(0) / (n-1)`。`bndiff2 = bndiff**2`。

![](img/a3d21168dc6b383673c85535a01b0c8f_456.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_458.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_460.png)

**公式推导**：
*   `dbndiff2`：求和的反向传播是广播。`dbndiff2 = (1/(n-1)) * torch.ones_like(bndiff2) * dbnvar`（利用广播）。
*   `dbndiff` (第二条路径)：`d(x^2)/dx = 2x`，所以 `dbndiff += 2 * bndiff * dbndiff2`。

![](img/a3d21168dc6b383673c85535a01b0c8f_462.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_464.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_466.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_468.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_470.png)

**代码实现**：
```python
# 流向 bndiff2
dbndiff2 = (1.0/(n-1)) * torch.ones_like(bndiff2) * dbnvar
# 第二条路径流向 bndiff
dbndiff += 2 * bndiff * dbndiff2
```
验证通过，现在 `dbndiff` 也正确了。

![](img/a3d21168dc6b383673c85535a01b0c8f_472.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_474.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_476.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_478.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_480.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_482.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_484.png)

### 计算 Dhprebn 和 Dbndiff_mean (均值减法)

![](img/a3d21168dc6b383673c85535a01b0c8f_486.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_487.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_489.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_491.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_493.png)

`bndiff = hprebn - bndiff_mean`。`bndiff_mean = hprebn.mean(0, keepdim=True)`。

![](img/a3d21168dc6b383673c85535a01b0c8f_495.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_497.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_499.png)

**公式推导**：
*   `dhprebn` (第一条路径)：直接来自 `dbndiff`，梯度为 `1 * dbndiff`。
*   `dbndiff_mean`：来自 `dbndiff`，梯度为 `-dbndiff.sum(0, keepdim=True)`。
*   `hprebn` (第二条路径)：来自 `bndiff_mean`。均值的反向传播是广播。`dhprebn += (1/n) * torch.ones_like(hprebn) * dbndiff_mean`。

![](img/a3d21168dc6b383673c85535a01b0c8f_501.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_503.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_505.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_507.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_509.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_511.png)

**代码实现**：
```python
# 第一条路径
dhprebn = dbndiff.clone()
dbndiff_mean = (-dbndiff).sum(0, keepdim=True)
# 第二条路径
dhprebn += (1.0/n) * torch.ones_like(hprebn) * dbndiff_mean
```
验证通过。

![](img/a3d21168dc6b383673c85535a01b0c8f_513.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_515.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_516.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_518.png)

### 计算 Dembcat, Dw1, Db1 (第二个线性层)

![](img/a3d21168dc6b383673c85535a01b0c8f_520.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_522.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_524.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_525.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_527.png)

`hprebn = embcat @ w1 + b1`。这与第一个线性层类似。

![](img/a3d21168dc6b383673c85535a01b0c8f_529.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_531.png)

**公式推导（维度匹配）**：
*   `dembcat`：形状 `(32, 30)`。`dhprebn` 是 `(32, 64)`，`w1` 是 `(30, 64)`。`dembcat = dhprebn @ w1.T`。
*   `dw1`：形状 `(30, 64)`。`embcat` 是 `(32, 30)`。`dw1 = embcat.T @ dhprebn`。
*   `db1`：形状 `(64,)`。`db1 = dhprebn.sum(0)`。

![](img/a3d21168dc6b383673c85535a01b0c8f_533.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_535.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_536.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_538.png)

**代码实现**：
```python
dembcat = dhprebn @ w1.T
dw1 = embcat.T @ dhprebn
db1 = dhprebn.sum(0)
```
验证通过。

![](img/a3d21168dc6b383673c85535a01b0c8f_540.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_542.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_544.png)

### 计算 Demb 和 Dc (嵌入层)

![](img/a3d21168dc6b383673c85535a01b0c8f_546.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_548.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_550.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_552.png)

`embcat` 是 `emb` 张量的视图重塑。`emb` 由嵌入表 `C` 根据索引 `xb` 查找得到。

**公式推导**：
*   `demb`：视图操作的反向传播就是重塑梯度。`demb = dembcat.view(emb.shape)`。
*   `dC`：这是一个查找表（嵌入层）的反向传播。需要将 `demb` 中每个位置的梯度累加到 `C` 中对应的行。由于 `C` 的同一行可能被多个样本使用，梯度需要求和。

![](img/a3d21168dc6b383673c85535a01b0c8f_554.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_555.png)

**代码实现**：
```python
# 视图操作的反向传播
demb = dembcat.view(emb.shape)
# 嵌入查找的反向传播：将梯度散射回 C 的对应行
dC = torch.zeros_like(C)
for k in range(xb.shape[0]):
    for j in range(xb.shape[1]):
        idx = xb[k, j]
        dC[idx] += demb[k, j]
```
验证通过。至此，我们完成了整个计算图的逐原子反向传播！

![](img/a3d21168dc6b383673c85535a01b0c8f_557.png)

---

## 练习二：交叉熵损失的高效反向传播 🧮

上一节我们通过繁琐的步骤计算了损失函数的梯度。本节我们将看到，通过数学推导，可以直接得到损失对 logits 梯度的简洁表达式。

我们使用 PyTorch 的 `F.cross_entropy` 验证了其损失值与之前分解计算的结果相同，但效率更高。现在，我们希望直接计算 `dlogits`。

**数学推导**：
对于单个样本，损失 `L = -log(p_y)`，其中 `p_y = exp(l_y) / sum_j(exp(l_j))`，`l` 是 logits。
经过求导（需分情况讨论 `i == y` 和 `i != y`），可以得到简洁结果：
`dL / dl_i = p_i - (i == y ? 1 : 0)`
对于批量数据，还需要考虑平均操作带来的 `1/n` 缩放。

![](img/a3d21168dc6b383673c85535a01b0c8f_559.png)

**代码实现**：
```python
# 计算 softmax 概率
probs = F.softmax(logits, dim=1)
# 梯度公式：probs - 1(在正确位置)
dlogits = probs.clone()
dlogits[range(n), yb] -= 1
# 考虑损失的平均操作
dlogits /= n
```
验证表明，计算结果与 PyTorch 自动微分的梯度在数值上高度一致（差异 ~1e-9）。

**直观理解**：
`dlogits` 的每一行，可以看作是在推动概率分布：将正确类别位置的梯度减1，其余位置保持为模型预测的概率。每一行的梯度之和为0，意味着“推”和“拉”的力度平衡。模型预测错误越严重（正确位置概率低），梯度提供的“拉力”就越大。

---

![](img/a3d21168dc6b383673c85535a01b0c8f_561.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_563.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_564.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_566.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_567.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_568.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_569.png)

## 练习三：批归一化的高效反向传播 ⚙️

![](img/a3d21168dc6b383673c85535a01b0c8f_571.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_572.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_574.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_575.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_576.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_578.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_580.png)

类似地，我们可以为批归一化层推导一个统一、高效的反向传播公式，而不是像练习一那样逐原子操作。

![](img/a3d21168dc6b383673c85535a01b0c8f_582.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_584.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_586.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_588.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_590.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_591.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_593.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_595.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_596.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_598.png)

前向传播：`y = bn(x)`。给定 `dy`，求 `dx`。

![](img/a3d21168dc6b383673c85535a01b0c8f_600.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_602.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_604.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_606.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_607.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_609.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_611.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_613.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_614.png)

**数学推导**：
这是一个符号求导过程。设输入为 `x` (形状 `(n, d)`)，输出为 `y`。批归一化步骤包括计算均值 `mu`、方差 `sigma^2`、归一化值 `x_hat`，然后缩放偏移得到 `y`。
通过链式法则，并仔细处理求和与广播（特别是 `mu` 和 `sigma^2` 是标量对向量的影响），可以推导出 `dx` 的表达式。推导过程涉及较复杂的代数运算，但最终可以得到一个仅使用 `dy`、`x_hat`、`sigma^2` 等已知量的公式。

![](img/a3d21168dc6b383673c85535a01b0c8f_616.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_618.png)

![](img/a3d21168dc6b383673c85535a01b0c8f_620.png)

**代码实现**：
```python
# 简化版批归一化反向传播实现
# 注意：此代码块展示了概念，实际实现需要处理维度广播
# dbnraw 对应公式中的 dy
# bnraw 对应 x_hat
# bnvar 对应 sigma^2
# 以下是一个可能的向量化实现示意
d_bnraw = dbnraw # 来自上一层的梯度
# ... 根据推导公式计算 dhprebn ...
# 实际代码较长，涉及对推导公式的精确翻译和广播处理