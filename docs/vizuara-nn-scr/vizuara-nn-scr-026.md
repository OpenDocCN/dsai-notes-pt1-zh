#  026：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p26 P26 Lecture 26 - Coding the RMSProp Optimizer with Neural Network training [BV1iEHPzGEpa_p26]

![](img/4a2a0d7855f1b20b5cda524d7d1ee88c_0.png)

🎼Yeah. Hello, everyone. Welcome to this lecture in the neuralural Network from Sctch series.

In this lecture, we are going to learn about a new optimizer for training neural networks. And that is called as.

---

## 概述

在本节课中，我们将学习如何使用RMSProp优化器来训练神经网络。

---

## RMSProp优化器简介

RMSProp（Root Mean Square Propagation）是一种用于训练神经网络的优化算法。它通过调整学习率来加速训练过程，并减少震荡。

![](img/4a2a0d7855f1b20b5cda524d7d1ee88c_0.png)

**公式**：
\[ \text{learning\_rate} = \frac{\alpha}{\sqrt{\text{momentum} + \epsilon}} \]

其中：
- \(\alpha\) 是初始学习率。
- \(\text{momentum}\) 是动量项，用于加速梯度下降。
- \(\epsilon\) 是一个很小的正数，用于防止除以零。

---

## 编码RMSProp优化器

以下是使用Python实现RMSProp优化器的代码示例：

```python
class RMSPropOptimizer:
    def __init__(self, learning_rate=0.01, momentum=0.9, epsilon=1e-8):
        self.learning_rate = learning_rate
        self.momentum = momentum
        self.epsilon = epsilon
        self.velocity = 0

    def update(self, gradient):
        self.velocity = self.momentum * self.velocity + (1 - self.momentum) * gradient ** 2
        self.learning_rate = self.learning_rate / (self.velocity + self.epsilon) ** 0.5
        return -self.learning_rate * gradient
```

---

## 使用RMSProp优化器训练神经网络

以下是使用RMSProp优化器训练神经网络的示例：

```python
# 假设我们有一个简单的神经网络
# ...

# 创建RMSProp优化器
optimizer = RMSPropOptimizer(learning_rate=0.01, momentum=0.9, epsilon=1e-8)

# 训练神经网络
for epoch in range(num_epochs):
    for data, target in dataset:
        # 计算梯度
        # ...

        # 更新参数
        optimizer.update(gradient)
        # ...
```

---

## 总结

本节课中，我们学习了如何使用RMSProp优化器来训练神经网络。RMSProp通过调整学习率来加速训练过程，并减少震荡。通过编码RMSProp优化器，我们可以将其应用于神经网络训练中。

---

通过本节课的学习，我们了解了RMSProp优化器的基本原理和实现方法。希望这些知识能够帮助你在神经网络训练中取得更好的效果。