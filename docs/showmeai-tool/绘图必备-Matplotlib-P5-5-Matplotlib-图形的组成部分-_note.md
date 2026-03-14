# 绘图必备 Matplotlib，P5：5）Matplotlib 图形的组成部分 🧩

在本节课中，我们将要学习 Matplotlib 图形的核心组成部分。理解这些概念是创建和定制图表的基础。我们将从创建数据开始，逐步构建图形，并详细解释图形（Figure）、坐标轴（Axes）等关键元素。

## 第一部分：创建数据与基础绘图

![](img/77d641e47cdae93cdad4881dd7e907b0_1.png)

为了清晰地展示绘图过程，我们首先需要创建一些数据。我们将使用 NumPy 库来生成数据，因为 Matplotlib 通常与 NumPy 数组配合良好。

![](img/77d641e47cdae93cdad4881dd7e907b0_3.png)

```python
import numpy as np
import matplotlib.pyplot as plt

![](img/77d641e47cdae93cdad4881dd7e907b0_5.png)

# 创建 x 数据：从 -3 到 3 的整数数组
x = np.array([-3, -2, -1, 0, 1, 2, 3])
# 创建 y 数据：x 的平方
y = x ** 2
```

![](img/77d641e47cdae93cdad4881dd7e907b0_7.png)

运行上述代码后，`x` 是一个包含 [-3, -2, -1, 0, 1, 2, 3] 的数组，`y` 则是 [9, 4, 1, 0, 1, 4, 9]。

### 你的绘图公式 📝

![](img/77d641e47cdae93cdad4881dd7e907b0_9.png)

在 Matplotlib 中创建图表有一个推荐的基本公式。虽然存在其他方法，但初学者应始终遵循此结构，以确保代码清晰且功能完整。

![](img/77d641e47cdae93cdad4881dd7e907b0_11.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_12.png)

以下是绘图的基本公式：

```python
fig, ax = plt.subplots()  # 步骤1：创建图形和坐标轴
# 步骤2：在此处写下你的绘图代码
ax.plot(x, y)             # 例如，绘制 x 和 y
plt.show()                # 步骤3：显示图形
```

![](img/77d641e47cdae93cdad4881dd7e907b0_14.png)

让我们应用这个公式来绘制我们的第一个图表。

![](img/77d641e47cdae93cdad4881dd7e907b0_16.png)

```python
fig, ax = plt.subplots()
ax.plot(x, y)
plt.show()
```

运行代码后，我们得到了一个基础的抛物线图。图表可能看起来有些“锯齿状”，这是因为我们只提供了7个数据点。但这已经展示了绘图的核心流程：实例化图形和坐标轴，在坐标轴上绘图，最后显示结果。

![](img/77d641e47cdae93cdad4881dd7e907b0_18.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_20.png)

## 第二部分：理解图形的组成部分

![](img/77d641e47cdae93cdad4881dd7e907b0_22.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_24.png)

上一节我们介绍了绘图的基本流程，本节中我们来看看 Matplotlib 图形的具体组成部分。理解这些术语对于后续的图表定制至关重要。

![](img/77d641e47cdae93cdad4881dd7e907b0_26.png)

Matplotlib 图形主要包含两个核心对象：**图形（Figure）** 和 **坐标轴（Axes）**。

*   **图形（Figure）**：这是整个图表窗口或图像文件。无论图表中包含多少内容，整个画布就是一个 Figure 对象。你可以将其想象为一张空白的画纸。
*   **坐标轴（Axes）**：这是实际进行绘图的区域，包含了 x 轴、y 轴、刻度、标签以及绘制的线条、点等。一个 Figure 中可以包含一个或多个 Axes 对象（即子图）。

为了更清晰地说明，我们可以创建一个包含多个子图的图形。

![](img/77d641e47cdae93cdad4881dd7e907b0_28.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_30.png)

```python
# 创建一个包含一行两列两个坐标轴的图形
fig, axs = plt.subplots(1, 2)
# 在第一个坐标轴（索引0）上绘制 x 和 y
axs[0].plot(x, y)
# 在第二个坐标轴（索引1）上绘制 x 的三次方
axs[1].plot(x, x**3)
plt.show()
```

![](img/77d641e47cdae93cdad4881dd7e907b0_32.png)

运行后，你会看到一个 Figure 中包含两个并排的 Axes。每个 Axes 都有自己独立的 x 轴和 y 轴。

我们可以进一步扩展，创建一个 2x2 的坐标轴网格。

![](img/77d641e47cdae93cdad4881dd7e907b0_34.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_35.png)

```python
# 创建一个包含两行两列四个坐标轴的图形
fig, axs = plt.subplots(2, 2)
# 在 (0,0) 位置的坐标轴上绘图
axs[0, 0].plot(x, y)
# 在 (0,1) 位置的坐标轴上绘图
axs[0, 1].plot(x, x**3)
# 在 (1,0) 位置的坐标轴上绘图
axs[1, 0].plot(x, np.exp(x))
# 在 (1,1) 位置的坐标轴上绘图
axs[1, 1].plot(x, np.log(x+4)) # 对 x+4 取对数，避免 log(0)
plt.show()
```

现在，一个 Figure 中包含了四个不同的 Axes，每个都绘制了不同的函数。这强调了 **所有绘图都发生在一个 Figure 内的一个或多个 Axes 对象上** 这一核心概念。

![](img/77d641e47cdae93cdad4881dd7e907b0_37.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_39.png)

## 第三部分：让图形更平滑

![](img/77d641e47cdae93cdad4881dd7e907b0_41.png)

我们之前绘制的抛物线看起来不平滑，这是因为我们只提供了7个离散的数据点。Matplotlib 只是用直线将这些点连接起来。

为了直观展示这一点，我们可以改用散点图来绘制这些点。

```python
fig, ax = plt.subplots()
ax.scatter(x, y) # 使用散点图
plt.show()
```

![](img/77d641e47cdae93cdad4881dd7e907b0_43.png)

散点图清楚地显示，我们只有7个孤立的点。要获得平滑的曲线，我们需要为函数提供更多、更密集的数据点。

![](img/77d641e47cdae93cdad4881dd7e907b0_45.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_47.png)

我们可以使用 NumPy 的 `arange` 函数来生成一个数值间隔更小的数组。

![](img/77d641e47cdae93cdad4881dd7e907b0_49.png)

```python
# 生成从-5到5，间隔为0.01的1000个数据点
x_dense = np.arange(-5, 5, 0.01)
y_dense = x_dense ** 2

![](img/77d641e47cdae93cdad4881dd7e907b0_51.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_53.png)

fig, ax = plt.subplots()
ax.plot(x_dense, y_dense)
plt.show()
```

![](img/77d641e47cdae93cdad4881dd7e907b0_55.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_56.png)

现在，图表变得非常平滑，因为我们在更小的区间内用直线连接了1000个点。数据点的密度决定了线条的平滑程度。

![](img/77d641e47cdae93cdad4881dd7e907b0_58.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_60.png)

让我们再绘制一个更复杂的平滑函数作为示例。

![](img/77d641e47cdae93cdad4881dd7e907b0_62.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_64.png)

```python
# 创建一个阻尼振荡的函数
y_osc = np.exp(-x_dense) * np.sin(10 * x_dense)

![](img/77d641e47cdae93cdad4881dd7e907b0_66.png)

fig, ax = plt.subplots()
ax.plot(x_dense, y_osc)
plt.show()
```

![](img/77d641e47cdae93cdad4881dd7e907b0_68.png)

这个图形看起来既美丽又平滑，这完全得益于我们使用了足够多的数据点来定义它。

## 总结

本节课中我们一起学习了 Matplotlib 图形的核心组成部分。

![](img/77d641e47cdae93cdad4881dd7e907b0_70.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_72.png)

1.  **绘图基础**：我们掌握了创建图表的基本公式：`fig, ax = plt.subplots()` -> 绘图代码 -> `plt.show()`。
2.  **核心概念**：我们明确了 **图形（Figure）** 是整个画布，而 **坐标轴（Axes）** 是画布上实际的绘图区域，一个 Figure 可以包含多个 Axes（子图）。
3.  **图形平滑度**：我们了解到图表的平滑度取决于输入数据点的数量。使用 `np.arange` 等工具生成密集数据点是获得平滑曲线的关键。

![](img/77d641e47cdae93cdad4881dd7e907b0_73.png)

![](img/77d641e47cdae93cdad4881dd7e907b0_75.png)

理解这些基础概念是有效使用 Matplotlib 进行数据可视化的第一步。在接下来的课程中，我们将学习如何在 Axes 上添加标题、标签、图例，以及如何定制线条样式和颜色。