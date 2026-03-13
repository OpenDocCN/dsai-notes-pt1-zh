# 135：Bagging 与随机森林模型评估 🌲📊

![](img/4a9906896b6c7764f88ffcf625efa72b_0.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_2.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_3.png)

在本节课程中，我们将学习如何使用随机森林（Random Forest）和极端随机树（Extra Trees）模型，并通过观察袋外误差（Out-of-Bag Error）随决策树数量增加的变化，来确定模型性能达到稳定（即误差平台期）所需的最佳树数量。我们将使用 `scikit-learn` 库进行实践。

![](img/4a9906896b6c7764f88ffcf625efa72b_5.png)

---

## 概述：模型误差与树数量的关系

![](img/4a9906896b6c7764f88ffcf625efa72b_7.png)

上一节我们介绍了Bagging的基本概念。本节中，我们将通过代码演示，观察随机森林模型的袋外误差如何随着决策树数量的增加而变化。核心目标是找到误差开始趋于稳定的“平台期”，从而确定构建最优模型所需的足够树数量。

![](img/4a9906896b6c7764f88ffcf625efa72b_8.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_9.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_10.png)

由于我们只改变模型中的树数量，因此可以使用 `warm_start=True` 参数。这样，模型会在现有模型的基础上继续添加更多的树，而无需从头开始重新训练。我们将使用 `set_params` 方法来更新已初始化分类器的树数量。

---

![](img/4a9906896b6c7764f88ffcf625efa72b_12.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_13.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_14.png)

## 第一部分：随机森林的袋外误差分析

![](img/4a9906896b6c7764f88ffcf625efa72b_15.png)

以下是实现随机森林模型并追踪其袋外误差随树数量变化的步骤。

![](img/4a9906896b6c7764f88ffcf625efa72b_17.png)

首先，从 `sklearn.ensemble` 导入 `RandomForestClassifier`，并初始化模型对象。我们需要获取袋外分数（OOB Score），因此设置 `oob_score=True`。袋外分数是指：对于每一棵决策树，用其训练时未使用的数据（即袋外数据）来计算的误差。

![](img/4a9906896b6c7764f88ffcf625efa72b_19.png)

我们设置一个随机种子 `random_state`，并将 `warm_start` 设为 `True`。由于随机森林可以并行运行，我们设置 `n_jobs=-1` 以使用所有可用的处理器核心。

![](img/4a9906896b6c7764f88ffcf625efa72b_21.png)

```python
from sklearn.ensemble import RandomForestClassifier

![](img/4a9906896b6c7764f88ffcf625efa72b_22.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_24.png)

# 初始化随机森林分类器
rf = RandomForestClassifier(
    n_estimators=1,          # 初始树数量设为1，后续会更新
    warm_start=True,         # 允许增量添加树
    oob_score=True,          # 启用袋外误差计算
    random_state=42,
    n_jobs=-1                # 使用所有CPU核心并行计算
)
```

![](img/4a9906896b6c7764f88ffcf625efa72b_26.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_27.png)

为了追踪每个树数量对应的袋外误差，我们创建一个空列表。

![](img/4a9906896b6c7764f88ffcf625efa72b_28.png)

```python
oob_error_list = []
```

![](img/4a9906896b6c7764f88ffcf625efa72b_30.png)

接着，我们循环遍历一系列不同的树数量（例如从15到400），观察误差的变化。

```python
tree_numbers = [15, 20, 30, 40, 50, 75, 100, 150, 200, 250, 300, 350, 400]

![](img/4a9906896b6c7764f88ffcf625efa72b_32.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_33.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_34.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_35.png)

for n_trees in tree_numbers:
    # 更新模型参数，仅改变树的数量
    rf.set_params(n_estimators=n_trees)
    # 在训练集上拟合模型
    rf.fit(X_train, y_train)
    # 计算袋外误差：1 - 袋外分数
    oob_error = 1 - rf.oob_score_
    # 将结果存储为Pandas Series，索引为树数量
    oob_error_list.append(pd.Series({'n_trees': n_trees, 'oob_error': oob_error}))
```

最后，将所有结果合并成一个DataFrame，并将树数量设为索引。

![](img/4a9906896b6c7764f88ffcf625efa72b_37.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_38.png)

```python
rf_oob_df = pd.concat(oob_error_list, axis=1).T.set_index('n_trees')
```

![](img/4a9906896b6c7764f88ffcf625efa72b_40.png)

运行代码后，我们将得到一个DataFrame，其中包含每个树数量对应的袋外误差。可以看到，误差最初随着树数量增加而下降，在大约100到150棵树时开始趋于稳定（即达到平台期）。

![](img/4a9906896b6c7764f88ffcf625efa72b_42.png)

为了更直观地观察这一趋势，我们可以绘制误差曲线图。

![](img/4a9906896b6c7764f88ffcf625efa72b_44.png)

```python
rf_oob_df.plot(marker='o', linewidth=5)
plt.ylabel('Out-of-Bag Error')
plt.show()
```

从图中可以清晰地看到误差的下降过程以及在约100棵树后进入平台期。需要注意的是，图中Y轴的范围可能很小（例如从0.048到0.056），这意味着在误差较低时，不同树数量之间的性能差异非常微小。因此，在选择最终模型时，应综合考虑计算成本和微小的性能提升。

![](img/4a9906896b6c7764f88ffcf625efa72b_46.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_47.png)

---

![](img/4a9906896b6c7764f88ffcf625efa72b_49.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_50.png)

## 第二部分：极端随机树的对比分析

![](img/4a9906896b6c7764f88ffcf625efa72b_52.png)

现在，我们将同样的分析方法应用于极端随机树（Extra Trees Classifier）。极端随机树与随机森林类似，但在选择节点分裂时更加随机。

![](img/4a9906896b6c7764f88ffcf625efa72b_54.png)

关键步骤与随机森林基本相同，但有一个重要区别：**必须显式设置 `bootstrap=True`**。因为默认情况下，`ExtraTreesClassifier` 的 `bootstrap` 参数为 `False`，这意味着它会使用整个数据集来拟合每棵树，从而无法计算袋外误差。袋外误差的计算依赖于自助采样（bootstrap sampling）产生的袋外数据。

![](img/4a9906896b6c7764f88ffcf625efa72b_56.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_57.png)

```python
from sklearn.ensemble import ExtraTreesClassifier

# 初始化极端随机树分类器
et = ExtraTreesClassifier(
    n_estimators=1,
    warm_start=True,
    oob_score=True,
    bootstrap=True,          # 必须设置为True才能计算袋外误差
    random_state=42,
    n_jobs=-1
)
```

![](img/4a9906896b6c7764f88ffcf625efa72b_59.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_60.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_61.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_62.png)

后续的循环、拟合、误差计算和结果存储步骤与随机森林完全一致。

```python
et_oob_list = []
for n_trees in tree_numbers:
    et.set_params(n_estimators=n_trees)
    et.fit(X_train, y_train)
    oob_error = 1 - et.oob_score_
    et_oob_list.append(pd.Series({'n_trees': n_trees, 'oob_error': oob_error}))

![](img/4a9906896b6c7764f88ffcf625efa72b_64.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_65.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_66.png)

et_oob_df = pd.concat(et_oob_list, axis=1).T.set_index('n_trees')
```

![](img/4a9906896b6c7764f88ffcf625efa72b_68.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_70.png)

---

![](img/4a9906896b6c7764f88ffcf625efa72b_72.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_74.png)

## 第三部分：模型性能对比

![](img/4a9906896b6c7764f88ffcf625efa72b_76.png)

为了比较随机森林和极端随机树的性能，我们将两个模型的误差结果合并到同一个DataFrame中。

![](img/4a9906896b6c7764f88ffcf625efa72b_78.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_79.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_81.png)

```python
combined_df = pd.concat([rf_oob_df, et_oob_df], axis=1)
combined_df.columns = ['Random Forest OOB Error', 'Extra Trees OOB Error']
```

![](img/4a9906896b6c7764f88ffcf625efa72b_83.png)

然后，我们可以将两者的误差曲线绘制在同一张图上进行对比。

![](img/4a9906896b6c7764f88ffcf625efa72b_85.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_87.png)

```python
combined_df.plot(marker='o', linewidth=5)
plt.ylabel('Out-of-Bag Error')
plt.show()
```

![](img/4a9906896b6c7764f88ffcf625efa72b_89.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_90.png)

对于这个特定的例子（通常情况也是如此），随机森林的性能略优于极端随机树。在图中，随机森林的误差线始终位于极端随机树误差线的下方。

---

![](img/4a9906896b6c7764f88ffcf625efa72b_92.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_93.png)

## 总结

![](img/4a9906896b6c7764f88ffcf625efa72b_94.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_95.png)

本节课中，我们一起学习了如何通过分析袋外误差随决策树数量变化的曲线，来确定随机森林和极端随机树模型的最佳树数量。关键发现是，模型误差会随着树数量的增加而下降，并在达到一定数量后进入平台期。此时，增加更多的树对模型性能的提升微乎其微。

![](img/4a9906896b6c7764f88ffcf625efa72b_97.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_98.png)

我们还对比了两种集成方法，并观察到在此例中随机森林的表现通常优于极端随机树。掌握这种方法有助于我们在构建模型时，在性能和计算效率之间做出平衡的决策。

![](img/4a9906896b6c7764f88ffcf625efa72b_100.png)

![](img/4a9906896b6c7764f88ffcf625efa72b_101.png)

在接下来的课程中，我们将从这些模型中选出性能更优的一个，并深入探讨其各项误差指标。