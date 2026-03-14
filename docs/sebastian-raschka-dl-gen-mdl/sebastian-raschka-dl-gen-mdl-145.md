# 145：PyTorch中的手写数字变分自编码器代码示例 🧠

![](img/b2e6c1239a4480a5add3f26581c2e9bb_1.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_2.png)

在本节课中，我们将学习如何使用PyTorch构建一个用于MNIST手写数字的变分自编码器。我们将从最简单的卷积结构开始，逐步解析其与普通自编码器的核心区别，并理解如何通过KL散度损失和重参数化技巧来学习一个平滑的潜在空间分布。

## 概述

![](img/b2e6c1239a4480a5add3f26581c2e9bb_4.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_5.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_7.png)

变分自编码器是一种生成模型，它不仅能学习数据的压缩表示，还能学习数据的潜在概率分布。这使得我们可以从该分布中采样，生成新的、类似训练数据的新样本。本节课我们将专注于一个二维潜在空间的MNIST示例，以便直观地可视化学习过程。

![](img/b2e6c1239a4480a5add3f26581c2e9bb_9.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_10.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_12.png)

## 准备工作与数据加载

![](img/b2e6c1239a4480a5add3f26581c2e9bb_14.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_16.png)

首先，我们进行一些常规设置并加载MNIST数据集。由于变分自编码器是无监督模型，我们只需要图像数据，不需要标签。

![](img/b2e6c1239a4480a5add3f26581c2e9bb_18.png)

```python
import torch
from torchvision import datasets, transforms

# 超参数设置
batch_size = 256
learning_rate = 0.0005
num_epochs = 50

![](img/b2e6c1239a4480a5add3f26581c2e9bb_20.png)

# 加载MNIST数据集，仅使用图像
train_dataset = datasets.MNIST(root='./data',
                               train=True,
                               transform=transforms.ToTensor(),
                               download=True)
train_loader = torch.utils.data.DataLoader(dataset=train_dataset,
                                           batch_size=batch_size,
                                           shuffle=True)
```

![](img/b2e6c1239a4480a5add3f26581c2e9bb_22.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_24.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_26.png)

## 构建变分自编码器模型 🏗️

![](img/b2e6c1239a4480a5add3f26581c2e9bb_28.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_30.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_32.png)

上一节我们设置了数据管道，本节中我们来看看模型的核心架构。变分自编码器同样由编码器和解码器组成，但关键区别在于编码器输出的是潜在分布的参数（均值和方差的对数），而非确定性的编码。

![](img/b2e6c1239a4480a5add3f26581c2e9bb_34.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_36.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_38.png)

以下是模型定义的核心部分：

![](img/b2e6c1239a4480a5add3f26581c2e9bb_40.png)

```python
import torch.nn as nn
import torch.nn.functional as F

![](img/b2e6c1239a4480a5add3f26581c2e9bb_42.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_44.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_45.png)

class ConvVAE(nn.Module):
    def __init__(self):
        super(ConvVAE, self).__init__()
        # 编码器部分：将28x28图像压缩为潜在空间参数
        self.encoder = nn.Sequential(
            nn.Conv2d(1, 32, kernel_size=3, stride=2, padding=1), # 输出: 14x14x32
            nn.ReLU(),
            nn.Conv2d(32, 64, kernel_size=3, stride=2, padding=1), # 输出: 7x7x64
            nn.ReLU(),
            nn.Flatten(),
        )
        # 为潜在空间的两个维度分别定义均值和方差对数的全连接层
        self.fc_mu = nn.Linear(7*7*64, 2)    # 均值向量 μ
        self.fc_logvar = nn.Linear(7*7*64, 2) # 方差对数向量 log(σ^2)

        # 解码器部分：将潜在变量重构回图像
        self.decoder_input = nn.Linear(2, 7*7*64)
        self.decoder = nn.Sequential(
            nn.ConvTranspose2d(64, 32, kernel_size=3, stride=2, padding=1, output_padding=1),
            nn.ReLU(),
            nn.ConvTranspose2d(32, 1, kernel_size=3, stride=2, padding=1, output_padding=1),
            nn.Sigmoid() # 将像素值压缩到[0,1]范围
        )

    def reparameterize(self, mu, logvar):
        """重参数化技巧：从N(μ, σ^2)采样，同时保持梯度可传播。"""
        std = torch.exp(0.5 * logvar) # 计算标准差 σ
        eps = torch.randn_like(std)   # 从标准正态分布采样噪声 ε
        return mu + eps * std         # z = μ + ε * σ

    def encode(self, x):
        """编码过程：输入图像，输出潜在变量z。"""
        h = self.encoder(x)
        mu, logvar = self.fc_mu(h), self.fc_logvar(h)
        z = self.reparameterize(mu, logvar)
        return z, mu, logvar

    def decode(self, z):
        """解码过程：输入潜在变量z，输出重构图像。"""
        h = self.decoder_input(z)
        h = h.view(-1, 64, 7, 7) # 重塑为卷积层需要的形状
        reconstruction = self.decoder(h)
        # 裁剪到原始28x28尺寸（转置卷积可能导致尺寸+1）
        return reconstruction[:, :, :28, :28]

    def forward(self, x):
        """前向传播：返回重构图像、潜在变量z、均值μ和方差对数logvar。"""
        z, mu, logvar = self.encode(x)
        reconstruction = self.decode(z)
        return reconstruction, z, mu, logvar
```

## 理解重参数化技巧与损失函数 ⚙️

![](img/b2e6c1239a4480a5add3f26581c2e9bb_47.png)

我们已经定义了模型结构，现在需要理解其训练目标。变分自编码器的损失函数由两部分组成：重构损失和KL散度损失。

**重构损失**衡量解码器输出与原始输入的相似度，我们使用均方误差。其公式为：
`MSE = (1/N) * Σ (x_i - x̂_i)^2`

![](img/b2e6c1239a4480a5add3f26581c2e9bb_49.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_51.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_53.png)

**KL散度损失**迫使编码器学习的潜在分布`q(z|x)`接近标准正态分布`p(z)=N(0,1)`。对于每个潜在维度`j`，其公式为：
`KL_loss_j = -0.5 * Σ (1 + log(σ_j^2) - μ_j^2 - σ_j^2)`

![](img/b2e6c1239a4480a5add3f26581c2e9bb_55.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_57.png)

总损失是两者的加权和：
`Total Loss = Reconstruction_Loss + β * KL_Loss`
其中`β`是一个超参数，用于平衡两项损失。

![](img/b2e6c1239a4480a5add3f26581c2e9bb_59.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_61.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_63.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_65.png)

以下是训练循环中计算损失的关键代码：

![](img/b2e6c1239a4480a5add3f26581c2e9bb_67.png)

```python
def train(model, train_loader, optimizer, epoch, beta=1.0):
    model.train()
    train_loss = 0
    for batch_idx, (data, _) in enumerate(train_loader): # 忽略标签
        optimizer.zero_grad()
        recon_batch, z, mu, logvar = model(data) # 前向传播

        # 1. 计算重构损失 (MSE)
        recon_loss = F.mse_loss(recon_batch, data, reduction='sum')

        # 2. 计算KL散度损失
        # KL = -0.5 * sum(1 + log(sigma^2) - mu^2 - sigma^2)
        kl_loss = -0.5 * torch.sum(1 + logvar - mu.pow(2) - logvar.exp())

        # 3. 组合总损失
        loss = recon_loss + beta * kl_loss

        loss.backward()
        optimizer.step()
        train_loss += loss.item()
    print(f'Epoch {epoch}, Average Loss: {train_loss / len(train_loader.dataset):.4f}')
```

![](img/b2e6c1239a4480a5add3f26581c2e9bb_69.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_71.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_72.png)

## 训练模型与结果分析 📈

![](img/b2e6c1239a4480a5add3f26581c2e9bb_74.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_76.png)

现在，我们可以初始化模型、优化器并开始训练。

![](img/b2e6c1239a4480a5add3f26581c2e9bb_78.png)

```python
model = ConvVAE()
optimizer = torch.optim.Adam(model.parameters(), lr=learning_rate)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_80.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_82.png)

for epoch in range(1, num_epochs + 1):
    train(model, train_loader, optimizer, epoch)
```

![](img/b2e6c1239a4480a5add3f26581c2e9bb_84.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_85.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_87.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_89.png)

训练过程中，重构损失和KL损失会相互博弈。通常，重构损失迅速下降，而KL损失可能先下降后略有上升，但总损失呈下降趋势。训练完成后，我们可以进行以下分析：

1.  **可视化重构效果**：对比原始图像和重构图像，由于潜在空间只有二维，重构图像可能会有些模糊或数字类别混淆。
2.  **可视化潜在空间**：将所有训练图像的潜在编码`z`在二维平面上画出，并用颜色标注其真实标签。一个训练良好的VAE，其潜在空间应该大致是中心为零、类似高斯分布的，并且不同数字的簇可能会有一定分离（尽管在二维中会严重重叠）。
3.  **从潜在空间采样生成**：这是VAE最强大的功能。我们可以从标准正态分布`N(0,1)`中随机采样一个点`z`，然后通过解码器生成一张新的“手写数字”图像。

![](img/b2e6c1239a4480a5add3f26581c2e9bb_91.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_92.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_94.png)

以下是生成新图像的示例代码：

![](img/b2e6c1239a4480a5add3f26581c2e9bb_96.png)

```python
def generate_samples(model, num_samples=10):
    """从学习到的潜在分布中采样并生成新图像。"""
    model.eval()
    with torch.no_grad():
        # 从标准正态分布采样
        z = torch.randn(num_samples, 2)
        # 仅使用解码器生成图像
        samples = model.decode(z)
        return samples

![](img/b2e6c1239a4480a5add3f26581c2e9bb_98.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_99.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_101.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_103.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_104.png)

# 生成并显示10个样本
new_digits = generate_samples(model, 10)
# ... 使用matplotlib等库显示new_digits中的图像
```

![](img/b2e6c1239a4480a5add3f26581c2e9bb_106.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_108.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_110.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_112.png)

生成的图像可能看起来合理，也可能有一些无法辨认的结果，这主要归因于我们使用了极度压缩的二维潜在空间。增大潜在空间的维度（例如20维或更大）通常会显著提高生成图像的质量和多样性。

![](img/b2e6c1239a4480a5add3f26581c2e9bb_114.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_116.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_118.png)

## 总结

![](img/b2e6c1239a4480a5add3f26581c2e9bb_119.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_121.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_123.png)

![](img/b2e6c1239a4480a5add3f26581c2e9bb_125.png)

本节课中我们一起学习了如何使用PyTorch实现一个卷积变分自编码器来处理MNIST数据集。我们深入探讨了其与普通自编码器的关键区别：通过编码器输出分布的参数（均值和方差），并利用**重参数化技巧**实现可微分的随机采样。模型的损失函数结合了**重构损失**和**KL散度损失**，前者保证生成质量，后者规范潜在空间使其接近标准正态分布。尽管我们的二维示例在生成效果上受限，但它清晰地展示了VAE的工作原理：学习数据的潜在概率分布，从而能够从该分布中采样并生成新的数据样本。