# 绘图必备Matplotlib，P9：在Matplotlib中使用子图 📊

![](img/558551509e8aea69421f10f8e194eb25_2.png)

在本节课中，我们将学习如何在Matplotlib中创建和排列多个子图。子图功能允许您在一个图形窗口中并排显示多个图表，这对于数据对比和展示非常有用。我们将从基础概念开始，逐步学习如何创建子图、在不同子图中绘制不同类型的图表，以及如何调整布局以避免元素重叠。

## 概述

子图是Matplotlib中组织多个坐标轴（Axes）的主要方式。通过`plt.subplots()`函数，我们可以轻松地创建一个包含指定行数和列数的图形网格。每个网格单元都是一个独立的坐标轴对象，我们可以在上面绘制不同的数据。

![](img/558551509e8aea69421f10f8e194eb25_9.png)

## 创建基础子图

通常，我们使用`plt.subplots()`函数来创建子图。该函数返回一个图形（Figure）对象和一个坐标轴（Axes）对象或数组。

以下是创建一个单一坐标轴的基础方法：

```python
fig, ax = plt.subplots()
```

这行代码创建了一个包含单个坐标轴的图形。变量`fig`代表整个图形窗口，而`ax`代表我们可以在上面绘图的坐标轴。

## 创建多子图网格

![](img/558551509e8aea69421f10f8e194eb25_15.png)

![](img/558551509e8aea69421f10f8e194eb25_17.png)

要创建包含多个子图的网格，我们需要向`plt.subplots()`函数传入行数和列数。

![](img/558551509e8aea69421f10f8e194eb25_19.png)

例如，创建一个2行2列（共4个子图）的网格：

![](img/558551509e8aea69421f10f8e194eb25_21.png)

```python
fig, axs = plt.subplots(nrows=2, ncols=2)
```

![](img/558551509e8aea69421f10f8e194eb25_23.png)

执行这行代码后，`axs`变量将变成一个二维的NumPy数组，其形状为(2, 2)，对应我们请求的2x2网格。数组中的每个元素都是一个Matplotlib坐标轴对象。

![](img/558551509e8aea69421f10f8e194eb25_25.png)

我们可以通过索引来访问特定的子图。例如，`axs[0, 0]`对应第一行第一列的子图，`axs[0, 1]`对应第一行第二列的子图，依此类推。

![](img/558551509e8aea69421f10f8e194eb25_27.png)

## 在子图中绘制图表

![](img/558551509e8aea69421f10f8e194eb25_29.png)

![](img/558551509e8aea69421f10f8e194eb25_30.png)

创建子图网格后，我们可以在每个坐标轴对象上独立绘图，就像操作单个坐标轴一样。

![](img/558551509e8aea69421f10f8e194eb25_32.png)

![](img/558551509e8aea69421f10f8e194eb25_33.png)

以下是在不同子图中绘制不同类型图表的示例。假设我们有一个名为`df`的DataFrame，其中包含‘age’、‘chol’（胆固醇）、‘trestbps’（静息血压）、‘thalach’（最大心率）和‘oldpeak’等列。

![](img/558551509e8aea69421f10f8e194eb25_35.png)

![](img/558551509e8aea69421f10f8e194eb25_37.png)

首先，我们按年龄对数据进行分组并计算某些指标的平均值：

![](img/558551509e8aea69421f10f8e194eb25_39.png)

```python
age_group = df.groupby('age').mean()
```

![](img/558551509e8aea69421f10f8e194eb25_41.png)

![](img/558551509e8aea69421f10f8e194eb25_43.png)

现在，我们在2x2网格的每个子图中绘制不同的图表：

![](img/558551509e8aea69421f10f8e194eb25_45.png)

1.  在第一行第一列（`axs[0, 0]`）绘制平均静息血压的条形图。
2.  在第一行第二列（`axs[0, 1]`）绘制平均胆固醇的折线图。
3.  在第二行第一列（`axs[1, 0]`）绘制最大心率与运动引起的ST段抑制（oldpeak）的散点图。
4.  在第二行第二列（`axs[1, 1]`）绘制平均ST段抑制（oldpeak）的条形图。

![](img/558551509e8aea69421f10f8e194eb25_47.png)

![](img/558551509e8aea69421f10f8e194eb25_49.png)

对应的绘图代码如下：

![](img/558551509e8aea69421f10f8e194eb25_51.png)

![](img/558551509e8aea69421f10f8e194eb25_53.png)

```python
# 子图1：条形图 (平均静息血压)
axs[0, 0].bar(age_group.index, age_group['trestbps'])
axs[0, 0].set_xlabel('Age')
axs[0, 0].set_ylabel('Average Resting BP')
axs[0, 0].set_title('Average Resting Blood Pressure by Age')

![](img/558551509e8aea69421f10f8e194eb25_55.png)

# 子图2：折线图 (平均胆固醇)
axs[0, 1].plot(age_group.index, age_group['chol'])
axs[0, 1].set_xlabel('Age')
axs[0, 1].set_ylabel('Average Cholesterol')
axs[0, 1].set_title('Average Cholesterol by Age')

![](img/558551509e8aea69421f10f8e194eb25_56.png)

![](img/558551509e8aea69421f10f8e194eb25_57.png)

![](img/558551509e8aea69421f10f8e194eb25_58.png)

# 子图3：散点图 (最大心率 vs ST段抑制)
axs[1, 0].scatter(df['thalach'], df['oldpeak'])
axs[1, 0].set_xlabel('Max Heart Rate Achieved')
axs[1, 0].set_ylabel('ST Depression Induced by Exercise')
axs[1, 0].set_title('Max HR vs. ST Depression')

![](img/558551509e8aea69421f10f8e194eb25_60.png)

# 子图4：条形图 (平均ST段抑制)
axs[1, 1].bar(age_group.index, age_group['oldpeak'])
axs[1, 1].set_xlabel('Age')
axs[1, 1].set_ylabel('Average Oldpeak')
axs[1, 1].set_title('Average ST Depression by Age')
```

![](img/558551509e8aea69421f10f8e194eb25_61.png)

![](img/558551509e8aea69421f10f8e194eb25_63.png)

通过在每个坐标轴对象上调用`.set_xlabel()`, `.set_ylabel()`, `.set_title()`等方法，我们可以为每个子图单独设置坐标轴标签和标题。

![](img/558551509e8aea69421f10f8e194eb25_65.png)

![](img/558551509e8aea69421f10f8e194eb25_67.png)

## 调整图形大小和布局

![](img/558551509e8aea69421f10f8e194eb25_69.png)

当子图数量较多时，默认的图形尺寸可能显得拥挤。我们可以通过`figsize`参数在创建图形时调整其大小。

![](img/558551509e8aea69421f10f8e194eb25_71.png)

![](img/558551509e8aea69421f10f8e194eb25_72.png)

```python
fig, axs = plt.subplots(nrows=2, ncols=2, figsize=(12, 8))
```

![](img/558551509e8aea69421f10f8e194eb25_74.png)

![](img/558551509e8aea69421f10f8e194eb25_76.png)

参数`figsize`接受一个元组，表示图形的宽度和高度（单位为英寸）。较大的图形可以提供更多空间来清晰展示每个子图。

![](img/558551509e8aea69421f10f8e194eb25_78.png)

即使调整了大小，子图的标签、标题等元素有时仍可能重叠。Matplotlib提供了一个便捷的函数`plt.tight_layout()`来自动调整子图参数，使它们能够很好地适应图形区域。

![](img/558551509e8aea69421f10f8e194eb25_80.png)

![](img/558551509e8aea69421f10f8e194eb25_81.png)

在调用`plt.show()`显示图形之前，添加以下代码：

![](img/558551509e8aea69421f10f8e194eb25_83.png)

```python
plt.tight_layout()
plt.show()
```

![](img/558551509e8aea69421f10f8e194eb25_85.png)

`plt.tight_layout()`会尝试智能地重新排列子图周围的间距，尽量减少重叠。虽然它并非万能，但在大多数情况下能显著改善布局效果。如果自动调整后仍有问题，则可能需要手动调整子图的位置或边距。

![](img/558551509e8aea69421f10f8e194eb25_87.png)

## 总结

![](img/558551509e8aea69421f10f8e194eb25_88.png)

![](img/558551509e8aea69421f10f8e194eb25_89.png)

本节课中我们一起学习了Matplotlib子图的核心功能。我们了解了如何使用`plt.subplots()`函数创建单个或多个子图网格，并通过索引在特定坐标轴上绘制不同类型的图表。同时，我们也掌握了如何为每个子图设置独立的标签和标题，以及如何通过调整图形大小和使用`plt.tight_layout()`来优化整体布局，确保图表清晰易读。子图是进行多数据可视化对比的强大工具，熟练掌握它将极大提升您的数据展示能力。