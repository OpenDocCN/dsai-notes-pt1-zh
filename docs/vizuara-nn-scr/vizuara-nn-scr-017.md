#  017：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p17 P17 Lecture 17 - Backpropagation on the ReLU activation function [BV1iEHPzGEpa_p17]

## 🎼构建神经网络：ReLU激活函数的反向传播

### 概述
在本节课中，我们将学习ReLU激活函数的反向传播过程。

### 上一节回顾
在上一节课中，我们学习了如何使用Python编写反向传播的基本构建块。

![](img/2f586caeff31110451b305d348bd0739_0.png)

### 本节内容
以下是本节课的主要内容：

#### 1. ReLU激活函数
ReLU（Rectified Linear Unit）激活函数是一种常用的非线性激活函数，其公式如下：

**公式：**
\[ f(x) = \max(0, x) \]

#### 2. ReLU激活函数的反向传播
ReLU激活函数的反向传播过程相对简单。当输入值 \( x \) 大于0时，梯度为1；当输入值 \( x \) 小于等于0时，梯度为0。其公式如下：

**公式：**
\[ \frac{\partial L}{\partial z} = \begin{cases} 
1 & \text{if } z > 0 \\
0 & \text{if } z \leq 0 
\end{cases} \]

其中，\( L \) 表示损失函数，\( z \) 表示ReLU激活函数的输出。

#### 3. 实例分析
以下是一个ReLU激活函数的反向传播实例：

**代码：**
```python
def relu_derivative(z):
    if z > 0:
        return 1
    else:
        return 0

![](img/2f586caeff31110451b305d348bd0739_2.png)

# 假设输入值z为-2
z = -2
# 计算梯度
gradient = relu_derivative(z)
print("Gradient:", gradient)
```

输出结果为：
```
Gradient: 0
```

### 总结
本节课中，我们一起学习了ReLU激活函数及其反向传播过程。希望这节课的内容能够帮助您更好地理解神经网络的基本原理。