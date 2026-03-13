# 126：37_决策树的优缺点 🌳

![](img/f068b768ed49501cdf72692c51c6fcdc_1.png)

在本节课中，我们将要学习决策树模型的优缺点，并探讨如何通过剪枝来防止过拟合。我们还将了解决策树在实际应用中的优势，以及如何在Scikit-learn中实现决策树模型。

![](img/f068b768ed49501cdf72692c51c6fcdc_3.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_4.png)

---

![](img/f068b768ed49501cdf72692c51c6fcdc_6.png)

## 决策树的高方差与过拟合问题

![](img/f068b768ed49501cdf72692c51c6fcdc_8.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_10.png)

决策树往往具有高方差，这意味着它们容易过拟合。

![](img/f068b768ed49501cdf72692c51c6fcdc_12.png)

由于决策树不做出强假设（例如线性可分），它倾向于找到能完美解释训练集的结构。因此，数据中的微小变化会极大地影响预测结果，导致模型无法在训练集之外的数据上良好泛化。

![](img/f068b768ed49501cdf72692c51c6fcdc_14.png)

---

![](img/f068b768ed49501cdf72692c51c6fcdc_16.png)

## 解决方案：通过剪枝控制复杂度

为了解决过拟合问题，我们可以对决策树进行剪枝。一种常见的方法是设置最大深度，以限制树的分裂次数。

![](img/f068b768ed49501cdf72692c51c6fcdc_18.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_19.png)

例如，如果我们设置 `max_depth = 2`，我们将得到一个深度仅为2的剪枝树。这意味着我们会剪掉一些叶节点，并为更高层的节点分配该节点中多数样本的预测类别。

![](img/f068b768ed49501cdf72692c51c6fcdc_20.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_21.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_22.png)

---

## 其他剪枝标准

![](img/f068b768ed49501cdf72692c51c6fcdc_24.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_25.png)

剪枝不仅可以通过树深度来定义，还可以使用其他标准。

以下是几种常见的剪枝标准：

![](img/f068b768ed49501cdf72692c51c6fcdc_27.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_28.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_29.png)

*   **分类错误率阈值**：如果一个叶节点中样本的正确分类比例（例如达到95%），则认为该节点足够好，无需进一步分裂。
*   **信息增益阈值**：只有当分裂能带来足够的信息增益时，才继续分裂。这可以通过设置一个最小信息增益值来实现。
*   **子集最小样本数**：可以设定一个叶节点所需的最小样本数。例如，如果某个节点只剩下3个样本，我们可能不希望将其进一步分裂为包含2个和1个样本的两个叶节点，而是直接将该节点的预测类别定为这3个样本中的多数类。

![](img/f068b768ed49501cdf72692c51c6fcdc_31.png)

---

![](img/f068b768ed49501cdf72692c51c6fcdc_33.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_34.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_35.png)

## 决策树的优势

![](img/f068b768ed49501cdf72692c51c6fcdc_37.png)

现在，我们来总结一下决策树的优点。

![](img/f068b768ed49501cdf72692c51c6fcdc_39.png)

决策树由一系列问答组成，这使其非常易于解释和可视化。当然，如果树过于庞大，解释会变得复杂，但我们仍然可以关注那些做出主要决策的根节点。

![](img/f068b768ed49501cdf72692c51c6fcdc_41.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_42.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_43.png)

在商业应用中，模型的可解释性是一个巨大优势。以客户流失预测为例，决策树可以生成清晰的规则，例如：“月消费超过X元 **且** 月数据使用量低于Y GB的客户更容易流失”。这样的结论很容易向决策者解释。

![](img/f068b768ed49501cdf72692c51c6fcdc_45.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_46.png)

此外，决策树易于处理各种类型的数据。算法可以自动将特征转换为二元问题。与基于距离或线性的算法不同，决策树不需要对特征进行缩放处理。缩放只会改变节点处的问题数值，但数值的顺序保持不变，因此不会影响分裂点的创建。

![](img/f068b768ed49501cdf72692c51c6fcdc_48.png)

---

![](img/f068b768ed49501cdf72692c51c6fcdc_49.png)

## 在Scikit-learn中实现决策树

![](img/f068b768ed49501cdf72692c51c6fcdc_51.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_52.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_53.png)

在实践中，我们可以使用Scikit-learn库来实现决策树。

![](img/f068b768ed49501cdf72692c51c6fcdc_55.png)

以下是实现步骤：

![](img/f068b768ed49501cdf72692c51c6fcdc_57.png)

1.  **导入类**：从 `sklearn.tree` 模块导入决策树分类器。
    ```python
    from sklearn.tree import DecisionTreeClassifier
    ```

![](img/f068b768ed49501cdf72692c51c6fcdc_59.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_60.png)

2.  **创建实例**：通过设置超参数来实例化决策树分类器。
    ```python
    dtc = DecisionTreeClassifier(criterion='gini',
                                 max_features=10,
                                 max_depth=5)
    ```
    *   `criterion`：分裂标准，可以是 `'gini'`（基尼不纯度）或 `'entropy'`（信息熵）。
    *   `max_features`：每次分裂时考虑的最大特征数，这有助于减少过拟合。
    *   `max_depth`：树的最大深度，用于控制模型复杂度。

![](img/f068b768ed49501cdf72692c51c6fcdc_62.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_63.png)

3.  **训练与预测**：在训练数据上拟合模型，然后在测试集上进行预测。
    ```python
    dtc.fit(X_train, y_train)
    predictions = dtc.predict(X_test)
    ```

4.  **调参与回归**：与其他算法一样，可以使用交叉验证来调整超参数。如果需要解决回归问题，只需使用 `DecisionTreeRegressor` 类，其超参数与分类器类似，主要区别在于输出变量是连续的。

![](img/f068b768ed49501cdf72692c51c6fcdc_65.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_66.png)

---

![](img/f068b768ed49501cdf72692c51c6fcdc_67.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_68.png)

## 本节总结

![](img/f068b768ed49501cdf72692c51c6fcdc_70.png)

本节课我们一起学习了决策树的核心内容。

![](img/f068b768ed49501cdf72692c51c6fcdc_71.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_72.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_74.png)

我们首先回顾了分类问题，之前学过的逻辑回归和支持向量机是线性模型，而K近邻算法则需要较大的计算量。

![](img/f068b768ed49501cdf72692c51c6fcdc_76.png)

接着，我们引入了决策树的概念及其在分类中的应用。我们理解了如何使用熵和信息增益来决定最佳分裂点，以构建决策树。

![](img/f068b768ed49501cdf72692c51c6fcdc_78.png)

最后，我们讨论了剪枝决策树以解决过拟合问题的重要性，因为决策树非常容易过拟合。

![](img/f068b768ed49501cdf72692c51c6fcdc_80.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_81.png)

![](img/f068b768ed49501cdf72692c51c6fcdc_82.png)

至此，我们将进入实验环节，在实践中加深对决策树的理解。