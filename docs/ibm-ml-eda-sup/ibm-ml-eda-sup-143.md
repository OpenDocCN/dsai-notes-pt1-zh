# 143：梯度提升与AdaBoost模型实践 📈

![](img/d970090f0a2e272b44b760d16fe1e7e5_1.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_2.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_4.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_5.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_6.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_7.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_8.png)

在本节课中，我们将学习如何使用梯度提升（Gradient Boosting）和AdaBoost模型。我们将通过循环不同的树数量来观察模型准确率的变化，并使用网格搜索与交叉验证来优化模型参数。课程内容将涵盖模型训练、性能评估以及如何保存和加载训练好的模型。

![](img/d970090f0a2e272b44b760d16fe1e7e5_10.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_11.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_12.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_13.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_14.png)

---

![](img/d970090f0a2e272b44b760d16fe1e7e5_16.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_17.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_19.png)

## 梯度提升模型实践 🌲

![](img/d970090f0a2e272b44b760d16fe1e7e5_21.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_22.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_24.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_25.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_27.png)

上一节我们介绍了集成学习的概念，本节中我们来看看如何具体实现梯度提升模型。我们的目标是循环使用不同数量的树来构建模型，并观察错误率如何随着树的数量增加而变化。

![](img/d970090f0a2e272b44b760d16fe1e7e5_28.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_30.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_31.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_33.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_34.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_35.png)

为了加速模型拟合过程，我们将最大特征数（`max_features`）设置为5。这是因为我们的数据集包含561个特征，如果使用全部特征，模型训练将非常耗时。虽然这可能会略微影响模型性能，但在此例中，准确率大致相同。

![](img/d970090f0a2e272b44b760d16fe1e7e5_37.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_39.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_40.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_41.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_43.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_45.png)

以下是实现步骤：

![](img/d970090f0a2e272b44b760d16fe1e7e5_47.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_48.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_50.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_52.png)

1.  导入必要的库：`GradientBoostingClassifier` 和 `accuracy_score`。
2.  定义一个树数量的列表，例如从15到400。
3.  初始化一个空列表来存储每个模型对应的错误率。
4.  循环遍历树数量列表中的每一个值。

![](img/d970090f0a2e272b44b760d16fe1e7e5_54.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_55.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_57.png)

在循环的每一步中，我们需要：

![](img/d970090f0a2e272b44b760d16fe1e7e5_59.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_60.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_62.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_63.png)

*   初始化梯度提升分类器，设置 `n_estimators` 为当前循环的树数量，`max_features=5`，`random_state=42`。
*   在训练集（`X_train`, `y_train`）上拟合模型。
*   使用拟合好的模型对测试集（`X_test`）进行预测。
*   计算预测的准确率，错误率 = 1 - 准确率。
*   将（树数量，错误率）作为一个序列保存起来。

![](img/d970090f0a2e272b44b760d16fe1e7e5_65.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_66.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_67.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_69.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_70.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_71.png)

循环结束后，将所有序列合并成一个数据框，以便分析和绘图。

![](img/d970090f0a2e272b44b760d16fe1e7e5_73.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_75.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_76.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_77.png)

运行上述代码后，我们会得到一个错误率数据框。通过绘制错误率随树数量变化的曲线，我们可以观察到：随着树数量的增加，错误率持续下降；但当树数量达到较高值时（如200到400棵），错误率的下降速度减缓，呈现收益递减的趋势。

![](img/d970090f0a2e272b44b760d16fe1e7e5_79.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_80.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_81.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_83.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_84.png)

---

![](img/d970090f0a2e272b44b760d16fe1e7e5_85.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_86.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_88.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_89.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_91.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_93.png)

## 网格搜索优化梯度提升模型 ⚙️

![](img/d970090f0a2e272b44b760d16fe1e7e5_94.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_96.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_97.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_98.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_100.png)

在观察了树数量对模型的影响后，本节我们将使用网格搜索结合交叉验证来进一步优化梯度提升模型。我们将固定树的数量为400棵，重点调整学习率、子采样率和最大特征数这几个关键参数。

![](img/d970090f0a2e272b44b760d16fe1e7e5_102.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_103.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_104.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_106.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_108.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_109.png)

以下是参数网格的定义：

![](img/d970090f0a2e272b44b760d16fe1e7e5_111.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_113.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_114.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_115.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_117.png)

*   **学习率 (`learning_rate`)**：尝试 `[0.1, 0.01, 0.001]`。学习率控制每棵树对最终结果的贡献权重，较低的学习率通常需要更多的树。
*   **子采样率 (`subsample`)**：尝试 `[1.0, 0.5]`。子采样率表示每棵树训练时使用的数据比例，小于1.0可以引入正则化效果，防止过拟合。
*   **最大特征数 (`max_features`)**：尝试 `[2, 3, 4]`。限制每棵树分裂时考虑的特征数量，也是一种正则化手段。

![](img/d970090f0a2e272b44b760d16fe1e7e5_119.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_120.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_122.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_124.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_125.png)

我们使用 `GridSearchCV` 进行搜索，以准确率作为优化目标。由于提升模型是顺序训练的，无法像装袋法那样高度并行化，因此网格搜索会花费较长时间。

![](img/d970090f0a2e272b44b760d16fe1e7e5_127.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_128.png)

模型训练完成后，我们可以查看最优参数组合。例如，可能得到 `{‘learning_rate’: 0.1, ‘max_features’: 4, ‘subsample’: 0.5}`。使用这个最优模型在测试集上进行预测，并生成分类报告和混淆矩阵。

![](img/d970090f0a2e272b44b760d16fe1e7e5_130.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_131.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_133.png)

结果显示，优化后的梯度提升模型性能优异，整体准确率达到99%。混淆矩阵显示，主要的错误发生在“坐着”和“站着”这两个容易混淆的类别之间。

![](img/d970090f0a2e272b44b760d16fe1e7e5_135.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_137.png)

**重要提示**：由于训练耗时，建议使用 `pickle` 库保存训练好的模型对象，以便后续直接加载使用，无需重新训练。

![](img/d970090f0a2e272b44b760d16fe1e7e5_139.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_140.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_141.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_143.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_145.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_147.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_149.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_150.png)

保存模型的代码如下：
```python
import pickle
with open(‘gbc_model.pkl‘, ‘wb‘) as f:
    pickle.dump(grid_search_model, f)
```
加载模型的代码如下：
```python
with open(‘gbc_model.pkl‘, ‘rb‘) as f:
    loaded_model = pickle.load(f)
```

![](img/d970090f0a2e272b44b760d16fe1e7e5_152.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_153.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_154.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_156.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_157.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_158.png)

---

![](img/d970090f0a2e272b44b760d16fe1e7e5_160.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_161.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_162.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_164.png)

## AdaBoost模型实践与对比 ⚖️

![](img/d970090f0a2e272b44b760d16fe1e7e5_165.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_166.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_168.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_170.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_171.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_173.png)

最后，我们将同样的网格搜索方法应用于AdaBoost模型，并与梯度提升模型的结果进行对比。AdaBoost默认使用决策树桩（`max_depth=1`）作为弱学习器。

![](img/d970090f0a2e272b44b760d16fe1e7e5_175.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_176.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_178.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_180.png)

我们设置类似的参数网格进行搜索，可能得到的最优参数是 `{‘learning_rate’: 0.01, ‘n_estimators’: 100}`。使用最优的AdaBoost模型进行预测并评估。

![](img/d970090f0a2e272b44b760d16fe1e7e5_182.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_183.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_184.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_186.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_187.png)

通过对比分类报告可以发现，在这个特定数据集上，梯度提升模型的性能（准确率99%）显著优于AdaBoost模型。AdaBoost模型的召回率、F1分数和准确率都较低，混淆矩阵显示其在类别1和类别2上出现了较多的错误分类。

![](img/d970090f0a2e272b44b760d16fe1e7e5_189.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_190.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_191.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_193.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_194.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_196.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_197.png)

这种差异可能是因为AdaBoost的损失函数对异常值更为敏感，而梯度提升通过更稳健的损失函数（如偏差）能够获得更好的性能。这个例子说明了在实际问题中尝试不同模型并进行比较的重要性。

![](img/d970090f0a2e272b44b760d16fe1e7e5_199.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_201.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_203.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_205.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_206.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_207.png)

---

![](img/d970090f0a2e272b44b760d16fe1e7e5_209.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_211.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_212.png)

![](img/d970090f0a2e272b44b760d16fe1e7e5_213.png)

本节课中我们一起学习了梯度提升和AdaBoost模型的实现与调优。我们实践了通过循环改变树的数量来观察模型性能趋势，并使用网格搜索交叉验证来寻找最优超参数。我们还学习了如何使用 `pickle` 保存和加载模型。最后，通过对比两个模型的结果，我们了解到不同提升算法在不同数据集上可能表现各异。在接下来的课程中，我们将开始学习 stacking 集成方法和投票分类器。