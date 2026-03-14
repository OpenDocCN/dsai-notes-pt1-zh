# 语言建模的详解入门：构建 makemore：P3：激活与梯度，批归一化 🧠

![](img/88dd9899d3a9ef459d32c32418af4090_1.png)

![](img/88dd9899d3a9ef459d32c32418af4090_2.png)

![](img/88dd9899d3a9ef459d32c32418af4090_4.png)

![](img/88dd9899d3a9ef459d32c32418af4090_6.png)

![](img/88dd9899d3a9ef459d32c32418af4090_7.png)

![](img/88dd9899d3a9ef459d32c32418af4090_8.png)

![](img/88dd9899d3a9ef459d32c32418af4090_10.png)

![](img/88dd9899d3a9ef459d32c32418af4090_12.png)

![](img/88dd9899d3a9ef459d32c32418af4090_14.png)

![](img/88dd9899d3a9ef459d32c32418af4090_15.png)

![](img/88dd9899d3a9ef459d32c32418af4090_17.png)

![](img/88dd9899d3a9ef459d32c32418af4090_19.png)

在本节课中，我们将继续构建 makemore 项目，并深入探讨神经网络训练中的两个核心概念：**激活值**和**梯度**。我们将分析它们在训练过程中的行为，并学习如何通过**权重初始化**和**批归一化**等技术来稳定和优化深度神经网络的训练。

![](img/88dd9899d3a9ef459d32c32418af4090_20.png)

![](img/88dd9899d3a9ef459d32c32418af4090_22.png)

![](img/88dd9899d3a9ef459d32c32418af4090_24.png)

---

![](img/88dd9899d3a9ef459d32c32418af4090_26.png)

![](img/88dd9899d3a9ef459d32c32418af4090_28.png)

## 概述：为什么需要关注激活与梯度？

![](img/88dd9899d3a9ef459d32c32418af4090_30.png)

![](img/88dd9899d3a9ef459d32c32418af4090_32.png)

![](img/88dd9899d3a9ef459d32c32418af4090_34.png)

![](img/88dd9899d3a9ef459d32c32418af4090_36.png)

![](img/88dd9899d3a9ef459d32c32418af4090_38.png)

![](img/88dd9899d3a9ef459d32c32418af4090_40.png)

![](img/88dd9899d3a9ef459d32c32418af4090_42.png)

![](img/88dd9899d3a9ef459d32c32418af4090_44.png)

上一节我们实现了一个用于字符级语言建模的多层感知机。在转向更复杂的循环神经网络之前，我们需要对神经网络内部的激活值和反向传播的梯度有更深入的理解。理解它们的分布和行为，对于理解为何某些网络结构难以优化以及后续的改进技术至关重要。

![](img/88dd9899d3a9ef459d32c32418af4090_46.png)

![](img/88dd9899d3a9ef459d32c32418af4090_48.png)

![](img/88dd9899d3a9ef459d32c32418af4090_50.png)

![](img/88dd9899d3a9ef459d32c32418af4090_52.png)

![](img/88dd9899d3a9ef459d32c32418af4090_54.png)

![](img/88dd9899d3a9ef459d32c32418af4090_56.png)

![](img/88dd9899d3a9ef459d32c32418af4090_58.png)

![](img/88dd9899d3a9ef459d32c32418af4090_59.png)

![](img/88dd9899d3a9ef459d32c32418af4090_61.png)

![](img/88dd9899d3a9ef459d32c32418af4090_63.png)

---

![](img/88dd9899d3a9ef459d32c32418af4090_65.png)

![](img/88dd9899d3a9ef459d32c32418af4090_67.png)

![](img/88dd9899d3a9ef459d32c32418af4090_69.png)

![](img/88dd9899d3a9ef459d32c32418af4090_70.png)

![](img/88dd9899d3a9ef459d32c32418af4090_72.png)

![](img/88dd9899d3a9ef459d32c32418af4090_74.png)

![](img/88dd9899d3a9ef459d32c32418af4090_76.png)

![](img/88dd9899d3a9ef459d32c32418af4090_78.png)

![](img/88dd9899d3a9ef459d32c32418af4090_80.png)

## 1. 审视初始化的损失值 🔍

![](img/88dd9899d3a9ef459d32c32418af4090_82.png)

![](img/88dd9899d3a9ef459d32c32418af4090_84.png)

首先，我们审视当前模型的初始化状态。在第一次迭代时，我们记录到的损失值高达 27，这远高于我们的预期。

![](img/88dd9899d3a9ef459d32c32418af4090_86.png)

![](img/88dd9899d3a9ef459d32c32418af4090_88.png)

![](img/88dd9899d3a9ef459d32c32418af4090_90.png)

对于一个有 27 个可能字符的分类问题，在初始化时，网络没有理由偏向任何一个字符。因此，我们希望输出是一个均匀分布，每个字符的概率约为 `1/27`。对应的期望损失（负对数似然）应为：

![](img/88dd9899d3a9ef459d32c32418af4090_92.png)

![](img/88dd9899d3a9ef459d32c32418af4090_94.png)

![](img/88dd9899d3a9ef459d32c32418af4090_95.png)

![](img/88dd9899d3a9ef459d32c32418af4090_97.png)

![](img/88dd9899d3a9ef459d32c32418af4090_99.png)

```python
import torch
expected_loss = -torch.log(torch.tensor(1/27.0))
# 输出：tensor(3.2958)
```

![](img/88dd9899d3a9ef459d32c32418af4090_101.png)

![](img/88dd9899d3a9ef459d32c32418af4090_103.png)

![](img/88dd9899d3a9ef459d32c32418af4090_104.png)

![](img/88dd9899d3a9ef459d32c32418af4090_106.png)

![](img/88dd9899d3a9ef459d32c32418af4090_107.png)

初始损失 27 意味着网络在初始化时就“过于自信地犯错”，输出概率分布极不均匀。这通常是由于输出层的权重 `W2` 和偏置 `B2` 初始化不当，导致 `logits` 值过大。

![](img/88dd9899d3a9ef459d32c32418af4090_109.png)

![](img/88dd9899d3a9ef459d32c32418af4090_110.png)

![](img/88dd9899d3a9ef459d32c32418af4090_112.png)

![](img/88dd9899d3a9ef459d32c32418af4090_114.png)

![](img/88dd9899d3a9ef459d32c32418af4090_116.png)

![](img/88dd9899d3a9ef459d32c32418af4090_118.png)

![](img/88dd9899d3a9ef459d32c32418af4090_120.png)

![](img/88dd9899d3a9ef459d32c32418af4090_121.png)

![](img/88dd9899d3a9ef459d32c32418af4090_123.png)

![](img/88dd9899d3a9ef459d32c32418af4090_125.png)

![](img/88dd9899d3a9ef459d32c32418af4090_126.png)

![](img/88dd9899d3a9ef459d32c32418af4090_128.png)

![](img/88dd9899d3a9ef459d32c32418af4090_130.png)

![](img/88dd9899d3a9ef459d32c32418af4090_132.png)

![](img/88dd9899d3a9ef459d32c32418af4090_134.png)

**解决方案**：我们应缩小输出层权重 `W2` 的尺度，并将偏置 `B2` 初始化为零或接近零的小值。

![](img/88dd9899d3a9ef459d32c32418af4090_136.png)

![](img/88dd9899d3a9ef459d32c32418af4090_138.png)

![](img/88dd9899d3a9ef459d32c32418af4090_140.png)

![](img/88dd9899d3a9ef459d32c32418af4090_142.png)

![](img/88dd9899d3a9ef459d32c32418af4090_143.png)

![](img/88dd9899d3a9ef459d32c32418af4090_145.png)

![](img/88dd9899d3a9ef459d32c32418af4090_147.png)

![](img/88dd9899d3a9ef459d32c32418af4090_149.png)

```python
# 改进的初始化
W2 = torch.randn(n_hidden, vocab_size) * 0.01  # 缩小权重
B2 = torch.zeros(vocab_size)                    # 偏置置零
```

![](img/88dd9899d3a9ef459d32c32418af4090_151.png)

![](img/88dd9899d3a9ef459d32c32418af4090_153.png)

![](img/88dd9899d3a9ef459d32c32418af4090_155.png)

![](img/88dd9899d3a9ef459d32c32418af4090_157.png)

![](img/88dd9899d3a9ef459d32c32418af4090_159.png)

![](img/88dd9899d3a9ef459d32c32418af4090_161.png)

调整后，初始损失会降至接近 3.3 的合理范围，避免了训练初期不必要的“权重压缩”阶段，使优化更高效。

![](img/88dd9899d3a9ef459d32c32418af4090_163.png)

![](img/88dd9899d3a9ef459d32c32418af4090_165.png)

![](img/88dd9899d3a9ef459d32c32418af4090_167.png)

![](img/88dd9899d3a9ef459d32c32418af4090_168.png)

![](img/88dd9899d3a9ef459d32c32418af4090_170.png)

![](img/88dd9899d3a9ef459d32c32418af4090_172.png)

---

![](img/88dd9899d3a9ef459d32c32418af4090_174.png)

![](img/88dd9899d3a9ef459d32c32418af4090_176.png)

![](img/88dd9899d3a9ef459d32c32418af4090_178.png)

![](img/88dd9899d3a9ef459d32c32418af4090_180.png)

## 2. 隐藏层激活的饱和问题 ⚠️

![](img/88dd9899d3a9ef459d32c32418af4090_182.png)

![](img/88dd9899d3a9ef459d32c32418af4090_183.png)

![](img/88dd9899d3a9ef459d32c32418af4090_185.png)

![](img/88dd9899d3a9ef459d32c32418af4090_187.png)

![](img/88dd9899d3a9ef459d32c32418af4090_188.png)

![](img/88dd9899d3a9ef459d32c32418af4090_189.png)

![](img/88dd9899d3a9ef459d32c32418af4090_191.png)

即使输出层初始化正确，隐藏层的激活值也可能存在问题。我们使用 `tanh` 作为激活函数，其输出范围在 `(-1, 1)`。

![](img/88dd9899d3a9ef459d32c32418af4090_193.png)

![](img/88dd9899d3a9ef459d32c32418af4090_195.png)

![](img/88dd9899d3a9ef459d32c32418af4090_197.png)

在初始化时，如果输入到 `tanh` 的预激活值 `H_preact` 过大（例如，绝对值远大于 0），`tanh` 的输出就会饱和在 `-1` 或 `1` 附近。这会导致一个严重问题：在反向传播时，梯度会消失。

![](img/88dd9899d3a9ef459d32c32418af4090_199.png)

![](img/88dd9899d3a9ef459d32c32418af4090_201.png)

![](img/88dd9899d3a9ef459d32c32418af4090_203.png)

![](img/88dd9899d3a9ef459d32c32418af4090_205.png)

![](img/88dd9899d3a9ef459d32c32418af4090_206.png)

![](img/88dd9899d3a9ef459d32c32418af4090_208.png)

![](img/88dd9899d3a9ef459d32c32418af4090_210.png)

![](img/88dd9899d3a9ef459d32c32418af4090_211.png)

**梯度消失的原理**：
`tanh` 的导数为 `1 - tanh(x)^2`。当 `tanh(x)` 接近 `±1` 时，其导数接近 0。这意味着，对于饱和的神经元，其权重和偏置几乎无法通过梯度下降进行更新。

![](img/88dd9899d3a9ef459d32c32418af4090_213.png)

![](img/88dd9899d3a9ef459d32c32418af4090_214.png)

![](img/88dd9899d3a9ef459d32c32418af4090_215.png)

![](img/88dd9899d3a9ef459d32c32418af4090_217.png)

![](img/88dd9899d3a9ef459d32c32418af4090_219.png)

我们可以通过直方图检查隐藏层激活 `H` 的分布：

![](img/88dd9899d3a9ef459d32c32418af4090_221.png)

![](img/88dd9899d3a9ef459d32c32418af4090_223.png)

![](img/88dd9899d3a9ef459d32c32418af4090_225.png)

```python
import matplotlib.pyplot as plt
plt.hist(H.view(-1).tolist(), bins=50)
```

![](img/88dd9899d3a9ef459d32c32418af4090_227.png)

![](img/88dd9899d3a9ef459d32c32418af4090_229.png)

![](img/88dd9899d3a9ef459d32c32418af4090_231.png)

![](img/88dd9899d3a9ef459d32c32418af4090_233.png)

![](img/88dd9899d3a9ef459d32c32418af4090_235.png)

![](img/88dd9899d3a9ef459d32c32418af4090_237.png)

![](img/88dd9899d3a9ef459d32c32418af4090_239.png)

如果大量激活值集中在 `-1` 或 `1` 附近，就表明存在饱和问题。更严重的是，如果某个神经元对所有样本都饱和（即“死亡神经元”），它将永远无法学习。

![](img/88dd9899d3a9ef459d32c32418af4090_241.png)

![](img/88dd9899d3a9ef459d32c32418af4090_242.png)

![](img/88dd9899d3a9ef459d32c32418af4090_244.png)

![](img/88dd9899d3a9ef459d32c32418af4090_246.png)

![](img/88dd9899d3a9ef459d32c32418af4090_248.png)

![](img/88dd9899d3a9ef459d32c32418af4090_250.png)

![](img/88dd9899d3a9ef459d32c32418af4090_252.png)

**解决方案**：同样，我们需要调整第一层权重 `W1` 和偏置 `B1` 的初始化尺度，使 `H_preact` 的分布更集中，避免进入 `tanh` 的饱和区。

![](img/88dd9899d3a9ef459d32c32418af4090_254.png)

```python
# 改进的初始化
W1 = torch.randn(block_size * n_embd, n_hidden) * 0.2  # 适当缩小
B1 = torch.zeros(n_hidden) * 0.01                      # 小偏置引入熵
```

![](img/88dd9899d3a9ef459d32c32418af4090_256.png)

![](img/88dd9899d3a9ef459d32c32418af4090_258.png)

![](img/88dd9899d3a9ef459d32c32418af4090_260.png)

通过调整，`H_preact` 的分布会更接近均值为 0、标准差适中的高斯分布，`tanh` 的激活值将更多地落在其敏感的非饱和区域。

![](img/88dd9899d3a9ef459d32c32418af4090_262.png)

![](img/88dd9899d3a9ef459d32c32418af4090_264.png)

![](img/88dd9899d3a9ef459d32c32418af4090_266.png)

![](img/88dd9899d3a9ef459d32c32418af4090_268.png)

![](img/88dd9899d3a9ef459d32c32418af4090_270.png)

![](img/88dd9899d3a9ef459d32c32418af4090_272.png)

![](img/88dd9899d3a9ef459d32c32418af4090_274.png)

---

![](img/88dd9899d3a9ef459d32c32418af4090_276.png)

![](img/88dd9899d3a9ef459d32c32418af4090_278.png)

## 3. 权重初始化的数学原理 📐

![](img/88dd9899d3a9ef459d32c32418af4090_280.png)

![](img/88dd9899d3a9ef459d32c32418af4090_282.png)

![](img/88dd9899d3a9ef459d32c32418af4090_283.png)

![](img/88dd9899d3a9ef459d32c32418af4090_285.png)

![](img/88dd9899d3a9ef459d32c32418af4090_286.png)

![](img/88dd9899d3a9ef459d32c32418af4090_287.png)

![](img/88dd9899d3a9ef459d32c32418af4090_289.png)

![](img/88dd9899d3a9ef459d32c32418af4090_290.png)

![](img/88dd9899d3a9ef459d32c32418af4090_292.png)

![](img/88dd9899d3a9ef459d32c32418af4090_294.png)

![](img/88dd9899d3a9ef459d32c32418af4090_296.png)

![](img/88dd9899d3a9ef459d32c32418af4090_297.png)

对于深度网络，手动调整每个层的缩放因子是不现实的。我们需要一个系统化的初始化方法。

![](img/88dd9899d3a9ef459d32c32418af4090_299.png)

![](img/88dd9899d3a9ef459d32c32418af4090_301.png)

![](img/88dd9899d3a9ef459d32c32418af4090_303.png)

![](img/88dd9899d3a9ef459d32c32418af4090_305.png)

![](img/88dd9899d3a9ef459d32c32418af4090_307.png)

![](img/88dd9899d3a9ef459d32c32418af4090_308.png)

考虑一个简单的线性层：`y = x @ W`，其中 `x` 和 `W` 的元素都采样自标准正态分布 `N(0, 1)`。如果 `x` 的维度（`fan_in`）为 `n`，那么 `y` 中每个元素的方差将是 `n`（因为它是 `n` 个独立方差为 1 的变量之和）。因此，`y` 的标准差是 `sqrt(n)`。

![](img/88dd9899d3a9ef459d32c32418af4090_310.png)

![](img/88dd9899d3a9ef459d32c32418af4090_312.png)

![](img/88dd9899d3a9ef459d32c32418af4090_313.png)

![](img/88dd9899d3a9ef459d32c32418af4090_315.png)

![](img/88dd9899d3a9ef459d32c32418af4090_317.png)

![](img/88dd9899d3a9ef459d32c32418af4090_319.png)

![](img/88dd9899d3a9ef459d32c32418af4090_321.png)

![](img/88dd9899d3a9ef459d32c32418af4090_323.png)

为了使 `y` 的方差保持为 1（即保持激活尺度稳定），我们需要将权重 `W` 的尺度缩放 `1 / sqrt(fan_in)`。

![](img/88dd9899d3a9ef459d32c32418af4090_325.png)

![](img/88dd9899d3a9ef459d32c32418af4090_326.png)

![](img/88dd9899d3a9ef459d32c32418af4090_328.png)

![](img/88dd9899d3a9ef459d32c32418af4090_329.png)

![](img/88dd9899d3a9ef459d32c32418af4090_331.png)

![](img/88dd9899d3a9ef459d32c32418af4090_332.png)

**Kaiming 初始化**：
对于使用 ReLU 等激活函数的网络，由于它们会“丢弃”一半的分布（负值置零），需要进行补偿。Kaiming He 等人提出的初始化方法将权重的标准差设为 `sqrt(2 / fan_in)`。

![](img/88dd9899d3a9ef459d32c32418af4090_334.png)

![](img/88dd9899d3a9ef459d32c32418af4090_336.png)

![](img/88dd9899d3a9ef459d32c32418af4090_338.png)

![](img/88dd9899d3a9ef459d32c32418af4090_340.png)

![](img/88dd9899d3a9ef459d32c32418af4090_341.png)

![](img/88dd9899d3a9ef459d32c32418af4090_343.png)

![](img/88dd9899d3a9ef459d32c32418af4090_344.png)

![](img/88dd9899d3a9ef459d32c32418af4090_345.png)

![](img/88dd9899d3a9ef459d32c32418af4090_347.png)

对于 `tanh` 激活函数，PyTorch 中使用的增益因子是 `5/3`。因此，初始化公式为：

![](img/88dd9899d3a9ef459d32c32418af4090_349.png)

![](img/88dd9899d3a9ef459d32c32418af4090_350.png)

![](img/88dd9899d3a9ef459d32c32418af4090_352.png)

![](img/88dd9899d3a9ef459d32c32418af4090_354.png)

![](img/88dd9899d3a9ef459d32c32418af4090_356.png)

![](img/88dd9899d3a9ef459d32c32418af4090_357.png)

![](img/88dd9899d3a9ef459d32c32418af4090_358.png)

![](img/88dd9899d3a9ef459d32c32418af4090_360.png)

![](img/88dd9899d3a9ef459d32c32418af4090_361.png)

![](img/88dd9899d3a9ef459d32c32418af4090_363.png)

![](img/88dd9899d3a9ef459d32c32418af4090_364.png)

![](img/88dd9899d3a9ef459d32c32418af4090_365.png)

![](img/88dd9899d3a9ef459d32c32418af4090_367.png)

![](img/88dd9899d3a9ef459d32c32418af4090_369.png)

```python
gain = 5/3  # tanh 的推荐增益
std = gain / math.sqrt(fan_in)
W = torch.randn(fan_in, fan_out) * std
```

![](img/88dd9899d3a9ef459d32c32418af4090_371.png)

![](img/88dd9899d3a9ef459d32c32418af4090_372.png)

![](img/88dd9899d3a9ef459d32c32418af4090_373.png)

![](img/88dd9899d3a9ef459d32c32418af4090_375.png)

![](img/88dd9899d3a9ef459d32c32418af4090_376.png)

![](img/88dd9899d3a9ef459d32c32418af4090_378.png)

![](img/88dd9899d3a9ef459d32c32418af4090_380.png)

![](img/88dd9899d3a9ef459d32c32418af4090_382.png)

![](img/88dd9899d3a9ef459d32c32418af4090_384.png)

这种半原则性的方法可以扩展到更深层的网络，无需手动调参。

![](img/88dd9899d3a9ef459d32c32418af4090_386.png)

![](img/88dd9899d3a9ef459d32c32418af4090_388.png)

![](img/88dd9899d3a9ef459d32c32418af4090_389.png)

![](img/88dd9899d3a9ef459d32c32418af4090_390.png)

![](img/88dd9899d3a9ef459d32c32418af4090_392.png)

![](img/88dd9899d3a9ef459d32c32418af4090_394.png)

---

![](img/88dd9899d3a9ef459d32c32418af4090_396.png)

![](img/88dd9899d3a9ef459d32c32418af4090_398.png)

![](img/88dd9899d3a9ef459d32c32418af4090_399.png)

![](img/88dd9899d3a9ef459d32c32418af4090_401.png)

![](img/88dd9899d3a9ef459d32c32418af4090_403.png)

![](img/88dd9899d3a9ef459d32c32418af4090_405.png)

![](img/88dd9899d3a9ef459d32c32418af4090_406.png)

## 4. 批归一化：自动控制激活分布 🛡️

![](img/88dd9899d3a9ef459d32c32418af4090_408.png)

![](img/88dd9899d3a9ef459d32c32418af4090_409.png)

![](img/88dd9899d3a9ef459d32c32418af4090_411.png)

![](img/88dd9899d3a9ef459d32c32418af4090_413.png)

![](img/88dd9899d3a9ef459d32c32418af4090_415.png)

尽管精心设计的初始化有助于稳定训练，但对于非常深的网络，精确控制所有层的激活分布仍然非常困难。**批归一化** 应运而生，它通过一个可微操作，在训练过程中主动将激活值标准化。

![](img/88dd9899d3a9ef459d32c32418af4090_417.png)

![](img/88dd9899d3a9ef459d32c32418af4090_418.png)

![](img/88dd9899d3a9ef459d32c32418af4090_420.png)

![](img/88dd9899d3a9ef459d32c32418af4090_422.png)

![](img/88dd9899d3a9ef459d32c32418af4090_424.png)

![](img/88dd9899d3a9ef459d32c32418af4090_426.png)

![](img/88dd9899d3a9ef459d32c32418af4090_428.png)

![](img/88dd9899d3a9ef459d32c32418af4090_430.png)

**批归一化的核心思想**：
如果希望某层的输入（即前一层的激活）是均值为 0、方差为 1 的高斯分布，为什么不直接对每个小批量的数据进行标准化呢？

![](img/88dd9899d3a9ef459d32c32418af4090_432.png)

![](img/88dd9899d3a9ef459d32c32418af4090_434.png)

**批归一化层的操作**：
对于一个输入张量 `x`（形状为 `[batch_size, features]`）：
1.  计算当前小批量的均值 `μ_B` 和方差 `σ_B^2`。
2.  对输入进行标准化：`x_hat = (x - μ_B) / sqrt(σ_B^2 + ε)`，其中 `ε` 是一个防止除零的小常数。
3.  引入可学习的缩放参数 `γ` 和偏移参数 `β`：`y = γ * x_hat + β`。

![](img/88dd9899d3a9ef459d32c32418af4090_436.png)

![](img/88dd9899d3a9ef459d32c32418af4090_437.png)

![](img/88dd9899d3a9ef459d32c32418af4090_439.png)

![](img/88dd9899d3a9ef459d32c32418af4090_441.png)

![](img/88dd9899d3a9ef459d32c32418af4090_443.png)

在训练时，使用当前批次的统计量。同时，该层会维护一个**运行均值**和**运行方差**，通过指数移动平均在训练过程中更新。

![](img/88dd9899d3a9ef459d32c32418af4090_444.png)

![](img/88dd9899d3a9ef459d32c32418af4090_445.png)

![](img/88dd9899d3a9ef459d32c32418af4090_447.png)

![](img/88dd9899d3a9ef459d32c32418af4090_448.png)

![](img/88dd9899d3a9ef459d32c32418af4090_450.png)

![](img/88dd9899d3a9ef459d32c32418af4090_452.png)

![](img/88dd9899d3a9ef459d32c32418af4090_454.png)

![](img/88dd9899d3a9ef459d32c32418af4090_455.png)

![](img/88dd9899d3a9ef459d32c32418af4090_457.png)

![](img/88dd9899d3a9ef459d32c32418af4090_458.png)

![](img/88dd9899d3a9ef459d32c32418af4090_460.png)

![](img/88dd9899d3a9ef459d32c32418af4090_461.png)

在推理（或测试）时，则使用训练过程中积累的运行统计量，而不是当前批次的统计量。这使得网络可以对单个样本进行前向传播。

![](img/88dd9899d3a9ef459d32c32418af4090_463.png)

![](img/88dd9899d3a9ef459d32c32418af4090_465.png)

![](img/88dd9899d3a9ef459d32c32418af4090_467.png)

![](img/88dd9899d3a9ef459d32c32418af4090_469.png)

![](img/88dd9899d3a9ef459d32c32418af4090_471.png)

![](img/88dd9899d3a9ef459d32c32418af4090_472.png)

![](img/88dd9899d3a9ef459d32c32418af4090_474.png)

**批归一化的优势与代价**：
*   **优势**：极大地稳定了深度网络的训练，对初始化不那么敏感，并且由于引入了批次内的噪声，还具有一定的正则化效果。
*   **代价**：它耦合了批次内样本的计算。一个样本的输出会受到同批次其他样本的影响，这有时会导致意想不到的 bug，并使推理逻辑复杂化。

![](img/88dd9899d3a9ef459d32c32418af4090_476.png)

![](img/88dd9899d3a9ef459d32c32418af4090_477.png)

![](img/88dd9899d3a9ef459d32c32418af4090_478.png)

![](img/88dd9899d3a9ef459d32c32418af4090_479.png)

![](img/88dd9899d3a9ef459d32c32418af4090_481.png)

![](img/88dd9899d3a9ef459d32c32418af4090_482.png)

![](img/88dd9899d3a9ef459d32c32418af4090_484.png)

![](img/88dd9899d3a9ef459d32c32418af4090_486.png)

![](img/88dd9899d3a9ef459d32c32418af4090_488.png)

![](img/88dd9899d3a9ef459d32c32418af4090_490.png)

![](img/88dd9899d3a9ef459d32c32418af4090_492.png)

![](img/88dd9899d3a9ef459d32c32418af4090_494.png)

![](img/88dd9899d3a9ef459d32c32418af4090_495.png)

**代码示例**：
```python
# 训练阶段
bn_mean = x.mean(dim=0, keepdim=True)
bn_var = x.var(dim=0, keepdim=True, unbiased=False)
x_hat = (x - bn_mean) / torch.sqrt(bn_var + 1e-5)
y = bn_gain * x_hat + bn_bias

![](img/88dd9899d3a9ef459d32c32418af4090_497.png)

![](img/88dd9899d3a9ef459d32c32418af4090_498.png)

![](img/88dd9899d3a9ef459d32c32418af4090_499.png)

![](img/88dd9899d3a9ef459d32c32418af4090_500.png)

![](img/88dd9899d3a9ef459d32c32418af4090_502.png)

![](img/88dd9899d3a9ef459d32c32418af4090_504.png)

![](img/88dd9899d3a9ef459d32c32418af4090_505.png)

![](img/88dd9899d3a9ef459d32c32418af4090_507.png)

![](img/88dd9899d3a9ef459d32c32418af4090_509.png)

![](img/88dd9899d3a9ef459d32c32418af4090_511.png)

![](img/88dd9899d3a9ef459d32c32418af4090_513.png)

![](img/88dd9899d3a9ef459d32c32418af4090_514.png)

![](img/88dd9899d3a9ef459d32c32418af4090_515.png)

![](img/88dd9899d3a9ef459d32c32418af4090_516.png)

![](img/88dd9899d3a9ef459d32c32418af4090_517.png)

![](img/88dd9899d3a9ef459d32c32418af4090_519.png)

# 更新运行统计量 (动量更新)
running_mean = 0.999 * running_mean + 0.001 * bn_mean
running_var = 0.999 * running_var + 0.001 * bn_var

![](img/88dd9899d3a9ef459d32c32418af4090_521.png)

![](img/88dd9899d3a9ef459d32c32418af4090_523.png)

![](img/88dd9899d3a9ef459d32c32418af4090_525.png)

![](img/88dd9899d3a9ef459d32c32418af4090_527.png)

![](img/88dd9899d3a9ef459d32c32418af4090_529.png)

![](img/88dd9899d3a9ef459d32c32418af4090_531.png)

![](img/88dd9899d3a9ef459d32c32418af4090_532.png)

![](img/88dd9899d3a9ef459d32c32418af4090_533.png)

![](img/88dd9899d3a9ef459d32c32418af4090_535.png)

![](img/88dd9899d3a9ef459d32c32418af4090_536.png)

![](img/88dd9899d3a9ef459d32c32418af4090_538.png)

![](img/88dd9899d3a9ef459d32c32418af4090_540.png)

![](img/88dd9899d3a9ef459d32c32418af4090_542.png)

![](img/88dd9899d3a9ef459d32c32418af4090_544.png)

# 推理阶段
x_hat = (x - running_mean) / torch.sqrt(running_var + 1e-5)
y = bn_gain * x_hat + bn_bias
```

![](img/88dd9899d3a9ef459d32c32418af4090_546.png)

![](img/88dd9899d3a9ef459d32c32418af4090_548.png)

**使用注意**：当线性层后接批归一化层时，线性层的偏置 `bias` 是多余的，因为它会在归一化时被减去。因此，通常将线性层的 `bias=False`，让批归一化层的 `β` 参数负责偏移。

![](img/88dd9899d3a9ef459d32c32418af4090_550.png)

![](img/88dd9899d3a9ef459d32c32418af4090_551.png)

![](img/88dd9899d3a9ef459d32c32418af4090_552.png)

![](img/88dd9899d3a9ef459d32c32418af4090_554.png)

![](img/88dd9899d3a9ef459d32c32418af4090_556.png)

![](img/88dd9899d3a9ef459d32c32418af4090_558.png)

![](img/88dd9899d3a9ef459d32c32418af4090_560.png)

---

![](img/88dd9899d3a9ef459d32c32418af4090_562.png)

![](img/88dd9899d3a9ef459d32c32418af4090_564.png)

![](img/88dd9899d3a9ef459d32c32418af4090_566.png)

![](img/88dd9899d3a9ef459d32c32418af4090_568.png)

![](img/88dd9899d3a9ef459d32c32418af4090_570.png)

![](img/88dd9899d3a9ef459d32c32418af4090_571.png)

![](img/88dd9899d3a9ef459d32c32418af4090_573.png)

![](img/88dd9899d3a9ef459d32c32418af4090_575.png)

## 5. 诊断工具：监控训练健康度 📊

![](img/88dd9899d3a9ef459d32c32418af4090_577.png)

![](img/88dd9899d3a9ef459d32c32418af4090_579.png)

![](img/88dd9899d3a9ef459d32c32418af4090_581.png)

![](img/88dd9899d3a9ef459d32c32418af4090_583.png)

![](img/88dd9899d3a9ef459d32c32418af4090_585.png)

![](img/88dd9899d3a9ef459d32c32418af4090_587.png)

![](img/88dd9899d3a9ef459d32c32418af4090_589.png)

![](img/88dd9899d3a9ef459d32c32418af4090_591.png)

![](img/88dd9899d3a9ef459d32c32418af4090_593.png)

在训练神经网络时，监控以下统计量对于诊断问题至关重要：

![](img/88dd9899d3a9ef459d32c32418af4090_595.png)

![](img/88dd9899d3a9ef459d32c32418af4090_597.png)

![](img/88dd9899d3a9ef459d32c32418af4090_599.png)

![](img/88dd9899d3a9ef459d32c32418af4090_601.png)

![](img/88dd9899d3a9ef459d32c32418af4090_602.png)

![](img/88dd9899d3a9ef459d32c32418af4090_604.png)

![](img/88dd9899d3a9ef459d32c32418af4090_606.png)

![](img/88dd9899d3a9ef459d32c32418af4090_608.png)

![](img/88dd9899d3a9ef459d32c32418af4090_610.png)

![](img/88dd9899d3a9ef459d32c32418af4090_612.png)

![](img/88dd9899d3a9ef459d32c32418af4090_614.png)

![](img/88dd9899d3a9ef459d32c32418af4090_615.png)

1.  **前向传播激活直方图**：检查各层激活值是否过度饱和或过度稀疏。
2.  **反向传播梯度直方图**：检查梯度是否消失或爆炸。
3.  **参数更新比率**：计算 `(学习率 * 梯度).std() / 参数.std()` 的对数。这个比率反映了参数相对其当前值的变化幅度。一个经验法则是，这个比率在 `1e-3` 左右（即对数尺度下约为 -3）是比较健康的。如果远大于此，可能学习率太高；如果远小于此，可能学习率太低。

![](img/88dd9899d3a9ef459d32c32418af4090_617.png)

![](img/88dd9899d3a9ef459d32c32418af4090_619.png)

![](img/88dd9899d3a9ef459d32c32418af4090_620.png)

![](img/88dd9899d3a9ef459d32c32418af4090_621.png)

![](img/88dd9899d3a9ef459d32c32418af4090_623.png)

![](img/88dd9899d3a9ef459d32c32418af4090_625.png)

![](img/88dd9899d3a9ef459d32c32418af4090_627.png)

![](img/88dd9899d3a9ef459d32c32418af4090_629.png)

![](img/88dd9899d3a9ef459d32c32418af4090_631.png)

![](img/88dd9899d3a9ef459d32c32418af4090_633.png)

![](img/88dd9899d3a9ef459d32c32418af4090_634.png)

![](img/88dd9899d3a9ef459d32c32418af4090_635.png)

![](img/88dd9899d3a9ef459d32c32418af4090_636.png)

通过绘制这些比率随训练迭代的变化曲线，可以直观地判断网络各层的学习速度是否均衡，以及学习率设置是否合适。

![](img/88dd9899d3a9ef459d32c32418af4090_638.png)

![](img/88dd9899d3a9ef459d32c32418af4090_639.png)

![](img/88dd9899d3a9ef459d32c32418af4090_641.png)

![](img/88dd9899d3a9ef459d32c32418af4090_643.png)

![](img/88dd9899d3a9ef459d32c32418af4090_645.png)

![](img/88dd9899d3a9ef459d32c32418af4090_647.png)

![](img/88dd9899d3a9ef459d32c32418af4090_649.png)

![](img/88dd9899d3a9ef459d32c32418af4090_651.png)

---

![](img/88dd9899d3a9ef459d32c32418af4090_653.png)

![](img/88dd9899d3a9ef459d32c32418af4090_655.png)

![](img/88dd9899d3a9ef459d32c32418af4090_657.png)

![](img/88dd9899d3a9ef459d32c32418af4090_659.png)

![](img/88dd9899d3a9ef459d32c32418af4090_661.png)

## 总结 🎯

![](img/88dd9899d3a9ef459d32c32418af4090_663.png)

![](img/88dd9899d3a9ef459d32c32418af4090_665.png)

![](img/88dd9899d3a9ef459d32c32418af4090_667.png)

![](img/88dd9899d3a9ef459d32c32418af4090_669.png)

![](img/88dd9899d3a9ef459d32c32418af4090_671.png)

![](img/88dd9899d3a9ef459d32c32418af4090_672.png)

![](img/88dd9899d3a9ef459d32c32418af4090_674.png)

![](img/88dd9899d3a9ef459d32c32418af4090_675.png)

![](img/88dd9899d3a9ef459d32c32418af4090_677.png)

![](img/88dd9899d3a9ef459d32c32418af4090_679.png)

![](img/88dd9899d3a9ef459d32c32418af4090_681.png)

![](img/88dd9899d3a9ef459d32c32418af4090_683.png)

本节课我们一起深入探讨了神经网络训练的内部机制：

![](img/88dd9899d3a9ef459d32c32418af4090_685.png)

![](img/88dd9899d3a9ef459d32c32418af4090_686.png)

![](img/88dd9899d3a9ef459d32c32418af4090_687.png)

![](img/88dd9899d3a9ef459d32c32418af4090_689.png)

![](img/88dd9899d3a9ef459d32c32418af4090_690.png)

![](img/88dd9899d3a9ef459d32c32418af4090_692.png)

![](img/88dd9899d3a9ef459d32c32418af4090_693.png)

![](img/88dd9899d3a9ef459d32c32418af4090_695.png)

![](img/88dd9899d3a9ef459d32c32418af4090_696.png)

![](img/88dd9899d3a9ef459d32c32418af4090_697.png)

1.  **初始化的重要性**：不恰当的初始化会导致输出层过于自信、隐藏层饱和，从而使得训练初期低效，甚至阻碍网络学习。我们学习了如何通过缩放权重来获得合理的初始激活分布。
2.  **梯度流与饱和**：我们分析了 `tanh`、`ReLU` 等激活函数在饱和区会导致梯度消失的问题，理解了“死亡神经元”的概念。
3.  **系统化初始化**：介绍了基于方差守恒原则的 Kaiming 初始化方法，它为我们提供了一种无需手动调参的、可扩展的初始化策略。
4.  **批归一化**：作为一种强大的技术，批归一化通过在网络内部显式地标准化激活值，极大地稳定了深度网络的训练，降低了对初始化的精细要求。我们也了解了其内部机制和使用时的注意事项。
5.  **训练诊断**：我们学习了一套实用的工具，用于监控前向激活、反向梯度以及参数更新比率，这些工具能帮助我们判断网络是否处于健康训练状态。

![](img/88dd9899d3a9ef459d32c32418af4090_699.png)

![](img/88dd9899d3a9ef459d32c32418af4090_701.png)

![](img/88dd9899d3a9ef459d32c32418af4090_702.png)

![](img/88dd9899d3a9ef459d32c32418af4090_703.png)

![](img/88dd9899d3a9ef459d32c32418af4090_704.png)

![](img/88dd9899d3a9ef459d32c32418af4090_705.png)

![](img/88dd9899d3a9ef459d32c32418af4090_706.png)

![](img/88dd9899d3a9ef459d32c32418af4090_708.png)

![](img/88dd9899d3a9ef459d32c32418af4090_709.png)

![](img/88dd9899d3a9ef459d32c32418af4090_711.png)

![](img/88dd9899d3a9ef459d32c32418af4090_712.png)

![](img/88dd9899d3a9ef459d32c32418af4090_714.png)

理解激活和梯度的行为是掌握深度学习的基石。虽然现代技术（如批归一化、残差连接、高级优化器）让训练变得更加鲁棒，但底层原理依然至关重要，尤其是在设计和调试新模型架构时。在接下来的课程中，我们将把这些知识应用于更复杂的循环神经网络。