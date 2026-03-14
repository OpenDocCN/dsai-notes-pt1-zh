# 154：PyTorch中生成人脸图像的DCGAN代码示例 🎭

在本节课中，我们将学习如何在PyTorch中实现一个深度卷积生成对抗网络，用于生成人脸图像。我们将基于之前讨论的MNIST GAN示例进行构建，主要区别在于数据集和网络架构。

![](img/267b69d5601c7f8b5e5f6df929788cec_1.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_3.png)

## 概述 📋

![](img/267b69d5601c7f8b5e5f6df929788cec_5.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_7.png)

上一节我们介绍了GAN的基本概念和训练技巧。本节中，我们将具体实现一个用于生成人脸图像的DCGAN。核心变化是使用卷积层替代全连接层，并处理更复杂的CelebA数据集。

## 数据集准备 📁

我们使用CelebA数据集，它包含从谷歌图片搜索收集的名人面部图像。为了简化训练，我们将图像裁剪并调整大小为64x64像素。

```python
# 示例：加载并预处理CelebA数据集
dataset = CelebADataset(...)
transform = transforms.Compose([
    transforms.CenterCrop(178),
    transforms.Resize(64),
    transforms.ToTensor(),
    transforms.Normalize((0.5,), (0.5,))
])
```

![](img/267b69d5601c7f8b5e5f6df929788cec_9.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_10.png)

较小的图像尺寸（64x64而非128x128）有助于更稳定、更快速地训练常规DCGAN。图像质量、数据集大小、颜色多样性和视角等因素都会影响最终结果。

![](img/267b69d5601c7f8b5e5f6df929788cec_12.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_14.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_16.png)

## 网络架构 🏗️

我们的DCGAN包含生成器和判别器两部分。

### 生成器

生成器接收一个默认维度为100的噪声向量，通过转置卷积层进行上采样，最终生成64x64的图像。

以下是生成器的核心结构示意：

```python
class Generator(nn.Module):
    def __init__(self, latent_dim=100):
        super().__init__()
        self.main = nn.Sequential(
            # 转置卷积层，从 latent_dim 上采样
            nn.ConvTranspose2d(latent_dim, 512, 4, 1, 0, bias=False),
            nn.BatchNorm2d(512),
            nn.ReLU(True),
            # ... 更多层 ...
            # 最后一层使用Tanh激活，将像素值映射到[-1, 1]
            nn.ConvTranspose2d(64, 3, 4, 2, 1, bias=False),
            nn.Tanh()
        )
```

网络使用转置卷积进行上采样，配合批归一化和ReLU激活函数。最后一层使用`Tanh`激活函数，确保输出像素值在[-1, 1]的范围内。

### 判别器

判别器是一个卷积网络，用于判断输入图像是真实的还是生成的。

以下是判别器的核心结构示意：

```python
class Discriminator(nn.Module):
    def __init__(self):
        super().__init__()
        self.main = nn.Sequential(
            # 卷积层，下采样图像
            nn.Conv2d(3, 64, 4, 2, 1, bias=False),
            nn.LeakyReLU(0.2, inplace=True),
            # ... 更多层 ...
            # 最后一层卷积将特征图变为1x1，然后展平为单个值
            nn.Conv2d(512, 1, 4, 1, 0, bias=False),
            nn.Flatten()
        )
```

判别器使用卷积层、LeakyReLU激活函数和批归一化。最终，通过一个卷积核大小为4的卷积层，将4x4的特征图转换为1x1，再展平为一个标量值，代表图像为真的概率。我们使用二元交叉熵损失函数。

## 训练过程与代码 🔄

训练代码与之前的MNIST GAN示例类似，但针对图像数据进行了适配。

以下是训练循环的关键步骤：

1.  **训练判别器**：用真实图像和生成器生成的假图像分别计算损失，并更新判别器参数。
2.  **训练生成器**：用生成器生成的图像试图“欺骗”判别器，计算损失并更新生成器参数。

我们使用Adam优化器。尝试使用SGD训练判别器效果不佳，需要更多的学习率调参。

```python
# 优化器定义
opt_disc = torch.optim.Adam(disc.parameters(), lr=lr_disc)
opt_gen = torch.optim.Adam(gen.parameters(), lr=lr_gen)
```

![](img/267b69d5601c7f8b5e5f6df929788cec_18.png)

一个值得讨论的潜在技巧是：是否为判别器和生成器使用不同的噪声批次。在标准流程中，同一批生成的假图像既用于训练判别器识别假图像，也用于训练生成器欺骗判别器。使用两个独立的新噪声批次可能是一个值得尝试的改进点，尽管其实际效果尚未验证。

![](img/267b69d5601c7f8b5e5f6df929788cec_20.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_21.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_23.png)

## 训练结果与问题分析 📊

![](img/267b69d5601c7f8b5e5f6df929788cec_25.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_26.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_27.png)

经过20个周期的训练（约1小时），我们观察到了合理的进展。

![](img/267b69d5601c7f8b5e5f6df929788cec_29.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_30.png)

以下是训练过程中的关键观察：

![](img/267b69d5601c7f8b5e5f6df929788cec_31.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_33.png)

*   **损失曲线**：判别器损失没有归零（归零意味着判别器过于强大），生成器损失呈上升趋势，这是一个良好的信号。
*   **生成图像质量**：随着训练进行，生成的图像从最初的无法辨认，逐渐显现出人脸特征。虽然部分图像仍不完美，但整体效果尚可接受。

![](img/267b69d5601c7f8b5e5f6df929788cec_35.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_36.png)

以下是第10个周期生成的部分图像示例：

![](img/267b69d5601c7f8b5e5f6df929788cec_38.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_40.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_42.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_20.png)

![](img/267b69d5601c7f8b5e5f6df929788cec_44.png)

然而，训练过程也可能失败。例如，当在生成器中错误地使用常规ReLU而非LeakyReLU时，训练初期看似正常，但随后可能出现损失尖峰和**模式崩溃**。

![](img/267b69d5601c7f8b5e5f6df929788cec_46.png)

以下是模式崩溃的示例，所有生成图像变得高度相似：

![](img/267b69d5601c7f8b5e5f6df929788cec_38.png)

## 总结与学习建议 💡

本节课中，我们一起学习了在PyTorch中实现DCGAN来生成人脸图像。我们涵盖了从数据集准备、卷积架构设计到训练调试的完整流程。

实现和调试GAN代码具有挑战性，是深度学习项目中常见的复杂任务。掌握它需要时间、实践和耐心。建议的学习路径包括：

*   **亲自动手**：尝试自己实现代码，或修改现有代码，应用不同的训练技巧。
*   **深入调试**：通过实验理解代码的每一部分，例如尝试不同的激活函数观察效果。
*   **项目实践**：在实际项目中应用所学知识，这是巩固技能的最佳方式。
*   **利用资源**：在遇到问题时，积极搜索网络资源、阅读论文并与他人讨论。

![](img/267b69d5601c7f8b5e5f6df929788cec_48.png)

深度学习编码能力的提升是一个持续的过程，需要通过不断的项目实践和问题解决来积累经验。