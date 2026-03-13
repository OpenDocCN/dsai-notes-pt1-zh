# 017：二维NumPy数组

![](img/d3a4b6b58b4625069ddef19f9a50ff85_0.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_2.png)

在本节课中，我们将学习如何创建和操作二维NumPy数组。二维数组是数据科学和机器学习中处理表格数据的基础。我们将涵盖数组的创建、索引、切片以及基本运算。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_4.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_6.png)

---

![](img/d3a4b6b58b4625069ddef19f9a50ff85_8.png)

## 🏗️ 二维数组的创建与结构

我们可以创建多维的NumPy数组。本节将重点介绍二维数组，但NumPy也支持构建更高维度的数组。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_10.png)

考虑列表 `A`，它包含三个嵌套列表，每个列表大小相同。每个列表用不同颜色标记以便理解。我们可以将该列表转换为NumPy数组，如下所示：

![](img/d3a4b6b58b4625069ddef19f9a50ff85_12.png)

```python
import numpy as np
A = [[11, 12, 13], [21, 22, 23], [31, 32, 33]]
A = np.array(A)
```

![](img/d3a4b6b58b4625069ddef19f9a50ff85_14.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_15.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_16.png)

将NumPy数组可视化为矩形数组很有帮助。每个嵌套列表对应矩阵的一行。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_17.png)

---

![](img/d3a4b6b58b4625069ddef19f9a50ff85_19.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_21.png)

## 📐 数组的维度与形状

![](img/d3a4b6b58b4625069ddef19f9a50ff85_23.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_25.png)

我们可以使用属性 `ndim` 来获取数组的轴数或维度数，这被称为秩。在NumPy中，秩指的是嵌套列表的层数，而非矩阵的线性独立列数。

第一个列表代表第一个维度（轴0），它包含的另一组列表代表第二个维度（轴1）。列表包含的列表数量与维度无关，而与列表的形状有关。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_27.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_28.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_29.png)

与一维数组类似，属性 `shape` 返回一个元组。使用矩形表示法有助于理解：元组的第一个元素对应原始列表中嵌套列表的数量（即行数），第二个元素对应每个嵌套列表的大小（即列数）。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_31.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_32.png)

例如，对于数组 `A`：
- `A.ndim` 返回 `2`
- `A.shape` 返回 `(3, 3)`

![](img/d3a4b6b58b4625069ddef19f9a50ff85_34.png)

我们还可以使用属性 `size` 获取数组的总元素数，即行数与列数的乘积。对于 `A`，`A.size` 返回 `9`。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_36.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_37.png)

建议在实验中尝试不同形状的数组并查看其他属性。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_39.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_40.png)

---

![](img/d3a4b6b58b4625069ddef19f9a50ff85_42.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_43.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_45.png)

## 🔍 二维数组的索引与切片

![](img/d3a4b6b58b4625069ddef19f9a50ff85_46.png)

我们可以使用方括号访问数组的不同元素。以下图像展示了列表式表示法与索引约定之间的关系。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_48.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_49.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_50.png)

第一个括号内的索引对应不同的嵌套列表（即不同的行），第二个括号内的索引对应嵌套列表内的特定元素（即列）。

使用矩形表示法时，第一个索引对应行索引，第二个索引对应列索引。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_52.png)

我们也可以使用单个括号来访问元素，但通常使用两个索引更清晰。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_53.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_54.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_55.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_56.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_57.png)

**索引示例：**
- `A[1, 2]` 访问第二行、第三列的元素，值为 `23`。
- `A[0, 0]` 访问第一行、第一列的元素，值为 `11`。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_58.png)

**切片示例：**
- `A[0, 0:2]` 访问第一行的前两列。
- `A[0:2, 2]` 访问前两行的最后一列。

---

## ➕ 二维数组的基本运算

![](img/d3a4b6b58b4625069ddef19f9a50ff85_60.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_61.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_62.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_63.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_64.png)

### 数组加法
数组加法的过程与矩阵加法相同。考虑矩阵 `X` 和 `Y`，我们将相同位置的元素相加。结果是一个与 `X` 和 `Y` 大小相同的新矩阵，其中每个元素是 `X` 和 `Y` 对应元素的和。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_65.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_66.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_67.png)

在NumPy中，我们可以这样实现：
```python
X = np.array([[1, 0], [0, 1]])
Y = np.array([[2, 1], [1, 2]])
Z = X + Y  # 结果：[[3, 1], [1, 3]]
```

![](img/d3a4b6b58b4625069ddef19f9a50ff85_69.png)

### 标量乘法
将NumPy数组乘以标量与将矩阵乘以标量相同。考虑矩阵 `Y`，将其乘以标量 `2`，即矩阵中的每个元素都乘以 `2`。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_70.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_71.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_72.png)

在NumPy中：
```python
Y = np.array([[2, 1], [1, 2]])
Z = 2 * Y  # 结果：[[4, 2], [2, 4]]
```

![](img/d3a4b6b58b4625069ddef19f9a50ff85_74.png)

### 逐元素乘法（哈达玛积）
两个数组的乘法对应于逐元素乘积或哈达玛积。考虑数组 `X` 和 `Y`，哈达玛积是将相同位置的元素相乘。

在NumPy中：
```python
X = np.array([[1, 0], [0, 1]])
Y = np.array([[2, 1], [1, 2]])
Z = X * Y  # 结果：[[2, 0], [0, 2]]
```

![](img/d3a4b6b58b4625069ddef19f9a50ff85_76.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_77.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_78.png)

### 矩阵乘法
我们也可以使用NumPy数组进行矩阵乘法。矩阵乘法稍微复杂一些，但以下是基本概述。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_79.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_80.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_81.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_82.png)

考虑矩阵 `A` 和 `B`。在线性代数中，在将矩阵 `A` 乘以矩阵 `B` 之前，我们必须确保 `A` 的列数等于 `B` 的行数。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_84.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_85.png)

为了得到新矩阵的第 `i` 行第 `j` 列元素，我们取 `A` 的第 `i` 行与 `B` 的第 `j` 列的点积。

在NumPy中，我们可以使用 `np.dot()` 函数或 `@` 运算符进行矩阵乘法：
```python
A = np.array([[0, 1, 1], [1, 0, 1]])
B = np.array([[1, 1], [1, 1], [-1, 1]])
C = np.dot(A, B)  # 或 C = A @ B
# 结果：[[0, 2], [0, 2]]
```

![](img/d3a4b6b58b4625069ddef19f9a50ff85_87.png)

---

![](img/d3a4b6b58b4625069ddef19f9a50ff85_88.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_89.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_90.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_91.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_92.png)

## 📝 总结

![](img/d3a4b6b58b4625069ddef19f9a50ff85_93.png)

本节课我们一起学习了二维NumPy数组的核心概念。我们了解了如何创建二维数组，如何获取其维度和形状信息。我们掌握了使用索引和切片来访问数组中的特定元素或子集。最后，我们学习了二维数组的基本运算，包括数组加法、标量乘法、逐元素乘法（哈达玛积）以及矩阵乘法。这些操作是进行数据分析和科学计算的基础。

![](img/d3a4b6b58b4625069ddef19f9a50ff85_95.png)

![](img/d3a4b6b58b4625069ddef19f9a50ff85_97.png)

NumPy的功能远不止于此，建议访问 [numpy.org](https://numpy.org) 探索更多内容。