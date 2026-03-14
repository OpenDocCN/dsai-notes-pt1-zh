#  025：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p25 P25 Lecture 25 - 编写ADAGRAD优化器用于神经网络训练

🎼大家好，欢迎来到神经网络从零开始的系列课程。今天，我们将学习一个重要的优化器，用于神经网络训练。

## 概述

在本节课中，我们将学习如何编写ADAGRAD优化器，这是一种用于神经网络训练的优化算法。

## ADAGRAD优化器简介

ADAGRAD（Adaptive Gradient）是一种自适应学习率优化算法，它根据每个参数的历史梯度来调整学习率。这种算法特别适用于处理稀疏数据。

### ADAGRAD公式

![](img/c9f700588b9509ef70ce4c6f8419e7b8_0.png)

ADAGRAD的更新规则可以用以下公式表示：

$$
 \theta_{t+1} = \theta_{t} - \frac{\eta}{\sqrt{\sum_{i=1}^{n} (g_{t}^{(i)})^2}} \cdot g_{t}^{(i)} 
$$

其中：
- $\theta_{t}$ 是第 $t$ 次迭代的参数。
- $\theta_{t+1}$ 是第 $t+1$ 次迭代的参数。
- $\eta$ 是学习率。
- $g_{t}^{(i)}$ 是第 $t$ 次迭代中第 $i$ 个参数的梯度。

## 编写ADAGRAD优化器

以下是使用Python编写ADAGRAD优化器的步骤：

1. **初始化参数**：首先，我们需要初始化参数和梯度。

2. **计算梯度**：在每次迭代中，计算损失函数关于参数的梯度。

3. **更新参数**：使用ADAGRAD公式更新参数。

### 示例代码

```python
def adagrad_optimizer(params, gradients, learning_rate=0.01):
    for i, param in enumerate(params):
        grad = gradients[i]
        squared_grad_sum = squared_grad_sum[i] + grad ** 2
        updated_param = param - (learning_rate / (sqrt(squared_grad_sum) + 1e-8)) * grad
        params[i] = updated_param
        squared_grad_sum[i] = squared_grad_sum
```

## 总结

本节课中，我们一起学习了如何编写ADAGRAD优化器。通过理解ADAGRAD的原理和公式，我们可以更好地应用于神经网络训练中。