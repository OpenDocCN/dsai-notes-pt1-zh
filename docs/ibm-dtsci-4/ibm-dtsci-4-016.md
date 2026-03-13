# 016：Python应用于数据科学、AI和开发 - P16 🧮 一维NumPy数组教程

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_0.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_2.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_3.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_4.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_5.png)

在本节课中，我们将要学习NumPy库的基础知识，特别是关于一维数组（1D数组）的内容。NumPy是科学计算的核心库，为数据科学、人工智能和开发提供了强大的数组操作功能。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_7.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_8.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_10.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_12.png)

## 概述 📋

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_14.png)

NumPy是一个用于科学计算的Python库。它提供了许多有用的函数。NumPy具有速度快、内存效率高等优点。NumPy也是Pandas库的基础。本视频将涵盖一维NumPy数组的基础知识、数组创建、索引与切片、基本操作以及通用函数。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_16.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_17.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_18.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_20.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_21.png)

## 创建NumPy数组 🛠️

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_23.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_24.png)

上一节我们介绍了NumPy的概况，本节中我们来看看如何创建一个NumPy数组。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_26.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_27.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_28.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_30.png)

一个Python列表是一个容器，允许你存储和访问数据。每个元素都与一个索引相关联。我们可以使用方括号访问每个元素，如下所示。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_32.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_34.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_36.png)

一个NumPy数组（或ND数组）与列表类似。它的大小通常是固定的，并且每个元素都是相同类型（在本例中是整数）。我们可以通过首先导入NumPy，然后将列表转换为NumPy数组。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_37.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_39.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_40.png)

以下是创建和访问NumPy数组的步骤：

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_41.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_43.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_44.png)

1.  **导入NumPy库**：首先需要导入NumPy。
    ```python
    import numpy as np
    ```
2.  **将列表转换为数组**：使用`np.array()`函数。
    ```python
    a = np.array([0, 1, 2, 3, 4])
    ```
3.  **访问数组元素**：与列表类似，使用整数索引和方括号。
    ```python
    a[0]  # 访问第一个元素
    ```

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_46.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_48.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_50.png)

数组`a`的存储方式如下。如果我们检查数组的类型，会得到`numpy.ndarray`。由于NumPy数组包含相同类型的数据，我们可以使用属性`dtype`来获取数组元素的数据类型，在本例中是64位整数。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_52.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_53.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_54.png)

## 数组属性 📊

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_56.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_57.png)

现在我们已经创建了数组，让我们来回顾一些基本的数组属性。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_59.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_60.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_62.png)

以下是使用数组`a`可以查看的一些关键属性：

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_64.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_66.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_68.png)

*   **`size`**：数组中元素的数量。因为有五个元素，所以结果是5。
*   **`ndim`**：表示数组的维数或秩。在一维情况下，值为1。
*   **`shape`**：一个整数元组，表示数组在每个维度上的大小。对于一维数组，形状是`(n,)`，其中`n`是元素个数。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_70.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_71.png)

我们也可以创建包含实数的NumPy数组。当我们检查数组类型时，得到的是`numpy.ndarray`。如果我们检查属性`dtype`，会看到`float64`，因为元素不是整数。NumPy还有许多其他属性，可以查阅NumPy官网了解更多。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_72.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_73.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_75.png)

## 索引与切片 🔪

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_77.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_78.png)

了解了数组的基本属性后，本节我们来看看如何访问和修改数组中的元素。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_80.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_81.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_82.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_83.png)

我们可以更改数组的第一个元素为100。数组的第一个值现在是100。我们也可以更改数组的第五个元素。现在第五个元素是0。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_85.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_86.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_88.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_89.png)

与列表和元组类似，我们可以对NumPy数组进行切片。数组的元素对应以下索引。我们可以选择索引1到3的元素，并将其赋值给一个新的NumPy数组`c`。这些元素确实对应于索引。与列表一样，我们不计算与最后一个索引对应的元素。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_91.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_93.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_94.png)

我们可以为相应的索引分配新值。数组`c`现在有了新的值。有关NumPy用法的更多示例，请参阅实验或NumPy官网。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_96.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_97.png)

## 基本数组运算 ➕➖✖️

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_99.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_100.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_102.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_103.png)

NumPy使得在数据科学中常见的许多操作变得更加容易。与常规Python相比，这些相同的操作在NumPy中通常计算速度更快，并且需要更少的内存。让我们回顾一下一维数组上的一些操作。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_105.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_107.png)

我们将在欧几里得向量的背景下查看许多操作，以使内容更有趣。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_109.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_110.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_111.png)

**向量加法**是数据科学中广泛使用的操作。考虑具有两个元素的向量`u`。类似地，考虑具有两个分量的向量`v`。在向量加法中，我们创建一个新向量`z`。`z`的第一个分量是向量`u`和`v`的第一个分量之和。同样，第二个分量是`u`和`v`的第二个分量之和。这个新向量`z`现在是向量`u`和`v`的线性组合。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_113.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_114.png)

使用NumPy，我们可以用一行代码执行向量加法。如果使用Python列表执行相同的任务，则需要多行代码，如屏幕右侧所示。此外，NumPy代码的运行速度也会快得多。如果你有大量数据，这一点很重要。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_116.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_117.png)

**向量减法**可以通过将加号改为减号来执行。如果对两个列表执行向量减法，则需要多行代码，如屏幕右侧所示。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_119.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_120.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_121.png)

**向量与标量的乘法**是另一个常见的操作。考虑向量`y`。我们只需将向量乘以一个标量值，在本例中是2。向量的每个分量都乘以2。在这种情况下，每个分量都加倍。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_123.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_124.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_125.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_126.png)

使用NumPy，向量与标量的乘法只需要一行代码。如果使用Python列表执行相同的任务，则需要多行代码，如屏幕右侧所示。此外，该操作也会慢得多。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_128.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_130.png)

**哈达玛积（逐元素乘积）** 是数据科学中另一个广泛使用的操作。考虑以下两个向量`u`和`v`，`u`和`v`的哈达玛积是一个新向量`z`。`z`的第一个分量是`u`和`v`的第一个元素的乘积。类似地，第二个分量是`u`和`v`的第二个元素的乘积。结果向量由`u`和`v`的逐元素乘积组成。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_131.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_132.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_134.png)

我们也可以用一行NumPy代码执行哈达玛积。如果对两个列表执行哈达玛积，则需要多行代码，如屏幕右侧所示。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_136.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_137.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_139.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_140.png)

**点积**是数据科学中另一个广泛使用的操作。考虑向量`u`和`v`。点积是由以下项给出的单个数字，表示两个向量的相似程度。我们将`v`和`u`的第一个分量相乘。然后我们乘以第二个分量并将结果相加。结果是一个数字，表示两个向量的相似程度。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_142.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_143.png)

我们也可以使用NumPy的`dot`函数执行点积，并将其赋值给变量`result`。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_145.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_147.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_148.png)

## 广播与通用函数 📡

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_150.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_151.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_153.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_154.png)

考虑数组`u`。如果我们向数组添加一个标量值，NumPy会将该值添加到每个元素。这个特性被称为**广播**。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_156.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_157.png)

一个**通用函数**是操作于ND数组上的函数。我们可以将通用函数应用于NumPy数组。考虑数组`a`，我们可以使用方法`mean`计算`a`中所有元素的平均值。这对应于所有元素的平均值。在本例中，结果是0。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_159.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_161.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_162.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_163.png)

还有许多其他函数。例如，考虑NumPy数组`b`，我们可以使用方法`max`找到最大值。我们看到最大值是5，因此方法`max`返回5。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_165.png)

## 使用NumPy进行函数映射与绘图 📈

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_166.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_167.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_168.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_170.png)

我们可以使用NumPy创建将NumPy数组映射到新NumPy数组的函数。让我们在屏幕左侧实现一些代码，并使用屏幕右侧来演示发生了什么。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_172.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_173.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_175.png)

我们可以在NumPy中访问`pi`的值。我们可以在弧度中创建以下NumPy数组`x`。这个数组对应以下向量。我们可以将函数`sine`应用于数组`x`，并将值赋给数组`y`。这将对数组中的每个元素应用正弦函数。这对应于将正弦函数应用于向量的每个分量。结果是一个新数组`y`，其中每个值对应于对数组`x`中每个元素应用正弦函数的结果。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_177.png)

用于绘制数学函数的一个有用函数是`linspace`。`linspace`在指定间隔内返回均匀间隔的数字。我们指定序列的起点、序列的终点，参数`num`表示要生成的样本数，在本例中是5，样本之间的间隔是1。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_179.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_180.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_181.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_182.png)

如果我们将参数`num`改为9，我们会在从-2到2的区间内得到9个均匀间隔的数字。结果是后续样本之间的差值是0.5，而不是之前的1。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_184.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_185.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_187.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_188.png)

我们可以使用函数`linspace`从区间0到2π生成100个均匀间隔的样本。我们可以使用NumPy函数`sin`将数组`x`映射到新数组`y`。我们可以导入库`pyplot`作为`plt`来帮助我们绘制函数。由于我们使用的是Jupyter笔记本，我们使用命令`%matplotlib inline`来显示图表。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_190.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_192.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_194.png)

以下命令绘制一个图形。第一个输入对应于水平或X轴的值。第二个输入对应于垂直或Y轴的值。

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_195.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_197.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_199.png)

## 总结 🎯

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_201.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_203.png)

![](img/401ef7fdd8ee9093bf6fc6a11bb90342_204.png)

本节课中我们一起学习了一维NumPy数组的核心知识。我们涵盖了数组的创建、属性访问、索引切片、以及包括向量加法、减法、标量乘法、哈达玛积和点积在内的基本运算。我们还介绍了广播机制和通用函数，并学习了如何使用`linspace`和数学函数进行数组映射与简单绘图。NumPy的功能远不止于此，你可以通过实验或访问NumPy官网来探索更多高级特性。掌握NumPy是进行高效数据科学计算的重要基础。