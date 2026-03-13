# 097：多分类评估指标与Python实现

在本节课中，我们将学习如何将二分类评估指标（如ROC曲线、精确率-召回率曲线）扩展到多分类问题，并了解如何使用Python的Scikit-learn库来计算这些指标。

![](img/81fb43155f2fbf086450047accb3c8ed_1.png)

![](img/81fb43155f2fbf086450047accb3c8ed_2.png)

![](img/81fb43155f2fbf086450047accb3c8ed_3.png)

---

![](img/81fb43155f2fbf086450047accb3c8ed_4.png)

## 多分类混淆矩阵与准确率

![](img/81fb43155f2fbf086450047accb3c8ed_6.png)

![](img/81fb43155f2fbf086450047accb3c8ed_7.png)

上一节我们介绍了二分类的评估指标。本节中，我们来看看当分类问题扩展到三个或更多类别时，情况会如何变化。

![](img/81fb43155f2fbf086450047accb3c8ed_9.png)

在多分类问题中，混淆矩阵会变得更大。假设我们有一个三分类问题，模型完美预测了所有样本。那么，混淆矩阵的主对角线（蓝色部分）将包含所有正确预测的样本数。

![](img/81fb43155f2fbf086450047accb3c8ed_1.png)
![](img/81fb43155f2fbf086450047accb3c8ed_2.png)
![](img/81fb43155f2fbf086450047accb3c8ed_3.png)
![](img/81fb43155f2fbf086450047accb3c8ed_4.png)

此时，准确率的计算方式与二分类类似，即主对角线上的样本总数除以所有样本的总数。

**公式**：
`准确率 = 主对角线样本数之和 / 总样本数`

![](img/81fb43155f2fbf086450047accb3c8ed_6.png)
![](img/81fb43155f2fbf086450047accb3c8ed_7.png)
![](img/81fb43155f2fbf086450047accb3c8ed_9.png)

---

![](img/81fb43155f2fbf086450047accb3c8ed_11.png)

![](img/81fb43155f2fbf086450047accb3c8ed_12.png)

![](img/81fb43155f2fbf086450047accb3c8ed_13.png)

![](img/81fb43155f2fbf086450047accb3c8ed_14.png)

## 多分类评估策略：一对多方法

![](img/81fb43155f2fbf086450047accb3c8ed_15.png)

![](img/81fb43155f2fbf086450047accb3c8ed_16.png)

![](img/81fb43155f2fbf086450047accb3c8ed_17.png)

对于ROC曲线、精确率、召回率等指标，并没有直接推广到多分类的标准方法。但我们可以采用“一对多”的策略来评估每个类别。

![](img/81fb43155f2fbf086450047accb3c8ed_18.png)

![](img/81fb43155f2fbf086450047accb3c8ed_20.png)

以下是具体的做法：
*   **针对每个类别**，将其视为“正类”，将所有其他类别合并视为“负类”。
*   分别计算该类别的**精确率**、**召回率**、**特异度**等指标。
*   例如，可以计算“类别1 vs 其余”、“类别2 vs 其余”等。

![](img/81fb43155f2fbf086450047accb3c8ed_21.png)

在处理多分类问题时，选择合适的评估指标至关重要。我们需要思考误分类的成本：将类别1误判为类别2的代价是什么？将类别3误判为类别1的代价又是什么？

这时，更大的混淆矩阵就能帮助我们做出决策。混淆矩阵展示了预测类别与实际类别的对应关系，我们可以从中清晰地看到，例如，“预测为类别1但实际为类别2”的样本有多少。

![](img/81fb43155f2fbf086450047accb3c8ed_23.png)

![](img/81fb43155f2fbf086450047accb3c8ed_11.png)
![](img/81fb43155f2fbf086450047accb3c8ed_12.png)
![](img/81fb43155f2fbf086450047accb3c8ed_13.png)
![](img/81fb43155f2fbf086450047accb3c8ed_14.png)
![](img/81fb43155f2fbf086450047accb3c8ed_15.png)
![](img/81fb43155f2fbf086450047accb3c8ed_16.png)
![](img/81fb43155f2fbf086450047accb3c8ed_17.png)
![](img/81fb43155f2fbf086450047accb3c8ed_18.png)

![](img/81fb43155f2fbf086450047accb3c8ed_24.png)

![](img/81fb43155f2fbf086450047accb3c8ed_25.png)

![](img/81fb43155f2fbf086450047accb3c8ed_26.png)

---

## 🔧 使用Python计算评估指标

了解了理论后，我们来看看如何用Python实现。我们可以使用Scikit-learn库轻松计算这些指标。

![](img/81fb43155f2fbf086450047accb3c8ed_28.png)

![](img/81fb43155f2fbf086450047accb3c8ed_29.png)

Scikit-learn的评估指标位于`sklearn.metrics`模块中。导入和使用方式通常遵循相似的语法：输入参数分别是真实标签和预测标签。

![](img/81fb43155f2fbf086450047accb3c8ed_30.png)

![](img/81fb43155f2fbf086450047accb3c8ed_31.png)

![](img/81fb43155f2fbf086450047accb3c8ed_32.png)

**代码示例：计算准确率**
```python
from sklearn.metrics import accuracy_score
# y_test 是真实标签，y_pred 是模型预测的标签
accuracy = accuracy_score(y_test, y_pred)
```

![](img/81fb43155f2fbf086450047accb3c8ed_23.png)
![](img/81fb43155f2fbf086450047accb3c8ed_24.png)
![](img/81fb43155f2fbf086450047accb3c8ed_25.png)
![](img/81fb43155f2fbf086450047accb3c8ed_26.png)

`sklearn.metrics`提供了丰富的评估指标，以下是一些常用的函数：
*   `precision_score`：计算精确率。
*   `recall_score`：计算召回率。
*   `f1_score`：计算F1分数。
*   `roc_auc_score`：计算ROC曲线下面积（AUC）。
*   `confusion_matrix`：计算混淆矩阵。
*   `roc_curve`：计算ROC曲线的点。
*   `precision_recall_curve`：计算精确率-召回率曲线的点。

![](img/81fb43155f2fbf086450047accb3c8ed_28.png)
![](img/81fb43155f2fbf086450047accb3c8ed_29.png)
![](img/81fb43155f2fbf086450047accb3c8ed_30.png)
![](img/81fb43155f2fbf086450047accb3c8ed_31.png)
![](img/81fb43155f2fbf086450047accb3c8ed_32.png)

![](img/81fb43155f2fbf086450047accb3c8ed_34.png)

![](img/81fb43155f2fbf086450047accb3c8ed_35.png)

![](img/81fb43155f2fbf086450047accb3c8ed_36.png)

![](img/81fb43155f2fbf086450047accb3c8ed_37.png)

**重要提示**：不同函数需要的输入可能不同。
*   像**准确率**、**F1分数**、**精确率**、**召回率**这类指标，通常需要输入**预测的类别标签**（例如0或1）。
*   而像**ROC AUC分数**和**精确率-召回率曲线**，则需要输入**预测的概率值**。

![](img/81fb43155f2fbf086450047accb3c8ed_38.png)

![](img/81fb43155f2fbf086450047accb3c8ed_34.png)
![](img/81fb43155f2fbf086450047accb3c8ed_35.png)
![](img/81fb43155f2fbf086450047accb3c8ed_36.png)
![](img/81fb43155f2fbf086450047accb3c8ed_37.png)
![](img/81fb43155f2fbf086450047accb3c8ed_38.png)

如果不确定该传入哪种数据，查阅官方文档是最佳选择。

![](img/81fb43155f2fbf086450047accb3c8ed_40.png)

![](img/81fb43155f2fbf086450047accb3c8ed_41.png)

---

![](img/81fb43155f2fbf086450047accb3c8ed_42.png)

## 📝 本节总结

![](img/81fb43155f2fbf086450047accb3c8ed_44.png)

![](img/81fb43155f2fbf086450047accb3c8ed_45.png)

本节课中我们一起学习了以下内容：

1.  **多分类评估基础**：我们分析了如何将误差类型（如I类错误和II类错误）的概念扩展到多分类场景，并将其与真实标签和预测标签联系起来。
    ![](img/81fb43155f2fbf086450047accb3c8ed_40.png)
    ![](img/81fb43155f2fbf086450047accb3c8ed_41.png)
    ![](img/81fb43155f2fbf086450047accb3c8ed_42.png)

![](img/81fb43155f2fbf086450047accb3c8ed_47.png)

2.  **关键评估指标**：我们讨论了除准确率外，多种衡量分类结果的方法，包括**精确率**、**召回率**、**F1分数**、**ROC曲线**和**精确率-召回率曲线**。
    ![](img/81fb43155f2fbf086450047accb3c8ed_44.png)
    ![](img/81fb43155f2fbf086450047accb3c8ed_45.png)

![](img/81fb43155f2fbf086450047accb3c8ed_48.png)

![](img/81fb43155f2fbf086450047accb3c8ed_49.png)

3.  **指标选择与业务结合**：我们认识到，最终选择的误差指标和模型，取决于具体的业务问题。我们需要根据目标是追求**整体准确率**、**降低假阳性数量**，还是**提高真阳性数量**来做出决策。
    ![](img/81fb43155f2fbf086450047accb3c8ed_47.png)
    ![](img/81fb43155f2fbf086450047accb3c8ed_48.png)
    ![](img/81fb43155f2fbf086450047accb3c8ed_49.png)

关于误差测量的章节到此结束。接下来，我们将进入逻辑回归的实战练习，并在实践中应用这些不同的分类评估指标。

![](img/81fb43155f2fbf086450047accb3c8ed_51.png)

![](img/81fb43155f2fbf086450047accb3c8ed_52.png)

![](img/81fb43155f2fbf086450047accb3c8ed_53.png)

![](img/81fb43155f2fbf086450047accb3c8ed_51.png)
![](img/81fb43155f2fbf086450047accb3c8ed_52.png)
![](img/81fb43155f2fbf086450047accb3c8ed_53.png)