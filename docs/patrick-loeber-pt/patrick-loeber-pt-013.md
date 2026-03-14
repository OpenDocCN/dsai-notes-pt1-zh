# 013：前馈神经网络实现手写数字分类 🧠

在本节课中，我们将综合运用之前学到的知识，实现一个多层前馈神经网络，用于对著名的MNIST手写数字数据集进行分类。我们将使用数据加载器加载数据、应用变换、构建包含输入层、隐藏层和输出层的神经网络、应用激活函数、设置损失函数和优化器，并实现一个支持批量训练的训练循环。最后，我们将评估模型并计算准确率。此外，我们还会确保代码在有GPU支持的情况下也能在GPU上运行。

## 导入必要的库

首先，我们需要导入所有必需的库。

```python
import torch
import torch.nn as nn
import torchvision.datasets as datasets
import torchvision.transforms as transforms
import matplotlib.pyplot as plt
```

## 设备配置

为了确保代码能在GPU上运行（如果可用），我们需要进行设备配置。

![](img/4f9b5a50cb66458551d079eafb4a5a3c_1.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_3.png)

```python
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
```

![](img/4f9b5a50cb66458551d079eafb4a5a3c_5.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_7.png)

## 定义超参数

接下来，我们定义一些超参数，这些参数将决定模型的结构和训练过程。

![](img/4f9b5a50cb66458551d079eafb4a5a3c_9.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_10.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_11.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_13.png)

```python
input_size = 784    # 28x28 像素的图像被展平为784维向量
hidden_size = 100   # 隐藏层神经元数量
num_classes = 10    # 数字0-9，共10个类别
num_epochs = 2      # 训练轮数
batch_size = 100    # 批量大小
learning_rate = 0.001 # 学习率
```

## 加载并准备MNIST数据集

![](img/4f9b5a50cb66458551d079eafb4a5a3c_15.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_17.png)

我们将使用PyTorch内置的`torchvision.datasets`来下载和加载MNIST数据集，并使用`DataLoader`来创建可迭代的数据加载器。

```python
# 加载训练数据集
train_dataset = datasets.MNIST(root='./data',
                               train=True,
                               transform=transforms.ToTensor(),
                               download=True)

# 加载测试数据集
test_dataset = datasets.MNIST(root='./data',
                              train=False,
                              transform=transforms.ToTensor())

![](img/4f9b5a50cb66458551d079eafb4a5a3c_19.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_20.png)

# 创建数据加载器
train_loader = torch.utils.data.DataLoader(dataset=train_dataset,
                                           batch_size=batch_size,
                                           shuffle=True)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_22.png)

test_loader = torch.utils.data.DataLoader(dataset=test_dataset,
                                          batch_size=batch_size,
                                          shuffle=False)
```

![](img/4f9b5a50cb66458551d079eafb4a5a3c_24.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_25.png)

## 查看数据样本

在开始构建模型之前，让我们先查看一下数据的样子，以确认数据加载正确。

![](img/4f9b5a50cb66458551d079eafb4a5a3c_27.png)

```python
# 获取一个批次的样本
examples = iter(train_loader)
samples, labels = examples.next()

![](img/4f9b5a50cb66458551d079eafb4a5a3c_29.png)

# 打印样本和标签的形状
print(samples.shape)  # 应为 [100, 1, 28, 28]
print(labels.shape)   # 应为 [100]

# 可视化前6个样本
for i in range(6):
    plt.subplot(2, 3, i+1)
    plt.imshow(samples[i][0], cmap='gray')
plt.show()
```

![](img/4f9b5a50cb66458551d079eafb4a5a3c_31.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_32.png)

## 构建前馈神经网络模型

现在，我们来构建一个包含一个隐藏层的全连接神经网络。我们将创建一个继承自`nn.Module`的类。

![](img/4f9b5a50cb66458551d079eafb4a5a3c_34.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_35.png)

```python
class NeuralNet(nn.Module):
    def __init__(self, input_size, hidden_size, num_classes):
        super(NeuralNet, self).__init__()
        # 定义第一个线性层（输入层 -> 隐藏层）
        self.l1 = nn.Linear(input_size, hidden_size)
        # 定义ReLU激活函数
        self.relu = nn.ReLU()
        # 定义第二个线性层（隐藏层 -> 输出层）
        self.l2 = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        # 前向传播过程
        out = self.l1(x)
        out = self.relu(out)
        out = self.l2(out)
        # 注意：这里不应用Softmax，因为交叉熵损失函数会内部处理
        return out

![](img/4f9b5a50cb66458551d079eafb4a5a3c_37.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_39.png)

# 实例化模型
model = NeuralNet(input_size, hidden_size, num_classes).to(device)
```

## 定义损失函数和优化器

我们需要一个损失函数来衡量模型的预测误差，以及一个优化器来根据误差更新模型的参数。

```python
# 交叉熵损失函数（适用于多分类问题）
criterion = nn.CrossEntropyLoss()
# Adam优化器
optimizer = torch.optim.Adam(model.parameters(), lr=learning_rate)
```

## 训练模型

![](img/4f9b5a50cb66458551d079eafb4a5a3c_41.png)

![](img/4f9b5a50cb66458551d079eafb4a5a3c_42.png)

训练循环是模型学习的核心。我们将遍历多个轮次（epochs），在每个轮次中遍历所有批次（batches）的数据，进行前向传播、计算损失、反向传播和参数更新。

```python
# 总步数（批次数）
n_total_steps = len(train_loader)

for epoch in range(num_epochs):
    for i, (images, labels) in enumerate(train_loader):
        # 将数据移动到设备（CPU或GPU）
        images = images.reshape(-1, 28*28).to(device)
        labels = labels.to(device)

        # 前向传播
        outputs = model(images)
        loss = criterion(outputs, labels)

        # 反向传播和优化
        optimizer.zero_grad()  # 清空梯度
        loss.backward()        # 计算梯度
        optimizer.step()       # 更新参数

        # 每100步打印一次损失
        if (i+1) % 100 == 0:
            print(f'Epoch [{epoch+1}/{num_epochs}], Step [{i+1}/{n_total_steps}], Loss: {loss.item():.4f}')
```

## 评估模型

训练完成后，我们需要在测试集上评估模型的性能，计算其准确率。

```python
# 在评估模式下，不计算梯度
with torch.no_grad():
    n_correct = 0
    n_samples = 0
    for images, labels in test_loader:
        images = images.reshape(-1, 28*28).to(device)
        labels = labels.to(device)
        outputs = model(images)

        # 获取预测值（最大概率的索引）
        _, predictions = torch.max(outputs, 1)
        n_samples += labels.shape[0]
        n_correct += (predictions == labels).sum().item()

    # 计算准确率
    acc = 100.0 * n_correct / n_samples
    print(f'Accuracy of the network on the 10000 test images: {acc} %')
```

## 总结

在本节课中，我们一起完成了一个完整的前馈神经网络项目，用于手写数字分类。我们学习了如何：

1.  **配置设备**，使代码能兼容CPU和GPU。
2.  **定义超参数**，如网络结构、训练轮数和学习率。
3.  **加载和预处理**MNIST数据集，并使用`DataLoader`进行批量处理。
4.  **构建神经网络模型**，包括线性层和激活函数。
5.  **定义损失函数和优化器**，以指导模型的学习过程。
6.  **实现训练循环**，包括前向传播、损失计算、反向传播和参数更新。
7.  **评估模型性能**，在测试集上计算分类准确率。

![](img/4f9b5a50cb66458551d079eafb4a5a3c_44.png)

通过这个实践，你将前几节课的知识点串联了起来，实现了一个能够实际工作的机器学习模型。你可以尝试调整超参数（如隐藏层大小、学习率、训练轮数），观察它们对模型性能的影响。