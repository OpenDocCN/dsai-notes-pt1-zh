# 046：矩阵乘法 🧮

在本节课中，我们将要学习NumPy中矩阵乘法的概念、计算方法及其与普通元素乘法的区别。我们将从二维矩阵乘法开始，逐步扩展到三维数组，并了解如何将NumPy数组转换为矩阵对象以使用不同的乘法规则。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_1.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_2.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_3.png)

## 矩阵乘法与元素乘法的区别

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_4.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_5.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_6.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_7.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_8.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_9.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_10.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_11.png)

上一节我们介绍了NumPy数组的基本算术运算，如加法、减法、乘法和除法，这些运算都是**逐元素**进行的。本节中我们来看看一种特殊的运算——**矩阵乘法**。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_12.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_14.png)

矩阵乘法与普通的逐元素乘法完全不同。在逐元素乘法中，两个形状相同的数组对应位置的元素直接相乘。例如，对于数组 `a` 和 `b`：
```python
a * b  # 结果为 [a[0]*b[0], a[1]*b[1], ...]
```

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_16.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_17.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_19.png)

而矩阵乘法的规则是：结果矩阵中第 `i` 行第 `j` 列的元素，等于第一个矩阵的第 `i` 行与第二个矩阵的第 `j` 列对应元素的乘积之和。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_21.png)

其核心公式为：
**`C[i, j] = Σ (A[i, k] * B[k, j])`**
其中，`Σ` 表示对 `k` 求和。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_23.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_24.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_25.png)

## 二维矩阵乘法计算

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_27.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_28.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_29.png)

让我们通过一个具体例子来理解。假设我们有两个数组：
- 数组 `a` 为 `[[11, 12, 13]]`（1行3列）
- 数组 `b` 为 `[[1], [1], [1]]`（3行1列）

矩阵乘法的计算过程如下：
1.  取 `a` 的第一行 `[11, 12, 13]`。
2.  取 `b` 的第一列 `[1, 1, 1]`。
3.  计算点积：`11*1 + 12*1 + 13*1 = 36`。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_31.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_32.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_33.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_34.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_35.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_36.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_37.png)

因此，结果矩阵的第一个（也是唯一一个）元素是 `36`。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_39.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_40.png)

在NumPy中，我们使用 `np.dot()` 函数或 `@` 运算符来进行矩阵乘法。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_42.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_44.png)

以下是使用 `np.dot()` 的代码示例：
```python
import numpy as np
a = np.array([[11, 12, 13]])
b = np.array([[1], [1], [1]])
result = np.dot(a, b)  # 结果为 [[36]]
```

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_46.png)

## 矩阵乘法的维度规则

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_47.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_48.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_50.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_51.png)

进行矩阵乘法时，必须遵守特定的维度规则：**第一个矩阵的列数必须等于第二个矩阵的行数**。

如果数组 `a` 的形状是 `(m, n)`，数组 `b` 的形状是 `(p, q)`，那么只有当 `n == p` 时，`np.dot(a, b)` 才是有效的。结果矩阵的形状将是 `(m, q)`。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_53.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_54.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_55.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_56.png)

## 三维数组的矩阵乘法

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_58.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_60.png)

`np.dot()` 函数的功能非常强大，它同样适用于更高维度的数组。对于三维数组，`np.dot()` 会按照特定的规则在最后一个轴和倒数第二个轴上进行求和计算。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_62.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_64.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_66.png)

以下是如何创建三维数组并进行矩阵乘法的示例：
```python
import numpy as np
# 创建一个三维数组 x
x = np.array([[[3, 1, 2],
               [5, 2, 3],
               [4, 1, 1]],
              [[3, 2, 4],
               [2, 1, 5],
               [3, 5, 4]]])
# 创建一个与 x 形状相同的全1数组 y
y = np.ones_like(x)
# 进行三维矩阵乘法
result_3d = np.dot(x, y)
```

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_68.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_70.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_72.png)

`np.dot(x, y)` 会自动处理三维情况下的复杂计算，这是NumPy库高效和强大的体现。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_73.png)

## NumPy数组与矩阵对象

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_75.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_76.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_77.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_78.png)

在NumPy中，除了数组（`ndarray`），还有一种专门的**矩阵（matrix）** 对象。矩阵对象的一个关键特性是：`*` 运算符被重载为执行矩阵乘法，而不是逐元素乘法。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_79.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_80.png)

以下是创建矩阵对象并使用 `*` 进行矩阵乘法的示例：
```python
import numpy as np
a = np.array([[11, 12, 13]])
b = np.array([[1], [1], [1]])
# 将数组转换为矩阵对象
ma = np.matrix(a)
mb = np.matrix(b)
# 在矩阵对象中，* 执行矩阵乘法
result_matrix = ma * mb  # 结果为 matrix([[36]])
```
需要注意的是，虽然矩阵对象在某些线性代数运算上更直观，但NumPy官方文档建议优先使用通用的 `ndarray` 数组，并使用 `np.dot()`、`np.matmul()` 或 `@` 运算符来进行矩阵乘法，因为数组的应用范围更广，功能也更全面。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_82.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_83.png)

## 核心要点总结

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_84.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_85.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_86.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_87.png)

本节课中我们一起学习了NumPy中的矩阵乘法：
1.  **概念区分**：矩阵乘法（`np.dot()`）与逐元素乘法（`*`）是两种完全不同的运算。
2.  **计算规则**：矩阵乘法遵循“行乘列”的规则，结果元素是点积之和。
3.  **维度要求**：进行矩阵乘法的两个数组必须满足特定的形状匹配条件。
4.  **高维扩展**：`np.dot()` 函数可以应用于二维及更高维度的数组。
5.  **对象差异**：NumPy的矩阵（`matrix`）对象将 `*` 定义为矩阵乘法，而数组（`ndarray`）对象则需使用特定函数或运算符。

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_89.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_90.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_91.png)

![](img/bbf7af71fbe74ab94e8c7aa5c5cd1b2d_92.png)

理解并掌握矩阵乘法是进行数据分析、机器学习以及任何涉及线性代数运算的基础。