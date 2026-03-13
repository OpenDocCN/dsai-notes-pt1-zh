# 081：多项式特征与正则化演示（第一部分）📊

![](img/b2ac712cb25f0490f3a7935794d0e97c_0.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_2.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_3.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_4.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_5.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_6.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_7.png)

在本节课中，我们将学习如何使用多项式特征和正则化技术来拟合数据，并理解过拟合与欠拟合之间的平衡。我们将通过一个具体的演示，展示如何从带有噪声的数据中恢复出真实的底层函数模型。

![](img/b2ac712cb25f0490f3a7935794d0e97c_8.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_10.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_12.png)

---

![](img/b2ac712cb25f0490f3a7935794d0e97c_13.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_14.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_16.png)

## 第一部分：数据准备与可视化 📈

![](img/b2ac712cb25f0490f3a7935794d0e97c_17.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_18.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_19.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_20.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_22.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_24.png)

首先，我们需要导入必要的库并加载数据。我们将使用一个包含噪声的数据集，其真实底层函数是一个正弦波。

![](img/b2ac712cb25f0490f3a7935794d0e97c_26.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_27.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_28.png)

以下是需要导入的库：

![](img/b2ac712cb25f0490f3a7935794d0e97c_30.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_31.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_32.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_34.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_35.png)

```python
import os
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_37.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_38.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_39.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_41.png)

接下来，我们设置数据路径并加载数据。数据文件 `X_Y_sinoid_data.csv` 包含了在真实函数基础上添加了随机噪声的数据点。

![](img/b2ac712cb25f0490f3a7935794d0e97c_43.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_44.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_46.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_48.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_50.png)

```python
data_path = ‘data’
file_path = os.path.join(data_path, ‘X_Y_sinoid_data.csv’)
data = pd.read_csv(file_path)
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_52.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_54.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_56.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_58.png)

为了进行比较，我们还需要生成真实的底层函数。我们将在0到1的区间内生成100个等间距的x值，并计算对应的y值，其函数为 **y = sin(2πx)**。

![](img/b2ac712cb25f0490f3a7935794d0e97c_60.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_62.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_64.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_66.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_68.png)

```python
x_real = np.linspace(0, 1, 100)
y_real = np.sin(2 * np.pi * x_real)
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_70.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_71.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_73.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_74.png)

现在，让我们将带有噪声的数据点和真实的函数绘制在同一张图上，以便直观地理解我们的任务。

![](img/b2ac712cb25f0490f3a7935794d0e97c_76.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_77.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_79.png)

```python
sns.set_style(‘whitegrid’)
sns.set_palette(‘deep’)

![](img/b2ac712cb25f0490f3a7935794d0e97c_81.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_82.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_84.png)

ax = data.plot(x=‘x’, y=‘y’, linestyle=‘’, marker=‘o’, label=‘Data’)
ax.plot(x_real, y_real, linestyle=‘--’, label=‘Real Function’)
ax.legend()
plt.xlabel(‘x data’)
plt.ylabel(‘y data’)
plt.show()
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_86.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_87.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_88.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_90.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_92.png)

从图中我们可以看到，蓝色圆点代表带有噪声的观测数据，红色虚线代表我们希望模型能够逼近的真实正弦函数。我们的目标是找到一个模型，既能很好地拟合数据的整体趋势，又不会过度拟合那些随机噪声。

![](img/b2ac712cb25f0490f3a7935794d0e97c_93.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_95.png)

---

![](img/b2ac712cb25f0490f3a7935794d0e97c_96.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_97.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_98.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_99.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_101.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_102.png)

## 第二部分：使用多项式特征与线性回归（无正则化）🔍

![](img/b2ac712cb25f0490f3a7935794d0e97c_103.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_105.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_106.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_108.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_110.png)

上一节我们观察了数据，本节中我们来看看如何使用高阶多项式特征来拟合这些数据。我们将创建一个20次多项式，并使用普通的线性回归进行拟合，以此展示过拟合现象。

![](img/b2ac712cb25f0490f3a7935794d0e97c_112.png)

首先，从 `sklearn` 导入必要的模块并创建多项式特征转换器。

![](img/b2ac712cb25f0490f3a7935794d0e97c_114.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_115.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_116.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_117.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_119.png)

```python
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression

![](img/b2ac712cb25f0490f3a7935794d0e97c_120.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_122.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_123.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_125.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_126.png)

degree = 20
poly = PolynomialFeatures(degree=degree)
lr = LinearRegression()
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_128.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_129.png)

**注意**：`PolynomialFeatures` 要求输入是二维数组（形状为 `[n_samples, n_features]`）或 `DataFrame`。因此，我们需要确保 `X` 数据是二维格式。

![](img/b2ac712cb25f0490f3a7935794d0e97c_131.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_132.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_133.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_135.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_136.png)

以下是准备数据、拟合模型并进行预测的步骤：

![](img/b2ac712cb25f0490f3a7935794d0e97c_138.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_140.png)

```python
X_data = data[[‘x’]]  # 使用双括号以保持DataFrame结构
y_data = data[‘y’]

![](img/b2ac712cb25f0490f3a7935794d0e97c_142.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_143.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_144.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_145.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_146.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_148.png)

X_poly = poly.fit_transform(X_data)  # 将特征转换为20次多项式
lr.fit(X_poly, y_data)               # 在多项式特征上拟合线性回归模型
y_pred = lr.predict(X_poly)          # 对训练数据进行预测
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_150.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_152.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_154.png)

现在，我们将预测结果、原始数据以及真实函数一起绘制出来。

![](img/b2ac712cb25f0490f3a7935794d0e97c_156.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_157.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_159.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_160.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_162.png)

```python
plt.figure(figsize=(10, 6))
plt.plot(data[‘x’], data[‘y’], ‘o’, label=‘Data’, alpha=0.8)
plt.plot(x_real, y_real, ‘--’, label=‘Real Function’)
plt.plot(data[‘x’], y_pred, ‘^’, label=‘Predictions with Polynomial Features’, alpha=0.5)
plt.legend()
plt.xlabel(‘x data’)
plt.ylabel(‘y data’)
plt.show()
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_163.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_165.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_166.png)

从结果图中可以清晰地看到，20次多项式模型完美地穿过了每一个数据点（包括噪声），其预测曲线（绿色三角标记）剧烈波动，与平滑的真实函数（红色虚线）相去甚远。这就是典型的**过拟合**：模型过于复杂，捕捉了数据中的噪声，导致泛化能力变差。

![](img/b2ac712cb25f0490f3a7935794d0e97c_168.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_169.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_171.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_172.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_174.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_176.png)

---

![](img/b2ac712cb25f0490f3a7935794d0e97c_177.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_179.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_180.png)

## 第三部分：引入正则化：岭回归与Lasso回归 🛡️

![](img/b2ac712cb25f0490f3a7935794d0e97c_182.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_183.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_184.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_186.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_187.png)

上一节我们看到了无正则化的高阶多项式会导致严重的过拟合。本节中，我们将引入正则化技术来控制模型复杂度。我们将分别使用**岭回归 (Ridge Regression)** 和 **Lasso回归**，并观察它们如何改善模型。

![](img/b2ac712cb25f0490f3a7935794d0e97c_189.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_190.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_192.png)

首先，导入正则化回归模型。

![](img/b2ac712cb25f0490f3a7935794d0e97c_193.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_194.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_196.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_198.png)

```python
from sklearn.linear_model import Ridge, Lasso
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_200.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_202.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_204.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_206.png)

接下来，我们使用相同的20次多项式特征，但分别用岭回归和Lasso回归进行拟合。岭回归使用L2正则化，Lasso回归使用L1正则化。

![](img/b2ac712cb25f0490f3a7935794d0e97c_207.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_209.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_211.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_212.png)

```python
# 岭回归，正则化强度 alpha=0.001
ridge = Ridge(alpha=0.001)
ridge.fit(X_poly, y_data)
y_pred_ridge = ridge.predict(X_poly)

![](img/b2ac712cb25f0490f3a7935794d0e97c_214.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_216.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_218.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_220.png)

# Lasso回归，正则化强度 alpha=0.0001
lasso = Lasso(alpha=0.0001)
lasso.fit(X_poly, y_data)
y_pred_lasso = lasso.predict(X_poly)
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_222.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_223.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_224.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_226.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_228.png)

现在，我们将所有模型的拟合结果进行对比可视化。

![](img/b2ac712cb25f0490f3a7935794d0e97c_230.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_231.png)

```python
plt.figure(figsize=(10, 6))
plt.plot(data[‘x’], data[‘y’], ‘o’, label=‘Data’, alpha=0.8)
plt.plot(x_real, y_real, ‘--’, label=‘Real Function’)
plt.plot(data[‘x’], y_pred, ‘^’, label=‘LR (Overfit)’, alpha=0.5)
plt.plot(data[‘x’], y_pred_ridge, ‘s’, label=‘Ridge’, alpha=0.5)
plt.plot(data[‘x’], y_pred_lasso, ‘d’, label=‘Lasso’, alpha=0.5)
plt.legend()
plt.xlabel(‘x data’)
plt.ylabel(‘y data’)
plt.show()
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_233.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_234.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_235.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_237.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_238.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_239.png)

从图中可以看出，加入了正则化的岭回归和Lasso回归的预测曲线（方形和菱形标记）变得更加平滑，并且更接近真实的底层函数（红色虚线）。这表明正则化有效地降低了模型的方差，在偏差和方差之间取得了更好的平衡，缓解了过拟合问题。

![](img/b2ac712cb25f0490f3a7935794d0e97c_241.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_243.png)

---

![](img/b2ac712cb25f0490f3a7935794d0e97c_245.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_246.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_247.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_249.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_250.png)

### 分析模型系数 📉

![](img/b2ac712cb25f0490f3a7935794d0e97c_252.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_253.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_254.png)

为了更深入地理解正则化的作用，我们来比较一下不同模型的系数（权重）大小。正则化的一个核心作用就是限制系数的大小。

![](img/b2ac712cb25f0490f3a7935794d0e97c_256.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_257.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_258.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_259.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_261.png)

首先，我们提取并计算各个模型系数的绝对值。

![](img/b2ac712cb25f0490f3a7935794d0e97c_263.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_265.png)

```python
coeff_df = pd.DataFrame()
coeff_df[‘Linear’] = lr.coef_.ravel()
coeff_df[‘Ridge’] = ridge.coef_.ravel()
coeff_df[‘Lasso’] = lasso.coef_.ravel()

![](img/b2ac712cb25f0490f3a7935794d0e97c_267.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_269.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_271.png)

coeff_df_abs = coeff_df.applymap(abs)
print(coeff_df_abs.describe())
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_273.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_274.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_275.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_276.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_278.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_279.png)

通过描述性统计可以发现，无正则化的线性回归模型系数值巨大（数量级达到10^14），而岭回归和Lasso回归的系数值被显著缩小（在10^1数量级）。此外，Lasso回归还将许多系数直接压缩为零，实现了特征选择的效果。

![](img/b2ac712cb25f0490f3a7935794d0e97c_281.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_283.png)

最后，我们通过双Y轴图来直观展示系数大小的差异。

![](img/b2ac712cb25f0490f3a7935794d0e97c_285.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_286.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_287.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_288.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_289.png)

```python
fig, ax1 = plt.subplots(figsize=(12, 6))

![](img/b2ac712cb25f0490f3a7935794d0e97c_291.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_293.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_294.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_296.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_298.png)

colors = sns.color_palette()
# 在第一个Y轴上绘制线性回归的系数（数值很大）
ax1.plot(lr.coef_, color=colors[0], marker=‘o’, label=‘Linear Regression Coef’)
ax1.set_ylabel(‘Linear Regression Coefficients’, color=colors[0])
ax1.tick_params(axis=‘y’, labelcolor=colors[0])
ax1.set_ylim([-2e14, 2e14])

![](img/b2ac712cb25f0490f3a7935794d0e97c_300.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_302.png)

# 创建第二个共享X轴的Y轴
ax2 = ax1.twinx()
# 在第二个Y轴上绘制岭回归和Lasso的系数（数值较小）
ax2.plot(ridge.coef_, color=colors[1], marker=‘s’, label=‘Ridge Regression Coef’)
ax2.plot(lasso.coef_, color=colors[2], marker=‘d’, label=‘Lasso Regression Coef’)
ax2.set_ylabel(‘Ridge & Lasso Coefficients’, color=‘black’)
ax2.tick_params(axis=‘y’)
ax2.set_ylim([-25, 25])

![](img/b2ac712cb25f0490f3a7935794d0e97c_304.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_305.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_306.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_308.png)

# 合并图例
lines_1, labels_1 = ax1.get_legend_handles_labels()
lines_2, labels_2 = ax2.get_legend_handles_labels()
ax2.legend(lines_1 + lines_2, labels_1 + labels_2, loc=‘upper right’)

![](img/b2ac712cb25f0490f3a7935794d0e97c_309.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_310.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_312.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_313.png)

plt.xlabel(‘Coefficient Index’)
plt.title(‘Comparison of Model Coefficients (with and without regularization)’)
plt.show()
```

![](img/b2ac712cb25f0490f3a7935794d0e97c_314.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_315.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_316.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_318.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_320.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_321.png)

该图清晰地展示了正则化的威力：无正则化的模型系数波动剧烈且绝对值极大，而经过正则化的模型系数被约束在一个很小的范围内，Lasso回归的许多系数更是变成了零。

![](img/b2ac712cb25f0490f3a7935794d0e97c_323.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_325.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_326.png)

---

![](img/b2ac712cb25f0490f3a7935794d0e97c_328.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_330.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_331.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_332.png)

## 总结 🎯

![](img/b2ac712cb25f0490f3a7935794d0e97c_334.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_335.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_337.png)

本节课中我们一起学习了多项式特征和正则化的核心应用。

![](img/b2ac712cb25f0490f3a7935794d0e97c_339.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_340.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_342.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_343.png)

1.  **数据与目标**：我们使用了一个由 **y = sin(2πx)** 加噪声生成的数据集，目标是让模型学习真实的底层函数。
2.  **过拟合问题**：直接使用高阶（20次）多项式特征配合线性回归会导致严重的过拟合，模型完美拟合噪声，泛化能力差。
3.  **正则化解决方案**：我们引入了**岭回归 (L2正则化)** 和 **Lasso回归 (L1正则化)**。
    *   它们通过在损失函数中增加惩罚项来限制模型系数的大小。
    *   结果证明，正则化后的模型预测曲线更平滑，更接近真实函数，有效控制了过拟合。
4.  **系数分析**：通过比较模型系数，我们发现正则化能显著缩小系数值，Lasso回归还能产生稀疏解（许多系数为零），起到了自动特征选择的作用。

![](img/b2ac712cb25f0490f3a7935794d0e97c_345.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_346.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_348.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_349.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_350.png)

![](img/b2ac712cb25f0490f3a7935794d0e97c_351.png)

通过本课的演示，我们直观地理解了偏差-方差权衡，以及正则化如何作为一种强大的工具来提升模型的泛化性能。在下一部分，我们将在真实数据集上应用这些技术，并评估它们在测试集上的表现。