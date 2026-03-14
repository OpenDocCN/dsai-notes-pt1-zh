# 绘图必备Matplotlib，P14：`fig.add_subplot(111)` 是什么意思？ 🤔

在本节课中，我们将要学习Matplotlib中一个常见但可能令人困惑的写法：`fig.add_subplot(111)`。我们将详细解释它的作用、参数含义以及它与创建子图的其他方式有何不同。

![](img/5f2ab3c17c0f8a480827c5087baec5a2_1.png)

---

## 概述：`add_subplot` 的基本作用

首先，我们需要理解`fig.add_subplot()`的核心功能。它的作用是为一个已经存在的图形（`Figure`）对象添加一个坐标轴（`Axes`）对象。

![](img/5f2ab3c17c0f8a480827c5087baec5a2_3.png)

坐标轴是实际绘制图表（如折线图、柱状图）的区域。没有坐标轴，图形只是一个空白的画布。

为了演示这一点，我们先创建一个没有任何坐标轴的空白图形。

```python
import matplotlib.pyplot as plt

![](img/5f2ab3c17c0f8a480827c5087baec5a2_7.png)

fig = plt.figure()  # 创建一个空白图形
plt.show()
```

![](img/5f2ab3c17c0f8a480827c5087baec5a2_8.png)

运行上述代码，你会看到一个空白的窗口。这是因为图形虽然存在，但内部没有可以绘图的坐标轴。

---

![](img/5f2ab3c17c0f8a480827c5087baec5a2_10.png)

## 使用 `add_subplot` 添加坐标轴

上一节我们介绍了空白图形，本节中我们来看看如何使用`add_subplot`为它添加坐标轴。

现在，我们使用`add_subplot`方法来为这个图形添加一个坐标轴。

![](img/5f2ab3c17c0f8a480827c5087baec5a2_12.png)

```python
import matplotlib.pyplot as plt

fig = plt.figure()  # 创建一个空白图形
ax = fig.add_subplot(111)  # 添加一个坐标轴
plt.show()
```

![](img/5f2ab3c17c0f8a480827c5087baec5a2_14.png)

运行代码后，你会看到图形中出现了带有刻度的坐标轴。我们可以打印`ax`的类型来验证。

![](img/5f2ab3c17c0f8a480827c5087baec5a2_16.png)

```python
print(type(ax))
# 输出类似：<class 'matplotlib.axes._subplots.AxesSubplot'>
```

这证实了`add_subplot`方法返回的是一个坐标轴对象。这就是它的第一步核心功能：**向现有图形添加坐标轴**。

![](img/5f2ab3c17c0f8a480827c5087baec5a2_18.png)

---

## 解读参数：`111` 代表什么？

![](img/5f2ab3c17c0f8a480827c5087baec5a2_20.png)

理解了`add_subplot`的基本作用后，我们面临最核心的问题：参数`111`究竟是什么意思？这确实容易让人困惑。

`add_subplot`的参数用于定义子图的网格布局和选择激活哪个位置。它主要有两种调用方式：

![](img/5f2ab3c17c0f8a480827c5087baec5a2_22.png)

1.  **三个独立参数**：`add_subplot(nrows, ncols, index)`
    *   `nrows`: 子图网格的行数。
    *   `ncols`: 子图网格的列数。
    *   `index`: 当前要创建的坐标轴在网格中的位置索引（从1开始，按行从左到右计数）。

2.  **一个三位整数参数**：`add_subplot(111)` 或 `add_subplot(234)`
    *   这是将上述三个数字连写在一起。**第一位是行数，第二位是列数，第三位是索引**。

因此，`fig.add_subplot(111)` 等价于 `fig.add_subplot(1, 1, 1)`。

它的意思是：**创建一个1行1列的子图网格（也就是只有一个图），并在这个唯一的位置（索引1）创建坐标轴**。所以，它本质上就是创建一个单图。

---

![](img/5f2ab3c17c0f8a480827c5087baec5a2_24.png)

![](img/5f2ab3c17c0f8a480827c5087baec5a2_25.png)

## 更复杂的布局示例

为了加深理解，我们来看一个更复杂的例子。假设我们想在一个图形中创建多个子图，但只想先初始化其中的一个。

![](img/5f2ab3c17c0f8a480827c5087baec5a2_27.png)

![](img/5f2ab3c17c0f8a480827c5087baec5a2_28.png)

以下代码创建了一个2行3列（共6个位置）的网格布局，并在第2个位置（索引2）创建一个坐标轴。

```python
import matplotlib.pyplot as plt

![](img/5f2ab3c17c0f8a480827c5087baec5a2_30.png)

fig = plt.figure()
# 在2行3列的网格中，于索引2的位置创建坐标轴
ax1 = fig.add_subplot(2, 3, 2)
# 这等价于：ax1 = fig.add_subplot(232)

ax1.plot([0, 1, 2], [0, 1, 0])  # 在这个坐标轴上绘图
plt.show()
```

我们甚至可以继续添加其他子图：

![](img/5f2ab3c17c0f8a480827c5087baec5a2_32.png)

```python
import matplotlib.pyplot as plt

![](img/5f2ab3c17c0f8a480827c5087baec5a2_34.png)

fig = plt.figure()

![](img/5f2ab3c17c0f8a480827c5087baec5a2_36.png)

# 第一个图：单图
ax1 = fig.add_subplot(111)
ax1.plot([0, 1], [0, 1], label='Main Plot')

# 第二个图：添加到新的网格布局中（2行3列，索引3）
ax2 = fig.add_subplot(2, 3, 3)
ax2.plot([0, 1, 2], [2, 1, 2], label='Subplot 2')

![](img/5f2ab3c17c0f8a480827c5087baec5a2_38.png)

plt.legend()
plt.tight_layout() # 调整布局，防止标签重叠
plt.show()
```

![](img/5f2ab3c17c0f8a480827c5087baec5a2_40.png)

这段代码会在一个图形窗口中显示两个图表：一个占据整个窗口的单图，另一个则位于2x3网格布局的特定位置。

---

## 总结与建议

本节课中我们一起学习了`fig.add_subplot()`方法，特别是`fig.add_subplot(111)`这种写法的含义。

我们来总结一下关键点：
*   **核心功能**：`fig.add_subplot()`用于向现有图形（`Figure`）添加坐标轴（`Axes`）。
*   **参数`111`**：它是`add_subplot(1, 1, 1)`的简写，表示创建**一个1行1列网格中的第一个（也是唯一一个）子图**。
*   **参数通用规则**：三位整数`abc`分别代表**行数(a)、列数(b)、索引(c)**。

对于初学者，Matplotlib创建子图的方式有多种（如`plt.subplots()`），这可能会造成混淆。我的建议是：

**尽量选择一种你理解且习惯的方法，并在一段时间内坚持使用它。** 对于大多数情况，在脚本开头使用`plt.subplots()`函数一次性创建图形和所有坐标轴数组是更清晰、更推荐的方式。`add_subplot`则在需要动态或按条件向已有图形添加坐标轴时非常有用。

![](img/5f2ab3c17c0f8a480827c5087baec5a2_42.png)

希望本教程能帮助你澄清这个常见的疑问！