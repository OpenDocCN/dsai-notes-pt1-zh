# 056：线性回归简介（第二部分）📊

![](img/c4e3b38f161648c65d814cfeae35f3fe_1.png)

在本节课中，我们将学习线性回归建模的最佳实践，包括如何评估模型性能、理解R平方指标，以及如何在Python中使用Scikit-learn库实现线性回归。

![](img/c4e3b38f161648c65d814cfeae35f3fe_3.png)

---

## 模型评估最佳实践 🎯

![](img/c4e3b38f161648c65d814cfeae35f3fe_5.png)

上一节我们介绍了线性回归的基本概念。本节中，我们来看看如何评估和比较不同模型的性能。

![](img/c4e3b38f161648c65d814cfeae35f3fe_7.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_8.png)

任何建模任务的最佳实践，首先是确定我们希望最小化的**成本函数**。

![](img/c4e3b38f161648c65d814cfeae35f3fe_10.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_1.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_11.png)

成本函数为我们提供了一种比较不同模型优劣的方法。

![](img/c4e3b38f161648c65d814cfeae35f3fe_3.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_13.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_14.png)

接着，我们需要开发多个模型。这些模型可能具有不同的超参数，甚至是完全不同的算法，以找出哪个模型能给出最好的预测。

![](img/c4e3b38f161648c65d814cfeae35f3fe_16.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_5.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_17.png)

最后，我们可以根据成本函数比较结果，并选择最佳模型。

![](img/c4e3b38f161648c65d814cfeae35f3fe_7.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_8.png)

---

![](img/c4e3b38f161648c65d814cfeae35f3fe_19.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_20.png)

## 理解R平方指标 📈

![](img/c4e3b38f161648c65d814cfeae35f3fe_21.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_22.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_23.png)

除了均方误差，另一个需要牢记的重要指标是**R平方**。

![](img/c4e3b38f161648c65d814cfeae35f3fe_10.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_11.png)

我们来分解一下它的组成部分。

![](img/c4e3b38f161648c65d814cfeae35f3fe_25.png)

以下是R平方公式的组成部分：

![](img/c4e3b38f161648c65d814cfeae35f3fe_26.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_27.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_28.png)

1.  **残差平方和**：衡量真实值与预测值之间的距离，类似于我们之前看到的成本函数中的部分。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_13.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_14.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_29.png)

2.  **总平方和**：衡量真实值与真实值平均值之间的距离。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_16.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_17.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_31.png)

**残差平方和**代表了模型**未能解释**的变异。想象一条穿过所有数据点的回归线，它与实际数据点的垂直距离就是模型未能解释的部分。

![](img/c4e3b38f161648c65d814cfeae35f3fe_32.png)

**总平方和**代表了**总变异**。想象一条水平的均值线，结果变量围绕其均值的波动就是总变异。

![](img/c4e3b38f161648c65d814cfeae35f3fe_19.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_20.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_21.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_22.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_23.png)

因此，**R平方**衡量的是模型**已解释**的变异比例。

其公式为：
**R² = 1 - (SSE / SST)**

![](img/c4e3b38f161648c65d814cfeae35f3fe_34.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_35.png)

其中：
*   **SSE** 是残差平方和（未解释的变异）。
*   **SST** 是总平方和（总变异）。

![](img/c4e3b38f161648c65d814cfeae35f3fe_36.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_37.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_38.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_25.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_26.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_27.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_28.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_29.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_39.png)

R平方衡量了模型在多大程度上减少了未解释的方差。这个值越接近**1**，说明模型对总体方差的解释能力越好。

![](img/c4e3b38f161648c65d814cfeae35f3fe_41.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_31.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_32.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_42.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_43.png)

---

![](img/c4e3b38f161648c65d814cfeae35f3fe_45.png)

## 关于模型复杂度的注意事项 ⚠️

![](img/c4e3b38f161648c65d814cfeae35f3fe_47.png)

我们需要记住，随着模型变得**更复杂**，我们总是可以更好地拟合每一个数据点。

![](img/c4e3b38f161648c65d814cfeae35f3fe_49.png)

例如，我们可以添加更多特征。即使这些特征没有任何预测能力，它们也**永远不会降低R平方值**。因为如果添加的特征没有预测力，我们可以将其系数设为零，这样就不会影响我们已解释的变异量。

![](img/c4e3b38f161648c65d814cfeae35f3fe_34.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_35.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_36.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_37.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_38.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_39.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_51.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_52.png)

另外请注意，R平方并不仅限于线性回归使用。

![](img/c4e3b38f161648c65d814cfeae35f3fe_54.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_55.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_41.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_42.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_43.png)

实际上，它可以用于**任何回归模型**，以理解该模型所解释的方差。

![](img/c4e3b38f161648c65d814cfeae35f3fe_45.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_57.png)

---

![](img/c4e3b38f161648c65d814cfeae35f3fe_58.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_59.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_60.png)

## 在Python中实现线性回归 🐍

![](img/c4e3b38f161648c65d814cfeae35f3fe_61.png)

了解了理论之后，我们来看看如何在Python中实际操作。

![](img/c4e3b38f161648c65d814cfeae35f3fe_63.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_47.png)

以下是使用Scikit-learn实现线性回归的步骤：

![](img/c4e3b38f161648c65d814cfeae35f3fe_65.png)

1.  **导入回归类**：第一步是导入包含回归方法的类。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_49.png)
    我们之前简单介绍过Scikit-learn，但这是第一次用它来构建预测器。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_51.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_52.png)
    具体代码是：
    ```python
    from sklearn.linear_model import LinearRegression
    ```
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_54.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_55.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_66.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_67.png)

2.  **创建模型实例**：下一步是创建这个类的一个实例，这就是我们的算法。它还没有拟合任何数据集。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_57.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_58.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_59.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_60.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_61.png)
    具体代码是：
    ```python
    lr = LinearRegression() # lr 是我们的线性回归模型对象，尚未拟合数据
    ```

3.  **拟合模型**：然后，我们在数据上拟合这个实例，以尽可能降低损失函数。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_63.png)
    我们将用于训练模型的数据（即已有的数据）称为 `X_train` 和 `y_train`。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_65.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_66.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_67.png)
    具体代码是：
    ```python
    lr.fit(X_train, y_train) # 现在模型已经拟合，我们得到了系数
    ```

![](img/c4e3b38f161648c65d814cfeae35f3fe_69.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_70.png)

4.  **进行预测**：拟合后，我们可以调用 `lr.predict()` 方法。这个预测方法现在可用于已拟合的模型，以对尚未见过、没有标签的测试集进行预测，结果就是我们的预测值。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_69.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_70.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_71.png)
    具体代码是：
    ```python
    predictions = lr.predict(X_test)
    ```

![](img/c4e3b38f161648c65d814cfeae35f3fe_71.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_73.png)

我们将在本节之后的Notebook中详细看到这个过程。

![](img/c4e3b38f161648c65d814cfeae35f3fe_75.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_73.png)

---

![](img/c4e3b38f161648c65d814cfeae35f3fe_77.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_78.png)

## 课程总结 📝

让我们回顾一下到目前为止学到的内容。

![](img/c4e3b38f161648c65d814cfeae35f3fe_80.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_75.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_81.png)

本节课中我们一起学习了：

*   **线性回归**以及我们需要了解的**损失函数**，还有如何根据不同的参数（β0 和 β1）来优化它。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_77.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_78.png)

*   **建模最佳实践**：确保首先明确我们的模型和损失函数是什么，然后考察不同的模型，最后根据损失函数优化并选择我们应该使用的最佳模型。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_80.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_81.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_83.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_84.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_85.png)

*   **误差度量方法**：我们讨论了为什么不能简单相减（会产生正负值），而是使用这些值的**平方**来得出最优模型。我们还详细介绍了**R平方**，以及它如何帮助我们定义模型对总体方差的解释程度。
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_83.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_84.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_85.png)
    ![](img/c4e3b38f161648c65d814cfeae35f3fe_86.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_86.png)

接下来，我们将进入Notebook进行实战，期待在那里与你相见。

![](img/c4e3b38f161648c65d814cfeae35f3fe_88.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_89.png)

![](img/c4e3b38f161648c65d814cfeae35f3fe_88.png)
![](img/c4e3b38f161648c65d814cfeae35f3fe_89.png)