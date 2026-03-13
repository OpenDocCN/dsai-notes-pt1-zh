# 081：一维NumPy入门 🧮

![](img/e690c93beb7dc0474e89ee5f0af68774_0.png)

在本节课中，我们将学习NumPy库的基础知识，特别是关于一维数组（1D Numpy Arrays）的内容。NumPy是科学计算的核心库，也是Pandas等高级工具的基础。我们将从数组创建开始，逐步学习索引、切片以及各种数组操作。

![](img/e690c93beb7dc0474e89ee5f0af68774_2.png)

---

![](img/e690c93beb7dc0474e89ee5f0af68774_3.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_4.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_5.png)

## 概述 📋

NumPy是一个用于科学计算的Python库。它提供了高性能的多维数组对象，以及用于处理这些数组的工具。与原生Python列表相比，NumPy数组在速度和内存效率上具有显著优势，是数据科学和人工智能领域的基石。

![](img/e690c93beb7dc0474e89ee5f0af68774_7.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_8.png)

---

![](img/e690c93beb7dc0474e89ee5f0af68774_10.png)

## 创建NumPy数组

![](img/e690c93beb7dc0474e89ee5f0af68774_12.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_14.png)

上一节我们介绍了NumPy的重要性，本节中我们来看看如何创建一个NumPy数组。

Python列表是一种可以存储和访问数据的容器。每个元素都与一个索引相关联。

![](img/e690c93beb7dc0474e89ee5f0af68774_16.png)

```python
# Python列表示例
a = [0, 1, 2, 3, 4]
```

![](img/e690c93beb7dc0474e89ee5f0af68774_17.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_18.png)

我们可以使用方括号来访问每个元素。

![](img/e690c93beb7dc0474e89ee5f0af68774_20.png)

NumPy数组（或ND数组）与列表类似，但通常大小固定，且每个元素都是相同的数据类型（例如，都是整数）。

![](img/e690c93beb7dc0474e89ee5f0af68774_21.png)

我们可以通过首先导入NumPy库，然后将列表转换为NumPy数组。

![](img/e690c93beb7dc0474e89ee5f0af68774_23.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_24.png)

```python
import numpy as np
# 将列表转换为NumPy数组
a = np.array([0, 1, 2, 3, 4])
```

![](img/e690c93beb7dc0474e89ee5f0af68774_26.png)

我们可以通过索引访问数据，就像列表一样。

![](img/e690c93beb7dc0474e89ee5f0af68774_27.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_28.png)

数组 `a` 在内存中的存储形式如下。如果我们检查数组的类型，会得到 `numpy.ndarray`。

![](img/e690c93beb7dc0474e89ee5f0af68774_30.png)

```python
type(a) # 输出：numpy.ndarray
```

![](img/e690c93beb7dc0474e89ee5f0af68774_32.png)

由于NumPy数组包含相同类型的数据，我们可以使用属性 `dtype` 来获取数组元素的数据类型。

```python
a.dtype # 输出：dtype('int64')
```

![](img/e690c93beb7dc0474e89ee5f0af68774_34.png)

在这个例子中，数据类型是64位整数。

---

![](img/e690c93beb7dc0474e89ee5f0af68774_36.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_37.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_38.png)

## 数组属性

让我们使用数组 `a` 来回顾一些基本的数组属性。

![](img/e690c93beb7dc0474e89ee5f0af68774_40.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_41.png)

属性 `size` 表示数组中元素的数量。因为有五个元素，所以结果是5。

![](img/e690c93beb7dc0474e89ee5f0af68774_43.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_45.png)

```python
a.size # 输出：5
```

![](img/e690c93beb7dc0474e89ee5f0af68774_47.png)

当我们学习更高维度时，下面两个属性的意义会更明显，但让我们先了解一下。
属性 `ndim` 表示数组的维数或秩（rank），在这个一维数组的例子中，结果是1。

```python
a.ndim # 输出：1
```

![](img/e690c93beb7dc0474e89ee5f0af68774_49.png)

属性 `shape` 是一个整数元组，表示数组在每个维度上的大小。

![](img/e690c93beb7dc0474e89ee5f0af68774_50.png)

```python
a.shape # 输出：(5,)
```

![](img/e690c93beb7dc0474e89ee5f0af68774_52.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_53.png)

我们可以创建包含实数的NumPy数组。当我们检查数组类型时，同样得到 `numpy.ndarray`。

```python
b = np.array([3.1, 11.02, 6.2, 213.2, 5.2])
```

![](img/e690c93beb7dc0474e89ee5f0af68774_55.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_56.png)

如果我们检查属性 `dtype`，会看到 `float64`，因为元素不是整数。

![](img/e690c93beb7dc0474e89ee5f0af68774_58.png)

NumPy还有许多其他属性，可以访问 `numpy.org` 查看更多信息。

![](img/e690c93beb7dc0474e89ee5f0af68774_60.png)

---

![](img/e690c93beb7dc0474e89ee5f0af68774_62.png)

## 索引与切片

![](img/e690c93beb7dc0474e89ee5f0af68774_64.png)

现在，我们来看看一些索引和切片的方法。

我们可以如下更改数组的第一个元素为100。

![](img/e690c93beb7dc0474e89ee5f0af68774_66.png)

```python
a[0] = 100
```

![](img/e690c93beb7dc0474e89ee5f0af68774_67.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_68.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_69.png)

数组的第一个值现在是100。我们也可以更改数组的第五个元素。

![](img/e690c93beb7dc0474e89ee5f0af68774_71.png)

```python
a[4] = 0
```

与列表和元组类似，我们可以对NumPy数组进行切片。数组的元素对应以下索引。

![](img/e690c93beb7dc0474e89ee5f0af68774_73.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_74.png)

我们可以选择索引1到3的元素，并将其赋值给一个新的NumPy数组 `c`。

```python
c = a[1:4]
```

![](img/e690c93beb7dc0474e89ee5f0af68774_76.png)

这些元素确实对应于索引1、2、3。和列表一样，切片不包含结束索引对应的元素。

![](img/e690c93beb7dc0474e89ee5f0af68774_77.png)

我们可以为相应的索引分配新值，如下所示。数组 `c` 现在有了新的值。

![](img/e690c93beb7dc0474e89ee5f0af68774_79.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_80.png)

```python
c[0:2] = 300, 400
```

更多关于NumPy索引和切片的例子，可以参考实验或 `numpy.org`。

![](img/e690c93beb7dc0474e89ee5f0af68774_82.png)

---

## 数组的基本运算

![](img/e690c93beb7dc0474e89ee5f0af68774_84.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_85.png)

NumPy使得数据科学中常见的许多操作变得更加容易。与常规Python相比，这些相同的操作在NumPy中通常计算速度更快，且需要的内存更少。

![](img/e690c93beb7dc0474e89ee5f0af68774_87.png)

让我们回顾一下一维数组上的一些操作。为了使内容更有趣，我们将在欧几里得向量（Euclidean Vectors）的背景下探讨许多运算。

![](img/e690c93beb7dc0474e89ee5f0af68774_88.png)

### 向量加法

![](img/e690c93beb7dc0474e89ee5f0af68774_90.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_91.png)

向量加法是数据科学中广泛使用的操作。考虑具有两个元素的向量 **u**。同样，考虑具有两个分量的向量 **v**。在向量加法中，我们创建一个新向量 **z**。

向量 **z** 的第一个分量是向量 **u** 和 **v** 的第一个分量之和。类似地，第二个分量是 **u** 和 **v** 的第二个分量之和。这个新向量 **z** 现在是向量 **u** 和 **v** 的线性组合。

![](img/e690c93beb7dc0474e89ee5f0af68774_93.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_94.png)

使用NumPy，我们可以用一行代码执行向量加法。

```python
u = np.array([1, 0])
v = np.array([0, 1])
z = u + v # 输出：array([1, 1])
```

![](img/e690c93beb7dc0474e89ee5f0af68774_96.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_97.png)

如果使用Python列表，则需要多行代码，并且NumPy代码运行速度会快得多，这在处理大量数据时非常重要。

### 向量减法

![](img/e690c93beb7dc0474e89ee5f0af68774_99.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_100.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_101.png)

我们也可以通过将加号改为减号来执行向量减法。

```python
z = u - v # 输出：array([ 1, -1])
```

![](img/e690c93beb7dc0474e89ee5f0af68774_103.png)

在Python列表上执行向量减法需要多行代码。

![](img/e690c93beb7dc0474e89ee5f0af68774_104.png)

### 标量乘法

![](img/e690c93beb7dc0474e89ee5f0af68774_106.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_107.png)

向量与标量的乘法是另一个常用操作。考虑向量 **y**。我们只需将向量乘以一个标量值，例如2。向量的每个分量都乘以2。

使用NumPy，标量乘法只需要一行代码。

![](img/e690c93beb7dc0474e89ee5f0af68774_109.png)

```python
y = np.array([1, 2])
z = 2 * y # 输出：array([2, 4])
```

![](img/e690c93beb7dc0474e89ee5f0af68774_110.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_111.png)

如果使用Python列表执行相同任务，则需要多行代码，而且操作速度也会慢得多。

### 哈达玛积（逐元素乘积）

![](img/e690c93beb7dc0474e89ee5f0af68774_113.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_114.png)

哈达玛积（Hadamard Product）是数据科学中另一个广泛使用的操作。考虑两个向量 **u** 和 **v**，它们的哈达玛积是一个新向量 **z**。**z** 的第一个分量是 **u** 和 **v** 的第一个元素的乘积。类似地，第二个分量是第二个元素的乘积。

![](img/e690c93beb7dc0474e89ee5f0af68774_115.png)

我们也可以用NumPy的一行代码执行哈达玛积。

![](img/e690c93beb7dc0474e89ee5f0af68774_117.png)

```python
u = np.array([1, 2])
v = np.array([3, 2])
z = u * v # 输出：array([3, 4])
```

在两个列表上执行哈达玛积需要多行代码。

![](img/e690c93beb7dc0474e89ee5f0af68774_119.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_120.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_121.png)

### 点积

![](img/e690c93beb7dc0474e89ee5f0af68774_123.png)

点积是数据科学中另一个广泛使用的操作。考虑向量 **u** 和 **v**。点积是一个由以下项给出的单个数字，表示两个向量的相似程度。

我们也可以使用NumPy的函数 `dot` 来执行点积。

![](img/e690c93beb7dc0474e89ee5f0af68774_125.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_126.png)

```python
result = np.dot(u, v) # 输出：7
```

![](img/e690c93beb7dc0474e89ee5f0af68774_128.png)

---

## 通用函数与广播

![](img/e690c93beb7dc0474e89ee5f0af68774_130.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_131.png)

考虑数组 **u**。如果我们在数组上加一个标量值，NumPy会将该值加到每个元素上。这个特性被称为广播（Broadcasting）。

![](img/e690c93beb7dc0474e89ee5f0af68774_133.png)

```python
u = np.array([1, 2, 3, -1])
z = u + 1 # 输出：array([2, 3, 4, 0])
```

![](img/e690c93beb7dc0474e89ee5f0af68774_135.png)

通用函数（Universal Function）是一种对ND数组进行操作的函数。

![](img/e690c93beb7dc0474e89ee5f0af68774_136.png)

我们可以将通用函数应用于NumPy数组。考虑数组 **a**，我们可以使用方法 `mean` 计算所有元素的平均值。

![](img/e690c93beb7dc0474e89ee5f0af68774_138.png)

```python
a = np.array([1, -1, 1, -1])
mean_a = a.mean() # 输出：0.0
```

![](img/e690c93beb7dc0474e89ee5f0af68774_139.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_141.png)

这对应于所有元素的平均值。在这个例子中，结果是0。

![](img/e690c93beb7dc0474e89ee5f0af68774_142.png)

还有许多其他函数。例如，考虑NumPy数组 **b**，我们可以使用方法 `max` 找到最大值。

![](img/e690c93beb7dc0474e89ee5f0af68774_144.png)

```python
b = np.array([1, 2, 3, 4, 5])
max_b = b.max() # 输出：5
```

![](img/e690c93beb7dc0474e89ee5f0af68774_145.png)

我们看到最大值是5，因此 `max` 方法返回5。

![](img/e690c93beb7dc0474e89ee5f0af68774_147.png)

---

![](img/e690c93beb7dc0474e89ee5f0af68774_149.png)

## 使用NumPy进行数学计算与绘图

![](img/e690c93beb7dc0474e89ee5f0af68774_150.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_151.png)

我们可以使用NumPy创建将NumPy数组映射到新NumPy数组的函数。

我们可以在NumPy中按如下方式访问π的值。我们可以创建以下以弧度为单位的NumPy数组。

![](img/e690c93beb7dc0474e89ee5f0af68774_153.png)

```python
x = np.array([0, np.pi/2, np.pi])
```

![](img/e690c93beb7dc0474e89ee5f0af68774_154.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_155.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_156.png)

这个数组对应一个向量。我们可以将函数 `sin` 应用于数组 `x`，并将值赋给数组 `y`。这会将正弦函数应用于数组中的每个元素。

![](img/e690c93beb7dc0474e89ee5f0af68774_158.png)

结果是一个新的数组 `y`，其中每个值对应于对数组 `x` 中每个元素应用正弦函数的结果。

![](img/e690c93beb7dc0474e89ee5f0af68774_160.png)

用于绘制数学函数的一个有用函数是 `linspace`。`linspace` 在指定区间内返回均匀间隔的数字。

![](img/e690c93beb7dc0474e89ee5f0af68774_162.png)

我们指定序列的起点、终点。参数 `num` 表示要生成的样本数量。

```python
# 生成5个从-2到2的均匀间隔数字
np.linspace(-2, 2, num=5)
# 生成9个从-2到2的均匀间隔数字
np.linspace(-2, 2, num=9)
```

![](img/e690c93beb7dc0474e89ee5f0af68774_164.png)

我们可以使用 `linspace` 函数从区间0到2π生成100个均匀间隔的样本。

```python
x = np.linspace(0, 2*np.pi, num=100)
```

![](img/e690c93beb7dc0474e89ee5f0af68774_166.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_167.png)

我们可以使用NumPy函数 `sin` 将数组 `x` 映射到一个新数组 `y`。

![](img/e690c93beb7dc0474e89ee5f0af68774_168.png)

```python
y = np.sin(x)
```

![](img/e690c93beb7dc0474e89ee5f0af68774_170.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_171.png)

我们可以导入库 `matplotlib.pyplot` 来帮助我们绘制函数。

![](img/e690c93beb7dc0474e89ee5f0af68774_173.png)

```python
import matplotlib.pyplot as plt
# 在Jupyter Notebook中使用的命令
%matplotlib inline
plt.plot(x, y)
```

![](img/e690c93beb7dc0474e89ee5f0af68774_174.png)

第一个输入对应于水平轴（X轴）的值，第二个输入对应于垂直轴（Y轴）的值。

![](img/e690c93beb7dc0474e89ee5f0af68774_176.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_178.png)

关于NumPy，你还可以做更多事情。可以查看 `numpy.org` 上的实验或文档以获取更多信息。

![](img/e690c93beb7dc0474e89ee5f0af68774_180.png)

---

![](img/e690c93beb7dc0474e89ee5f0af68774_181.png)

## 总结 🎯

![](img/e690c93beb7dc0474e89ee5f0af68774_183.png)

在本节课中，我们一起学习了NumPy一维数组的基础知识。我们从如何创建和转换数组开始，了解了数组的基本属性（如 `size`、`shape`、`dtype`）。接着，我们探索了索引、切片以及如何修改数组元素。

![](img/e690c93beb7dc0474e89ee5f0af68774_185.png)

然后，我们深入学习了NumPy强大的数组运算能力，包括向量加法、减法、标量乘法、哈达玛积和点积，并理解了NumPy在效率和简洁性上的优势。我们还介绍了广播机制和通用函数，它们使得对数组的整体操作变得非常简单。

![](img/e690c93beb7dc0474e89ee5f0af68774_187.png)

最后，我们了解了如何使用 `linspace` 生成数值序列，并结合 `matplotlib` 进行简单的函数绘图，展示了NumPy在科学计算和可视化中的实际应用。

![](img/e690c93beb7dc0474e89ee5f0af68774_189.png)

![](img/e690c93beb7dc0474e89ee5f0af68774_190.png)

掌握一维NumPy数组是理解更高维数据和进行复杂数据操作的第一步，它为后续学习更高级的数据分析工具（如Pandas）奠定了坚实的基础。