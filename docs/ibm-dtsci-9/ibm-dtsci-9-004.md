# 004：Python机器学习基础 📚

![](img/edbd0652a54fdac6ef5fd08bb9100e54_0.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_2.png)

在本节课中，我们将学习如何使用Python进行机器学习。Python是一种流行且强大的通用编程语言，近年来已成为数据科学家的首选语言。你可以使用Python编写机器学习算法，并且效果很好。然而，Python中已经实现了很多模块和库，可以极大地简化你的工作。本课程将介绍这些Python包，并在实验中使用它们，以提供更好的实践体验。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_4.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_5.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_7.png)

## Python核心科学计算库 🧮

![](img/edbd0652a54fdac6ef5fd08bb9100e54_9.png)

上一节我们介绍了Python在机器学习中的重要性，本节中我们来看看支撑机器学习的基础科学计算库。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_11.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_12.png)

**NumPy** 是一个数学库，用于在Python中处理n维数组。它使你能够高效地进行计算。由于其出色的能力，它在处理数组、字典、函数、数据类型和图像方面优于常规Python。你需要了解NumPy。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_14.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_16.png)

**SciPy** 是一个数值算法和特定领域工具箱的集合，包括信号处理、优化、统计学等。SciPy是进行科学和高性能计算的一个优秀库。

**Matplotlib** 是一个非常流行的绘图包，提供2D和3D绘图功能。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_18.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_19.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_20.png)

掌握这三个构建在Python之上的包的基础知识，对于希望解决实际问题的数据科学家来说是一笔宝贵的财富。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_22.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_23.png)

如果你不熟悉这些包，建议你先学习《使用Python进行数据分析》课程。该课程涵盖了这些包中的大部分实用主题。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_24.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_26.png)

## 数据处理与机器学习库 🤖

![](img/edbd0652a54fdac6ef5fd08bb9100e54_28.png)

了解了基础科学计算库后，我们接下来看看专门用于数据处理和机器学习的强大库。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_29.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_31.png)

**Pandas** 库是一个非常高层次的Python库，它提供了高性能、易于使用的数据结构。它拥有许多用于数据导入、操作和分析的函数。特别是，它为操作数值表和时间序列提供了数据结构和操作。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_33.png)

**Scikit-learn** 是一个机器学习算法和工具的集合，这也是我们本课程的重点，你将学习如何使用它。由于我们将在实验中大量使用Scikit-learn，让我详细解释一下它，并展示它为何在数据科学家中如此受欢迎。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_35.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_36.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_38.png)

Scikit-learn是Python编程语言的免费机器学习库。它包含了大多数分类、回归和聚类算法，并且设计用于与Python数值和科学库NumPy和SciPy协同工作。此外，它包含了非常完善的文档。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_40.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_41.png)

最重要的是，使用Scikit-learn只需几行Python代码就能轻松实现机器学习模型。机器学习流程中需要完成的大多数任务都已经在Scikit-learn中实现，包括数据预处理、特征选择、特征提取、训练测试集划分、定义算法、拟合模型、调参、预测、评估以及导出模型。

让我展示一个使用这个库的例子。你现在不需要理解代码，只需看看如何用几行代码轻松构建一个模型。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_43.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_44.png)

以下是使用Scikit-learn构建支持向量机分类器的核心代码示例：

![](img/edbd0652a54fdac6ef5fd08bb9100e54_46.png)

```python
# 导入必要的模块
from sklearn import svm
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import confusion_matrix

![](img/edbd0652a54fdac6ef5fd08bb9100e54_48.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_49.png)

# 1. 数据预处理：标准化
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# 2. 划分训练集和测试集 (通常在数据加载后立即进行)
# X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# 3. 设置算法（估计器）
clf = svm.SVC(gamma=0.001, C=100.)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_51.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_52.png)

# 4. 使用训练集训练模型
clf.fit(X_train_scaled, y_train)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_53.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_55.png)

# 5. 使用测试集进行预测
predictions = clf.predict(X_test_scaled)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_57.png)

# 6. 评估模型准确性（例如使用混淆矩阵）
cm = confusion_matrix(y_test, predictions)
```

![](img/edbd0652a54fdac6ef5fd08bb9100e54_59.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_60.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_61.png)

基本上，机器学习算法受益于数据集的标准化。如果你的数据集中存在一些异常值或不同尺度的字段，你必须修复它们。Scikit-learn的预处理包提供了几个常见的实用函数和转换器类，用于将原始特征向量更改为适合建模的向量形式。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_63.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_64.png)

你必须将数据集拆分为训练集和测试集，以训练你的模型，然后单独测试模型的准确性。Scikit-learn可以用一行代码为你将数组或矩阵随机拆分为训练子集和测试子集。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_66.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_68.png)

然后你可以设置你的算法。例如，你可以使用支持向量分类算法构建一个分类器。我们称我们的估计器实例为`clf`并初始化其参数。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_70.png)

现在，你可以通过将训练集传递给`fit`方法来用训练集训练你的模型，`clf`模型学习对未知案例进行分类。

![](img/edbd0652a54fdac6ef5fd08bb9100e54_72.png)

![](img/edbd0652a54fdac6ef5fd08bb9100e54_74.png)

然后我们可以使用测试集来运行预测。结果告诉我们每个未知值的类别是什么。此外，你可以使用不同的指标来评估模型的准确性，例如使用混淆矩阵来显示结果。最后，保存你的模型。

你可能会发现所有这些或部分机器学习术语令人困惑，但别担心，我们将在接下来的视频中讨论所有这些主题。要记住的最重要的一点是，使用Scikit-learn，整个机器学习任务的过程只需几行代码即可简单完成。

请注意，虽然使用NumPy或SciPy包也可以完成所有这些工作，但不会那么容易，当然，如果你使用纯Python编程来实现所有这些任务，则需要更多的编码。

## 总结 📝

本节课中我们一起学习了Python在机器学习中的应用基础。我们介绍了核心的科学计算库NumPy、SciPy和Matplotlib，它们是数据处理和可视化的基石。接着，我们深入探讨了专门用于机器学习的库Scikit-learn，了解了它如何通过简洁的代码实现从数据预处理到模型评估的完整机器学习流程。掌握这些工具是成为一名高效的数据科学家和机器学习实践者的关键第一步。