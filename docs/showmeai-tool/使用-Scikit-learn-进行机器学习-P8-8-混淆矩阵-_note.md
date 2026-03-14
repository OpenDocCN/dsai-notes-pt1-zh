# 机器学习课程 P8：混淆矩阵 📊

![](img/7fa687d432e52faf3c8af2fb5cde9289_0.png)

在本节课中，我们将学习如何评估分类模型的性能。我们将超越简单的“准确率”，深入了解模型可能犯的不同类型的错误。为此，我们将重点学习一个核心工具：**混淆矩阵**。

## 概述：从准确率到更细致的评估

![](img/7fa687d432e52faf3c8af2fb5cde9289_2.png)

![](img/7fa687d432e52faf3c8af2fb5cde9289_3.png)

上一节我们介绍了使用 `score` 方法快速评估模型。然而，单一的准确率分数有时不足以描述模型的表现。例如，在医疗诊断或金融风控中，不同类型的错误（如误诊健康人或漏诊病人）代价截然不同。因此，我们需要一种方法来区分这些错误。

![](img/7fa687d432e52faf3c8af2fb5cde9289_4.png)

本节中，我们将看看如何通过 **混淆矩阵** 来细致地分析模型的预测结果。

## 准备示例数据

![](img/7fa687d432e52faf3c8af2fb5cde9289_6.png)

首先，我们创建一些简单的虚拟数据来演示。假设我们有一个特征 `x` 和一个布尔目标值 `y`：当 `x` 为正数时 `y` 为 `True`，为负数时 `y` 为 `False`。

![](img/7fa687d432e52faf3c8af2fb5cde9289_8.png)

![](img/7fa687d432e52faf3c8af2fb5cde9289_10.png)

```python
import pandas as pd
import numpy as np

![](img/7fa687d432e52faf3c8af2fb5cde9289_12.png)

# 创建示例数据
np.random.seed(42)
data = pd.DataFrame({
    'x': np.random.randn(8),  # 8个随机数
    'y': np.random.randn(8) > 0  # y值根据x是否大于0决定（此处为演示简化逻辑）
})
# 为了演示，我们手动划分训练集和测试集（实际中应使用交叉验证或随机划分）
train_data = data.iloc[:4]
test_data = data.iloc[4:]
```

![](img/7fa687d432e52faf3c8af2fb5cde9289_14.png)

## 训练模型并获取预测

我们将使用逻辑回归模型（虽然名为“回归”，但它是一个分类器）进行训练和预测。

![](img/7fa687d432e52faf3c8af2fb5cde9289_16.png)

```python
from sklearn.linear_model import LogisticRegression

![](img/7fa687d432e52faf3c8af2fb5cde9289_17.png)

# 初始化模型
lr = LogisticRegression()
# 使用训练数据拟合模型
lr.fit(train_data[['x']], train_data['y'])
# 在测试集上进行预测
predictions = lr.predict(test_data[['x']])
# 获取测试集的真实值
actuals = test_data['y']
```

## 理解 `score` 方法

![](img/7fa687d432e52faf3c8af2fb5cde9289_19.png)

之前我们使用 `lr.score(test_data[['x']], test_data['y'])` 得到了一个分数（例如0.75）。这个 `score` 方法是模型的一个快捷评估方式。对于 `LogisticRegression`，它默认返回的是 **平均准确率**。

平均准确率的计算公式是：
`准确率 = (正确预测的数量) / (总预测数量)`

![](img/7fa687d432e52faf3c8af2fb5cde9289_21.png)

我们可以手动计算它，这有助于理解后续更复杂的指标。

```python
from sklearn.metrics import accuracy_score

# 手动计算准确率
manual_accuracy = accuracy_score(actuals, predictions)
print(f"模型准确率: {manual_accuracy}")
```

## 引入混淆矩阵

准确率告诉我们“做对了多少”，但没有告诉我们“错在哪里”。混淆矩阵可以解决这个问题。

![](img/7fa687d432e52faf3c8af2fb5cde9289_23.png)

**混淆矩阵** 是一个表格，用于描述分类模型在测试集上的性能。它显示了每个类别的样本被正确和错误分类的具体情况。

以下是使用 Scikit-learn 计算混淆矩阵的方法：

![](img/7fa687d432e52faf3c8af2fb5cde9289_25.png)

![](img/7fa687d432e52faf3c8af2fb5cde9289_26.png)

```python
from sklearn.metrics import confusion_matrix

# 计算混淆矩阵
cm = confusion_matrix(actuals, predictions)
print("混淆矩阵 (原始数组):")
print(cm)
```

为了更清晰地解读，我们将其转换为带有标签的 DataFrame：

![](img/7fa687d432e52faf3c8af2fb5cde9289_28.png)

![](img/7fa687d432e52faf3c8af2fb5cde9289_30.png)

```python
# 将混淆矩阵转换为易读的DataFrame
cm_df = pd.DataFrame(cm,
                     index=['实际 False', '实际 True'],   # 行标签：真实类别
                     columns=['预测 False', '预测 True']) # 列标签：预测类别
print("\n带标签的混淆矩阵:")
print(cm_df)
```

输出可能类似于：
```
预测 False  预测 True
实际 False         2          1
实际 True          0          1
```

## 解读混淆矩阵

![](img/7fa687d432e52faf3c8af2fb5cde9289_32.png)

![](img/7fa687d432e52faf3c8af2fb5cde9289_33.png)

在二分类问题中，混淆矩阵的四个格子有特定的名称：

*   **真阳性 (True Positive, TP)**：实际为 `True`，模型也预测为 `True`。这是正确的预测。
*   **真阴性 (True Negative, TN)**：实际为 `False`，模型也预测为 `False`。这也是正确的预测。
*   **假阳性 (False Positive, FP)**：实际为 `False`，但模型预测为 `True`。也称为 **第一类错误** 或 “误报”。
*   **假阴性 (False Negative, FN)**：实际为 `True`，但模型预测为 `False`。也称为 **第二类错误** 或 “漏报”。

我们可以从 DataFrame 中提取这些值：

![](img/7fa687d432e52faf3c8af2fb5cde9289_35.png)

```python
# 假设 cm_df 是上面得到的 DataFrame
TN = cm_df.iloc[0, 0]  # 实际False，预测False
FP = cm_df.iloc[0, 1]  # 实际False，预测True
FN = cm_df.iloc[1, 0]  # 实际True， 预测False
TP = cm_df.iloc[1, 1]  # 实际True， 预测True

print(f"真阴性(TN): {TN}")
print(f"假阳性(FP): {FP}")
print(f"假阴性(FN): {FN}")
print(f"真阳性(TP): {TP}")
```

![](img/7fa687d432e52faf3c8af2fb5cde9289_37.png)

![](img/7fa687d432e52faf3c8af2fb5cde9289_39.png)

## 从混淆矩阵衍生的关键指标

基于 TP, TN, FP, FN 这四个基本值，我们可以计算出许多更有洞察力的评估指标：

以下是几个最常用的指标及其计算公式：

![](img/7fa687d432e52faf3c8af2fb5cde9289_41.png)

1.  **精确率 (Precision)**：在所有被模型预测为阳性的样本中，真正是阳性的比例。关注 **预测的准确性**。
    `精确率 = TP / (TP + FP)`

![](img/7fa687d432e52faf3c8af2fb5cde9289_43.png)

2.  **召回率 (Recall)**：在所有实际为阳性的样本中，被模型正确预测出来的比例。关注 **找出所有正例的能力**。
    `召回率 = TP / (TP + FN)`

![](img/7fa687d432e52faf3c8af2fb5cde9289_44.png)

3.  **F1分数 (F1-Score)**：精确率和召回率的调和平均数，用于平衡两者。
    `F1 = 2 * (精确率 * 召回率) / (精确率 + 召回率)`

![](img/7fa687d432e52faf3c8af2fb5cde9289_46.png)

![](img/7fa687d432e52faf3c8af2fb5cde9289_47.png)

这些指标在 Scikit-learn 中都可以直接计算：

![](img/7fa687d432e52faf3c8af2fb5cde9289_49.png)

![](img/7fa687d432e52faf3c8af2fb5cde9289_51.png)

```python
from sklearn.metrics import precision_score, recall_score, f1_score

![](img/7fa687d432e52faf3c8af2fb5cde9289_53.png)

precision = precision_score(actuals, predictions)
recall = recall_score(actuals, predictions)
f1 = f1_score(actuals, predictions)

![](img/7fa687d432e52faf3c8af2fb5cde9289_55.png)

print(f"精确率: {precision:.2f}")
print(f"召回率: {recall:.2f}")
print(f"F1分数: {f1:.2f}")
```

![](img/7fa687d432e52faf3c8af2fb5cde9289_57.png)

## 多分类问题的混淆矩阵

![](img/7fa687d432e52faf3c8af2fb5cde9289_59.png)

混淆矩阵同样适用于多分类问题。例如，一个识别猫、狗、老鼠的图片分类器，其混淆矩阵能清晰展示模型最容易混淆哪些类别。

![](img/7fa687d432e52faf3c8af2fb5cde9289_61.png)

```python
# 多分类示例（假设已有数据）
# actuals_multi = ['狗', '猫', '狗', '老鼠', ...]
# predictions_multi = ['狗', '狗', '猫', '老鼠', ...]

# 计算时指定类别顺序
cm_multi = confusion_matrix(actuals_multi, predictions_multi, labels=['狗', '猫', '老鼠'])
cm_multi_df = pd.DataFrame(cm_multi,
                           index=['实际 狗', '实际 猫', '实际 老鼠'],
                           columns=['预测 狗', '预测 猫', '预测 老鼠'])
print(cm_multi_df)
```
通过这个矩阵，我们可以一目了然地看到：有多少只狗被误认为猫，系统是否总能正确识别老鼠等。

## 总结

本节课中我们一起学习了：

![](img/7fa687d432e52faf3c8af2fb5cde9289_63.png)

1.  **模型评估的局限性**：单一的准确率分数无法区分错误类型。
2.  **混淆矩阵的构建与解读**：我们学习了如何使用 `sklearn.metrics.confusion_matrix` 计算混淆矩阵，并将其转换为易于解读的 DataFrame 格式。
3.  **核心概念**：我们明确了 **真阳性(TP)、真阴性(TN)、假阳性(FP)、假阴性(FN)** 的定义，它们是所有高级评估指标的基础。
4.  **衍生指标**：基于混淆矩阵，我们介绍了 **精确率、召回率和F1分数** 这三个关键指标，它们分别从不同角度衡量模型性能，适用于对不同类型的错误有不同容忍度的场景（如医疗诊断、垃圾邮件过滤）。
5.  **应用范围**：混淆矩阵不仅适用于二分类问题，也能直观地展示多分类模型中各类别之间的混淆情况。

![](img/7fa687d432e52faf3c8af2fb5cde9289_65.png)

混淆矩阵是理解和沟通分类模型性能的基石。掌握它，你就能更全面、更有说服力地分析和改进你的机器学习模型。