#  027：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p27 P27 Lecture 27 -Coding the ADAM optimizer for neural networks [BV1iEHPzGEpa_p27]

![](img/a45ef7f50066f1f582d60c0a204ff373_0.png)

🎼Yeah. Hello everyone, welcome to this lecture in the neuralural Network from Sctch series.

Today I' am very excited because today we are going to learn about an optimizer.

Which is used in most modern machine.

## 概述

在本节课中，我们将学习如何为神经网络编写ADAM优化器。ADAM是一种广泛使用的优化算法，用于机器学习中的梯度下降。

## ADAM优化器简介

![](img/a45ef7f50066f1f582d60c0a204ff373_0.png)

ADAM（Adaptive Moment Estimation）是一种自适应学习率优化算法，它结合了Momentum和RMSprop的优点。以下是ADAM优化器的一些关键概念：

- **学习率（Learning Rate）**：控制模型参数更新的步长。
- **动量（Momentum）**：利用之前梯度的信息来加速学习过程。
- **RMSprop**：一种自适应学习率方法，根据参数的平方梯度来调整学习率。

### 公式

ADAM优化器的更新公式如下：

$$
\begin{align*}
v_t & = \beta_1 v_{t-1} + (1 - \beta_1) g_t \\
s_t & = \beta_2 s_{t-1} + (1 - \beta_2) g_t^2 \\
\hat{v}_t & = \frac{v_t}{1 - \beta_1^t} \\
\hat{s}_t & = \frac{s_t}{1 - \beta_2^t} \\
\theta_t & = \theta_{t-1} - \frac{\alpha}{\sqrt{\hat{s}_t} + \epsilon} \hat{v}_t
\end{align*}
$$

其中：

- $v_t$ 和 $s_t$ 分别是动量和平方梯度的估计值。
- $\beta_1$ 和 $\beta_2$ 是超参数，通常取值为0.9。
- $\alpha$ 是学习率。
- $\epsilon$ 是一个很小的常数，用于防止除以零。

## 编写ADAM优化器

以下是使用Python编写ADAM优化器的基本步骤：

1. **初始化参数**：设置学习率、动量、RMSprop等超参数。
2. **计算梯度**：计算模型参数的梯度。
3. **更新参数**：根据梯度更新模型参数。

### 代码

```python
class AdamOptimizer:
    def __init__(self, learning_rate=0.001, beta1=0.9, beta2=0.999, epsilon=1e-8):
        self.learning_rate = learning_rate
        self.beta1 = beta1
        self.beta2 = beta2
        self.epsilon = epsilon
        self.m = 0
        self.v = 0
        self.s = 0

    def update(self, params, gradients):
        self.m = self.beta1 * self.m + (1 - self.beta1) * gradients
        self.v = self.beta2 * self.v + (1 - self.beta2) * (gradients ** 2)
        m_hat = self.m / (1 - self.beta1 ** self.t)
        v_hat = self.v / (1 - self.beta2 ** self.t)
        params -= self.learning_rate * (m_hat / (self.epsilon + v_hat ** 0.5))
```

## 总结

本节课中，我们学习了如何为神经网络编写ADAM优化器。通过理解ADAM优化器的原理和公式，我们可以更好地调整模型参数，提高模型的性能。希望这节课对您有所帮助！