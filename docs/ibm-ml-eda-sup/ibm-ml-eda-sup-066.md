# 066：17_多项式回归 📈

![](img/14ccea59bfed89581b1ac27dbcb9e41c_1.png)

在本节课中，我们将学习如何扩展线性回归模型，通过引入多项式特征来捕捉数据中的非线性关系。我们将探讨如何在模型复杂度与泛化能力之间找到平衡，并了解这一框架如何应用于后续更复杂的机器学习模型。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_2.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_3.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_4.png)

---

![](img/14ccea59bfed89581b1ac27dbcb9e41c_6.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_7.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_8.png)

## 学习目标 🎯

上一节我们介绍了交叉验证的概念，本节中我们来看看如何通过多项式回归来调整模型的复杂度。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_10.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_11.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_12.png)

在本节中，我们将涵盖以下内容：
*   扩展线性回归，以便构建更复杂或更简单的模型，从而在交叉验证时找到给定留出集下的最佳复杂度。
*   使用多项式特征来捕捉非线性效应。这样，我们仍然可以使用线性回归这个线性模型，来对非线性关系进行建模。
*   简要介绍其他可用于回归和分类的模型。我们不会在此深入讲解，因为我们只讨论机器学习中寻找过拟合与欠拟合平衡的框架。但这里学到的所有内容，将适用于本系列后续课程中学习的每一个模型。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_13.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_15.png)

---

![](img/14ccea59bfed89581b1ac27dbcb9e41c_16.png)

## 创建新特征：多项式回归 🔧

![](img/14ccea59bfed89581b1ac27dbcb9e41c_18.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_19.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_20.png)

除了标准化缩放器或其他我们做过的预处理步骤外，我们拥有的另一个预处理技巧是：基于已有特征创建新特征。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_21.png)

我们在之前的过拟合讨论以及上一门课程的多项式讨论中已经见过这种方法。例如，在预算与票房的关系中，我们可以在模型中加入预算的平方项，从而建立预算与票房之间的非线性关系。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_23.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_24.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_25.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_26.png)

以下是创建多项式特征的核心步骤：
1.  **导入工具**：从 `sklearn.preprocessing` 导入 `PolynomialFeatures`。
2.  **创建实例**：初始化一个 `PolynomialFeatures` 对象，并设置 `degree` 参数（例如设为2）。
3.  **拟合并转换**：使用 `.fit_transform()` 方法，用原始数据拟合该对象并生成新的多项式特征。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_28.png)

```python
from sklearn.preprocessing import PolynomialFeatures

![](img/14ccea59bfed89581b1ac27dbcb9e41c_29.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_30.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_32.png)

# 创建多项式特征转换器，设置多项式阶数为2
poly_feat = PolynomialFeatures(degree=2)
# 拟合并转换数据
X_poly = poly_feat.fit_transform(X)
```

![](img/14ccea59bfed89581b1ac27dbcb9e41c_34.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_35.png)

生成的算法仍然是线性回归，因为结果仍然是特征的线性组合，只不过这些特征是我们将原始预算变量进行平方或立方等变换后得到的。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_37.png)

一个特征与另一个特征之间的非线性关系并不会使算法本身变成非线性。正如我们刚才所说，它仍然是我们现在创建的不同变量的线性组合。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_39.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_40.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_41.png)

---

## 特征的创造性组合 💡

![](img/14ccea59bfed89581b1ac27dbcb9e41c_43.png)

在创建这些额外特征时，我们可以发挥创造性，它们不仅限于二次方或三次方。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_44.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_45.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_46.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_47.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_48.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_49.png)

例如，我们可以创建一个新变量，它仅仅是特征一与特征二的乘积。正如你在Notebook中看到的，多项式回归默认会为我们创建这些交互项。

---

![](img/14ccea59bfed89581b1ac27dbcb9e41c_51.png)

## 如何选择正确的函数形式？ 🤔

![](img/14ccea59bfed89581b1ac27dbcb9e41c_52.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_53.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_54.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_55.png)

面对所有可能的变换、变量和组合，我们如何选择最有意义的那一个？

我们可以尝试一系列方法，并检查每个制造出来的特征与结果之间的相关性。我们在整个过程中创建新特征，并查看它们与结果的相关性，以决定是否应将它们纳入模型。理想情况下，我们会对哪些变量可能有效有一个预设的想法，并从此入手。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_57.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_58.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_59.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_60.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_61.png)

但只要特征数量不是多到难以处理，我们也可以创建所有可能的组合，并使用特征选择技术来查看，当我们后续进行交叉验证时，纳入某些交互项会产生积极还是消极的影响。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_63.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_64.png)

---

![](img/14ccea59bfed89581b1ac27dbcb9e41c_66.png)

## 多项式回归的优势与框架延伸 🌉

![](img/14ccea59bfed89581b1ac27dbcb9e41c_67.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_68.png)

通过添加多项式特征来调整标准的线性回归方法，是解决我们之前讨论的基本问题（即预测和解释）的众多方法之一。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_70.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_71.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_72.png)

当你创建这些多项式项时，你可能能够在留出集上获得更好的预测效果，因为你现在有了一个更复杂的模型，可能能够拟合实际图形的曲率。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_74.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_75.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_76.png)

同时，如果你添加了交互项，你也可能获得更好的解释性。例如，如果你将一个值与另一个值相乘，或将一个除以另一个（如人均人口或家庭数），你就能更好地理解每个系数的含义。

---

![](img/14ccea59bfed89581b1ac27dbcb9e41c_78.png)

## 通用评估框架 🔄

![](img/14ccea59bfed89581b1ac27dbcb9e41c_79.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_80.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_81.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_82.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_83.png)

随着我们进入模型评估阶段，需要记住，我们现在学习的同一框架可用于评估各种回归和分类问题。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_85.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_86.png)

我们可以尝试一系列方法并检查关系，如前所述。我们可以利用新特征来支持我们将在整个系列中讨论的许多新示例。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_88.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_89.png)

我们将被介绍逻辑回归、K近邻、决策树、支持向量机、随机森林、结合不同算法的集成方法，以及深度学习方法。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_90.png)

对于其中的每一个，我们都将遵循相同的理念：在模型复杂度和泛化能力、过拟合与欠拟合之间找到平衡，或者我们称之为**偏差-方差权衡**。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_92.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_93.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_94.png)

所有这些都将使用相同的变换逻辑来创建更复杂的特征。稍后，我们还将看到正则化，以确保无论使用什么模型，我们都能在留出集中找到复杂度与误差的正确平衡。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_96.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_97.png)

这里学习的理念是，我们正在学习所需的所有框架，以便你能够在后续课程中快速上手，准备好利用你数据科学工具箱中的所有高级工具。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_99.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_100.png)

---

![](img/14ccea59bfed89581b1ac27dbcb9e41c_102.png)

## 总结 📝

![](img/14ccea59bfed89581b1ac27dbcb9e41c_104.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_105.png)

本节课中我们一起学习了：
*   再次讨论了如何扩展线性回归，不仅通过多项式特征，还包括交互项。
*   讨论了如何使用多项式特征来捕捉非线性效应，并通过图表进行了回顾。
*   讨论了这如何成为寻找欠拟合与过拟合平衡的基本框架，并将延伸至本系列中我们将学习的所有更复杂的模型。

![](img/14ccea59bfed89581b1ac27dbcb9e41c_107.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_108.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_109.png)

![](img/14ccea59bfed89581b1ac27dbcb9e41c_110.png)

牢记这一点，我们将继续探讨这种偏差与方差的权衡，并更深入地理解为什么这是你在所有机器学习模型中都需要牢记的最重要的权衡。