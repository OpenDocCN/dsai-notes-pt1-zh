# 56：使用 scikit-learn 进行机器学习（第二部分）🚀

![](img/a165a3dbdfe2a42de38b6b11de2edad6_1.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_3.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_5.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_7.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_9.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_11.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_13.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_14.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_16.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_18.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_20.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_22.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_24.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_26.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_28.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_30.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_32.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_33.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_35.png)

在本节课中，我们将要学习 scikit-learn 中的回归模型、特征工程、数据预处理、无监督学习（如降维和聚类）以及模型评估与选择等核心概念。我们将通过具体的代码示例和公式来深入理解这些内容。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_37.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_39.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_41.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_42.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_44.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_46.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_48.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_50.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_52.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_54.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_56.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_58.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_60.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_62.png)

---

![](img/a165a3dbdfe2a42de38b6b11de2edad6_64.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_66.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_68.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_70.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_72.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_74.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_76.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_78.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_80.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_82.png)

## 概述 📋

![](img/a165a3dbdfe2a42de38b6b11de2edad6_84.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_86.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_88.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_90.png)

上一节我们介绍了分类模型，本节中我们来看看回归模型。我们将从线性回归开始，探讨如何通过特征工程增强模型能力，然后介绍 K-近邻回归器。接着，我们会学习数据预处理的重要性，包括特征缩放和主成分分析（PCA）。最后，我们将了解如何使用交叉验证和网格搜索来评估和优化模型。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_92.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_94.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_96.png)

---

![](img/a165a3dbdfe2a42de38b6b11de2edad6_98.png)

## 1. 回归模型与特征工程

![](img/a165a3dbdfe2a42de38b6b11de2edad6_100.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_102.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_104.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_106.png)

### 1.1 线性回归与非线性特征

![](img/a165a3dbdfe2a42de38b6b11de2edad6_108.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_110.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_112.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_114.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_115.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_117.png)

我们有一个变量 `x`，其目标值是 `x + sin(4x)`。线性模型本身无法捕捉这种非线性关系。为了增强模型，我们可以通过特征工程，将 `sin(4x)` 作为一个新特征加入到数据中。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_119.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_121.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_123.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_125.png)

以下是实现步骤：
```python
import numpy as np
# 假设 x_train 和 x_test 是原始特征
x_train_augmented = np.concatenate([x_train, np.sin(4 * x_train)], axis=1)
x_test_augmented = np.concatenate([x_test, np.sin(4 * x_test)], axis=1)
```
然后，我们使用这个增加了新特征的数据来训练线性回归模型并进行预测。通过这种方式，即使使用线性模型，我们也能捕捉到数据中的非线性模式，因为我们将先验知识编码到了特征提取部分。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_127.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_129.png)

**核心思想**：将添加新特征（原始特征的非线性变换）和拟合线性回归模型这两个步骤结合起来，就构成了一个非线性模型。这使得我们可以在特征提取部分融入对问题的先验知识，最后使用简单的线性回归模型进行最终预测。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_131.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_133.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_135.png)

### 1.2 K-近邻回归器

![](img/a165a3dbdfe2a42de38b6b11de2edad6_137.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_139.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_141.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_143.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_144.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_145.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_147.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_149.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_151.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_153.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_155.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_157.png)

接下来，我们使用 K-近邻回归器。它的工作原理与 K-近邻分类器类似，但用于回归任务。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_158.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_160.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_162.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_164.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_166.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_168.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_170.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_172.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_174.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_176.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_177.png)

我们可以定义回归器中邻居的数量，并在训练数据上拟合模型。
```python
from sklearn.neighbors import KNeighborsRegressor
knn_reg = KNeighborsRegressor(n_neighbors=1)
knn_reg.fit(x_train, y_train)
y_pred_train = knn_reg.predict(x_train)
```
当 `n_neighbors=1` 时，在训练集上的预测会完全拟合数据点。然而，我们更关心模型在测试集上的表现。非参数模型（如 K-近邻）通常比线性模型表现更好，因为它利用邻居信息进行插值，从而能捕捉一些非线性。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_178.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_179.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_181.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_183.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_185.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_187.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_189.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_191.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_193.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_195.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_197.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_199.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_201.png)

我们可以计算决定系数 R² 分数来比较模型性能。例如，K-近邻回归器可能得到 0.91 的分数，而线性回归可能只有 0.79。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_203.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_205.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_207.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_209.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_211.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_213.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_215.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_217.png)

**调整邻居数量**：增加邻居数量（例如 5 或 10）可以起到正则化效果，防止模型过拟合噪声。在训练集上，预测不会完全精确，但能更好地跟随趋势。在测试集上，性能可能会从 0.91 提升到 0.94，因为模型对噪声更加鲁棒。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_219.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_221.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_223.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_225.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_227.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_229.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_231.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_233.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_235.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_237.png)

**K-近邻的优缺点**：
*   **优点**：作为非参数模型，它对数据结构假设很少，仅假设局部点之间的距离是有意义的。与需要手动特征工程的线性模型不同，它能自动发现数据中的周期性结构。
*   **缺点**：无法在训练集区域外进行周期性外推。而如果在线性模型中通过特征工程编码了周期性，则可以进行完美外推。此外，K-近邻需要存储整个训练集来进行预测，这在大数据或嵌入式部署中可能带来存储、计算速度和隐私方面的问题。因此，在生产系统中不常用，但作为与更高级算法（如随机森林）比较的基线非常有效。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_239.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_241.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_243.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_245.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_246.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_248.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_250.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_252.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_254.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_256.png)

---

![](img/a165a3dbdfe2a42de38b6b11de2edad6_258.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_260.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_262.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_264.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_266.png)

## 2. 数据预处理与特征缩放

![](img/a165a3dbdfe2a42de38b6b11de2edad6_268.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_270.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_272.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_274.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_276.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_278.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_280.png)

上一节我们介绍了回归模型，本节中我们来看看数据预处理。不同特征可能具有不同的尺度和单位，这会影响许多机器学习算法（如 K-近邻、基于梯度下降的算法）的性能。因此，通常需要一个预处理步骤来缩放特征。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_282.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_284.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_286.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_288.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_290.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_292.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_294.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_296.png)

### 2.1 标准化 (StandardScaler)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_298.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_300.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_302.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_304.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_306.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_308.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_310.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_312.png)

最常用的缩放方法之一是标准化：对每个特征单独进行中心化（减去均值）并缩放（除以标准差），使其均值为 0，标准差为 1。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_314.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_316.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_318.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_320.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_322.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_324.png)

公式如下：
对于特征列 `x`，标准化后的值 `z` 为：
`z = (x - μ) / σ`
其中 `μ` 是特征均值，`σ` 是特征标准差。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_326.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_328.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_330.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_332.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_334.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_336.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_337.png)

在 scikit-learn 中，可以使用 `StandardScaler`：
```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
scaler.fit(X_train) # 仅在训练集上计算均值和标准差
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test) # 使用训练集的统计量来转换测试集
```
**重要原则**：用于转换测试集的统计量（均值和标准差）必须仅从训练集计算，而不能包含测试集数据，否则会导致信息泄露，使模型评估结果过于乐观。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_339.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_341.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_343.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_345.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_347.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_349.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_350.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_352.png)

### 2.2 其他缩放方法

scikit-learn 提供了多种缩放器，适用于不同场景：

![](img/a165a3dbdfe2a42de38b6b11de2edad6_354.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_356.png)

以下是几种常见缩放器的对比：
*   **StandardScaler**：标准化。适用于特征大致服从高斯分布且没有极端值的情况。
*   **MinMaxScaler**：将数据缩放到 [0, 1] 区间。适用于稀疏数据，希望保持零值不变的情况。
*   **RobustScaler**：使用中位数和四分位数范围进行缩放，对异常值不敏感。
*   **QuantileTransformer**：使用分位数信息将数据映射到均匀分布，能有效压缩异常值，但引入了非线性变换。
*   **Normalizer**：对每个样本（行）进行归一化，使其范数（如 L1 或 L2）为 1。常用于文本或图像数据，使样本长度不影响结果。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_358.png)

**选择依据**：缩放方法的选择取决于数据和模型。例如，树模型通常对缩放不敏感，而线性模型和 K-近邻则需要。如果数据是近似高斯的连续值且无异常值，`StandardScaler` 是最常见的选择。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_360.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_362.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_364.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_366.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_368.png)

---

![](img/a165a3dbdfe2a42de38b6b11de2edad6_370.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_372.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_374.png)

## 3. 无监督学习：降维与聚类

### 3.1 主成分分析 (PCA) 🎯

PCA 是一种用于降维的无监督方法。其目标是找到一个新的特征子空间（主成分），使得数据在这些新方向上的方差最大，从而保留最重要的信息。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_376.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_378.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_379.png)

**工作原理**：PCA 找到数据方差最大的方向作为第一主成分，与之正交且方差次大的方向作为第二主成分，依此类推。然后可以将数据投影到少数几个主成分上，实现降维。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_381.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_383.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_385.png)

在 scikit-learn 中使用 PCA：
```python
from sklearn.decomposition import PCA
pca = PCA(n_components=2) # 保留两个主成分
X_train_pca = pca.fit_transform(X_train_scaled)
```
我们可以查看每个主成分解释的方差比例：
```python
print(pca.explained_variance_ratio_)
```
通常，我们可以设置 `n_components` 为一个浮点数（如 0.95），表示保留解释 95% 方差的成分。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_387.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_389.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_391.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_393.png)

**特点与局限**：PCA 是无监督的，不利用目标变量 `y` 的信息。它寻找的是解释数据总体方差最大的方向，但这个方向不一定是对分类或回归最有判别力的方向。有时有用的信号可能存在于低方差子空间中，使用 PCA 可能会丢弃这些信号。因此，PCA 不能保证作为监督模型的预处理步骤总是有效的，但它常用于数据可视化。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_395.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_396.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_397.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_399.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_401.png)

### 3.2 聚类分析 🔍

![](img/a165a3dbdfe2a42de38b6b11de2edad6_403.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_405.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_407.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_409.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_411.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_413.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_415.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_417.png)

聚类是一种无监督学习，旨在将数据样本分组，使得同一组内的样本彼此相似。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_419.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_421.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_423.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_425.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_427.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_429.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_431.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_433.png)

#### 3.2.1 K-均值聚类 (K-Means)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_435.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_437.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_439.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_441.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_443.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_445.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_447.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_449.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_451.png)

K-Means 需要预先指定聚类数量 `k`。算法会找到 `k` 个簇中心，并将每个样本分配到最近的中心。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_453.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_455.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_457.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_458.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_459.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_461.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_462.png)

```python
from sklearn.cluster import KMeans
kmeans = KMeans(n_clusters=3)
cluster_labels = kmeans.fit_predict(X)
```
**注意**：聚类算法分配的标签（如 0, 1, 2）是任意的，与数据真实的类别标签没有对应关系。因此，不能使用分类准确率等监督指标来评估聚类质量。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_464.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_466.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_468.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_470.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_472.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_474.png)

**评估聚类质量**：可以使用调整兰德指数 (Adjusted Rand Index, ARI)。它通过比较样本对在真实标签和聚类标签中的归属一致性来评估聚类效果，对标签排列保持不变性。完美匹配时 ARI 为 1，随机聚类时接近 0。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_476.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_478.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_479.png)

**选择聚类数量 `k`**：一个常用的方法是“肘部法则”，绘制不同 `k` 值对应的簇内误差平方和（惯性），选择惯性下降速度骤减的点（肘部）对应的 `k` 值。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_481.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_483.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_485.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_487.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_489.png)

**K-Means 的假设与局限**：K-Means 假设每个簇是球形的、大小相近。如果数据结构不符合这些假设（如嵌套结构、流形结构），K-Means 可能效果不佳。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_491.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_493.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_495.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_497.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_499.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_501.png)

#### 3.2.2 其他聚类算法

![](img/a165a3dbdfe2a42de38b6b11de2edad6_503.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_505.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_507.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_509.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_511.png)

scikit-learn 提供了多种聚类算法，适用于不同数据结构：
*   **DBSCAN**：基于密度的聚类，不需要指定簇数量，能识别噪声点，对任意形状的簇有效。
*   **AgglomerativeClustering**：层次聚类。
*   **SpectralClustering**：谱聚类，能发现复杂的聚类结构，但计算量较大。
*   **GaussianMixture**：高斯混合模型，软分配聚类，可以处理非球形的簇。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_513.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_515.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_517.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_519.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_521.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_523.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_525.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_527.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_529.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_531.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_533.png)

在实践中，DBSCAN 和 HDBSCAN（第三方库）通常是非常实用和鲁棒的选择。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_535.png)

---

![](img/a165a3dbdfe2a42de38b6b11de2edad6_537.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_539.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_541.png)

## 4. 模型评估与超参数调优

![](img/a165a3dbdfe2a42de38b6b11de2edad6_543.png)

### 4.1 交叉验证 (Cross-Validation)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_545.png)

为了更稳定地评估模型性能，避免单次训练-测试分割的随机性，我们使用交叉验证。最常用的是 K 折交叉验证。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_547.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_549.png)

**K 折交叉验证流程**：
1.  将数据集随机打乱后，平均分成 K 个子集（折）。
2.  依次将每个子集作为验证集，其余 K-1 个子集作为训练集。
3.  训练 K 个模型，并在对应的验证集上评估，得到 K 个性能分数。
4.  计算这 K 个分数的平均值和标准差，作为模型性能的估计。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_551.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_553.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_555.png)

在 scikit-learn 中实现：
```python
from sklearn.model_selection import cross_val_score, KFold
cv = KFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(estimator, X, y, cv=cv)
print(scores.mean(), scores.std())
```
**注意事项**：如果数据本身有顺序（如按标签排序），必须使用 `shuffle=True` 进行打乱，否则会导致验证集包含训练集从未见过的类别，得到错误的评估结果。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_557.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_559.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_561.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_563.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_565.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_567.png)

**交叉验证策略**：
*   `StratifiedKFold`：分层 K 折，确保每个折中各类别的比例与原始数据集一致，适用于分类问题。
*   `ShuffleSplit`：随机排列后，多次随机划分训练集和验证集，可以灵活控制训练集和验证集的大小。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_569.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_571.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_573.png)

### 4.2 超参数调优：网格搜索 (GridSearchCV)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_575.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_577.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_579.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_581.png)

机器学习模型有许多超参数（如 K-近邻的 `n_neighbors`，SVM 的 `C` 和 `gamma`）。手动寻找最优组合非常耗时。网格搜索可以自动化这个过程。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_583.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_585.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_587.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_589.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_591.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_593.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_595.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_597.png)

**网格搜索流程**：
1.  定义要搜索的超参数网格（所有可能的组合）。
2.  对网格中的每一组超参数，使用交叉验证评估模型性能。
3.  选择在交叉验证上平均性能最好的一组超参数。
4.  用这组最优超参数在整个训练集上重新训练最终模型。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_599.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_601.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_603.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_605.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_607.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_608.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_610.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_612.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_614.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_615.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_617.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_618.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_620.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_622.png)

```python
from sklearn.model_selection import GridSearchCV
from sklearn.svm import SVC

![](img/a165a3dbdfe2a42de38b6b11de2edad6_624.png)

param_grid = {'C': [0.1, 1, 10, 100], 'gamma': [0.001, 0.01, 0.1, 1]}
grid_search = GridSearchCV(SVC(), param_grid, cv=5)
grid_search.fit(X_train, y_train)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_626.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_628.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_630.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_632.png)

print("Best parameters:", grid_search.best_params_)
print("Best cross-validation score:", grid_search.best_score_)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_634.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_636.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_638.png)

best_model = grid_search.best_estimator_ # 获取最优模型
```
**随机搜索 (RandomizedSearchCV)**：当超参数组合太多时，网格搜索计算代价高。随机搜索从指定的分布中随机采样超参数组合进行尝试，通常能以更少的尝试次数找到近似最优解。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_640.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_642.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_644.png)

**更高级的调优**：可以使用如 Scikit-Optimize 等第三方库进行贝叶斯优化，更智能地搜索超参数空间。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_646.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_648.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_650.png)

### 4.3 完整的模型选择流程 🚀

![](img/a165a3dbdfe2a42de38b6b11de2edad6_652.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_654.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_656.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_658.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_660.png)

一个严谨的机器学习项目应遵循以下流程：
1.  **初始分割**：将全部数据划分为训练集和最终测试集。**最终测试集只在最后评估时使用一次**，以得到模型泛化能力的无偏估计。
2.  **模型开发与调优**：在训练集上进行。
    *   进行数据预处理、特征工程。
    *   使用交叉验证评估不同模型或同一模型的不同超参数。
    *   使用网格搜索或随机搜索找到最优超参数组合。
3.  **最终评估**：使用最优模型和超参数，在整个训练集上重新训练模型，然后在**最终测试集**上进行一次评估，得到最终的性能报告。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_662.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_663.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_665.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_667.png)

---

![](img/a165a3dbdfe2a42de38b6b11de2edad6_669.png)

## 总结 🎉

![](img/a165a3dbdfe2a42de38b6b11de2edad6_671.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_673.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_675.png)

本节课中我们一起学习了 scikit-learn 机器学习的多个核心主题：

![](img/a165a3dbdfe2a42de38b6b11de2edad6_677.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_679.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_681.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_683.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_685.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_687.png)

1.  **回归与特征工程**：通过添加非线性特征，可以使线性模型捕捉复杂模式。K-近邻回归作为非参数模型，有其适用场景和局限性。
2.  **数据预处理**：特征缩放（如标准化）对许多算法至关重要，需要正确地在训练集和测试集上应用。
3.  **无监督学习**：
    *   **PCA**：用于降维和可视化，通过最大化方差来寻找数据的主成分。
    *   **聚类**：如 K-Means，用于发现数据中的分组结构，评估时需使用如调整兰德指数等无监督指标。
4.  **模型评估与选择**：
    *   **交叉验证**：提供了更稳健的模型性能评估方法。
    *   **超参数调优**：使用网格搜索或随机搜索自动化寻找最佳模型配置的过程。
    *   **完整流程**：强调了将数据分为训练集、验证集（通过交叉验证模拟）和最终测试集的重要性，以确保评估结果的可靠性。

![](img/a165a3dbdfe2a42de38b6b11de2edad6_689.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_691.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_693.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_694.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_696.png)

![](img/a165a3dbdfe2a42de38b6b11de2edad6_698.png)

掌握这些概念和工具，你将能够构建、评估并优化更加强大和可靠的机器学习模型。