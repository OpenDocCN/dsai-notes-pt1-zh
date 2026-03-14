# 35：模型选择与评估指标 📊

![](img/7e52b57fa472fea66be6726d0e22fac3_0.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_2.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_4.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_6.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_8.png)

在本节课中，我们将学习如何从一系列不同复杂度的模型（例如 M0, M1, ..., Mp）中选择最佳模型。核心在于，我们需要一种方法来**估计测试误差**，以便在不同模型间进行比较和选择。

![](img/7e52b57fa472fea66be6726d0e22fac3_10.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_11.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_12.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_13.png)

为了估计测试误差，主要有两种思路。上一节我们介绍了模型选择的需求，本节中我们来看看具体的评估方法。

![](img/7e52b57fa472fea66be6726d0e22fac3_15.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_16.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_18.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_20.png)

## 两种估计测试误差的途径

![](img/7e52b57fa472fea66be6726d0e22fac3_22.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_23.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_24.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_26.png)

第一种途径是**间接估计**。其核心思想是，通过计算训练误差，并对其进行调整以修正过拟合带来的偏差，从而得到一个近似于测试误差的估计值。

![](img/7e52b57fa472fea66be6726d0e22fac3_28.png)

第二种途径是**直接估计**。这涉及到使用本书第5章介绍的方法，例如交叉验证或验证集方法。其做法是：将数据分为训练集和测试集，在训练集上拟合模型，然后在独立的测试集上评估其性能。这是一种非常直接的测试误差估计方法。

![](img/7e52b57fa472fea66be6726d0e22fac3_30.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_31.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_32.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_33.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_35.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_37.png)

接下来，我们将详细讨论这两种途径下的具体方法。

![](img/7e52b57fa472fea66be6726d0e22fac3_39.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_41.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_43.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_44.png)

## 间接估计方法：调整训练误差

![](img/7e52b57fa472fea66be6726d0e22fac3_46.png)

CP、AIC、BIC 和调整R方等方法，都是通过对训练误差进行调整，来为我们提供测试误差的估计。

![](img/7e52b57fa472fea66be6726d0e22fac3_48.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_49.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_50.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_51.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_52.png)

这些方法都可以用于在不同变量数量的模型中进行选择。

![](img/7e52b57fa472fea66be6726d0e22fac3_53.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_54.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_56.png)

以下是使用信用数据，通过最优子集选择法得到的每个最佳模型大小对应的 CP、BIC 和调整R方图示。我们先观察这张图，然后讨论这些指标是如何定义的。

![](img/7e52b57fa472fea66be6726d0e22fac3_58.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_60.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_22.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_62.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_64.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_66.png)

在这张图中，横轴代表每个模型的预测变量数量。纵轴则分别代表 CP、BIC（贝叶斯信息准则）和调整R方。我们稍后会定义这三个指标。

![](img/7e52b57fa472fea66be6726d0e22fac3_68.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_69.png)

大致来说，我们希望 CP 和 BIC 的值尽可能小，而调整R方的值尽可能大。

![](img/7e52b57fa472fea66be6726d0e22fac3_71.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_72.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_74.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_76.png)

观察曲线形状可以发现：
*   CP 在包含 **6个** 预测变量的模型上达到最小。
*   BIC 在包含 **4个** 预测变量的模型上达到最小。
*   调整R方在包含 **6个** 预测变量的模型上达到最大。

![](img/7e52b57fa472fea66be6726d0e22fac3_78.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_79.png)

这表明我们应该使用大约 4 到 6 个预测变量。实际上，如果更仔细地观察这些图，会发现曲线在预测变量数量达到 3 或 4 个之后基本趋于平缓。因此，基于这些图表，可以认为对于这份信用数据，我们最多只需要 3 到 4 个预测变量就能做出很好的预测。

![](img/7e52b57fa472fea66be6726d0e22fac3_81.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_82.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_84.png)

### 1. Mallow‘s Cp
Mallow‘s Cp 是对训练RSS的调整，旨在估计测试RSS。其定义公式如下：

![](img/7e52b57fa472fea66be6726d0e22fac3_86.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_87.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_88.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_90.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_92.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_94.png)

对于一个包含 **d** 个预测变量（包括截距项）的模型，其 Cp 值为：
`Cp = (1/n) * [RSS + 2 * d * σ_hat^2]`

![](img/7e52b57fa472fea66be6726d0e22fac3_95.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_97.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_99.png)

其中：
*   **RSS** 是该模型的残差平方和。
*   **σ_hat^2** 是线性模型中误差项 ε 方差的估计值。
*   **n** 是样本量。

![](img/7e52b57fa472fea66be6726d0e22fac3_101.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_102.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_104.png)

计算所有候选模型（M0 到 Mp）的 Cp 值，然后选择 **Cp 值最小的模型**。

![](img/7e52b57fa472fea66be6726d0e22fac3_106.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_107.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_108.png)

关于 σ_hat^2 的估计，通常使用包含所有 **p** 个预测变量的“全模型”的残差均方（MSE）来估计。这意味着 Cp 方法通常要求 **n > p**，否则全模型无法拟合或误差估计不准确。

![](img/7e52b57fa472fea66be6726d0e22fac3_110.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_111.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_113.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_115.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_116.png)

### 2. AIC（赤池信息准则）
AIC 是另一个紧密相关的准则，其定义为：
`AIC = -2 * log(L) + 2 * d`

![](img/7e52b57fa472fea66be6726d0e22fac3_117.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_119.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_121.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_122.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_124.png)

其中：
*   **L** 是所估计模型的最大似然值。
*   **d** 是模型中的参数数量（预测变量数+1）。

![](img/7e52b57fa472fea66be6726d0e22fac3_126.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_128.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_129.png)

这个定义看起来很通用，因为 AIC 可以应用于多种模型类型（如逻辑回归）。对于线性模型，可以证明 `-2*log(L)` 与 `RSS/σ_hat^2` 成比例。因此，**在线性模型且 σ_hat^2 已知的情况下，选择最小 AIC 的模型等价于选择最小 Cp 的模型**。但对于其他类型的模型，AIC 是一个独立且有效的方法。

![](img/7e52b57fa472fea66be6726d0e22fac3_131.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_133.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_135.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_137.png)

### 3. BIC（贝叶斯信息准则）
BIC 与 AIC、Cp 类似，但源于贝叶斯论证。其公式为：
`BIC = (1/n) * [RSS + log(n) * d * σ_hat^2]`

![](img/7e52b57fa472fea66be6726d0e22fac3_139.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_141.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_143.png)

同样，我们选择 **BIC 值最小的模型**。

![](img/7e52b57fa472fea66be6726d0e22fac3_145.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_147.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_149.png)

BIC 与 AIC 的主要区别在于惩罚项：AIC 使用 `2 * d`，而 BIC 使用 `log(n) * d`。通常，当 **n > 7** 时，`log(n) > 2`。这意味着 **BIC 对模型复杂度的惩罚比 AIC 更重**，因此 BIC 倾向于选择变量更少的、更简单的模型。

### 4. 调整R方
我们曾在第3章见过 R方。R方定义为：
`R^2 = 1 - (RSS / TSS)`
其中 TSS 是总平方和 `Σ(y_i - y_bar)^2`。

但无法直接比较不同变量数模型的 R方，因为增加变量总会提高 R方。调整R方通过引入对模型复杂度的惩罚来解决这个问题：
`Adjusted R^2 = 1 - [RSS/(n-d-1)] / [TSS/(n-1)]`

![](img/7e52b57fa472fea66be6726d0e22fac3_151.png)

我们希望 **调整R方的值尽可能大**。与 Cp、AIC、BIC 相比，调整R方的一个显著优点是**不需要估计 σ_hat^2**，因此在原则上可以应用于 **p > n** 的情况。此外，对于非统计学背景的研究者来说，调整R方也更容易解释。然而，它的一个局限性是难以推广到线性模型之外的其他模型类型（如逻辑回归）。

![](img/7e52b57fa472fea66be6726d0e22fac3_153.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_154.png)

## 直接估计方法：交叉验证与验证集

![](img/7e52b57fa472fea66be6726d0e22fac3_155.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_156.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_158.png)

以上讨论的间接方法（尤其是Cp、AIC、BIC）通常需要知道模型的参数数量 **d**，并且依赖于线性模型的假设。然而，对于一些更复杂的模型（如岭回归、Lasso），其有效的参数数量 **d** 并不明确。

![](img/7e52b57fa472fea66be6726d0e22fac3_160.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_161.png)

这时，**交叉验证** 和 **验证集** 方法的优势就体现出来了。无论模型多么复杂或“怪异”，我们总是可以将数据分割，用一部分拟合，用另一部分测试，从而直接估计其测试误差。这种方法的普适性是其最大的优点之一，我们将在下一节详细讨论。

![](img/7e52b57fa472fea66be6726d0e22fac3_163.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_164.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_165.png)

## 总结

![](img/7e52b57fa472fea66be6726d0e22fac3_167.png)

![](img/7e52b57fa472fea66be6726d0e22fac3_168.png)

本节课中我们一起学习了模型选择的两种核心思路：
1.  **间接估计测试误差**：通过调整训练误差来近似测试误差，主要方法包括：
    *   **Mallow‘s Cp**：适用于线性模型，需估计误差方差，要求 n > p。
    *   **AIC**：适用性更广，可用于多种模型，在线性模型下与 Cp 等价。
    *   **BIC**：与 AIC 类似，但对模型复杂度的惩罚更重，倾向于选择更简单的模型。
    *   **调整R方**：无需估计误差方差，可适用于 p > n 的情况，易于解释，但难以推广到非线性模型。
2.  **直接估计测试误差**：通过数据分割（验证集、交叉验证）来直接评估模型在新数据上的性能。这种方法最为直接和通用，是后续课程的重点。

这些指标为我们提供了从多个候选模型中进行客观选择的量化工具。在实践中，通常需要结合多种指标和领域知识来做出最终决策。