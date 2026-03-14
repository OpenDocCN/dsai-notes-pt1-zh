# 3：统计学习与模型基础 📊

![](img/9d35f65df6a7f977cfdd2d88df7444f5_0.png)

在本节课中，我们将要学习统计学习的基本概念，特别是模型的作用、如何构建模型以及其中涉及的关键问题。我们将从一个营销数据的例子开始，逐步引入模型的数学表示、理想预测函数（回归函数）的概念，以及如何从数据中估计这个函数。

## 概述与数据示例

![](img/9d35f65df6a7f977cfdd2d88df7444f5_2.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_3.png)

我们面前有三张图表，展示了一次营销活动的销售额数据。这些销售额分别是电视广告、广播广告和报纸广告投入金额的函数。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_0.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_5.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_6.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_2.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_3.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_8.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_9.png)

至少在前两张图中，可以看到某种趋势。事实上，我们已经在每张图中用一条线性回归线来概括这种趋势。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_11.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_5.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_6.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_12.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_14.png)

我们看到存在某种关系，前两个看起来比第三个关系更强。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_16.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_17.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_8.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_9.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_19.png)

在这种情况下，我们通常希望了解响应变量“销售额”与所有这三个预测变量之间的联合关系。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_21.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_11.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_12.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_23.png)

我们希望理解它们如何共同作用来影响销售额。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_25.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_26.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_14.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_27.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_28.png)

你可以把这看作是想要将销售额建模为一个函数。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_30.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_16.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_17.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_32.png)

这个函数是电视、广播和报纸广告投入的联合函数。那么我们该怎么做呢？

![](img/9d35f65df6a7f977cfdd2d88df7444f5_34.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_36.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_19.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_37.png)

## 建立符号系统

在深入细节之前，我们先建立一些符号。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_39.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_40.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_21.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_41.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_43.png)

销售额是我们希望预测或建模的响应或目标变量，我们通常用字母 **Y** 来指代它。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_45.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_47.png)

电视广告是特征、输入或预测变量之一，我们称它为 **x1**。同样地，广播广告是 **x2**，依此类推。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_23.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_49.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_25.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_26.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_27.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_28.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_50.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_51.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_52.png)

在这个例子中，我们有三个预测变量，我们可以用一个向量来统称它们。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_54.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_30.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_56.png)

令 **x** 等于一个包含三个分量的向量：**x1**、**x2** 和 **x3**。我们通常将向量视为列向量。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_57.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_32.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_59.png)

以上就是一些基本的符号。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_60.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_61.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_34.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_63.png)

现在，用这种更紧凑的符号，我们可以将模型写为：**y = f(x) + ε**。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_65.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_36.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_37.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_67.png)

这里的 **ε** 是误差项。这个误差项是一个“兜底”项，它捕捉了 **Y** 的测量误差以及其他差异。我们的函数 **f(x)** 永远无法完美地建模 **Y**，总会有一些东西是函数无法捕捉的，这些都包含在误差项中。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_69.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_70.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_71.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_39.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_40.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_41.png)

再次强调，这里的 **f(x)** 现在是这个向量 **x** 的函数，**x** 包含这三个参数（分量）。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_73.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_74.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_43.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_75.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_45.png)

三个分量。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_77.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_78.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_79.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_47.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_80.png)

## 模型函数 f(x) 的用途

![](img/9d35f65df6a7f977cfdd2d88df7444f5_82.png)

一个好的函数 **f** 有什么用呢？以下是其主要用途：

![](img/9d35f65df6a7f977cfdd2d88df7444f5_84.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_86.png)

*   **进行预测**：利用一个好的 **f**，我们可以在新的点 **x**（用小写 **x** 表示一个具体的实例）上预测 **Y**。符号 **X = x** 中，大写 **X** 是变量，小写 **x** 是一个具体的实例值。
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_49.png)
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_50.png)
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_51.png)
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_52.png)
    即报纸、广播和电视广告的具体投入值。
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_54.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_88.png)

*   **理解变量重要性**：通过模型，我们可以理解 **x** 的哪些分量（如果有 **p** 个预测变量，就是 **p** 个分量）对于解释 **Y** 是重要的，哪些是无关的。
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_56.png)
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_57.png)
    例如，如果我们将收入建模为人口统计学变量的函数，资历和教育年限可能对收入有很大影响，但婚姻状况通常没有。我们希望模型能告诉我们这一点。
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_59.png)
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_60.png)
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_61.png)

*   **理解影响方式**：根据 **f** 的复杂程度，我们或许能够理解每个分量 **Xj** 以何种特定方式影响 **Y**。
    ![](img/9d35f65df6a7f977cfdd2d88df7444f5_63.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_90.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_91.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_92.png)

模型有很多用途，以上是其中一些。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_94.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_65.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_96.png)

## 理想的预测函数：回归函数

![](img/9d35f65df6a7f977cfdd2d88df7444f5_98.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_99.png)

那么，这个函数 **f** 是什么？是否存在一个理想的 **f**？

![](img/9d35f65df6a7f977cfdd2d88df7444f5_101.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_67.png)

在图中，我们有一个来自总体的点的大样本。这里只有一个预测变量 **X** 和一个响应变量 **Y**。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_103.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_104.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_69.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_70.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_71.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_106.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_108.png)

你可以看到这是一个散点图，有很多点，这里有2000个点。让我们把这看作是整个总体，或者一个非常大总体的代表。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_73.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_74.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_75.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_110.png)

现在，让我们思考一下一个好的函数 **f** 可能是什么。不仅仅思考整个函数，让我们思考在 **x = 4** 这个点上，我们希望 **f** 取什么值。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_111.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_112.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_113.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_77.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_78.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_79.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_80.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_115.png)

你会注意到，在 **x = 4** 处，**Y** 有很多值。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_116.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_118.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_82.png)

但一个函数只能取一个值。函数将返回一个值。那么什么是一个好值呢？

![](img/9d35f65df6a7f977cfdd2d88df7444f5_84.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_120.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_121.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_122.png)

一个好的值是返回那些 **x = 4** 的 **Y** 值的平均值。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_123.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_124.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_86.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_126.png)

我们用数学符号这样写：

![](img/9d35f65df6a7f977cfdd2d88df7444f5_88.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_128.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_129.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_130.png)

函数在值4处的值，是给定 **X = 4** 时 **Y** 的期望值。期望值只是平均值的花哨说法。这实际上是一个条件平均值，给定 **X = 4**。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_132.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_90.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_91.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_92.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_134.png)

由于在 **x = 4** 处函数只能提供一个值，平均值似乎是一个不错的选择。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_94.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_96.png)

如果我们在 **X** 的每个值上都这样做，即在每一个 **X** 值处，我们都返回具有该 **X** 值的 **Y** 的平均值（例如在 **x = 5** 处），那么就会描出我们这里看到的这条红色曲线，这被称为**回归函数**。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_136.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_98.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_99.png)

同样，我们希望得到这个小条件切片中的平均值。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_101.png)

回归函数在每一个 **X** 值处给出给定 **X** 时 **Y** 的条件期望。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_103.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_104.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_138.png)

在某种意义上，这就是对于一个总体（这里指 **Y** 和单个 **X**）的理想函数。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_140.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_141.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_106.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_143.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_108.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_145.png)

## 回归函数的性质与预测误差

![](img/9d35f65df6a7f977cfdd2d88df7444f5_146.png)

让我们进一步讨论这个回归函数。它也可以为向量 **x** 定义。例如，如果 **x** 有三个分量，那么它就是在给定 **x** 的三个分量的特定实例时，**Y** 的条件期望。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_148.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_110.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_111.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_112.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_113.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_149.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_150.png)

考虑一下，让我们把 **X** 想象成二维的，因为我们可以思考三维空间。假设 **X** 位于桌面上（二维），**Y** 垂直竖立。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_152.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_115.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_116.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_154.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_118.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_156.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_157.png)

思路是一样的。我们有一个 **Y** 和 **X** 的连续云团。我们走到平面上一个具有两个坐标 **x1** 和 **x2** 的特定点 **x**，然后问函数在该点的好值是什么。我们只需向上进入该点之上的切片，平均该点上方的 **Y** 值，并在平面上的所有点都这样做。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_159.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_120.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_121.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_122.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_123.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_124.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_160.png)

我们说它是关于函数 **f** 的 **Y** 的理想或最优预测器。这意味着，实际上它是关于一个损失函数而言的，具体来说，这个特定的函数 **f(x)** 选择将最小化平方误差和，我们这样写：

![](img/9d35f65df6a7f977cfdd2d88df7444f5_162.png)

**E[(Y - f(X))²]**

![](img/9d35f65df6a7f977cfdd2d88df7444f5_163.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_164.png)

并且，它在所有可能的函数 **g** 中最小化这个期望值。也就是说，在每一个点 **X** 处，它最小化了平均预测误差。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_126.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_128.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_129.png)
![](img/9d35f65df6a7f977cfdd2d88df7444f5_130.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_166.png)

再次，**Y** 减去 **g(X)** 的期望值，在所有函数 **g** 中。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_167.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_168.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_169.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_132.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_171.png)

在每一个点 **X** 处。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_172.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_134.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_174.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_175.png)

现在，在每一个点 **x**，我们都会犯错误，因为如果我们用这个函数来预测 **Y**，在每个点 **x** 处都有很多 **Y** 值。我们把这些错误称为 **ε**，这些是**不可约误差**。你可能知道理想函数 **f**，但它当然不能在每个点 **x** 做出完美预测，所以它必须犯一些错误，但平均而言它做得很好。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_136.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_177.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_178.png)

对于任何估计值 **f̂(x)**（我们倾向于在估计量上加“帽子”符号来表示它们是从数据中估计出来的），我们可以将 **x** 处的平方预测误差分解为两部分。一部分是不可约部分，即误差的方差。另一部分是可约部分，即我们的估计值 **f̂(x)** 与真实函数 **f(x)** 之间的差异的平方。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_179.png)

因此，期望预测误差分解为这两部分。记住这一点很重要，如果我们想改进模型，我们可以通过改变估计 **f(x)** 的方式来改进这个可约部分。

![](img/9d35f65df6a7f977cfdd2d88df7444f5_181.png)

![](img/9d35f65df6a7f977cfdd2d88df7444f5_182.png)

![](img/9d35f65df6a7f977cfdd2d88df7444