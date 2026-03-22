# 042：零一数组的创建与应用 🧮

![](img/ce15de5c0e72402ae730d998c8173ef4_1.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_2.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_3.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_4.png)

在本节课中，我们将学习如何使用NumPy库快速创建全零或全一的数组，并探讨一个结合用户输入和数组操作的实际应用问题。

![](img/ce15de5c0e72402ae730d998c8173ef4_6.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_8.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_10.png)

## 创建全零和全一数组

![](img/ce15de5c0e72402ae730d998c8173ef4_12.png)

上一节我们介绍了NumPy数组的基础知识，本节中我们来看看如何快速创建具有特定形状和初始值的数组。NumPy提供了便捷的函数来创建全零或全一的数组。

![](img/ce15de5c0e72402ae730d998c8173ef4_14.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_15.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_17.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_19.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_20.png)

以下是创建全一数组的代码示例：

![](img/ce15de5c0e72402ae730d998c8173ef4_22.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_23.png)

```python
import numpy as np
o = np.ones((2, 3))
print(o)
```
运行上述代码，将得到一个形状为2行3列、元素全为浮点数1的数组。

![](img/ce15de5c0e72402ae730d998c8173ef4_25.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_27.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_29.png)

默认情况下，`np.ones` 和 `np.zeros` 函数创建的是浮点数类型的数组。如果需要整数类型的数组，可以通过 `dtype` 参数指定。

以下是创建整数类型全一数组的代码：

![](img/ce15de5c0e72402ae730d998c8173ef4_31.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_32.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_33.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_34.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_35.png)

```python
o = np.ones((2, 3), dtype=int)
print(o)
```

![](img/ce15de5c0e72402ae730d998c8173ef4_37.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_39.png)

同样地，我们可以创建全零数组。以下是创建全零数组的代码示例：

![](img/ce15de5c0e72402ae730d998c8173ef4_41.png)

```python
z = np.zeros((10, 15), dtype=int)
print(z)
```

![](img/ce15de5c0e72402ae730d998c8173ef4_43.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_45.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_47.png)

## 应用实例：根据用户输入创建自定义数组

![](img/ce15de5c0e72402ae730d998c8173ef4_49.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_50.png)

掌握了创建基础数组的方法后，我们来看一个综合性的应用问题。这个问题将结合用户输入、字符串处理和数组计算。

![](img/ce15de5c0e72402ae730d998c8173ef4_52.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_53.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_54.png)

问题描述如下：从用户处获取一个表示数组形状的字符串（例如 “3,4”），然后创建一个该形状的数组。数组中的每个元素值等于其行号乘以列号。

![](img/ce15de5c0e72402ae730d998c8173ef4_56.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_58.png)

为了清晰地理解要求，我们来看一个具体例子。如果用户输入 “3,4”，我们需要创建一个3行4列的数组。其元素值计算规则如下：
*   第0行：所有元素值为 0 * 列号，因此是 `[0, 0, 0, 0]`
*   第1行：元素值为 1 * 列号，因此是 `[0, 1, 2, 3]`
*   第2行：元素值为 2 * 列号，因此是 `[0, 2, 4, 6]`

![](img/ce15de5c0e72402ae730d998c8173ef4_60.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_61.png)

最终生成的数组应为：
```
[[0 0 0 0]
 [0 1 2 3]
 [0 2 4 6]]
```

![](img/ce15de5c0e72402ae730d998c8173ef4_63.png)

## 问题解决方案

![](img/ce15de5c0e72402ae730d998c8173ef4_65.png)

现在，我们来一步步解决这个问题。以下是实现该功能的完整代码：

![](img/ce15de5c0e72402ae730d998c8173ef4_67.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_68.png)

```python
import numpy as np

![](img/ce15de5c0e72402ae730d998c8173ef4_70.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_71.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_72.png)

# 1. 获取用户输入
shape_str = input(“请输入数组的形状（例如 3,4）: “)

# 2. 处理输入字符串，分割出行数和列数
shape_list = shape_str.split(‘,’)
print(shape_list)  # 例如输入”3,4″，则输出 [‘3’, ‘4’]

![](img/ce15de5c0e72402ae730d998c8173ef4_74.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_75.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_76.png)

# 3. 将字符串转换为整数，并获取行数和列数
rows = int(shape_list[0])
cols = int(shape_list[1])

![](img/ce15de5c0e72402ae730d998c8173ef4_78.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_80.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_81.png)

# 4. 创建一个全零数组作为基础
arr = np.zeros((rows, cols), dtype=int)

![](img/ce15de5c0e72402ae730d998c8173ef4_83.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_84.png)

# 5. 使用循环计算每个元素的值（行号 * 列号）
for i in range(rows):          # i 代表行索引（行号）
    for j in range(cols):      # j 代表列索引（列号）
        arr[i, j] = i * j

![](img/ce15de5c0e72402ae730d998c8173ef4_86.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_88.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_89.png)

# 6. 打印最终结果
print(arr)
```

代码执行步骤如下：
1.  使用 `input()` 函数获取用户输入的形状字符串。
2.  使用 `split(‘,’)` 方法将字符串按逗号分割，得到一个字符串列表。
3.  将列表中的字符串元素转换为整数，分别赋值给 `rows`（行数）和 `cols`（列数）。
4.  使用 `np.zeros` 创建一个指定形状的全零整数数组作为初始模板。
5.  使用两层嵌套循环遍历数组的每个位置。外层循环变量 `i` 是行号，内层循环变量 `j` 是列号。
6.  根据规则 `元素值 = 行号 * 列号`，即 `arr[i, j] = i * j`，为每个元素赋值。
7.  打印出最终计算得到的数组。

![](img/ce15de5c0e72402ae730d998c8173ef4_91.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_92.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_93.png)

![](img/ce15de5c0e72402ae730d998c8173ef4_94.png)

本节课中我们一起学习了如何使用 `np.ones` 和 `np.zeros` 函数快速创建特定值的数组，并通过一个实际案例，综合运用了输入处理、类型转换和数组操作来解决问题。掌握这些基础但强大的数组创建方法，是进行高效数据分析和科学计算的重要一步。