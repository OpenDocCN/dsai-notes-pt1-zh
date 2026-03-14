# 59：支持向量机（SVM）入门 🚀

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_0.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_2.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_3.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_5.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_7.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_8.png)

在本节课中，我们将要学习一种名为**支持向量机**的分类方法。这是一种直接寻找特征空间中分隔不同类别平面的技术，自诞生以来约20年，它依然是分类任务中最流行和有效的方法之一。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_10.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_11.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_12.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_13.png)

---

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_15.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_16.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_17.png)

## 超平面是什么？📐

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_19.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_20.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_22.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_23.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_25.png)

上一节我们介绍了支持向量机的核心思想是寻找分隔平面。在深入之前，我们需要理解什么是超平面。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_27.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_29.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_31.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_32.png)

在P维特征空间中，超平面是一个维度为P-1的平坦子空间。其方程是一个线性方程，形式如下：

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_34.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_35.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_36.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_37.png)

**公式：** `β₀ + β₁X₁ + β₂X₂ + ... + βₚXₚ = 0`

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_39.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_41.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_42.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_44.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_46.png)

其中，`β₀` 称为截距。如果 `β₀ = 0`，则超平面经过原点。向量 `β₁` 到 `βₚ`（不包括 `β₀`）被称为**法向量**，它指向垂直于超平面表面的方向。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_48.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_50.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_52.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_53.png)

在二维空间中，超平面就是一条直线。当法向量是单位向量（即其分量平方和为1）时，函数 `f(X) = β₀ + β₁X₁ + β₂X₂ + ... + βₚXₚ` 计算出的值，其绝对值等于点 `X` 到该超平面的**欧几里得距离**。符号表示点在超平面的哪一侧：一侧为正，另一侧为负，超平面上的点距离为零。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_55.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_57.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_58.png)

---

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_60.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_61.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_62.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_64.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_66.png)

## 分隔超平面 ✂️

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_68.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_69.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_71.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_73.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_74.png)

理解了超平面后，我们现在可以讨论分隔超平面的概念。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_76.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_78.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_79.png)

假设我们有一些数据点，属于两个类别（例如蓝色和紫色）。如果一个超平面能够将所有蓝色点置于一侧，所有紫色点置于另一侧，那么这个超平面就是一个**分隔超平面**。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_80.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_82.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_84.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_86.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_88.png)

我们可以将类别标签编码为 `+1`（例如蓝色）和 `-1`（例如紫色）。对于一个成功的分隔超平面，对于所有数据点 `i`，以下条件成立：

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_89.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_91.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_93.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_94.png)

**公式：** `y_i * (β₀ + β₁X_i1 + β₂X_i2 + ... + βₚX_ip) > 0`

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_96.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_98.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_99.png)

这意味着每个点都被正确分类到了超平面的正确一侧。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_101.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_102.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_103.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_105.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_107.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_108.png)

然而，对于一组可线性分隔的数据，通常存在无数个可能的分隔超平面。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_110.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_112.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_114.png)

---

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_116.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_117.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_119.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_121.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_122.png)

## 最大间隔分类器 🛡️

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_124.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_125.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_127.png)

既然存在多个分隔超平面，我们该如何选择？这就引出了**最大间隔分类器**的思想。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_129.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_130.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_131.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_133.png)

其核心思想是：在所有可能的分隔超平面中，选择那个能创造出两个类别之间**最大间隔**或**最宽间隙**的一个。这个间隔定义为离超平面最近的点的距离。选择最大间隔分类器的直觉是，在训练数据上创造一个大的“安全边界”，可能使模型对未来的新数据（测试数据）具有更好的泛化能力。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_135.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_136.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_137.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_138.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_140.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_142.png)

以下是将其形式化为一个优化问题的方法：

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_144.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_146.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_147.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_149.png)

1.  约束法向量 `β` 为单位长度：`β₁² + β₂² + ... + βₚ² = 1`。这确保了 `f(X)` 的值是点到超平面的有符号距离。
2.  我们希望所有数据点都至少离超平面 `M` 个单位远。即对于所有点 `i`：
    **公式：** `y_i * (β₀ + β₁X_i1 + ... + βₚX_ip) ≥ M`
3.  我们的目标是找到参数 `β₀, β₁, ..., βₚ`，使得这个边界 `M` 的值**最大化**。

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_151.png)

![](img/e4ed7fc58d924a51e3a0ef5ea731ea1a_153.png)

这个优化问题可以通过凸优化技术求解。在实践中，我们可以使用软件包（例如R语言中的 `e1071` 包的 `svm` 函数）来找到这个最优超平面。

---

## 总结与展望 📈

本节课中我们一起学习了支持向量机的基础。我们从超平面的数学定义开始，理解了如何用它来分隔两类数据，并介绍了追求最大分类间隔的**最大间隔分类器**及其数学优化形式。

然而，最大间隔分类器要求数据必须**严格线性可分**，这在现实问题中往往不成立。数据中可能存在噪声，或者类别本身就是非线性交织的。

因此，下一节我们将探讨当无法找到完美分隔超平面时的解决方案。支持向量机技术主要通过两种方式变得更具创造力：一是**软化**“分隔”的定义，允许一些点落在间隔内或错误的一侧；二是通过**扩大特征空间**（例如使用核技巧），使得在更高维空间中线性分隔成为可能。这将帮助我们构建更强大、更实用的分类器。