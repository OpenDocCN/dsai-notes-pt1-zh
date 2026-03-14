# 33：向前逐步选择法 📈

![](img/5b1ef7b1f5affc37f07abcf5e971a807_0.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_2.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_4.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_5.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_7.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_8.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_9.png)

在本节课中，我们将要学习一种名为“向前逐步选择”的变量选择方法。我们将了解它如何工作，它与“最优子集选择法”有何不同，以及它的优缺点。

## 概述

上一节我们介绍了最优子集选择法，它试图在所有可能的变量组合中找到最佳模型。然而，当预测变量数量 `P` 较大时，这种方法在计算和统计上都会遇到困难。本节中，我们来看看一种更高效、更实用的替代方法：向前逐步选择法。

## 向前逐步选择法的工作原理

向前逐步选择法从一个非常简单的模型开始，然后逐步添加预测变量。

**核心步骤**如下：

![](img/5b1ef7b1f5affc37f07abcf5e971a807_11.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_12.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_14.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_15.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_17.png)

1.  **从零模型开始**：我们从一个不包含任何预测变量的模型开始，这个模型只包含截距项。我们称之为零模型 `M0`。
    *   在最小二乘回归中，这意味着我们只用响应变量的均值来预测每一个观测值。

![](img/5b1ef7b1f5affc37f07abcf5e971a807_19.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_20.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_21.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_22.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_23.png)

2.  **逐步添加变量**：然后，我们尝试向模型中一次添加一个预测变量，直到所有 `P` 个预测变量都被包含进来。
    *   在每一步 `k`，我们考虑将当前模型 `M_{k-1}` 与一个额外的预测变量组合。我们评估所有尚未包含在模型中的预测变量，并选择那个能最大程度**减少残差平方和**或**增加 R 平方**的变量。
    *   这个过程会生成一系列嵌套的模型：`M0`, `M1`, `M2`, ..., `MP`。

![](img/5b1ef7b1f5affc37f07abcf5e971a807_25.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_27.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_28.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_29.png)

3.  **从序列中选择最终模型**：与最优子集选择法一样，我们最终会得到 `P+1` 个不同复杂度的模型。我们不能直接用 RSS 或 R 平方来比较它们，因为模型大小不同。我们需要使用交叉验证、AIC 或 BIC 等准则来从中选择一个最佳模型。

![](img/5b1ef7b1f5affc37f07abcf5e971a807_31.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_32.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_33.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_35.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_37.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_38.png)

以下是向前逐步选择法的算法流程简述：

![](img/5b1ef7b1f5affc37f07abcf5e971a807_39.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_41.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_42.png)

```
初始化：令 M0 为只包含截距的模型。
对于 k = 0, 1, ..., P-1：
    1. 考虑所有可以添加到模型 Mk 中的 P-k 个预测变量。
    2. 将每个变量分别加入 Mk，拟合 P-k 个新模型。
    3. 从这 P-k 个模型中，选择使 RSS 减少最多（或 R² 增加最多）的模型。
    4. 将该模型设为 M_{k+1}。
最终，我们从 M0, M1, ..., MP 中选出一个最优模型。
```

![](img/5b1ef7b1f5affc37f07abcf5e971a807_44.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_45.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_47.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_49.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_51.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_52.png)

## 与最优子集选择法的关键区别

![](img/5b1ef7b1f5affc37f07abcf5e971a807_53.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_55.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_56.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_57.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_58.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_60.png)

虽然目标相似，但向前逐步选择法与最优子集选择法有一个根本性的区别：

![](img/5b1ef7b1f5affc37f07abcf5e971a807_62.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_64.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_66.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_68.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_70.png)

*   **最优子集选择法**：在寻找包含 `k` 个变量的最佳模型时，它会考察 **所有** 可能的 `C(P, k)` 种变量组合。总模型数量为 `2^P`。
*   **向前逐步选择法**：在寻找包含 `k` 个变量的模型时，它只考察那些**包含前一步 `M_{k-1}` 模型所有变量，再加上一个新变量**的模型。它不会回溯或替换已选入的变量。总考察的模型数量约为 `P^2` 个。

![](img/5b1ef7b1f5affc37f07abcf5e971a807_72.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_74.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_75.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_77.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_79.png)

这个区别带来了巨大的计算优势。当 `P=40` 时，`2^P` 是一个天文数字（超过一万亿），而 `P^2` 仅为 1600。因此，向前逐步选择法可以处理预测变量数量更多的问题。

![](img/5b1ef7b1f5affc37f07abcf5e971a807_81.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_83.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_85.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_87.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_89.png)

## 向前逐步选择法的局限性

![](img/5b1ef7b1f5affc37f07abcf5e971a807_91.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_93.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_94.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_96.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_98.png)

然而，这种效率的提升是有代价的。由于向前逐步选择法只考察了所有可能模型的一个子集，它**不能保证**找到全局最优的模型（即对于给定的模型大小 `k`，RSS 最小的模型）。

**一个例子**：假设在某个数据集中，包含4个变量的全局最优模型是 `{变量A， 变量B， 变量C， 变量D}`。但向前逐步选择法可能在前三步依次选择了 `{变量A， 变量B， 变量C}`。在第四步，它只能尝试在 `{A, B, C}` 的基础上添加一个新变量（比如 `变量E`），而无法得到不包含 `变量C` 但包含 `变量D` 的模型 `{A, B, D, E}`。因此，它找到的4变量模型可能不如最优子集选择法找到的好。

![](img/5b1ef7b1f5affc37f07abcf5e971a807_100.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_101.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_102.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_103.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_104.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_105.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_106.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_108.png)

这种情况通常发生在预测变量之间存在**相关性**时。如果变量彼此独立，两种方法通常会得到相同的结果。

![](img/5b1ef7b1f5affc37f07abcf5e971a807_110.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_111.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_113.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_115.png)

## 总结

![](img/5b1ef7b1f5affc37f07abcf5e971a807_117.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_118.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_119.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_121.png)

本节课中我们一起学习了向前逐步选择法。

![](img/5b1ef7b1f5affc37f07abcf5e971a807_122.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_123.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_124.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_125.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_127.png)

*   **核心思想**：从零模型开始，每次添加一个对当前模型改进最大的预测变量，生成一系列嵌套模型。
*   **优势**：计算效率远高于最优子集选择法（考察 `~P^2` 个模型 vs `2^P` 个模型），能够处理更多预测变量，并且通过限制搜索空间，有助于缓解过拟合问题。
*   **劣势**：它是一种**贪心算法**，由于搜索路径固定且不可回溯，可能无法找到全局最优的模型子集，特别是当预测变量间存在高度相关性时。

![](img/5b1ef7b1f5affc37f07abcf5e971a807_129.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_130.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_131.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_132.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_134.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_135.png)

![](img/5b1ef7b1f5affc37f07abcf5e971a807_136.png)

因此，向前逐步选择法是在计算可行性、统计性能和模型简洁性之间取得平衡的一种实用工具。当预测变量数量较多（例如超过10-20个）时，它通常是比最优子集选择法更可行的选择。