#  004：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p04 P4 Lecture 4 - Implementing the Dense Layer Class in Python [BV1iEHPzGEpa_p4]

## 🎼 从零开始构建神经网络：第4讲 - 在Python中实现密集层类

### 概述
在本节课中，我们将学习如何在Python中实现神经网络中的密集层类。

![](img/8b918287c28caf724f2ed3f7a7cc2d99_0.png)

### 密集层类实现

#### 1. 导入必要的库
```python
import numpy as np
```

#### 2. 创建密集层类
```python
class DenseLayer:
    def __init__(self, input_size, output_size):
        self.weights = np.random.randn(output_size, input_size)
        self.bias = np.zeros((output_size, 1))
        self.input_size = input_size
        self.output_size = output_size
```

#### 3. 前向传播
```python
def forward(self, x):
    self.z = np.dot(self.weights, x) + self.bias
    return self.z
```

#### 4. 反向传播
```python
def backward(self, x, y, learning_rate):
    output_error = y - self.z
    output_delta = output_error * self.z
    input_delta = output_delta.dot(self.weights.T)
    self.weights -= learning_rate * input_delta.dot(x.T)
    self.bias -= learning_rate * output_delta
```

### 总结
本节课中我们一起学习了如何在Python中实现神经网络中的密集层类。通过创建一个类，我们定义了输入和输出大小、权重和偏置，并实现了前向传播和反向传播方法。