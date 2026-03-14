# PyTorch 极简实战教程 P8：L8- 逻辑回归 🧠

在本节课中，我们将学习如何使用 PyTorch 实现逻辑回归模型。逻辑回归是一种用于解决二分类问题的经典算法。我们将遵循典型的 PyTorch 工作流程：准备数据、构建模型、定义损失函数与优化器，最后进行训练与评估。

---

## 数据准备 📊

![](img/d4aae67709503eb865c057216c0046f3_1.png)

上一节我们介绍了课程目标，本节中我们来看看如何为逻辑回归准备数据。我们将使用一个经典的二分类数据集——乳腺癌数据集，并进行必要的预处理。

![](img/d4aae67709503eb865c057216c0046f3_3.png)

首先，导入所需的库。

![](img/d4aae67709503eb865c057216c0046f3_5.png)

![](img/d4aae67709503eb865c057216c0046f3_6.png)

```python
import torch
import torch.nn as nn
import numpy as np
from sklearn import datasets
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
```

![](img/d4aae67709503eb865c057216c0046f3_8.png)

![](img/d4aae67709503eb865c057216c0046f3_10.png)

以下是数据加载与预处理的步骤：

![](img/d4aae67709503eb865c057216c0046f3_12.png)

1.  加载乳腺癌数据集。
2.  划分训练集和测试集。
3.  使用 `StandardScaler` 对特征进行标准化，使其均值为0，方差为1。
4.  将数据转换为 PyTorch 张量，并调整标签的形状。

```python
# 1. 加载数据
bc = datasets.load_breast_cancer()
X, y = bc.data, bc.target
n_samples, n_features = X.shape
print(f'样本数: {n_samples}, 特征数: {n_features}')

![](img/d4aae67709503eb865c057216c0046f3_14.png)

# 2. 划分数据集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=1234)

# 3. 特征标准化
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)

![](img/d4aae67709503eb865c057216c0046f3_16.png)

# 4. 转换为PyTorch张量并调整形状
X_train = torch.from_numpy(X_train.astype(np.float32))
X_test = torch.from_numpy(X_test.astype(np.float32))
y_train = torch.from_numpy(y_train.astype(np.float32))
y_test = torch.from_numpy(y_test.astype(np.float32))

![](img/d4aae67709503eb865c057216c0046f3_18.png)

![](img/d4aae67709503eb865c057216c0046f3_20.png)

y_train = y_train.view(y_train.shape[0], 1)
y_test = y_test.view(y_test.shape[0], 1)
```

![](img/d4aae67709503eb865c057216c0046f3_22.png)

---

![](img/d4aae67709503eb865c057216c0046f3_23.png)

![](img/d4aae67709503eb865c057216c0046f3_25.png)

## 构建模型 🏗️

数据准备完成后，接下来我们构建逻辑回归模型。逻辑回归模型本质上是线性层与 Sigmoid 激活函数的组合。

![](img/d4aae67709503eb865c057216c0046f3_27.png)

![](img/d4aae67709503eb865c057216c0046f3_29.png)

我们通过继承 `nn.Module` 类来定义自己的模型。

```python
class LogisticRegression(nn.Module):
    def __init__(self, n_input_features):
        super(LogisticRegression, self).__init__()
        self.linear = nn.Linear(n_input_features, 1)

    def forward(self, x):
        y_predicted = torch.sigmoid(self.linear(x))
        return y_predicted
```

![](img/d4aae67709503eb865c057216c0046f3_31.png)

![](img/d4aae67709503eb865c057216c0046f3_32.png)

![](img/d4aae67709503eb865c057216c0046f3_33.png)

模型创建如下，输入特征数为30。

![](img/d4aae67709503eb865c057216c0046f3_35.png)

```python
model = LogisticRegression(n_features)
```

![](img/d4aae67709503eb865c057216c0046f3_37.png)

![](img/d4aae67709503eb865c057216c0046f3_38.png)

---

![](img/d4aae67709503eb865c057216c0046f3_40.png)

## 定义损失函数与优化器 ⚙️

![](img/d4aae67709503eb865c057216c0046f3_41.png)

![](img/d4aae67709503eb865c057216c0046f3_43.png)

模型构建好后，需要定义衡量预测好坏的损失函数和用于更新模型参数的优化器。对于二分类问题，我们使用二元交叉熵损失（BCELoss）。

优化器我们选择随机梯度下降（SGD）。

![](img/d4aae67709503eb865c057216c0046f3_45.png)

![](img/d4aae67709503eb865c057216c0046f3_46.png)

```python
criterion = nn.BCELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)
```

![](img/d4aae67709503eb865c057216c0046f3_47.png)

---

## 训练循环 🔁

![](img/d4aae67709503eb865c057216c0046f3_49.png)

![](img/d4aae67709503eb865c057216c0046f3_50.png)

![](img/d4aae67709503eb865c057216c0046f3_51.png)

现在进入核心的训练环节。我们将进行多次迭代（epoch），每次迭代包含前向传播、损失计算、反向传播和参数更新四个步骤。

![](img/d4aae67709503eb865c057216c0046f3_52.png)

以下是训练循环的代码结构：

![](img/d4aae67709503eb865c057216c0046f3_54.png)

1.  前向传播计算预测值。
2.  计算预测值与真实标签之间的损失。
3.  反向传播计算梯度。
4.  优化器根据梯度更新模型参数。
5.  清空梯度，为下一次迭代做准备。

```python
num_epochs = 100

for epoch in range(num_epochs):
    # 前向传播与损失计算
    y_predicted = model(X_train)
    loss = criterion(y_predicted, y_train)

    # 反向传播
    loss.backward()

    # 参数更新
    optimizer.step()

    # 清空梯度
    optimizer.zero_grad()

    # 打印训练信息
    if (epoch+1) % 10 == 0:
        print(f'epoch: {epoch+1}, loss = {loss.item():.4f}')
```

![](img/d4aae67709503eb865c057216c0046f3_57.png)

![](img/d4aae67709503eb865c057216c0046f3_58.png)

---

![](img/d4aae67709503eb865c057216c0046f3_61.png)

## 模型评估 ✅

![](img/d4aae67709503eb865c057216c0046f3_64.png)

训练完成后，我们需要在测试集上评估模型的性能。评估时不需要计算梯度，因此使用 `torch.no_grad()` 上下文管理器。

评估指标我们使用准确率，即预测正确的样本占总样本的比例。

```python
with torch.no_grad():
    y_predicted = model(X_test)
    # 将Sigmoid输出转换为类别 (0 或 1)
    y_predicted_cls = y_predicted.round()
    acc = y_predicted_cls.eq(y_test).sum() / float(y_test.shape[0])
    print(f'准确率 = {acc:.4f}')
```

![](img/d4aae67709503eb865c057216c0046f3_67.png)

![](img/d4aae67709503eb865c057216c0046f3_70.png)

运行上述代码，模型在测试集上达到了约 0.89 的准确率。你可以通过调整迭代次数 (`num_epochs`)、学习率 (`lr`) 或尝试不同的优化器来进一步提升模型性能。

---

![](img/d4aae67709503eb865c057216c0046f3_74.png)

![](img/d4aae67709503eb865c057216c0046f3_75.png)

![](img/d4aae67709503eb865c057216c0046f3_76.png)

## 总结 📝

![](img/d4aae67709503eb865c057216c0046f3_78.png)

本节课中我们一起学习了如何使用 PyTorch 实现逻辑回归。我们完整地走过了机器学习项目的关键步骤：从数据加载与预处理，到自定义模型类，再到定义损失函数、优化器并进行训练循环，最后对模型性能进行评估。这个流程是构建更复杂神经网络模型的基础，希望你能熟练掌握。