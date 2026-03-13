# 119：绘制线性SVM决策边界 📊

![](img/473ee5946d3b0a649553904dac590c9f_0.png)

在本节课中，我们将学习如何为线性支持向量机分类器创建并可视化决策边界。我们将使用一个葡萄酒数据集，通过代码逐步展示如何生成一个包含数据点和分类决策区域的图形。

---

## 概述

![](img/473ee5946d3b0a649553904dac590c9f_2.png)

上一节我们介绍了支持向量机的基本概念。本节中，我们将实际操作，使用线性SVC模型拟合数据，并绘制出能够清晰区分红葡萄酒与白葡萄酒的决策边界。整个过程涉及数据采样、模型训练、网格预测和等高线图绘制。

![](img/473ee5946d3b0a649553904dac590c9f_4.png)

![](img/473ee5946d3b0a649553904dac590c9f_6.png)

---

![](img/473ee5946d3b0a649553904dac590c9f_7.png)

![](img/473ee5946d3b0a649553904dac590c9f_9.png)

## 步骤详解

![](img/473ee5946d3b0a649553904dac590c9f_11.png)

### 1. 模型训练与数据准备

首先，我们初始化一个线性支持向量分类器，并使用全部训练数据（特征`X`和标签`Y`）进行拟合，以获得最佳模型。

```python
from sklearn.svm import LinearSVC
lsvc = LinearSVC()
lsvc.fit(X, Y)
```

![](img/473ee5946d3b0a649553904dac590c9f_13.png)

![](img/473ee5946d3b0a649553904dac590c9f_15.png)

接着，我们从特征`X`中随机抽取300个样本，并获取其对应的标签。这样做是为了避免在二维图中绘制过多数据点导致图形过于拥挤。

![](img/473ee5946d3b0a649553904dac590c9f_17.png)

```python
import numpy as np
sample_indices = np.random.choice(X.shape[0], 300, replace=False)
X_color = X[sample_indices]
Y_color = Y[sample_indices]
```

![](img/473ee5946d3b0a649553904dac590c9f_19.png)

然后，我们将数值标签（1和0）映射为颜色字符串（‘red’和‘yellow’），以便在散点图中进行颜色编码。

![](img/473ee5946d3b0a649553904dac590c9f_21.png)

```python
Y_color = np.array(['red' if val == 1 else 'yellow' for val in Y_color])
```

![](img/473ee5946d3b0a649553904dac590c9f_23.png)

![](img/473ee5946d3b0a649553904dac590c9f_24.png)

### 2. 绘制基础散点图

![](img/473ee5946d3b0a649553904dac590c9f_26.png)

![](img/473ee5946d3b0a649553904dac590c9f_28.png)

在绘制决策边界之前，我们先创建一个只包含数据点的散点图。以下是绘制散点图的代码。

![](img/473ee5946d3b0a649553904dac590c9f_30.png)

![](img/473ee5946d3b0a649553904dac590c9f_32.png)

我们使用`matplotlib`创建图形，并根据`Y_color`中的颜色信息绘制样本点。

![](img/473ee5946d3b0a649553904dac590c9f_34.png)

![](img/473ee5946d3b0a649553904dac590c9f_35.png)

![](img/473ee5946d3b0a649553904dac590c9f_36.png)

```python
import matplotlib.pyplot as plt
fig, ax = plt.subplots()
ax.scatter(X_color[:, 0], X_color[:, 1], c=Y_color)
plt.show()
```

![](img/473ee5946d3b0a649553904dac590c9f_38.png)

![](img/473ee5946d3b0a649553904dac590c9f_39.png)

此时，图形展示了红点和黄点的分布，但还没有划分它们的决策线。

![](img/473ee5946d3b0a649553904dac590c9f_41.png)

![](img/473ee5946d3b0a649553904dac590c9f_43.png)

### 3. 创建预测网格

![](img/473ee5946d3b0a649553904dac590c9f_45.png)

![](img/473ee5946d3b0a649553904dac590c9f_47.png)

为了绘制决策边界，我们需要在整个特征空间（从0到1的范围）内创建一个密集的网格点，并对每个点进行预测。

![](img/473ee5946d3b0a649553904dac590c9f_49.png)

首先，使用`numpy`的`arange`函数生成从0到1，步长为0.005的序列，作为网格的坐标基础。

![](img/473ee5946d3b0a649553904dac590c9f_51.png)

![](img/473ee5946d3b0a649553904dac590c9f_52.png)

```python
xx = np.arange(0, 1.005, 0.005)
yy = np.arange(0, 1.005, 0.005)
```

![](img/473ee5946d3b0a649553904dac590c9f_54.png)

![](img/473ee5946d3b0a649553904dac590c9f_55.png)

然后，使用`meshgrid`函数生成网格矩阵`Xx`和`Yy`。`Xx`矩阵的每一行相同，`Yy`矩阵的每一列相同，这共同构成了一个覆盖整个区域的坐标网格。

![](img/473ee5946d3b0a649553904dac590c9f_57.png)

![](img/473ee5946d3b0a649553904dac590c9f_59.png)

```python
Xx, Yy = np.meshgrid(xx, yy)
```

为了便于模型预测，我们将网格矩阵展平（`ravel`）并组合成一个二维数组，其中每一行代表一个网格点的(x, y)坐标。

![](img/473ee5946d3b0a649553904dac590c9f_61.png)

```python
X_grid = np.column_stack([Xx.ravel(), Yy.ravel()])
```

![](img/473ee5946d3b0a649553904dac590c9f_63.png)

![](img/473ee5946d3b0a649553904dac590c9f_64.png)

### 4. 进行网格预测并绘制决策区域

![](img/473ee5946d3b0a649553904dac590c9f_66.png)

![](img/473ee5946d3b0a649553904dac590c9f_67.png)

![](img/473ee5946d3b0a649553904dac590c9f_69.png)

现在，我们使用训练好的线性SVC模型对这个密集的网格点进行预测。

![](img/473ee5946d3b0a649553904dac590c9f_71.png)

```python
Z = lsvc.predict(X_grid)
```

![](img/473ee5946d3b0a649553904dac590c9f_73.png)

预测结果`Z`是一个一维数组，包含了每个网格点的预测标签（1或0）。为了绘制等高线图，我们需要将其重新塑形（`reshape`）为与`Xx`相同的二维形状。

![](img/473ee5946d3b0a649553904dac590c9f_75.png)

```python
Z = Z.reshape(Xx.shape)
```

![](img/473ee5946d3b0a649553904dac590c9f_77.png)

![](img/473ee5946d3b0a649553904dac590c9f_79.png)

![](img/473ee5946d3b0a649553904dac590c9f_81.png)

最后，我们在之前的散点图基础上，使用`contourf`函数绘制决策区域的填充等高线图。模型预测为1的区域将显示为红色，预测为0的区域显示为黄色。

![](img/473ee5946d3b0a649553904dac590c9f_83.png)

```python
ax.contourf(Xx, Yy, Z, alpha=0.3, cmap=plt.cm.RdYlBu)
plt.show()
```

![](img/473ee5946d3b0a649553904dac590c9f_85.png)

生成的图形中，红色直线即为决策边界，其下方区域被分类为红葡萄酒，上方区域被分类为白葡萄酒。

---

![](img/473ee5946d3b0a649553904dac590c9f_87.png)

![](img/473ee5946d3b0a649553904dac590c9f_89.png)

## 总结

本节课中我们一起学习了如何可视化线性SVM的决策边界。关键步骤包括：训练线性SVC模型、对数据进行采样和颜色编码、创建覆盖特征空间的预测网格、使用模型进行网格预测，以及最终结合散点图和等高线图展示决策区域。这个方法帮助我们直观地理解分类器是如何在二维空间中划分不同类别的。

![](img/473ee5946d3b0a649553904dac590c9f_91.png)

![](img/473ee5946d3b0a649553904dac590c9f_92.png)

![](img/473ee5946d3b0a649553904dac590c9f_93.png)

![](img/473ee5946d3b0a649553904dac590c9f_94.png)

在接下来的课程中，我们将运用这个绘图技巧，来观察不同核函数（如高斯核）以及超参数（如正则化强度）对SVM模型决策边界形状的影响。