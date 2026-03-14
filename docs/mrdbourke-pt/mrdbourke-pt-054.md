#  54：创建直线数据集 📈

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_0.png)

在本节课中，我们将学习如何创建一个简单的直线数据集，并调整我们之前构建的模型来拟合它。这是一种重要的故障排除方法：当模型在复杂问题上表现不佳时，先在一个已知能解决的简单问题上测试它。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_2.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_3.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_4.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_5.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_7.png)

## 概述

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_9.png)

在之前的视频中，我们尝试构建一个模型来区分蓝点和红点，但之前的努力都失败了。我们尝试了多种改进模型的方法，例如增加训练轮数、添加更多层、增加隐藏单元、更改激活函数和学习率等。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_11.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_13.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_14.png)

一种我喜欢的故障排除方法是，将大问题分解，先测试一个更小、更简单的问题。我们知道在之前的章节中，我们构建过一个可以拟合直线的模型。因此，我们将创建一个直线数据集，看看我们当前的模型是否能学到任何东西。如果连直线都拟合不了，那说明我们的模型架构或训练过程存在根本性问题。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_16.png)

## 创建直线数据集

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_18.png)

首先，我们创建一个简单的线性回归数据集。以下是创建数据集的步骤：

```python
import torch

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_20.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_22.png)

# 设置权重和偏置
weight = 0.7
bias = 0.3

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_24.png)

# 创建特征数据 X
start = 0
end = 1
step = 0.01
X_regression = torch.arange(start, end, step).unsqueeze(dim=1)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_26.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_27.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_28.png)

# 使用线性回归公式创建标签数据 y
y_regression = weight * X_regression + bias
```

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_30.png)

我们使用了线性回归公式 **`y = weight * X + bias`** 来生成数据，这是一个没有误差项 `epsilon` 的简化版本。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_31.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_33.png)

## 划分训练集和测试集

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_35.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_37.png)

创建数据集后，下一步是将其划分为训练集和测试集。这有助于我们评估模型在未见过的数据上的表现。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_39.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_40.png)

以下是划分数据集的代码：

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_42.png)

```python
# 设置训练集分割比例
train_split = int(0.8 * len(X_regression))

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_44.png)

# 创建训练集
X_train_regression = X_regression[:train_split]
y_train_regression = y_regression[:train_split]

# 创建测试集
X_test_regression = X_regression[train_split:]
y_test_regression = y_regression[train_split:]
```

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_46.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_47.png)

划分后，我们检查了数据集的形状，确认训练集有80个样本，测试集有20个样本。

## 可视化数据

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_49.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_50.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_51.png)

在机器学习中，可视化数据至关重要。我们使用之前创建的 `plot_predictions` 辅助函数来绘制数据。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_52.png)

```python
# 假设 plot_predictions 函数已从 helper_functions.py 导入
plot_predictions(train_data=X_train_regression,
                 train_labels=y_train_regression,
                 test_data=X_test_regression,
                 test_labels=y_test_regression)
```

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_54.png)

可视化帮助我们直观地理解数据分布，确保我们处理的是有意义的数据，而不是一堆无意义的数字。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_56.png)

## 调整模型以适应新数据

现在，我们需要调整之前为分类任务构建的 `model_1`，使其能够处理这个回归数据集。关键区别在于输入和输出的形状。

对于我们的直线数据集：
*   **输入特征**：每个样本只有一个特征值（X坐标）。
*   **输出**：我们预测一个连续的数值（Y坐标）。

而之前的 `model_1` 是为二维特征（用于区分蓝红点）设计的。因此，我们需要创建一个新模型，将其第一层的输入特征数从2改为1。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_58.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_59.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_61.png)

以下是使用 `nn.Sequential` 创建 `model_2` 的代码：

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_63.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_64.png)

```python
import torch.nn as nn

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_66.png)

# 创建模型架构，仅改变输入特征数
model_2 = nn.Sequential(
    nn.Linear(in_features=1, out_features=10),  # 输入层：1个特征 -> 10个隐藏单元
    nn.Linear(in_features=10, out_features=10), # 隐藏层：10个单元 -> 10个单元
    nn.Linear(in_features=10, out_features=1)   # 输出层：10个单元 -> 1个输出（回归值）
)
```

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_68.png)

## 设置损失函数和优化器

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_70.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_72.png)

由于任务从分类变为回归，我们需要相应地更改损失函数。对于回归问题，我们使用L1损失（平均绝对误差）。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_74.png)

```python
# 设置损失函数和优化器
loss_fn = nn.L1Loss()  # 回归任务使用 L1Loss
optimizer = torch.optim.SGD(params=model_2.parameters(), lr=0.01) # 使用随机梯度下降优化器
```

## 训练模型

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_76.png)

我们将模型和数据移动到目标设备（如GPU），然后进行训练循环。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_78.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_80.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_81.png)

以下是训练循环的核心代码：

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_82.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_84.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_86.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_88.png)

```python
# 设置训练轮数
epochs = 1000

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_90.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_92.png)

# 将数据移动到与模型相同的设备
X_train_regression = X_train_regression.to(device)
y_train_regression = y_train_regression.to(device)
X_test_regression = X_test_regression.to(device)
y_test_regression = y_test_regression.to(device)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_94.png)

for epoch in range(epochs):
    ### 训练
    model_2.train()
    # 1. 前向传播
    y_pred = model_2(X_train_regression)
    # 2. 计算损失
    loss = loss_fn(y_pred, y_train_regression)
    # 3. 优化器梯度清零
    optimizer.zero_grad()
    # 4. 反向传播
    loss.backward()
    # 5. 优化器更新参数
    optimizer.step()

    ### 测试
    model_2.eval()
    with torch.inference_mode():
        test_pred = model_2(X_test_regression)
        test_loss = loss_fn(test_pred, y_test_regression)

    # 每100轮打印一次损失
    if epoch % 100 == 0:
        print(f"Epoch: {epoch} | Train loss: {loss:.5f} | Test loss: {test_loss:.5f}")
```

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_96.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_97.png)

运行训练后，我们观察到训练损失和测试损失都在下降，这表明模型正在学习！

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_99.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_101.png)

## 总结

本节课中，我们一起学习了如何通过创建一个简单的直线数据集来进行模型故障排除。我们回顾了数据创建、划分、可视化的流程，并调整了模型架构、损失函数以适应回归任务。最后，我们成功训练了模型，并观察到损失下降，证明模型能够学习这个简单问题的模式。

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_103.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_104.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_105.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_106.png)

![](img/4c823f6e9e9ecb4c96ca9f0d86a04e1b_107.png)

这是一种强大的策略：当面对复杂问题时，先在一个你能掌控的简单问题上验证你的模型和流程。在下一节课中，我们将使用训练好的模型进行预测，并可视化结果，以进一步确认模型的有效性。