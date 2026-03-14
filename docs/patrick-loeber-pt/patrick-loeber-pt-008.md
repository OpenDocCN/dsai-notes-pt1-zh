# 008：逻辑回归实现 🧠

在本节课中，我们将学习如何使用PyTorch实现逻辑回归模型。逻辑回归是一种用于解决二分类问题的经典算法。我们将遵循典型的PyTorch工作流程：准备数据、定义模型、设置损失函数和优化器，最后进行训练和评估。

---

## 数据准备 📊

首先，我们需要导入必要的库并加载数据集。我们将使用Scikit-learn中的乳腺癌数据集，这是一个经典的二分类数据集。

```python
import torch
import torch.nn as nn
import numpy as np
from sklearn import datasets
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
```

以下是数据准备的具体步骤：

![](img/0afed1d607ee8c3661079d47e79c3f72_1.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_3.png)

1.  加载数据集并查看其形状。
2.  将数据集分割为训练集和测试集。
3.  使用`StandardScaler`对特征进行标准化处理，使其均值为0，方差为1。
4.  将NumPy数组转换为PyTorch张量，并调整标签的形状。

![](img/0afed1d607ee8c3661079d47e79c3f72_5.png)

```python
# 1. 加载数据
b = datasets.load_breast_cancer()
X, y = b.data, b.target
n_samples, n_features = X.shape
print(f'样本数: {n_samples}, 特征数: {n_features}')

![](img/0afed1d607ee8c3661079d47e79c3f72_7.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_9.png)

# 2. 分割数据
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=1234)

# 3. 标准化特征
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)

# 4. 转换为PyTorch张量并调整形状
X_train = torch.from_numpy(X_train.astype(np.float32))
X_test = torch.from_numpy(X_test.astype(np.float32))
y_train = torch.from_numpy(y_train.astype(np.float32))
y_test = torch.from_numpy(y_test.astype(np.float32))
y_train = y_train.view(y_train.shape[0], 1)
y_test = y_test.view(y_test.shape[0], 1)
```

![](img/0afed1d607ee8c3661079d47e79c3f72_11.png)

---

![](img/0afed1d607ee8c3661079d47e79c3f72_13.png)

## 定义模型 🏗️

![](img/0afed1d607ee8c3661079d47e79c3f72_15.png)

上一节我们准备好了数据，本节中我们来看看如何定义逻辑回归模型。逻辑回归模型由一个线性层和一个Sigmoid激活函数组成。

![](img/0afed1d607ee8c3661079d47e79c3f72_17.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_18.png)

我们通过继承`nn.Module`类来创建自定义模型。在`__init__`方法中初始化线性层，在`forward`方法中定义前向传播过程。

![](img/0afed1d607ee8c3661079d47e79c3f72_20.png)

```python
class LogisticRegression(nn.Module):
    def __init__(self, n_input_features):
        super(LogisticRegression, self).__init__()
        self.linear = nn.Linear(n_input_features, 1)

    def forward(self, x):
        y_predicted = torch.sigmoid(self.linear(x))
        return y_predicted
```

定义好模型类后，我们可以实例化它。输入特征的数量是30。

![](img/0afed1d607ee8c3661079d47e79c3f72_22.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_24.png)

```python
model = LogisticRegression(n_features)
```

![](img/0afed1d607ee8c3661079d47e79c3f72_26.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_27.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_28.png)

---

![](img/0afed1d607ee8c3661079d47e79c3f72_30.png)

## 设置损失函数与优化器 ⚙️

![](img/0afed1d607ee8c3661079d47e79c3f72_32.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_33.png)

模型定义完成后，我们需要设置训练所需的损失函数和优化器。对于二分类问题，我们使用二元交叉熵损失（`BCELoss`）。优化器我们选择随机梯度下降（`SGD`）。

![](img/0afed1d607ee8c3661079d47e79c3f72_35.png)

```python
# 损失函数
criterion = nn.BCELoss()

# 优化器
learning_rate = 0.01
optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)
```

![](img/0afed1d607ee8c3661079d47e79c3f72_37.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_38.png)

---

![](img/0afed1d607ee8c3661079d47e79c3f72_39.png)

## 训练循环 🔁

现在进入核心的训练环节。我们将进行多次迭代（epoch），在每次迭代中执行前向传播、计算损失、反向传播和参数更新。

![](img/0afed1d607ee8c3661079d47e79c3f72_41.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_42.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_43.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_44.png)

以下是训练循环的步骤：

1.  将训练数据输入模型，得到预测值。
2.  使用损失函数计算预测值与真实标签之间的误差。
3.  调用`loss.backward()`进行反向传播，计算梯度。
4.  调用`optimizer.step()`根据梯度更新模型参数。
5.  调用`optimizer.zero_grad()`清空梯度，为下一次迭代做准备。

![](img/0afed1d607ee8c3661079d47e79c3f72_46.png)

```python
num_epochs = 100

for epoch in range(num_epochs):
    # 前向传播与损失计算
    y_predicted = model(X_train)
    loss = criterion(y_predicted, y_train)

    # 反向传播与参数更新
    loss.backward()
    optimizer.step()
    optimizer.zero_grad()

    # 每10轮打印一次损失
    if (epoch+1) % 10 == 0:
        print(f'epoch: {epoch+1}, loss = {loss.item():.4f}')
```

---

![](img/0afed1d607ee8c3661079d47e79c3f72_48.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_49.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_50.png)

## 模型评估 ✅

![](img/0afed1d607ee8c3661079d47e79c3f72_52.png)

训练完成后，我们需要在测试集上评估模型的性能。评估时，我们使用`torch.no_grad()`上下文管理器来禁用梯度计算，以提高效率并防止不必要的内存占用。

![](img/0afed1d607ee8c3661079d47e79c3f72_54.png)

评估步骤如下：

1.  使用训练好的模型对测试集进行预测。
2.  将Sigmoid函数的输出（0到1之间的值）通过四舍五入转换为0或1的类别标签。
3.  将预测的类别与真实标签进行比较，计算准确率。

```python
with torch.no_grad():
    y_predicted = model(X_test)
    y_predicted_cls = y_predicted.round() # 将概率转换为类别 (0 或 1)
    acc = y_predicted_cls.eq(y_test).sum() / float(y_test.shape[0])
    print(f'准确率 = {acc:.4f}')
```

![](img/0afed1d607ee8c3661079d47e79c3f72_56.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_57.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_59.png)

运行上述代码，我们得到了大约89%的准确率。你可以通过调整迭代次数（`num_epochs`）、学习率（`learning_rate`）或尝试不同的优化器来进一步提高模型性能。

![](img/0afed1d607ee8c3661079d47e79c3f72_60.png)

---

![](img/0afed1d607ee8c3661079d47e79c3f72_62.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_63.png)

![](img/0afed1d607ee8c3661079d47e79c3f72_64.png)

## 总结 📝

![](img/0afed1d607ee8c3661079d47e79c3f72_66.png)

本节课中我们一起学习了如何使用PyTorch实现逻辑回归。我们回顾了完整的工作流程：从数据加载和预处理，到定义包含线性层和Sigmoid函数的模型，再到设置二元交叉熵损失和SGD优化器，最后完成了训练循环并在测试集上评估了模型准确率。这个流程是构建更复杂神经网络的基础。