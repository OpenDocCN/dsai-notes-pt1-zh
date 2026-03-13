# 100：逻辑回归实验（第三部分）📊

![](img/9472d88b9d2ea4daf1fd2c7beca99026_1.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_3.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_5.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_6.png)

在本节课中，我们将学习如何利用训练好的逻辑回归模型进行预测，并计算多种评估指标来量化模型的性能。我们将重点关注预测标签、概率输出以及如何计算精确度、召回率、F1分数、准确率和ROC AUC曲线下面积。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_8.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_9.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_10.png)

---

![](img/9472d88b9d2ea4daf1fd2c7beca99026_12.png)

上一节我们完成了逻辑回归模型的训练。本节中，我们将使用测试集进行预测，并系统地评估模型性能。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_14.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_16.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_18.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_20.png)

## 生成预测与概率

![](img/9472d88b9d2ea4daf1fd2c7beca99026_22.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_24.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_25.png)

首先，我们需要为每个模型生成预测标签和对应的类别概率。预测标签用于计算准确率等指标，而概率则用于绘制ROC曲线。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_27.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_28.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_29.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_31.png)

以下是实现步骤：

![](img/9472d88b9d2ea4daf1fd2c7beca99026_33.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_34.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_36.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_38.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_40.png)

1.  **初始化存储结构**：创建两个空列表，分别用于存放所有模型的预测结果和概率结果。
    ```python
    y_pred = []          # 存储预测标签
    y_prob = []          # 存储预测概率
    ```

![](img/9472d88b9d2ea4daf1fd2c7beca99026_41.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_43.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_45.png)

2.  **准备模型与标签**：将模型名称和模型对象组合在一起，以便循环调用。
    ```python
    coef_labels = ['LR', 'L1', 'L2']
    coef_models = [log_reg, log_reg_l1, log_reg_l2]
    ```

![](img/9472d88b9d2ea4daf1fd2c7beca99026_47.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_49.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_50.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_51.png)

3.  **循环进行预测**：遍历每个模型，对测试集 `X_test` 进行预测。
    *   使用 `model.predict(X_test)` 获取预测标签，并将其存储为Pandas Series，以模型名称命名。
    *   使用 `model.predict_proba(X_test)` 获取每个样本属于各个类别的概率。这将生成一个形状为 `(n_samples, n_classes)` 的数组。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_53.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_54.png)

4.  **组织概率数据**：为概率数据创建一个多级索引的DataFrame。第一级索引是模型名称（如‘LR’），第二级索引是类别标签（0, 1, 2...）。这样能清晰地区分不同模型、不同类别的预测概率。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_56.png)

运行完成后，`y_pred` 将包含三个Series，分别对应三个模型的预测标签。`y_prob` 是一个具有多级列索引的DataFrame，方便我们按模型和类别提取概率。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_58.png)

## 计算模型评估指标 🧮

![](img/9472d88b9d2ea4daf1fd2c7beca99026_60.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_62.png)

获得预测结果后，我们可以计算多种指标来评估模型性能。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_64.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_66.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_68.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_69.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_71.png)

首先需要导入必要的评估函数：
```python
from sklearn.metrics import precision_recall_fscore_support, confusion_matrix, accuracy_score, roc_auc_score, label_binarize
```

![](img/9472d88b9d2ea4daf1fd2c7beca99026_73.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_75.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_76.png)

以下是核心指标的计算方法：

![](img/9472d88b9d2ea4daf1fd2c7beca99026_78.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_79.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_80.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_82.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_83.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_85.png)

*   **精确度、召回率与F1分数**：使用 `precision_recall_fscore_support(y_true, y_pred, average='weighted')`。参数 `average='weighted'` 会考虑每个类别的样本数量，计算加权平均值，从而得到一个整体的分数。
*   **准确率**：使用 `accuracy_score(y_true, y_pred)`，计算正确预测的样本比例。
*   **ROC AUC分数**：使用 `roc_auc_score(y_true, y_prob, multi_class='ovr')`。计算多分类ROC曲线下面积时，需要将真实标签 `y_true` 进行二值化处理（使用 `label_binarize`），并且传入的是预测概率 `y_prob` 而非标签。
*   **混淆矩阵**：使用 `confusion_matrix(y_true, y_pred)`。它可以直观展示每个类别被正确分类和误分类的情况。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_87.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_88.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_90.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_91.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_92.png)

我们将为LR、L1、L2三个模型分别计算上述指标，并将结果汇总在一个DataFrame中，便于比较。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_94.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_96.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_98.png)

## 可视化混淆矩阵 📈

![](img/9472d88b9d2ea4daf1fd2c7beca99026_99.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_101.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_102.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_104.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_106.png)

混淆矩阵是理解模型错误类型的强大工具。我们将为三个模型分别绘制混淆矩阵热力图。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_108.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_109.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_110.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_112.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_114.png)

实现步骤如下：

![](img/9472d88b9d2ea4daf1fd2c7beca99026_116.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_118.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_119.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_121.png)

1.  创建一个2x2的子图布局（虽然只有三个模型）。
2.  遍历前三个子图和对应的模型标签。
3.  在每个子图中，使用Seaborn的 `heatmap` 函数绘制对应模型的混淆矩阵。
4.  通过颜色深浅和标注的数值，可以快速识别模型最容易混淆的类别对。理想情况下，热力图的主对角线颜色最深。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_123.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_124.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_125.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_126.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_128.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_130.png)

例如，分析热力图后，我们可能发现模型经常将“坐着”（Sitting）和“站着”（Standing）这两个动作混淆，这与现实场景中某些设备（如智能手表）的识别难点相似。

![](img/9472d88b9d2ea4daf1fd2c7beca99026_132.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_134.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_136.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_138.png)

---

![](img/9472d88b9d2ea4daf1fd2c7beca99026_140.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_141.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_142.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_143.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_145.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_146.png)

![](img/9472d88b9d2ea4daf1fd2c7beca99026_147.png)

本节课中我们一起学习了逻辑回归模型评估的完整流程。我们掌握了如何获取预测标签和概率，计算了包括精确度、召回率、F1分数、准确率和ROC AUC在内的关键评估指标，并通过混淆矩阵可视化了模型的错误模式。这些技能是评估任何分类模型性能的基础。接下来，在课程中我们将开始学习K近邻算法。