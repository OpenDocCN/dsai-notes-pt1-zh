# 133：44_随机森林 🌲

![](img/6e3001af190788e3cb6b7360a96fccf7_1.png)

在本节课中，我们将学习如何通过引入更多随机性来进一步降低模型的方差，从而超越简单的Bagging方法。我们将重点介绍**随机森林**的原理、优势以及实践应用。

---

![](img/6e3001af190788e3cb6b7360a96fccf7_3.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_4.png)

## 概述：从Bagging到随机森林

![](img/6e3001af190788e3cb6b7360a96fccf7_6.png)

上一节我们介绍了Bagging（自助聚合）方法，它通过构建多个独立的决策树并聚合其结果来降低方差。然而，由于自助采样（有放回抽样）的特性，这些树之间可能存在高度相关性，从而限制了方差的降低效果。

本节中，我们来看看如何通过引入更多随机性来减少树之间的相关性，这就是**随机森林**的核心思想。

![](img/6e3001af190788e3cb6b7360a96fccf7_8.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_9.png)

---

![](img/6e3001af190788e3cb6b7360a96fccf7_10.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_12.png)

## 方差降低的理论基础

![](img/6e3001af190788e3cb6b7360a96fccf7_14.png)

如果Bagging产生了`n`棵独立的树，每棵树的方差为`σ²`，那么聚合后的方差将是`σ² / n`。这意味着，使用的树越多（假设树之间独立），整体方差降低得就越多。

**公式**：
```
聚合方差 = σ² / n
```

![](img/6e3001af190788e3cb6b7360a96fccf7_16.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_17.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_18.png)

但在现实中，由于我们采用有放回抽样，这些树并非完全独立，它们很可能高度相关。如果相关性接近1，那么方差几乎不会降低。这很直观：如果反复使用相同或非常相似的树，你并没有获得新的信息。

---

![](img/6e3001af190788e3cb6b7360a96fccf7_20.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_21.png)

## 随机森林的解决方案：引入特征随机性

![](img/6e3001af190788e3cb6b7360a96fccf7_23.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_24.png)

为了确保每棵决策树彼此不同，从而降低相关性，我们需要引入更多随机性。

![](img/6e3001af190788e3cb6b7360a96fccf7_26.png)

解决方案是：限制每棵树构建时可使用的特征数量。也就是说，每棵树不仅基于随机抽样的行（样本）构建，还基于随机抽样的列（特征）构建。

![](img/6e3001af190788e3cb6b7360a96fccf7_28.png)

默认情况下：
*   对于**分类**模型，随机选择的特征子集大小是总特征数的**平方根**。
*   对于**回归**模型，随机选择的特征子集大小是总特征数的**三分之一**。

![](img/6e3001af190788e3cb6b7360a96fccf7_30.png)

**代码示例（概念）**：
```python
# 对于分类任务
num_features_for_classification = sqrt(total_features)

![](img/6e3001af190788e3cb6b7360a96fccf7_32.png)

# 对于回归任务
num_features_for_regression = total_features / 3
```

![](img/6e3001af190788e3cb6b7360a96fccf7_33.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_34.png)

这种方法迫使每棵树基于不同的特征子集做出决策，从而增加了树之间的差异性。

![](img/6e3001af190788e3cb6b7360a96fccf7_36.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_38.png)

---

## 什么是随机森林？🌳

![](img/6e3001af190788e3cb6b7360a96fccf7_40.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_41.png)

由此产生的算法被称为**随机森林**。本质上，随机森林就是我们刚学过的Bagging，但进行了扩展：它不仅对行（样本）进行随机子采样，还对列（特征）进行随机子采样。

![](img/6e3001af190788e3cb6b7360a96fccf7_42.png)

因此，我们构建每棵树时使用的数据子集，是行和列的双重随机子集。

![](img/6e3001af190788e3cb6b7360a96fccf7_44.png)

一般来说，随机森林需要比简单Bagging**更多数量的树**。但通过这些额外的树，我们通常能获得比简单Bagging更好的样本外准确率。

![](img/6e3001af190788e3cb6b7360a96fccf7_46.png)

---

![](img/6e3001af190788e3cb6b7360a96fccf7_48.png)

## 如何确定随机森林中树的数量？

![](img/6e3001af190788e3cb6b7360a96fccf7_50.png)

与Bagging类似，我们可以通过观察**袋外误差（Out-of-Bag Error）** 随树数量增加的变化趋势来确定。

具体方法是：测试不同树数量下的袋外误差，找到误差开始趋于平稳的阈值。超过这个点后，增加更多的树将不再显著改善结果。

![](img/6e3001af190788e3cb6b7360a96fccf7_52.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_53.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_54.png)

---

![](img/6e3001af190788e3cb6b7360a96fccf7_56.png)

## 随机森林的实践应用

![](img/6e3001af190788e3cb6b7360a96fccf7_58.png)

在实践中运行随机森林与之前学过的分类方法非常相似。

![](img/6e3001af190788e3cb6b7360a96fccf7_60.png)

以下是使用`scikit-learn`实现随机森林分类的步骤：

![](img/6e3001af190788e3cb6b7360a96fccf7_62.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_63.png)

1.  **导入类**
    ```python
    from sklearn.ensemble import RandomForestClassifier
    ```

![](img/6e3001af190788e3cb6b7360a96fccf7_64.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_66.png)

2.  **初始化模型**
    这里我们将树的数量（`n_estimators`）设置为50。
    ```python
    model = RandomForestClassifier(n_estimators=50)
    ```

![](img/6e3001af190788e3cb6b7360a96fccf7_67.png)

3.  **训练与预测**
    ```python
    model.fit(X_train, y_train) # 在训练集上拟合
    predictions = model.predict(X_test) # 在测试集上预测
    ```

4.  **超参数调优**
    与决策树类似，我们可以使用交叉验证来寻找能带来更好样本外拟合效果的超参数组合。

![](img/6e3001af190788e3cb6b7360a96fccf7_69.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_70.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_71.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_72.png)

**对于回归任务**：
只需使用`RandomForestRegressor`代替`RandomForestClassifier`，并确保你的目标变量`y`是连续值而非分类标签。
```python
from sklearn.ensemble import RandomForestRegressor
model = RandomForestRegressor(n_estimators=50)
```

---

![](img/6e3001af190788e3cb6b7360a96fccf7_74.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_75.png)

## 超越随机森林：极端随机树 🌲➡️🎲

![](img/6e3001af190788e3cb6b7360a96fccf7_77.png)

如果随机森林仍然过拟合（方差降低得不够），我们可以引入**更多**的随机性。

我们还可以在每棵决策树中**随机选择分裂点**。回忆一下，决策树通常使用贪心搜索来寻找最佳分裂点。如果某个特征在给定的子集中可用，它可能总是被选为决策树顶部的第一个分裂点。

![](img/6e3001af190788e3cb6b7360a96fccf7_79.png)

而**极端随机树（Extra Trees）** 方法则会随机选择在哪个特征、以及在该特征的哪个值上进行分裂。

![](img/6e3001af190788e3cb6b7360a96fccf7_80.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_81.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_82.png)

这些额外的随机树恰如其名地被称为“极端随机树”。其希望在于，即使每个分裂点是随机的，只要我们有足够多的树，每个叶节点仍然能由多数类主导，当所有树的结果聚合在一起时，整体模型仍然是一个好的分类器，即使每个单独的组件（树）稍弱一些。

**极端随机树的语法**：
步骤与随机森林完全相同，只是导入的类不同。
```python
from sklearn.ensemble import ExtraTreesClassifier # 用于分类
# 或
from sklearn.ensemble import ExtraTreesRegressor # 用于回归

![](img/6e3001af190788e3cb6b7360a96fccf7_84.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_85.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_86.png)

model = ExtraTreesClassifier(n_estimators=50)
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```
同样，我们可以使用交叉验证来调整参数。这提供了一种比Bagging或随机森林随机性更强的分类方法。

---

![](img/6e3001af190788e3cb6b7360a96fccf7_88.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_89.png)

## 本节总结 🎯

![](img/6e3001af190788e3cb6b7360a96fccf7_90.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_91.png)

在本节中，我们介绍了通过组合模型来提升预测能力的概念，这被称为**集成方法**。

我们首先介绍了**Bootstrap聚合（Bagging）**，其中：
*   **自助采样（Bootstrapping）** 步骤：从原始训练集中获取随机子集来构建分类器。
*   **聚合（Aggregation）** 步骤：使用多数投票法将各个分类器的分类结果聚合起来。

![](img/6e3001af190788e3cb6b7360a96fccf7_93.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_94.png)

最后，我们展示了如何从Bagging演进到随机性更强、更不易过拟合的模型版本：
1.  **随机森林**：在Bagging基础上，增加了对特征的随机子采样。
2.  **极端随机树**：在随机森林基础上，连决策树的分裂点也进行随机选择。

![](img/6e3001af190788e3cb6b7360a96fccf7_96.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_97.png)

![](img/6e3001af190788e3cb6b7360a96fccf7_98.png)

掌握了这些知识后，我们将在接下来的实战笔记本中看到它们的具体应用。