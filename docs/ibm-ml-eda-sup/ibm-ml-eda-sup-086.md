# 086：正则化详解（选修）第1部分 📚

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_0.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_2.png)

在本节课中，我们将学习数据标准化（Standardization）的原理与实现，并回顾其在回归模型（特别是线性回归）中的应用。我们将通过手动计算和调用库函数两种方式来理解标准化过程，并探讨如何利用标准化后的系数来评估特征的重要性。

---

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_4.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_5.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_6.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_7.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_8.png)

## 导入数据与库 📥

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_10.png)

首先，我们需要导入必要的库并加载数据集。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_11.png)

```python
import numpy as np
import pandas as pd
from helper import boston_dataframe
```

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_13.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_14.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_15.png)

我们将使用波士顿房价数据集。通过`boston_dataframe`函数，我们可以获取数据及其描述。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_17.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_18.png)

```python
boston = boston_dataframe()
boston_data = boston[0]  # 特征数据
boston_description = boston[1]  # 数据描述
```

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_20.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_21.png)

`boston_data`现在是一个Pandas DataFrame，包含了所有特征变量。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_23.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_24.png)

---

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_26.png)

## 理解数据标准化 🎯

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_28.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_30.png)

数据标准化是指对每个变量进行转换，使其更接近标准正态分布。标准正态分布的均值为0，标准差为1。转换公式为：

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_32.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_33.png)

**`z = (x - μ) / σ`**

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_35.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_36.png)

其中，`x`是原始值，`μ`是均值，`σ`是标准差。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_38.png)

---

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_40.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_41.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_43.png)

## 准备特征与目标变量 🔧

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_45.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_46.png)

我们首先需要从数据集中分离出特征（X）和目标变量（y）。

```python
y_col = 'MEDV'  # 目标变量：房屋中位数价值
X = boston_data.drop(y_col, axis=1)  # 特征变量
y = boston_data[y_col]  # 目标变量
```

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_48.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_49.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_50.png)

---

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_52.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_53.png)

## 使用Scikit-learn进行标准化 ⚙️

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_55.png)

Scikit-learn提供了`StandardScaler`类来方便地进行标准化。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_57.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_58.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_60.png)

```python
from sklearn.preprocessing import StandardScaler

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_61.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_63.png)

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

`fit_transform`方法会计算训练数据的均值和标准差，然后进行转换。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_65.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_66.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_67.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_68.png)

---

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_70.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_71.png)

## 手动实现标准化过程 🛠️

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_73.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_75.png)

为了深入理解标准化的原理，我们可以使用NumPy手动实现这一过程。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_76.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_78.png)

以下是手动标准化的步骤：

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_80.png)

1.  计算每个特征列的均值。
2.  计算每个特征列的标准差。
3.  对每个数据点，执行 `(值 - 均值) / 标准差`。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_82.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_83.png)

```python
# 将DataFrame转换为NumPy数组
X_array = X.values

# 手动计算均值和标准差（沿列方向，即axis=0）
mean = X_array.mean(axis=0)
std = X_array.std(axis=0)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_85.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_86.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_87.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_88.png)

# 执行标准化
X_manual_scaled = (X_array - mean) / std
```

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_89.png)

我们可以使用`np.allclose`函数来验证手动计算的结果与`StandardScaler`的结果是否一致（允许微小的浮点数误差）。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_91.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_92.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_93.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_94.png)

```python
print(np.allclose(X_scaled, X_manual_scaled))  # 应输出 True
```

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_96.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_98.png)

---

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_100.png)

## 标准化对线性回归的影响 📊

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_101.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_102.png)

上一节我们实现了标准化，本节中我们来看看标准化如何影响线性回归模型系数的解释。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_104.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_105.png)

首先，我们在**未标准化**的数据上拟合一个线性回归模型。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_106.png)

```python
from sklearn.linear_model import LinearRegression

lr = LinearRegression()
lr.fit(X, y)
print(lr.coef_)
```

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_108.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_109.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_110.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_111.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_112.png)

此时，系数的大小差异很大，因为它们依赖于原始特征的不同尺度。虽然这便于解释“一个单位的变化对目标的影响”，但无法直接比较哪个特征更重要。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_114.png)

接下来，我们在**标准化**后的数据上拟合另一个线性回归模型。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_115.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_116.png)

```python
lr_scaled = LinearRegression()
lr_scaled.fit(X_scaled, y)
print(lr_scaled.coef_)
```

现在，所有特征都处于同一尺度（均值为0，标准差为1）。系数的绝对值大小可以直接反映该特征对目标变量的影响程度（重要性）。正值表示正相关，负值表示负相关。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_118.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_119.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_120.png)

---

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_121.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_122.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_123.png)

## 识别最重要的特征 🏆

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_125.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_126.png)

为了清晰地看到哪个特征影响力最大，我们可以将特征名与标准化后的系数对应起来。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_128.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_129.png)

以下是创建特征重要性表格的步骤：

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_131.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_132.png)

1.  使用`zip`函数将特征名和系数配对。
2.  将其转换为DataFrame以便于查看和排序。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_134.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_136.png)

```python
# 将特征名与系数配对
coef_df = pd.DataFrame(zip(X.columns, lr_scaled.coef_), columns=['feature', 'coefficient'])

# 按系数绝对值排序，找出最重要的特征
coef_df['abs_coef'] = coef_df['coefficient'].abs()
print(coef_df.sort_values('abs_coef', ascending=False))
```

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_138.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_139.png)

例如，结果可能显示`RM`（房间数量）具有最大的正系数，而`LSTAT`（低收入人口比例）具有最大的负系数，这与我们的直觉相符。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_140.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_141.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_143.png)

---

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_145.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_146.png)

## 总结 📝

本节课中我们一起学习了数据标准化的核心概念与实现方法。我们了解到：
1.  标准化的目的是将数据转换为均值为0、标准差为1的分布。
2.  可以使用Scikit-learn的`StandardScaler`快速实现，也可以通过NumPy手动计算来理解其原理。
3.  在线性回归中，对标准化后的数据拟合模型，得到的系数可以直接用于评估特征的相对重要性，系数绝对值越大，特征影响力越强。

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_148.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_149.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_150.png)

![](img/2bfd6be60b4fc6c9f1dcc4b575b617d0_151.png)

下一节，我们将把标准化流程与训练-测试集划分结合起来，并探索Lasso和Ridge等正则化回归方法，以防止模型过拟合。