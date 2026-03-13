# 024：数据探索分析（EDA）实验解决方案（第四部分）📊

![](img/eb0ae3a78138de7d376260f7b17e5cc3_1.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_2.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_3.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_4.png)

在本节课中，我们将学习如何完成数据探索分析（EDA）实验的最后两个问题。我们将重点学习使用Seaborn库创建复杂的箱线图和配对图，以可视化数据集中不同特征与鸢尾花种类之间的关系。这些可视化技巧对于理解数据分布和特征间的相关性至关重要。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_5.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_6.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_7.png)

上一节我们介绍了数据清洗和基本统计，本节中我们来看看如何通过高级可视化来深入探索数据。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_9.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_10.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_11.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_12.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_13.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_14.png)

## 问题9：创建分组箱线图 📦

![](img/eb0ae3a78138de7d376260f7b17e5cc3_16.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_18.png)

我们需要创建一个箱线图，其中X轴按特征（如花萼长度、花萼宽度等）分隔，并且对于每个特征，再按鸢尾花的三个不同种类（Setosa, Versicolor, Virginica）细分为三个箱线图。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_20.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_21.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_23.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_25.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_27.png)

Seaborn的箱线图功能对此非常有用，但需要注意，**数据格式**必须为“长格式”而非“宽格式”。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_29.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_30.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_32.png)

**核心概念：长格式 vs 宽格式**
*   **宽格式**：每个特征（如花萼长度）是一列。
    ```python
    # 示例：每一行包含多个特征值
    | species | sepal_length | sepal_width | petal_length | petal_width |
    ```
*   **长格式**：每一行只包含一个观测值（一个特征的一个测量值）。
    ```python
    # 示例：每一行只包含一个特征值
    | species | measurement_type | measurement_value |
    ```

![](img/eb0ae3a78138de7d376260f7b17e5cc3_34.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_36.png)

Seaborn的`sns.boxplot()`函数在处理长格式数据时更为灵活。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_38.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_39.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_40.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_41.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_43.png)

### 数据重塑：从宽格式转为长格式

![](img/eb0ae3a78138de7d376260f7b17e5cc3_45.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_46.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_47.png)

以下是转换数据格式的步骤：

![](img/eb0ae3a78138de7d376260f7b17e5cc3_48.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_49.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_50.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_52.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_54.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_56.png)

首先，我们查看当前数据的结构。数据框的每一行包含一个样本的所有特征（花萼长度、花萼宽度等）。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_58.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_60.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_61.png)

为了转换格式，我们需要执行以下操作：

![](img/eb0ae3a78138de7d376260f7b17e5cc3_63.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_65.png)

1.  **设置索引**：将‘species’列设置为索引，确保它不会被视为需要堆叠的特征列。
    ```python
    data_indexed = data.set_index(‘species’)
    ```
2.  **堆叠数据**：使用`.stack()`方法将多个特征列“堆叠”成一列，从而创建多级索引。
    ```python
    data_stacked = data_indexed.stack()
    ```
3.  **重置索引**：将堆叠后形成的多级索引转换回单独的列。
    ```python
    data_long = data_stacked.reset_index()
    ```
4.  **重命名列**：为新的列赋予有意义的名称。
    ```python
    data_long = data_long.rename(columns={‘level_1’: ‘measurement’, 0: ‘size’})
    ```

![](img/eb0ae3a78138de7d376260f7b17e5cc3_67.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_68.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_69.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_70.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_71.png)

完成这些步骤后，数据框将从150行（每个样本一行）变为600行（每个样本的每个特征一行），符合Seaborn绘图所需的“长格式”。

### 使用Seaborn绘制分组箱线图

![](img/eb0ae3a78138de7d376260f7b17e5cc3_73.png)

数据准备就绪后，绘图过程相对简单。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_74.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_75.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_76.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_77.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_78.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_79.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_80.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_81.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_82.png)

以下是绘图代码的关键部分：
```python
import seaborn as sns
import matplotlib.pyplot as plt

![](img/eb0ae3a78138de7d376260f7b17e5cc3_84.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_86.png)

# 设置图形样式（可选，用于美化）
sns.set_style(“whitegrid”)
sns.set_context(“talk”)
plt.figure(figsize=(10, 6))

![](img/eb0ae3a78138de7d376260f7b17e5cc3_87.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_88.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_90.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_91.png)

# 绘制箱线图
# x: 测量类型（花萼长度等）
# y: 测量值（size）
# hue: 按物种分组
sns.boxplot(x=‘measurement’, y=‘size’, hue=‘species’, data=data_long)
plt.show()
```
运行这段代码，我们将得到一个箱线图。X轴上是四个不同的特征，每个特征下又按三种鸢尾花种类分成了三个并排的箱体，清晰地展示了不同种类在各个特征上的值域、中位数和异常值分布。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_93.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_94.png)

## 问题10：创建配对图 🔄

![](img/eb0ae3a78138de7d376260f7b17e5cc3_95.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_96.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_97.png)

接下来，我们使用Seaborn的配对图功能来检查所有特征两两之间的相关性。这在探索阶段非常强大，可以用一行代码生成一个复杂的矩阵图。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_99.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_100.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_101.png)

**核心代码**非常简单：
```python
# 绘制配对图，并按物种着色
plot = sns.pairplot(data, hue=‘species’)
# 移除图例（可选）
plot._legend.remove()
plt.show()
```

![](img/eb0ae3a78138de7d376260f7b17e5cc3_103.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_104.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_105.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_106.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_107.png)

在这张图中：
*   对角线位置显示的是每个特征在不同物种下的**分布直方图**。
*   非对角线位置显示的是任意两个特征之间的**散点图**，点按物种着色。
*   通过观察散点图的趋势，我们可以直观判断特征间的相关性。例如，花瓣长度和花瓣宽度通常呈现强烈的正相关关系（一个增大，另一个也增大），这在散点图中表现为明显的向上趋势。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_109.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_110.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_111.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_112.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_114.png)

配对图让我们能够一次性审视所有特征对之间的关系，高效地发现潜在的模式和聚类情况。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_116.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_118.png)

## 课程总结 🎯

![](img/eb0ae3a78138de7d376260f7b17e5cc3_120.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_121.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_122.png)

本节课中我们一起学习了数据探索分析（EDA）中两个重要的高级可视化技巧：
1.  **分组箱线图**：我们掌握了如何将数据从“宽格式”重塑为“长格式”，并利用`sns.boxplot()`的`hue`参数，绘制出按类别分组的箱线图，用于比较不同组在多个特征上的分布差异。
2.  **配对图**：我们使用了`sns.pairplot()`这一强大工具，仅用一行代码就生成了特征关系矩阵图，它结合了散点图和直方图，是探索多维数据内部相关性和聚类结构的利器。

![](img/eb0ae3a78138de7d376260f7b17e5cc3_123.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_124.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_125.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_127.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_129.png)

![](img/eb0ae3a78138de7d376260f7b17e5cc3_130.png)

通过本实验，你应该体会到Seaborn库在简化复杂可视化任务方面的优势。掌握这些技能，能让你在未来的数据分析工作中更快速、更直观地从数据中提取洞察。