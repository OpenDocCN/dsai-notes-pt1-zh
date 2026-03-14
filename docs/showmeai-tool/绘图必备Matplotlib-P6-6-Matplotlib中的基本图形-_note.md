# 绘图必备Matplotlib，P6：6）Matplotlib中的基本图形 📊

![](img/459a65c80f9f13fc0f4dccf9dd01c356_1.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_2.png)

在本节课中，我们将学习如何使用Matplotlib绘制几种基本图形。我们将从一个实际的数据集（心脏病数据集）出发，逐步探索散点图、折线图、柱状图和直方图的绘制方法。通过本课，你将掌握使用Matplotlib进行数据可视化的核心流程。

---

![](img/459a65c80f9f13fc0f4dccf9dd01c356_4.png)

## 加载我们的数据 📂

在开始绘图之前，我们需要先加载数据。我们将使用Pandas库来读取和处理数据。如果你对Pandas不熟悉，建议学习相关教程，但本课会提供足够的信息让你跟上。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_6.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_7.png)

首先，我们导入Pandas并加载数据集。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_9.png)

```python
import pandas as pd

![](img/459a65c80f9f13fc0f4dccf9dd01c356_11.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_13.png)

# 加载心脏病数据集
df = pd.read_csv('heart_disease_uci.csv')
```

![](img/459a65c80f9f13fc0f4dccf9dd01c356_15.png)

加载完成后，我们可以查看数据的基本信息，以了解其结构。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_16.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_18.png)

以下是查看数据的一些常用方法：

*   `df.head()`：查看数据的前五行。
*   `df.shape`：查看数据的行数和列数。
*   `df.describe()`：查看各列的描述性统计信息（如均值、标准差等）。
*   `df.info()`：查看数据集的整体信息，包括非空值数量和数据类型。

通过查看`df.describe()`，我们发现数据集的平均年龄约为54岁，心脏病患病率（`target`列的平均值）约为54%。

现在我们已经准备好数据，可以开始进行绘图了。

---

![](img/459a65c80f9f13fc0f4dccf9dd01c356_20.png)

## 基本图形 📈

![](img/459a65c80f9f13fc0f4dccf9dd01c356_22.png)

上一节我们成功加载了数据，本节中我们来看看如何使用Matplotlib绘制几种基本图形。绘制任何图形的通用公式是：
1.  创建图形和坐标轴：`fig, ax = plt.subplots()`
2.  在坐标轴上调用绘图方法：例如 `ax.scatter(x, y)`
3.  显示图形：`plt.show()`

我们将通过改变第二步中的方法来绘制不同类型的图形。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_24.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_25.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_26.png)

### 散点图

散点图用于展示两个连续变量之间的关系。例如，我们想探究年龄与胆固醇水平之间是否存在关联。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_28.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_30.png)

首先，我们准备数据。`x`是年龄列，`y`是胆固醇列。

```python
# 准备数据
x = df['age'].values
y = df['chol'].values
```

![](img/459a65c80f9f13fc0f4dccf9dd01c356_32.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_34.png)

然后，我们按照绘图公式创建散点图。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_36.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_38.png)

```python
import matplotlib.pyplot as plt

![](img/459a65c80f9f13fc0f4dccf9dd01c356_40.png)

fig, ax = plt.subplots()
ax.scatter(x, y)  # 调用scatter方法绘制散点图
plt.show()
```

![](img/459a65c80f9f13fc0f4dccf9dd01c356_42.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_44.png)

从生成的散点图初步观察，年龄与胆固醇之间没有非常强烈的线性关系。

有时，我们更关心按组（例如年龄）聚合后的趋势。我们可以先计算每个年龄的平均胆固醇水平，再绘制散点图。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_46.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_47.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_48.png)

```python
# 按年龄分组并计算平均值
df_grouped_by_age = df.groupby('age').mean()

![](img/459a65c80f9f13fc0f4dccf9dd01c356_50.png)

# 准备聚合后的数据
x_avg = df_grouped_by_age.index
y_avg = df_grouped_by_age['chol'].values

![](img/459a65c80f9f13fc0f4dccf9dd01c356_52.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_54.png)

# 绘制平均值的散点图
fig, ax = plt.subplots()
ax.scatter(x_avg, y_avg)
plt.show()
```

这张图显示出的正相关趋势比原始数据点更明显一些。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_56.png)

### 折线图

折线图常用于展示数据随时间或其他连续变量的变化趋势。如果我们想连接散点图中的点，观察其走势，就可以使用折线图。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_58.png)

我们使用与散点图相同的数据（聚合后的年龄与平均胆固醇），但调用不同的方法。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_60.png)

```python
fig, ax = plt.subplots()
ax.plot(x_avg, y_avg)  # 调用plot方法绘制折线图
plt.show()
```

![](img/459a65c80f9f13fc0f4dccf9dd01c356_62.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_64.png)

绘制线图只需将`ax.scatter()`替换为`ax.plot()`。需要注意的是，对于某些数据类型，用折线连接点是否合理需要根据实际情况判断。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_66.png)

### 柱状图

![](img/459a65c80f9f13fc0f4dccf9dd01c356_67.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_69.png)

柱状图非常适合比较不同类别之间的数值差异。例如，我们可以用柱状图来展示不同年龄段的平均胆固醇水平。

我们继续使用聚合后的数据，并调用`bar`方法。

```python
fig, ax = plt.subplots()
ax.bar(x_avg, y_avg)  # 调用bar方法绘制柱状图
plt.show()
```

在柱状图中，第一个参数是x轴（类别），第二个参数是y轴（数值）。

有时，我们可能需要绘制水平柱状图。这时需要调用`barh`方法，并且参数的顺序会发生变化：第一个参数变为y轴（类别），第二个参数变为x轴（数值）。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_71.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_73.png)

```python
fig, ax = plt.subplots()
ax.barh(x_avg, y_avg)  # 调用barh方法绘制水平柱状图
plt.show()
```

### 直方图

![](img/459a65c80f9f13fc0f4dccf9dd01c356_75.png)

直方图用于展示单个变量的分布情况，它将数据划分到多个“箱子”中，并显示每个箱子内数据点的数量。例如，我们想查看数据集中年龄的分布。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_77.png)

直方图只需要一个变量。我们调用`hist`方法。

```python
# 准备年龄数据
ages = df['age'].values

![](img/459a65c80f9f13fc0f4dccf9dd01c356_79.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_80.png)

fig, ax = plt.subplots()
ax.hist(ages, bins=20)  # 调用hist方法绘制直方图，bins参数指定箱子数量
plt.show()
```

![](img/459a65c80f9f13fc0f4dccf9dd01c356_82.png)

从直方图可以看出，数据集中的人数主要集中在两个年龄段。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_84.png)

如果你想自定义直方图（如改变箱子数量），可以在方法中传入相应的参数。在Jupyter Notebook中，你可以在方法名后加`?`并运行单元格来查看所有可用参数和说明。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_86.png)

---

![](img/459a65c80f9f13fc0f4dccf9dd01c356_88.png)

## 组合使用与总结 🎯

![](img/459a65c80f9f13fc0f4dccf9dd01c356_90.png)

我们之前学习了如何创建包含多个子图的图形。在这些子图中，你可以自由组合不同类型的图表，只需在每个坐标轴对象上调用不同的绘图方法即可。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_92.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_93.png)

本节课中我们一起学习了Matplotlib中五种基本图形的绘制方法：
1.  **散点图 (`ax.scatter()`)**: 用于研究两个变量之间的关系。
2.  **折线图 (`ax.plot()`)**: 用于展示数据的变化趋势。
3.  **柱状图 (`ax.bar()`)**: 用于比较不同类别的数值。
4.  **水平柱状图 (`ax.barh()`)**: 柱状图的水平版本。
5.  **直方图 (`ax.hist()`)**: 用于展示单个变量的分布。

![](img/459a65c80f9f13fc0f4dccf9dd01c356_94.png)

![](img/459a65c80f9f13fc0f4dccf9dd01c356_96.png)

掌握这些基本图形后，你已经能够应对许多数据可视化需求。Matplotlib的功能非常强大，每种绘图方法都有丰富的参数可供调整，以定制图表的外观。接下来的课程，我们将学习如何为图表添加颜色、标签、图例等元素，让图表更加美观和信息丰富。