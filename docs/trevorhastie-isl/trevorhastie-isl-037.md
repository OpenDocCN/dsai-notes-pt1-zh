# 37：岭回归与 Lasso 📊

![](img/2beab32f0a7a882b1b6e1154ba206066_0.png)

在本节课中，我们将要学习两种强大的回归方法：岭回归和 Lasso。这两种方法通过引入惩罚项来缩小回归系数，从而在模型拟合与复杂度之间取得平衡，尤其适用于变量数量庞大的数据集。

![](img/2beab32f0a7a882b1b6e1154ba206066_2.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_4.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_6.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_8.png)

上一节我们介绍了基于最小二乘法的模型选择方法，本节中我们来看看一种不同的方法——收缩法。

![](img/2beab32f0a7a882b1b6e1154ba206066_10.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_11.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_13.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_14.png)

## 岭回归简介

![](img/2beab32f0a7a882b1b6e1154ba206066_16.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_17.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_18.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_19.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_21.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_23.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_25.png)

岭回归与最小二乘法不同，它在最小化残差平方和的基础上，增加了一个惩罚项。这个惩罚项会促使系数向零收缩。

![](img/2beab32f0a7a882b1b6e1154ba206066_27.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_28.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_30.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_32.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_33.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_34.png)

最小二乘法的目标是最小化残差平方和：
`RSS = Σ(y_i - ŷ_i)^2`

![](img/2beab32f0a7a882b1b6e1154ba206066_35.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_37.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_39.png)

岭回归则最小化以下目标函数：
`RSS + λ * Σ(β_j)^2`

![](img/2beab32f0a7a882b1b6e1154ba206066_41.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_43.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_45.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_46.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_48.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_49.png)

其中，λ 是一个调节参数，通过交叉验证等方法选择。λ 越大，对非零系数的惩罚越大，系数越被压缩向零。

![](img/2beab32f0a7a882b1b6e1154ba206066_51.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_53.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_55.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_57.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_59.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_60.png)

## 岭回归的工作原理

![](img/2beab32f0a7a882b1b6e1154ba206066_62.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_64.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_65.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_67.png)

当 λ = 0 时，岭回归退化为普通最小二乘法。随着 λ 增大，惩罚项的影响力增强，系数被持续压缩。如果 λ 极大，所有系数都将趋近于零。

![](img/2beab32f0a7a882b1b6e1154ba206066_69.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_70.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_71.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_72.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_73.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_75.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_77.png)

以下是岭回归的核心特点：
*   **收缩效应**：惩罚项促使系数向零收缩。
*   **调节参数 λ**：控制拟合优度与系数大小之间的权衡。
*   **变量标准化**：由于惩罚项对系数大小敏感，在应用岭回归前，通常需要将预测变量标准化，使其具有可比性。

![](img/2beab32f0a7a882b1b6e1154ba206066_78.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_80.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_81.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_82.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_83.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_84.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_86.png)

## 岭回归示例分析

![](img/2beab32f0a7a882b1b6e1154ba206066_88.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_90.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_92.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_93.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_95.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_97.png)

以信用数据为例，我们可以绘制系数随 λ 变化的路径图。

![](img/2beab32f0a7a882b1b6e1154ba206066_99.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_100.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_102.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_104.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_106.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_108.png)

在 λ 接近 0 的左侧，我们得到几乎完整的最小二乘估计。随着 λ 增大，系数被推向零。当 λ 极大时，所有系数基本为零。

![](img/2beab32f0a7a882b1b6e1154ba206066_110.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_111.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_112.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_113.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_115.png)

另一种常见的绘图方式是展示标准化系数相对于其 L2 范数的变化。L2 范数定义为：
`||β||_2 = sqrt(Σ β_j^2)`

![](img/2beab32f0a7a882b1b6e1154ba206066_116.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_117.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_119.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_121.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_123.png)

在这种图形中，右侧对应 λ=0（最小二乘估计），左侧对应 λ 极大（系数为零）。

![](img/2beab32f0a7a882b1b6e1154ba206066_125.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_127.png)

## 岭回归的偏差-方差权衡

![](img/2beab32f0a7a882b1b6e1154ba206066_129.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_130.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_132.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_134.png)

在一个模拟示例中（45个预测变量，50个观测值），我们可以观察岭回归的偏差、方差和测试误差。

![](img/2beab32f0a7a882b1b6e1154ba206066_136.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_137.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_139.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_141.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_142.png)

随着 λ 从 0 开始增大，偏差基本不变，但方差显著下降。均方误差（偏差与方差之和）呈现 U 形曲线，并在某个中间 λ 值处达到最小。这表明，通过适当收缩系数，岭回归可以降低测试误差，优于普通最小二乘法。

![](img/2beab32f0a7a882b1b6e1154ba206066_144.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_146.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_147.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_149.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_151.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_153.png)

## 从岭回归到 Lasso

![](img/2beab32f0a7a882b1b6e1154ba206066_155.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_157.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_158.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_159.png)

观察岭回归的系数路径图，你可能会注意到一个特点：系数虽然会变得非常小，但**几乎永远不会精确等于零**。这意味着模型不会进行真正的变量选择，所有变量始终保留在模型中。

![](img/2beab32f0a7a882b1b6e1154ba206066_161.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_163.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_165.png)

对于那些系数很小的变量，如果能让它们精确为零，模型会更简洁、更容易解释。这便引出了我们下一个要介绍的方法——Lasso。Lasso 通过修改惩罚项的形式，能够将某些系数精确地收缩至零，从而实现变量选择。

![](img/2beab32f0a7a882b1b6e1154ba206066_167.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_168.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_169.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_170.png)

![](img/2beab32f0a7a882b1b6e1154ba206066_171.png)

本节课中我们一起学习了岭回归的基本原理。我们了解到，岭回归通过在损失函数中加入系数的 L2 惩罚项，来缩小系数、控制模型方差，并在偏差与方差之间取得平衡，从而可能提升模型预测性能。同时，我们也看到了岭回归在变量选择上的局限性，这为学习 Lasso 方法做好了铺垫。