# 055：PyTorch中的逻辑回归代码示例 📝

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_1.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_3.png)

在本节课中，我们将通过一个完整的代码示例，来总结之前四个视频中讨论的逻辑回归核心概念。我们将学习如何从零开始实现逻辑回归，以及如何使用PyTorch的高级API来实现相同的功能。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_5.png)

---

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_7.png)

## 概述

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_9.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_10.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_12.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_14.png)

逻辑回归是一种用于二分类问题的经典算法。其核心思想是通过一个线性模型结合Sigmoid激活函数，将输入映射到0到1之间的概率值。我们通过最小化损失函数（如负对数似然）来训练模型参数。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_16.png)

上一节我们详细推导了逻辑回归的梯度下降更新规则。本节中，我们来看看如何将这些数学公式转化为实际的PyTorch代码。

---

## 1. 从零开始实现逻辑回归

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_18.png)

为了深入理解逻辑回归的工作原理，我们首先手动实现其前向传播和反向传播过程。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_20.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_21.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_22.png)

### 1.1 模型定义

我们创建一个`LogisticRegression`类，它包含权重、偏置以及必要的方法。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_24.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_26.png)

```python
import torch
import numpy as np

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_28.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_29.png)

class LogisticRegression:
    def __init__(self, num_features):
        self.weights = torch.zeros(1, num_features)  # 初始化权重为零
        self.bias = torch.zeros(1)                   # 初始化偏置为零

    def forward(self, x):
        # 计算净输入 z = w*x + b
        z = torch.mm(x, self.weights.t()) + self.bias
        # 应用Sigmoid激活函数得到概率 a = σ(z)
        a = self._sigmoid(z)
        return a

    def backward(self, x, y, a):
        # 计算损失L关于输出a的梯度：dL/da = (a - y) / (a*(1-a))
        # 注意：结合Sigmoid导数后，关于净输入z的梯度简化为 (a - y)
        dloss_dz = a - y
        # 计算损失L关于权重w的梯度：dL/dw = (a - y) * x
        dloss_dw = torch.mm(x.t(), dloss_dz)
        # 计算损失L关于偏置b的梯度：dL/db = sum(a - y)
        dloss_db = torch.sum(dloss_dz, dim=0)
        return dloss_dw, dloss_db

    def predict(self, x):
        # 前向传播得到概率
        a = self.forward(x)
        # 根据阈值0.5进行类别预测
        predictions = (a > 0.5).float()
        return predictions

    def evaluate(self, x, y):
        predictions = self.predict(x)
        accuracy = torch.sum(predictions == y).float() / y.shape[0]
        return accuracy

    def _sigmoid(self, z):
        # Sigmoid函数：σ(z) = 1 / (1 + exp(-z))
        return 1. / (1. + torch.exp(-z))

    def _loss(self, y, a):
        # 负对数似然损失（二元交叉熵）
        # L = - [y*log(a) + (1-y)*log(1-a)]
        loss = -torch.sum(y * torch.log(a) + (1 - y) * torch.log(1 - a))
        return loss
```

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_31.png)

以下是关键步骤的说明：
*   `__init__`: 初始化模型参数。
*   `forward`: 执行前向传播，计算预测概率。
*   `backward`: 根据链式法则计算损失函数关于权重和偏置的梯度。
*   `predict` 和 `evaluate`: 用于模型预测和性能评估。
*   `_sigmoid` 和 `_loss`: 定义激活函数和损失函数。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_33.png)

### 1.2 训练循环

接下来，我们编写训练函数来迭代更新模型参数。

```python
def train(model, x_train, y_train, epochs, learning_rate):
    train_loss = []
    for epoch in range(epochs):
        # 1. 前向传播
        a = model.forward(x_train)
        # 2. 计算损失
        loss = model._loss(y_train, a)
        train_loss.append(loss.item())
        # 3. 反向传播计算梯度
        dloss_dw, dloss_db = model.backward(x_train, y_train, a)
        # 4. 梯度下降更新参数
        model.weights -= learning_rate * dloss_dw.t()
        model.bias -= learning_rate * dloss_db
        # 5. 记录训练精度
        acc = model.evaluate(x_train, y_train)
        if (epoch+1) % 10 == 0:
            print(f‘Epoch: {epoch+1:03d}/{epochs:03d} | Loss: {loss:.4f} | Acc: {acc:.2f}‘)
    return train_loss
```

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_35.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_37.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_39.png)

### 1.3 运行示例

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_41.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_43.png)

我们使用一个简单的二分类数据集进行训练和测试。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_45.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_47.png)

```python
# 准备数据（示例，需替换为实际数据）
# X_train, y_train, X_test, y_test = load_your_data()
X_train_t = torch.tensor(X_train, dtype=torch.float32)
y_train_t = torch.tensor(y_train, dtype=torch.float32).view(-1, 1)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_49.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_51.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_53.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_55.png)

# 初始化模型
num_features = X_train.shape[1]
model1 = LogisticRegression(num_features)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_57.png)

# 训练模型
epochs = 30
lr = 0.1
loss_history = train(model1, X_train_t, y_train_t, epochs, lr)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_59.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_60.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_62.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_64.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_66.png)

# 查看最终参数
print(‘Weights:‘, model1.weights)
print(‘Bias:‘, model1.bias)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_68.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_69.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_70.png)

# 在测试集上评估
X_test_t = torch.tensor(X_test, dtype=torch.float32)
y_test_t = torch.tensor(y_test, dtype=torch.float32).view(-1, 1)
test_acc = model1.evaluate(X_test_t, y_test_t)
print(f‘Test Accuracy: {test_acc:.2f}‘)
```

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_72.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_73.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_75.png)

通过从零实现，我们清晰地看到了梯度计算和参数更新的每一步。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_77.png)

---

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_79.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_80.png)

## 2. 使用PyTorch模块API实现逻辑回归

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_82.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_83.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_84.png)

在实践中，我们更常使用PyTorch提供的高级API，它们更简洁且自动处理梯度计算。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_86.png)

### 2.1 利用`nn.Module`定义模型

PyTorch的`torch.nn`模块提供了构建神经网络所需的所有基础组件。

```python
import torch.nn as nn

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_88.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_89.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_90.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_92.png)

class LogisticRegressionPyTorch(nn.Module):
    def __init__(self, num_features):
        super(LogisticRegressionPyTorch, self).__init__()
        # 定义一个线性层：y = xA^T + b
        self.linear = nn.Linear(num_features, 1)
        # 为了与手动实现对比，将权重和偏置初始化为零
        self.linear.weight.data.fill_(0.0)
        self.linear.bias.data.fill_(0.0)

    def forward(self, x):
        # 线性层输出后接Sigmoid函数
        logits = self.linear(x)
        probabilities = torch.sigmoid(logits)
        return probabilities
```

### 2.2 使用优化器和损失函数进行训练

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_94.png)

PyTorch的`torch.optim`模块提供了各种优化算法，`nn`模块中也包含了标准的损失函数。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_96.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_97.png)

```python
def train_pytorch(model, x_train, y_train, epochs, learning_rate):
    # 定义损失函数（二元交叉熵）
    # 设置 reduction=‘sum‘ 以与手动实现的损失求和方式保持一致
    loss_fn = nn.BCELoss(reduction=‘sum‘)
    # 定义优化器（随机梯度下降）
    optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)

    for epoch in range(epochs):
        # 前向传播
        probabilities = model(x_train)
        # 计算损失
        loss = loss_fn(probabilities, y_train)
        # 反向传播前清零梯度
        optimizer.zero_grad()
        # 反向传播计算梯度
        loss.backward()
        # 使用优化器更新参数
        optimizer.step()

        if (epoch+1) % 10 == 0:
            with torch.no_grad():
                predictions = (probabilities > 0.5).float()
                acc = (predictions == y_train).float().mean()
            print(f‘Epoch: {epoch+1:03d}/{epochs:03d} | Loss: {loss:.4f} | Acc: {acc:.2f}‘)
```

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_99.png)

### 2.3 运行PyTorch版本

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_101.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_103.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_105.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_107.png)

```python
# 初始化PyTorch模型
model2 = LogisticRegressionPyTorch(num_features)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_109.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_111.png)

# 训练模型
train_pytorch(model2, X_train_t, y_train_t, epochs, lr)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_113.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_115.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_117.png)

# 查看并比较参数
print(‘PyTorch Model Weights:‘, model2.linear.weight.data)
print(‘PyTorch Model Bias:‘, model2.linear.bias.data)
print(‘Manual Model Weights:‘, model1.weights)
print(‘Manual Model Bias:‘, model1.bias)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_118.png)

# 测试精度应该相同
with torch.no_grad():
    test_probs = model2(X_test_t)
    test_preds = (test_probs > 0.5).float()
    test_acc_pt = (test_preds == y_test_t).float().mean()
print(f‘PyTorch Test Accuracy: {test_acc_pt:.2f}‘)
```

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_120.png)

可以看到，两个模型学习到的权重和偏置是相同的，验证了我们手动实现的正确性。PyTorch的实现更加简洁高效。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_122.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_123.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_124.png)

---

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_126.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_127.png)

## 总结

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_129.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_131.png)

本节课中我们一起学习了逻辑回归在PyTorch中的两种实现方式：
1.  **从零实现**：我们手动编写了前向传播、Sigmoid激活、损失计算以及反向传播的梯度公式。这种方式有助于深刻理解算法底层原理。
2.  **使用PyTorch API实现**：我们利用`nn.Linear`、`nn.BCELoss`和`torch.optim.SGD`等高级模块，快速构建并训练了逻辑回归模型。这是实际项目中的推荐做法。

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_133.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_134.png)

![](img/54ee9d1a5027fa4bec5b5df80ebc8872_135.png)

两种方法最终得到了相同的模型参数，证明了我们对逻辑回归梯度下降过程的理解是正确的。通过本节的代码实践，你应该对逻辑回归的工作流程有了更直观的认识。下一节，我们将探讨如何将这些概念推广到多类别分类问题，即Softmax回归。