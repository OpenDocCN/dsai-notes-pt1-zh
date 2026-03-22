数据分析高级Python：P40：NumPy数组的索引与切片 🧮

![](img/2a3a0afbe16b9be21ab21cf16380514e_1.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_2.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_4.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_6.png)

在本节课中，我们将要学习NumPy数组的索引与切片操作。索引用于访问数组中的单个元素，而切片则用于获取数组的一个子集。理解这两种操作对于高效处理数据至关重要。

![](img/2a3a0afbe16b9be21ab21cf16380514e_8.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_10.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_11.png)

上一节我们介绍了NumPy数组的基本概念，本节中我们来看看如何具体访问和提取数组中的数据。

![](img/2a3a0afbe16b9be21ab21cf16380514e_13.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_14.png)

### 低效的索引方式

![](img/2a3a0afbe16b9be21ab21cf16380514e_16.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_17.png)

以下方式不是一种高效的做法。

![](img/2a3a0afbe16b9be21ab21cf16380514e_19.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_20.png)

因为 `x[2][2]` 这种写法等同于以下步骤：

![](img/2a3a0afbe16b9be21ab21cf16380514e_22.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_23.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_24.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_25.png)

1.  首先，`x[2]` 会创建一个临时变量。
    ```python
    temp = x[2]
    ```
2.  然后，从这个临时变量中索引第二个元素。
    ```python
    a = temp[2]
    ```
3.  最终，变量 `a` 的值是 `15`。

![](img/2a3a0afbe16b9be21ab21cf16380514e_27.png)

这种做法效率不高。它会在内存和时间效率方面造成问题。

![](img/2a3a0afbe16b9be21ab21cf16380514e_29.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_30.png)

### 高效的NumPy索引方式

NumPy提供了一种更好的方式。使用 `x[2, 2]` 可以直接获取值，而无需创建临时变量。

![](img/2a3a0afbe16b9be21ab21cf16380514e_32.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_33.png)

这就是获取数组中特定值的方法。

![](img/2a3a0afbe16b9be21ab21cf16380514e_35.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_36.png)

### 负索引的使用

![](img/2a3a0afbe16b9be21ab21cf16380514e_37.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_38.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_39.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_40.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_42.png)

负索引允许从数组末尾开始计数。

例如，`x[-1]` 将返回最后一行。
`x[-1, 2]` 将返回最后一行的第三个元素（索引为2），其值为 `58`。

![](img/2a3a0afbe16b9be21ab21cf16380514e_44.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_45.png)

### 综合索引示例

![](img/2a3a0afbe16b9be21ab21cf16380514e_47.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_48.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_49.png)

可以组合使用正负索引。
*   `x[-1, -1]` 返回最后一个元素 `40`。
*   `x[1, -1]` 返回第二行（索引1）的最后一列（索引-1），其值为 `19`。
*   `x[1, -2]` 返回第二行的倒数第二列，其值为 `45`。

![](img/2a3a0afbe16b9be21ab21cf16380514e_50.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_52.png)

以下是列索引的对应关系：
*   正向索引：`0, 1, 2, 3, 4, 5`
*   反向索引：`-6, -5, -4, -3, -2, -1`

![](img/2a3a0afbe16b9be21ab21cf16380514e_54.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_55.png)

这就是NumPy数组中索引的工作方式。

### 一维数组切片

![](img/2a3a0afbe16b9be21ab21cf16380514e_57.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_58.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_59.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_60.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_61.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_62.png)

在学习了索引之后，接下来我们看看切片操作。切片是Python和NumPy中一个非常巧妙的概念。

![](img/2a3a0afbe16b9be21ab21cf16380514e_63.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_64.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_66.png)

切片操作使用冒号 `:` 运算符。

为了让理解更清晰，我们先从一维数组开始。

![](img/2a3a0afbe16b9be21ab21cf16380514e_68.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_69.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_70.png)

假设有一个一维数组 `y`：
```python
y = np.array([10, 20, 30, 40, 50, 2, 7])
```
*   `y[5]` 返回索引5的元素 `2`。
*   如果想切片获取 `[40, 50, 2]` 这部分（索引3到5），需要使用 `y[3:6]`。注意，起始索引3是包含的，而结束索引6是排除的。
*   `y[:6]` 会从第一个元素（索引0）开始，切片到索引6（排除）为止。
*   `y[3:]` 会从索引3开始，切片到最后一个元素。

![](img/2a3a0afbe16b9be21ab21cf16380514e_71.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_72.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_73.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_74.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_76.png)

这就是一维数组的切片，它可以获取数组的特定部分。

![](img/2a3a0afbe16b9be21ab21cf16380514e_78.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_79.png)

### 二维数组切片

![](img/2a3a0afbe16b9be21ab21cf16380514e_81.png)

现在，让我们看看切片在二维数组中如何工作。

![](img/2a3a0afbe16b9be21ab21cf16380514e_83.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_84.png)

假设我们想从数组 `x` 中提取一个矩形区域，例如包含元素 `45, 78, 15, 18` 的2x2子矩阵。

![](img/2a3a0afbe16b9be21ab21cf16380514e_85.png)

我们需要指定行和列的范围：
*   **行**：我们想要第一行和第二行（索引1和2）。切片写法是 `1:3`（因为结束索引3是排除的）。
*   **列**：我们想要第二列和第三列（索引2和3）。切片写法是 `2:4`。

因此，完整的切片操作是 `x[1:3, 2:4]`。这将返回一个新的NumPy数组，即我们想要的子矩阵。

![](img/2a3a0afbe16b9be21ab21cf16380514e_87.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_88.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_89.png)

通过切片，我们可以从原数组中提取出需要的部分。

![](img/2a3a0afbe16b9be21ab21cf16380514e_90.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_91.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_92.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_93.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_94.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_95.png)

### 切片的高级用法

![](img/2a3a0afbe16b9be21ab21cf16380514e_97.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_99.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_100.png)

我们可以省略切片的起始或结束索引。
*   `x[:3, 2:4]`：行从开始到索引2（3-1），列从索引2到3。
*   `x[1:, 2:4]`：行从索引1到最后，列从索引2到3。
*   `x[:, 2:4]`：所有行，列从索引2到3。
*   `x[2, :]`：第三行（索引2）的所有列。
*   `x[:, :]`：返回整个数组。

### 带步长的切片

![](img/2a3a0afbe16b9be21ab21cf16380514e_102.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_103.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_104.png)

切片还可以指定步长，用于间隔选取元素。

![](img/2a3a0afbe16b9be21ab21cf16380514e_105.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_106.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_107.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_108.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_109.png)

语法是 `start:stop:step`。
*   例如，`x[::2]` 会从开始到结束，每隔一行选取一行。
*   `x[:, ::2]` 会选取所有行，但每行列的步长为2。

![](img/2a3a0afbe16b9be21ab21cf16380514e_111.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_112.png)

![](img/2a3a0afbe16b9be21ab21cf16380514e_113.png)

本节课中我们一起学习了NumPy数组的索引与切片。索引（如 `x[2,2]`）用于访问单个元素，应避免使用 `x[2][2]` 这种低效方式。切片（使用 `:` 运算符）用于提取子数组，可以通过指定起始、结束和步长来灵活控制提取的范围。掌握这些操作是进行高效数组数据处理的基础。