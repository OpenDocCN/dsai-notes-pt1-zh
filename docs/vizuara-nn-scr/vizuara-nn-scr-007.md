#  007：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p07 P7 讲座 7 - 使用Python编写一个神经网络的正向传播（无损失）[BV1iEHPzGEpa_p7]

🎼是的。欢迎来到神经网络从零开始构建系列讲座的这一讲。首先。

我非常感谢您一直坚持学习这个系列。而且，这真是太好了。

---

## 概述

在本节课中，我们将学习如何使用Python编写一个神经网络的正向传播过程，不涉及损失计算。

---

## 准备工作

在开始之前，请确保您已经安装了以下Python库：

- NumPy
- Matplotlib

您可以使用以下命令安装这些库：

```bash
pip install numpy matplotlib
```

![](img/04624f22661f15b080023abaf21b2dde_0.png)

---

## 正向传播过程

以下是使用Python实现神经网络正向传播过程的步骤：

### 1. 导入必要的库

```python
import numpy as np
import matplotlib.pyplot as plt
```

### 2. 定义神经网络结构

```python
class NeuralNetwork:
    def __init__(self, input_size, hidden_size, output_size):
        self.input_size = input_size
        self.hidden_size = hidden_size
        self.output_size = output_size
        
        # 初始化权重
        self.weights_input_to_hidden = np.random.randn(input_size, hidden_size)
        self.weights_hidden_to_output = np.random.randn(hidden_size, output_size)
        
        # 初始化偏置
        self.bias_hidden = np.zeros((1, hidden_size))
        self.bias_output = np.zeros((1, output_size))
```

### 3. 定义激活函数

```python
def sigmoid(x):
    return 1 / (1 + np.exp(-x))
```

### 4. 定义正向传播函数

```python
def forward_pass(self, inputs):
    # 输入层到隐藏层的线性变换
    hidden_linear = np.dot(inputs, self.weights_input_to_hidden)
    # 应用激活函数
    hidden = sigmoid(hidden_linear + self.bias_hidden)
    
    # 隐藏层到输出层的线性变换
    output_linear = np.dot(hidden, self.weights_hidden_to_output)
    # 应用激活函数
    output = sigmoid(output_linear + self.bias_output)
    
    return output
```

### 5. 测试正向传播

```python
# 创建神经网络实例
nn = NeuralNetwork(2, 3, 1)

# 输入数据
inputs = np.array([[0, 0], [0, 1], [1, 0], [1, 1]])

# 计算输出
outputs = nn.forward_pass(inputs)

# 打印输出
print(outputs)
```

---

## 总结

本节课中，我们一起学习了如何使用Python编写一个神经网络的正向传播过程。在下一节课中，我们将学习如何计算损失并优化神经网络。