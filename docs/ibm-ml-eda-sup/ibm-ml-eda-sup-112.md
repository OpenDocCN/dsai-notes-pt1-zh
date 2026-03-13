# 112：支持向量机中的正则化 📏

在本节课中，我们将学习支持向量机（SVM）中的正则化概念。我们将回顾SVM的基本原理，探讨过拟合问题，并理解如何通过正则化来平衡模型的复杂度和分类错误。最后，我们将介绍如何在Python中使用Scikit-learn库实现线性支持向量机。

---

## 支持向量机回顾 🔍

上一节我们介绍了支持向量机的基本概念。本节中，我们来看看关于支持向量机我们已经学习了哪些内容。

我们知道支持向量机是一种线性分类器。它不返回概率，而是返回标签，例如“1”或“0”，代表“流失”与“未流失”。这些标签由数据点落在决策边界的哪一侧决定。决策边界最初是通过确定那个能最小化错误、并在两个类别之间找到最宽间隔的超平面或直线来找到的。这个边界完全由数据中的支持向量决定，即那些位于我们之前看到的间隔边界上的点。



![](img/724424c2da0ae2367468b6cfc8f7bd05_1.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_1.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_2.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_3.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_2.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_4.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_5.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_3.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_7.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_4.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_5.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_9.png)

---

![](img/724424c2da0ae2367468b6cfc8f7bd05_11.png)

## 过拟合与正则化 ⚖️

![](img/724424c2da0ae2367468b6cfc8f7bd05_13.png)

我们接着讨论了如果坚持这种形式化方法，实际上可能使数据过拟合。



![](img/724424c2da0ae2367468b6cfc8f7bd05_7.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_15.png)

我们在这里看到的是能最小化错误分类的最佳边界，它最小化了我们的合页损失。

![](img/724424c2da0ae2367468b6cfc8f7bd05_9.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_17.png)

这对应于我们整体成本函数中的第一项，即左侧所示的部分，它仅仅是错误分类的成本。

![](img/724424c2da0ae2367468b6cfc8f7bd05_18.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_11.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_20.png)

给定我们这里的边界，这一项是最小的。



![](img/724424c2da0ae2367468b6cfc8f7bd05_22.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_13.png)

然而，对于支持向量机整体函数中的第二项，即正则化项。

![](img/724424c2da0ae2367468b6cfc8f7bd05_24.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_15.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_26.png)

拥有一个系数更高、更复杂的决策边界会带来额外的成本。因此，我们可能在最小化成本函数的第一部分，但同时，正则化项的另一部分却相当高。



![](img/724424c2da0ae2367468b6cfc8f7bd05_28.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_17.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_29.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_18.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_31.png)

那么我们应该怎么做？这里我们有一个复杂度较低的决策边界。



![](img/724424c2da0ae2367468b6cfc8f7bd05_20.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_33.png)

对于这个边界，因为我们现在有一些错误分类（例如左侧粉色的点）。

![](img/724424c2da0ae2367468b6cfc8f7bd05_34.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_35.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_22.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_37.png)

我们的第一项，即我们试图最小化的错误分类数量（合页损失）。



![](img/724424c2da0ae2367468b6cfc8f7bd05_24.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_39.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_40.png)

现在变得高了一些。

![](img/724424c2da0ae2367468b6cfc8f7bd05_26.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_42.png)

但正则化项应该以更大的程度减少，从而使整体成本函数最小化。



![](img/724424c2da0ae2367468b6cfc8f7bd05_43.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_28.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_45.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_29.png)

---

![](img/724424c2da0ae2367468b6cfc8f7bd05_47.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_48.png)

## 正则化参数 C 🎛️

![](img/724424c2da0ae2367468b6cfc8f7bd05_50.png)

我们需要注意到，这里的正则化效果可以通过参数 **C** 的值来调整。



![](img/724424c2da0ae2367468b6cfc8f7bd05_31.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_52.png)

其中，较小的 **C** 值意味着更强的正则化。**C** 越小，**1/C** 就越大，对高系数的惩罚也就越重，正则化效果越强。

![](img/724424c2da0ae2367468b6cfc8f7bd05_53.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_33.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_55.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_34.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_56.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_35.png)

---

## 深入理解正则化项 🧮

让我们更仔细地看看这里的正则化项。



![](img/724424c2da0ae2367468b6cfc8f7bd05_58.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_37.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_59.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_60.png)

向量 **β**（我们的每个系数）代表了与超平面（即决策边界）正交的向量。



![](img/724424c2da0ae2367468b6cfc8f7bd05_61.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_62.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_39.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_40.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_64.png)

回想一下，这些系数乘以各自的特征后相加，或者说这些系数与各自特征的点积。



![](img/724424c2da0ae2367468b6cfc8f7bd05_42.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_43.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_66.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_67.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_68.png)

最终将决定我们的标签落在决策边界的哪一侧。



![](img/724424c2da0ae2367468b6cfc8f7bd05_69.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_45.png)

该点积的值大于零属于某个类别，小于零则属于另一个类别。



![](img/724424c2da0ae2367468b6cfc8f7bd05_47.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_71.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_72.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_73.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_48.png)

点积之和恰好为零则代表了决策边界实际所在的位置。



![](img/724424c2da0ae2367468b6cfc8f7bd05_50.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_75.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_76.png)

因此，那条正交线的值就是我们计算的值，距离越远，正值或负值的绝对值就越大，具体取决于该线指向的方向。



![](img/724424c2da0ae2367468b6cfc8f7bd05_78.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_52.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_79.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_53.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_81.png)

这些系数越高，每个特征对决定正类或负类的影响就越大。



![](img/724424c2da0ae2367468b6cfc8f7bd05_82.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_55.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_84.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_56.png)

---

![](img/724424c2da0ae2367468b6cfc8f7bd05_86.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_87.png)

## Python 实现 🐍

现在让我们讨论如何使用 Python 中的 Scikit-learn 来实现支持向量机。

以下是实现线性支持向量机分类的步骤：

1.  **导入必要的类**：首先，我们需要从 `sklearn.svm` 导入线性支持向量分类模型。这里我们指定使用线性支持向量机，内核支持向量机将在下一个视频中讨论。
    ```python
    from sklearn.svm import LinearSVC
    ```

    ![](img/724424c2da0ae2367468b6cfc8f7bd05_58.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_59.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_60.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_61.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_62.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_89.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_90.png)

2.  **创建模型实例**：专注于线性支持向量机，我们要做的第一件事是创建该类的一个实例。创建实例时，我们将传入超参数，这里主要是正则化参数，设置惩罚项为 `‘l2‘`，**C** 值设为 `10`。记住，较低的 **C** 值意味着更强的正则化和更简单的模型。
    ```python
    svm_model = LinearSVC(penalty=‘l2‘, C=10)
    ```

    ![](img/724424c2da0ae2367468b6cfc8f7bd05_64.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_66.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_67.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_68.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_69.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_91.png)

3.  **训练模型**：然后在训练集上拟合这个实例，调用 `fit` 方法并传入 `X_train` 和 `y_train`。
    ```python
    svm_model.fit(X_train, y_train)
    ```

    ![](img/724424c2da0ae2367468b6cfc8f7bd05_71.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_72.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_73.png)

4.  **进行预测**：模型训练好后，我们可以用它来对测试集 `X_test` 进行预测，就像我们对其他模型所做的那样。
    ```python
    y_pred = svm_model.predict(X_test)
    ```

![](img/724424c2da0ae2367468b6cfc8f7bd05_93.png)

5.  **调参**：我们可以使用交叉验证来调整正则化参数 **C** 和我们选择的惩罚项，可以使用我们在早期课程中学到的 `GridSearchCV`。
    ```python
    from sklearn.model_selection import GridSearchCV
    ```

    ![](img/724424c2da0ae2367468b6cfc8f7bd05_75.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_76.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_94.png)

6.  **支持向量回归**：如果你想进行回归而不是分类，只需调用 `LinearSVR` 而不是 `LinearSVC`。运行所有相同的步骤，只是你的 `y_train` 必须是连续值。
    ```python
    from sklearn.svm import LinearSVR
    ```

    ![](img/724424c2da0ae2367468b6cfc8f7bd05_78.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_79.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_81.png)
    ![](img/724424c2da0ae2367468b6cfc8f7bd05_82.png)

---

## 总结 📝

本节课中我们一起学习了支持向量机中的正则化。

![](img/724424c2da0ae2367468b6cfc8f7bd05_96.png)

我们讨论了支持向量机的分类方法，即如何使用来自不同类别的支持向量来找到具有最大间隔的决策边界。



![](img/724424c2da0ae2367468b6cfc8f7bd05_97.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_84.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_86.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_87.png)

我们将支持向量机与逻辑回归进行了比较：支持向量机预测的是实际标签，而逻辑回归预测的是概率；支持向量机的成本函数只惩罚边界错误一侧的点，而逻辑回归惩罚所有结果。



![](img/724424c2da0ae2367468b6cfc8f7bd05_89.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_90.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_91.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_99.png)

支持向量机仅依赖于那些支持向量，然后利用这些支持向量得出最大间隔。这里需要注意的一点区别是，这意味着支持向量机可能对落在间隔内的值更敏感，但完全不受那些在间隔外被正确分类的大值（离群点）的影响。回想一下，这是逻辑回归旨在解决的一个主要问题，而支持向量机可能更强大，因为它们将完全忽略这些被正确识别的离群点，它们不会对我们整体模型产生任何影响。



![](img/724424c2da0ae2367468b6cfc8f7bd05_100.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_101.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_93.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_94.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_96.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_97.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_102.png)

我们还介绍了支持向量机的成本函数，即最小化错误分类的合页损失，以及附加的正则化项。我们讨论了正则化在支持向量机模型中实际如何工作，以及我们可以调整 **C** 的值，较低的 **C** 意味着更强的正则化和更简单的模型。



![](img/724424c2da0ae2367468b6cfc8f7bd05_99.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_100.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_101.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_102.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_104.png)

至此，我们结束了关于线性支持向量机的部分。接下来，我们将扩展这一内容，展示支持向量机通过使用称为“内核”的方法可以变得多么强大。



![](img/724424c2da0ae2367468b6cfc8f7bd05_105.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_106.png)

![](img/724424c2da0ae2367468b6cfc8f7bd05_104.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_105.png)
![](img/724424c2da0ae2367468b6cfc8f7bd05_106.png)