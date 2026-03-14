# 机器学习课程 P7：可视化决策边界 📊

![](img/d17dc8f7720589c00136fbad4aed332e_0.png)

在本节课中，我们将学习如何可视化分类器的决策边界。决策边界是分类模型用于区分不同类别的“分界线”。通过绘制它，我们可以直观地理解模型是如何根据输入特征（例如花瓣的长度和宽度）做出预测的。

![](img/d17dc8f7720589c00136fbad4aed332e_2.png)

---

## 理解决策边界与绘图原理

在回归任务中，我们通常通过绘制拟合线来可视化模型。对于有两个特征的分类任务，等效的可视化方法是在二维平面上绘制这两个特征（例如，X轴代表花瓣宽度，Y轴代表花瓣长度），并将平面划分为模型预测为不同类别的区域。

![](img/d17dc8f7720589c00136fbad4aed332e_4.png)

Matplotlib库中的 `contourf` 函数非常适合绘制这种区域图。我们的核心思路是：创建一个覆盖特征值范围的密集网格，用训练好的模型为网格中的每一个点做出预测，然后根据预测结果着色，从而形成决策区域。

![](img/d17dc8f7720589c00136fbad4aed332e_6.png)

---

![](img/d17dc8f7720589c00136fbad4aed332e_8.png)

## 创建预测网格

![](img/d17dc8f7720589c00136fbad4aed332e_10.png)

为了评估分类器在整个特征空间的表现，我们需要输入一系列特征值的组合。最简便的方法是使用NumPy的 `meshgrid` 函数。

![](img/d17dc8f7720589c00136fbad4aed332e_12.png)

![](img/d17dc8f7720589c00136fbad4aed332e_13.png)

以下是创建网格的步骤：

![](img/d17dc8f7720589c00136fbad4aed332e_15.png)

1.  为每个特征定义一个取值范围（例如，从0到10，步长为0.1）。
2.  使用 `np.meshgrid` 生成这两个范围所有组合的坐标矩阵。

![](img/d17dc8f7720589c00136fbad4aed332e_17.png)

```python
import numpy as np

![](img/d17dc8f7720589c00136fbad4aed332e_18.png)

![](img/d17dc8f7720589c00136fbad4aed332e_19.png)

# 定义特征值的范围
x_range = np.arange(0, 10, 0.1)  # 花瓣宽度范围
y_range = np.arange(0, 10, 0.1)  # 花瓣长度范围

![](img/d17dc8f7720589c00136fbad4aed332e_21.png)

![](img/d17dc8f7720589c00136fbad4aed332e_23.png)

# 创建网格坐标矩阵
xx, yy = np.meshgrid(x_range, y_range)
```

执行后，`xx` 和 `yy` 是两个形状相同的矩阵，它们共同定义了平面上每一个点的 (x, y) 坐标。

---

## 准备数据并进行预测

`meshgrid` 生成的坐标矩阵是二维的，但Scikit-learn模型的 `.predict` 方法通常需要二维数组格式的输入，其中每一行是一个样本。

![](img/d17dc8f7720589c00136fbad4aed332e_25.png)

因此，我们需要将坐标矩阵“展平”并组合成模型所需的格式。

![](img/d17dc8f7720589c00136fbad4aed332e_27.png)

以下是数据准备与预测的步骤：

1.  将 `xx` 和 `yy` 展平为一维数组。
2.  将它们组合成一个两列的DataFrame（或数组）。
3.  使用训练好的分类器对这个DataFrame进行预测。

![](img/d17dc8f7720589c00136fbad4aed332e_29.png)

```python
import pandas as pd

![](img/d17dc8f7720589c00136fbad4aed332e_31.png)

# 1. 将网格坐标矩阵展平
xx_flat = xx.ravel()
yy_flat = yy.ravel()

# 2. 创建包含所有坐标点的DataFrame
grid_df = pd.DataFrame({
    ‘petal_width‘: xx_flat,  # 对应X轴特征
    ‘petal_length‘: yy_flat   # 对应Y轴特征
})

# 3. 使用模型进行预测
# 假设 ‘clf‘ 是您已经训练好的分类器
predictions = clf.predict(grid_df)
```

现在，`predictions` 数组包含了网格中每一个点对应的类别预测（例如0或1）。

---

![](img/d17dc8f7720589c00136fbad4aed332e_33.png)

![](img/d17dc8f7720589c00136fbad4aed332e_34.png)

## 绘制决策边界与数据点

![](img/d17dc8f7720589c00136fbad4aed332e_35.png)

![](img/d17dc8f7720589c00136fbad4aed332e_36.png)

获得预测结果后，我们需要将其重新变回与 `xx`、`yy` 相同的网格形状，以便用 `contourf` 绘图。

![](img/d17dc8f7720589c00136fbad4aed332e_38.png)

![](img/d17dc8f7720589c00136fbad4aed332e_40.png)

同时，为了评估模型，我们可以在决策边界图上叠加原始数据点的散点图。

![](img/d17dc8f7720589c00136fbad4aed332e_42.png)

![](img/d17dc8f7720589c00136fbad4aed332e_44.png)

以下是完整的绘图步骤：

![](img/d17dc8f7720589c00136fbad4aed332e_46.png)

1.  将一维的预测结果 `predictions` 重塑为与 `xx` 相同的二维网格形状。
2.  使用 `plt.contourf` 绘制决策区域。
3.  使用 `plt.scatter` 分别绘制不同类别的原始数据点。

![](img/d17dc8f7720589c00136fbad4aed332e_48.png)

![](img/d17dc8f7720589c00136fbad4aed332e_50.png)

```python
import matplotlib.pyplot as plt

![](img/d17dc8f7720589c00136fbad4aed332e_52.png)

# 1. 将预测结果重塑为网格形状
prediction_grid = predictions.reshape(xx.shape)

![](img/d17dc8f7720589c00136fbad4aed332e_54.png)

# 2. 绘制决策边界区域
plt.contourf(xx, yy, prediction_grid, alpha=0.3, cmap=‘RdYlBu‘)

# 3. 绘制原始数据点（假设 df 是原始DataFrame，且‘species‘是类别列）
setosa = df[df[‘species‘] == ‘setosa‘]
others = df[df[‘species‘] != ‘setosa‘]

![](img/d17dc8f7720589c00136fbad4aed332e_56.png)

![](img/d17dc8f7720589c00136fbad4aed332e_58.png)

plt.scatter(setosa[‘petal_width‘], setosa[‘petal_length‘], c=‘red‘, label=‘Setosa‘, edgecolor=‘k‘)
plt.scatter(others[‘petal_width‘], others[‘petal_length‘], c=‘blue‘, label=‘Others‘, edgecolor=‘k‘)

![](img/d17dc8f7720589c00136fbad4aed332e_60.png)

plt.xlabel(‘Petal Width‘)
plt.ylabel(‘Petal Length‘)
plt.legend()
plt.title(‘Classifier Decision Boundary‘)
plt.show()
```

![](img/d17dc8f7720589c00136fbad4aed332e_62.png)

最终，您将得到一张图，其中背景色块展示了模型的决策区域，散点则显示了真实的数据分布，可以直观地看到分类正确和错误的点。

![](img/d17dc8f7720589c00136fbad4aed332e_64.png)

![](img/d17dc8f7720589c00136fbad4aed332e_65.png)

---

![](img/d17dc8f7720589c00136fbad4aed332e_67.png)

## 进阶：使用多项式特征的决策边界

![](img/d17dc8f7720589c00136fbad4aed332e_69.png)

上一节我们介绍了如何为简单模型绘制线性决策边界。本节中我们来看看，当使用更复杂的模型（如包含多项式特征的逻辑回归）时，决策边界如何变化。

![](img/d17dc8f7720589c00136fbad4aed332e_71.png)

通过Scikit-learn的 `Pipeline` 和 `PolynomialFeatures`，我们可以轻松构建一个非线性分类器。

![](img/d17dc8f7720589c00136fbad4aed332e_73.png)

![](img/d17dc8f7720589c00136fbad4aed332e_75.png)

以下是构建并使用多项式特征分类器的步骤：

1.  创建一个管道，依次包含多项式特征转换和逻辑回归模型。
2.  用相同的数据训练这个管道。
3.  重复之前的网格预测和绘图步骤，观察决策边界形状的变化。

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LogisticRegression

![](img/d17dc8f7720589c00136fbad4aed332e_77.png)

![](img/d17dc8f7720589c00136fbad4aed332e_78.png)

# 1. 创建管道
# 注意：每个步骤需要以 (名称, 估计器) 的元组形式给出
poly_clf = Pipeline([
    (‘poly‘, PolynomialFeatures(degree=2)),  # 添加二次项特征
    (‘lr‘, LogisticRegression())             # 逻辑回归分类器
])

![](img/d17dc8f7720589c00136fbad4aed332e_80.png)

# 2. 训练模型（假设 X_train, y_train 已定义）
poly_clf.fit(X_train, y_train)

![](img/d17dc8f7720589c00136fbad4aed332e_82.png)

# 3. 使用新模型 poly_clf 重复“准备数据并进行预测”及之后的绘图步骤
# ... (代码与之前章节类似，只需将 clf 替换为 poly_clf)
```

![](img/d17dc8f7720589c00136fbad4aed332e_84.png)

使用多项式特征后，您会发现决策边界从一条直线变成了曲线，这使模型能够拟合更复杂的数据模式。通过调整 `PolynomialFeatures` 的 `degree` 参数，可以控制模型的复杂度。

![](img/d17dc8f7720589c00136fbad4aed332e_86.png)

---

![](img/d17dc8f7720589c00136fbad4aed332e_88.png)

![](img/d17dc8f7720589c00136fbad4aed332e_89.png)

## 课程总结

![](img/d17dc8f7720589c00136fbad4aed332e_91.png)

本节课中我们一起学习了机器学习中决策边界的可视化方法。

![](img/d17dc8f7720589c00136fbad4aed332e_92.png)

![](img/d17dc8f7720589c00136fbad4aed332e_94.png)

我们首先理解了决策边界的概念及其在二维特征空间中的表现形式。接着，我们掌握了使用 `np.meshgrid` 创建评估网格、准备预测数据以及利用 `plt.contourf` 绘制着色区域的核心流程。最后，我们通过引入 `Pipeline` 和 `PolynomialFeatures` 探索了如何绘制非线性决策边界，并观察到模型复杂度对边界形状的影响。

![](img/d17dc8f7720589c00136fbad4aed332e_95.png)

![](img/d17dc8f7720589c00136fbad4aed332e_96.png)

这项技能对于直观理解模型行为、诊断模型问题（如欠拟合或过拟合）至关重要。