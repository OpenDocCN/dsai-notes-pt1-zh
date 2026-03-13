# 105：K最近邻算法详解 🧠

![](img/3a829ea320039d6464e70afa49369ce2_1.png)

在本节课中，我们将要学习K最近邻（K-Nearest Neighbors, KNN）算法的核心概念、实现细节及其与线性回归的对比。我们将重点关注KNN在分类和回归任务中的应用，并通过Scikit-learn库进行实际代码演示。

![](img/3a829ea320039d6464e70afa49369ce2_3.png)

![](img/3a829ea320039d6464e70afa49369ce2_4.png)

---

![](img/3a829ea320039d6464e70afa49369ce2_6.png)

## KNN与线性回归的对比 🔄

![](img/3a829ea320039d6464e70afa49369ce2_8.png)

上一节我们介绍了KNN的基本思想，本节中我们来看看KNN与线性回归在几个关键方面的差异。

![](img/3a829ea320039d6464e70afa49369ce2_10.png)

![](img/3a829ea320039d6464e70afa49369ce2_11.png)

以下是KNN与线性回归的对比要点：

![](img/3a829ea320039d6464e70afa49369ce2_13.png)

![](img/3a829ea320039d6464e70afa49369ce2_14.png)

*   **模型拟合速度**：线性回归的模型拟合相对较慢，因为它需要搜索最佳参数。而KNN的拟合过程很快，因为它只需要存储数据及其标签。
*   **内存效率**：一旦模型拟合完成，线性回归只需记住决定直线（或高维空间中的超平面）的系数，因此内存效率很高。公式表示为：`y = β₀ + β₁x₁ + ... + βₙxₙ`。相反，KNN需要记住所有的数据点，即整个训练集，因此内存占用较大。
*   **预测速度**：线性回归的预测只是向量的线性组合，因此非常快。而KNN需要计算新数据点到所有训练数据点的距离，并找出最近的K个邻居，这个过程需要更多时间。

![](img/3a829ea320039d6464e70afa49369ce2_16.png)

---

![](img/3a829ea320039d6464e70afa49369ce2_18.png)

## KNN分类模型的语法实现 🛠️

了解了理论对比后，我们来看看如何在代码中实际创建一个KNN分类模型。

以下是使用Scikit-learn创建和训练KNN分类器的步骤：

![](img/3a829ea320039d6464e70afa49369ce2_20.png)

1.  **导入KNN分类器**：
    ```python
    from sklearn.neighbors import KNeighborsClassifier
    ```

![](img/3a829ea320039d6464e70afa49369ce2_22.png)

![](img/3a829ea320039d6464e70afa49369ce2_24.png)

2.  **创建模型实例**：创建类实例时，需要传入关键的超参数`n_neighbors`（即K值）。
    ```python
    knn = KNeighborsClassifier(n_neighbors=3)
    ```
    这里我们将K设置为3。其他参数（如权重计算方式`weights`和距离度量`metric`）使用默认值。默认情况下，使用**均匀权重**（每个邻居的投票权重相同）和**欧几里得距离**。

![](img/3a829ea320039d6464e70afa49369ce2_26.png)

3.  **拟合模型**：使用训练数据拟合模型。
    ```python
    knn.fit(X_train, y_train)
    ```

4.  **进行预测**：对测试集进行预测。
    ```python
    y_pred = knn.predict(X_test)
    ```

![](img/3a829ea320039d6464e70afa49369ce2_28.png)

![](img/3a829ea320039d6464e70afa49369ce2_29.png)

在Scikit-learn中，`fit`和`predict`是预测器（如KNN、线性回归）的通用方法。对于预测器，`fit`方法通常需要传入特征`X`和标签`y`，以便学习预测`y`。而对于转换器（如`StandardScaler`），`fit_transform`方法通常只传入`X`。

![](img/3a829ea320039d6464e70afa49369ce2_31.png)

---

![](img/3a829ea320039d6464e70afa49369ce2_33.png)

![](img/3a829ea320039d6464e70afa49369ce2_34.png)

## KNN回归模型 📈

![](img/3a829ea320039d6464e70afa49369ce2_36.png)

![](img/3a829ea320039d6464e70afa49369ce2_37.png)

如果你希望将KNN用于回归任务，方法非常相似。

只需将`KNeighborsClassifier`替换为`KNeighborsRegressor`，并确保你的`y_train`和`y_test`是连续值即可，其他步骤保持不变。
```python
from sklearn.neighbors import KNeighborsRegressor
knn_reg = KNeighborsRegressor(n_neighbors=3)
knn_reg.fit(X_train, y_train)
y_pred = knn_reg.predict(X_test)
```

![](img/3a829ea320039d6464e70afa49369ce2_39.png)

![](img/3a829ea320039d6464e70afa49369ce2_40.png)

![](img/3a829ea320039d6464e70afa49369ce2_41.png)

---

![](img/3a829ea320039d6464e70afa49369ce2_42.png)

![](img/3a829ea320039d6464e70afa49369ce2_43.png)

## 本节内容总结 🎯

本节课中我们一起学习了K最近邻算法的多个关键方面。

![](img/3a829ea320039d6464e70afa49369ce2_45.png)

![](img/3a829ea320039d6464e70afa49369ce2_46.png)

以下是本节的核心知识点回顾：

![](img/3a829ea320039d6464e70afa49369ce2_48.png)

![](img/3a829ea320039d6464e70afa49369ce2_50.png)

![](img/3a829ea320039d6464e70afa49369ce2_51.png)

*   **KNN分类原理**：KNN通过查看一个新数据点的K个最近邻居，并采用**多数投票**原则进行分类。例如，若K=3，且两个最近邻居属于“流失”类，一个属于“未流失”类，则预测新客户会流失。
*   **决策边界与K值**：K值大小直接影响决策边界的形状。**较大的K值会导致较高的偏差**，极端情况（K等于数据点总数）下，模型总是预测多数类。**较小的K值会导致较高的方差**，极端情况（K=1）下，模型容易受到异常值的影响。
*   **距离度量与特征缩放**：我们强调了距离测量的重要性，最常用的是**欧几里得距离**和**曼哈顿距离**。同时，**特征缩放**至关重要，它能确保没有单个特征对预测结果产生过大或过小的影响。
*   **Scikit-learn实现**：最后，我们讨论了如何使用Scikit-learn库实现KNN，既可用于分类，也可用于回归。

![](img/3a829ea320039d6464e70afa49369ce2_53.png)

![](img/3a829ea320039d6464e70afa49369ce2_54.png)

接下来，我们将把课堂上学到的知识应用到实际的Notebook中，开始动手实践K最近邻算法。