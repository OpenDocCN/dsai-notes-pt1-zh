# 125：PyTorch中的迁移学习代码示例 🚀

在本节课中，我们将学习如何在PyTorch中实现迁移学习。具体来说，我们将使用一个在ImageNet数据集上预训练好的VGG16模型，然后在CIFAR-10目标数据集上进行微调。我们将冻结所有卷积层，只训练最后的全连接层，并替换输出层以适应CIFAR-10的10个类别。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_1.png)

## 概述与准备工作

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_3.png)

首先，我们需要导入必要的库并准备数据。代码的大部分内容与我们之前讨论VGG16时相同，但有两个关键点需要注意：输入图像的标准化参数。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_5.png)

以下是数据准备和标准化步骤：

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_7.png)

```python
import torch
import torchvision
import torchvision.transforms as transforms

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_8.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_9.png)

# 定义标准化参数
normalize = transforms.Normalize(mean=[0.485, 0.456, 0.406],
                                 std=[0.229, 0.224, 0.225])

# 创建数据转换流程
transform = transforms.Compose([
    transforms.ToTensor(),
    normalize
])
```

这里使用的标准化参数（均值和标准差）并非随意设定。它们来源于ImageNet数据集，因为我们将要使用的预训练模型正是在使用这些参数标准化的ImageNet数据上训练的。为了确保我们的输入与模型训练时的数据尺度一致，我们使用相同的值。

**关于标准化参数的讨论**：有时也建议根据你自己的数据集重新计算这些参数。这是因为你的图像可能在亮度、色彩等方面存在系统性差异。在另一个代码示例中，我计算了CIFAR-10数据集的均值和标准差，发现它们与ImageNet的参数非常接近。在实际应用中，如果两者差异显著，最好两种方法都尝试一下。使用ImageNet参数可以保持一致性，而使用自己数据集的参数则可能更好地适应数据特性。

## 加载预训练模型 🏗️

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_11.png)

现在，我们进入更有趣的部分——迁移学习。我们将加载预训练的VGG16模型。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_13.png)

```python
# 加载预训练的VGG16模型
model = torchvision.models.vgg16(pretrained=True)
print(model)
```

运行上述代码会输出模型的结构。VGG16模型主要分为两部分：
1.  **`features`**：这部分包含所有的卷积层，可以理解为自动特征提取器。
2.  **`classifier`**：这部分包含最后的全连接层（线性层），用于分类。

在`classifier`部分，有一个自适应平均池化层（AdaptiveAvgPool2d），其作用类似于我们之前讨论的全局平均池化，它负责连接卷积部分的最后一层和线性部分的第一层，确保维度匹配。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_15.png)

## 实施迁移学习策略 🔧

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_17.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_18.png)

我们的策略是冻结卷积层（`features`部分），只微调`classifier`部分的最后几层，并替换输出层。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_20.png)

以下是具体步骤：

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_22.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_24.png)

**第一步：冻结整个模型**
我们首先遍历模型的所有参数，并将它们的`requires_grad`属性设置为`False`。这样，在反向传播时这些参数就不会被更新。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_26.png)

```python
# 冻结模型所有参数
for param in model.parameters():
    param.requires_grad = False
```

**第二步：解冻并准备微调特定层**
我们只打算训练`classifier`模块中的最后三个线性层（即包含可训练参数的层）。我们需要查看`model.classifier`的结构来确定这些层的位置。

```python
print(model.classifier)
```

假设`model.classifier`是一个包含7个模块的`Sequential`容器，其中索引为1、3、6的模块是线性层（`Linear`）。我们将解冻这些层。

```python
# 解冻我们想要训练的全连接层
model.classifier[1].requires_grad = True  # 第一个线性层
model.classifier[3].requires_grad = True  # 第二个线性层
# 注意：我们不会解冻 model.classifier[6]，因为我们要替换它
```

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_28.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_29.png)

**第三步：替换输出层**
预训练模型的输出层有1000个单元（对应ImageNet的1000个类别）。对于CIFAR-10，我们需要将其替换为只有10个输出单元的线性层。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_31.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_33.png)

```python
# 替换最后的输出层
num_ftrs = model.classifier[6].in_features
model.classifier[6] = torch.nn.Linear(num_ftrs, 10)  # CIFAR-10有10个类
```

## 训练与关键发现 🚂

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_35.png)

完成模型修改后，就可以像往常一样进行训练了。一个有趣的现象是，由于模型是预训练的，训练从一开始就能达到不错的准确率。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_36.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_38.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_39.png)

然而，在最初的实验中，使用原始CIFAR-10图像尺寸（32x32）进行微调，最终性能并不理想（约77%的准确率）。这可能是因为VGG16是在更大的图像（至少224x224）上预训练的，网络中的卷积层对物体尺寸有一定的假设。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_41.png)

**解决方案：调整输入图像尺寸**
为了解决这个问题，我在第二个实现中将输入图像的大小调整为224x224。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_42.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_44.png)

```python
# 在数据转换中调整图像大小
transform_large = transforms.Compose([
    transforms.Resize((224, 224)),  # 调整图像大小
    transforms.ToTensor(),
    normalize
])
```

使用调整尺寸后的图像进行训练，模型性能得到了显著提升，准确率达到了约85%。这证明了输入尺寸与预训练数据的一致性对迁移学习效果的重要性。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_46.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_48.png)

**效率对比**
迁移学习不仅提升了性能，还提高了训练效率：
*   在32x32图像上微调50个epoch耗时约38分钟。
*   相比之下，从头开始训练一个类似的VGG16网络（非预训练）需要约90分钟。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_50.png)

时间节省主要归功于我们冻结了大部分卷积层，只训练了少数全连接层参数。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_52.png)

## 总结与扩展思考 💡

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_54.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_56.png)

本节课我们一起学习了在PyTorch中实现迁移学习的完整流程：
1.  使用预训练模型（如VGG16）。
2.  冻结卷积层作为固定的特征提取器。
3.  解冻并微调分类器中的特定全连接层。
4.  替换输出层以适应新的任务。
5.  注意输入数据的标准化和尺寸与预训练设置保持一致。

**核心要点回顾**：
*   **标准化参数**：使用与预训练模型一致的参数（如ImageNet的均值和标准差）。
*   **模型结构**：理解模型的`features`（卷积部分）和`classifier`（全连接部分）。
*   **参数冻结**：通过设置`param.requires_grad = False`来冻结参数。
*   **层替换**：修改`model.classifier`中的模块来替换输出层。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_58.png)

**扩展建议**：
在实际项目中，微调多少层是一个需要根据具体情况决定的超参数。根据相关研究，微调最后3个全连接层通常效果不错，但有时只训练最后一层或微调所有层可能更好。你可以尝试不同的策略：
*   **策略A**：仅训练新替换的输出层。
*   **策略B**：训练最后几个全连接层（如本教程所示）。
*   **策略C**：解冻所有层进行微调（学习率通常要设置得更小）。

此外，PyTorch的`torchvision.models`模块还提供了其他优秀的预训练模型，如ResNet、DenseNet、MobileNet和Inception等，你可以根据项目需求（如精度、速度、模型大小）进行选择。

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_60.png)

![](img/75fa3cc0c5748df52ac0ee6a57c7b411_62.png)

希望本教程能帮助你掌握迁移学习的基本方法，并将其应用到你的深度学习项目中。下一讲，我们将开始一个全新的主题：用于处理文本数据的循环神经网络（RNN）。