# 083：正则化技术演示（第三部分）📊

![](img/2bc5a947b5483bb7a89139cb197a71fd_0.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_2.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_3.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_5.png)

在本节课中，我们将学习三种不同的正则化技术：**岭回归 (Ridge)**、**套索回归 (Lasso)** 和**弹性网络 (Elastic Net)**。我们将了解它们如何通过向成本函数添加惩罚项来减少模型系数的大小，从而降低模型复杂度并防止过拟合。课程将结合代码演示，展示如何在 `scikit-learn` 中使用这些方法，并通过交叉验证选择最佳的超参数。

![](img/2bc5a947b5483bb7a89139cb197a71fd_7.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_9.png)

---

![](img/2bc5a947b5483bb7a89139cb197a71fd_10.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_11.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_13.png)

## 1. 岭回归 (Ridge) 与 L2 正则化 🏔️

![](img/2bc5a947b5483bb7a89139cb197a71fd_15.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_16.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_17.png)

上一节我们介绍了多项式特征和过拟合问题。本节中，我们来看看如何使用正则化技术来解决高方差（过拟合）问题。

岭回归使用 **L2 正则化** 来减小模型系数的幅度。其原理是向成本函数中添加一个额外的惩罚项，该惩罚项是每个系数的平方和。

![](img/2bc5a947b5483bb7a89139cb197a71fd_19.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_20.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_21.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_22.png)

**公式**：
`J(θ) = MSE(θ) + α * Σ(θ_j²)`

![](img/2bc5a947b5483bb7a89139cb197a71fd_23.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_24.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_26.png)

其中，`α` 是控制正则化强度的超参数。在存在高方差（即模型过于复杂）的情况下，通过减小系数的大小，可以降低模型的复杂度，从而减少方差。

![](img/2bc5a947b5483bb7a89139cb197a71fd_28.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_30.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_31.png)

在 `scikit-learn` 中，正则化模型通常包含内置交叉验证的版本，这可以方便地帮助我们选择最优的超参数。

![](img/2bc5a947b5483bb7a89139cb197a71fd_33.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_35.png)

---

![](img/2bc5a947b5483bb7a89139cb197a71fd_37.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_38.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_39.png)

## 2. 使用 RidgeCV 进行交叉验证 🔍

![](img/2bc5a947b5483bb7a89139cb197a71fd_41.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_42.png)

如果你还记得我们在交叉验证笔记本中讨论的内容，`GridSearchCV` 可以遍历给定模型的许多不同超参数。它还会进行多次训练/测试分割（例如4折、5折），然后找出在这些分割上平均性能最佳的超参数。

![](img/2bc5a947b5483bb7a89139cb197a71fd_44.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_45.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_46.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_47.png)

`RidgeCV` 本质上等同于指定模型为岭回归的 `GridSearchCV`。它简化了流程，让我们可以直接调用。

![](img/2bc5a947b5483bb7a89139cb197a71fd_49.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_51.png)

以下是使用 `RidgeCV` 的步骤：

![](img/2bc5a947b5483bb7a89139cb197a71fd_53.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_54.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_55.png)

1.  从 `sklearn.linear_model` 导入 `RidgeCV`。
2.  定义我们想要遍历的 `alpha` 值列表。
3.  初始化 `RidgeCV` 对象，传入 `alphas` 列表，并指定交叉验证的折数（例如 `cv=4`）。
4.  使用训练数据（`X_train`, `y_train`）拟合模型。
5.  使用之前定义的均方根误差函数，评估拟合后的模型在测试集上的预测性能。
6.  我们可以从拟合好的 `ridge_cv` 对象中获取最优的 `alpha` 值。

![](img/2bc5a947b5483bb7a89139cb197a71fd_57.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_58.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_59.png)

运行代码后，我们发现最优的 `alpha` 值是 10，此时的均方根误差为 32.205。

![](img/2bc5a947b5483bb7a89139cb197a71fd_61.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_62.png)

---

## 3. 套索回归 (Lasso) 与 L1 正则化 🎯

![](img/2bc5a947b5483bb7a89139cb197a71fd_64.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_65.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_66.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_67.png)

与 `RidgeCV` 类似，也存在 `LassoCV`。这个函数将使用 **L1 正则化** 以及我们讨论过的交叉验证。

L1 正则化是取系数的绝对值之和作为惩罚项。它同样会减小系数，但这次是选择性地将一些系数收缩至零，从而有效地执行特征选择。

![](img/2bc5a947b5483bb7a89139cb197a71fd_69.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_70.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_71.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_72.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_73.png)

**公式**：
`J(θ) = MSE(θ) + α * Σ|θ_j|`

![](img/2bc5a947b5483bb7a89139cb197a71fd_75.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_76.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_78.png)

---

![](img/2bc5a947b5483bb7a89139cb197a71fd_80.png)

## 4. 弹性网络 (Elastic Net) 与混合正则化 ⚖️

![](img/2bc5a947b5483bb7a89139cb197a71fd_82.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_83.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_84.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_85.png)

同样，也有内置交叉验证的弹性网络函数 `ElasticNetCV`。弹性网络结合了 L2 和 L1 正则化。

**公式**：
`J(θ) = MSE(θ) + α * [ρ * Σ|θ_j| + 0.5 * (1 - ρ) * Σ(θ_j²)]`

![](img/2bc5a947b5483bb7a89139cb197a71fd_87.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_88.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_89.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_90.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_92.png)

其中，`ρ` 是 `l1_ratio` 参数，控制 L1 惩罚项的比例。

![](img/2bc5a947b5483bb7a89139cb197a71fd_93.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_95.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_96.png)

接下来，我们将使用交叉验证拟合一个 Lasso 模型，并确定最优的 `alpha` 值及其对应的均方根误差。需要注意的是，Lasso 的最优 `alpha` 可能与岭回归不同，因为惩罚项的形式不同（系数平方 vs. 系数绝对值）。因此，你可能需要遍历不同的 `alpha` 值来找到最优模型。

![](img/2bc5a947b5483bb7a89139cb197a71fd_98.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_100.png)

关于如何选择 `alpha` 值，建议先使用一定数量的值进行尝试。如果列表中的最大值或最小值始终表现最好，则需要大幅扩大或缩小范围，直到找到一个可以精细调整的区间，最终逼近最优值。

![](img/2bc5a947b5483bb7a89139cb197a71fd_101.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_103.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_104.png)

在下面的演示中，我们将使用从 0.005 到 140 的一系列值。

![](img/2bc5a947b5483bb7a89139cb197a71fd_106.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_107.png)

我们设置 `alphas` 为这个数组，并将其传递给 `LassoCV`。对于 `LassoCV`，我们还需要指定 `max_iter`（最大迭代次数）。这是因为 Lasso 底层使用梯度下降进行优化，可能比岭回归耗时更长。设置 `max_iter` 是为了在算法无法收敛之前限制迭代次数。目前你无需深究，只需知道 Lasso 运行时间可能更长，我们将在后续课程中深入学习梯度下降。

![](img/2bc5a947b5483bb7a89139cb197a71fd_109.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_110.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_112.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_114.png)

运行后，我们看到最优 `alpha` 值为 120，均方根误差为 37.010。我们还可以查看 Lasso 消除了多少特征：在总共 251 个系数中，只有 102 个是非零的，这意味着我们减少了一半以上的系数。

![](img/2bc5a947b5483bb7a89139cb197a71fd_116.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_118.png)

---

![](img/2bc5a947b5483bb7a89139cb197a71fd_119.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_121.png)

## 5. 实施弹性网络回归 🔄

![](img/2bc5a947b5483bb7a89139cb197a71fd_123.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_124.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_125.png)

现在，我们使用弹性网络。弹性网络将我们的 `lambda`（即 `alpha`）值分配一部分给岭回归的系数平方惩罚，一部分给套索回归的系数绝对值惩罚。

![](img/2bc5a947b5483bb7a89139cb197a71fd_127.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_128.png)

这里，我们将为 `l1_ratio` 参数取 0.1 到 0.9 之间的 9 个值。`l1_ratio=0.1` 表示大部分权重在岭回归部分，而 `l1_ratio=0.9` 表示大部分权重在套索回归部分。

![](img/2bc5a947b5483bb7a89139cb197a71fd_130.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_131.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_132.png)

我们调用 `ElasticNetCV`，传入之前定义的 `alphas` 数组和 `l1_ratios` 列表。由于需要遍历更多参数组合，拟合可能需要一些时间。拟合完成后，我们可以计算其在测试集上的预测值和均方根误差。弹性网络与岭回归和套索回归类似，只是多了一个 `l1_ratio` 超参数。

![](img/2bc5a947b5483bb7a89139cb197a71fd_134.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_135.png)

---

![](img/2bc5a947b5483bb7a89139cb197a71fd_136.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_138.png)

## 6. 模型性能对比 📈

![](img/2bc5a947b5483bb7a89139cb197a71fd_140.png)

现在，我们将比较所创建的所有模型的均方根误差：线性回归、岭回归、套索回归和弹性网络。

![](img/2bc5a947b5483bb7a89139cb197a71fd_141.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_142.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_143.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_144.png)

我们将创建一个包含这些均方根误差的序列，索引为对应的模型标签（‘linear‘， ‘ridge‘， ‘lasso‘， ‘elastic net‘），然后将其转换为 DataFrame 并重命名列。通过对比，我们可以看到正则化显著改善了均方根误差，使其大幅降低。

![](img/2bc5a947b5483bb7a89139cb197a71fd_146.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_147.png)

在本例中，弹性网络的表现略优于岭回归，而两者都优于套索回归。具体选择哪种模型取决于你的需求和目标：如果你想大幅减少系数并进行特征选择，套索回归可能更合适；如果你希望模型运行非常快，岭回归是更好的选择；如果你想找到最优的平衡点，则可以使用弹性网络。

---

![](img/2bc5a947b5483bb7a89139cb197a71fd_149.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_150.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_151.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_152.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_153.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_154.png)

## 7. 可视化预测结果 📊

![](img/2bc5a947b5483bb7a89139cb197a71fd_156.png)

最后，我们将为每个正则化模型绘制预测值与实际值的散点图，这与之前对线性回归所做的类似。

![](img/2bc5a947b5483bb7a89139cb197a71fd_157.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_158.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_159.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_161.png)

我们的标签列表是 [‘Ridge‘， ‘Lasso‘， ‘Elastic Net‘]，模型列表是对应已拟合好的模型对象（`ridge_cv`， `lasso_cv`， `elastic_net_cv`）。通过 `zip` 函数将模型和标签配对，然后遍历每个组合，使用相同的坐标轴绘制测试集实际值 (`y_test`) 与模型预测值的散点图，并添加图例。

![](img/2bc5a947b5483bb7a89139cb197a71fd_163.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_164.png)

这个图表本身可能不提供太多额外信息。要评估每个模型的预测能力，查看均方根误差更有价值。

![](img/2bc5a947b5483bb7a89139cb197a71fd_165.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_166.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_168.png)

---

![](img/2bc5a947b5483bb7a89139cb197a71fd_170.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_171.png)

## 总结 🎓

![](img/2bc5a947b5483bb7a89139cb197a71fd_173.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_174.png)

本节课中，我们一起学习了三种核心的正则化技术：**岭回归 (L2)**、**套索回归 (L1)** 和**弹性网络 (混合)**。我们了解了它们如何通过向损失函数添加惩罚项来控制模型复杂度、防止过拟合。通过 `scikit-learn` 中的 `RidgeCV`、`LassoCV` 和 `ElasticNetCV`，我们实践了如何利用交叉验证自动选择最佳的正则化强度参数 (`alpha`)，并对比了不同模型在测试集上的性能。结果表明，适当地使用正则化可以显著提升模型的泛化能力。

![](img/2bc5a947b5483bb7a89139cb197a71fd_176.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_177.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_178.png)

![](img/2bc5a947b5483bb7a89139cb197a71fd_179.png)

本节关于正则化演示的内容到此结束。接下来，我们将有一节更深入的讲座来探讨正则化的工作原理，之后还会有使用正则化的练习。