# 61：支持向量机中的特征扩展与核函数 🧠

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_0.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_2.png)

在本节课中，我们将学习如何通过特征扩展和核函数来解决线性支持向量机无法处理非线性可分数据的问题。我们将从简单的多项式扩展开始，逐步深入到更优雅、更强大的核方法。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_4.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_5.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_6.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_8.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_10.png)

---

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_11.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_13.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_15.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_16.png)

## 概述

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_18.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_20.png)

上一节我们介绍了软间隔支持向量机，它允许一些数据点违反间隔边界。然而，在某些情况下，即使使用软间隔，数据在原始特征空间中也无法被线性超平面有效分离。本节中，我们来看看如何通过将数据映射到更高维的空间来解决这个问题，并最终引入核函数这一强大工具。

---

## 特征扩展：多项式方法

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_22.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_23.png)

当数据在原始特征空间中线性不可分时，一个自然的解决方案是进行特征扩展。标准做法是引入原始特征的变换，例如多项式项。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_25.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_26.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_28.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_29.png)

例如，假设我们最初只有两个特征 `x1` 和 `x2`。我们可以添加 `x1` 的平方、`x2` 的平方、`x1` 的立方、`x1` 与 `x2` 的乘积等项。这就是多项式扩展。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_31.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_32.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_34.png)

通过这种方式，我们可以将数据从 `p` 维空间（本例中为2维）映射到一个更高维的空间。添加的变换变量越多，在这个高维空间中找到分离超平面的可能性就越大。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_36.png)

以下是多项式扩展的一个例子：
```python
# 原始特征: x1, x2
# 二次多项式扩展后的特征: x1, x2, x1^2, x2^2, x1*x2
```

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_37.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_38.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_40.png)

然后，我们在扩展后的高维空间中拟合一个**线性**支持向量机。当我们将这个高维空间中的线性决策边界投影回原始特征空间时，它在原始空间中就变成了一个**非线性**的决策边界。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_42.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_44.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_45.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_47.png)

例如，如果我们使用二次多项式（包含 `x1`, `x2`, `x1^2`, `x2^2`, `x1*x2`），那么在高维空间中的决策边界是这些新变量的线性组合，其形式如下：
`β0 + β1*x1 + β2*x2 + β3*x1^2 + β4*x2^2 + β5*x1*x2 = 0`

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_48.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_50.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_52.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_54.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_56.png)

但在原始变量 `(x1, x2)` 的视角下，由于包含了平方项和交叉项，这个边界是非线性的。这种方法能很好地解决之前无法分离的类别问题。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_58.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_60.png)

---

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_62.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_63.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_65.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_66.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_68.png)

## 从特征扩展到核函数

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_70.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_72.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_74.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_76.png)

虽然多项式扩展是一个可行的方法，但它并非最佳选择。特别是在高维数据中，高阶多项式会产生非常多的特征项，导致计算复杂度和过拟合风险急剧增加。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_78.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_79.png)

因此，我们需要一种更优雅、更可控的方法来为支持向量分类器引入非线性。这就是**核函数**的用武之地。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_81.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_82.png)

在深入核函数之前，我们需要理解**内积**在支持向量分类器中的关键作用。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_84.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_85.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_86.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_87.png)

---

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_89.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_90.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_91.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_92.png)

## 内积的作用

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_94.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_95.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_96.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_98.png)

对于两个 `p` 维向量 `xi` 和 `xi‘`，它们的内积定义为各分量乘积之和：
`<xi, xi‘> = Σ (xi_j * xi‘_j)`，其中 `j` 从 1 到 `p`。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_99.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_101.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_103.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_105.png)

一个重要的发现是，线性支持向量分类器的解可以写成以下形式：
`f(x) = β0 + Σ α_i * yi * <xi, x>`

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_107.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_108.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_110.png)

这里，`x` 是一个新的待预测点，求和是对所有训练样本 `i` 进行的。`α_i` 是模型求解过程中得到的参数（许多 `α_i` 最终为 0）。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_112.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_113.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_114.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_115.png)

为了估计这些参数 `α_i`，我们实际上只需要所有训练观测点两两之间的内积。也就是说，我们只需要一个 `n x n` 的内积矩阵（其中 `n` 是样本数）。拥有这个矩阵，我们就可以拟合支持向量分类器。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_117.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_118.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_120.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_122.png)

那些 `α_i` 不为零的训练点被称为**支持向量**。它们决定了决策边界的位置和方向。这是一种在**数据空间**中的稀疏性，与 LASSO 在特征空间中的稀疏性不同。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_124.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_126.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_128.png)

---

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_129.png)

## 核函数：高效的高维内积计算

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_131.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_132.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_133.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_135.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_137.png)

根据上一节的结论，如果我们能计算所有观测点对之间的内积，以及所有训练点与新测试点之间的内积，那么我们就能拟合并评估支持向量机。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_139.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_141.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_142.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_144.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_145.png)

核函数 `K(xi, xi‘)` 正是这样一个计算两个向量在某个（可能是隐式的）高维特征空间中内积的函数。我们甚至不需要知道这个高维空间具体是什么。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_147.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_148.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_149.png)

一个具体的例子是**多项式核**：
`K(xi, xi‘) = (1 + <xi, xi‘>)^d`

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_151.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_153.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_155.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_157.png)

这个核函数计算的是在 `d` 次多项式展开所对应的高维空间中的内积。这个空间的维度可能非常高（组合数 `C(p+d, d)`），但通过核函数，我们无需显式地计算所有高维特征，直接就能得到内积结果。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_159.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_161.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_163.png)

另一个极其流行的核函数是**径向基函数核**：
`K(xi, xi‘) = exp(-γ * ||xi - xi‘||^2)`

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_165.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_167.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_168.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_170.png)

其中，`||xi - xi‘||^2` 是两点间欧氏距离的平方，`γ` 是一个调节参数。这个核函数对应于一个**无限维**的特征空间。尽管维度无限，但通过核技巧，我们仍然可以高效地工作。`γ` 参数控制着模型的复杂度：`γ` 越大，决策边界越曲折（可能过拟合）；`γ` 越小，决策边界越平滑。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_171.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_173.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_174.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_176.png)

使用径向基核，我们可以为之前复杂的数据集拟合出非常灵活且有效的非线性决策边界。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_177.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_179.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_181.png)

---

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_183.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_184.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_185.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_186.png)

## 总结

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_188.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_189.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_190.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_192.png)

本节课中我们一起学习了支持向量机处理非线性问题的核心方法。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_193.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_195.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_197.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_198.png)

1.  **特征扩展**：通过添加多项式等特征，将数据映射到高维空间以实现线性可分。
2.  **内积表示**：理解了支持向量机的解可以基于训练数据点之间的内积来表示，并引入了支持向量的概念。
3.  **核函数**：引入了核函数这一强大工具，它能够隐式地在高维甚至无限维特征空间中计算内积，从而避免了显式特征扩展带来的计算灾难。我们重点介绍了多项式核和径向基核。

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_200.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_202.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_203.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_205.png)

![](img/a80361fc3dd2ea0ff755e13cfe3d4b1e_206.png)

通过特征扩展和核函数，支持向量机从强大的线性分类器进化成了同样强大的非线性分类器。在接下来的课程中，我们将通过实际例子来应用这些核方法。