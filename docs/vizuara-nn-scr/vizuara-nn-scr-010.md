#  010：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p10 P10 讲座 10 - 神经网络中的偏导数和梯度

🎼Yeah。大家好。欢迎来到神经网络从零开始的下一讲。

在本讲中，我们将主要涵盖三个内容。

### 1. 导数

![](img/c9dc500bf2aefa96a67155f85a054050_0.png)

导数是微积分中的一个基本概念，用于描述函数在某一点的瞬时变化率。在神经网络中，导数用于计算损失函数相对于网络参数的梯度，从而进行参数的优化。

**公式**：\( f'(x) = \lim_{\Delta x \to 0} \frac{f(x + \Delta x) - f(x)}{\Delta x} \)

### 2. 梯度

梯度是向量，表示函数在某一点的局部变化率。在神经网络中，梯度用于指导网络参数的更新，以最小化损失函数。

**公式**：\( \nabla f(x) = \left( \frac{\partial f}{\partial x_1}, \frac{\partial f}{\partial x_2}, ..., \frac{\partial f}{\partial x_n} \right) \)

### 3. 偏导数

![](img/c9dc500bf2aefa96a67155f85a054050_2.png)

偏导数是导数的一种特殊情况，用于描述函数在某一个变量上的变化率，而其他变量保持不变。在神经网络中，偏导数用于计算损失函数相对于网络参数的梯度。

**公式**：\( \frac{\partial f}{\partial x_i} = \lim_{\Delta x_i \to 0} \frac{f(x_1, x_2, ..., x_i + \Delta x_i, ..., x_n) - f(x_1, x_2, ..., x_i, ..., x_n)}{\Delta x_i} \)

通过以上三个概念，我们可以更好地理解神经网络中的参数优化过程。在本节课中，我们一起学习了神经网络中的偏导数和梯度。