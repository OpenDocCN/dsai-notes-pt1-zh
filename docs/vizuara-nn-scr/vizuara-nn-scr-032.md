#  032：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p32 P32 Lecture 31 - Dropout layers in neural networks - Full code [BV1iEHPzGEpa_p32]

## 🎼Dropout 层在神经网络中的应用

### 概述
在本节课中，我们将学习神经网络中的一个重要概念——Dropout。

### 什么是Dropout？

Dropout 是一种正则化技术，用于防止神经网络过拟合。它通过在训练过程中随机“丢弃”或禁用网络中的一些神经元来实现。

![](img/253ca2a72ec0e1612fa4a7ffb7b0a566_0.png)

### Dropout 的工作原理

以下是 Dropout 的工作原理：

1. 在训练过程中，对于每个训练样本，随机选择一定比例的神经元并将其输出置为零。
2. 在测试过程中，不执行任何丢弃操作，所有神经元都参与计算。

### Dropout 的公式

```python
P(dropout) = 1 - keep_prob
```

其中，`P(dropout)` 是神经元被丢弃的概率，`keep_prob` 是神经元被保留的概率。

### Dropout 的代码实现

以下是一个简单的 Dropout 层的代码实现：

```python
import numpy as np

![](img/253ca2a72ec0e1612fa4a7ffb7b0a566_2.png)

def dropout(X, keep_prob):
    """
    Dropout 层
    :param X: 输入数据
    :param keep_prob: 保留概率
    :return: 丢弃后的数据
    """
    mask = np.random.binomial(1, keep_prob, size=X.shape)
    return X * mask
```

### 总结

本节课中，我们一起学习了 Dropout 层在神经网络中的应用。Dropout 是一种有效的正则化技术，可以帮助我们防止神经网络过拟合。