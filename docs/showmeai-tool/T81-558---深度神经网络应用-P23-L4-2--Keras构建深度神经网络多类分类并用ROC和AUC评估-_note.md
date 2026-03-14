# T81-558 ｜ 深度神经网络应用 - P23：L4.2 - 使用Keras构建深度神经网络进行多类分类并用ROC和AUC评估 📊🧠

![](img/0c3de78b0d0924709e45c8de93aa7c5d_1.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_3.png)

在本节课中，我们将学习如何使用Keras构建深度神经网络来处理分类问题，特别是二分类和多类分类。我们将重点介绍如何评估分类模型的性能，包括使用ROC曲线、AUC值、混淆矩阵和对数损失等关键指标。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_5.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_7.png)

---

## 概述

![](img/0c3de78b0d0924709e45c8de93aa7c5d_9.png)

本节课分为两个主要部分。首先，我们将探讨二分类问题，使用威斯康星乳腺癌数据集作为例子，并学习如何通过ROC曲线和AUC来评估模型性能。其次，我们将转向多类分类问题，介绍如何使用分类交叉熵损失函数，并通过混淆矩阵和对数损失来深入分析模型的表现。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_11.png)

---

## 二分类问题 🎯

![](img/0c3de78b0d0924709e45c8de93aa7c5d_13.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_14.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_15.png)

上一节我们介绍了课程的整体结构，本节中我们来看看二分类问题。二分类是指目标变量只有两种可能结果的问题，例如判断肿瘤是恶性还是良性。

### 评估指标：超越准确率

![](img/0c3de78b0d0924709e45c8de93aa7c5d_17.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_18.png)

在二分类中，仅使用准确率评估模型可能不够全面。我们需要考虑不同类型的错误：
*   **假阳性**：模型错误地将阴性样本预测为阳性。
*   **假阴性**：模型错误地将阳性样本预测为阴性。

这两种错误的代价可能完全不同。例如，在医疗诊断中，假阴性（漏诊）的后果可能比假阳性（误诊）更严重。

以下是评估二分类模型时常用的核心概念：
*   **真正例**：实际为阳性，预测也为阳性。
*   **真反例**：实际为阴性，预测也为阴性。
*   **灵敏度**：模型正确识别阳性样本的能力。公式为：`TP / (TP + FN)`。
*   **特异度**：模型正确识别阴性样本的能力。公式为：`TN / (TN + FP)`。

### 阈值与ROC曲线

![](img/0c3de78b0d0924709e45c8de93aa7c5d_20.png)

神经网络的输出通常是一个连续值（例如，通过Sigmoid激活函数输出0到1之间的概率）。我们需要设定一个**阈值**来决定将哪个值归类为阳性。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_21.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_22.png)

**ROC曲线**描绘了在不同阈值下，模型的**真正例率**与**假正例率**之间的关系。
*   **真正例率**：`TPR = TP / (TP + FN)`，即灵敏度。
*   **假正例率**：`FPR = FP / (FP + TN)`，即1 - 特异度。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_24.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_25.png)

ROC曲线下的面积称为**AUC**。AUC值越接近1.0，表示模型性能越好；AUC为0.5表示模型与随机猜测无异。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_26.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_27.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_28.png)

### 代码实现：二分类神经网络

![](img/0c3de78b0d0924709e45c8de93aa7c5d_30.png)

以下是使用Keras构建二分类神经网络的示例代码框架：

![](img/0c3de78b0d0924709e45c8de93aa7c5d_31.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_32.png)

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

model = Sequential()
model.add(Dense(20, input_dim=X_train.shape[1], activation='relu'))
model.add(Dense(10, activation='relu'))
model.add(Dense(1, activation='sigmoid')) # 二分类，使用单个Sigmoid神经元

![](img/0c3de78b0d0924709e45c8de93aa7c5d_34.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_35.png)

model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
# 注意：损失函数使用‘binary_crossentropy’
```

![](img/0c3de78b0d0924709e45c8de93aa7c5d_36.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_37.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_38.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_39.png)

训练模型后，我们可以计算预测概率并绘制ROC曲线。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_41.png)

---

![](img/0c3de78b0d0924709e45c8de93aa7c5d_43.png)

## 多类分类问题 🗂️

上一节我们介绍了二分类及其评估方法，本节中我们来看看多类分类问题。多类分类是指目标变量有两个以上类别的问题。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_45.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_46.png)

### 损失函数：分类交叉熵

![](img/0c3de78b0d0924709e45c8de93aa7c5d_47.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_48.png)

对于多类分类，神经网络的输出层神经元数量应等于类别数，并使用**Softmax**激活函数，它将输出转换为每个类别的概率分布。

对应的损失函数是**分类交叉熵**。切勿在多类分类中使用二元交叉熵。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_50.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_51.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_52.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_53.png)

以下是构建多类分类神经网络的示例代码：

![](img/0c3de78b0d0924709e45c8de93aa7c5d_54.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_55.png)

```python
from tensorflow.keras.utils import to_categorical

![](img/0c3de78b0d0924709e45c8de93aa7c5d_57.png)

# 假设有3个类别，将标签转换为one-hot编码
y_train_onehot = to_categorical(y_train, num_classes=3)

model = Sequential()
model.add(Dense(20, input_dim=X_train.shape[1], activation='relu'))
model.add(Dense(10, activation='relu'))
model.add(Dense(3, activation='softmax')) # 输出层神经元数等于类别数

![](img/0c3de78b0d0924709e45c8de93aa7c5d_59.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_60.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_61.png)

model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])
# 注意：损失函数使用‘categorical_crossentropy’
```

![](img/0c3de78b0d0924709e45c8de93aa7c5d_62.png)

### 评估指标：准确率与对数损失

*   **准确率**：最直观的指标，计算正确预测的样本比例。
*   **对数损失**：比准确率更严格的指标。它不仅考虑预测是否正确，还考虑模型预测的“置信度”。模型对错误答案越自信，惩罚越大。

对数损失的公式为：
`Log Loss = -1/N * Σ Σ y_i_j * log(p_i_j)`
其中，`N`是样本数，`i`遍历样本，`j`遍历类别，`y_i_j`是样本`i`属于类别`j`的真实标签（one-hot编码），`p_i_j`是模型预测样本`i`属于类别`j`的概率。

对数损失值越低越好，完美模型的损失为0。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_64.png)

### 混淆矩阵

![](img/0c3de78b0d0924709e45c8de93aa7c5d_66.png)

混淆矩阵是分析多类分类模型错误的强大工具。它是一个`N x N`的矩阵（N为类别数），行代表真实标签，列代表预测标签。对角线上的值表示被正确分类的样本数，而非对角线上的值则揭示了模型混淆了哪些类别。

通过分析混淆矩阵，我们可以识别出模型在哪些特定类别对上容易出错，从而进行针对性的优化。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_68.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_69.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_70.png)

---

![](img/0c3de78b0d0924709e45c8de93aa7c5d_71.png)

## 总结

![](img/0c3de78b0d0924709e45c8de93aa7c5d_73.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_74.png)

本节课中我们一起学习了深度神经网络在分类任务中的应用。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_76.png)

![](img/0c3de78b0d0924709e45c8de93aa7c5d_77.png)

1.  对于**二分类**，我们使用单个Sigmoid输出神经元和`binary_crossentropy`损失函数。评估时，除了准确率，应重点关注**ROC曲线**和**AUC**值，它们能全面反映模型在不同阈值下的性能。
2.  对于**多类分类**，我们使用与类别数相同的Softmax输出神经元和`categorical_crossentropy`损失函数。评估时，可以使用**准确率**，但**对数损失**能提供更细致的评估。**混淆矩阵**则是可视化分析分类错误、指导模型改进的必备工具。

![](img/0c3de78b0d0924709e45c8de93aa7c5d_79.png)

理解这些核心概念和评估方法，对于构建和优化实用的分类神经网络至关重要。在下一部分，我们将探讨如何用神经网络处理回归问题。