# 037：创建NumPy数组 🧮

![](img/8bec742d4871df6c06697e0bcf1a4a1e_1.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_2.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_4.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_5.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_6.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_7.png)

在本节课中，我们将学习如何创建NumPy数组。NumPy数组是进行高效数值计算的基础，它比Python原生列表更节省空间，计算速度也更快。我们将介绍两种创建数组的核心函数：`arange`和`linspace`。

![](img/8bec742d4871df6c06697e0bcf1a4a1e_9.png)

上一节我们了解了NumPy的优势，本节中我们来看看如何实际创建NumPy数组。

![](img/8bec742d4871df6c06697e0bcf1a4a1e_11.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_12.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_13.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_14.png)

## 使用 `arange` 函数创建数组

![](img/8bec742d4871df6c06697e0bcf1a4a1e_16.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_17.png)

`arange` 函数用于在给定的区间内返回均匀间隔的值。其基本语法是 `np.arange(start, stop, step)`。

![](img/8bec742d4871df6c06697e0bcf1a4a1e_18.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_19.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_20.png)

以下是使用 `arange` 函数创建数组的几种方式：

![](img/8bec742d4871df6c06697e0bcf1a4a1e_22.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_23.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_24.png)

*   **指定起始和结束值**：`np.arange(1, 10)` 会创建一个从1（包含）到10（不包含）的整数数组。
    ```python
    import numpy as np
    a = np.arange(1, 10)
    print(a)  # 输出: [1 2 3 4 5 6 7 8 9]
    ```

![](img/8bec742d4871df6c06697e0bcf1a4a1e_26.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_27.png)

*   **仅指定结束值**：`np.arange(10)` 会创建一个从0（默认起始值）到9的整数数组。
    ```python
    a = np.arange(10)
    print(a)  # 输出: [0 1 2 3 4 5 6 7 8 9]
    ```

![](img/8bec742d4871df6c06697e0bcf1a4a1e_29.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_30.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_31.png)

*   **指定步长**：`np.arange(0.3, 1.5, 0.7)` 会创建一个从0.3开始，以0.7为步长递增，直到小于1.5的浮点数数组。
    ```python
    a = np.arange(0.3, 1.5, 0.7)
    print(a)  # 输出: [0.3 1. ]
    ```

![](img/8bec742d4871df6c06697e0bcf1a4a1e_33.png)

*   **创建递减数组**：通过设置负的步长，可以创建递减的数组，例如 `np.arange(10.5, 0.5, -2)`。
    ```python
    a = np.arange(10.5, 0.5, -2)
    print(a)  # 输出: [10.5  8.5  6.5  4.5  2.5]
    ```

![](img/8bec742d4871df6c06697e0bcf1a4a1e_35.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_36.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_37.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_38.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_39.png)

## 使用 `linspace` 函数创建数组

![](img/8bec742d4871df6c06697e0bcf1a4a1e_41.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_43.png)

`linspace` 函数用于在指定的起始值和结束值之间，返回指定数量的、**等间距**的数据点。其基本语法是 `np.linspace(start, stop, num)`。

![](img/8bec742d4871df6c06697e0bcf1a4a1e_45.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_46.png)

以下是 `linspace` 函数的特点和用法：

![](img/8bec742d4871df6c06697e0bcf1a4a1e_48.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_50.png)

*   **默认生成50个点**：如果不指定 `num` 参数，`np.linspace(1, 10)` 默认会在1到10之间生成50个等间距的点。
    ```python
    a = np.linspace(1, 10)
    print(a)  # 输出一个包含50个元素的数组
    print(type(a))  # 输出: <class 'numpy.ndarray'>
    ```

![](img/8bec742d4871df6c06697e0bcf1a4a1e_52.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_53.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_54.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_55.png)

*   **指定生成点的数量**：`np.linspace(1, 10, 7)` 会在1到10之间生成7个等间距的点。
    ```python
    a = np.linspace(1, 10, 7)
    print(a)  # 输出: [ 1.   2.5  4.   5.5  7.   8.5 10. ]
    ```
    可以看到，相邻点之间的差值（如2.5-1=1.5， 4-2.5=1.5）是相等的。

![](img/8bec742d4871df6c06697e0bcf1a4a1e_56.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_58.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_59.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_60.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_61.png)

![](img/8bec742d4871df6c06697e0bcf1a4a1e_62.png)

本节课中我们一起学习了创建NumPy数组的两种主要方法：使用 `arange` 函数生成按固定步长变化的序列，以及使用 `linspace` 函数生成指定数量的等间距点。理解并掌握这些方法是高效使用NumPy进行数据分析的第一步。