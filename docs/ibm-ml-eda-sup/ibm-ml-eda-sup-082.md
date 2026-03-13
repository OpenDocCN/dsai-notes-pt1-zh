# 082：多项式特征与正则化演示（第二部分）📊

在本节课中，我们将继续使用艾奥瓦州埃姆斯市的房价数据集，完成数据预处理、特征工程，并建立一个基础的线性回归模型作为后续正则化模型的比较基准。我们将学习如何导入数据、处理分类变量、进行对数变换，并评估模型性能。

![](img/7de10170a1cb9354da6e6b8158b2a582_1.png)

---

![](img/7de10170a1cb9354da6e6b8158b2a582_3.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_4.png)

## 数据导入与预处理 🗂️

首先，我们需要导入必要的库并加载数据集。我们将使用 `pandas` 进行数据处理，`scikit-learn` 进行数据分割和模型构建。

```python
import pandas as pd
from sklearn.model_selection import train_test_split
import numpy as np
```

![](img/7de10170a1cb9354da6e6b8158b2a582_6.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_7.png)

我们将加载数据集，并使用独热编码处理其中的分类特征。独热编码会将每个分类变量转换为多个二进制列。

![](img/7de10170a1cb9354da6e6b8158b2a582_9.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_11.png)

```python
file_path = 'path_to_your_data.csv'
data = pd.read_csv(file_path)
data = pd.get_dummies(data, drop_first=True)
```

`drop_first=True` 参数用于避免由独热编码引起的多重共线性问题。处理后，数据集的特征数量会增加。

![](img/7de10170a1cb9354da6e6b8158b2a582_13.png)

---

## 数据分割 📊

![](img/7de10170a1cb9354da6e6b8158b2a582_15.png)

接下来，我们将数据集分割为训练集和测试集。这里我们使用 `train_test_split` 函数，将70%的数据用于训练，30%用于测试。

![](img/7de10170a1cb9354da6e6b8158b2a582_16.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_17.png)

```python
train, test = train_test_split(data, test_size=0.3, random_state=42)
```

![](img/7de10170a1cb9354da6e6b8158b2a582_19.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_21.png)

这样我们就得到了 `train` 和 `test` 两个数据集，分别用于模型训练和评估。

---

![](img/7de10170a1cb9354da6e6b8158b2a582_23.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_25.png)

## 特征的对数变换 📈

许多数值特征可能呈现偏态分布。为了使其更接近正态分布，我们将对偏度大于0.75的特征进行对数变换。

以下是识别需要变换的特征的步骤：

![](img/7de10170a1cb9354da6e6b8158b2a582_27.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_28.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_29.png)

1.  首先，筛选出非独热编码的数值特征（即唯一值数量大于2的特征）。
2.  计算这些特征的偏度。
3.  选择偏度大于0.75的特征进行变换。

![](img/7de10170a1cb9354da6e6b8158b2a582_31.png)

```python
# 识别数值特征
numeric_feats = data.dtypes[data.dtypes != 'object'].index

![](img/7de10170a1cb9354da6e6b8158b2a582_33.png)

# 计算偏度
skewed_feats = data[numeric_feats].apply(lambda x: x.skew())
skewed_feats = skewed_feats[skewed_feats > 0.75]

![](img/7de10170a1cb9354da6e6b8158b2a582_35.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_36.png)

# 对偏态特征进行对数变换
for feat in skewed_feats.index:
    if feat != 'SalePrice': # 不对目标变量进行变换
        train[feat] = np.log1p(train[feat])
        test[feat] = np.log1p(test[feat])
```

![](img/7de10170a1cb9354da6e6b8158b2a582_38.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_39.png)

`np.log1p` 函数计算的是 log(1+x)，这可以避免当特征值为0时出现数学错误。

![](img/7de10170a1cb9354da6e6b8158b2a582_41.png)

---

![](img/7de10170a1cb9354da6e6b8158b2a582_43.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_45.png)

## 准备建模数据 🎯

![](img/7de10170a1cb9354da6e6b8158b2a582_47.png)

在应用对数变换后，我们需要将数据集分离为特征矩阵 `X` 和目标变量 `y`。

![](img/7de10170a1cb9354da6e6b8158b2a582_49.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_51.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_53.png)

```python
# 分离特征和目标变量
feature_cols = [col for col in train.columns if col != 'SalePrice']

![](img/7de10170a1cb9354da6e6b8158b2a582_55.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_57.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_59.png)

X_train = train[feature_cols]
y_train = train['SalePrice']
X_test = test[feature_cols]
y_test = test['SalePrice']
```

![](img/7de10170a1cb9354da6e6b8158b2a582_61.png)

现在，`X_train` 和 `y_train` 用于训练模型，`X_test` 和 `y_test` 用于评估模型性能。

![](img/7de10170a1cb9354da6e6b8158b2a582_63.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_64.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_65.png)

---

## 定义评估指标 📏

![](img/7de10170a1cb9354da6e6b8158b2a582_67.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_68.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_69.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_71.png)

为了评估模型，我们将使用均方根误差（RMSE）。RMSE是预测值与真实值之间差异的度量，其单位与目标变量相同，便于解释。

```python
from sklearn.metrics import mean_squared_error

![](img/7de10170a1cb9354da6e6b8158b2a582_73.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_75.png)

def rmse(y_true, y_pred):
    return np.sqrt(mean_squared_error(y_true, y_pred))
```

![](img/7de10170a1cb9354da6e6b8158b2a582_77.png)

这个函数接收真实值 `y_true` 和预测值 `y_pred`，计算并返回RMSE。

---

![](img/7de10170a1cb9354da6e6b8158b2a582_79.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_80.png)

## 建立与评估线性回归模型 🤖

现在，我们建立一个基础的线性回归模型，并使用RMSE评估其在测试集上的性能。

![](img/7de10170a1cb9354da6e6b8158b2a582_82.png)

```python
from sklearn.linear_model import LinearRegression

![](img/7de10170a1cb9354da6e6b8158b2a582_84.png)

# 创建并训练模型
lr_model = LinearRegression()
lr_model.fit(X_train, y_train)

![](img/7de10170a1cb9354da6e6b8158b2a582_86.png)

# 在测试集上进行预测
y_pred = lr_model.predict(X_test)

# 计算RMSE
lr_rmse = rmse(y_test, y_pred)
print(f"线性回归模型的RMSE为: {lr_rmse:.4f}")
```

![](img/7de10170a1cb9354da6e6b8158b2a582_88.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_89.png)

我们还可以通过绘制预测值与真实值的散点图来直观地检查模型性能。理想情况下，所有点应落在对角线上。

![](img/7de10170a1cb9354da6e6b8158b2a582_91.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_92.png)

```python
import matplotlib.pyplot as plt

![](img/7de10170a1cb9354da6e6b8158b2a582_94.png)

plt.figure(figsize=(6, 6))
plt.scatter(y_test, y_pred, alpha=0.5)
plt.plot([y_test.min(), y_test.max()], [y_test.min(), y_test.max()], 'r--', lw=2)
plt.xlabel('实际售价')
plt.ylabel('预测售价')
plt.title('线性回归：预测值 vs 实际值')
plt.show()
```

![](img/7de10170a1cb9354da6e6b8158b2a582_96.png)

---

![](img/7de10170a1cb9354da6e6b8158b2a582_98.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_100.png)

## 总结 📝

本节课中，我们一起学习了机器学习工作流中的关键步骤：
1.  **数据预处理**：使用独热编码处理分类变量。
2.  **特征工程**：通过对数变换处理偏态数值特征，使其分布更接近正态。
3.  **数据准备**：将数据分割为训练集和测试集，并分离特征与目标变量。
4.  **模型建立与评估**：构建了一个基础的线性回归模型，并使用均方根误差（RMSE）和散点图评估其性能。

![](img/7de10170a1cb9354da6e6b8158b2a582_102.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_104.png)

![](img/7de10170a1cb9354da6e6b8158b2a582_105.png)

这个基础的线性回归模型为我们提供了一个性能基准。在接下来的课程中，我们将引入**正则化**技术（如岭回归、Lasso回归和弹性网络），通过惩罚模型复杂度来防止过拟合，并观察它们是否能提升模型在测试集上的表现。