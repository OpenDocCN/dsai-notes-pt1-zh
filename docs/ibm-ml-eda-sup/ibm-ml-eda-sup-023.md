# 023：数据探索分析（EDA）笔记本解决方案（第三部分）📊

![](img/5cc8b75f5bc60a543043d138a7852ff9_0.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_2.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_3.png)

在本节课中，我们将继续学习如何使用Python的Pandas和Matplotlib库进行数据探索分析（EDA）。我们将重点学习如何创建直方图和箱线图，以可视化数据集中不同特征的分布情况。

![](img/5cc8b75f5bc60a543043d138a7852ff9_5.png)

---

![](img/5cc8b75f5bc60a543043d138a7852ff9_7.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_8.png)

## 创建单个特征的直方图 📈

上一节我们介绍了数据的基本操作，本节中我们来看看如何可视化单个特征的分布。我们将为鸢尾花数据集中的“花瓣长度”特征创建一个直方图。

![](img/5cc8b75f5bc60a543043d138a7852ff9_10.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_11.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_12.png)

以下是使用Matplotlib的`Axes`对象创建直方图的步骤：

![](img/5cc8b75f5bc60a543043d138a7852ff9_14.png)

```python
import matplotlib.pyplot as plt

![](img/5cc8b75f5bc60a543043d138a7852ff9_16.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_17.png)

# 创建一个图形和坐标轴对象
fig, ax = plt.subplots()

![](img/5cc8b75f5bc60a543043d138a7852ff9_19.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_21.png)

# 在坐标轴上绘制‘petal_length’列的直方图，并指定箱数为25
ax.hist(data['petal_length'], bins=25)

# 设置坐标轴标签和标题
ax.set_xlabel('花瓣长度 (厘米)')
ax.set_ylabel('频数')
ax.set_title('花瓣长度分布直方图')

![](img/5cc8b75f5bc60a543043d138a7852ff9_23.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_24.png)

# 显示图形
plt.show()
```

![](img/5cc8b75f5bc60a543043d138a7852ff9_25.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_27.png)

---

![](img/5cc8b75f5bc60a543043d138a7852ff9_28.png)

## 使用Pandas快捷方法绘图 🚀

![](img/5cc8b75f5bc60a543043d138a7852ff9_30.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_31.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_32.png)

除了Matplotlib，Pandas也提供了非常便捷的绘图功能，可以简化代码。

以下是使用Pandas的`.plot.hist()`方法创建相同直方图的代码：

![](img/5cc8b75f5bc60a543043d138a7852ff9_34.png)

```python
# 直接使用Pandas Series的绘图方法
data['petal_length'].plot.hist(bins=25)

![](img/5cc8b75f5bc60a543043d138a7852ff9_35.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_36.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_37.png)

# 获取当前坐标轴并设置标签
plt.gca().set_xlabel('花瓣长度 (厘米)')
plt.gca().set_ylabel('频数')
plt.gca().set_title('花瓣长度分布直方图')

![](img/5cc8b75f5bc60a543043d138a7852ff9_39.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_40.png)

plt.show()
```

![](img/5cc8b75f5bc60a543043d138a7852ff9_42.png)

---

![](img/5cc8b75f5bc60a543043d138a7852ff9_44.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_45.png)

## 创建多个特征的叠加直方图 🎨

![](img/5cc8b75f5bc60a543043d138a7852ff9_47.png)

现在，我们来看看如何将四个特征（花瓣长度、花瓣宽度、萼片长度、萼片宽度）的直方图叠加绘制在同一张图上，以便于比较。

![](img/5cc8b75f5bc60a543043d138a7852ff9_49.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_50.png)

以下是实现方法：

![](img/5cc8b75f5bc60a543043d138a7852ff9_51.png)

```python
# 使用Pandas对整个DataFrame绘图，绘制所有数值列的直方图
data.plot.hist(alpha=0.5, bins=25)

![](img/5cc8b75f5bc60a543043d138a7852ff9_53.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_54.png)

# 设置X轴标签
plt.gca().set_xlabel('尺寸 (厘米)')
plt.show()
```

![](img/5cc8b75f5bc60a543043d138a7852ff9_56.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_57.png)

**代码解释**：
*   `data.plot.hist()`：对`data` DataFrame中的所有数值列绘制直方图。
*   `alpha=0.5`：设置透明度为0.5，使得叠加的直方图都能清晰可见。

![](img/5cc8b75f5bc60a543043d138a7852ff9_59.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_60.png)

---

![](img/5cc8b75f5bc60a543043d138a7852ff9_62.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_64.png)

## 创建子图：四个独立的直方图 🖼️

![](img/5cc8b75f5bc60a543043d138a7852ff9_66.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_68.png)

有时，我们更希望将每个特征的直方图单独显示在一个子图中。Pandas的`.hist()`方法可以轻松实现这一点。

以下是创建2x2子图布局的代码：

![](img/5cc8b75f5bc60a543043d138a7852ff9_70.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_72.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_74.png)

```python
# 调整图形大小以获得更好的显示效果
plt.figure(figsize=(8, 8))

# 为DataFrame中的每个数值列创建单独的直方图子图
axes_list = data.hist(bins=25, grid=False)

![](img/5cc8b75f5bc60a543043d138a7852ff9_76.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_77.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_78.png)

# 将返回的坐标轴对象数组展平为一维列表
axes_flat = axes_list.flatten()

![](img/5cc8b75f5bc60a543043d138a7852ff9_80.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_82.png)

# 遍历每个坐标轴，为其设置合适的标签
for ax in axes_flat:
    # 如果坐标轴位于最后一行，设置X轴标签
    if ax.is_last_row():
        ax.set_xlabel('尺寸 (厘米)')
    # 如果坐标轴位于第一列，设置Y轴标签
    if ax.is_first_col():
        ax.set_ylabel('频数')

![](img/5cc8b75f5bc60a543043d138a7852ff9_84.png)

plt.tight_layout() # 自动调整子图参数，使之填充整个图像区域
plt.show()
```

![](img/5cc8b75f5bc60a543043d138a7852ff9_86.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_87.png)

---

![](img/5cc8b75f5bc60a543043d138a7852ff9_89.png)

## 创建按类别分组的箱线图 📦

最后，我们学习如何创建箱线图来比较不同鸢尾花种类（`species`）在各个特征上的分布差异。

![](img/5cc8b75f5bc60a543043d138a7852ff9_91.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_92.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_93.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_94.png)

以下是使用Pandas创建分组箱线图的代码：

![](img/5cc8b75f5bc60a543043d138a7852ff9_96.png)

```python
# 调整图形大小
plt.figure(figsize=(10, 6))

![](img/5cc8b75f5bc60a543043d138a7852ff9_98.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_99.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_101.png)

# 按‘species’列分组，为其他数值列创建箱线图
data.boxplot(by='species')

![](img/5cc8b75f5bc60a543043d138a7852ff9_103.png)

plt.suptitle('') # 可选：移除自动生成的标题
plt.tight_layout()
plt.show()
```

![](img/5cc8b75f5bc60a543043d138a7852ff9_105.png)

**代码解释**：
*   `data.boxplot(by=‘species’)`：这个命令会为`data`中除`by`参数指定列之外的所有数值列创建箱线图。每个特征的箱线图都会按`species`的不同类别（Setosa, Versicolor, Virginica）进行分组显示。

![](img/5cc8b75f5bc60a543043d138a7852ff9_106.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_107.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_109.png)

---

![](img/5cc8b75f5bc60a543043d138a7852ff9_111.png)

## 总结 📝

![](img/5cc8b75f5bc60a543043d138a7852ff9_112.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_113.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_115.png)

本节课中我们一起学习了数据探索分析（EDA）中几种重要的可视化技巧：
1.  我们学习了使用Matplotlib和Pandas两种方法创建**单个特征的直方图**。
2.  我们掌握了如何绘制**多个特征的叠加直方图**，并使用`alpha`参数控制透明度以便观察。
3.  我们实践了使用`.hist()`方法快速创建**包含多个子图的图形**，并自动化设置坐标轴标签。
4.  最后，我们使用`.boxplot(by=‘column’)`语法轻松创建了**按类别分组的箱线图**，用于比较不同组别的数据分布。

![](img/5cc8b75f5bc60a543043d138a7852ff9_117.png)

![](img/5cc8b75f5bc60a543043d138a7852ff9_118.png)

这些可视化工具能帮助我们快速理解数据的分布、集中趋势和离散程度，是数据分析和机器学习项目准备阶段的关键步骤。