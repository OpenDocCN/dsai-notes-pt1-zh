# 21：判别分析与朴素贝叶斯分类器 📊

![](img/d194df07a854c29ad12dbefd4185dbe5_1.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_2.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_3.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_4.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_6.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_8.png)

在本节课中，我们将学习判别分析方法的扩展，包括二次判别分析和朴素贝叶斯分类器。我们还将探讨判别分析与逻辑回归之间的区别与联系。

![](img/d194df07a854c29ad12dbefd4185dbe5_10.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_11.png)

上一节我们介绍了基于高斯密度、且各类协方差相同的线性判别分析。本节中，我们来看看当放宽这些假设时，会得到哪些更通用的分类方法。

![](img/d194df07a854c29ad12dbefd4185dbe5_13.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_15.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_17.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_19.png)

## 从线性判别到更通用的形式

![](img/d194df07a854c29ad12dbefd4185dbe5_20.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_22.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_24.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_26.png)

我们之前使用的判别规则形式是通用的。我们可以使用其他密度估计方法，并将其代入此规则来获得分类器。

![](img/d194df07a854c29ad12dbefd4185dbe5_28.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_30.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_32.png)

到目前为止，我们一直假设每个类别中的特征 `X` 服从具有相同方差的高斯分布。

![](img/d194df07a854c29ad12dbefd4185dbe5_34.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_36.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_37.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_38.png)

如果每个类别中的方差不同呢？我们可以将不同的协方差矩阵形式代入。之前因为方差相等，二次项发生了巧妙的抵消。当各类方差不同时，二次项不会抵消。

![](img/d194df07a854c29ad12dbefd4185dbe5_40.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_42.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_44.png)

因此，此时的判别函数将是 `X` 的二次函数。

![](img/d194df07a854c29ad12dbefd4185dbe5_46.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_47.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_48.png)

## 二次判别分析 🔄

![](img/d194df07a854c29ad12dbefd4185dbe5_50.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_52.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_53.png)

这种形式被称为**二次判别分析**。

![](img/d194df07a854c29ad12dbefd4185dbe5_54.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_56.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_57.png)

其判别函数包含以下部分：
*   一个涉及第 `k` 类协方差矩阵 `Σ_k` 的距离项 `(x - μ_k)^T Σ_k^{-1} (x - μ_k)`。
*   一个与先验概率 `π_k` 相关的项。
*   一个来自协方差矩阵的行列式项 `log |Σ_k|`。

![](img/d194df07a854c29ad12dbefd4185dbe5_59.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_60.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_61.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_63.png)

由于二次项在不同类别中各不相同，这会给出一个**弯曲的判别边界**。

![](img/d194df07a854c29ad12dbefd4185dbe5_65.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_66.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_68.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_69.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_71.png)

以下是二次判别分析与线性判别分析的比较：
*   当真实边界为线性时（左图），线性判别分析表现良好；二次判别分析给出的边界略有弯曲，但对误分类性能影响不大。
*   当数据来自协方差不同的情况时（右图），真实的决策边界是弯曲的。二次判别分析能很好地捕捉它，而线性判别分析则给出完全不同的边界。

![](img/d194df07a854c29ad12dbefd4185dbe5_73.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_74.png)

**二次判别分析**在变量数量较少时很有吸引力。当特征数量很大时，需要估计这些大型协方差矩阵，计算可能变得困难。

![](img/d194df07a854c29ad12dbefd4185dbe5_75.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_76.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_77.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_79.png)

## 朴素贝叶斯分类器 🎯

![](img/d194df07a854c29ad12dbefd4185dbe5_81.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_82.png)

另一种有用的方法，尤其是在特征数量很多时，是**朴素贝叶斯分类器**。

![](img/d194df07a854c29ad12dbefd4185dbe5_84.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_85.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_86.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_87.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_89.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_90.png)

其核心假设是：在每个类别内部，特征变量是**条件独立**的。这意味着每个类别的联合密度可以分解为各个特征边缘密度的乘积。

![](img/d194df07a854c29ad12dbefd4185dbe5_92.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_93.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_94.png)

在线性判别分析的框架下，这相当于假设每个类别的协方差矩阵 `Σ_k` 是对角矩阵。

![](img/d194df07a854c29ad12dbefd4185dbe5_95.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_97.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_98.png)

以下是朴素贝叶斯的优势：
*   **参数大幅减少**：估计一个完整的 `P×P` 协方差矩阵需要 `P^2` 个参数。若假设为对角矩阵，则只需估计 `P` 个参数（对角线上的方差）。
*   **高维问题中的实用性**：这个假设看似很强且通常不成立，但在高维分类问题中非常有用。因为分类时我们只关心哪个类别的概率最大，而不需要概率估计绝对精确。虽然可能得到有偏的概率估计，但用更少参数估计带来的**方差降低**，常常能换来良好的分类性能。

![](img/d194df07a854c29ad12dbefd4185dbe5_100.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_101.png)

朴素贝叶斯的判别函数形式相对简单。对于第 `k` 类，其判别函数包含：
*   每个特征的贡献：`(x_j - μ_{kj})^2 / σ_{kj}^2`。
*   一个行列式项。
*   一个先验概率项。

![](img/d194df07a854c29ad12dbefd4185dbe5_103.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_104.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_105.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_106.png)

计算每个类别的判别函数值，然后将样本分到值最大的那个类别。

![](img/d194df07a854c29ad12dbefd4185dbe5_108.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_109.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_110.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_111.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_112.png)

朴素贝叶斯还可以处理**混合特征**（定量和定性特征）：
*   对于定量特征，使用高斯密度。
*   对于定性特征，用直方图或概率质量函数（基于离散类别的经验概率）代替高斯密度。

![](img/d194df07a854c29ad12dbefd4185dbe5_114.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_115.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_117.png)

这使得朴素贝叶斯非常方便实用。

![](img/d194df07a854c29ad12dbefd4185dbe5_119.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_121.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_122.png)

## 与逻辑回归的比较与联系 🔗

![](img/d194df07a854c29ad12dbefd4185dbe5_124.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_126.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_128.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_130.png)

我们已经看到了两种分类方法：逻辑回归和线性判别分析。它们之间有何异同？

![](img/d194df07a854c29ad12dbefd4185dbe5_132.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_134.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_135.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_137.png)

可以证明，对于**两类别**的线性判别分析，其**对数几率** `log(P(Y=1|X)/P(Y=0|X))` 是 `X` 的线性函数。这与逻辑回归的形式完全相同。

![](img/d194df07a854c29ad12dbefd4185dbe5_139.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_141.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_143.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_145.png)

因此，两者都给出**线性的对数几率模型**。它们的区别在于**参数估计的方式**：
*   **逻辑回归**使用**条件似然**，基于 `P(Y|X)` 进行估计。这在机器学习中被称为**判别式学习**。
*   **判别分析**使用**全似然**，通过建模每个类别中 `X` 的分布（均值和方差）以及类先验概率，来估计参数。这相当于对 `X` 和 `Y` 的联合分布进行建模，被称为**生成式学习**。

![](img/d194df07a854c29ad12dbefd4185dbe5_147.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_149.png)

尽管存在这种根本区别，但在实践中，两者的结果通常非常相似。

![](img/d194df07a854c29ad12dbefd4185dbe5_151.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_152.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_154.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_156.png)

此外，逻辑回归也可以通过显式地在模型中加入特征的**二次项**（如 `X^2`, `X_i * X_j`）来拟合**二次边界**，这与二次判别分析的功能类似。

![](img/d194df07a854c29ad12dbefd4185dbe5_158.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_159.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_161.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_163.png)

## 总结与预告 📝

![](img/d194df07a854c29ad12dbefd4185dbe5_165.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_167.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_168.png)

本节课我们一起学习了判别分析方法的两个重要扩展：
1.  **二次判别分析**：允许每个类别有不同的协方差矩阵，从而能够捕捉非线性的决策边界。
2.  **朴素贝叶斯分类器**：假设特征在类别内条件独立，极大地减少了高维问题中的参数估计量，在实践中常表现优异。

我们还辨析了线性判别分析与逻辑回归的联系（都产生线性对数几率）与区别（生成式 vs 判别式学习）。

![](img/d194df07a854c29ad12dbefd4185dbe5_170.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_171.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_172.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_173.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_174.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_175.png)

在接下来的课程中，我们将回到这些方法，探讨它们更通用的版本。重要的是，我们将讨论另一种非常流行的分类方法——**支持向量机**。

![](img/d194df07a854c29ad12dbefd4185dbe5_177.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_179.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_180.png)

![](img/d194df07a854c29ad12dbefd4185dbe5_182.png)

**下节预告**：接下来的章节是关于**交叉验证**和**自助法**。我们将邀请到这些方法的先驱之一，Brad Efron 教授，来为我们讲解。