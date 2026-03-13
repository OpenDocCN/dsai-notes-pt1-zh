# 101：K最近邻分类算法 🧠

![](img/f86aea66ad78a24992da917b992e31a2_1.png)

![](img/f86aea66ad78a24992da917b992e31a2_2.png)

在本节课中，我们将学习课程中的第二个算法：K最近邻（K-Nearest Neighbors，简称KNN）。我们将探讨KNN在分类问题中的应用，理解其决策边界，并学习如何通过调整K值来优化模型。此外，我们还将讨论距离度量的重要性以及特征缩放的必需性。最后，我们将演示如何在Scikit-learn中实现KNN分类，并简要介绍KNN在回归问题中的应用。

![](img/f86aea66ad78a24992da917b992e31a2_4.png)

![](img/f86aea66ad78a24992da917b992e31a2_6.png)

---

![](img/f86aea66ad78a24992da917b992e31a2_7.png)

![](img/f86aea66ad78a24992da917b992e31a2_9.png)

## K最近邻分类方法介绍

![](img/f86aea66ad78a24992da917b992e31a2_10.png)

上一节我们介绍了逻辑回归，本节中我们来看看K最近邻分类方法。K最近邻是一种基于实例的学习算法，它通过查找与目标数据点最相似的K个已知标签的数据点，并根据这些邻居的标签来预测目标点的类别。

在电信客户流失预测的例子中，我们使用历史数据，包括客户的通话时长（分钟）和数据使用量（千兆字节）。我们将这些数据绘制在图表上，X轴表示通话时长，Y轴表示数据使用量。根据客户是否流失，我们用蓝色表示流失客户，红色表示未流失客户。

![](img/f86aea66ad78a24992da917b992e31a2_12.png)

![](img/f86aea66ad78a24992da917b992e31a2_13.png)

![](img/f86aea66ad78a24992da917b992e31a2_14.png)

![](img/f86aea66ad78a24992da917b992e31a2_15.png)

![](img/f86aea66ad78a24992da917b992e31a2_16.png)

假设我们有一个新客户（用紫色点表示），我们需要预测该客户是否会流失。直观上，我们会观察与该客户特征相似的其他客户，并根据这些邻居的流失情况来做出预测。

![](img/f86aea66ad78a24992da917b992e31a2_17.png)

![](img/f86aea66ad78a24992da917b992e31a2_19.png)

---

![](img/f86aea66ad78a24992da917b992e31a2_20.png)

![](img/f86aea66ad78a24992da917b992e31a2_22.png)

## K值选择与决策边界

![](img/f86aea66ad78a24992da917b992e31a2_24.png)

在KNN算法中，K值的选择至关重要。K值决定了我们考虑多少个最近邻居来做出预测。较小的K值可能导致模型对噪声敏感，而较大的K值可能使模型过于平滑。

![](img/f86aea66ad78a24992da917b992e31a2_25.png)

![](img/f86aea66ad78a24992da917b992e31a2_26.png)

![](img/f86aea66ad78a24992da917b992e31a2_28.png)

以下是K值选择的几个关键点：

![](img/f86aea66ad78a24992da917b992e31a2_30.png)

![](img/f86aea66ad78a24992da917b992e31a2_31.png)

*   **K=1**：仅考虑最近的一个邻居。预测结果直接由该邻居的标签决定。
*   **K=2**：考虑最近的两个邻居。如果两个邻居的标签不同，则会出现平局情况。
*   **K=3**：考虑最近的三个邻居。通过多数投票决定预测结果，避免了平局问题。

![](img/f86aea66ad78a24992da917b992e31a2_33.png)

通常建议选择奇数的K值，以避免投票时出现平局。K值是一个超参数，我们需要通过调优来找到最适合模型的K值。

![](img/f86aea66ad78a24992da917b992e31a2_34.png)

![](img/f86aea66ad78a24992da917b992e31a2_35.png)

![](img/f86aea66ad78a24992da917b992e31a2_37.png)

---

![](img/f86aea66ad78a24992da917b992e31a2_38.png)

![](img/f86aea66ad78a24992da917b992e31a2_40.png)

## 距离度量与特征缩放

![](img/f86aea66ad78a24992da917b992e31a2_42.png)

![](img/f86aea66ad78a24992da917b992e31a2_43.png)

KNN算法依赖于距离度量来计算数据点之间的相似性。常用的距离度量包括欧几里得距离和曼哈顿距离。欧几里得距离的公式如下：

![](img/f86aea66ad78a24992da917b992e31a2_45.png)

![](img/f86aea66ad78a24992da917b992e31a2_46.png)

**公式**：`distance = sqrt((x2 - x1)^2 + (y2 - y1)^2)`

![](img/f86aea66ad78a24992da917b992e31a2_48.png)

![](img/f86aea66ad78a24992da917b992e31a2_49.png)

在使用KNN之前，特征缩放是一个非常重要的步骤。如果特征具有不同的量纲或范围，距离度量可能会被量纲较大的特征主导，导致模型性能下降。因此，我们需要对特征进行标准化或归一化处理，确保所有特征在相同的尺度上。

![](img/f86aea66ad78a24992da917b992e31a2_50.png)

---

![](img/f86aea66ad78a24992da917b992e31a2_52.png)

![](img/f86aea66ad78a24992da917b992e31a2_53.png)

![](img/f86aea66ad78a24992da917b992e31a2_54.png)

## Scikit-learn中的KNN实现

![](img/f86aea66ad78a24992da917b992e31a2_56.png)

在Scikit-learn中，我们可以使用`KNeighborsClassifier`类来实现KNN分类。以下是一个简单的代码示例：

![](img/f86aea66ad78a24992da917b992e31a2_58.png)

![](img/f86aea66ad78a24992da917b992e31a2_59.png)

```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

![](img/f86aea66ad78a24992da917b992e31a2_61.png)

![](img/f86aea66ad78a24992da917b992e31a2_62.png)

![](img/f86aea66ad78a24992da917b992e31a2_63.png)

# 假设X是特征数据，y是标签
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

![](img/f86aea66ad78a24992da917b992e31a2_65.png)

# 特征缩放
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

![](img/f86aea66ad78a24992da917b992e31a2_66.png)

![](img/f86aea66ad78a24992da917b992e31a2_67.png)

![](img/f86aea66ad78a24992da917b992e31a2_69.png)

# 创建KNN分类器，设置K=3
knn = KNeighborsClassifier(n_neighbors=3)

# 训练模型
knn.fit(X_train_scaled, y_train)

![](img/f86aea66ad78a24992da917b992e31a2_71.png)

![](img/f86aea66ad78a24992da917b992e31a2_72.png)

![](img/f86aea66ad78a24992da917b992e31a2_73.png)

![](img/f86aea66ad78a24992da917b992e31a2_74.png)

# 预测
predictions = knn.predict(X_test_scaled)
```

此外，KNN也可以用于回归问题，通过计算K个最近邻居的平均值来预测连续值。

![](img/f86aea66ad78a24992da917b992e31a2_76.png)

---

![](img/f86aea66ad78a24992da917b992e31a2_77.png)

![](img/f86aea66ad78a24992da917b992e31a2_78.png)

![](img/f86aea66ad78a24992da917b992e31a2_79.png)

![](img/f86aea66ad78a24992da917b992e31a2_80.png)

![](img/f86aea66ad78a24992da917b992e31a2_81.png)

## 总结

![](img/f86aea66ad78a24992da917b992e31a2_83.png)

![](img/f86aea66ad78a24992da917b992e31a2_84.png)

本节课中我们一起学习了K最近邻分类算法。我们了解了KNN的基本原理，包括如何通过查找最近邻居来进行分类预测。我们探讨了K值选择的重要性，以及如何通过调整K值来优化模型的决策边界。我们还强调了距离度量和特征缩放在KNN中的关键作用，并演示了如何在Scikit-learn中实现KNN分类。最后，我们简要提到了KNN在回归问题中的应用。掌握这些概念将帮助你在实际项目中有效地应用KNN算法。