# 152：随机与合成过采样 📊

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_0.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_2.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_4.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_5.png)

在本节课中，我们将学习如何处理类别不平衡问题，重点介绍两种过采样方法：随机过采样和合成过采样。我们将详细探讨它们的原理、适用场景以及具体变体。

## 概述

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_7.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_8.png)

过采样的核心目标是**增加少数类样本的数量**，使其规模与多数类相近，从而改善模型在类别不平衡数据上的表现。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_10.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_11.png)

上一节我们介绍了类别不平衡的基本概念，本节中我们来看看两种具体的过采样技术。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_13.png)

## 随机过采样 🔄

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_14.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_15.png)

随机过采样是最简单的方法。其操作是**对少数类样本进行有放回的随机重采样**。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_17.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_19.png)

这种方法不考虑样本在特征空间中的位置，也不关心哪些点更能代表少数类的真实分布。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_21.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_23.png)

以下是随机过采样的关键点：
*   该方法对**分类数据**效果最好。
*   对于分类数据，样本点之间的距离可能不直接代表内在的相似性。
*   因此，我们无需担心是否存在数据簇，也无需在采样时精确地定义这些簇的边界。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_25.png)

## 合成过采样 🧬

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_27.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_28.png)

合成过采样是一种更高级的方法，它**创建新的、原本不存在的少数类样本**。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_30.png)

其基本思想基于K近邻算法，步骤如下：
1.  从少数类中选择一个样本点 `X_i`。
2.  找到该点的 `K` 个最近邻，并从中随机选择一个邻居 `X_zi`。
3.  在连接 `X_i` 和 `X_zi` 的线段上，随机生成一个新的样本点 `X_new`。
4.  对设定的 `K` 个邻居重复此过程。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_31.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_33.png)

合成过采样主要有两种主流方法，它们都以上述K近邻思想为基础：**SMOTE** 和 **ADASYN**。

### SMOTE 方法

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_35.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_36.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_37.png)

SMOTE 是“合成少数类过采样技术”的缩写，它可以进一步分为几个子类。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_39.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_41.png)

#### 常规 SMOTE

常规SMOTE将少数类样本点连接到**任何**最近邻（即使是其他类别的样本），然后利用这些连接线在其间随机生成新样本点。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_43.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_44.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_45.png)

#### 边界线 SMOTE

边界线SMOTE首先需要将少数类点分类为：**异常点**、**安全点**或**危险点**。
*   **异常点**：所有最近邻样本都来自不同类别。
*   **安全点**：所有最近邻样本都来自相同类别。
*   **危险点**：至少一半的最近邻样本来自相同类别（但并非全部）。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_47.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_48.png)

边界线SMOTE又分为两种类型：
*   **边界线 SMOTE-1**：仅连接少数类中的“危险点”到其他少数类点，并利用这些连接生成新样本。
*   **边界线 SMOTE-2**：连接少数类中的“危险点”到其附近的任何样本点，无论其类别。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_50.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_51.png)

#### SVM SMOTE

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_52.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_54.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_55.png)

SVM SMOTE 使用支持向量机分类器来寻找**支持向量**，然后基于这些支持向量来生成新的合成样本。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_57.png)

对于边界线SMOTE和SVM SMOTE，需要使用参数 `m_neighbors` 来定义邻域大小，以判断一个样本是危险点、安全点还是异常点。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_58.png)

### ADASYN 方法

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_60.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_61.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_62.png)

自适应合成采样与SMOTE的工作原理非常相似。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_64.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_66.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_67.png)

它同样从检查每个少数类样本点的邻域类别构成开始。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_69.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_70.png)

然而，**为每个点生成的样本数量，将与该点邻域中不属于同一类别的样本数量成比例**。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_72.png)

因此，ADASYN 会在“最近邻规则”不被尊重的区域（即类别边界模糊、容易误分类的区域）生成更多的样本，从而对那些原本可能被误分类的样本赋予更大的权重。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_74.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_76.png)

## 总结

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_77.png)

本节课我们一起学习了处理类别不平衡的过采样方法。
*   我们介绍了最简单的**随机过采样**，它适用于分类数据。
*   我们深入探讨了基于K近邻的**合成过采样**，包括SMOTE的多种变体（常规、边界线、SVM）以及ADASYN方法。
*   这些技术虽然基于K近邻思想，但有助于改善任何因类别不平衡而产生问题的分类任务。

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_79.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_80.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_81.png)

![](img/28764e6e6fdb2b0be165fe67d7b82b2e_82.png)

在下一节视频中，我们将介绍用于处理类别不平衡的另一种技术——欠采样的不同方法。