# 68：层次聚类 📊

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_0.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_2.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_3.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_5.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_6.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_8.png)

在本节课中，我们将要学习层次聚类。这是一种与K均值聚类不同的聚类方法，其主要优势在于无需预先指定聚类数量K。事实上，层次聚类能一次性提供所有可能的K值对应的聚类结果。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_10.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_11.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_13.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_14.png)

上一节我们介绍了K均值聚类及其需要预先选择K值的挑战。本节中，我们来看看层次聚类如何解决这个问题。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_15.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_16.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_18.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_19.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_21.png)

## 层次聚类的基本思想 🧩

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_22.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_23.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_24.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_26.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_28.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_29.png)

层次聚类最常见的形式是“凝聚式”或“自底向上”的聚类方法。它从每个单独的观测值（称为“叶子”）开始，逐步将它们合并，形成一个层次结构。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_31.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_32.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_34.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_35.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_37.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_39.png)

以下是层次聚类的核心操作步骤：

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_41.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_42.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_44.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_45.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_46.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_47.png)

1.  **初始化**：将每个观测点视为一个独立的簇。
2.  **寻找最近对**：计算所有簇对之间的距离，找到距离最近的两个簇。
3.  **合并簇**：将这两个最近的簇合并成一个新的簇。
4.  **重复**：重复步骤2和3，直到所有观测点都合并成一个簇。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_49.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_50.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_52.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_53.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_54.png)

这个过程会产生一个树状图，称为**系统树图**，它直观地展示了整个合并过程。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_56.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_57.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_58.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_59.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_60.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_61.png)

## 系统树图与切割 🌳

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_63.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_65.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_66.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_68.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_70.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_71.png)

系统树图是层次聚类结果最有效的总结方式。图的底部是各个观测点（叶子），每次合并操作都对应图中的一个连接点，其**高度**代表了被合并的两个簇之间的距离。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_73.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_74.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_76.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_78.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_80.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_81.png)

通过在不同高度“切割”系统树图，我们可以得到不同数量的簇。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_83.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_84.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_86.png)

*   在顶部切割，所有点属于一个簇（K=1）。
*   在中间某个高度切割，可以得到指定数量的簇（例如K=2或K=3）。
*   在底部切割，每个点自成一簇（K=n，n为样本数）。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_88.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_89.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_90.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_91.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_92.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_93.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_95.png)

因此，一次层次聚类运行，就为我们提供了从1到n所有可能聚类数量的结果。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_97.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_99.png)

## 关键概念：连接方式 🔗

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_101.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_103.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_105.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_107.png)

在合并过程中，当我们需要计算一个点与一个已有簇，或两个已有簇之间的距离时，就需要定义“连接”方式。以下是几种常见的连接方式定义：

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_109.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_110.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_111.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_112.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_114.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_115.png)

*   **完全连接**：两个簇之间的距离定义为两个簇中**最远**的两个点之间的距离。这是一种“最坏情况”的度量。
    *   `距离 = max(点i与点j之间的距离)，其中点i属于簇A，点j属于簇B`
*   **单连接**：两个簇之间的距离定义为两个簇中**最近**的两个点之间的距离。这是一种“最好情况”的度量，但容易产生长条形的链状簇。
    *   `距离 = min(点i与点j之间的距离)，其中点i属于簇A，点j属于簇B`
*   **平均连接**：两个簇之间的距离定义为两个簇中**所有点对之间距离的平均值**。这是一种折中的方法，通常能产生更平衡的簇。
    *   `距离 = average(点i与点j之间的距离)，其中点i属于簇A，点j属于簇B`
*   **质心连接**：两个簇之间的距离定义为两个簇的**质心**（即簇内所有点的平均值点）之间的欧氏距离。这种方法在系统树图中可能产生“反转”，因此不如前三种常用。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_116.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_118.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_120.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_121.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_122.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_124.png)

在实际应用中，**完全连接**和**平均连接**是最常用的两种方式。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_126.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_128.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_130.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_132.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_133.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_134.png)

## 距离度量的选择 📏

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_136.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_137.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_138.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_139.png)

除了连接方式，我们还需要选择计算点与点之间“距离”或“不相似度”的度量标准。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_141.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_143.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_144.png)

*   **欧氏距离**：这是最常用的距离度量，计算两点在特征空间中的直线距离。公式为：
    *   `d(x, y) = sqrt( sum( (x_i - y_i)^2 ) )`
*   **相关性距离**：当特征代表时间序列或某种模式时，我们可能更关心形状的相似性而非绝对位置的远近。此时可以使用相关性（或其绝对值）作为距离度量。相关性高的点被认为更“相似”。
    *   例如，在基因表达数据分析中，经常使用相关性来聚类具有相似表达模式的基因。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_146.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_147.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_148.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_149.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_150.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_151.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_152.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_153.png)

选择哪种度量取决于具体问题和数据特性。如果模式的“形状”比“水平”更重要，相关性可能是更好的选择。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_155.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_156.png)

## 实践注意事项 ⚠️

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_157.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_158.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_159.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_160.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_161.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_162.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_164.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_165.png)

以下是应用层次聚类（以及其他如K均值、主成分分析等方法）时需要注意的几个实际问题：

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_167.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_168.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_169.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_171.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_172.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_174.png)

1.  **变量标准化**：如果特征变量的量纲（单位）不同，数值范围大的变量会主导距离计算，从而主导聚类结果。通常，如果变量单位不同，需要先进行标准化（使均值为0，标准差为1）。即使单位相同，尝试标准化和不标准化两种方式有时也能提供不同的洞见。

2.  **选择聚类数量K**：对于层次聚类，通常没有公认的、绝对客观的方法来选择最佳K值。一种常见的定性方法是观察系统树图，寻找“手臂”最长（即合并距离突然增大）的地方，并在其下方进行切割，认为此处的聚类结构比较稳定。这需要结合领域知识进行判断。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_176.png)

3.  **特征选择**：用于驱动聚类的特征集合会直接影响最终结果。选择不同的特征子集可能会得到完全不同的聚类。这需要根据分析目标和对数据的理解来决定。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_178.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_179.png)

## 总结 📝

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_181.png)

本节课中我们一起学习了层次聚类。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_183.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_184.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_185.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_187.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_188.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_189.png)

*   我们了解了层次聚类是一种自底向上的凝聚式方法，它通过逐步合并最近的簇来构建一个层次结构。
*   我们认识了**系统树图**，它可视化地总结了整个合并过程，并且通过在不同高度切割树图，我们可以获得任意数量的簇。
*   我们探讨了关键的**连接方式**（完全、单、平均、质心连接）和**距离度量**（欧氏距离、相关性），理解了它们如何影响聚类结果。
*   最后，我们讨论了实践中的注意事项，包括变量标准化、聚类数量K的选择以及特征选择的重要性。

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_191.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_193.png)

![](img/e0de71a0df0e3a9f9eff1bb6ea7037ff_195.png)

层次聚类为我们提供了一种灵活且信息丰富的聚类视角，特别适合在不确定最佳聚类数量时进行探索性数据分析。