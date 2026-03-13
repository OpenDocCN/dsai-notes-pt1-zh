# 070：超参数调优与交叉验证实践 🎯

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_0.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_2.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_4.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_5.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_7.png)

在本节课中，我们将学习如何使用交叉验证进行超参数调优，以优化机器学习模型的性能。我们将重点介绍超参数与模型参数的区别，并通过Lasso回归的实例演示如何通过调整超参数来控制模型复杂度，最终找到能最好地泛化到新数据的模型配置。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_9.png)

## 超参数与模型参数 📊

上一节我们介绍了交叉验证的基本概念。本节中，我们来看看如何利用交叉验证来优化模型。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_11.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_12.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_13.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_14.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_15.png)

首先，我们需要区分超参数和模型参数。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_16.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_17.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_19.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_20.png)

*   **超参数** 是由我们用户手动调整的模型组成部分。例如，Lasso回归中的 `alpha` 值。
*   **模型参数** 是模型通过机器学习算法从数据中自动学习得到的值。例如，线性回归中的系数。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_22.png)

## 超参数调优与模型复杂度 🔧

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_24.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_25.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_26.png)

那么，我们如何找到能优化模型性能的超参数呢？

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_28.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_29.png)

我们通过 **超参数调优** 来实现，这个过程会用到 **交叉验证**。通过多次划分训练集和测试集，我们可以确定哪些超参数最有可能在未知样本上表现良好。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_31.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_32.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_33.png)

在理论部分我们见过模型复杂度与误差之间的关系曲线。我们的目标是找到曲线上的最佳点，即复杂度适中、在验证集上误差最小的位置。为此，我们需要测试许多不同的超参数，以观察哪个超参数能带来合适的复杂度水平。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_35.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_36.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_38.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_40.png)

一般来说，调整超参数会增加或降低模型的复杂度。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_41.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_43.png)

## 使用 `np.geomspace` 生成超参数值 📈

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_44.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_46.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_47.png)

接下来，我们快速介绍一个将用到的函数：`np.geomspace`。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_49.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_50.png)

`np.geomspace` 用于生成等比数列。例如，`np.geomspace(1, 1000, num=4)` 会生成4个值，每个值都是前一个值的固定倍数（这里是10倍）：1, 10, 100, 1000。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_51.png)

类似地，`np.geomspace(1, 27, num=4)` 会生成：1, 3, 9, 27，每个值是前一个值的3倍。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_53.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_54.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_55.png)

在本例中，我们将创建10个值，从 `1e-9`（即 0.000000001）开始，直到 1。这些值将作为Lasso回归中 `alpha` 参数的候选值。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_56.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_57.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_59.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_61.png)

## Lasso回归与超参数 `alpha` 🎯

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_63.png)

我们尚未详细介绍Lasso回归，目前你只需要知道：在Lasso模型中，改变 `alpha` 值会影响模型复杂度。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_65.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_66.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_68.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_69.png)

*   **`alpha` 值越高**，模型复杂度越低。
*   **`alpha` 值越低**，模型复杂度越高，越接近普通的线性回归。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_71.png)

当我们查看数据集中每个特征的实际系数输出时，这一点会更加清晰。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_73.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_74.png)

以下是实现超参数调优的核心步骤代码：

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_76.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_77.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_78.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_79.png)

```python
import numpy as np
from sklearn.linear_model import Lasso
from sklearn.preprocessing import PolynomialFeatures, StandardScaler
from sklearn.pipeline import Pipeline
from sklearn.model_selection import cross_val_predict
from sklearn.metrics import r2_score

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_81.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_82.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_83.png)

# 1. 定义超参数候选值（等比数列）
alphas = np.geomspace(1e-9, 1, num=10)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_85.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_87.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_88.png)

# 2. 初始化列表用于存储结果
scores = []
coefficients = []

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_90.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_91.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_92.png)

# 3. 遍历每个alpha值进行交叉验证
for alpha in alphas:
    # 创建Lasso模型实例，设置最大迭代次数以确保收敛
    lasso = Lasso(alpha=alpha, max_iter=100000)
    
    # 创建处理管道：先标准化数据，再应用Lasso回归
    estimator = Pipeline([("scaler", StandardScaler()),
                          ("lasso", lasso)])
    
    # 使用交叉验证获取预测值
    predictions = cross_val_predict(estimator, X, y, cv=5)
    
    # 计算R²分数并存储
    score = r2_score(y, predictions)
    scores.append(score)
    
    # 拟合模型以查看系数（注意：这里是在整个数据集上拟合，仅用于观察）
    estimator.fit(X, y)
    coefficients.append(estimator.named_steps['lasso'].coef_)
```

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_94.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_95.png)

运行上述代码后，我们可以查看每个 `alpha` 值对应的分数。从结果可以看出，从最复杂的模型（`alpha` 最小）到最简单的模型（`alpha` 最大），模型在验证集上的表现曲线相对平坦，说明在这个数据集上，单纯降低复杂度带来的收益有限。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_97.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_98.png)

为了更清楚地展示模型复杂度如何变化，我们可以观察系数。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_100.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_101.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_102.png)

*   对于很小的 `alpha` 值（复杂度降低很少），所有特征的系数都是不同的数值。
*   对于 `alpha=1` 这样较高的值（复杂度大大降低），许多系数变成了0。这些系数各自对应一个特征，系数为0意味着这些特征基本上被从预测模型中移除了。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_104.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_106.png)

绘制分数随 `alpha` 变化的图表，我们可以看到高复杂度与误差率之间的权衡。目前看来，使用标准缩放器的普通Lasso可能不需要太多的正则化。接下来，我们将看到当特征数量大幅增加时，情况会有所不同。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_108.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_110.png)

## 在管道中加入多项式特征 🔄

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_112.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_113.png)

在接下来的练习中，我们将在管道中加入多项式特征，然后重新运行交叉验证。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_114.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_116.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_117.png)

管道中的步骤按顺序执行。现在我们需要考虑，在管道中添加多项式特征的最佳顺序是什么。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_119.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_120.png)

这里存在一些讨论：
1.  **先标准化再添加多项式特征**：如果先标准化，原本全为正值的特征可能变为负值，这会改变所有交互项的含义。同时，特征值被缩放到更小的尺度（例如0.5和2），可能会削弱多项式项的效果。
2.  **先添加多项式特征再标准化**：这通常是推荐的最佳实践。这样做可以保持原始特征尺度生成的多项式项，然后再进行标准化，确保最终所有特征（包括新生成的多项式项）都在同一尺度上，便于模型处理。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_122.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_123.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_125.png)

我们将采用第二种方式：先进行多项式特征转换，再进行标准化。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_127.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_128.png)

以下是更新后的代码：

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_129.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_131.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_132.png)

```python
# 1. 初始化多项式特征转换器（这里设置度为2，即包含特征自身及两两交互项和平方项）
poly = PolynomialFeatures(degree=2)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_134.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_136.png)

# 2. 定义超参数候选值
alphas = np.geomspace(0.001, 1, num=10)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_138.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_140.png)

# 3. 初始化列表用于存储结果
scores_poly = []

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_142.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_143.png)

# 4. 遍历每个alpha值
for alpha in alphas:
    # 创建Lasso模型实例
    lasso = Lasso(alpha=alpha, max_iter=100000)
    
    # 更新管道：先多项式特征，再标准化，最后Lasso回归
    estimator = Pipeline([("poly", poly),
                          ("scaler", StandardScaler()),
                          ("lasso", lasso)])
    
    # 使用交叉验证获取预测值并计算分数
    predictions = cross_val_predict(estimator, X, y, cv=5)
    score = r2_score(y, predictions)
    scores_poly.append(score)
```

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_144.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_145.png)

运行这段代码可能需要一些时间（约30秒）。你可能会看到一个关于最大迭代次数的警告，但经过测试，结果已接近最优值。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_147.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_148.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_149.png)

## 分析结果与选择最佳模型 🏆

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_151.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_152.png)

代码运行完成后，我们可以查看每个 `alpha` 值对应的分数。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_154.png)

*   从非常小的 `alpha`（高复杂度）开始，模型可能泛化能力不佳。
*   当 `alpha` 略微升高时，泛化能力变得更好。
*   当 `alpha` 继续升高超过某个点后，性能可能达到峰值并开始下降。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_155.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_156.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_157.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_158.png)

观察图表，我们可能会发现 `alpha=0.01` 附近是超参数的最佳值，能够在新数据上获得良好的泛化性能。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_160.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_162.png)

找到最佳超参数后，我们可以用它来训练最终的模型：

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_164.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_166.png)

```python
# 使用找到的最佳alpha值构建最终模型管道
best_estimator = Pipeline([("poly", PolynomialFeatures(degree=2)),
                           ("scaler", StandardScaler()),
                           ("lasso", Lasso(alpha=0.01, max_iter=100000))])

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_168.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_169.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_171.png)

# 在整个训练集上拟合模型
best_estimator.fit(X, y)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_172.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_173.png)

# 查看模型在训练集上的R²分数（注意：这是在同一数据集上训练和评分，通常应使用保留测试集）
final_score = best_estimator.score(X, y)
print(f"模型在训练集上的R²分数为: {final_score}")

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_175.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_176.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_177.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_178.png)

# 查看最终模型的系数，由于使用了Lasso回归和alpha项，许多特征的系数被缩减为0
final_coefficients = best_estimator.named_steps['lasso'].coef_
print("非零系数的数量:", np.sum(final_coefficients != 0))
```

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_179.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_181.png)

当我们查看系数时，因为使用了Lasso回归和 `alpha` 正则项，实际上移除了许多特征——正如你所见，很多系数基本上被置为零。这体现了Lasso回归的特征选择能力。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_183.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_184.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_186.png)

## 总结 📝

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_187.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_188.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_190.png)

本节课中，我们一起学习了超参数调优的核心流程。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_192.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_193.png)

1.  **区分了超参数与模型参数**：超参数需手动调节，模型参数由算法学习。
2.  **理解了超参数调优的目的**：通过交叉验证找到使模型泛化性能最佳的超参数，平衡模型复杂度。
3.  **实践了Lasso回归的超参数调优**：使用 `np.geomspace` 生成候选值，遍历不同的 `alpha`，利用管道组织数据预处理（多项式特征生成、标准化）和模型训练步骤，并通过交叉验证评估每个超参数的性能。
4.  **掌握了管道中步骤顺序的重要性**：特别是多项式特征应在标准化之前添加，这是常见的最佳实践。
5.  **学会了选择与评估最佳模型**：根据交叉验证结果选择最佳超参数，并用其训练最终模型，同时注意到Lasso回归可以通过将系数设为零来进行特征选择。

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_195.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_196.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_197.png)

![](img/c8a7bdd9cbfec409cc42a6afdd8bab32_198.png)

在接下来的课程中，我们将介绍Ridge回归，并学习如何用简洁的代码将整个调优过程串联起来。