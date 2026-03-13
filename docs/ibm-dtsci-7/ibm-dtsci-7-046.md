# 046：岭回归 🏔️

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_1.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_3.png)

在本节课中，我们将要学习一种名为**岭回归**的线性回归技术。岭回归的核心目标是防止模型在训练过程中出现**过拟合**现象，从而提高模型在未知数据上的泛化能力。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_5.png)

上一节我们介绍了多项式回归及其可视化。本节中我们来看看当模型存在多个自变量或特征时，过拟合会成为一个显著问题。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_6.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_7.png)

## 过拟合问题与岭回归的引入

考虑一个由橙色曲线表示的四阶多项式函数。蓝色数据点由该函数生成。

我们可以使用一个十阶多项式来拟合这些数据。此时，蓝色估计函数能很好地近似真实的橙色函数。

然而，现实数据常包含异常值。例如，下图中的某个点似乎并非来自橙色函数。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_9.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_10.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_11.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_12.png)

如果我们使用十阶多项式函数来拟合包含此异常点的数据，得到的蓝色估计函数将变得不准确，无法良好估计真实的橙色函数。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_13.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_14.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_15.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_16.png)

检查估计函数的表达式，会发现估计出的多项式系数具有非常大的幅值，对于高阶项尤为明显。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_17.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_19.png)

**岭回归**通过引入参数 **alpha** 来控制这些多项式系数的大小。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_20.png)

## 参数 Alpha 的作用与选择

Alpha 是我们在拟合或训练模型之前需要选择的一个参数。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_22.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_23.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_24.png)

以下表格展示了 alpha 值递增时的情况。每一行代表一个递增的 alpha 值。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_25.png)

让我们看看不同的 alpha 值如何改变模型。此表展示了不同 alpha 值对应的多项式系数。列对应不同的多项式系数，行对应不同的 alpha 值。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_27.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_28.png)

随着 alpha 增大，参数值变小。这对高阶多项式特征最为明显。但必须谨慎选择 alpha 值。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_30.png)

以下是不同 alpha 值对模型拟合效果的影响：

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_32.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_33.png)

*   如果 alpha 过大，系数将趋近于 0，导致模型**欠拟合**数据。
*   如果 alpha 为 0，则等同于普通最小二乘法，过拟合现象明显。
*   当 alpha 等于 0.001 时，过拟合开始减弱。
*   当 alpha 等于 0.01 时，估计函数能较好地跟踪实际函数。
*   当 alpha 等于 1 时，我们看到了欠拟合的初步迹象，估计函数灵活性不足。
*   当 alpha 等于 10 时，出现极端欠拟合，模型甚至无法跟踪那两个数据点。

## 使用交叉验证选择 Alpha

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_35.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_36.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_37.png)

为了选择最佳的 alpha，我们使用交叉验证技术。

以下是使用 Python 的 scikit-learn 库进行岭回归预测的基本步骤：

1.  从 `sklearn.linear_model` 导入 `Ridge`。
2.  使用构造函数创建一个岭回归对象，其中参数 `alpha` 是构造函数的参数之一。
3.  使用 `fit` 方法训练模型。
4.  使用 `predict` 方法进行预测。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_39.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_40.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_41.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_42.png)

为了确定参数 alpha，我们使用一部分数据（训练集）进行训练，并使用另一个称为**验证集**的数据集来评估不同 alpha 值的效果。验证集类似于测试集，但专门用于选择像 alpha 这样的超参数。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_43.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_44.png)

选择 alpha 的基本流程如下：

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_46.png)

1.  从一个较小的 alpha 值开始。
2.  训练模型。
3.  使用验证数据进行预测。
4.  计算 R 平方分数并存储该值。
5.  对一个更大的 alpha 值重复此过程：再次训练模型，使用验证数据预测，计算并存储 R 平方值。
6.  为不同的 alpha 值重复此过程，训练模型并进行预测。
7.  **选择能够最大化 R 平方值的 alpha**。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_47.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_48.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_50.png)

注意，我们也可以使用其他指标（如均方误差）来选择 alpha 值。

## 实际案例：二手车数据集

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_52.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_53.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_54.png)

如果我们拥有大量特征，过拟合问题会更加严重。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_56.png)

下图展示了在二手车数据集上应用二阶多项式函数时的情况。纵轴表示不同的 R 平方值，横轴表示不同的 alpha 值。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_58.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_60.png)

图中红色曲线代表训练数据上的 R 平方，蓝色曲线代表验证数据上的 R 平方。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_62.png)

我们可以看到，随着 alpha 值增大，验证集上的 R 平方值增加，并在大约 0.75 处收敛。在这种情况下，我们选择最大的 alpha 值，因为对更高的 alpha 值进行实验影响甚微。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_64.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_65.png)

相反，随着 alpha 增加，测试数据上的 R 平方值会下降。这是因为 alpha 项起到了防止过拟合的作用，这可能会提升模型在未见数据上的表现，但模型在测试数据上的性能会变差。具体如何生成此图，请参阅相关实验部分。

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_67.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_69.png)

## 总结

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_71.png)

![](img/7a1700e9f3f6f5ef9d3aa77f2eb281be_72.png)

本节课中我们一起学习了**岭回归**。我们了解到，岭回归通过在损失函数中增加一个惩罚项（由参数 **alpha** 控制）来限制模型系数的大小，从而有效缓解**过拟合**问题。我们探讨了 alpha 值对模型拟合效果（从过拟合到欠拟合）的影响，并学习了如何使用**交叉验证**和**验证集**来选择最优的 alpha 值，以提升模型的泛化能力。