# 144：集成学习（Boosting与Voting Classifier）实践 🚀

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_0.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_2.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_4.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_5.png)

在本节课中，我们将学习如何结合不同的机器学习模型来提升预测性能。具体来说，我们将实践使用带正则化的逻辑回归模型，并将其与梯度提升树模型结合，通过投票分类器来获得更优的结果。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_7.png)

---

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_8.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_9.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_11.png)

## 构建带L2正则化的逻辑回归模型 🔧

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_13.png)

上一节我们介绍了梯度提升，本节中我们来看看如何结合其他模型。首先，我们需要构建一个基础模型。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_14.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_15.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_17.png)

我们创建一个使用**L2惩罚项**的逻辑回归模型。在Scikit-learn中，这通过设置 `penalty='l2'` 来实现。同时，我们使用`solver='lbfgs'`求解器，并设置`max_iter=500`以确保模型有足够的迭代次数来收敛到最优解。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_18.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_20.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_22.png)

以下是创建和训练模型的代码：
```python
# 创建逻辑回归模型，使用L2正则化
LRL2 = LogisticRegression(penalty='l2', solver='lbfgs', max_iter=500)
# 在训练集上拟合模型
LRL2.fit(X_train, y_train)
```
模型训练可能需要一些时间（约45秒到1分钟）。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_23.png)

---

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_25.png)

## 评估单一模型性能 📊

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_26.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_27.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_28.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_29.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_31.png)

模型训练完成后，我们需要评估其性能。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_33.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_35.png)

我们首先用训练好的模型对测试集进行预测，然后生成分类报告和混淆矩阵来评估效果。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_36.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_38.png)

```python
# 对测试集进行预测
y_pred_lrl2 = LRL2.predict(X_test)
# 打印分类报告
print(classification_report(y_test, y_pred_lrl2))
```
评估结果显示，该逻辑回归模型的准确率约为0.98，表现良好，但略低于之前我们使用的Boosting模型。通过混淆矩阵可以发现，模型最容易混淆的仍然是类别1和类别2。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_39.png)

---

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_41.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_42.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_43.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_44.png)

## 创建并训练投票分类器 🤝

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_46.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_48.png)

单一模型可能已达到其性能上限。为了追求更好的效果，我们可以将多个模型组合起来。接下来，我们将逻辑回归模型与之前训练好的梯度提升分类器结合，创建一个投票分类器。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_49.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_51.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_52.png)

投票分类器有两种投票方式：
*   **硬投票**：直接根据每个模型的预测类别进行投票。
*   **软投票**：根据每个模型预测的**类别概率**进行加权平均。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_54.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_55.png)

这里我们选择更精细的**软投票**方式。例如，如果GBC预测类别1的概率为70%，而LRL2预测类别1的概率为0%，那么软投票会倾向于选择类别0。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_57.png)

以下是创建和训练投票分类器的代码：
```python
# 创建投票分类器，使用软投票
vc = VotingClassifier(estimators=[('LRL2', LRL2), ('GBC', GBC)], voting='soft')
# 在训练集上拟合投票分类器
vc.fit(X_train, y_train)
```
**重要提示**：使用投票分类器或堆叠分类器时，Scikit-learn会重新拟合其中的每一个基模型。如果传入的基模型本身很复杂（例如包含网格搜索的模型），训练时间会非常长。在实际应用中，应先从简单模型开始评估，如果性能已满足需求，则无需追求过度复杂的集成，以平衡性能与计算成本。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_58.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_59.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_60.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_61.png)

---

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_63.png)

## 评估集成模型效果 🎯

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_64.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_65.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_66.png)

投票分类器训练完成后，我们对其性能进行评估。

我们使用与之前相同的方法：在测试集上进行预测，并生成分类报告和混淆矩阵。

```python
# 用投票分类器进行预测
y_pred_vc = vc.predict(X_test)
# 打印分类报告
print(classification_report(y_test, y_pred_vc))
```
结果显示，投票分类器的准确率回到了0.99，与之前最优的梯度提升分类器性能相当。观察混淆矩阵可以发现，在区分类别1和类别2时，集成模型的表现比单一的逻辑回归模型有所改善。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_68.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_69.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_70.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_71.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_72.png)

---

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_73.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_74.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_75.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_77.png)

## 总结与下节预告 📚

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_79.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_81.png)

本节课中我们一起学习了集成学习的另一个实用技巧——投票分类器。我们实践了以下步骤：
1.  构建并评估了一个带L2正则化的逻辑回归模型。
2.  将该逻辑回归模型与梯度提升树模型结合，创建了一个软投票分类器。
3.  评估了集成模型的性能，发现其能够达到与最优单一模型相媲美甚至更优的效果。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_83.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_84.png)

我们再次强调，在构建复杂模型前，务必先评估简单模型的性能，避免不必要的计算资源消耗。这适用于所有基于数据科学的业务决策。

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_86.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_87.png)

![](img/1bb42b7c7aa21aa1da89f7ddcd9789fb_88.png)

在接下来的课程中，我们将更深入地探讨如何处理类别不平衡问题及其不同的解决方案。