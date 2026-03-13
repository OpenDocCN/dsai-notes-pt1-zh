# 085：正则化深入解析（第二部分）📊

![](img/9f31ba5dad2834e3a700b87acadd73c1_1.png)

在本节课中，我们将从概率论的视角深入探讨正则化，理解岭回归（Ridge）和套索回归（Lasso）背后的先验分布假设，并总结正则化的核心目标与不同理解方式。

## 概述：从概率视角看正则化

上一节我们从几何角度理解了正则化，本节我们将从概率论的贝叶斯视角来审视它。正则化本质上是对回归系数施加了某种先验分布。

![](img/9f31ba5dad2834e3a700b87acadd73c1_3.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_4.png)

我们试图找出回归系数，并假设这些系数服从某个先验分布。我们将基于数据来更新这个先验，得到系数的后验分布。

![](img/9f31ba5dad2834e3a700b87acadd73c1_5.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_1.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_7.png)

这意味着，在建立系数与X、Y值之间的实际关系之前，我们已对系数的分布有了一定的先验认知。

![](img/9f31ba5dad2834e3a700b87acadd73c1_7.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_9.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_11.png)

最终，我们的目标是在给定X和Y数据的情况下，找到最优的系数。

## 贝叶斯框架下的正则化

![](img/9f31ba5dad2834e3a700b87acadd73c1_13.png)

在贝叶斯框架下，这个问题可以重新表述为：在给定系数 `β` 的情况下，得到结果变量 `Y` 的概率。

![](img/9f31ba5dad2834e3a700b87acadd73c1_15.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_9.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_16.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_17.png)

乘以我们对 `β` 的先验理解。

![](img/9f31ba5dad2834e3a700b87acadd73c1_19.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_11.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_21.png)

换句话说，如果我们试图找到给定数据下系数的概率，可以使用贝叶斯公式重新校准。

![](img/9f31ba5dad2834e3a700b87acadd73c1_22.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_13.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_24.png)

这样，我们就是在寻找在给定一组参数（即系数）的情况下得到目标值的概率，乘以我们系数的先验分布。

![](img/9f31ba5dad2834e3a700b87acadd73c1_26.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_15.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_16.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_17.png)

因此，我们的解决方案将取决于如何定义系数所服从的先验分布。

![](img/9f31ba5dad2834e3a700b87acadd73c1_19.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_28.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_29.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_30.png)

## L2与L1正则化的先验分布

![](img/9f31ba5dad2834e3a700b87acadd73c1_31.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_32.png)

当我们选择正则化的形式时，无论是岭回归还是套索回归，本质上都是在定义那个先验分布函数 `G`。

![](img/9f31ba5dad2834e3a700b87acadd73c1_34.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_21.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_22.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_36.png)

以下是两种正则化对应的先验分布：

![](img/9f31ba5dad2834e3a700b87acadd73c1_38.png)

*   对于L2正则化（岭回归），我们对每个系数施加一个**高斯先验（正态分布）**。我们假设系数是从正态分布中抽取的。
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_24.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_26.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_39.png)

*   对于L1正则化（套索回归），我们施加一个**拉普拉斯先验**。我们将在下一张幻灯片中看到这些分布的具体形态。核心思想同样是，我们的每个系数 `β` 都可能是从特定分布中抽取的。我们根据使用L2还是L1正则化，对每个 `β` 施加不同的先验分布。
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_28.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_29.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_30.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_31.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_32.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_41.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_42.png)

## 先验分布的行为差异

![](img/9f31ba5dad2834e3a700b87acadd73c1_44.png)

观察这些先验分布再次揭示了它们行为上的差异。

![](img/9f31ba5dad2834e3a700b87acadd73c1_46.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_34.png)

对于两者，我们都施加了一个以零为中心的分布。这意味着我们认为零是系数最可能的值。

![](img/9f31ba5dad2834e3a700b87acadd73c1_48.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_49.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_36.png)

但是，套索回归在零处有一个更尖锐、更高的峰值，因为它更倾向于将某些系数完全设为零。

![](img/9f31ba5dad2834e3a700b87acadd73c1_51.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_52.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_38.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_39.png)

## 超参数 λ 与先验分布方差

![](img/9f31ba5dad2834e3a700b87acadd73c1_54.png)

现在，将这一点与我们引入的超参数 `λ` 联系起来。`λ` 实际上意味着先验分布的**方差更小**。

![](img/9f31ba5dad2834e3a700b87acadd73c1_55.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_57.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_41.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_42.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_44.png)

如果你想象这个分布被压缩，方差变小意味着值更可能接近零。

![](img/9f31ba5dad2834e3a700b87acadd73c1_59.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_46.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_60.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_61.png)

因此，一个更高的 `λ` 值（它缩小了方差）意味着系数更小，因为这些系数更可能接近零。

![](img/9f31ba5dad2834e3a700b87acadd73c1_48.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_49.png)

## 正则化核心要点总结 🎯

现在，让我们回顾一下关于正则化我们所理解的一切。正则化的目标始终是优化复杂度权衡，以便在留出数据集或外部数据集上最小化误差。

![](img/9f31ba5dad2834e3a700b87acadd73c1_63.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_64.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_65.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_51.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_52.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_66.png)

以下是实现这一目标的关键点：

![](img/9f31ba5dad2834e3a700b87acadd73c1_68.png)

我们通过在成本函数中施加惩罚来降低模型复杂度。我们可以调整正则化项 `λ` 的值，以改变惩罚力度，并微调我们想要减少多少复杂度。

![](img/9f31ba5dad2834e3a700b87acadd73c1_54.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_55.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_70.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_71.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_72.png)

引入正则化会增加偏差，但会减少方差。这个权衡通常是值得的。我们常常发现初始模型对微小变化过于敏感，因此增加偏差、减少方差有助于我们找到一个更平衡、更具泛化能力的模型。

![](img/9f31ba5dad2834e3a700b87acadd73c1_57.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_59.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_60.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_61.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_74.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_75.png)

我们有两种主要选择：岭回归（Ridge/L2）或套索回归（Lasso/L1）。我们可以验证模型的选择以及 `λ` 值的强度。我们在 notebook 中看到过如何遍历超参数或模型的不同可能解。我们也讨论过一些优缺点，例如套索回归的特征选择和可解释性增强，以及岭回归如何更严厉地惩罚异常值，并且通常比套索回归运行得更快。

![](img/9f31ba5dad2834e3a700b87acadd73c1_63.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_64.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_65.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_66.png)

## 正则化的终极目标

正则化的目标始终是关于找到正确的偏差-方差权衡。

![](img/9f31ba5dad2834e3a700b87acadd73c1_68.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_77.png)

我们需要一个模型，它足够复杂以捕捉 X 和 Y 之间的真实关系（偏差不能太高），但也不能复杂到对训练集过拟合（方差不能太高）。

![](img/9f31ba5dad2834e3a700b87acadd73c1_78.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_79.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_80.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_70.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_71.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_72.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_81.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_82.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_83.png)

我们不能让方差太高，以至于 X 和 Y 之间呈现一种完美的关系，X 的微小变化就会对结果变量 Y 产生巨大影响。

![](img/9f31ba5dad2834e3a700b87acadd73c1_84.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_85.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_74.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_75.png)

## 理解正则化的三种途径

![](img/9f31ba5dad2834e3a700b87acadd73c1_87.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_88.png)

为了清晰地理解这一切是如何运作的，我们讨论了三种途径：

![](img/9f31ba5dad2834e3a700b87acadd73c1_89.png)

1.  **解析途径**：我们知道更小的系数必然导致更简单的模型。`小系数 -> 低复杂度模型`。
2.  **几何途径**：它向我们展示了在正则化的约束下，我们如何找到最优解，以及这如何导致岭回归和套索回归产生不同的系数结果。回顾我们的几何示例，我们看到由于套索回归的约束区域有“尖角”，而岭回归的约束区域更“圆滑”，因此套索回归的解更可能落在坐标轴上，从而将系数设为零。
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_77.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_78.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_79.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_80.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_81.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_82.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_83.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_84.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_85.png)
3.  **概率途径**：在贝叶斯问题的框架下解释套索和岭回归，即我们对试图学习的系数分布施加了特定的先验。
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_87.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_88.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_89.png)
    ![](img/9f31ba5dad2834e3a700b87acadd73c1_91.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_91.png)

## 课程总结

![](img/9f31ba5dad2834e3a700b87acadd73c1_93.png)

本节课中，我们一起深入探讨了正则化背后的细节，以及我们可以通过不同途径来理解正则化实际如何工作。

![](img/9f31ba5dad2834e3a700b87acadd73c1_94.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_93.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_94.png)

正则化的概念将在你的整个机器学习旅程中至关重要，因为在几乎每一个模型中，我们都会使用某种形式的正则化，以在偏差和方差之间找到正确的平衡。

![](img/9f31ba5dad2834e3a700b87acadd73c1_96.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_97.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_96.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_97.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_98.png)
![](img/9f31ba5dad2834e3a700b87acadd73c1_99.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_98.png)

![](img/9f31ba5dad2834e3a700b87acadd73c1_99.png)

至此，第二门课程的内容结束。期待在第三门课程中与你相见，谢谢。