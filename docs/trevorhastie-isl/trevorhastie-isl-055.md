# 55：Bagging与随机森林 🌲➡️🌳

![](img/857eeae4edbb93a1b757e58abf1783b4_0.png)

在本节课中，我们将要学习两种重要的集成学习方法：Bagging（自助聚合）和随机森林。这两种方法通过组合多个决策树来显著提升预测性能，是现代机器学习中非常强大的工具。

---

## Bagging的基本思想 🧠

![](img/857eeae4edbb93a1b757e58abf1783b4_2.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_4.png)

上一节我们介绍了决策树，但单棵决策树往往方差较高，容易过拟合。本节中我们来看看如何通过“平均”的思想来降低方差。

![](img/857eeae4edbb93a1b757e58abf1783b4_6.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_8.png)

Bagging，全称Bootstrap Aggregation（自助聚合），由Leo Breiman提出。其核心思想源于一个简单的统计学原理：如果我们有一组独立同分布的观测值，那么它们的平均值会比单个观测值具有更小的方差。

![](img/857eeae4edbb93a1b757e58abf1783b4_10.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_12.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_13.png)

具体来说，假设我们有N个独立观测值 \( Z_1, Z_2, ..., Z_N \)，每个的方差都是 \( \sigma^2 \)。那么它们的平均值 \( \bar{Z} \) 的方差为：
\[
\text{Var}(\bar{Z}) = \frac{\sigma^2}{N}
\]
通过平均，我们将方差降低了N倍。

![](img/857eeae4edbb93a1b757e58abf1783b4_15.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_17.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_18.png)

在监督学习中，这意味着如果我们有多个训练集，就可以在每个训练集上训练一个模型（例如决策树），然后平均它们的预测结果。然而，现实中我们通常只有一个训练集。

![](img/857eeae4edbb93a1b757e58abf1783b4_20.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_22.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_24.png)

Bagging巧妙地解决了这个问题。它通过**自助采样法**从原始训练集中生成多个“伪”训练集，然后在每个采样集上训练一个模型，最后对所有模型的预测结果进行平均。

![](img/857eeae4edbb93a1b757e58abf1783b4_26.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_28.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_30.png)

---

![](img/857eeae4edbb93a1b757e58abf1783b4_32.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_34.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_36.png)

## Bagging的算法步骤 📝

![](img/857eeae4edbb93a1b757e58abf1783b4_38.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_39.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_40.png)

以下是Bagging的具体实现步骤：

![](img/857eeae4edbb93a1b757e58abf1783b4_41.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_42.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_43.png)

1.  **自助采样**：从原始训练集中进行B次有放回抽样，每次抽取与原始训练集相同数量的样本。这会产生B个不同的“自助样本集”。每个自助样本集平均包含约63.2%的原始样本，其余约36.8%的样本未被选中，称为“袋外样本”。

![](img/857eeae4edbb93a1b757e58abf1783b4_45.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_47.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_48.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_49.png)

2.  **训练基模型**：在每个自助样本集上独立地训练一个决策树。与单独使用决策树时不同，在Bagging中我们通常**不需要对树进行剪枝**，可以生长为大型的、低偏差的树。方差问题将通过后续的平均步骤来解决。

![](img/857eeae4edbb93a1b757e58abf1783b4_51.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_52.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_54.png)

3.  **聚合预测**：对于回归问题，最终的预测是所有树预测值的简单平均。对于分类问题，则采用**多数投票法**。

![](img/857eeae4edbb93a1b757e58abf1783b4_56.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_58.png)

我们用数学公式来描述这个过程。设第b棵树的预测函数为 \( \hat{f}^{*b}(x) \)，那么Bagging的最终预测 \( \hat{f}_{\text{bag}}(x) \) 为：
\[
\hat{f}_{\text{bag}}(x) = \frac{1}{B} \sum_{b=1}^{B} \hat{f}^{*b}(x)
\]

![](img/857eeae4edbb93a1b757e58abf1783b4_60.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_61.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_62.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_63.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_65.png)

---

![](img/857eeae4edbb93a1b757e58abf1783b4_67.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_69.png)

## 袋外误差估计 🎒

Bagging有一个非常有用的副产品——**袋外误差估计**。由于每个自助样本集只使用了约三分之二的原始数据，剩下的三分之一（袋外样本）可以天然地作为该树的验证集。

对于一个特定的观测样本，我们可以找出所有**没有使用该样本进行训练**的树，然后用这些树的预测平均值来预测该样本。这样得到的预测误差，就是对模型泛化误差的一个无偏估计。

![](img/857eeae4edbb93a1b757e58abf1783b4_71.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_72.png)

当树的数量B很大时，这种袋外误差估计在效果上近似于**留一交叉验证**，而且计算成本几乎为零，这是Bagging和随机森林的一大优势。

![](img/857eeae4edbb93a1b757e58abf1783b4_73.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_74.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_75.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_76.png)

---

## 从Bagging到随机森林 🌳🌲

虽然Bagging通过平均降低了方差，但Breiman发现，如果被平均的模型之间相关性更低，平均的效果会更好。这就是**随机森林**的出发点。

随机森林在Bagging的基础上增加了一个关键步骤：**特征随机性**。

![](img/857eeae4edbb93a1b757e58abf1783b4_78.png)

以下是随机森林与标准Bagging的唯一区别：

![](img/857eeae4edbb93a1b757e58abf1783b4_80.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_81.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_82.png)

在构建每棵决策树的每个分裂节点时，随机森林不会考虑全部P个预测变量，而是**随机选取一个大小为m的子集**（通常 \( m \approx \sqrt{P} \)），只允许在这个子集中寻找最佳分裂点。这个随机选择在**每个分裂节点**都会重新进行。

![](img/857eeae4edbb93a1b757e58abf1783b4_83.png)

这听起来似乎有些疯狂，因为我们在每次分裂时都“丢弃”了大部分可能有用的信息。但其效果是迫使不同的树使用不同的变量进行分裂，从而降低了树与树之间的相关性。由于我们会构建大量树并进行平均，那些重要的预测变量总有机会在其他树或其他分裂节点上发挥作用。

---

## 实例分析：心脏病数据与基因数据 🧬

我们通过两个例子来看Bagging和随机森林的表现。

**心脏病数据示例**：
*   单棵决策树的测试误差较高（图中虚线）。
*   Bagging（黑色曲线）通过平均多棵树，将误差降低了约1%。
*   随机森林（后续会展示）通过引入特征随机性，在Bagging的基础上进一步将误差降低了1-2%。
*   图中的绿色曲线是袋外误差估计，它与真实测试误差趋势一致，验证了其有效性。

![](img/857eeae4edbb93a1b757e58abf1783b4_85.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_87.png)

**高维基因表达数据示例**：
这个例子有349名癌症患者，用4718个基因的表达数据来预测15种癌症类型（包括健康类型）。
*   单棵决策树在这个复杂问题上的预测错误率高达50-60%。
*   Bagging（M=P，即每次分裂考虑全部变量）显著提升了性能。
*   随机森林（M=√P，即每次分裂随机选取部分变量）的表现最好，比Bagging进一步提升了约3-4%的准确率。
*   一个重要的观察是：**增加树的数量不会导致过拟合**。误差会随着树的数量增加而下降并最终趋于稳定，更多的树只会降低方差而不会增加偏差。因此，我们可以通过观察袋外误差曲线来决定需要多少棵树。

![](img/857eeae4edbb93a1b757e58abf1783b4_88.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_89.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_91.png)

---

![](img/857eeae4edbb93a1b757e58abf1783b4_93.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_94.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_95.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_96.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_98.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_99.png)

## 总结 📚

![](img/857eeae4edbb93a1b757e58abf1783b4_101.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_103.png)

本节课中我们一起学习了两种基于决策树的集成学习算法。

![](img/857eeae4edbb93a1b757e58abf1783b4_105.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_106.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_108.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_109.png)

1.  **Bagging**：通过自助采样生成多个训练集，分别训练决策树后聚合其预测。其核心是利用平均来降低模型方差，并且天然提供了袋外误差估计这一高效的模型评估工具。
2.  **随机森林**：在Bagging的基础上，通过在树的每个分裂节点随机选择特征子集，来进一步降低树与树之间的相关性，从而获得比普通Bagging更好的预测性能，尤其是在高维数据上。

![](img/857eeae4edbb93a1b757e58abf1783b4_111.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_112.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_113.png)

![](img/857eeae4edbb93a1b757e58abf1783b4_114.png)

这两种方法牺牲了单棵决策树的可解释性，但换来了预测准确性的巨大提升，使其成为处理复杂现实问题的强大工具。