# 007：DDIM加速采样 🚀

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_0.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_2.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_3.png)

在本节课中，我们将学习一种名为DDIM（去噪扩散隐式模型）的新采样方法。该方法比我们目前使用的DDPM采样方法效率高出10倍以上。我们将了解其工作原理，并通过代码示例直观地对比其速度优势。

## 概述：为何需要更快的采样？

上一节我们介绍了DDPM的标准采样过程。本节中我们来看看其面临的主要挑战：采样速度慢。这是因为DDPM采样通常需要经历数百个时间步，且每一步都依赖于前一步，形成一个马尔可夫链过程。为了获得更多、更快的图像生成结果，研究者们开发了多种新的采样器，其中DDIM便是非常流行的一种。

## DDIM的核心思想：打破马尔可夫链

DDIM之所以更快，是因为它能够**跳过时间步**。它不再需要从时间步500、499、498……这样顺序执行。其关键在于**移除了采样过程中的所有随机性**，使其成为一个确定性的过程。本质上，DDIM首先预测最终输出的一个粗略草图，然后通过去噪过程对其进行细化。

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_5.png)

以下是DDIM与DDPM采样过程的直观对比：
*   **DDPM（左侧）**：遵循标准的马尔可夫链，逐步去噪，过程较慢。
*   **DDIM（右侧）**：可以跳过大量中间步骤，快速形成图像轮廓并细化。

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_7.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_9.png)

## 代码实现与对比

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_11.png)

以下是DDIM采样算法的关键部分。请注意，其中涉及一个步长参数，它决定了我们跳过的步数。

```python
# DDIM采样函数示例（关键参数）
n_steps = 20  # 总采样步数，远小于DDPM的500步
step_size = max(1, total_timesteps // n_steps)  # 计算跳过的步长
```

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_13.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_14.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_15.png)

在实验中，我们设置`n_steps=20`，这意味着采样过程大约只执行了原来的1/25（500/20）。运行后可以观察到，DDIM的采样速度极快，几乎瞬间就能看到图像的基本形态。

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_17.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_19.png)

## 速度与质量的权衡

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_21.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_22.png)

使用这种更快的采样方法时，并不总能获得与运行完整DDPM采样（如500步）相同水平的图像质量。但经验表明，对于一个在500步上训练的模型：
*   如果**采样500步**，DDPM通常表现更好。
*   如果**采样步数少于500步**，DDIM的表现则要优秀得多。

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_24.png)

因此，DDIM在追求速度的场景下具有巨大优势。

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_26.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_27.png)

## 性能对比测试

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_29.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_30.png)

我们可以使用`timeit`等工具来量化比较DDIM与DDPM的采样速度。在典型的测试中，DDIM能带来**数量级级别的加速**。建议你在自己的笔记本中运行代码，亲身体验这种速度差异。

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_32.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_33.png)

## 总结

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_34.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_35.png)

![](img/f8a2dc18a9ee4c9d787dd7ed2c145d6c_37.png)

本节课中我们一起学习了DDIM加速采样方法。我们了解到DDIM通过**跳过时间步**和**采用确定性采样过程**，打破了DDPM的马尔可夫链假设，从而实现了十倍以上的采样加速。虽然在高步数采样下DDPM的质量可能更优，但在低步数采样中，DDIM在速度和质量上提供了更佳的平衡。这为扩散模型的实际应用打开了新的大门。