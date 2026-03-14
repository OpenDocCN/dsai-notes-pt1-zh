#  028： Coded from scratch [BV1iEHPzGEpa_p28]

![](img/a3a86e176edf0632d4c17c05b98d6d4e_0.png)

🎼Yeah。Hello， everyone。Welcome to this video， which is titled neuralural Network from Sctch。

 a Masterclass。In this video， our main goal would be to design an entire neural network。

## 概述

在本节课中，我们将学习如何从头开始构建一个神经网络。我们将从基础概念开始，逐步深入到实现细节。

## 神经网络基础

### 神经元

神经网络由许多神经元组成，每个神经元都负责处理输入数据并产生输出。

**公式**： \( y = f(W \cdot x + b) \)

其中，\( y \) 是输出，\( W \) 是权重，\( x \) 是输入，\( b \) 是偏置，\( f \) 是激活函数。

### 激活函数

激活函数用于引入非线性，使神经网络能够学习复杂模式。

![](img/a3a86e176edf0632d4c17c05b98d6d4e_0.png)

**代码**： 
```python
def sigmoid(x):
    return 1 / (1 + math.exp(-x))
```

### 网络结构

神经网络通常由多个层组成，包括输入层、隐藏层和输出层。

- **输入层**：接收输入数据。
- **隐藏层**：处理输入数据并提取特征。
- **输出层**：产生最终输出。

## 构建神经网络

### 步骤 1：定义网络结构

首先，我们需要定义网络的层数和每层的神经元数量。

**代码**：
```python
class NeuralNetwork:
    def __init__(self, input_size, hidden_size, output_size):
        self.input_size = input_size
        self.hidden_size = hidden_size
        self.output_size = output_size
        # 初始化权重和偏置
```

### 步骤 2：初始化权重和偏置

权重和偏置是神经网络的关键参数，需要随机初始化。

**代码**：
```python
def initialize_weights(self):
    self.weights = np.random.randn(self.hidden_size, self.input_size)
    self.bias = np.random.randn(self.hidden_size, 1)
```

### 步骤 3：前向传播

前向传播是将输入数据通过网络，计算输出。

**代码**：
```python
def forward(self, x):
    self.hidden_layer = np.dot(self.weights, x) + self.bias
    self.output = sigmoid(self.hidden_layer)
```

### 步骤 4：反向传播

反向传播是计算损失并更新权重和偏置。

**代码**：
```python
def backward(self, x, y):
    error = y - self.output
    d_output = error * sigmoid_derivative(self.output)
    d_hidden_layer = np.dot(d_output, self.weights.T)
    d_weights = np.dot(d_hidden_layer, x)
    d_bias = np.sum(d_hidden_layer, axis=0)
    # 更新权重和偏置
```

### 步骤 5：训练网络

使用训练数据对网络进行训练。

**代码**：
```python
def train(self, x, y, epochs):
    for epoch in range(epochs):
        self.forward(x)
        self.backward(x, y)
```

## 总结

本节课中，我们一起学习了如何从头开始构建一个神经网络。我们介绍了神经元、激活函数、网络结构以及构建神经网络的步骤。希望这些知识能够帮助你更好地理解神经网络的工作原理。