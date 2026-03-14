# 课程 P2：L2- 神经网络 🧠

![](img/46ae3dd4f47308ef10799ea81647c68c_1.png)

在本节课中，我们将学习如何仅使用 Python 和 Numpy 库，从零开始实现一个基础的神经网络。我们将从理解其核心概念开始，逐步构建一个能够进行简单分类的神经网络模型。

---

![](img/46ae3dd4f47308ef10799ea81647c68c_3.png)

## 概述

![](img/46ae3dd4f47308ef10799ea81647c68c_5.png)

![](img/46ae3dd4f47308ef10799ea81647c68c_6.png)

神经网络是机器学习中一个强大的模型，它模仿人脑神经元的工作方式来处理信息。一个基本的神经网络由输入层、隐藏层和输出层组成，每层包含多个“神经元”。神经元之间通过带有权重的连接进行信息传递，并通过激活函数引入非线性。

![](img/46ae3dd4f47308ef10799ea81647c68c_8.png)

上一节我们介绍了 K-近邻算法，它是一种基于实例的学习方法。本节中，我们将看看另一种截然不同的、基于模型的学习方法——神经网络。

---

## 神经网络的核心概念

![](img/46ae3dd4f47308ef10799ea81647c68c_10.png)

![](img/46ae3dd4f47308ef10799ea81647c68c_11.png)

神经网络的核心在于**前向传播**和**反向传播**两个过程。
*   **前向传播**：输入数据从输入层流向输出层，得到预测结果。
*   **反向传播**：根据预测结果与真实值之间的误差，从输出层反向计算并更新网络中的权重，以减小误差。

![](img/46ae3dd4f47308ef10799ea81647c68c_13.png)

### 1. 神经元与激活函数

![](img/46ae3dd4f47308ef10799ea81647c68c_15.png)

![](img/46ae3dd4f47308ef10799ea81647c68c_16.png)

单个神经元接收多个输入，计算加权和后，通过一个**激活函数**产生输出。常用的激活函数是 **Sigmoid** 函数，它可以将任何值映射到 (0, 1) 区间，非常适合二分类问题。

其公式为：
**`σ(z) = 1 / (1 + e^(-z))`**

在代码中，我们可以这样实现：
```python
import numpy as np
def sigmoid(z):
    return 1 / (1 + np.exp(-z))
```

### 2. 网络结构与前向传播

![](img/46ae3dd4f47308ef10799ea81647c68c_18.png)

我们构建一个简单的三层网络（输入层、一层隐藏层、输出层）。前向传播的过程如下：
1.  计算隐藏层输入：`Z1 = X · W1 + b1`
2.  应用激活函数得到隐藏层输出：`A1 = sigmoid(Z1)`
3.  计算输出层输入：`Z2 = A1 · W2 + b2`
4.  应用激活函数得到最终输出（预测值）：`A2 = sigmoid(Z2)`

![](img/46ae3dd4f47308ef10799ea81647c68c_20.png)

其中，`X` 是输入数据，`W1`、`b1`、`W2`、`b2` 是需要学习的权重和偏置参数。

![](img/46ae3dd4f47308ef10799ea81647c68c_22.png)

### 3. 损失函数与反向传播

![](img/46ae3dd4f47308ef10799ea81647c68c_23.png)

![](img/46ae3dd4f47308ef10799ea81647c68c_25.png)

我们使用**交叉熵损失函数**来衡量预测值 `A2` 与真实标签 `Y` 之间的差距。对于二分类，损失 `L` 的公式为：
**`L = - (Y * log(A2) + (1-Y) * log(1-A2))` 的平均值**

![](img/46ae3dd4f47308ef10799ea81647c68c_27.png)

![](img/46ae3dd4f47308ef10799ea81647c68c_29.png)

反向传播的目标是计算损失函数相对于每个参数（`W1, b1, W2, b2`）的梯度（导数），然后使用梯度下降法更新参数。以下是梯度的计算公式（推导过程略）：
*   `dZ2 = A2 - Y`
*   `dW2 = (1/m) * A1.T · dZ2`
*   `db2 = (1/m) * np.sum(dZ2, axis=1, keepdims=True)`
*   `dZ1 = W2.T · dZ2 * sigmoid_derivative(Z1)` # `sigmoid_derivative(A1) = A1 * (1 - A1)`
*   `dW1 = (1/m) * X.T · dZ1`
*   `db1 = (1/m) * np.sum(dZ1, axis=1, keepdims=True)`

![](img/46ae3dd4f47308ef10799ea81647c68c_31.png)

参数更新公式为（`α` 是学习率）：
**`W = W - α * dW`**
**`b = b - α * db`**

---

## 实现步骤

以下是实现一个简单神经网络的步骤概览。

![](img/46ae3dd4f47308ef10799ea81647c68c_33.png)

![](img/46ae3dd4f47308ef10799ea81647c68c_35.png)

### 第一步：初始化参数
我们需要随机初始化网络的权重 `W` 和偏置 `b`。
```python
def initialize_parameters(n_x, n_h, n_y):
    W1 = np.random.randn(n_h, n_x) * 0.01
    b1 = np.zeros((n_h, 1))
    W2 = np.random.randn(n_y, n_h) * 0.01
    b2 = np.zeros((n_y, 1))
    return {"W1": W1, "b1": b1, "W2": W2, "b2": b2}
```

### 第二步：实现前向传播
根据前面的公式，编写函数计算从输入到输出的值。
```python
def forward_propagation(X, parameters):
    W1, b1, W2, b2 = parameters['W1'], parameters['b1'], parameters['W2'], parameters['b2']
    Z1 = np.dot(W1, X) + b1
    A1 = sigmoid(Z1)
    Z2 = np.dot(W2, A1) + b2
    A2 = sigmoid(Z2)
    cache = {"Z1": Z1, "A1": A1, "Z2": Z2, "A2": A2}
    return A2, cache
```

![](img/46ae3dd4f47308ef10799ea81647c68c_37.png)

![](img/46ae3dd4f47308ef10799ea81647c68c_39.png)

### 第三步：计算损失
实现交叉熵损失函数。
```python
def compute_cost(A2, Y):
    m = Y.shape[1]
    cost = -np.sum(Y * np.log(A2) + (1 - Y) * np.log(1 - A2)) / m
    return np.squeeze(cost)
```

### 第四步：实现反向传播
根据梯度公式，编写函数计算梯度。
```python
def backward_propagation(parameters, cache, X, Y):
    m = X.shape[1]
    W2 = parameters['W2']
    A1, A2 = cache['A1'], cache['A2']
    dZ2 = A2 - Y
    dW2 = np.dot(dZ2, A1.T) / m
    db2 = np.sum(dZ2, axis=1, keepdims=True) / m
    dZ1 = np.dot(W2.T, dZ2) * (A1 * (1 - A1))
    dW1 = np.dot(dZ1, X.T) / m
    db1 = np.sum(dZ1, axis=1, keepdims=True) / m
    grads = {"dW1": dW1, "db1": db1, "dW2": dW2, "db2": db2}
    return grads
```

### 第五步：更新参数
使用计算出的梯度，按照梯度下降法更新参数。
```python
def update_parameters(parameters, grads, learning_rate=0.01):
    parameters['W1'] -= learning_rate * grads['dW1']
    parameters['b1'] -= learning_rate * grads['db1']
    parameters['W2'] -= learning_rate * grads['dW2']
    parameters['b2'] -= learning_rate * grads['db2']
    return parameters
```

![](img/46ae3dd4f47308ef10799ea81647c68c_41.png)

![](img/46ae3dd4f47308ef10799ea81647c68c_43.png)

### 第六步：整合模型
将以上步骤整合到一个训练函数中，进行多次迭代（epoch）。
```python
def nn_model(X, Y, n_h, num_iterations=10000, learning_rate=0.01):
    n_x, n_y = X.shape[0], Y.shape[0]
    parameters = initialize_parameters(n_x, n_h, n_y)
    for i in range(num_iterations):
        A2, cache = forward_propagation(X, parameters)
        cost = compute_cost(A2, Y)
        grads = backward_propagation(parameters, cache, X, Y)
        parameters = update_parameters(parameters, grads, learning_rate)
        if i % 1000 == 0:
            print(f"迭代次数 {i}， 损失值 {cost}")
    return parameters
```

![](img/46ae3dd4f47308ef10799ea81647c68c_45.png)

### 第七步：进行预测
训练完成后，使用前向传播进行预测。
```python
def predict(parameters, X):
    A2, _ = forward_propagation(X, parameters)
    predictions = (A2 > 0.5).astype(int) # 将概率转换为0/1类别
    return predictions
```

![](img/46ae3dd4f47308ef10799ea81647c68c_47.png)

---

![](img/46ae3dd4f47308ef10799ea81647c68c_49.png)

![](img/46ae3dd4f47308ef10799ea81647c68c_51.png)

## 总结

![](img/46ae3dd4f47308ef10799ea81647c68c_53.png)

本节课中我们一起学习了神经网络的基础原理与实现。我们从单个神经元和激活函数（如Sigmoid）出发，理解了网络的前向传播过程。接着，我们引入了交叉熵损失函数，并详细探讨了通过反向传播算法计算梯度来更新权重的核心机制。最后，我们一步步地用Python和Numpy实现了参数初始化、前向传播、损失计算、反向传播、参数更新以及模型训练和预测的完整流程。通过本课，你已掌握了构建一个简单但功能完整的神经网络所需的所有基本组件。