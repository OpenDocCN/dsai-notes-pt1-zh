# PyTorch 极简实战教程！P7：L7- 线性回归 📈

在本节课中，我们将要学习如何使用 PyTorch 实现一个完整的线性回归模型。我们将遵循典型的 PyTorch 工作流程，从数据准备、模型设计、损失函数与优化器定义，到最终训练循环的构建与结果可视化。

---

## 步骤 0：准备数据 📊

首先，我们需要生成用于回归分析的数据集。我们将使用 `sklearn` 生成一个简单的线性数据集，并将其转换为 PyTorch 能够处理的张量格式。

以下是数据准备的具体步骤：

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_1.png)

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_2.png)

1.  导入必要的库。
2.  使用 `sklearn.datasets.make_regression` 生成包含 100 个样本、1 个特征的数据集。
3.  将生成的 NumPy 数组转换为 `torch.Tensor`，并确保数据类型为 `float32`。
4.  调整标签 `y` 的形状，使其成为一个列向量。

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_3.png)

```python
import torch
import torch.nn as nn
import numpy as np
from sklearn import datasets
import matplotlib.pyplot as plt

# 生成回归数据集
x_numpy, y_numpy = datasets.make_regression(n_samples=100, n_features=1, noise=20, random_state=1)

# 转换为 PyTorch 张量
x = torch.from_numpy(x_numpy.astype(np.float32))
y = torch.from_numpy(y_numpy.astype(np.float32))
# 将 y 重塑为列向量 (n_samples, 1)
y = y.view(y.shape[0], 1)

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_5.png)

# 获取样本数和特征数
n_samples, n_features = x.shape
```

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_6.png)

---

## 步骤 1：设计模型 🏗️

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_8.png)

上一节我们准备好了数据，本节中我们来看看如何设计模型。对于线性回归，模型非常简单，就是一个线性层。

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_10.png)

我们可以直接使用 PyTorch 内置的 `nn.Linear` 模块来定义我们的模型。该模块需要指定输入特征的数量和输出特征的数量。

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_12.png)

```python
# 定义模型
input_size = n_features
output_size = 1
model = nn.Linear(input_size, output_size)
```

---

## 步骤 2：定义损失函数与优化器 ⚙️

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_14.png)

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_15.png)

模型设计完成后，我们需要定义衡量模型预测好坏的损失函数，以及用于更新模型参数的优化器。

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_16.png)

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_18.png)

在线性回归任务中，我们通常使用**均方误差（MSE）**作为损失函数。对于优化器，我们选择**随机梯度下降（SGD）**。

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_20.png)

```python
# 定义损失函数和优化器
criterion = nn.MSELoss()  # 均方误差损失
learning_rate = 0.01
optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)  # 随机梯度下降优化器
```

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_22.png)

---

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_23.png)

## 步骤 3：训练循环 🔄

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_25.png)

现在，我们进入核心的训练环节。训练循环将重复执行前向传播、计算损失、反向传播和参数更新这几个步骤。

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_27.png)

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_29.png)

以下是训练循环的详细步骤：

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_31.png)

1.  设置训练轮数（epochs）。
2.  在每一轮中：
    *   进行前向传播，得到预测值。
    *   计算预测值与真实值之间的损失。
    *   进行反向传播，计算梯度。
    *   使用优化器更新模型参数。
    *   清空优化器中的梯度，为下一轮计算做准备。
3.  定期打印训练过程中的损失值，以便监控。

```python
num_epochs = 100

for epoch in range(num_epochs):
    # 前向传播和损失计算
    y_predicted = model(x)
    loss = criterion(y_predicted, y)

    # 反向传播
    loss.backward()

    # 参数更新
    optimizer.step()

    # 清空梯度
    optimizer.zero_grad()

    # 打印信息
    if (epoch+1) % 10 == 0:
        print(f‘epoch: {epoch+1}, loss = {loss.item():.4f}’)
```

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_33.png)

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_34.png)

---

## 步骤 4：结果可视化 📉

训练完成后，我们可以将模型的最终预测结果与原始数据一起绘制出来，直观地查看模型的拟合效果。

我们需要将模型的预测结果从计算图中分离出来，并转换为 NumPy 数组，以便使用 Matplotlib 进行绘图。

```python
# 获取最终预测结果并绘图
predicted = model(x).detach().numpy()  # .detach() 用于从计算图中分离张量

plt.plot(x_numpy, y_numpy, ‘ro’, label=‘Original data’)
plt.plot(x_numpy, predicted, ‘b-’, label=‘Fitted line’)
plt.legend()
plt.show()
```

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_36.png)

运行上述代码后，你将看到一条蓝色的直线拟合了红色的原始数据点，这表明我们的线性回归模型成功地学习了数据中的线性关系。

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_38.png)

---

![](img/ad2afccfb99dbe5b3a127586f9ddc4a3_40.png)

本节课中我们一起学习了使用 PyTorch 实现线性回归的完整流程。我们回顾了从数据准备、模型构建、损失与优化器配置，到训练循环和结果可视化的每一步。通过这个简单的例子，你可以掌握 PyTorch 进行模型训练的基本模式，为学习更复杂的模型打下基础。