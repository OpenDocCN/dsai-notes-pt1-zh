# 人工智能—推荐系统公开课（七月在线出品） - P1：从零开始写代码：使用MovieLens的电影评分数据实践FM 🎬

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_1.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_2.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_4.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_6.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_8.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_10.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_12.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_14.png)

在本节课中，我们将要学习如何从零开始，使用Python代码实现一个基础的因子分解机（FM）模型。我们将使用MovieLens的电影评分数据集，通过实践来理解FM模型的核心思想、参数更新过程以及如何评估模型效果。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_16.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_18.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_20.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_22.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_24.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_26.png)

## 概述

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_28.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_30.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_32.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_34.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_36.png)

我们将从下载和读取数据集开始，逐步构建一个简化的FM模型。这个模型将专注于用户ID和电影ID这两个特征，通过它们的隐向量内积来预测评分。我们会定义损失函数，实现梯度下降算法进行训练，并最终在训练集和测试集上评估模型的性能。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_38.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_40.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_42.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_43.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_45.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_47.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_49.png)

## 数据准备与读取

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_50.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_52.png)

首先，我们需要获取并理解数据。我们使用MovieLens数据集中的一个评分文件。该文件的结构很简单，每一行包含三个字段：用户ID、电影ID和评分（1-5分），以及一个时间戳。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_54.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_56.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_58.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_60.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_62.png)

为了简化模型，我们只使用评分数据。以下是读取数据文件的函数实现：

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_64.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_66.png)

```python
def load_data(filepath):
    data = []
    with open(filepath, 'r') as f:
        for line in f:
            # 去除首尾空格，并按双冒号分割
            parts = line.strip().split('::')
            user_id = int(parts[0])
            movie_id = int(parts[1])
            rating = float(parts[2])
            # 将三元组(用户ID, 电影ID, 评分)加入列表
            data.append((user_id, movie_id, rating))
    return data
```

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_68.png)

运行这个函数后，我们可以打印前几项数据来确认读取是否正确。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_70.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_72.png)

## 定义模型参数与预测函数

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_74.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_76.png)

上一节我们介绍了如何读取数据，本节中我们来看看如何定义模型的核心参数和预测函数。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_78.png)

我们首先定义几个超参数：
*   `M`: 用户数量（约6000）
*   `N`: 电影数量（约3000）
*   `K`: 隐向量的维度，这是一个需要通过交叉验证来选择的超参数。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_80.png)

我们使用`numpy`来存储和计算参数。用户隐向量矩阵 `U` 的形状是 `M x K`，电影隐向量矩阵 `V` 的形状是 `N x K`。我们使用高斯分布进行随机初始化。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_82.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_84.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_86.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_88.png)

```python
import numpy as np

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_90.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_92.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_94.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_96.png)

M = 6000
N = 3000
K = 5
reg = 0.01  # 正则化系数
step = 0.001  # 梯度下降步长

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_98.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_100.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_102.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_104.png)

# 初始化参数矩阵
U = np.random.randn(M, K)
V = np.random.randn(N, K)
```

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_106.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_108.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_110.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_112.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_114.png)

接下来，我们定义预测函数。为了专注于理解FM的二次项交互，我们暂时忽略线性项和常数项。在我们的简化场景中，只有用户ID和电影ID两个特征是非零的，因此FM的复杂求和公式退化为这两个特征对应隐向量的内积。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_116.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_118.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_120.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_122.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_124.png)

预测用户 `i` 对电影 `j` 的评分公式为：
**`prediction = np.dot(U[i], V[j])`**

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_126.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_128.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_130.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_132.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_134.png)

对应的Python函数如下：

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_136.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_138.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_140.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_142.png)

```python
def predict(i, j):
    """预测用户i对电影j的评分"""
    return np.dot(U[i], V[j])
```

## 定义损失函数与梯度

我们的目标是预测评分，这是一个回归问题，因此使用均方误差（MSE）作为损失函数，并加入L2正则化项以防止过拟合。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_144.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_146.png)

单个样本 `(i, j, r)` 的损失函数 `L` 定义为：
**`L = (prediction - r)^2 + reg * (||U[i]||^2 + ||V[j]||^2)`**

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_148.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_150.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_151.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_153.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_155.png)

其中 `r` 是真实评分，`prediction` 是模型预测值。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_157.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_159.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_161.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_163.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_165.png)

为了使用梯度下降法优化参数，我们需要计算损失函数对参数 `U[i]` 和 `V[j]` 的梯度。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_167.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_169.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_171.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_173.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_175.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_177.png)

以下是梯度的计算公式：
*   对 `U[i]` 的梯度 `g_i`：**`g_i = 2 * (prediction - r) * V[j] + 2 * reg * U[i]`**
*   对 `V[j]` 的梯度 `g_j`：**`g_j = 2 * (prediction - r) * U[i] + 2 * reg * V[j]`**

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_179.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_181.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_183.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_185.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_187.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_189.png)

## 实现模型训练过程

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_191.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_193.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_195.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_197.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_199.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_201.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_203.png)

有了梯度的计算公式，我们就可以实现训练函数了。我们将采用随机梯度下降（SGD）的方法，遍历数据集中的每一个样本，并根据计算出的梯度更新对应的参数。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_205.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_207.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_209.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_211.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_213.png)

以下是训练一个轮次（epoch）的代码：

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_215.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_216.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_218.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_220.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_222.png)

```python
def train_one_epoch(data):
    """遍历数据集一次，更新参数"""
    for i, j, r in data:
        p = predict(i, j)  # 预测值
        # 计算梯度
        grad_u = 2 * (p - r) * V[j] + 2 * reg * U[i]
        grad_v = 2 * (p - r) * U[i] + 2 * reg * V[j]
        # 梯度下降更新参数
        U[i] -= step * grad_u
        V[j] -= step * grad_v
```

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_224.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_226.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_228.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_230.png)

## 模型评估与效果验证

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_232.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_234.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_236.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_238.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_240.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_242.png)

训练过程中，我们需要监控模型在训练集和测试集上的表现，以判断模型是否学习有效以及是否出现过拟合。我们实现两个辅助函数：一个用于在给定数据集上生成所有预测值，另一个用于计算均方根误差（RMSE）。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_244.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_246.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_248.png)

以下是相关函数：

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_250.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_252.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_254.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_256.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_258.png)

```python
def make_predictions(data):
    """为给定数据集生成预测列表"""
    preds = []
    for i, j, r in data:
        preds.append(predict(i, j))
    return preds

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_260.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_262.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_264.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_266.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_267.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_269.png)

def rmse(data, predictions):
    """计算真实评分和预测评分之间的RMSE"""
    total_error = 0.0
    count = 0
    for idx, (i, j, r) in enumerate(data):
        p = predictions[idx]
        total_error += (p - r) ** 2
        count += 1
    return np.sqrt(total_error / count)
```

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_271.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_273.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_275.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_277.png)

为了进行可靠的评估，我们需要将数据集划分为训练集和测试集。例如，我们可以将前80万条数据作为训练集，后20万条数据作为测试集。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_279.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_281.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_283.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_285.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_287.png)

```python
# 假设 data 是加载的全部数据
split_idx = 800000
train_data = data[:split_idx]
test_data = data[split_idx:]

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_289.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_291.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_293.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_295.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_296.png)

# 训练前，计算初始RMSE
init_train_preds = make_predictions(train_data)
init_test_preds = make_predictions(test_data)
print(f"初始训练集RMSE: {rmse(train_data, init_train_preds):.4f}")
print(f"初始测试集RMSE: {rmse(test_data, init_test_preds):.4f}")

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_298.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_300.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_302.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_304.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_306.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_308.png)

# 进行多轮训练
num_epochs = 10
for epoch in range(num_epochs):
    train_one_epoch(train_data)
    train_preds = make_predictions(train_data)
    test_preds = make_predictions(test_data)
    print(f"Epoch {epoch+1}: 训练集RMSE: {rmse(train_data, train_preds):.4f}, 测试集RMSE: {rmse(test_data, test_preds):.4f}")
```

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_310.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_312.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_314.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_316.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_318.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_320.png)

## 超参数调试与分析

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_322.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_324.png)

在运行上述代码后，我们可能会发现模型表现不佳或训练不稳定。这时就需要调整超参数。以下是一些关键的调试点：

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_326.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_328.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_329.png)

1.  **参数初始化**：如果初始RMSE很大（例如接近4），说明随机初始化的隐向量内积范围与真实评分（1-5）不匹配。可以尝试调整初始化尺度。
2.  **学习率（步长）**：步长 `step` 过大可能导致梯度更新震荡甚至数值溢出；过小则会导致收敛缓慢。需要尝试不同的值。
3.  **隐向量维度 K**：`K` 值增大模型容量，可能提高拟合能力但也更容易过拟合；`K` 值减小则相反。需要通过交叉验证选择。
4.  **正则化系数 reg**：`reg` 用于控制模型复杂度。将其设为0可以移除正则化，可能使训练集误差更小但测试集误差变大（过拟合）；增大 `reg` 可以抑制过拟合。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_331.png)

在实践中，MovieLens数据集质量较高，简单的FM模型也能取得不错的效果，可能不容易观察到明显的过拟合现象。但通过调整这些参数，你可以更深入地理解它们对模型行为的影响。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_333.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_335.png)

## 总结

本节课中我们一起学习了如何从零开始实现一个用于评分预测的因子分解机（FM）模型。我们从数据读取开始，定义了模型的参数和预测函数，推导了损失函数及其梯度，并实现了梯度下降训练流程。最后，我们建立了模型评估方法，并讨论了超参数调试的重要性。

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_337.png)

![](img/f009ba3df01ec62a4a8a0fa0f03876b0_339.png)

我们实现的这个简化版FM，实际上等价于经典的SVD矩阵分解模型。它揭示了如何通过用户和物品的隐向量交互来捕捉复杂的特征关系。作为扩展，你可以尝试为模型添加线性项和常数项，或者融入更多的用户和电影特征，使其成为一个更完整的FM模型。