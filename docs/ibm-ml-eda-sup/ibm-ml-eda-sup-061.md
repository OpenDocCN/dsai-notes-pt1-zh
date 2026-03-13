# 061：训练集与测试集分割（第二部分）📊

![](img/ece5c8018bcf98ffd317229142e1d546_1.png)

![](img/ece5c8018bcf98ffd317229142e1d546_2.png)

在本节课中，我们将学习如何将数据集分割为训练集和测试集，并理解其背后的工作流程。这是评估机器学习模型泛化能力的关键步骤。

## 概述

![](img/ece5c8018bcf98ffd317229142e1d546_4.png)

我们将详细讲解使用训练数据拟合模型，以及使用测试数据评估模型的完整流程。同时，我们将介绍在Python中实现数据集分割的具体语法。

![](img/ece5c8018bcf98ffd317229142e1d546_5.png)

![](img/ece5c8018bcf98ffd317229142e1d546_6.png)

![](img/ece5c8018bcf98ffd317229142e1d546_7.png)

## 训练与测试工作流程

![](img/ece5c8018bcf98ffd317229142e1d546_9.png)

![](img/ece5c8018bcf98ffd317229142e1d546_10.png)

上一节我们介绍了数据集分割的基本概念，本节中我们来看看具体如何操作。

![](img/ece5c8018bcf98ffd317229142e1d546_12.png)

首先，我们从训练数据开始。训练数据包含特征变量 `X_train` 和结果变量 `y_train`。

![](img/ece5c8018bcf98ffd317229142e1d546_14.png)

![](img/ece5c8018bcf98ffd317229142e1d546_15.png)

我们使用 `.fit()` 方法，根据这个带有标签的数据集来学习模型的参数。这是使用Scikit-learn所有模型的常规做法，我们在线性回归中已经见过，后续也会一直使用。

![](img/ece5c8018bcf98ffd317229142e1d546_16.png)

![](img/ece5c8018bcf98ffd317229142e1d546_18.png)

```python
model.fit(X_train, y_train)
```

![](img/ece5c8018bcf98ffd317229142e1d546_19.png)

![](img/ece5c8018bcf98ffd317229142e1d546_21.png)

这个过程会得到一个定义了特征 `X` 与结果变量之间关系参数的模型。

![](img/ece5c8018bcf98ffd317229142e1d546_23.png)

接下来，我们处理测试数据。我们将测试集中的 `X_test` 输入到已经学习了参数的模型中。

![](img/ece5c8018bcf98ffd317229142e1d546_25.png)

![](img/ece5c8018bcf98ffd317229142e1d546_26.png)

然后，我们可以用这个模型来预测 `X_test` 对应的结果变量，就像这些测试数据原本没有标签一样。

![](img/ece5c8018bcf98ffd317229142e1d546_28.png)

```python
y_pred = model.predict(X_test)
```

![](img/ece5c8018bcf98ffd317229142e1d546_30.png)

![](img/ece5c8018bcf98ffd317229142e1d546_31.png)

这将为测试集中的每一个样本值生成一个预测值。

![](img/ece5c8018bcf98ffd317229142e1d546_32.png)

由于测试数据本质上是历史数据，我们可以将这些预测值与 `X_test` 对应的实际值进行比较。

然后，我们可以在 `y_test` 和 `y_pred` 之间计算误差指标。

![](img/ece5c8018bcf98ffd317229142e1d546_34.png)

![](img/ece5c8018bcf98ffd317229142e1d546_35.png)

这样，我们就可以得出模型在假设面对从未见过的新数据时的测试误差。

![](img/ece5c8018bcf98ffd317229142e1d546_36.png)

![](img/ece5c8018bcf98ffd317229142e1d546_37.png)

![](img/ece5c8018bcf98ffd317229142e1d546_38.png)

## Python实现语法

了解了理论流程后，现在我们来看看在Python中执行训练集-测试集分割所需的语法。

![](img/ece5c8018bcf98ffd317229142e1d546_40.png)

首先，我们需要从Scikit-learn库中导入相应的函数。

![](img/ece5c8018bcf98ffd317229142e1d546_41.png)

![](img/ece5c8018bcf98ffd317229142e1d546_42.png)

![](img/ece5c8018bcf98ffd317229142e1d546_43.png)

```python
from sklearn.model_selection import train_test_split
```

![](img/ece5c8018bcf98ffd317229142e1d546_45.png)

![](img/ece5c8018bcf98ffd317229142e1d546_46.png)

然后，执行分割操作非常简单。以下代码将数据分割，并将30%放入测试集。参数 `test_size=0.3` 指定了要留出多少比例的数据作为测试集。

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)
```

你也可以传入一个具体的数值。如果该值在0到1之间，程序会将其视为百分比；如果是其他值，则会将其作为测试集的精确样本数量。

在上面的例子中，我们传入了 `X` 和 `y`，因此输出是四个不同的值：`X_train`、`X_test`、`y_train` 和 `y_test`。所以在 `train_test_split` 的左侧需要有四个变量来接收这些值。

## 其他数据分割方法

![](img/ece5c8018bcf98ffd317229142e1d546_48.png)

![](img/ece5c8018bcf98ffd317229142e1d546_49.png)

![](img/ece5c8018bcf98ffd317229142e1d546_50.png)

除了简单的 `train_test_split`，还有其他数据分割方法。

![](img/ece5c8018bcf98ffd317229142e1d546_51.png)

![](img/ece5c8018bcf98ffd317229142e1d546_52.png)

![](img/ece5c8018bcf98ffd317229142e1d546_53.png)

例如，`ShuffleSplit` 允许你创建多个不同的分割，而不仅仅是一次训练集和测试集的分割。你可以有四个不同的训练集和测试集分割组合。

![](img/ece5c8018bcf98ffd317229142e1d546_54.png)

![](img/ece5c8018bcf98ffd317229142e1d546_55.png)

![](img/ece5c8018bcf98ffd317229142e1d546_56.png)

![](img/ece5c8018bcf98ffd317229142e1d546_57.png)

还有 `StratifiedShuffleSplit`，它可以确保你的结果变量中没有偏差。这是什么意思呢？假设你的数据集是医疗数据，标签为1表示诊断出癌症，0表示没有。可能99%的人口不会被诊断出癌症，只有1%会。我们希望在我们的测试集和训练集中保持这种99%对1%的比例。使用分层洗牌分割将确保我们保持这个99%对1%的比例。

![](img/ece5c8018bcf98ffd317229142e1d546_58.png)

![](img/ece5c8018bcf98ffd317229142e1d546_59.png)

## 总结

![](img/ece5c8018bcf98ffd317229142e1d546_61.png)

![](img/ece5c8018bcf98ffd317229142e1d546_62.png)

本节课中，我们一起学习了机器学习中训练集与测试集分割的核心工作流程。我们了解了如何使用训练数据拟合模型，以及如何使用测试数据评估模型的预测性能。我们还掌握了在Python中使用 `train_test_split` 函数的基本语法，并简要了解了其他如 `ShuffleSplit` 和 `StratifiedShuffleSplit` 等高级分割方法，这些方法有助于进行更稳健的模型验证。