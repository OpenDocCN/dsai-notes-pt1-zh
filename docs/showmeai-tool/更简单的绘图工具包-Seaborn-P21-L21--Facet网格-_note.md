# Seaborn 绘图教程，P21：L21 - Facet网格 🧩

![](img/8d5951280297357bd612b9daf49c99c3_1.png)

在本节课中，我们将要学习 Seaborn 库中一个非常强大的功能——**FacetGrid**。FacetGrid 允许你基于数据的一个或多个分类变量，在一个网格中创建多个子图，从而方便地进行数据对比和可视化分析。

## 概述

![](img/8d5951280297357bd612b9daf49c99c3_3.png)

FacetGrid 的核心思想是“分面绘图”。你可以指定数据框、行变量和列变量，Seaborn 会自动根据这些变量将数据分割到不同的子图中。这非常适合探索数据在不同类别下的分布模式。

![](img/8d5951280297357bd612b9daf49c99c3_5.png)

## 创建基础 FacetGrid

![](img/8d5951280297357bd612b9daf49c99c3_7.png)

上一节我们介绍了 FacetGrid 的概念，本节中我们来看看如何创建一个基础的网格。

![](img/8d5951280297357bd612b9daf49c99c3_8.png)

首先，我们需要导入必要的库并加载示例数据集 `tips`。

```python
import seaborn as sns
import matplotlib.pyplot as plt

# 加载数据集
tips = sns.load_dataset('tips')
```

![](img/8d5951280297357bd612b9daf49c99c3_10.png)

以下是创建一个基础 FacetGrid 的步骤：

![](img/8d5951280297357bd612b9daf49c99c3_11.png)

1.  初始化 `FacetGrid` 对象，传入数据框、行变量和列变量。
2.  使用 `.map()` 方法将绘图函数（如直方图、散点图）应用到网格的每个子图上。

![](img/8d5951280297357bd612b9daf49c99c3_13.png)

例如，我们想查看午餐和晚餐时段，吸烟者与非吸烟者的小费金额分布：

![](img/8d5951280297357bd612b9daf49c99c3_15.png)

```python
# 创建 FacetGrid，列基于‘time’，行基于‘smoker’
g = sns.FacetGrid(tips, col='time', row='smoker')
# 在每个子图上绘制‘total_bill’的直方图
g.map(plt.hist, 'total_bill', bins=8)
plt.show()
```

这段代码会生成一个 2x2 的网格图，清晰地展示了不同组合下总账单的分布情况。

## 自定义网格外观

创建了基础网格后，我们还可以对其进行各种自定义，使其更符合我们的需求。

![](img/8d5951280297357bd612b9daf49c99c3_17.png)

![](img/8d5951280297357bd612b9daf49c99c3_18.png)

以下是几个常用的自定义选项：

![](img/8d5951280297357bd612b9daf49c99c3_19.png)

*   **调整尺寸**：通过 `height` 和 `aspect` 参数控制每个子图的高度和宽高比。
*   **设置调色板**：使用 `palette` 参数为基于色调（hue）的变量分配颜色。
*   **控制顺序**：使用 `col_order` 或 `row_order` 参数指定列或行的显示顺序。
*   **传递绘图参数**：通过字典将参数（如点的大小、颜色）传递给底层的绘图函数。

让我们创建一个更复杂的例子，使用色调并自定义样式：

![](img/8d5951280297357bd612b9daf49c99c3_21.png)

```python
# 创建 FacetGrid，列基于‘sex’，色调基于‘smoker’
g = sns.FacetGrid(tips, col='sex', hue='smoker', height=4, aspect=1.3)

![](img/8d5951280297357bd612b9daf49c99c3_22.png)

# 定义标记样式字典
marker_dict = {'Yes': '^', 'No': 'v'}  # 吸烟者用上三角，非吸烟者用下三角
scatter_kws = {'s': 50, 'linewidth': 0.5, 'edgecolor': 'white'}

# 绘制散点图，并传入自定义参数
g.map(plt.scatter, 'total_bill', 'tip', **scatter_kws)

![](img/8d5951280297357bd612b9daf49c99c3_24.png)

# 添加图例
g.add_legend()
plt.show()
```

![](img/8d5951280297357bd612b9daf49c99c3_25.png)

## 高级应用：包装列与自定义绘图

FacetGrid 的功能非常灵活，可以处理更复杂的布局。

![](img/8d5951280297357bd612b9daf49c99c3_27.png)

![](img/8d5951280297357bd612b9daf49c99c3_28.png)

例如，当分类变量有很多类别时，我们可以使用 `col_wrap` 参数让列自动换行。我们还可以使用 `.map()` 方法应用任何 Matplotlib 绘图函数。

![](img/8d5951280297357bd612b9daf49c99c3_29.png)

以下是一个展示学生注意力与分数关系的例子，我们为每个学生创建一个子图：

![](img/8d5951280297357bd612b9daf49c99c3_30.png)

```python
# 加载另一个数据集
attention = sns.load_dataset('attention')

# 创建 FacetGrid，按‘subject’分列，每行显示5个
g = sns.FacetGrid(attention, col='subject', col_wrap=5, height=1.5)

# 在每个子图上绘制折线图
g.map(plt.plot, 'solutions', 'score', marker='o')

![](img/8d5951280297357bd612b9daf49c99c3_32.png)

plt.show()
```

这段代码会生成一个多行多列的网格，每张图显示一个学生在不同解题方案下的得分趋势。

## 总结

![](img/8d5951280297357bd612b9daf49c99c3_34.png)

![](img/8d5951280297357bd612b9daf49c99c3_35.png)

本节课中我们一起学习了 Seaborn 的 **FacetGrid** 功能。我们掌握了如何：

![](img/8d5951280297357bd612b9daf49c99c3_36.png)

![](img/8d5951280297357bd612b9daf49c99c3_37.png)

1.  创建基础的网格图来对比不同数据子集。
2.  使用 `height`、`aspect`、`palette` 等参数自定义网格外观。
3.  通过 `col_wrap` 处理多类别变量，并应用自定义的绘图函数和样式。

![](img/8d5951280297357bd612b9daf49c99c3_38.png)

FacetGrid 是进行多变量探索性数据分析的利器，它能帮助你快速发现数据中隐藏的模式和关系。通过组合不同的行、列和色调变量，你可以从多个维度审视你的数据。