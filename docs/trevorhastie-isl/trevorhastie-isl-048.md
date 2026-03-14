# 48：平滑样条 📈

![](img/3df0cb1889f7c63565c820cc8cfbd79e_1.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_3.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_5.png)

在本节课中，我们将要学习一种强大的非参数回归方法——平滑样条。我们将了解其核心思想、数学原理、如何通过R语言实现，以及如何选择关键的平滑参数。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_7.png)

---

![](img/3df0cb1889f7c63565c820cc8cfbd79e_9.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_10.png)

## 概述与“危险信号”⚠️

在Trevor Hastie和Robert Tibshirani的《统计学习导论》中，当内容变得更具挑战性时，书中会出现一个“危险信号”图标。平滑样条在数学上确实比之前的内容更复杂一些，但它同时也非常优美和强大。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_12.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_14.png)

上一节我们介绍了需要手动选择节点的回归样条，本节中我们来看看平滑样条，它提供了一种无需担心节点选择的拟合方法。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_16.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_17.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_19.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_20.png)

---

![](img/3df0cb1889f7c63565c820cc8cfbd79e_22.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_24.png)

## 平滑样条的核心思想 🎯

![](img/3df0cb1889f7c63565c820cc8cfbd79e_25.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_27.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_29.png)

平滑样条从一个完全不同的视角来解决问题。其核心是定义一个需要最小化的准则。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_31.png)

该准则包含两部分：
1.  **残差平方和**：这部分试图让函数 `g(x)` 在观测到的 `x` 点上尽可能接近观测值 `y`。
2.  **粗糙度惩罚项**：这部分约束我们搜索的函数必须是平滑的。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_33.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_34.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_35.png)

其数学形式如下：
```
最小化： Σ [yi - g(xi)]² + λ ∫ [g''(t)]² dt
```
其中：
*   `g(x)` 是我们要求解的未知函数。
*   `g''(t)` 是函数 `g` 的二阶导数。
*   `λ` 是一个非负的**调节参数**，称为粗糙度惩罚系数。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_37.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_39.png)

---

![](img/3df0cb1889f7c63565c820cc8cfbd79e_41.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_43.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_45.png)

### 理解惩罚项与参数 λ

二阶导数衡量的是函数的“弯曲”或“摆动”程度。惩罚项对函数在整个定义域内的总弯曲程度进行积分和平方（以消除正负号）。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_47.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_48.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_49.png)

参数 `λ` 控制着惩罚的力度：
*   当 **λ = 0** 时，惩罚项失效。优化过程会找到一个穿过所有数据点的插值函数（可能非常曲折）。
*   当 **λ → ∞** 时，惩罚项变得无穷大。为了最小化准则，函数必须使得 `g''(t) = 0`，这意味着 `g(x)` 必须是一个**线性函数**。

因此，通过让 `λ` 从 0 变化到无穷大，我们可以得到一系列从完全拟合数据的曲折函数到简单线性函数的解。这个问题的解就被称为**平滑样条**。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_51.png)

---

![](img/3df0cb1889f7c63565c820cc8cfbd79e_53.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_55.png)

## 平滑样条的性质与实现 🔧

![](img/3df0cb1889f7c63565c820cc8cfbd79e_57.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_58.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_59.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_60.png)

令人惊讶的是，上述优化问题的解是一个样条函数，并且是一个在**每个唯一的 `x` 观测值处都有一个节点**的自然三次样条。这听起来很夸张，但由于粗糙度惩罚项的存在，它并不会过度拟合。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_62.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_63.png)

以下是关于平滑样条实现的一些关键点：

![](img/3df0cb1889f7c63565c820cc8cfbd79e_65.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_67.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_69.png)

*   **避免节点选择**：最大的优点是无需选择节点位置，只需要选择参数 `λ`。
*   **R语言实现**：在R中，我们可以使用 `smooth.spline()` 函数轻松拟合平滑样条。
*   **线性平滑器**：拟合值可以写成 `ĝ = Sλ * y` 的形式，其中 `Sλ` 是一个由 `x` 的位置和 `λ` 值决定的 `n x n` 矩阵。这意味着平滑样条和线性回归一样，是一个**线性平滑器**。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_71.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_72.png)

---

![](img/3df0cb1889f7c63565c820cc8cfbd79e_74.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_75.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_77.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_78.png)

### 有效自由度

![](img/3df0cb1889f7c63565c820cc8cfbd79e_80.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_82.png)

虽然理论上节点数很多，但由于惩罚项的限制，模型的复杂度实际低得多。我们可以用**有效自由度**来衡量其复杂度。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_84.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_85.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_87.png)

有效自由度的计算公式为：
```
dfλ = trace(Sλ)
```
即平滑矩阵 `Sλ` 的迹（对角线元素之和）。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_89.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_90.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_91.png)

在实践中，我们可以通过指定期望的**有效自由度**来反向确定 `λ` 的值。例如，在R中调用 `smooth.spline(age, wage, df=10)` 会拟合一个复杂度大约相当于10阶多项式的平滑曲线，但其光滑度分布更加合理。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_93.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_95.png)

---

![](img/3df0cb1889f7c63565c820cc8cfbd79e_96.png)

## 选择平滑参数：交叉验证 🔍

![](img/3df0cb1889f7c63565c820cc8cfbd79e_98.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_99.png)

如果我们不想手动指定自由度，可以使用交叉验证来自动选择最优的 `λ`。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_101.png)

对于平滑样条，留一法交叉验证的残差平方和有一个非常优雅的计算公式：
```
CV(λ) = Σ [ (yi - ĝλ(xi)) / (1 - Sλ[ii]) ]²
```
其中 `Sλ[ii]` 是平滑矩阵 `S` 的第 `i` 个对角线元素。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_103.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_104.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_105.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_106.png)

这个公式的妙处在于，我们**无需真正进行n次拟合**（每次剔除一个数据点）。只需用全部数据拟合一次模型，就能通过上述公式计算出交叉验证误差。在R的 `smooth.spline()` 函数中，如果不指定 `λ` 或 `df`，默认就会使用交叉验证自动选择参数。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_108.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_109.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_111.png)

---

![](img/3df0cb1889f7c63565c820cc8cfbd79e_113.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_114.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_116.png)

### 一个例子

![](img/3df0cb1889f7c63565c820cc8cfbd79e_118.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_119.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_120.png)

在实际应用中，手动设定高自由度（如 `df=16`）的平滑样条与由交叉验证自动选择的样条（结果可能对应 `df=6.8`）可能看起来非常相似。请注意，有效自由度可以是非整数，这反映了模型复杂度的连续变化。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_122.png)

---

![](img/3df0cb1889f7c63565c820cc8cfbd79e_124.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_125.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_126.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_128.png)

## 总结 📝

![](img/3df0cb1889f7c63565c820cc8cfbd79e_130.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_132.png)

本节课中我们一起学习了平滑样条：
1.  它是一种通过**最小化“拟合误差+粗糙度惩罚”准则**来拟合非线性关系的方法。
2.  其核心参数 **λ** 在过拟合（曲折）与欠拟合（线性）之间进行权衡。
3.  它的解是一个**在每个数据点都有节点的自然三次样条**，但惩罚项控制了其实际复杂度。
4.  我们可以用**有效自由度**来直观理解模型复杂度，并可以通过**交叉验证**自动选择最优的 `λ`。
5.  在R中，可以方便地使用 `smooth.spline()` 函数来实现平滑样条拟合。

![](img/3df0cb1889f7c63565c820cc8cfbd79e_134.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_135.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_136.png)

![](img/3df0cb1889f7c63565c820cc8cfbd79e_137.png)

平滑样条提供了一种强大、优雅且自动化的非线性拟合工具，是统计学习工具箱中的重要组成部分。