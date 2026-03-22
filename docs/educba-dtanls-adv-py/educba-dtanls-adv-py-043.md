# 043：NumPy示例 🧮

![](img/4ed4c4dd5f66754dc2525157071572be_1.png)

![](img/4ed4c4dd5f66754dc2525157071572be_2.png)

在本节课中，我们将学习一个使用NumPy创建特定数组的完整示例。我们将从获取用户输入开始，逐步构建一个数组，其中每个元素的值是其行索引与列索引的乘积。通过这个例子，你将理解如何结合Python基础操作与NumPy数组操作来解决实际问题。

![](img/4ed4c4dd5f66754dc2525157071572be_3.png)

![](img/4ed4c4dd5f66754dc2525157071572be_4.png)

![](img/4ed4c4dd5f66754dc2525157071572be_5.png)

![](img/4ed4c4dd5f66754dc2525157071572be_6.png)

## 获取用户输入并处理

![](img/4ed4c4dd5f66754dc2525157071572be_8.png)

首先，我们需要从用户那里获取数组的行数和列数。用户输入通常以字符串形式接收，例如“3,4”。我们的任务是解析这个字符串，并将其转换为两个独立的整数。

![](img/4ed4c4dd5f66754dc2525157071572be_10.png)

![](img/4ed4c4dd5f66754dc2525157071572be_12.png)

![](img/4ed4c4dd5f66754dc2525157071572be_14.png)

以下是处理用户输入的步骤：

![](img/4ed4c4dd5f66754dc2525157071572be_16.png)

![](img/4ed4c4dd5f66754dc2525157071572be_18.png)

1.  使用 `input()` 函数获取用户输入的字符串。
2.  使用字符串的 `.split()` 方法，根据逗号分隔符将字符串分割成一个字符串列表。
3.  使用列表推导式，将列表中的每个字符串元素转换为整数类型。
4.  利用Python的列表解包功能，将整数列表中的两个值分别赋值给 `row` 和 `column` 变量。

![](img/4ed4c4dd5f66754dc2525157071572be_20.png)

```python
# 获取用户输入，例如：“3,4”
user_input = input(“Enter the shape of the array (rows,columns): “)

![](img/4ed4c4dd5f66754dc2525157071572be_22.png)

![](img/4ed4c4dd5f66754dc2525157071572be_23.png)

![](img/4ed4c4dd5f66754dc2525157071572be_25.png)

# 将输入字符串按逗号分割并转换为整数列表
row, column = [int(element) for element in user_input.split(‘,’)]

![](img/4ed4c4dd5f66754dc2525157071572be_27.png)

print(f“Row: {row}, Column: {column}”)
```

![](img/4ed4c4dd5f66754dc2525157071572be_29.png)

## 创建初始NumPy数组

上一节我们介绍了如何获取用户指定的行数和列数。本节中，我们来看看如何利用这些信息创建一个初始的NumPy数组。

我们将使用 `np.ones()` 函数创建一个指定形状且元素全为1的数组。你也可以使用 `np.zeros()` 创建全零数组。为了确保数组元素是整数类型，我们需要显式指定 `dtype` 参数。

![](img/4ed4c4dd5f66754dc2525157071572be_31.png)

![](img/4ed4c4dd5f66754dc2525157071572be_32.png)

![](img/4ed4c4dd5f66754dc2525157071572be_33.png)

```python
import numpy as np

![](img/4ed4c4dd5f66754dc2525157071572be_35.png)

# 根据用户输入的行和列，创建一个全为1的整数数组
initial_array = np.ones((row, column), dtype=int)
print(“Initial array of ones:”)
print(initial_array)
```

![](img/4ed4c4dd5f66754dc2525157071572be_37.png)

![](img/4ed4c4dd5f66754dc2525157071572be_39.png)

## 根据规则填充数组

![](img/4ed4c4dd5f66754dc2525157071572be_41.png)

现在我们已经有了一个指定形状的数组。接下来，我们需要根据题目要求，用每个元素的行索引乘以列索引的结果来填充这个数组。

![](img/4ed4c4dd5f66754dc2525157071572be_42.png)

![](img/4ed4c4dd5f66754dc2525157071572be_43.png)

![](img/4ed4c4dd5f66754dc2525157071572be_45.png)

我们将使用嵌套的 `for` 循环来遍历数组的每一个位置。外层循环遍历行索引 `i`，内层循环遍历列索引 `j`。对于位置 `(i, j)`，我们将其值设置为 `i * j`。

![](img/4ed4c4dd5f66754dc2525157071572be_47.png)

```python
# 遍历数组的每一行和每一列
for i in range(row):
    for j in range(column):
        # 将数组在(i, j)位置的值设置为行索引i乘以列索引j
        initial_array[i, j] = i * j

![](img/4ed4c4dd5f66754dc2525157071572be_49.png)

print(“Final array (row_index * column_index):”)
print(initial_array)
```

运行程序，输入“5,9”，你将得到一个5行9列的数组，其中每个元素都符合 `a[i, j] = i * j` 的规则。

## 总结

![](img/4ed4c4dd5f66754dc2525157071572be_51.png)

![](img/4ed4c4dd5f66754dc2525157071572be_52.png)

![](img/4ed4c4dd5f66754dc2525157071572be_53.png)

本节课中我们一起学习了如何构建一个完整的NumPy应用示例。我们从处理原始的用户输入字符串开始，将其转换为可用的整数维度。然后，我们利用这些维度创建了一个初始的NumPy数组。最后，我们通过循环遍历数组的每个位置，并根据行索引和列索引的乘积来填充数组，完成了题目的要求。这个例子展示了将基础的Python编程（输入处理、循环）与NumPy的数组操作相结合来解决数据问题的典型流程。