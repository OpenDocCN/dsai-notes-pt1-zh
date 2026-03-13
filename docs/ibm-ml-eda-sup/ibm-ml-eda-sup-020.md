# 020：使用分组与Seaborn进行数据探索性分析 📊

在本节课中，我们将学习如何使用Pandas的分组功能以及Seaborn库进行数据可视化，以深入探索数据集的特征和关系。我们将重点介绍分组聚合绘图、Seaborn的配对图、六边形箱图和分面网格图。

---

## 使用Pandas分组进行绘图 📈

![](img/3a25a47d97aae41bee0a696991de87a5_1.png)

![](img/3a25a47d97aae41bee0a696991de87a5_2.png)

![](img/3a25a47d97aae41bee0a696991de87a5_3.png)

上一节我们介绍了数据的基本统计。本节中，我们来看看如何根据类别对数据进行分组并可视化。

![](img/3a25a47d97aae41bee0a696991de87a5_5.png)

首先，我们从Pandas DataFrame开始，根据目标列（例如物种）进行分组，并计算每个特征（如花萼长度、花萼宽度等）的平均值。

![](img/3a25a47d97aae41bee0a696991de87a5_6.png)

![](img/3a25a47d97aae41bee0a696991de87a5_8.png)

以下是创建分组对象并计算均值的代码：

```python
grouped_df = df.groupby('target').mean()
```

![](img/3a25a47d97aae41bee0a696991de87a5_10.png)

![](img/3a25a47d97aae41bee0a696991de87a5_11.png)

![](img/3a25a47d97aae41bee0a696991de87a5_12.png)

接着，我们调用`.plot`方法进行绘图。默认的绘图类型是折线图。我们可以通过设置`linestyle=''`来移除线条。为了区分不同的特征，我们为每个特征指定颜色，例如红色、蓝色、黑色和绿色。

![](img/3a25a47d97aae41bee0a696991de87a5_13.png)

以下是设置图表参数的代码：

![](img/3a25a47d97aae41bee0a696991de87a5_15.png)

![](img/3a25a47d97aae41bee0a696991de87a5_16.png)

```python
grouped_df.plot(figsize=(4, 4), fontsize=10, color=['red', 'blue', 'black', 'green'])
```

生成的图表以不同物种为X轴，显示了每个特征的平均值。例如，红色线条代表花萼长度的平均值，蓝色线条代表花萼宽度的平均值。

![](img/3a25a47d97aae41bee0a696991de87a5_18.png)

![](img/3a25a47d97aae41bee0a696991de87a5_20.png)

---

## 使用Seaborn绘制配对图 🔄

![](img/3a25a47d97aae41bee0a696991de87a5_22.png)

现在，我们转向Seaborn库，它提供了更高级和美观的统计图形。一个非常有用的功能是`pairplot`（配对图）。

![](img/3a25a47d97aae41bee0a696991de87a5_23.png)

![](img/3a25a47d97aae41bee0a696991de87a5_24.png)

首先需要导入Seaborn库：

![](img/3a25a47d97aae41bee0a696991de87a5_26.png)

```python
import seaborn as sns
```

创建配对图非常简单，只需传入数据框即可。我们还可以通过`hue`参数根据类别（如物种）用颜色区分数据点，并通过`size`参数设置图形大小。

![](img/3a25a47d97aae41bee0a696991de87a5_28.png)

以下是绘制配对图的代码：

![](img/3a25a47d97aae41bee0a696991de87a5_29.png)

![](img/3a25a47d97aae41bee0a696991de87a5_30.png)

![](img/3a25a47d97aae41bee0a696991de87a5_31.png)

```python
sns.pairplot(data=df, hue='species', height=4)
```

配对图会为数据框中每对特征生成一个散点图矩阵。对角线位置显示的是单个特征的分布直方图。通过设置`hue='species'`，我们可以清晰地看到不同物种数据点的分布和关系。

![](img/3a25a47d97aae41bee0a696991de87a5_33.png)

![](img/3a25a47d97aae41bee0a696991de87a5_34.png)

![](img/3a25a47d97aae41bee0a696991de87a5_35.png)

例如，在业务场景中，我们可以用它来查看广告支出与收入之间的关系，以及各自的数据分布。

![](img/3a25a47d97aae41bee0a696991de87a5_37.png)

---

## 使用Seaborn绘制六边形箱图 🔍

Seaborn的另一个实用功能是`jointplot`，特别是其`kind='hex'`模式，可以生成六边形箱图。

六边形箱图类似于散点图，但它通过六边形的颜色深浅来表示数据点的密度。颜色越深的区域表示数据点越密集。

![](img/3a25a47d97aae41bee0a696991de87a5_39.png)

![](img/3a25a47d97aae41bee0a696991de87a5_40.png)

![](img/3a25a47d97aae41bee0a696991de87a5_41.png)

以下是绘制六边形箱图的代码：

![](img/3a25a47d97aae41bee0a696991de87a5_42.png)

![](img/3a25a47d97aae41bee0a696991de87a5_43.png)

![](img/3a25a47d97aae41bee0a696991de87a5_44.png)

![](img/3a25a47d97aae41bee0a696991de87a5_45.png)

```python
sns.jointplot(x='sepal_length', y='sepal_width', data=df, kind='hex')
```

![](img/3a25a47d97aae41bee0a696991de87a5_46.png)

该图表不仅展示了两个特征之间的关系，还在顶部和右侧分别附上了每个特征的直方图。这有助于我们理解数据的集中趋势和分布情况。

![](img/3a25a47d97aae41bee0a696991de87a5_48.png)

![](img/3a25a47d97aae41bee0a696991de87a5_49.png)

![](img/3a25a47d97aae41bee0a696991de87a5_50.png)

同样，在分析广告与收入关系时，这种图能有效展示两者重叠最多的区域。

---

![](img/3a25a47d97aae41bee0a696991de87a5_52.png)

![](img/3a25a47d97aae41bee0a696991de87a5_53.png)

## 使用分面网格进行多类别分析 🧩

![](img/3a25a47d97aae41bee0a696991de87a5_55.png)

![](img/3a25a47d97aae41bee0a696991de87a5_56.png)

最后，如果我们希望根据不同类别完全分离并对比数据，可以使用Seaborn的`FacetGrid`（分面网格）功能。

这类似于执行分组操作，但以并排图表的形式呈现。我们首先创建一个`FacetGrid`对象，指定按哪一列进行分面。

![](img/3a25a47d97aae41bee0a696991de87a5_58.png)

![](img/3a25a47d97aae41bee0a696991de87a5_59.png)

以下是创建分面网格并绘制直方图的代码：

![](img/3a25a47d97aae41bee0a696991de87a5_60.png)

![](img/3a25a47d97aae41bee0a696991de87a5_61.png)

```python
# 创建分面网格对象，按‘species’列分面
g = sns.FacetGrid(df, col='species', margin_titles=True)

![](img/3a25a47d97aae41bee0a696991de87a5_63.png)

# 在每个分面上映射花萼宽度的直方图
g.map(plt.hist, 'sepal_width')

![](img/3a25a47d97aae41bee0a696991de87a5_64.png)

![](img/3a25a47d97aae41bee0a696991de87a5_66.png)

# 创建另一个分面网格，绘制花萼长度的直方图
g2 = sns.FacetGrid(df, col='species')
g2.map(plt.hist, 'sepal_length')
```

![](img/3a25a47d97aae41bee0a696991de87a5_67.png)

通过这种方式，我们可以为每个物种单独生成花萼宽度和花萼长度的直方图，从而直观地进行跨类别比较。

![](img/3a25a47d97aae41bee0a696991de87a5_69.png)

---

![](img/3a25a47d97aae41bee0a696991de87a5_71.png)

![](img/3a25a47d97aae41bee0a696991de87a5_72.png)

## 总结 ✨

![](img/3a25a47d97aae41bee0a696991de87a5_74.png)

![](img/3a25a47d97aae41bee0a696991de87a5_75.png)

本节课中我们一起学习了探索性数据分析（EDA）中的几种高级可视化技术：

1.  **分组聚合绘图**：使用Pandas的`groupby`和`plot`功能，可视化不同类别下特征的平均值。
2.  **Seaborn配对图**：利用`sns.pairplot`快速审视数据集中所有特征对之间的关系和分布。
3.  **六边形箱图**：通过`sns.jointplot`的`kind='hex'`参数，可视化两个连续变量之间数据点的密度。
4.  **分面网格图**：使用`sns.FacetGrid`将数据按类别拆分，并在并排的子图中进行对比分析。

![](img/3a25a47d97aae41bee0a696991de87a5_77.png)

![](img/3a25a47d97aae41bee0a696991de87a5_78.png)

![](img/3a25a47d97aae41bee0a696991de87a5_79.png)

![](img/3a25a47d97aae41bee0a696991de87a5_80.png)

这些技术能帮助我们更好地理解数据特征、发现模式，并为后续的机器学习建模打下坚实基础。接下来，我们将在Jupyter Notebook实验课中实际操作这些方法。