# 108：PyTorch中的LeNet-5实现 🧠

在本节课中，我们将学习如何在PyTorch中实现经典的LeNet-5卷积神经网络，并将其应用于MNIST手写数字分类任务。我们将详细解析网络结构、数据预处理、模型定义以及训练过程的每一部分。

## 概述

![](img/e56dd1c8776a1e4867060fd3484f8f26_1.png)

LeNet-5是一个经典的卷积神经网络架构，最初设计用于手写字符识别。本节教程将指导你使用PyTorch框架，从零开始构建并训练一个LeNet-5模型来对MNIST数据集进行分类。我们将重点关注网络层的构建、数据流的处理以及训练循环的实现。

## 网络架构回顾

在深入代码之前，我们先回顾一下LeNet-5的标准架构。原论文中的输入图像尺寸为32x32，而MNIST数据集图像为28x28，因此我们需要对输入进行尺寸调整。

以下是LeNet-5的典型结构流程：
*   **输入**： 32x32 图像
*   **C1层**： 卷积层 (1个输入通道 -> 6个输出通道， 5x5卷积核) -> Tanh激活 -> 池化层 (2x2)
*   **C2层**： 卷积层 (6个输入通道 -> 16个输出通道， 5x5卷积核) -> Tanh激活 -> 池化层 (2x2)
*   **全连接层**： 展平特征图后，依次经过维度为120、84和10（对应10个数字类别）的全连接层。

卷积层通常用于**增加通道数**并保持空间尺寸大致不变（若使用填充）。池化层则用于**缩减空间尺寸（高度和宽度）**，同时保持通道数不变。网络的前半部分（卷积和池化）可视为**特征提取器**，后半部分（全连接层）则充当**分类器**。

## 代码实现详解

现在，让我们逐步分析在PyTorch中实现LeNet-5的代码。

![](img/e56dd1c8776a1e4867060fd3484f8f26_3.png)

### 1. 环境设置与数据准备

![](img/e56dd1c8776a1e4867060fd3484f8f26_5.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_6.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_7.png)

首先，我们导入必要的库并设置环境。为了确保结果的可复现性，我们固定了随机种子。请注意，某些GPU算法具有非确定性，如果遇到错误，可以注释掉相关设置种子代码。

![](img/e56dd1c8776a1e4867060fd3484f8f26_9.png)

```python
import torch
import torch.nn as nn
import torch.optim as optim
from torchvision import datasets, transforms

# 设置随机种子以保证可复现性（在GPU上可能无效）
torch.manual_seed(1)
```

接下来，我们准备MNIST数据集。关键步骤包括：
*   将图像尺寸从28x28**调整**为32x32以匹配LeNet-5的原始输入。
*   对像素值进行**归一化**，使其均值为0，范围在[-1, 1]之间。

```python
# 定义数据转换
transform = transforms.Compose([
    transforms.Resize((32, 32)),      # 调整尺寸
    transforms.ToTensor(),            # 转换为张量
    transforms.Normalize((0.5,), (0.5,)) # 归一化到[-1, 1]
])

![](img/e56dd1c8776a1e4867060fd3484f8f26_11.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_12.png)

# 加载训练和测试数据集
train_dataset = datasets.MNIST(root='./data', train=True, download=True, transform=transform)
test_dataset = datasets.MNIST(root='./data', train=False, download=True, transform=transform)

# 创建数据加载器
train_loader = torch.utils.data.DataLoader(train_dataset, batch_size=64, shuffle=True)
test_loader = torch.utils.data.DataLoader(test_dataset, batch_size=1000, shuffle=False)
```

![](img/e56dd1c8776a1e4867060fd3484f8f26_14.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_16.png)

### 2. 定义LeNet-5模型

我们使用PyTorch的`nn.Module`类来定义网络。为了使模型更通用，我们添加了`grayscale`参数，以便它能处理单通道（如MNIST）或三通道（如CIFAR-10）的输入。

以下是模型定义的核心部分：

![](img/e56dd1c8776a1e4867060fd3484f8f26_18.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_20.png)

```python
class LeNet5(nn.Module):
    def __init__(self, grayscale=True, num_classes=10):
        super(LeNet5, self).__init__()
        self.grayscale = grayscale
        self.num_classes = num_classes

        # 确定输入通道数
        in_channels = 1 if grayscale else 3

        # 特征提取器（卷积部分）
        self.features = nn.Sequential(
            # 第一个卷积块：C1层
            nn.Conv2d(in_channels, 6, kernel_size=5), # 公式: 输出尺寸 = (输入尺寸 - 核尺寸 + 1)
            nn.Tanh(),
            nn.AvgPool2d(kernel_size=2), # 公式: 输出尺寸 = 输入尺寸 / 步长（默认为核尺寸）

            # 第二个卷积块：C2层
            nn.Conv2d(6, 16, kernel_size=5),
            nn.Tanh(),
            nn.AvgPool2d(kernel_size=2),
        )

        # 分类器（全连接部分）
        self.classifier = nn.Sequential(
            nn.Linear(16 * 5 * 5, 120), # 第一个全连接层：输入维度需根据特征图尺寸计算
            nn.Tanh(),
            nn.Linear(120, 84),         # 第二个全连接层
            nn.Tanh(),
            nn.Linear(84, num_classes)  # 输出层，对应类别数
        )

    def forward(self, x):
        # 通过特征提取器
        x = self.features(x)
        # 展平特征图：将 (N, C, H, W) 转换为 (N, C*H*W)
        x = torch.flatten(x, 1)
        # 通过分类器
        x = self.classifier(x)
        return x
```

**关键点解析**：
*   `self.features`： 这是一个`nn.Sequential`容器，按顺序包含了两个“卷积块”。每个块由卷积层、激活函数（Tanh）和池化层组成，共同完成特征提取。
*   `self.classifier`： 这是另一个`nn.Sequential`容器，包含三个全连接层，充当分类器。
*   **展平操作**： 在`forward`函数中，卷积层输出的特征图是四维张量（批量大小, 通道数, 高度, 宽度）。全连接层需要二维输入（批量大小, 特征数），因此我们使用`torch.flatten(x, 1)`将第1维之后的所有维度展平。
*   **输入维度计算**： 第一个全连接层`nn.Linear(16 * 5 * 5, 120)`的输入维度`16*5*5`需要手动计算。这源于最后一个池化层输出的特征图尺寸：通道数=16，高度=5，宽度=5。如果不确定，可以在`forward`函数中先打印`x`的形状来确定。

![](img/e56dd1c8776a1e4867060fd3484f8f26_22.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_23.png)

### 3. 模型初始化与设备配置

创建模型实例，并根据可用硬件将其移动到GPU或CPU上。

```python
# 初始化模型
model = LeNet5(grayscale=True, num_classes=10)

# 检查是否有可用的GPU，并移动模型
device = torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')
model.to(device)
print(f‘Using device: {device}’)
```

### 4. 设置训练组件

![](img/e56dd1c8776a1e4867060fd3484f8f26_25.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_27.png)

我们使用交叉熵损失函数，它适用于多分类任务。优化器选择带动量的随机梯度下降（SGD），并搭配学习率调度器，以便在训练过程中动态降低学习率。

![](img/e56dd1c8776a1e4867060fd3484f8f26_29.png)

```python
# 定义损失函数
criterion = nn.CrossEntropyLoss()

![](img/e56dd1c8776a1e4867060fd3484f8f26_31.png)

# 定义优化器（带动量的SGD）
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.9)

# 定义学习率调度器（每5个epoch将学习率乘以0.1）
scheduler = optim.lr_scheduler.StepLR(optimizer, step_size=5, gamma=0.1)
```

![](img/e56dd1c8776a1e4867060fd3484f8f26_33.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_34.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_35.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_36.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_37.png)

### 5. 训练循环

![](img/e56dd1c8776a1e4867060fd3484f8f26_39.png)

训练循环是模型学习的核心。我们迭代多个轮次（epoch），在每个轮次中遍历所有训练数据的小批量（mini-batch），执行前向传播、计算损失、反向传播和参数更新。

以下是训练循环的简化逻辑：

![](img/e56dd1c8776a1e4867060fd3484f8f26_41.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_42.png)

```python
def train_model(model, train_loader, criterion, optimizer, scheduler, num_epochs=10):
    model.train()  # 将模型设置为训练模式
    for epoch in range(num_epochs):
        running_loss = 0.0
        correct = 0
        total = 0

        for inputs, labels in train_loader:
            # 将数据移动到指定设备
            inputs, labels = inputs.to(device), labels.to(device)

            # 清零梯度
            optimizer.zero_grad()

            # 前向传播
            outputs = model(inputs)
            loss = criterion(outputs, labels)

            # 反向传播和优化
            loss.backward()
            optimizer.step()

            # 统计信息
            running_loss += loss.item()
            _, predicted = outputs.max(1)
            total += labels.size(0)
            correct += predicted.eq(labels).sum().item()

        # 更新学习率
        scheduler.step()

        # 打印每个epoch的统计信息
        epoch_loss = running_loss / len(train_loader)
        epoch_acc = 100. * correct / total
        print(f‘Epoch [{epoch+1}/{num_epochs}], Loss: {epoch_loss:.4f}, Accuracy: {epoch_acc:.2f}%’)
```

![](img/e56dd1c8776a1e4867060fd3484f8f26_44.png)

### 6. 模型评估与预测

![](img/e56dd1c8776a1e4867060fd3484f8f26_45.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_47.png)

训练完成后，我们可以在测试集上评估模型性能，并可视化一些预测结果以进行定性检查。

```python
# 评估模型
model.eval()  # 将模型设置为评估模式
correct = 0
total = 0
with torch.no_grad():  # 评估时不计算梯度
    for inputs, labels in test_loader:
        inputs, labels = inputs.to(device), labels.to(device)
        outputs = model(inputs)
        _, predicted = outputs.max(1)
        total += labels.size(0)
        correct += predicted.eq(labels).sum().item()

![](img/e56dd1c8776a1e4867060fd3484f8f26_49.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_51.png)

test_acc = 100. * correct / total
print(f‘Test Accuracy: {test_acc:.2f}%’)

![](img/e56dd1c8776a1e4867060fd3484f8f26_53.png)

# 显示一些预测示例
def show_examples(model, data_loader, num_examples=10):
    model.eval()
    data_iter = iter(data_loader)
    images, labels = next(data_iter)
    images, labels = images.to(device), labels.to(device)

    with torch.no_grad():
        outputs = model(images[:num_examples])
        _, preds = outputs.max(1)

    for i in range(num_examples):
        print(f‘Predicted: {preds[i].item()}, True: {labels[i].item()}’)

![](img/e56dd1c8776a1e4867060fd3484f8f26_55.png)

show_examples(model, test_loader)
```

![](img/e56dd1c8776a1e4867060fd3484f8f26_57.png)

![](img/e56dd1c8776a1e4867060fd3484f8f26_59.png)

## 总结

![](img/e56dd1c8776a1e4867060fd3484f8f26_61.png)

在本节课中，我们一起学习了如何在PyTorch中实现并训练LeNet-5卷积神经网络。我们涵盖了以下核心内容：

1.  **网络结构理解**： 分析了LeNet-5由特征提取器（卷积层、池化层）和分类器（全连接层）组成的设计模式。
2.  **数据预处理**： 学会了调整输入图像尺寸并进行归一化，以满足模型输入要求。
3.  **模型构建**： 使用`nn.Sequential`和`nn.Module`模块化地定义了网络，并理解了展平操作在连接卷积层与全连接层时的必要性。
4.  **训练流程**： 设置了损失函数、优化器和学习率调度器，并实现了完整的训练循环，包括前向传播、损失计算、反向传播和参数更新。
5.  **评估与验证**： 在测试集上评估了模型性能，并查看了具体预测结果。

![](img/e56dd1c8776a1e4867060fd3484f8f26_63.png)

通过这个实践，你不仅掌握了LeNet-5的实现，也熟悉了使用PyTorch构建和训练卷积神经网络的标准流程。在接下来的课程中，我们将探索更复杂的网络架构，如AlexNet。