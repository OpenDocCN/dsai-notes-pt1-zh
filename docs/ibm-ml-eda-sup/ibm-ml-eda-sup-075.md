# 075：正则化与模型选择 📊

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_1.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_2.png)

在本节课中，我们将要学习正则化与模型选择。正则化是确保模型在偏差与方差之间取得平衡的关键技术之一。我们将探讨模型复杂度与误差的关系，介绍几种主流的正则化方法，并了解如何通过特征选择来优化模型性能。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_4.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_5.png)

## 模型复杂度与误差回顾 📈

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_6.png)

上一节我们介绍了构建模型时，我们希望训练误差和测试误差都尽可能小。下图展示了测试集误差（上方曲线）随模型复杂度增加而上升的趋势，这是因为模型过度拟合训练数据，导致泛化能力下降。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_8.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_9.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_10.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_11.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_1.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_2.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_13.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_14.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_15.png)

处理复杂度与误差关系的一种方法是直接使用更简单的模型。另一种方法则是使用正则化，它能在不改变模型基本结构的前提下，降低其复杂度。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_17.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_18.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_19.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_20.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_21.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_17.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_18.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_19.png)

## 什么是正则化？🔍

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_20.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_21.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_23.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_24.png)

在机器学习中，模型通过最小化某个成本函数来从数据中学习参数。以线性回归为例，其成本函数是均方误差（MSE），即预测值与实际值之差的平方的均值。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_26.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_27.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_29.png)

**公式：** `MSE = (1/n) * Σ(y_i - ŷ_i)^2`

正则化通过修改这个成本函数来实现。新的成本函数由原始成本函数 `M(W)` 加上一个正则化项 `λ * R(W)` 构成。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_31.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_32.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_33.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_35.png)

**公式：** `J(W) = M(W) + λ * R(W)`

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_36.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_38.png)

其中：
*   `λ` 是正则化强度参数。
*   `R(W)` 是权重参数 `W` 的某个函数。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_40.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_41.png)

正则化项 `λ * R(W)` 的作用是对模型复杂度进行惩罚。权重参数越强，惩罚就越大。由于我们最终要最小化整个成本函数 `J(W)`，这就会促使模型在拟合数据的同时，也保持较小的权重，从而降低模型复杂度，防止过拟合。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_43.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_44.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_43.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_44.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_45.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_46.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_47.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_48.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_45.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_46.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_47.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_48.png)

## 正则化强度参数 λ 的作用 ⚖️

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_50.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_51.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_52.png)

正则化强度参数 `λ` 让我们能够精细地管理模型的复杂度权衡。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_54.png)

*   **增大 λ**：意味着对较大的权重施加更重的惩罚，这会使模型变得更简单（偏差增加）。
*   **减小 λ**：惩罚变轻，模型可以变得更复杂（方差增加）。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_56.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_57.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_58.png)

如果模型出现过拟合（表现为训练误差很低，但测试误差很高），增加正则化强度可以帮助改善模型的泛化误差，降低方差。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_60.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_61.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_63.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_65.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_66.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_67.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_69.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_71.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_72.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_65.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_66.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_67.png)

## 正则化与特征选择 🎯

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_69.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_71.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_72.png)

正则化本质上也是一种特征选择的形式，因为它会评估每个特征的贡献度，并通过惩罚项来削弱或消除不重要的特征。这在 Lasso 回归中体现得最为明显，Lasso 会将某些特征的系数直接压缩至零，等效于在建模前手动移除了这些特征。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_77.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_78.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_80.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_81.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_82.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_74.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_75.png)

除了使用内置正则化的模型（如 Ridge 或 Lasso），另一种进行特征选择的方法是递归特征消除（RFE）。以下是其基本思路：

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_77.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_78.png)

通过交叉验证，逐一移除特征并评估模型在验证集上的性能。如果移除某个特征后模型性能没有下降甚至有所提升，就说明该特征可能不重要，可以安全移除。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_80.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_81.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_82.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_90.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_91.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_92.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_93.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_94.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_95.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_84.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_85.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_86.png)

## 为何要进行特征选择？🤔

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_88.png)

你可能会认为特征越多模型越好，但事实并非总是如此。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_90.png)

并非所有特征都是相关的。例如，在预测客户流失时，客户姓名可能没有预测价值，但它可能让模型在训练集上“记住”噪声，导致过拟合，从而在新数据上表现不佳。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_91.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_92.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_93.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_94.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_95.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_100.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_101.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_102.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_103.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_104.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_97.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_98.png)

特征选择除了能减少过拟合，还有以下好处：

以下是进行特征选择的主要优势：
*   **提升效率**：减少特征数量可以缩短模型训练时间。
*   **改善性能**：对于没有内置正则化的模型，移除无关特征能直接提升结果。
*   **增强可解释性**：识别出最重要的特征有助于理解模型决策，这通常是重要的业务需求。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_100.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_101.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_102.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_103.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_104.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_106.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_107.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_109.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_111.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_112.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_114.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_115.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_116.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_106.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_107.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_109.png)

## 总结 📝

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_111.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_112.png)

本节课我们一起学习了正则化与模型选择的核心概念。我们回顾了模型复杂度与误差的关系，理解了正则化通过向成本函数添加惩罚项来防止过拟合的原理，并认识了正则化强度参数 `λ` 如何控制偏差与方差的权衡。此外，我们还探讨了正则化作为一种自动特征选择的手段，以及手动进行特征选择（如递归特征消除）的价值和原因。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_114.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_115.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_116.png)

正则化是构建稳健、泛化能力强的机器学习模型的关键工具。在接下来的课程中，我们将深入探讨 Ridge 回归等具体的正则化方法。

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_118.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_119.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_120.png)

![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_118.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_119.png)
![](img/a3c6e82b411d94cb438e55b9a0ed9dbe_120.png)