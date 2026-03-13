# 094：5_实现逻辑回归模型 📊

![](img/bb179f0d418b6227caabe4575ecbc566_1.png)

在本节课中，我们将学习如何使用 Scikit-learn 库来构建和训练一个逻辑回归模型。我们将从导入模型开始，逐步完成模型的实例化、训练、预测和系数查看等步骤，并探讨逻辑回归在实际中的应用场景。

![](img/bb179f0d418b6227caabe4575ecbc566_3.png)

![](img/bb179f0d418b6227caabe4575ecbc566_4.png)

---

## 模型导入与实例化 🧩

首先，我们需要从 Scikit-learn 的线性模型模块中导入逻辑回归类。

```python
from sklearn.linear_model import LogisticRegression
```

![](img/bb179f0d418b6227caabe4575ecbc566_6.png)

![](img/bb179f0d418b6227caabe4575ecbc566_7.png)

![](img/bb179f0d418b6227caabe4575ecbc566_8.png)

导入模型后，下一步是实例化这个类，创建一个逻辑回归对象。在实例化时，我们可以设置一些超参数来控制模型。

![](img/bb179f0d418b6227caabe4575ecbc566_9.png)

以下是实例化逻辑回归模型的关键步骤：

![](img/bb179f0d418b6227caabe4575ecbc566_11.png)

![](img/bb179f0d418b6227caabe4575ecbc566_12.png)

*   **创建对象**：我们创建一个名为 `lr` 的对象。
*   **设置正则化**：通过 `penalty='l2'` 参数指定使用 L2 正则化来防止过拟合。L2 正则化的惩罚项是系数平方和。
*   **设置正则化强度**：通过 `C` 参数设置正则化强度。这里的 `C` 是之前学习过的正则化常数 λ 的倒数，因此 **`C` 值越大，表示惩罚越轻**。

```python
lr = LogisticRegression(penalty='l2', C=1.0)
```

![](img/bb179f0d418b6227caabe4575ecbc566_14.png)

![](img/bb179f0d418b6227caabe4575ecbc566_15.png)

---

## 模型训练与预测 🚀

上一节我们完成了模型的初始化，本节中我们来看看如何使用数据对模型进行训练，并用它来做出预测。

![](img/bb179f0d418b6227caabe4575ecbc566_17.png)

![](img/bb179f0d418b6227caabe4575ecbc566_18.png)

![](img/bb179f0d418b6227caabe4575ecbc566_19.png)

模型训练需要使用准备好的训练数据集，包括特征 `X_train` 和对应的标签 `y_train`。

![](img/bb179f0d418b6227caabe4575ecbc566_20.png)

![](img/bb179f0d418b6227caabe4575ecbc566_21.png)

```python
lr.fit(X_train, y_train)
```

模型训练完成后，我们就可以使用它来对新的数据（例如测试集 `X_test`）进行预测，得到分类结果。

![](img/bb179f0d418b6227caabe4575ecbc566_23.png)

![](img/bb179f0d418b6227caabe4575ecbc566_24.png)

![](img/bb179f0d418b6227caabe4575ecbc566_25.png)

```python
y_pred = lr.predict(X_test)
```

![](img/bb179f0d418b6227caabe4575ecbc566_26.png)

---

## 查看模型系数与扩展说明 🔍

与线性回归类似，训练好的逻辑回归模型也可以查看其各个特征的系数，这有助于我们理解不同特征对预测结果的影响程度。

![](img/bb179f0d418b6227caabe4575ecbc566_28.png)

![](img/bb179f0d418b6227caabe4575ecbc566_29.png)

![](img/bb179f0d418b6227caabe4575ecbc566_30.png)

```python
coefficients = lr.coef_
```

![](img/bb179f0d418b6227caabe4575ecbc566_31.png)

![](img/bb179f0d418b6227caabe4575ecbc566_32.png)

![](img/bb179f0d418b6227caabe4575ecbc566_33.png)

关于模型系数，有两点重要的补充说明：

*   如果你需要获取逻辑回归或线性回归系数的 **P 值** 以进行统计推断，建议使用 **Statsmodels** 包。它在统计推断方面功能更强，而 Scikit-learn 更侧重于通用的机器学习流程。
*   Scikit-learn 提供了 **`LogisticRegressionCV`** 等交叉验证方法，可以方便地尝试多组参数。该方法会在交叉验证集上搜索最佳参数，然后使用这组最佳参数重新拟合整个训练集的模型，其功能类似于我们之前学过的 `GridSearchCV`。

![](img/bb179f0d418b6227caabe4575ecbc566_35.png)

![](img/bb179f0d418b6227caabe4575ecbc566_36.png)

![](img/bb179f0d418b6227caabe4575ecbc566_37.png)

---

## 逻辑回归的应用场景 🌐

![](img/bb179f0d418b6227caabe4575ecbc566_39.png)

![](img/bb179f0d418b6227caabe4575ecbc566_40.png)

逻辑回归作为一种基础的分类算法，其应用非常广泛，远不止于我们例子中的客户流失预测。

以下是逻辑回归在其他领域的一些典型应用：

*   **客户消费预测**：根据历史购买数据，预测一个客户成为（例如）消费额前5%用户的可能性。
*   **客户参与度预测**：预测哪些客户在未来六个月内最有可能参与互动或活动。
*   **电子商务风控**：利用客户位置、IP地址等特征，预测某笔交易是否存在欺诈风险。
*   **金融与风险评估**：预测一笔贷款是否会违约。

![](img/bb179f0d418b6227caabe4575ecbc566_42.png)

![](img/bb179f0d418b6227caabe4575ecbc566_43.png)

---

![](img/bb179f0d418b6227caabe4575ecbc566_44.png)

![](img/bb179f0d418b6227caabe4575ecbc566_45.png)

![](img/bb179f0d418b6227caabe4575ecbc566_46.png)

## 模型的可解释性 💡

![](img/bb179f0d418b6227caabe4575ecbc566_47.png)

在使用逻辑回归时，我们需要权衡 **预测** 与 **解释** 这两个目标。

![](img/bb179f0d418b6227caabe4575ecbc566_49.png)

除了单纯预测类别标签，我们通常还希望评估每个因素对结果的影响程度。回想一下，逻辑回归的系数仍然具有重要的解释意义。在线性回归中，`x` 每变化一个单位，结果变量 `y` 平均变化 β 个单位。在逻辑回归中，`x` 每变化一个单位，**对数几率（log odds）** 会变化 β 个单位。这仍然为我们提供了 `x` 对结果变量影响大小的直观理解。

![](img/bb179f0d418b6227caabe4575ecbc566_51.png)

---

## 本节总结 📝

![](img/bb179f0d418b6227caabe4575ecbc566_53.png)

![](img/bb179f0d418b6227caabe4575ecbc566_54.png)

![](img/bb179f0d418b6227caabe4575ecbc566_55.png)

本节课中我们一起学习了逻辑回归模型的完整实现流程。

1.  我们首先将逻辑回归作为第一个分类算法引入，它能够输出不同类别的概率。
2.  接着，我们对比了线性回归与逻辑回归的异同，并揭示了两者之间的联系。
3.  最后，我们以客户流失预测为例，演示了如何使用 Scikit-learn 实现逻辑回归模型进行类别预测，并探讨了该模型及其他分类器可能适用的更多场景。

![](img/bb179f0d418b6227caabe4575ecbc566_57.png)

![](img/bb179f0d418b6227caabe4575ecbc566_58.png)

![](img/bb179f0d418b6227caabe4575ecbc566_59.png)

在下一个视频中，我们将讨论一个非常重要的话题：如何选择正确的分类评估指标。期待与你再次相见。