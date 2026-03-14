# 119：PyTorch中的ResNet-34代码示例 🧠💻

![](img/04f4e9ff792401422c961a2d284ec86d_1.png)

![](img/04f4e9ff792401422c961a2d284ec86d_2.png)

在本节课中，我们将学习如何在PyTorch中实现残差网络。我们将从两个角度进行探讨：首先，我会展示一个我自己编写的、用于理解原理的简单实现；然后，我们将查看并分析PyTorch社区提供的、更成熟的ResNet-34实现代码。

![](img/04f4e9ff792401422c961a2d284ec86d_4.png)

## 概述
残差网络通过引入“跳跃连接”解决了深度神经网络中的梯度消失和网络退化问题。本节将通过代码实践，帮助你理解残差块的结构以及如何构建一个深层残差网络。

![](img/04f4e9ff792401422c961a2d284ec86d_6.png)

![](img/04f4e9ff792401422c961a2d284ec86d_8.png)

---

![](img/04f4e9ff792401422c961a2d284ec86d_10.png)

![](img/04f4e9ff792401422c961a2d284ec86d_11.png)

## 1. 简单残差网络实现

上一节我们介绍了残差网络的基本概念，本节中我们来看看如何用代码实现一个基础的残差块。这个实现侧重于清晰展示原理，因此结构较为简单。

首先，我们导入必要的库并准备数据。为了专注于网络结构本身，这里使用MNIST数据集，因为它简单且易于加载。

![](img/04f4e9ff792401422c961a2d284ec86d_13.png)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
```

![](img/04f4e9ff792401422c961a2d284ec86d_15.png)

![](img/04f4e9ff792401422c961a2d284ec86d_16.png)

以下是实现一个基础残差块的代码。这个块对应输入与残差部分输出维度相同的情况。

![](img/04f4e9ff792401422c961a2d284ec86d_18.png)

![](img/04f4e9ff792401422c961a2d284ec86d_20.png)

![](img/04f4e9ff792401422c961a2d284ec86d_22.png)

```python
class ResidualBlock(nn.Module):
    def __init__(self, in_channels, out_channels, stride=1):
        super().__init__()
        self.conv1 = nn.Conv2d(in_channels, out_channels, kernel_size=3, stride=stride, padding=1, bias=False)
        self.bn1 = nn.BatchNorm2d(out_channels)
        self.relu = nn.ReLU(inplace=True) # inplace=True 表示原地操作，节省内存
        self.conv2 = nn.Conv2d(out_channels, out_channels, kernel_size=3, stride=1, padding=1, bias=False)
        self.bn2 = nn.BatchNorm2d(out_channels)

        # 快捷连接
        self.shortcut = nn.Sequential()
        if stride != 1 or in_channels != out_channels:
            self.shortcut = nn.Sequential(
                nn.Conv2d(in_channels, out_channels, kernel_size=1, stride=stride, bias=False),
                nn.BatchNorm2d(out_channels)
            )

    def forward(self, x):
        identity = x
        out = self.conv1(x)
        out = self.bn1(out)
        out = self.relu(out)
        out = self.conv2(out)
        out = self.bn2(out)
        out += self.shortcut(identity) # 跳跃连接
        out = self.relu(out)
        return out
```

![](img/04f4e9ff792401422c961a2d284ec86d_24.png)

![](img/04f4e9ff792401422c961a2d284ec86d_26.png)

**代码解析**：
*   `inplace=True` 参数让ReLU激活函数直接修改输入张量，而不是创建副本，这能略微提升内存使用效率。
*   `shortcut` 模块用于处理当输入输出维度不匹配（通过`stride`改变尺寸或通道数不同）时的情况，它通过一个1x1卷积来调整维度。
*   前向传播中，`out += self.shortcut(identity)` 实现了核心的跳跃连接操作。

接着，我们用一个简单的网络结构来测试这个残差块。

![](img/04f4e9ff792401422c961a2d284ec86d_28.png)

![](img/04f4e9ff792401422c961a2d284ec86d_29.png)

![](img/04f4e9ff792401422c961a2d284ec86d_30.png)

![](img/04f4e9ff792401422c961a2d284ec86d_32.png)

```python
class SimpleResNet(nn.Module):
    def __init__(self, num_classes=10):
        super().__init__()
        self.conv1 = nn.Conv2d(1, 4, kernel_size=3, stride=1, padding=1, bias=False)
        self.bn1 = nn.BatchNorm2d(4)
        self.relu = nn.ReLU(inplace=True)
        # 使用两个残差块
        self.layer1 = ResidualBlock(4, 4)
        self.layer2 = ResidualBlock(4, 4)
        self.avgpool = nn.AdaptiveAvgPool2d((1, 1))
        self.fc = nn.Linear(4, num_classes)

    def forward(self, x):
        x = self.conv1(x)
        x = self.bn1(x)
        x = self.relu(x)
        x = self.layer1(x)
        x = self.layer2(x)
        x = self.avgpool(x)
        x = torch.flatten(x, 1)
        x = self.fc(x)
        return x
```

![](img/04f4e9ff792401422c961a2d284ec86d_34.png)

![](img/04f4e9ff792401422c961a2d284ec86d_36.png)

![](img/04f4e9ff792401422c961a2d284ec86d_38.png)

![](img/04f4e9ff792401422c961a2d284ec86d_39.png)

训练这个简单网络后，在MNIST测试集上能达到约92%的准确率。这验证了我们残差块实现的基本正确性。

![](img/04f4e9ff792401422c961a2d284ec86d_41.png)

---

![](img/04f4e9ff792401422c961a2d284ec86d_43.png)

![](img/04f4e9ff792401422c961a2d284ec86d_45.png)

![](img/04f4e9ff792401422c961a2d284ec86d_47.png)

## 2. 更通用的残差块实现

理解了基础结构后，我们来看一个更通用、功能更完整的残差块实现。这个实现可以灵活地改变通道数和特征图尺寸。

![](img/04f4e9ff792401422c961a2d284ec86d_49.png)

![](img/04f4e9ff792401422c961a2d284ec86d_51.png)

以下是改进后的残差块代码，它明确区分了块内的两条路径。

```python
class BasicBlock(nn.Module):
    expansion = 1
    def __init__(self, in_channels, out_channels, stride=1):
        super().__init__()
        # 残差函数主路径
        self.conv1 = nn.Conv2d(in_channels, out_channels, kernel_size=3, stride=stride, padding=1, bias=False)
        self.bn1 = nn.BatchNorm2d(out_channels)
        self.relu = nn.ReLU(inplace=True)
        self.conv2 = nn.Conv2d(out_channels, out_channels, kernel_size=3, stride=1, padding=1, bias=False)
        self.bn2 = nn.BatchNorm2d(out_channels)
        # 快捷连接路径
        self.shortcut = nn.Sequential()
        if stride != 1 or in_channels != out_channels * self.expansion:
            self.shortcut = nn.Sequential(
                nn.Conv2d(in_channels, out_channels * self.expansion, kernel_size=1, stride=stride, bias=False),
                nn.BatchNorm2d(out_channels * self.expansion)
            )

    def forward(self, x):
        identity = x
        out = self.conv1(x)
        out = self.bn1(out)
        out = self.relu(out)
        out = self.conv2(out)
        out = self.bn2(out)
        out += self.shortcut(identity)
        out = self.relu(out)
        return out
```

![](img/04f4e9ff792401422c961a2d284ec86d_53.png)

![](img/04f4e9ff792401422c961a2d284ec86d_54.png)

![](img/04f4e9ff792401422c961a2d284ec86d_55.png)

![](img/04f4e9ff792401422c961a2d284ec86d_57.png)

![](img/04f4e9ff792401422c961a2d284ec86d_58.png)

在这个网络中，我们可以堆叠多个这样的基础块来增加深度。

![](img/04f4e9ff792401422c961a2d284ec86d_60.png)

```python
class DeeperResNet(nn.Module):
    def __init__(self, block, num_blocks, num_classes=10):
        super().__init__()
        self.in_channels = 64
        self.conv1 = nn.Conv2d(3, 64, kernel_size=3, stride=1, padding=1, bias=False)
        self.bn1 = nn.BatchNorm2d(64)
        self.relu = nn.ReLU(inplace=True)
        # 创建多个层，每层包含多个残差块
        self.layer1 = self._make_layer(block, 64, num_blocks[0], stride=1)
        self.layer2 = self._make_layer(block, 128, num_blocks[1], stride=2)
        self.layer3 = self._make_layer(block, 256, num_blocks[2], stride=2)
        self.layer4 = self._make_layer(block, 512, num_blocks[3], stride=2)
        self.avgpool = nn.AdaptiveAvgPool2d((1, 1))
        self.fc = nn.Linear(512 * block.expansion, num_classes)

    def _make_layer(self, block, out_channels, num_blocks, stride):
        strides = [stride] + [1] * (num_blocks - 1)
        layers = []
        for stride in strides:
            layers.append(block(self.in_channels, out_channels, stride))
            self.in_channels = out_channels * block.expansion
        return nn.Sequential(*layers)

    def forward(self, x):
        # ... 前向传播逻辑
        return x
```

这个实现允许我们通过传入不同的 `num_blocks` 列表（例如 `[2, 2, 2, 2]`）来方便地构建不同深度的网络。

![](img/04f4e9ff792401422c961a2d284ec86d_62.png)

![](img/04f4e9ff792401422c961a2d284ec86d_63.png)

![](img/04f4e9ff792401422c961a2d284ec86d_64.png)

![](img/04f4e9ff792401422c961a2d284ec86d_65.png)

![](img/04f4e9ff792401422c961a2d284ec86d_66.png)

![](img/04f4e9ff792401422c961a2d284ec86d_67.png)

![](img/04f4e9ff792401422c961a2d284ec86d_68.png)

---

## 3. 使用官方的ResNet-34实现

对于实际项目和研究，重新实现复杂的网络如ResNet-34既耗时又容易出错。更高效的做法是使用经过充分测试的现有实现。

PyTorch在 `torchvision.models` 中提供了ResNet的标准实现。我们可以直接使用并微调它。

```python
import torchvision.models as models
# 加载预定义的ResNet-34模型
model = models.resnet34(pretrained=False) # pretrained=True 可以加载在ImageNet上预训练的权重
num_ftrs = model.fc.in_features
# 修改最后的全连接层以适应我们的分类任务（例如CIFAR-10有10类）
model.fc = nn.Linear(num_ftrs, 10)
```

![](img/04f4e9ff792401422c961a2d284ec86d_70.png)

以下是使用官方ResNet-34在CIFAR-10数据集上进行训练的核心步骤：

![](img/04f4e9ff792401422c961a2d284ec86d_72.png)

1.  **数据准备**：加载并标准化CIFAR-10数据集。
2.  **模型定义**：如上述代码所示，实例化ResNet-34并修改最后一层。
3.  **训练循环**：定义损失函数（如交叉熵损失）和优化器（如SGD或Adam），然后进行标准的训练迭代。

![](img/04f4e9ff792401422c961a2d284ec86d_74.png)

![](img/04f4e9ff792401422c961a2d284ec86d_75.png)

在训练过程中，我们观察到ResNet-34相比之前的简单网络，在更短的时间内达到了更好的性能（例如在CIFAR-10上约48%的准确率），这体现了深度残差网络的有效性。当然，针对小数据集可能出现过拟合，可以考虑添加Dropout等正则化技术来提升泛化能力。

![](img/04f4e9ff792401422c961a2d284ec86d_77.png)

**核心建议**：在学习和理解原理时，从零开始实现是有价值的。但在实际应用中，应优先考虑使用像PyTorch官方模型库这样成熟、高效的代码库，这可以节省大量时间并避免不必要的错误。

![](img/04f4e9ff792401422c961a2d284ec86d_79.png)

![](img/04f4e9ff792401422c961a2d284ec86d_81.png)

![](img/04f4e9ff792401422c961a2d284ec86d_83.png)

---

## 总结

![](img/04f4e9ff792401422c961a2d284ec86d_85.png)

![](img/04f4e9ff792401422c961a2d284ec86d_86.png)

![](img/04f4e9ff792401422c961a2d284ec86d_87.png)

![](img/04f4e9ff792401422c961a2d284ec86d_88.png)

![](img/04f4e9ff792401422c961a2d284ec86d_89.png)

本节课中我们一起学习了残差网络在PyTorch中的实现。
*   我们首先实现了一个简单的残差块，理解了跳跃连接和快捷路径的代码形式。
*   接着，我们探讨了一个更通用、可堆叠的残差块实现，它能够构建更深的网络。
*   最后，我们介绍了如何直接使用PyTorch官方提供的、经过优化的ResNet-34模型，这是实际项目中的最佳实践。

![](img/04f4e9ff792401422c961a2d284ec86d_91.png)

通过本节的实践，你应该对残差网络的核心思想如何转化为代码有了清晰的认识，并掌握了在PyTorch中使用这类强大模型的两种方式。