# 108：K最近邻算法实践（第三部分）📊

![](img/c5a28d261316d48ae10768633c59d107_1.png)

在本节课中，我们将学习如何应用K最近邻（KNN）算法解决一个分类问题。我们将从数据预处理后的步骤开始，逐步完成特征与目标变量的划分、模型训练、评估，并通过调整K值来优化模型性能。

![](img/c5a28d261316d48ae10768633c59d107_2.png)

---

![](img/c5a28d261316d48ae10768633c59d107_4.png)

## 数据划分与模型初始化 🔧

![](img/c5a28d261316d48ae10768633c59d107_6.png)

上一节我们完成了数据预处理，本节中我们来看看如何将数据划分为训练集和测试集，并初始化我们的KNN模型。

首先，我们需要导入必要的库和函数。

```python
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import confusion_matrix, accuracy_score, f1_score, classification_report
```

![](img/c5a28d261316d48ae10768633c59d107_8.png)

接着，我们将数据分为特征变量（X）和目标变量（y）。目标变量是“客户流失”（churn），其余所有列都是特征。

![](img/c5a28d261316d48ae10768633c59d107_10.png)

```python
y = df['churn_value']
X = df.drop('churn_value', axis=1)
```

![](img/c5a28d261316d48ae10768633c59d107_12.png)

然后，我们使用 `train_test_split` 函数将数据划分为训练集和测试集，测试集占比为40%。

![](img/c5a28d261316d48ae10768633c59d107_14.png)

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, random_state=42)
```

数据划分完成后，我们初始化一个K值为3的K最近邻分类器，并用训练数据对其进行拟合。

![](img/c5a28d261316d48ae10768633c59d107_16.png)

![](img/c5a28d261316d48ae10768633c59d107_17.png)

```python
knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X_train, y_train)
```

---

![](img/c5a28d261316d48ae10768633c59d107_19.png)

![](img/c5a28d261316d48ae10768633c59d107_20.png)

## 模型评估与性能分析 📈

模型训练完成后，我们需要评估其在测试集上的表现。以下是评估模型性能的步骤。

![](img/c5a28d261316d48ae10768633c59d107_22.png)

首先，使用训练好的模型对测试集进行预测。

![](img/c5a28d261316d48ae10768633c59d107_24.png)

```python
y_pred = knn.predict(X_test)
```

为了全面评估模型，我们将查看分类报告、准确率分数和F1分数。分类报告会为每个类别（流失与未流失）提供精确率、召回率和F1分数。

![](img/c5a28d261316d48ae10768633c59d107_26.png)

![](img/c5a28d261316d48ae10768633c59d107_27.png)

```python
print(classification_report(y_test, y_pred))
print("Accuracy Score:", accuracy_score(y_test, y_pred))
print("F1 Score:", f1_score(y_test, y_pred))
```

![](img/c5a28d261316d48ae10768633c59d107_29.png)

分类报告的输出类似于以下结构：
- 对于标签`0`（未流失）：精确率约为0.90，召回率约为0.92。
- 对于标签`1`（流失）：精确率和召回率相对较低。
- 整体准确率约为86%。
- 数据存在不平衡，未流失的样本（2048个）远多于流失的样本。

![](img/c5a28d261316d48ae10768633c59d107_30.png)

接下来，我们通过混淆矩阵来可视化模型的预测结果，更直观地查看正确和错误的分类情况。

![](img/c5a28d261316d48ae10768633c59d107_32.png)

![](img/c5a28d261316d48ae10768633c59d107_33.png)

```python
cm = confusion_matrix(y_test, y_pred)
print(cm)
```

混淆矩阵的输出是一个2x2的数组：
- 行代表真实标签（`0` 和 `1`）。
- 列代表预测标签（`0` 和 `1`）。
- 对角线上的值（1880和554）是正确预测的数量。
- 非对角线上的值（168和216）是错误预测的数量。

为了更清晰地展示，我们可以使用热力图来绘制混淆矩阵。

![](img/c5a28d261316d48ae10768633c59d107_35.png)

```python
import matplotlib.pyplot as plt
import seaborn as sns

![](img/c5a28d261316d48ae10768633c59d107_37.png)

![](img/c5a28d261316d48ae10768633c59d107_38.png)

fig, ax = plt.subplots(figsize=(12, 12))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', ax=ax,
            xticklabels=['Not Churn', 'Churn'],
            yticklabels=['Not Churn', 'Churn'])
ax.set_xlabel('Predicted')
ax.set_ylabel('Actual')
ax.set_title('Confusion Matrix Heatmap')
plt.show()
```

![](img/c5a28d261316d48ae10768633c59d107_40.png)

---

![](img/c5a28d261316d48ae10768633c59d107_42.png)

![](img/c5a28d261316d48ae10768633c59d107_43.png)

## 调整K值以优化模型 ⚙️

在上一节中，我们使用了K=3的模型。本节中我们来看看如果将K值增加到5，模型的性能会发生什么变化。

![](img/c5a28d261316d48ae10768633c59d107_45.png)

![](img/c5a28d261316d48ae10768633c59d107_46.png)

我们使用相同的训练集和测试集，但将KNN分类器的`n_neighbors`参数改为5。

```python
knn_5 = KNeighborsClassifier(n_neighbors=5)
knn_5.fit(X_train, y_train)
y_pred_5 = knn_5.predict(X_test)
```

![](img/c5a28d261316d48ae10768633c59d107_48.png)

![](img/c5a28d261316d48ae10768633c59d107_49.png)

![](img/c5a28d261316d48ae10768633c59d107_51.png)

再次评估模型性能。

![](img/c5a28d261316d48ae10768633c59d107_53.png)

![](img/c5a28d261316d48ae10768633c59d107_55.png)

```python
print(classification_report(y_test, y_pred_5))
print("Accuracy Score (K=5):", accuracy_score(y_test, y_pred_5))
print("F1 Score (K=5):", f1_score(y_test, y_pred_5))
```

![](img/c5a28d261316d48ae10768633c59d107_57.png)

![](img/c5a28d261316d48ae10768633c59d107_59.png)

与K=3的结果相比，我们观察到：
- 对于流失客户（标签`1`）的精确率、召回率和F1分数有所提升。
- 整体准确率略有提高。
- F1分数从0.74提升到了0.76。

绘制K=5时的混淆矩阵热力图，可以直观地看到正确预测的数量（对角线上的1889和573）比K=3时更多。

![](img/c5a28d261316d48ae10768633c59d107_61.png)

![](img/c5a28d261316d48ae10768633c59d107_62.png)

![](img/c5a28d261316d48ae10768633c59d107_63.png)

![](img/c5a28d261316d48ae10768633c59d107_64.png)

---

![](img/c5a28d261316d48ae10768633c59d107_65.png)

## 寻找最佳K值 🎯

![](img/c5a28d261316d48ae10768633c59d107_67.png)

为了找到最适合此数据集的K值，我们将系统地测试K从1到40的所有取值，并观察F1分数和错误率的变化。

![](img/c5a28d261316d48ae10768633c59d107_68.png)

![](img/c5a28d261316d48ae10768633c59d107_69.png)

以下是寻找最佳K值的步骤：

![](img/c5a28d261316d48ae10768633c59d107_71.png)

首先，创建两个空列表来存储不同K值对应的F1分数和错误率。

```python
f1_scores = []
error_rates = []
```

![](img/c5a28d261316d48ae10768633c59d107_73.png)

![](img/c5a28d261316d48ae10768633c59d107_74.png)

然后，使用一个循环来训练和评估每个K值的模型。

![](img/c5a28d261316d48ae10768633c59d107_76.png)

```python
for k in range(1, 41):
    knn_temp = KNeighborsClassifier(n_neighbors=k)
    knn_temp.fit(X_train, y_train)
    pred_temp = knn_temp.predict(X_test)
    # 计算并存储F1分数
    f1 = f1_score(y_test, pred_temp)
    f1_scores.append(f1)
    # 计算并存储错误率（1 - 准确率）
    error = 1 - accuracy_score(y_test, pred_temp)
    error_rates.append(error)
```

![](img/c5a28d261316d48ae10768633c59d107_77.png)

![](img/c5a28d261316d48ae10768633c59d107_78.png)

![](img/c5a28d261316d48ae10768633c59d107_80.png)

将结果存储到DataFrame中，便于分析和绘图。

![](img/c5a28d261316d48ae10768633c59d107_82.png)

```python
import pandas as pd
f1_results = pd.DataFrame({'K': range(1, 41), 'F1_Score': f1_scores})
error_results = pd.DataFrame({'K': range(1, 41), 'Error_Rate': error_rates})
```

![](img/c5a28d261316d48ae10768633c59d107_84.png)

现在，我们可以绘制F1分数随K值变化的曲线图。

![](img/c5a28d261316d48ae10768633c59d107_86.png)

```python
plt.figure(figsize=(12, 8))
plt.plot(f1_results['K'], f1_results['F1_Score'], color='blue', linewidth=3)
plt.xlabel('K Value')
plt.ylabel('F1 Score')
plt.title('F1 Score vs. K Value')
plt.xticks(range(1, 41, 2))
plt.grid(True)
plt.savefig('f1_score_vs_k.png')
plt.show()
```

![](img/c5a28d261316d48ae10768633c59d107_88.png)

![](img/c5a28d261316d48ae10768633c59d107_89.png)

同样地，绘制错误率随K值变化的曲线图。

![](img/c5a28d261316d48ae10768633c59d107_91.png)

![](img/c5a28d261316d48ae10768633c59d107_92.png)

```python
plt.figure(figsize=(12, 8))
plt.plot(error_results['K'], error_results['Error_Rate'], color='red', linewidth=3)
plt.xlabel('K Value')
plt.ylabel('Error Rate')
plt.title('Error Rate vs. K Value')
plt.xticks(range(1, 41, 2))
plt.grid(True)
plt.savefig('error_rate_vs_k.png')
plt.show()
```

![](img/c5a28d261316d48ae10768633c59d107_94.png)

通过分析图表，我们可以发现：
- F1分数随着K值的增加先上升后趋于平稳，在K值约为19到20时达到最佳。
- 错误率在K值约为21时达到最低点，之后开始上升。
- 因此，对于这个数据集，K值在19到21之间可能是最优选择。

![](img/c5a28d261316d48ae10768633c59d107_95.png)

---

![](img/c5a28d261316d48ae10768633c59d107_97.png)

![](img/c5a28d261316d48ae10768633c59d107_99.png)

## 总结 📝

本节课中我们一起学习了K最近邻算法的完整实践流程：
1.  **数据准备**：将预处理后的数据划分为特征和目标变量，并进一步拆分为训练集和测试集。
2.  **模型训练与评估**：初始化KNN分类器（先使用K=3），进行训练，并通过分类报告、准确率、F1分数和混淆矩阵全面评估模型性能。
3.  **模型优化**：通过调整K值（尝试K=5）来观察性能变化，发现适当增加K值可以提升对少数类（流失客户）的预测能力。
4.  **超参数调优**：系统性地测试K从1到40的取值，通过绘制F1分数和错误率曲线，找到了适用于该数据集的最佳K值范围（约19-21）。

![](img/c5a28d261316d48ae10768633c59d107_101.png)

![](img/c5a28d261316d48ae10768633c59d107_102.png)

通过本实验，我们掌握了使用KNN解决分类问题的核心步骤，并理解了如何通过评估指标和超参数调优来提升模型效果。