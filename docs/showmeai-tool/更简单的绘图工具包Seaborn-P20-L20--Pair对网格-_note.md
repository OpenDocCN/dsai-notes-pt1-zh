# 更简单的绘图工具包Seaborn，P20：L20 - Pair对网格 📊

在本节课中，我们将要学习Seaborn库中的`PairGrid`功能。`PairGrid`提供了比`pairplot`更精细的控制能力，允许你自定义网格中不同位置的图表类型，从而创建高度定制化的多变量关系可视化。

## 概述

![](img/fac7241cd51289d163c52ef517203e4f_2.png)

上一节我们介绍了`pairplot`，它可以快速生成数据集中所有数值变量两两关系的图表矩阵。本节中我们来看看`PairGrid`，它允许你为网格的不同区域（如上三角、下三角、对角线）指定不同的绘图函数，实现更灵活的图表组合。

![](img/fac7241cd51289d163c52ef517203e4f_3.png)

## 创建基础PairGrid

首先，我们需要导入必要的库并加载示例数据。

```python
import seaborn as sns
import matplotlib.pyplot as plt

# 加载鸢尾花数据集
iris = sns.load_dataset('iris')
```

接下来，我们创建一个基础的`PairGrid`对象。我们将根据`species`（物种）列对数据进行着色。

![](img/fac7241cd51289d163c52ef517203e4f_5.png)

![](img/fac7241cd51289d163c52ef517203e4f_6.png)

```python
# 创建一个PairGrid对象，根据'species'着色
g = sns.PairGrid(iris, hue='species')
```

![](img/fac7241cd51289d163c52ef517203e4f_7.png)

创建网格后，我们需要指定在哪些位置绘制什么类型的图表。以下是在网格的上三角、下三角和对角线位置都绘制散点图的方法。

![](img/fac7241cd51289d163c52ef517203e4f_8.png)

```python
# 在网格的所有位置（上、下、对角线）绘制散点图
g.map(plt.scatter)
```

![](img/fac7241cd51289d163c52ef517203e4f_9.png)

运行上述代码，你将看到一个布满散点图的矩阵。这些图表展示了数据框中各数值列之间的交互和相关性。

## 自定义不同位置的图表

`PairGrid`的强大之处在于可以分别为网格的不同区域指定图表。假设我们想在对角线上绘制直方图，而在上三角和下三角绘制散点图。

以下是实现这一目标的步骤：

1.  首先，在对角线位置绘制直方图。
2.  然后，在非对角线位置（即上三角和下三角）绘制散点图。

![](img/fac7241cd51289d163c52ef517203e4f_12.png)

```python
# 创建一个PairGrid对象
g = sns.PairGrid(iris, hue='species')

![](img/fac7241cd51289d163c52ef517203e4f_13.png)

# 在对角线位置绘制直方图
g.map_diag(plt.hist)

![](img/fac7241cd51289d163c52ef517203e4f_14.png)

# 在非对角线位置（上三角和下三角）绘制散点图
g.map_offdiag(plt.scatter)
```

![](img/fac7241cd51289d163c52ef517203e4f_15.png)

现在，网格的对角线显示的是每个变量的分布直方图，而其他位置则保留了展示变量间关系的散点图。

## 为上三角和下三角指定不同图表

![](img/fac7241cd51289d163c52ef517203e4f_17.png)

![](img/fac7241cd51289d163c52ef517203e4f_18.png)

我们还可以进行更细致的控制，例如在上三角和下三角分别绘制不同类型的图表。以下是在上三角绘制散点图，在下三角绘制核密度估计图（KDE图）的示例。

```python
# 创建一个PairGrid对象
g = sns.PairGrid(iris, hue='species')

# 在对角线绘制直方图
g.map_diag(plt.hist)

# 在上三角绘制散点图
g.map_upper(plt.scatter)

# 在下三角绘制KDE图
g.map_lower(sns.kdeplot)
```

![](img/fac7241cd51289d163c52ef517203e4f_20.png)

通过组合`map_diag`、`map_upper`和`map_lower`方法，你可以轻松构建出信息丰富且外观专业的可视化图表。

![](img/fac7241cd51289d163c52ef517203e4f_21.png)

## 使用PairGrid绘制指定变量

![](img/fac7241cd51289d163c52ef517203e4f_22.png)

![](img/fac7241cd51289d163c52ef517203e4f_23.png)

有时，你可能不想绘制数据集中所有的数值变量，而只想关注其中几个特定变量之间的关系。`PairGrid`也支持这种操作。

以下示例展示了如何创建一个只包含`sepal_length`（萼片长度）、`sepal_width`（萼片宽度）、`petal_length`（花瓣长度）和`petal_width`（花瓣宽度）这四个变量的自定义网格。

```python
# 定义要使用的变量
vars = ['sepal_length', 'sepal_width', 'petal_length', 'petal_width']

# 创建PairGrid，指定x_vars和y_vars
g = sns.PairGrid(iris, hue='species', x_vars=vars, y_vars=vars)

# 在整个网格上绘制散点图
g.map(plt.scatter)

![](img/fac7241cd51289d163c52ef517203e4f_25.png)

# 添加图例
g.add_legend()
```

![](img/fac7241cd51289d163c52ef517203e4f_26.png)

通过设置`x_vars`和`y_vars`参数，你可以精确控制网格的行和列分别代表哪些变量，从而创建出完全符合你分析需求的图表矩阵。

![](img/fac7241cd51289d163c52ef517203e4f_27.png)

## 总结

![](img/fac7241cd51289d163c52ef517203e4f_28.png)

本节课中我们一起学习了Seaborn的`PairGrid`功能。我们了解到：

![](img/fac7241cd51289d163c52ef517203e4f_29.png)

*   `PairGrid`提供了比`pairplot`更强大的自定义能力。
*   可以使用`map_diag`、`map_upper`、`map_lower`和`map_offdiag`方法为网格的不同区域指定绘图函数。
*   可以通过`x_vars`和`y_vars`参数选择特定的变量来构建网格。
*   这种灵活性使得`PairGrid`成为创建复杂、多变量数据探索图表的理想工具。

![](img/fac7241cd51289d163c52ef517203e4f_30.png)

掌握`PairGrid`将帮助你更有效地揭示数据中隐藏的模式和关系。