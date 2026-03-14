# PyTorch 极简实战教程！P13：L13- 前馈神经网络 🧠

在本节课中，我们将实现第一个多层神经网络，用于基于著名的MNIST数据集进行手写数字分类。我们将把之前学到的所有知识结合起来，包括使用数据加载器、应用变换、构建网络结构、设置损失函数与优化器，并实现一个完整的训练与评估循环。

---

## 1. 导入库与设备配置 🛠️

首先，我们需要导入必要的库，并配置代码以支持GPU运行。

```python
import torch
import torch.nn as nn
import torchvision.datasets as datasets
import torchvision.transforms as transforms
import matplotlib.pyplot as plt
```

接下来，我们进行设备配置。这段代码会检查是否有可用的GPU，并据此设置运行设备。

![](img/ee06be2433f88c7e880533bc41c954a0_1.png)

![](img/ee06be2433f88c7e880533bc41c954a0_3.png)

```python
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
```

![](img/ee06be2433f88c7e880533bc41c954a0_5.png)

![](img/ee06be2433f88c7e880533bc41c954a0_7.png)

---

![](img/ee06be2433f88c7e880533bc41c954a0_9.png)

## 2. 定义超参数 ⚙️

![](img/ee06be2433f88c7e880533bc41c954a0_11.png)

在开始构建模型之前，我们需要定义一些超参数。这些参数决定了模型的结构和训练过程。

![](img/ee06be2433f88c7e880533bc41c954a0_13.png)

*   **输入大小**：MNIST图像尺寸为28x28，展平后为784。
*   **隐藏层大小**：我们设置为100个神经元。
*   **类别数量**：数字0-9，共10类。
*   **训练轮数**：设置为2，以便快速演示。
*   **批量大小**：设置为100。
*   **学习率**：设置为0.001。

![](img/ee06be2433f88c7e880533bc41c954a0_14.png)

![](img/ee06be2433f88c7e880533bc41c954a0_15.png)

```python
input_size = 784
hidden_size = 100
num_classes = 10
num_epochs = 2
batch_size = 100
learning_rate = 0.001
```

![](img/ee06be2433f88c7e880533bc41c954a0_17.png)

---

## 3. 加载与准备数据 📦

![](img/ee06be2433f88c7e880533bc41c954a0_19.png)

![](img/ee06be2433f88c7e880533bc41c954a0_21.png)

上一节我们定义了超参数，本节中我们来看看如何加载和准备MNIST数据集。

首先，我们下载数据集并应用变换，将图像转换为PyTorch张量。

```python
train_dataset = datasets.MNIST(root='./data', train=True, transform=transforms.ToTensor(), download=True)
test_dataset = datasets.MNIST(root='./data', train=False, transform=transforms.ToTensor())
```

接着，我们创建数据加载器，用于在训练和测试时按批次提供数据。

![](img/ee06be2433f88c7e880533bc41c954a0_23.png)

![](img/ee06be2433f88c7e880533bc41c954a0_24.png)

```python
train_loader = torch.utils.data.DataLoader(dataset=train_dataset, batch_size=batch_size, shuffle=True)
test_loader = torch.utils.data.DataLoader(dataset=test_dataset, batch_size=batch_size, shuffle=False)
```

![](img/ee06be2433f88c7e880533bc41c954a0_25.png)

![](img/ee06be2433f88c7e880533bc41c954a0_26.png)

![](img/ee06be2433f88c7e880533bc41c954a0_28.png)

为了直观了解数据，我们可以查看一个批次的样本。

![](img/ee06be2433f88c7e880533bc41c954a0_30.png)

```python
examples = iter(train_loader)
samples, labels = examples.next()
print(samples.shape, labels.shape) # 输出: torch.Size([100, 1, 28, 28]) torch.Size([100])
```

![](img/ee06be2433f88c7e880533bc41c954a0_31.png)

---

## 4. 构建神经网络模型 🏗️

![](img/ee06be2433f88c7e880533bc41c954a0_33.png)

数据准备就绪后，现在我们来构建核心的前馈神经网络模型。

![](img/ee06be2433f88c7e880533bc41c954a0_35.png)

我们的网络结构包含一个输入层、一个带有ReLU激活函数的隐藏层，以及一个输出层。

![](img/ee06be2433f88c7e880533bc41c954a0_37.png)

```python
class NeuralNet(nn.Module):
    def __init__(self, input_size, hidden_size, num_classes):
        super(NeuralNet, self).__init__()
        self.l1 = nn.Linear(input_size, hidden_size)
        self.relu = nn.ReLU()
        self.l2 = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        out = self.l1(x)
        out = self.relu(out)
        out = self.l2(out)
        return out

![](img/ee06be2433f88c7e880533bc41c954a0_39.png)

model = NeuralNet(input_size, hidden_size, num_classes).to(device)
```

![](img/ee06be2433f88c7e880533bc41c954a0_40.png)

**注意**：在输出层我们没有使用Softmax激活函数，因为后续使用的交叉熵损失函数会内部处理它。

---

![](img/ee06be2433f88c7e880533bc41c954a0_42.png)

## 5. 定义损失函数与优化器 🎯

![](img/ee06be2433f88c7e880533bc41c954a0_43.png)

模型构建完成后，我们需要定义如何衡量模型的错误（损失函数）以及如何根据错误来更新模型参数（优化器）。

![](img/ee06be2433f88c7e880533bc41c954a0_45.png)

![](img/ee06be2433f88c7e880533bc41c954a0_47.png)

我们使用交叉熵损失函数，它适用于多分类问题。优化器选择Adam，它是一种高效且常用的优化算法。

```python
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=learning_rate)
```

![](img/ee06be2433f88c7e880533bc41c954a0_49.png)

---

## 6. 训练循环 🔄

现在进入核心环节：训练循环。我们将遍历数据多个轮次，不断优化模型参数。

![](img/ee06be2433f88c7e880533bc41c954a0_51.png)

以下是训练循环的步骤：
1.  将数据转移到指定设备（CPU/GPU）。
2.  前向传播：计算模型预测。
3.  计算损失：比较预测与真实标签。
4.  反向传播：计算梯度。
5.  优化器步骤：更新模型参数。

![](img/ee06be2433f88c7e880533bc41c954a0_52.png)

```python
n_total_steps = len(train_loader)

for epoch in range(num_epochs):
    for i, (images, labels) in enumerate(train_loader):
        # 1. 将数据转移到设备并调整形状
        images = images.reshape(-1, 28*28).to(device)
        labels = labels.to(device)

        # 2. 前向传播
        outputs = model(images)
        loss = criterion(outputs, labels)

        # 3. 反向传播与优化
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

        # 打印训练信息
        if (i+1) % 100 == 0:
            print(f'Epoch [{epoch+1}/{num_epochs}], Step [{i+1}/{n_total_steps}], Loss: {loss.item():.4f}')
```

---

## 7. 模型测试与评估 📊

训练完成后，我们需要在未见过的测试集上评估模型的性能，计算其分类准确率。

在评估时，我们使用`torch.no_grad()`上下文管理器来禁用梯度计算，以节省内存和计算资源。

```python
with torch.no_grad():
    n_correct = 0
    n_samples = 0
    for images, labels in test_loader:
        images = images.reshape(-1, 28*28).to(device)
        labels = labels.to(device)
        outputs = model(images)

        # torch.max返回（最大值，最大值索引），我们取索引作为预测类别
        _, predictions = torch.max(outputs, 1)
        n_samples += labels.shape[0]
        n_correct += (predictions == labels).sum().item()

    acc = 100.0 * n_correct / n_samples
    print(f'模型在测试集上的准确率: {acc} %')
```

---

## 总结 📝

![](img/ee06be2433f88c7e880533bc41c954a0_54.png)

本节课中我们一起学习了如何用PyTorch实现一个完整的前馈神经网络项目。我们从配置环境、定义超参数开始，逐步完成了数据加载、模型构建、训练循环和最终评估。你学会了如何将图像数据展平输入全连接层，如何使用ReLU激活函数，以及如何利用交叉熵损失和Adam优化器来训练一个多分类模型。最终，我们成功训练了一个在MNIST数据集上准确率约95%的简单神经网络。