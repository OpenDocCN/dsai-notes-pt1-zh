# 087：Lasso回归详解与实践 🎯

![](img/c5e77473cf60ceaa1a7e178c880358cb_0.png)

在本节课中，我们将深入学习Lasso回归，理解其与普通线性回归的区别，并通过代码实践探索正则化参数`alpha`如何影响模型复杂度与性能。

![](img/c5e77473cf60ceaa1a7e178c880358cb_2.png)

## 概述

上一节我们介绍了正则化的基本概念。本节我们将聚焦于**Lasso回归**，这是一种通过向成本函数添加系数绝对值惩罚项来实现特征选择和防止过拟合的技术。我们将通过对比不同`alpha`值下的模型表现，来直观理解正则化的作用。

---

## Lasso回归与线性回归的区别

![](img/c5e77473cf60ceaa1a7e178c880358cb_4.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_6.png)

Lasso回归与普通线性回归的核心区别在于其**成本函数**。Lasso在普通最小二乘法的成本函数基础上，增加了一项对模型系数绝对值的惩罚。

![](img/c5e77473cf60ceaa1a7e178c880358cb_8.png)

**Lasso的成本函数公式**可以表示为：
`J(θ) = MSE(θ) + α * Σ|θ_i|`
其中，`α`是控制正则化强度的超参数，`Σ|θ_i|`是所有模型系数绝对值的总和（L1范数）。

![](img/c5e77473cf60ceaa1a7e178c880358cb_9.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_10.png)

## 特征标准化的重要性

![](img/c5e77473cf60ceaa1a7e178c880358cb_12.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_13.png)

在Lasso回归中，**特征标准化**比在线性回归中更为重要。

![](img/c5e77473cf60ceaa1a7e178c880358cb_15.png)

在线性回归中，系数大小不受惩罚，因此特征尺度主要影响系数的解释性。而在Lasso中，惩罚项直接作用于系数绝对值。如果特征尺度不同，系数大小会受原始特征尺度影响，导致尺度大的特征对应的系数更容易被惩罚（压缩至零），这并非基于特征重要性，而是基于其数值尺度。

![](img/c5e77473cf60ceaa1a7e178c880358cb_17.png)

因此，在使用Lasso前，必须将所有特征标准化到相同尺度，以确保公平的惩罚。

---

![](img/c5e77473cf60ceaa1a7e178c880358cb_19.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_20.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_22.png)

## Lasso回归实践：数据准备

以下是构建Lasso回归模型前的数据准备步骤，包括生成多项式特征和标准化。

![](img/c5e77473cf60ceaa1a7e178c880358cb_24.png)

首先，我们导入必要的库并创建多项式特征。

```python
# 导入Lasso模型和多项式特征生成器
from sklearn.linear_model import Lasso
from sklearn.preprocessing import PolynomialFeatures
from sklearn.preprocessing import StandardScaler

![](img/c5e77473cf60ceaa1a7e178c880358cb_26.png)

# 初始化多项式特征对象，设置度为2，不包含偏置项（截距）
poly = PolynomialFeatures(degree=2, include_bias=False)
```

![](img/c5e77473cf60ceaa1a7e178c880358cb_27.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_29.png)

我们不包含偏置项（`include_bias=False`），因为Lasso模型会自动处理截距项。如果包含，会生成一列全为1的特征，并学习其系数，但这是多余的。

![](img/c5e77473cf60ceaa1a7e178c880358cb_31.png)

接着，我们生成多项式特征并进行标准化。

![](img/c5e77473cf60ceaa1a7e178c880358cb_33.png)

```python
# 对原始特征X生成多项式特征
X_poly = poly.fit_transform(X)

![](img/c5e77473cf60ceaa1a7e178c880358cb_34.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_35.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_36.png)

# 初始化标准化器并对多项式特征进行标准化
scaler = StandardScaler()
X_poly_scaled = scaler.fit_transform(X_poly)
```
变量`X_poly_scaled`就是我们准备好的、经过标准化处理的多项式特征。

![](img/c5e77473cf60ceaa1a7e178c880358cb_38.png)

---

![](img/c5e77473cf60ceaa1a7e178c880358cb_40.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_41.png)

## 探索正则化强度：Alpha参数的影响

![](img/c5e77473cf60ceaa1a7e178c880358cb_43.png)

现在，我们使用默认参数的Lasso模型进行拟合，并观察其系数。

![](img/c5e77473cf60ceaa1a7e178c880358cb_45.png)

```python
# 使用默认alpha值（1.0）初始化Lasso模型
lasso_default = Lasso()
lasso_default.fit(X_poly_scaled, y)

![](img/c5e77473cf60ceaa1a7e178c880358cb_47.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_48.png)

# 查看模型系数
coefficients = lasso_default.coef_
```
默认情况下，`alpha`值为1.0。这是一个相对较大的值，意味着较强的正则化，会导致许多系数被压缩为零，从而得到一个更简单的模型。

![](img/c5e77473cf60ceaa1a7e178c880358cb_50.png)

为了量化模型复杂度，我们通常关注两个指标：
1.  **系数总幅度**：所有系数绝对值的和。总和越大，模型可能越复杂。
2.  **非零系数数量**：未被正则化压缩至零的特征数量。数量越少，模型越稀疏。

![](img/c5e77473cf60ceaa1a7e178c880358cb_52.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_53.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_54.png)

以下是计算这两个指标的代码逻辑：
```python
# 计算系数总幅度
magnitude = np.sum(np.abs(coefficients))
# 计算非零系数数量
non_zero_count = np.sum(coefficients != 0)
```

接下来，我们通过对比`alpha=0.1`（弱正则化）和`alpha=1.0`（强正则化）来验证其影响。

![](img/c5e77473cf60ceaa1a7e178c880358cb_56.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_57.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_58.png)

**理论预期**：`alpha`值越大，正则化越强，会导致系数总幅度越小，非零系数数量越少（更多系数被置零）。

![](img/c5e77473cf60ceaa1a7e178c880358cb_60.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_62.png)

**实践验证**：
我们分别用`alpha=0.1`和`alpha=1.0`训练Lasso模型，并计算上述指标。
*   当`alpha=0.1`时，我们可能得到总幅度为26.13，非零系数为23个。
*   当`alpha=1.0`时，我们可能得到总幅度为8.4，非零系数仅为7个。

结果符合预期：更强的正则化（更高的`alpha`）产生了幅度更小、更稀疏的系数向量，即更简单的模型。

![](img/c5e77473cf60ceaa1a7e178c880358cb_64.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_65.png)

---

## 模型评估与泛化能力

现在，我们评估默认模型（`alpha=1.0`）在训练数据上的R²分数。

```python
from sklearn.metrics import r2_score
y_pred_train = lasso_default.predict(X_poly_scaled)
r2_train = r2_score(y, y_pred_train)
# 结果可能约为0.72
```

![](img/c5e77473cf60ceaa1a7e178c880358cb_67.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_68.png)

**一个重要概念**：正则化的目的是防止对训练数据过拟合，从而提升模型在**未见过的数据**（测试集）上的泛化能力。因此，在训练集上增加正则化强度，通常会**降低**模型在训练集上的表现分数（如R²）。性能提升应体现在测试集或验证集上。

![](img/c5e77473cf60ceaa1a7e178c880358cb_70.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_72.png)

为了评估泛化能力，我们必须使用训练-测试集分割。

```python
from sklearn.model_selection import train_test_split

![](img/c5e77473cf60ceaa1a7e178c880358cb_74.png)

# 注意：这里使用未标准化的多项式特征进行分割
X_train, X_test, y_train, y_test = train_test_split(X_poly, y, test_size=0.3, random_state=42)

![](img/c5e77473cf60ceaa1a7e178c880358cb_76.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_78.png)

# 仅在训练集上拟合标准化器，并转换训练集和测试集
scaler_train = StandardScaler()
X_train_scaled = scaler_train.fit_transform(X_train)
X_test_scaled = scaler_train.transform(X_test) # 使用训练集的参数转换测试集

![](img/c5e77473cf60ceaa1a7e178c880358cb_80.png)

# 在标准化后的训练集上训练Lasso模型
lasso_for_test = Lasso(alpha=1.0) # 使用强正则化
lasso_for_test.fit(X_train_scaled, y_train)

![](img/c5e77473cf60ceaa1a7e178c880358cb_82.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_84.png)

# 在测试集上进行预测并计算R²分数
y_pred_test = lasso_for_test.predict(X_test_scaled)
r2_test_alpha1 = r2_score(y_test, y_pred_test)
# 结果可能较低，例如0.33
```

![](img/c5e77473cf60ceaa1a7e178c880358cb_85.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_87.png)

测试集上的低分数（0.33）表明，`alpha=1.0`的正则化可能过强，导致模型过于简单（高偏差），无法捕捉数据中的真实关系。

![](img/c5e77473cf60ceaa1a7e178c880358cb_89.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_91.png)

让我们尝试一个更弱的正则化（`alpha=0.1`）：
```python
lasso_weak = Lasso(alpha=0.1)
lasso_weak.fit(X_train_scaled, y_train)
y_pred_test_weak = lasso_weak.predict(X_test_scaled)
r2_test_alpha01 = r2_score(y_test, y_pred_test_weak)
# 结果可能会提高，例如0.65
```

![](img/c5e77473cf60ceaa1a7e178c880358cb_93.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_94.png)

测试集性能的提升说明，`alpha=0.1`的模型找到了更好的偏差-方差平衡点。而`alpha=1.0`的模型则因正则化过度，模型过于“愚笨”。

![](img/c5e77473cf60ceaa1a7e178c880358cb_96.png)

---

![](img/c5e77473cf60ceaa1a7e178c880358cb_98.png)

## 总结

本节课我们一起深入学习了Lasso回归。
*   我们理解了Lasso通过**L1惩罚项**（系数绝对值之和）在成本函数中实现正则化，既能防止过拟合，又能进行特征选择。
*   我们认识到对特征进行**标准化**在Lasso中至关重要，以确保惩罚的公平性。
*   我们通过实践验证了超参数 **`alpha`** 的作用：`alpha`越大，正则化越强，模型系数总幅度越小，非零特征越少，模型越简单。
*   关键的一点是，正则化的优劣必须通过模型在**测试集或验证集**上的泛化性能来判断，而非训练集。我们的实验表明，找到合适的`alpha`值以获得最佳的偏差-方差平衡，是模型调优的核心。

![](img/c5e77473cf60ceaa1a7e178c880358cb_100.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_101.png)

![](img/c5e77473cf60ceaa1a7e178c880358cb_103.png)

在下一节课中，我们将尝试不同的`alpha`值，系统性地寻找最优的正则化强度，以最大化模型在测试集上的性能指标。