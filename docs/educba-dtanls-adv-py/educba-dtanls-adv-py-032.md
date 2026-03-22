数据分析高级Python：P32：使用列表推导式与NumPy进行温度转换 📊

![](img/3a0c909b9ad13a24d7c2712d2bbba604_1.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_3.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_5.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_7.png)

在本节课中，我们将学习如何使用Python的列表推导式和NumPy库来高效地进行温度转换，并初步了解如何使用Matplotlib进行数据可视化。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_9.png)

上一节我们介绍了数据处理的基本概念，本节中我们来看看如何将摄氏温度列表转换为华氏温度。

### 使用列表推导式进行转换

![](img/3a0c909b9ad13a24d7c2712d2bbba604_11.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_12.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_13.png)

首先，我们有一个包含摄氏温度的列表。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_15.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_16.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_18.png)

```python
temp = [21.1, 23.5, 20.6, 22.1, 25.0, 19.8, 18.9, 24.2, 22.7]
```

![](img/3a0c909b9ad13a24d7c2712d2bbba604_20.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_22.png)

我们的目标是为列表中的每个元素创建一个新列表。新列表中的每个元素由原列表中的对应元素通过公式转换而来。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_24.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_26.png)

转换公式为：**华氏温度 = 摄氏温度 × 9 / 5 + 32**

![](img/3a0c909b9ad13a24d7c2712d2bbba604_27.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_29.png)

以下是使用列表推导式完成此操作的方法：

![](img/3a0c909b9ad13a24d7c2712d2bbba604_31.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_33.png)

```python
fahrenheit = [element * 9 / 5 + 32 for element in temp]
```

这段代码会遍历 `temp` 列表中的每个元素，对每个元素执行公式计算，并将结果收集到一个名为 `fahrenheit` 的新列表中。打印 `fahrenheit` 列表将显示所有转换后的华氏温度值。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_35.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_36.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_37.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_38.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_39.png)

### 使用NumPy数组进行转换

![](img/3a0c909b9ad13a24d7c2712d2bbba604_41.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_42.png)

现在，让我们看看如何使用NumPy库以更高效的方式完成相同的任务。首先，我们需要从列表创建一个NumPy数组。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_44.png)

```python
import numpy as np
c = np.array(temp)
```

打印 `c` 的类型，你会发现它不再是普通的Python列表，而是一个 `numpy.ndarray` 对象。NumPy数组的优势在于可以对整个数组执行操作，而无需显式地遍历每个元素。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_46.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_47.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_48.png)

要从摄氏温度数组创建华氏温度数组，可以直接对整个数组应用转换公式：

![](img/3a0c909b9ad13a24d7c2712d2bbba604_49.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_50.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_51.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_53.png)

```python
f = c * 9 / 5 + 32
```

![](img/3a0c909b9ad13a24d7c2712d2bbba604_55.png)

打印 `f`，你会得到与列表推导式相同的结果。NumPy在底层进行了优化，这种向量化操作通常比循环或列表推导式在时间和空间上更高效。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_57.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_58.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_60.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_61.png)

### 使用Matplotlib进行数据可视化

数据转换后，我们常常希望将其可视化。Matplotlib是Python中一个强大的绘图库。

首先，导入Matplotlib的pyplot模块：

![](img/3a0c909b9ad13a24d7c2712d2bbba604_63.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_64.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_65.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_66.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_67.png)

```python
import matplotlib.pyplot as plt
```

![](img/3a0c909b9ad13a24d7c2712d2bbba604_68.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_69.png)

要绘制摄氏温度随时间变化的折线图，可以使用以下代码：

```python
plt.plot(temp)
plt.show()
```

![](img/3a0c909b9ad13a24d7c2712d2bbba604_71.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_72.png)

这将在新窗口中显示一个简单的折线图，其中X轴是数据点的索引（0, 1, 2...），Y轴是温度值。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_73.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_74.png)

如果我们希望X轴代表具体的日期，可以提供一个日期列表：

![](img/3a0c909b9ad13a24d7c2712d2bbba604_76.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_77.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_78.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_79.png)

```python
days = [22, 23, 24, 25, 26, 27, 28, 29, 30]
plt.plot(days, temp)
plt.show()
```

![](img/3a0c909b9ad13a24d7c2712d2bbba604_80.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_82.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_83.png)

现在，图表将在X轴上显示日期，在Y轴上显示对应的温度值，使得数据在时间维度上的变化一目了然。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_85.png)

Matplotlib的pyplot模块提供了一个类似MATLAB的、过程化的绘图接口，但它完全免费且开源，功能强大。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_87.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_88.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_89.png)

### 总结

![](img/3a0c909b9ad13a24d7c2712d2bbba604_91.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_92.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_94.png)

本节课中我们一起学习了：
1.  使用**列表推导式** `[element * 9 / 5 + 32 for element in temp]` 进行元素级转换。
2.  使用**NumPy数组**进行向量化运算 `c * 9 / 5 + 32`，这种方法在处理大型数据集时效率更高。
3.  使用**Matplotlib**的 `plt.plot()` 和 `plt.show()` 函数对转换后的数据进行基本可视化，并可以自定义坐标轴标签。

![](img/3a0c909b9ad13a24d7c2712d2bbba604_96.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_97.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_98.png)

![](img/3a0c909b9ad13a24d7c2712d2bbba604_99.png)

通过结合这些工具，我们可以更高效地处理和分析数据，并为后续学习更复杂的数据科学和物联网应用打下基础。在接下来的课程中，我们将更深入地探讨NumPy、Matplotlib以及Pandas等库。