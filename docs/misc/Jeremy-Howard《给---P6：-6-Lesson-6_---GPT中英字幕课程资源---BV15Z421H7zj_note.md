# 课程6：表格数据与决策树 🌳

![](img/7383febe20d317edb4f4d72ed5e239c6_1.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_3.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_5.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_7.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_9.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_11.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_13.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_14.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_16.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_18.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_20.png)

在本节课中，我们将要学习如何处理表格数据，并深入探讨决策树和随机森林这两种强大的机器学习模型。我们将从泰坦尼克号数据集入手，一步步构建模型，并理解其背后的原理。

![](img/7383febe20d317edb4f4d72ed5e239c6_22.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_24.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_26.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_28.png)

---

![](img/7383febe20d317edb4f4d72ed5e239c6_30.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_32.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_34.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_36.png)

## 从单一规则到决策树

![](img/7383febe20d317edb4f4d72ed5e239c6_38.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_40.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_41.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_43.png)

上一节我们介绍了如何为表格数据寻找一个好的二元分割。本节中我们来看看如何将多个这样的分割组合起来，形成一棵决策树。

![](img/7383febe20d317edb4f4d72ed5e239c6_45.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_47.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_49.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_51.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_53.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_55.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_57.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_59.png)

我们之前使用泰坦尼克号数据集，通过寻找最佳分割（例如基于性别 `sex` 或票价对数 `log_fare`）来预测乘客是否幸存。我们使用了一个简单的评分标准来衡量分割的好坏，即分割后两组内幸存率的标准差要尽可能小。

![](img/7383febe20d317edb4f4d72ed5e239c6_61.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_63.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_65.png)

我们找到了一个非常好的单一规则（One Rule）：根据性别进行分割。女性幸存率很高，而男性幸存率很低。

![](img/7383febe20d317edb4f4d72ed5e239c6_67.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_69.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_71.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_73.png)

**公式：** 对于一个二元分割，我们希望最大化组内同质性。一种常见的度量是基尼不纯度（Gini Impurity），对于二分类问题，其公式为：
`Gini = 1 - (p_survived^2 + p_died^2)`
其中 `p` 是组内某类别的比例。基尼值越低，说明该组越“纯净”。

![](img/7383febe20d317edb4f4d72ed5e239c6_75.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_77.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_79.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_81.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_83.png)

为了进一步提升，我们可以对分割后的每个子组再次进行分割。例如，在“男性”组内，我们可以再次寻找最佳分割变量（如年龄 `age`）；在“女性”组内，也可以再次寻找（如船舱等级 `pclass`）。

以下是手动实现这一过程的代码思路：
```python
# 伪代码：在男性子集中寻找最佳分割
male_data = df[df[‘sex’] == ‘male’]
best_split_for_males = find_best_split(male_data, ‘survived’)
```

这样我们就得到了一棵简单的决策树：首先根据性别分割，然后根据年龄分割男性，根据船舱等级分割女性。

![](img/7383febe20d317edb4f4d72ed5e239c6_85.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_87.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_89.png)

---

![](img/7383febe20d317edb4f4d72ed5e239c6_91.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_93.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_95.png)

## 使用 Scikit-Learn 构建决策树

![](img/7383febe20d317edb4f4d72ed5e239c6_97.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_99.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_101.png)

手动构建多层分割很繁琐。我们可以使用 `scikit-learn` 库中的 `DecisionTreeClassifier` 来自动完成这个过程。

![](img/7383febe20d317edb4f4d72ed5e239c6_103.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_105.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_107.png)

以下是构建并可视化一棵决策树的关键步骤：
```python
from sklearn.tree import DecisionTreeClassifier
from sklearn import tree
import matplotlib.pyplot as plt

![](img/7383febe20d317edb4f4d72ed5e239c6_109.png)

# 创建决策树分类器，限制最多4个叶节点
m = DecisionTreeClassifier(max_leaf_nodes=4)
m.fit(X, y) # X是特征，y是标签

![](img/7383febe20d317edb4f4d72ed5e239c6_111.png)

# 绘制决策树
plt.figure(figsize=(12,8))
tree.plot_tree(m, feature_names=X.columns, filled=True)
plt.show()
```

![](img/7383febe20d317edb4f4d72ed5e239c6_113.png)

决策树的一个巨大优势是**需要极少的预处理**。它不关心特征的分布（如异常值、长尾分布），也能直接处理用数字编码的分类变量，通常不需要创建哑变量（dummy variables）。这使得决策树成为探索性数据分析（EDA）和建立基线的绝佳起点。

![](img/7383febe20d317edb4f4d72ed5e239c6_115.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_116.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_118.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_120.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_122.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_124.png)

对于表格数据，我总是**首先使用基于决策树的方法**来建立基线，因为它很难出错。

---

![](img/7383febe20d317edb4f4d72ed5e239c6_126.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_128.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_130.png)

## 从决策树到随机森林 🌲🌲🌲

![](img/7383febe20d317edb4f4d72ed5e239c6_132.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_134.png)

单一决策树的能力有限，容易受到数据中随机波动的影响。为了获得更强大、更稳定的模型，我们可以使用**集成学习**方法。

本节中我们来看看如何通过“装袋法”（Bagging）来集成多个决策树，形成随机森林。

![](img/7383febe20d317edb4f4d72ed5e239c6_136.png)

核心思想由 Leo Breiman 提出：如果我们有很多个**无偏但彼此不相关**的弱模型（例如浅层决策树），那么对它们的预测结果取平均，其误差的平均值会趋近于零，从而得到比任何单个模型都更好的预测。

![](img/7383febe20d317edb4f4d72ed5e239c6_138.png)

**如何生成多个不同的弱模型？**
一个简单的方法是：每次训练时，从数据中**随机抽取一个子集**（例如50%或75%的行），并用这个子集构建一棵决策树。重复这个过程很多次。

![](img/7383febe20d317edb4f4d72ed5e239c6_140.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_142.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_144.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_146.png)

这样生成的模型集合称为**随机森林**（Random Forest）。在构建每棵树时，随机森林通常还会**随机选择一部分特征**进行分割，以进一步降低树之间的相关性。

![](img/7383febe20d317edb4f4d72ed5e239c6_148.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_150.png)

以下是手动实现一个简化版随机森林的代码逻辑：
```python
def rf_predict(X, n_trees=100, sample_frac=0.75):
    predictions = []
    for i in range(n_trees):
        # 1. 随机抽取样本子集
        X_sample = X.sample(frac=sample_frac)
        # 2. 训练一棵决策树
        tree = DecisionTreeClassifier().fit(X_sample, y_sample)
        # 3. 记录预测
        predictions.append(tree.predict(X))
    # 4. 对所有树的预测取平均（分类问题可取众数）
    return np.mean(predictions, axis=0)
```

![](img/7383febe20d317edb4f4d72ed5e239c6_152.png)

在实践中，我们直接使用 `scikit-learn` 的 `RandomForestClassifier`：
```python
from sklearn.ensemble import RandomForestClassifier
m = RandomForestClassifier(n_estimators=100, min_samples_leaf=50)
m.fit(X_train, y_train)
preds = m.predict(X_valid)
```

![](img/7383febe20d317edb4f4d72ed5e239c6_154.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_156.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_158.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_160.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_162.png)

在大多数合理大小的真实数据集上，随机森林几乎总是比单棵决策树表现更好。它继承了决策树的所有优点（无需复杂预处理、抗异常值、可处理分类变量），同时通过集成大大提升了准确性和鲁棒性。

---

## 随机森林的强大工具：特征重要性

![](img/7383febe20d317edb4f4d72ed5e239c6_164.png)

随机森林不仅能做预测，还能为我们提供关于数据的深刻洞见。其中最重要的工具之一就是**特征重要性**（Feature Importance）图。

![](img/7383febe20d317edb4f4d72ed5e239c6_166.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_168.png)

以下是计算特征重要性的基本原理：
1.  对于森林中的每一棵决策树，记录每个特征被用于分割的次数，以及每次分割时基尼不纯度的减少量（即该分割带来的“信息增益”）。
2.  对所有树的结果进行汇总，对每个特征，将其带来的基尼不纯度减少总量进行平均或求和。
3.  绘制图表，显示每个特征对模型预测的整体贡献程度。

![](img/7383febe20d317edb4f4d72ed5e239c6_170.png)

在泰坦尼克号的例子中，特征重要性图会清晰地显示 `sex`（性别）是最重要的特征，其次是 `pclass`（船舱等级），其他特征的重要性则远远落后。

![](img/7383febe20d317edb4f4d72ed5e239c6_172.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_174.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_176.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_178.png)

**为什么这很有用？**
*   **数据理解**：快速识别数据集中最具预测力的关键变量。
*   **特征工程**：可以专注于清洗和优化那些重要的特征。
*   **降维**：可以安全地移除重要性很低的特征，简化模型。

![](img/7383febe20d317edb4f4d72ed5e239c6_180.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_182.png)

对于任何新的表格数据集，我建议首先运行一个随机森林并绘制特征重要性图。这通常只需几秒到几分钟，就能让你迅速抓住问题的关键。

![](img/7383febe20d317edb4f4d72ed5e239c6_184.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_186.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_188.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_190.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_192.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_194.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_196.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_198.png)

---

![](img/7383febe20d317edb4f4d72ed5e239c6_200.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_202.png)

## 模型解释与部分依赖图

![](img/7383febe20d317edb4f4d72ed5e239c6_204.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_206.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_208.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_210.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_212.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_214.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_216.png)

除了特征重要性，随机森林还支持其他强大的解释性工具：

![](img/7383febe20d317edb4f4d72ed5e239c6_218.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_220.png)

1.  **预测置信度**：对于随机森林的预测，我们可以计算所有树预测结果的**标准差**。标准差高意味着不同树的预测差异大，表明模型对该样本的预测不确定性高。

![](img/7383febe20d317edb4f4d72ed5e239c6_222.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_224.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_226.png)

2.  **部分依赖图**：用于理解单个特征与预测目标之间的关系，同时保持其他所有特征不变。
    *   **方法**：将数据集中某个特征（如 `year_made`）的值统一设置为某个特定值（如1950），然后用模型对所有样本进行预测并取平均。重复此过程遍历该特征的所有值，然后绘制平均预测值随该特征值变化的曲线。
    *   **意义**：它显示了“在其他条件相同的情况下”，该特征如何影响预测结果。这对于理解模型行为和进行敏感性分析至关重要。

3.  **单样本预测解释**：类似于特征重要性，但针对**单个预测**。可以分析对于某个特定的乘客，哪些特征对其预测结果（幸存或遇难）的影响最大，以及影响的方向（是提高还是降低了幸存概率）。这有助于回答“为什么模型对这个样本做出了这样的预测？”

![](img/7383febe20d317edb4f4d72ed5e239c6_228.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_230.png)

---

![](img/7383febe20d317edb4f4d72ed5e239c6_232.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_234.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_236.png)

## 实践演练：Kaggle竞赛迭代流程 🚀

![](img/7383febe20d317edb4f4d72ed5e239c6_238.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_240.png)

理论学习之后，让我们看看如何将这些知识应用于实践。我将以Kaggle上的“水稻病害分类”竞赛为例，简述一个高效的迭代流程。

![](img/7383febe20d317edb4f4d72ed5e239c6_242.png)

**核心哲学**：
*   **快速迭代**：构建一个能在一两分钟内完成训练和评估的流程。速度是关键，它允许你尝试大量想法。
*   **尽早提交**：不要追求完美。尽快向Kaggle提交一个基线模型（即使很差），然后每天尝试改进并提交。排行榜的反馈是残酷而真实的。
*   **关注验证集**：确保你的验证集能可靠地反映模型在未知数据上的表现。

![](img/7383febe20d317edb4f4d72ed5e239c6_244.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_246.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_248.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_250.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_252.png)

**我的迭代步骤**：
1.  **基线模型**：使用`fastai`库快速构建一个图像分类流程，选择训练速度快的模型（如ResNet26d），训练几分钟后立即提交。首次提交排名后80%。
2.  **加速与改进**：
    *   **发现瓶颈**：在Kaggle上训练时发现CPU是瓶颈（数据加载慢）。
    *   **预处理**：将训练图像预先缩放到统一尺寸，大幅减少每个epoch的训练时间。
    *   **升级模型**：根据我们之前的研究，换用更高效的架构（如ConvNeXt），在相似速度下获得显著更低的错误率。
3.  **尝试不同技巧**：
    *   **数据增强**：比较不同的图像预处理方法（挤压、裁剪、填充）。
    *   **测试时增强**：对验证集/测试集的图像进行多种增强，将预测结果平均，以提升稳定性（TTA）。
    *   **调整输入尺寸**：尝试使用原始图像的矩形比例进行缩放，而非强制转为正方形。
4.  **持续迭代与提交**：每做一个有希望的改动，就评估其效果，并提交到Kaggle。通过这个过程，我的模型错误率从12%降到了2%以下，排名进入了前25%。

![](img/7383febe20d317edb4f4d72ed5e239c6_254.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_256.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_258.png)

**关于工具和流程**：
*   我使用`fastkaggle`库来简化数据下载和提交流程。
*   我为每个主要的实验步骤创建单独的Jupyter Notebook，并按顺序命名，以保持组织性。
*   我会将优秀的Notebook公开分享到Kaggle，这不仅是为了分享代码，也是锻炼清晰、有说服力地传达结果的能力。

![](img/7383febe20d317edb4f4d72ed5e239c6_260.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_262.png)

**关于AutoML和超参数调优**：
*   我几乎从不使用AutoML或复杂的超参数网格搜索。
*   我更倾向于像科学家一样工作：提出假设（例如，“模型A是否比模型B更适合这个数据？”），设计简单的对照实验来验证，然后得出结论。
*   对于学习率，我使用`学习率查找器`，而不是盲目搜索。
*   我的经验法则是：**随机森林**最快、最省心；**梯度提升机**通常更准但需要调参；**深度学习**在图像、文本等领域有优势，并且要利用迁移学习。

---

![](img/7383febe20d317edb4f4d72ed5e239c6_264.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_266.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_268.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_270.png)

## 总结

![](img/7383febe20d317edb4f4d72ed5e239c6_272.png)

本节课中我们一起学习了：
1.  **决策树**：一种通过一系列二元分割进行预测的直观模型，需要极少的预处理，是表格数据分析的绝佳起点。
2.  **随机森林**：通过集成大量决策树，利用“装袋法”降低方差，得到更强大、更稳定的模型。它继承了决策树的易用性，并显著提升了性能。
3.  **模型解释**：随机森林提供了强大的工具来理解模型，包括**特征重要性**、**部分依赖图**和**单样本预测解释**，这些对于理解数据和建立信任至关重要。
4.  **实践方法论**：我们通过一个Kaggle竞赛案例，展示了如何通过快速迭代、科学实验和关注核心问题（如验证集、计算瓶颈）来有效地提升模型性能。

![](img/7383febe20d317edb4f4d72ed5e239c6_274.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_276.png)

![](img/7383febe20d317edb4f4d72ed5e239c6_278.png)

记住，对于表格数据，从随机森林开始总是一个安全而有效的选择。它不仅能给你一个坚实的基线，还能为你提供深入理解数据的窗口。