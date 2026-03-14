#  005：Vizuara【中英⚡从零开始构建神经网络｜Building Neural Networks from Scratch】 p05 P5 Lecture 5 - Python中的广播和数组求和

🎼Yeah。大家好，欢迎来到本节课，我们将从零开始构建神经网络。

在进一步探讨激活函数，如Sigmoid、ReLU等之前，

以下是本节课的主要内容：

## 1. 引言

在本节课中，我们将学习如何在Python中进行广播和数组求和，这些是神经网络中非常重要的概念。

## 2. 广播（Broadcasting）

广播是NumPy中的一种操作，它允许我们在不同形状的数组之间进行元素级操作。

![](img/f9974a13ad846f0441798c2bed32f9ef_0.png)

**公式**：假设有两个数组A和B，它们的形状分别为(A, B)和(C, D)，那么在广播操作中，A的形状将扩展为(A, B, C, D)，B的形状将扩展为(C, D)。

```python
import numpy as np

A = np.array([1, 2, 3])
B = np.array([4, 5])

result = A[:, np.newaxis] * B[np.newaxis, :]
print(result)
```

## 3. 数组求和

数组求和是神经网络中用于计算激活函数输出的一种常见操作。

**公式**：假设有一个数组A，其形状为(N, M)，那么A的求和结果为N * M。

```python
import numpy as np

A = np.array([[1, 2], [3, 4]])
result = np.sum(A)
print(result)
```

## 4. 总结

本节课中，我们学习了Python中的广播和数组求和，这些概念对于构建神经网络至关重要。

在本节课中，我们一起学习了如何在Python中进行广播和数组求和。希望这些知识能够帮助你在构建神经网络的过程中更加得心应手。