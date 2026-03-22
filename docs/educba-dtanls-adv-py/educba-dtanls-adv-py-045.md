数据分析高级Python：构建与优化：45：使用缩放器处理NumPy数组 🔢

![](img/84133d2c5903baee481bbe12458ff78e_0.png)

![](img/84133d2c5903baee481bbe12458ff78e_2.png)

![](img/84133d2c5903baee481bbe12458ff78e_4.png)

![](img/84133d2c5903baee481bbe12458ff78e_6.png)

在本节课中，我们将要学习如何在NumPy数组上进行数值运算。我们将从最简单的标量运算开始，逐步过渡到数组之间的算术运算，并理解NumPy如何高效地处理这些操作。

![](img/84133d2c5903baee481bbe12458ff78e_8.png)

![](img/84133d2c5903baee481bbe12458ff78e_10.png)

---

![](img/84133d2c5903baee481bbe12458ff78e_12.png)

![](img/84133d2c5903baee481bbe12458ff78e_13.png)

![](img/84133d2c5903baee481bbe12458ff78e_15.png)

上一节我们介绍了NumPy数组的基础知识，本节中我们来看看如何对数组进行数值运算。首先，我们将从标量运算开始。

![](img/84133d2c5903baee481bbe12458ff78e_17.png)

![](img/84133d2c5903baee481bbe12458ff78e_19.png)

### 使用标量进行运算

![](img/84133d2c5903baee481bbe12458ff78e_20.png)

![](img/84133d2c5903baee481bbe12458ff78e_22.png)

标量运算是指将一个单一的数值（标量）与数组中的每一个元素进行运算。以下是具体步骤：

![](img/84133d2c5903baee481bbe12458ff78e_24.png)

![](img/84133d2c5903baee481bbe12458ff78e_25.png)

![](img/84133d2c5903baee481bbe12458ff78e_27.png)

第一步，导入NumPy库并创建一个数组。

![](img/84133d2c5903baee481bbe12458ff78e_29.png)

![](img/84133d2c5903baee481bbe12458ff78e_30.png)

```python
import numpy as np
a = np.array([15, 8, 4, 51])
```

![](img/84133d2c5903baee481bbe12458ff78e_32.png)

![](img/84133d2c5903baee481bbe12458ff78e_34.png)

![](img/84133d2c5903baee481bbe12458ff78e_36.png)

第二步，将标量 `2` 与数组 `a` 相加。

```python
a = a + 2
print(a)
```
执行上述代码后，数组 `a` 中的每个元素都会增加 `2`，结果为 `[17, 10, 6, 53]`。

NumPy支持所有基本的算术运算符。以下是其他运算符的示例：

![](img/84133d2c5903baee481bbe12458ff78e_38.png)

![](img/84133d2c5903baee481bbe12458ff78e_39.png)

![](img/84133d2c5903baee481bbe12458ff78e_40.png)

![](img/84133d2c5903baee481bbe12458ff78e_41.png)

![](img/84133d2c5903baee481bbe12458ff78e_42.png)

![](img/84133d2c5903baee481bbe12458ff78e_43.png)

以下是使用不同运算符的示例：
*   **乘法**：`a * 2` 会将每个元素乘以 `2`。
*   **除法**：`a / 2` 会将每个元素除以 `2`，结果为浮点数。
*   **整除**：`a // 2` 会得到每个元素除以 `2` 后的整数部分。
*   **取模**：`a % 2` 会得到每个元素除以 `2` 后的余数（结果为 `0` 或 `1`）。
*   **幂运算**：`a ** 2` 会计算每个元素的平方。

![](img/84133d2c5903baee481bbe12458ff78e_45.png)

![](img/84133d2c5903baee481bbe12458ff78e_47.png)

![](img/84133d2c5903baee481bbe12458ff78e_49.png)

与使用Python列表相比，NumPy的标量运算非常高效。如果要对列表进行相同操作，需要使用循环遍历每个元素，而NumPy则一次性完成所有计算。

![](img/84133d2c5903baee481bbe12458ff78e_51.png)

![](img/84133d2c5903baee481bbe12458ff78e_53.png)

---

![](img/84133d2c5903baee481bbe12458ff78e_55.png)

![](img/84133d2c5903baee481bbe12458ff78e_56.png)

![](img/84133d2c5903baee481bbe12458ff78e_57.png)

了解了标量与数组的运算后，接下来我们看看两个数组之间如何进行算术运算。

![](img/84133d2c5903baee481bbe12458ff78e_59.png)

![](img/84133d2c5903baee481bbe12458ff78e_61.png)

![](img/84133d2c5903baee481bbe12458ff78e_63.png)

![](img/84133d2c5903baee481bbe12458ff78e_64.png)

### 对数组进行算术运算

![](img/84133d2c5903baee481bbe12458ff78e_66.png)

![](img/84133d2c5903baee481bbe12458ff78e_68.png)

数组间的算术运算是指两个形状相同的数组，其对应位置的元素进行运算。

![](img/84133d2c5903baee481bbe12458ff78e_70.png)

![](img/84133d2c5903baee481bbe12458ff78e_71.png)

首先，我们创建两个形状相同的数组 `a` 和 `b`。

![](img/84133d2c5903baee481bbe12458ff78e_73.png)

![](img/84133d2c5903baee481bbe12458ff78e_75.png)

![](img/84133d2c5903baee481bbe12458ff78e_77.png)

```python
import numpy as np
a = np.array([[11, 12, 13], [21, 22, 23], [31, 32, 33]])
b = np.ones((3, 3), dtype=int)  # 创建一个3x3的全1数组
print(a)
print(b)
```

![](img/84133d2c5903baee481bbe12458ff78e_78.png)

![](img/84133d2c5903baee481bbe12458ff78e_80.png)

![](img/84133d2c5903baee481bbe12458ff78e_82.png)

现在，我们可以对这两个数组进行运算：

![](img/84133d2c5903baee481bbe12458ff78e_84.png)

![](img/84133d2c5903baee481bbe12458ff78e_85.png)

![](img/84133d2c5903baee481bbe12458ff78e_87.png)

以下是数组间运算的示例：
*   **加法**：`a + b` 会将 `a` 和 `b` 中对应位置的元素相加。
*   **减法**：`a - b` 会将 `a` 和 `b` 中对应位置的元素相减。
*   **乘法**：`a * b` 执行的是**逐元素乘法**，即将 `a` 和 `b` 中对应位置的元素相乘，而不是矩阵乘法。

![](img/84133d2c5903baee481bbe12458ff78e_89.png)

![](img/84133d2c5903baee481bbe12458ff78e_90.png)

这些运算都是逐元素进行的，要求参与运算的数组形状必须一致。

![](img/84133d2c5903baee481bbe12458ff78e_92.png)

![](img/84133d2c5903baee481bbe12458ff78e_94.png)

---

![](img/84133d2c5903baee481bbe12458ff78e_96.png)

![](img/84133d2c5903baee481bbe12458ff78e_98.png)

![](img/84133d2c5903baee481bbe12458ff78e_99.png)

本节课中我们一起学习了NumPy数组的数值运算。我们首先掌握了如何使用标量与数组进行高效运算，包括加、减、乘、除等。然后，我们学习了如何对两个形状相同的数组进行逐元素的算术运算。NumPy的这些特性使其在数据分析和科学计算中比原生Python列表更加高效和便捷。