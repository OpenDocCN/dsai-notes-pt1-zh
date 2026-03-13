# 139：Boosting算法中的损失函数与关键概念 🎯

![](img/41eb28c768a6b6f824ec05372c7aeed7_1.png)

在本节课中，我们将要学习Boosting算法中使用的损失函数，并深入理解Adaboost与梯度提升（Gradient Boosting）的核心机制及其区别。我们还将探讨如何通过调整超参数来优化模型性能。

---

![](img/41eb28c768a6b6f824ec05372c7aeed7_3.png)

## 损失函数在Boosting中的作用

![](img/41eb28c768a6b6f824ec05372c7aeed7_4.png)

上一节我们介绍了Boosting的基本思想，本节中我们来看看在Boosting过程中，如何通过损失函数来衡量和修正模型的错误。

![](img/41eb28c768a6b6f824ec05372c7aeed7_6.png)

在每个阶段或针对每一个弱学习器，我们可以为数据集中的每个点计算一个**间隔**。

![](img/41eb28c768a6b6f824ec05372c7aeed7_8.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_1.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_10.png)

对于正确分类的点，其间隔值为正，在图中位于右侧。

![](img/41eb28c768a6b6f824ec05372c7aeed7_6.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_12.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_14.png)

对于错误分类的点，其间隔值为负，在图中位于左侧。

![](img/41eb28c768a6b6f824ec05372c7aeed7_8.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_16.png)

损失函数的值可以被视为样本点到决策边界的“距离”。

![](img/41eb28c768a6b6f824ec05372c7aeed7_18.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_19.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_10.png)

就像逻辑回归与线性回归的区别一样，我们可以选择对远离边界的误分类点进行**重度惩罚**或**轻度惩罚**。

![](img/41eb28c768a6b6f824ec05372c7aeed7_21.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_12.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_23.png)

损失函数决定了惩罚的方式，也决定了我们将要使用的具体Boosting算法类型。

![](img/41eb28c768a6b6f824ec05372c7aeed7_25.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_16.png)

---

![](img/41eb28c768a6b6f824ec05372c7aeed7_27.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_28.png)

## 常见的Boosting损失函数

![](img/41eb28c768a6b6f824ec05372c7aeed7_30.png)

以下是实践中常用的几种损失函数选项。

### 0-1损失函数

![](img/41eb28c768a6b6f824ec05372c7aeed7_32.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_34.png)

最常被讨论的损失函数是**0-1损失**。

![](img/41eb28c768a6b6f824ec05372c7aeed7_18.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_36.png)

该函数对错误分类的点返回1，对正确分类的点忽略不计。

![](img/41eb28c768a6b6f824ec05372c7aeed7_38.png)

**公式表示：**
`L(y, f(x)) = I(y ≠ sign(f(x)))`
其中 `I()` 是指示函数。

![](img/41eb28c768a6b6f824ec05372c7aeed7_40.png)

然而，这是一个理论上的损失函数，在实践中很少使用。

![](img/41eb28c768a6b6f824ec05372c7aeed7_42.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_43.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_23.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_45.png)

原因在于它**不可微**，且不光滑、不凸，难以优化。

![](img/41eb28c768a6b6f824ec05372c7aeed7_47.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_25.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_49.png)

因此，在实践中我们会使用其他类型的损失函数。

---

![](img/41eb28c768a6b6f824ec05372c7aeed7_51.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_52.png)

### Adaboost的指数损失函数

首先，我们来看看Adaboost使用的损失函数。

![](img/41eb28c768a6b6f824ec05372c7aeed7_54.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_55.png)

**Adaboost**（自适应提升）是最早投入实践的Boosting算法之一。

![](img/41eb28c768a6b6f824ec05372c7aeed7_57.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_32.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_59.png)

它使用**指数损失函数**。

![](img/41eb28c768a6b6f824ec05372c7aeed7_34.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_61.png)

**公式表示：**
`L(y, f(x)) = exp(-y * f(x))`

![](img/41eb28c768a6b6f824ec05372c7aeed7_62.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_63.png)

如图所示，间隔值非常负的点（即严重误分类的点）会对损失产生强烈影响。

![](img/41eb28c768a6b6f824ec05372c7aeed7_36.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_65.png)

如果一个点距离边界很远且分类错误，它会对整体误差做出巨大贡献。

![](img/41eb28c768a6b6f824ec05372c7aeed7_66.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_67.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_40.png)

这使得Adaboost对**异常值非常敏感**。

![](img/41eb28c768a6b6f824ec05372c7aeed7_69.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_70.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_42.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_72.png)

---

### 梯度提升的对数似然损失函数

![](img/41eb28c768a6b6f824ec05372c7aeed7_74.png)

接下来，我们介绍与另一种Boosting算法相关的损失函数。

![](img/41eb28c768a6b6f824ec05372c7aeed7_75.png)

**梯度提升**是另一个实践中常用的流行算法。

![](img/41eb28c768a6b6f824ec05372c7aeed7_49.png)

它使用**对数似然损失函数**（或称逻辑损失）。

![](img/41eb28c768a6b6f824ec05372c7aeed7_77.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_47.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_78.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_79.png)

**公式表示（二分类）：**
`L(y, f(x)) = log(1 + exp(-2 * y * f(x)))`

![](img/41eb28c768a6b6f824ec05372c7aeed7_81.png)

对于误分类点，即使其间隔值很大，对数似然损失函数的增长也相对平缓。

![](img/41eb28c768a6b6f824ec05372c7aeed7_83.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_51.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_85.png)

这使得梯度提升比Adaboost对异常值更具**鲁棒性**。

![](img/41eb28c768a6b6f824ec05372c7aeed7_54.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_87.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_88.png)

---

## Bagging与Boosting的对比

![](img/41eb28c768a6b6f824ec05372c7aeed7_90.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_91.png)

现在，让我们比较一下目前讨论过的两种集成方法：Bagging和Boosting。

对于**Boosting**，我们使用整个数据集来训练每一个分类器。

![](img/41eb28c768a6b6f824ec05372c7aeed7_93.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_94.png)

对于**Bagging**，我们回忆一下，它使用自助采样法得到子样本，每个分类器只在一个自助样本上训练。

![](img/41eb28c768a6b6f824ec05372c7aeed7_61.png)

以下是两者的主要区别：

**1. 基学习器的性质与关系**
*   **Bagging**：基学习器（通常为决策树）彼此独立，且通常是完全生长的树，而非树桩。
*   **Boosting**：基学习器是弱学习器（如浅层树），它们被**顺序创建**，每个学习器都建立在之前步骤的基础上。

![](img/41eb28c768a6b6f824ec05372c7aeed7_96.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_97.png)

**2. 训练数据的考量**
*   **Bagging**：只考虑当前自助样本的数据。
*   **Boosting**：不仅考虑当前数据，在构建每个后续学习器时，还会考虑之前模型的**残差**（错误）。

![](img/41eb28c768a6b6f824ec05372c7aeed7_98.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_99.png)

**3. 投票机制**
*   **Bagging**：所有样本在最终分类中权重相等（平等投票）。
*   **Boosting**：在每次迭代中，通过给之前犯错的样本**赋予更高权重**来尝试纠正错误，因此每个弱学习器有不同的权重。

**4. 过拟合风险**
*   **Bagging**：通常不担心树的数量过多导致过拟合。
*   **Boosting**：必须警惕过拟合的风险。

![](img/41eb28c768a6b6f824ec05372c7aeed7_101.png)

为什么Boosting容易过拟合？因为随着树的数量增加，我们持续尝试改进之前树木所犯的错误。到某个阶段，我们确实会面临过拟合的危险，因为我们不断地在修正过去的误差。

![](img/41eb28c768a6b6f824ec05372c7aeed7_102.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_103.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_90.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_105.png)

---

![](img/41eb28c768a6b6f824ec05372c7aeed7_107.png)

## 优化Boosting模型：关键超参数

考虑到过拟合风险，我们需要使用交叉验证来确定Boosting模型中应使用的**正确树的数量**。

![](img/41eb28c768a6b6f824ec05372c7aeed7_93.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_109.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_110.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_111.png)

此外，为了正确正则化模型，还需要优化**学习率**。

学习率代表每一步对模型的修正程度。较低的学率意味着每次修正幅度小，因此可能需要更多的树。学习率和树的数量这两个超参数是相关的。

![](img/41eb28c768a6b6f824ec05372c7aeed7_113.png)

理想情况下，为了减少每一步的修正量，应将学习率（也称为**收缩率**）设置为小于1的值，以缩减每个后续学习器的影响。

![](img/41eb28c768a6b6f824ec05372c7aeed7_115.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_116.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_101.png)

梯度提升中另一个可用的参数是**子采样率**。

这个参数通过引入随机性来帮助减少过拟合。使用子采样意味着基学习器不在整个数据集上训练，这不仅能加快优化速度，也起到一定的正则化作用，因为模型不会完美拟合所有数据。

![](img/41eb28c768a6b6f824ec05372c7aeed7_118.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_119.png)

实践中，单独使用子采样对降低测试误差的帮助可能不大，但将其与较低的学习率结合（避免过度修正）通常效果很好。

![](img/41eb28c768a6b6f824ec05372c7aeed7_120.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_115.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_122.png)

另一个参数是**最大特征数**。它决定了在寻找每个分裂点时要考虑的特征数量，这同样可以降低模型的潜在复杂度。

![](img/41eb28c768a6b6f824ec05372c7aeed7_118.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_124.png)

需要指出的是，这些超参数调优的效果在实践中取决于具体的数据集。通常，在决定使用哪些不同的超参数时，应考虑使用交叉验证。

![](img/41eb28c768a6b6f824ec05372c7aeed7_125.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_126.png)

---

![](img/41eb28c768a6b6f824ec05372c7aeed7_128.png)

## 总结

本节课中我们一起学习了：
1.  **损失函数的核心作用**：在Boosting中用于衡量错误并指导模型迭代修正。
2.  **两种主要损失函数**：
    *   Adaboost使用的**指数损失**，对异常值敏感。
    *   梯度提升使用的**对数似然损失**，对异常值更鲁棒。
3.  **Bagging与Boosting的详细对比**：从基学习器关系、数据使用、投票机制到过拟合风险。
4.  **Boosting的关键超参数**：包括树的数量、学习率、子采样率和最大特征数，以及它们如何用于控制模型复杂度和防止过拟合。

![](img/41eb28c768a6b6f824ec05372c7aeed7_130.png)

![](img/41eb28c768a6b6f824ec05372c7aeed7_131.png)

在下一节课中，我们将通过一个简短的演练，学习在Scikit-learn中使用梯度提升和Adaboost的实际语法。