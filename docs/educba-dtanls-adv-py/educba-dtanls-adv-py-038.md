# 038：不同维度的NumPy数组 📊

![](img/db1bfafa2f8831d2e1d408ad67930dfc_1.png)

![](img/db1bfafa2f8831d2e1d408ad67930dfc_3.png)

在本节课中，我们将学习如何创建不同维度的NumPy数组。我们将从零维数组开始，逐步过渡到一维、二维乃至更高维度的数组，并学习如何查看数组的维度和形状。

---

![](img/db1bfafa2f8831d2e1d408ad67930dfc_5.png)

## 零维数组

零维数组本质上就是一个单一的值。它是最简单的NumPy数组形式。

首先，我们需要导入NumPy模块。

![](img/db1bfafa2f8831d2e1d408ad67930dfc_7.png)

![](img/db1bfafa2f8831d2e1d408ad67930dfc_8.png)

```python
import numpy as np
```

接下来，我们创建一个零维数组。使用`np.array()`方法，并传入一个单一的值。

```python
x = np.array(42)
print(x)
print(type(x))
print("维度:", np.ndim(x))
```

运行上述代码，输出结果如下：
- `x`的值是`42`。
- `x`的类型是`numpy.ndarray`。
- `x`的维度是`0`。

![](img/db1bfafa2f8831d2e1d408ad67930dfc_10.png)

零维数组在实际数据分析中不常用，但它是理解数组维度的基础。

---

## 一维数组

![](img/db1bfafa2f8831d2e1d408ad67930dfc_12.png)

上一节我们介绍了零维数组，本节中我们来看看一维数组。一维数组类似于一个列表，包含一系列有序的元素。

以下是创建一维数组的步骤：

1.  创建一个整数类型的一维数组。
2.  创建一个浮点数类型的一维数组。
3.  打印数组及其属性。

![](img/db1bfafa2f8831d2e1d408ad67930dfc_14.png)

```python
# 创建整数数组
a = np.array([1, 2, 3])
# 创建浮点数数组
b = np.array([1.1, 2.5, 6.3])

![](img/db1bfafa2f8831d2e1d408ad67930dfc_16.png)

![](img/db1bfafa2f8831d2e1d408ad67930dfc_17.png)

print("数组 a:", a)
print("数组 b:", b)
print("a 的数据类型:", a.dtype)
print("b 的数据类型:", b.dtype)
print("a 的维度:", np.ndim(a))
print("b 的维度:", np.ndim(b))
```

![](img/db1bfafa2f8831d2e1d408ad67930dfc_19.png)

除了使用`np.ndim()`函数，我们还可以通过数组对象的`.ndim`属性来获取维度。

```python
print("a 的维度 (使用属性):", a.ndim)
```

---

## 二维及多维数组

理解了零维和一维数组后，现在我们来探讨二维及多维数组。二维数组可以看作是一个“列表的列表”，即矩阵。

![](img/db1bfafa2f8831d2e1d408ad67930dfc_21.png)

以下是创建二维数组的方法：

```python
# 创建一个二维数组（矩阵）
A = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])

![](img/db1bfafa2f8831d2e1d408ad67930dfc_23.png)

![](img/db1bfafa2f8831d2e1d408ad67930dfc_25.png)

print("二维数组 A:")
print(A)
print("A 的维度:", A.ndim)
```

输出显示`A`是一个3x3的矩阵，其维度为`2`。

![](img/db1bfafa2f8831d2e1d408ad67930dfc_27.png)

那么，我们能否创建三维数组呢？答案是肯定的。三维数组可以理解为“列表的列表的列表”。

```python
# 创建一个三维数组
A_3d = np.array([[[1, 2], [3, 4]],
                 [[5, 6], [7, 8]],
                 [[9, 10], [11, 12]]])

![](img/db1bfafa2f8831d2e1d408ad67930dfc_29.png)

print("三维数组 A_3d:")
print(A_3d)
print("A_3d 的维度:", A_3d.ndim)
```

在这个例子中，我们创建了一个结构更复杂的三维数组，其维度为`3`。通过这种方式，我们可以构建任意维度的数组。

![](img/db1bfafa2f8831d2e1d408ad67930dfc_31.png)

---

![](img/db1bfafa2f8831d2e1d408ad67930dfc_32.png)

## 数组的形状

除了维度，数组的**形状**也是一个重要属性。形状描述了数组在每个维度上的大小。

我们可以使用`.shape`属性来查看数组的形状。

![](img/db1bfafa2f8831d2e1d408ad67930dfc_34.png)

```python
print("一维数组 a 的形状:", a.shape)
print("二维数组 A 的形状:", A.shape)
print("三维数组 A_3d 的形状:", A_3d.shape)
```

![](img/db1bfafa2f8831d2e1d408ad67930dfc_36.png)

对于一维数组`a = [1, 2, 3]`，其形状是`(3,)`，表示它有3个元素。
对于二维数组`A`，其形状是`(3, 3)`，表示它是一个3行3列的矩阵。
对于三维数组`A_3d`，其形状是`(3, 2, 2)`，表示它由3个2x2的矩阵组成。

![](img/db1bfafa2f8831d2e1d408ad67930dfc_37.png)

理解形状有助于我们掌握数组的数据结构，为后续的数据操作和计算打下基础。

---

![](img/db1bfafa2f8831d2e1d408ad67930dfc_39.png)

本节课中我们一起学习了如何创建和识别不同维度的NumPy数组，包括零维、一维、二维和三维数组。我们还学习了如何通过`.ndim`属性查看数组的维度，以及通过`.shape`属性查看数组的形状。这些基本概念是使用NumPy进行高效数据分析的基石。