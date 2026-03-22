# 044：NumPy示例续篇 🔢

![](img/8ef99fae43e2641878e3c95a92e3158a_0.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_2.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_4.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_6.png)

在本节课中，我们将继续学习NumPy数组的创建方法，包括如何基于现有数组的形状创建全1或全0数组、如何复制数组以及如何创建单位矩阵（或称恒等矩阵）。这些是进行高效数值计算的基础。

![](img/8ef99fae43e2641878e3c95a92e3158a_7.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_9.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_11.png)

上一节我们介绍了使用`np.array()`等函数创建数组的基本方法。本节中我们来看看几种更高级的数组创建技巧。

![](img/8ef99fae43e2641878e3c95a92e3158a_13.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_15.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_16.png)

## 基于现有数组形状创建新数组

![](img/8ef99fae43e2641878e3c95a92e3158a_18.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_20.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_22.png)

有时我们需要创建一个与现有数组形状相同，但元素内容不同的新数组。NumPy为此提供了便捷的函数。

假设已经存在一个数组 `x`：
```python
import numpy as np
x = np.array([2, 5, 18])
```

![](img/8ef99fae43e2641878e3c95a92e3158a_24.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_25.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_26.png)

现在，我们可以创建一个与 `x` 形状相同但元素全为1的新数组。以下是具体方法：
```python
X = np.ones_like(x)
print(X)
```
运行上述代码，`X` 将是一个形状与 `x` 相同，但所有元素都为1的数组。`np.ones_like()` 函数的作用就是创建一个新数组，并用1填充，其形状与传入的数组参数相同。

![](img/8ef99fae43e2641878e3c95a92e3158a_28.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_29.png)

类似地，我们也可以创建元素全为0的数组：
```python
Y = np.zeros_like(x)
print(Y)
```
`np.zeros_like()` 函数会创建一个形状与 `x` 相同，但所有元素都为0的新数组 `Y`。

![](img/8ef99fae43e2641878e3c95a92e3158a_31.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_32.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_33.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_35.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_36.png)

## 复制数组

![](img/8ef99fae43e2641878e3c95a92e3158a_38.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_40.png)

在数据处理中，我们经常需要复制一个数组，以避免修改原始数据。NumPy数组提供了 `copy()` 方法来实现深度复制。

![](img/8ef99fae43e2641878e3c95a92e3158a_42.png)

以下是复制数组的步骤：
1.  首先导入NumPy并创建一个数组。
2.  使用数组的 `.copy()` 方法创建其副本。

![](img/8ef99fae43e2641878e3c95a92e3158a_44.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_45.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_47.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_48.png)

具体代码如下：
```python
import numpy as np
x = np.array([2, 5, 18])
y = x.copy()
print(y)
```
执行后，`y` 将是 `x` 的一个完整副本。对 `y` 的修改不会影响原始的 `x` 数组。

![](img/8ef99fae43e2641878e3c95a92e3158a_50.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_51.png)

## 创建单位矩阵

![](img/8ef99fae43e2641878e3c95a92e3158a_53.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_55.png)

单位矩阵是一个方阵，其主对角线上的元素均为1，其余元素均为0。在线性代数运算中非常常用。

![](img/8ef99fae43e2641878e3c95a92e3158a_57.png)

NumPy提供了两种主要方法来创建单位矩阵。

![](img/8ef99fae43e2641878e3c95a92e3158a_59.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_60.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_61.png)

第一种是使用 `np.identity()` 函数。该函数创建一个指定大小的正方形单位矩阵。
```python
# 创建一个4x4的单位矩阵
identity_matrix = np.identity(4)
print(identity_matrix)
```
`np.identity(4)` 会生成一个4行4列的单位矩阵。

![](img/8ef99fae43e2641878e3c95a92e3158a_62.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_63.png)

第二种方法是使用更通用的 `np.eye()` 函数。这个函数功能更强大，可以创建非方阵的对角矩阵，并可以指定对角线的位置。
```python
# 创建一个5行9列的数组，主对角线为1
eye_matrix = np.eye(5, 9)
print(eye_matrix)
```
`np.eye(5, 9)` 创建一个5行9列的数组，从左上角开始的主对角线元素为1。

![](img/8ef99fae43e2641878e3c95a92e3158a_65.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_66.png)

`np.eye()` 函数还有一个重要的参数 `k`，用于控制对角线的位置：
*   `k=0`（默认）：主对角线。
*   `k=1`：主对角线向上偏移一行的对角线。
*   `k=-1`：主对角线向下偏移一行的对角线。

![](img/8ef99fae43e2641878e3c95a92e3158a_68.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_69.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_70.png)

例如：
```python
print(np.eye(5, 9, k=1))   # 对角线向上偏移一行
print(np.eye(5, 9, k=-2))  # 对角线向下偏移两行
```
通过调整 `k` 值，我们可以轻松创建具有不同对角线位置的矩阵。

![](img/8ef99fae43e2641878e3c95a92e3158a_72.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_73.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_74.png)

![](img/8ef99fae43e2641878e3c95a92e3158a_75.png)

本节课中我们一起学习了NumPy中几种实用的数组创建方法：使用 `ones_like` 和 `zeros_like` 基于形状快速创建数组，使用 `copy()` 方法安全地复制数组，以及使用 `identity()` 和功能更灵活的 `eye()` 函数来创建单位矩阵或对角矩阵。掌握这些方法能为后续进行数组运算和线性代数计算打下坚实基础。在接下来的课程中，我们将深入探讨NumPy的算术运算。