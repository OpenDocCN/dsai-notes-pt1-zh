#  033：Python中的NumPy入门

在本节课中，我们将学习NumPy。NumPy是Python中用于科学计算的基础库，尤其在机器学习和数据科学领域至关重要。我们将从Python基础语法回顾开始，然后深入探讨NumPy是什么、为何重要，并通过实践练习来掌握其基本用法。

![](img/66d552279681f8aceedf9f46d54d53bf_1.png)

## Python基础语法快速回顾

上一节我们介绍了本节课的目标，本节中我们先快速回顾一下Python的基础语法。Python是一种语法易读、易于学习的编程语言，它是机器学习和数据科学领域最流行的语言。

Python流行的原因包括其丰富的库生态系统和强大的社区支持。以下是Python中的一些基本数据类型和控制结构：

*   **int**：整数类型。
*   **float**：浮点数类型，即带小数点的数字，例如 `2.3`。
*   **str**：字符串类型。
*   **bool**：布尔类型，取值为 `True` 或 `False`。
*   **list**：列表类型。我们将花一些时间讨论Python列表，因为后面会对比NumPy在数组操作上的优势。
*   **条件语句与循环**：包括 `if-else` 条件判断、`for` 循环和 `while` 循环等。
*   **函数**：使用 `def` 关键字定义代码块，可以重复调用。

如果你需要更系统地学习Python语法，可以参考提供的链接资源。为了课程的完整性，我们在此做一个简要概述。

## 什么是NumPy及其应用场景

在回顾了Python基础后，本节我们来看看NumPy是什么以及它如何用于处理数据。NumPy是Numerical Python的缩写，它是一个强大的Python库，主要用于处理大型多维数组和矩阵。

NumPy提供了一个高性能的多维数组对象 `ndarray`，以及用于操作这些数组的大量函数。在数据科学和机器学习中，我们经常需要高效地执行数值运算，NumPy正是为此而设计。

## NumPy在数据科学中的奠基性作用

了解了NumPy的基本定义后，我们来探讨它为何在数据科学和机器学习领域具有如此奠基性的地位。当NumPy被引入时，它解决了Python原生列表在数值计算上的几个关键瓶颈：

1.  **性能**：NumPy数组在内存中连续存储，并且运算由预编译的C代码执行，速度远快于Python列表的循环操作。
2.  **功能**：NumPy提供了大量高级数学函数，方便进行向量化操作，无需编写显式循环。
3.  **广播机制**：NumPy的广播功能允许不同形状的数组进行算术运算，这大大简化了代码。

正是这些特性，使得NumPy成为后续众多科学计算库（如Pandas、SciPy、Scikit-learn）的基础。

## NumPy实践练习

理论介绍完毕，现在让我们通过一些动手练习来熟悉NumPy。我们将使用Google Colab来编写和运行Python代码。

首先，我们需要导入NumPy库。在Python中，通常使用 `np` 作为NumPy的别名。

```python
import numpy as np
```

以下是几个核心概念的实践示例：

**1. 创建数组**
我们可以从Python列表创建NumPy数组。

```python
# 创建一维数组
arr_1d = np.array([1, 2, 3, 4, 5])
print("一维数组:", arr_1d)

# 创建二维数组（矩阵）
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
print("二维数组:\n", arr_2d)
```

**2. 数组属性**
每个NumPy数组都有描述其形状、大小和数据类型等信息的属性。

```python
print("数组形状 (shape):", arr_2d.shape)
print("数组维度 (ndim):", arr_2d.ndim)
print("数组元素总数 (size):", arr_2d.size)
print("数组数据类型 (dtype):", arr_2d.dtype)
```

**3. 生成特殊数组**
NumPy提供了快速生成特定数组的函数。

```python
# 生成全零数组
zeros_arr = np.zeros((2, 3))
print("全零数组:\n", zeros_arr)

# 生成全一数组
ones_arr = np.ones((3, 2))
print("全一数组:\n", ones_arr)

# 生成单位矩阵
identity_mat = np.eye(3)
print("3x3单位矩阵:\n", identity_mat)
```

**4. 数组运算**
NumPy支持元素级运算和矩阵运算。

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

# 元素加法
print("a + b =", a + b)

# 元素乘法
print("a * b =", a * b)

# 点积（内积）
print("a 和 b 的点积:", np.dot(a, b))
```

**5. 数组索引与切片**
访问和修改数组元素的方法与Python列表类似。

```python
arr = np.array([[10, 20, 30], [40, 50, 60], [70, 80, 90]])

# 访问单个元素
print("第二行第三列的元素:", arr[1, 2])

# 切片
print("第一行的所有元素:", arr[0, :])
print("第二列的所有元素:", arr[:, 1])
```

## 总结

![](img/66d552279681f8aceedf9f46d54d53bf_3.png)

本节课中，我们一起学习了NumPy的基础知识。我们首先快速回顾了Python的基本语法，然后介绍了NumPy库及其在高效处理数值数据方面的核心作用。我们探讨了NumPy相比Python原生列表的优势，包括其卓越的性能、丰富的功能和广播机制。最后，我们通过实际的代码练习，学习了如何创建数组、查看数组属性、进行数组运算以及索引切片。掌握NumPy是深入学习机器学习和数据科学的重要一步。