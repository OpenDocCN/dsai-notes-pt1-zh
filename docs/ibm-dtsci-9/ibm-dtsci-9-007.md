# 007：简单线性回归 📈

![](img/5cc5fa07698be71068f7dc9ab34ea734_1.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_2.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_4.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_5.png)

在本节课中，我们将学习线性回归的基本概念，这是一种用于预测连续值的机器学习方法。我们将通过一个汽车二氧化碳排放预测的实例，理解简单线性回归的工作原理、如何找到最佳拟合线，以及如何使用该模型进行预测。

![](img/5cc5fa07698be71068f7dc9ab34ea734_7.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_8.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_9.png)

---

![](img/5cc5fa07698be71068f7dc9ab34ea734_11.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_12.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_14.png)

## 概述

![](img/5cc5fa07698be71068f7dc9ab34ea734_15.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_16.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_18.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_19.png)

线性回归是一种用于描述两个或多个变量之间线性关系的模型。它通过拟合一条直线（或超平面）来预测一个连续的因变量（目标值）。本节课我们将重点学习**简单线性回归**，即只使用一个自变量来预测因变量的情况。

![](img/5cc5fa07698be71068f7dc9ab34ea734_21.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_22.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_23.png)

---

## 什么是线性回归？

![](img/5cc5fa07698be71068f7dc9ab34ea734_25.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_26.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_27.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_28.png)

线性回归是一种近似线性模型，用于描述变量之间的关系。在简单线性回归中，存在两个变量：
*   **因变量 (Dependent Variable)**：我们想要预测的值，必须是连续值。
*   **自变量 (Independent Variable)**：用于预测因变量的值，可以是连续值或分类值。

![](img/5cc5fa07698be71068f7dc9ab34ea734_30.png)

**核心公式**：`y_hat = θ₀ + θ₁ * x₁`
*   `y_hat` 是预测值。
*   `x₁` 是自变量。
*   `θ₀` 是截距。
*   `θ₁` 是斜率（或梯度）。

---

![](img/5cc5fa07698be71068f7dc9ab34ea734_32.png)

## 线性回归的类型

![](img/5cc5fa07698be71068f7dc9ab34ea734_34.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_35.png)

线性回归模型主要分为两种：

![](img/5cc5fa07698be71068f7dc9ab34ea734_37.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_38.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_39.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_41.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_42.png)

以下是两种主要类型：
*   **简单线性回归**：使用一个自变量来估计因变量。例如，使用发动机排量预测二氧化碳排放量。
*   **多元线性回归**：使用多个自变量来估计因变量。例如，使用发动机排量和气缸数预测二氧化碳排放量。

![](img/5cc5fa07698be71068f7dc9ab34ea734_44.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_46.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_47.png)

本节课我们专注于简单线性回归。

![](img/5cc5fa07698be71068f7dc9ab34ea734_49.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_51.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_52.png)

---

![](img/5cc5fa07698be71068f7dc9ab34ea734_54.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_55.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_57.png)

## 线性回归如何工作？

![](img/5cc5fa07698be71068f7dc9ab34ea734_58.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_60.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_62.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_63.png)

为了理解线性回归，我们可以将数据可视化。以发动机排量作为自变量（x轴），排放量作为因变量（y轴）绘制散点图。

![](img/5cc5fa07698be71068f7dc9ab34ea734_65.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_66.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_67.png)

散点图可以清晰地展示变量之间的关系。如果数据点大致呈线性分布，我们就可以尝试用一条直线来拟合它们。这条直线就是我们的回归模型，可以用来预测新数据点的值。

![](img/5cc5fa07698be71068f7dc9ab34ea734_69.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_70.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_71.png)

上一节我们介绍了线性回归的基本概念，本节中我们来看看如何找到这条“最佳拟合线”。

![](img/5cc5fa07698be71068f7dc9ab34ea734_72.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_74.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_75.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_76.png)

---

![](img/5cc5fa07698be71068f7dc9ab34ea734_78.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_79.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_80.png)

## 寻找最佳拟合线

![](img/5cc5fa07698be71068f7dc9ab34ea734_82.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_83.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_85.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_86.png)

最佳拟合线的目标是使模型预测值与实际值之间的误差最小。这个误差也称为**残差**，即数据点到拟合回归线的垂直距离。

![](img/5cc5fa07698be71068f7dc9ab34ea734_88.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_90.png)

我们的目标是使所有数据点的残差平均值最小。在数学上，这通常通过最小化**均方误差**来实现。

![](img/5cc5fa07698be71068f7dc9ab34ea734_92.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_93.png)

**均方误差公式**：`MSE = (1/n) * Σ(y_i - y_hat_i)²`
*   `n` 是数据点的数量。
*   `y_i` 是第 i 个实际值。
*   `y_hat_i` 是第 i 个预测值。

![](img/5cc5fa07698be71068f7dc9ab34ea734_95.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_97.png)

线性回归的目标就是找到参数 `θ₀` 和 `θ₁`，使得 MSE 最小化。

![](img/5cc5fa07698be71068f7dc9ab34ea734_99.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_100.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_101.png)

---

![](img/5cc5fa07698be71068f7dc9ab34ea734_103.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_105.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_106.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_107.png)

## 如何计算参数 θ₀ 和 θ₁？

![](img/5cc5fa07698be71068f7dc9ab34ea734_109.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_110.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_111.png)

我们可以使用数学公式直接根据数据计算最佳拟合线的参数 `θ₀`（截距）和 `θ₁`（斜率）。

![](img/5cc5fa07698be71068f7dc9ab34ea734_113.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_114.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_116.png)

计算步骤如下：
1.  计算自变量 `x` 的平均值 `x̄` 和因变量 `y` 的平均值 `ȳ`。
2.  使用以下公式计算斜率 `θ₁`：
    `θ₁ = Σ((x_i - x̄) * (y_i - ȳ)) / Σ((x_i - x̄)²)`
3.  使用以下公式计算截距 `θ₀`：
    `θ₀ = ȳ - θ₁ * x̄`

![](img/5cc5fa07698be71068f7dc9ab34ea734_118.png)

> **注意**：在实际应用中，我们通常使用Python（如scikit-learn库）、R或Scala等工具来自动计算这些参数，但理解其背后的原理非常重要。

通过计算，我们可能得到类似 `θ₀ = 125.74`， `θ₁ = 39` 的结果。那么我们的线性模型就是：`CO2排放量 = 125.74 + 39 * 发动机排量`。

---

## 使用模型进行预测

![](img/5cc5fa07698be71068f7dc9ab34ea734_120.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_122.png)

找到线性方程的参数后，对新数据进行预测就变得非常简单，只需将自变量的值代入方程即可。

![](img/5cc5fa07698be71068f7dc9ab34ea734_124.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_125.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_127.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_128.png)

例如，使用我们得到的模型 `CO2排放量 = 125 + 39 * 发动机排量`，预测一辆发动机排量为2.4的汽车的排放量：
`预测排放量 = 125 + 39 * 2.4 = 218.6`

![](img/5cc5fa07698be71068f7dc9ab34ea734_130.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_131.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_133.png)

---

![](img/5cc5fa07698be71068f7dc9ab34ea734_134.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_136.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_137.png)

## 线性回归的优点

![](img/5cc5fa07698be71068f7dc9ab34ea734_139.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_140.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_142.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_143.png)

线性回归之所以被广泛使用，有以下几个原因：
*   **简单易懂**：模型原理直观，易于理解和解释。
*   **计算高效**：训练和预测速度通常很快。
*   **无需复杂调参**：不像K近邻需要选择K值，或神经网络需要调整学习率，线性回归的参数可以通过数学方法直接计算。

![](img/5cc5fa07698be71068f7dc9ab34ea734_145.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_146.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_148.png)

---

![](img/5cc5fa07698be71068f7dc9ab34ea734_150.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_151.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_152.png)

## 总结

![](img/5cc5fa07698be71068f7dc9ab34ea734_154.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_155.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_157.png)

![](img/5cc5fa07698be71068f7dc9ab34ea734_158.png)

本节课我们一起学习了简单线性回归。我们了解了它如何通过一个自变量来预测连续的因变量，掌握了最佳拟合线的概念及其代表方程 `y_hat = θ₀ + θ₁ * x₁`。我们探讨了通过最小化均方误差来寻找最佳参数 `θ₀` 和 `θ₁` 的原理，并学习了如何使用最终的线性模型对新数据进行预测。线性回归因其简单、快速和可解释性强，成为机器学习中一个基础且重要的工具。