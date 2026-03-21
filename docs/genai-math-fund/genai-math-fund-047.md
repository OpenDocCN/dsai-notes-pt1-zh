# 047：DDPM的实现 🎼

![](img/c2c3aeb7b3b3badd1c6059c608068f11_1.png)

在本教程中，我们将动手实现最先进的扩散模型（Diffusion Model）的核心思想。我们将从理论回顾开始，逐步讲解前向过程、反向过程、训练流程以及代码实现，最终生成图像样本。

## 概述：扩散模型流程

首先，我们通过一张简单的图像来回顾扩散模型的前向和反向过程。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_1.png)

上图展示了扩散模型的核心流程。左侧是**前向过程**，从原始数据 `x0` 开始，逐步添加噪声，经过 `T` 步后，数据 `xT` 最终变为一个标准高斯分布（均值为0，方差为I）。右侧是**反向过程**，从高斯噪声 `xT` 开始，逐步去噪，最终恢复出原始数据 `x0`。

上一节我们介绍了扩散模型的理论基础，本节中我们来看看如何用代码实现它。

## 前向过程与反向过程

### 前向过程
前向过程是一个固定的、没有可学习参数的概率过程。其目的是通过逐步添加噪声，将数据分布转化为一个简单的已知分布（如高斯分布）。

给定初始数据 `x0`，第 `t` 步的加噪数据 `xt` 可以通过以下公式计算：
`xt = sqrt(alpha_bar_t) * x0 + sqrt(1 - alpha_bar_t) * epsilon`
其中，`epsilon` 是从标准高斯分布中采样的噪声，`alpha_bar_t` 是预先计算好的系数。

这个过程是确定性的吗？不，因为每一步都需要从高斯分布中采样噪声 `epsilon`，所以它是一个**概率性过程**。

### 反向过程
反向过程是扩散模型生成新数据的核心。它从一个纯高斯噪声 `xT` 开始，通过一个学习到的去噪模型（如U-Net），逐步预测并移除噪声，最终得到清晰的数据 `x0`。

与GAN或VAE的一步生成不同，扩散模型的生成需要顺序执行所有 `T` 个去噪步骤，因此更为耗时。

## 训练流程详解

现在，让我们深入理解扩散模型的训练过程。我们将使用U-Net作为去噪模型，你需要对U-Net架构有所了解。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_3.png)

训练的目标是让模型学会预测添加到数据中的噪声。以下是训练步骤的分解：

1.  **准备数据**：取一个批量的真实数据 `x0`。
2.  **采样时间步**：为批量中的每个样本随机采样一个时间步 `t`（范围从1到T）。
3.  **计算加噪数据**：使用前向过程公式，为每个 `x0` 和对应的 `t` 计算加噪后的 `xt`。
4.  **模型预测**：将 `xt` 和对应的时间步 `t`（经过嵌入处理）一起输入U-Net模型，得到模型预测的噪声 `epsilon_theta`。
5.  **计算损失**：计算模型预测的噪声 `epsilon_theta` 与真实添加的噪声 `epsilon` 之间的均方误差（MSE）。
6.  **参数更新**：通过反向传播计算梯度，并更新U-Net模型的参数。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_5.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_7.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_9.png)

需要注意的是，在完整的损失函数中，除了上述主要项，还应包含一项针对 `t=1` 时 `x0` 重建的损失。但在许多简化实现中，可能只使用噪声预测损失。

### 时间步嵌入
在将时间步 `t` 输入U-Net时，不能直接将其作为标量加入，因为 `xt` 是高维张量，标量 `t` 可能会被忽略。因此，我们需要对 `t` 进行嵌入（Embedding）。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_11.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_13.png)

以下是两种常见的嵌入方法：
*   **正弦位置嵌入**：这是Transformer中常用的方法，能更好地编码时间步的顺序信息。
*   **简单通道添加**：一种更简单的方法。例如，对于单通道的32x32图像，我们将 `t` 扩展成一个32x32的矩阵（所有元素值都为 `t`），然后将其作为额外通道与 `xt` 拼接。这样，输入就从 `[1, 32, 32]` 变成了 `[2, 32, 32]`。本教程代码将采用这种方法。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_15.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_17.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_19.png)

## 代码实现：从理论到实践

![](img/c2c3aeb7b3b3badd1c6059c608068f11_21.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_23.png)

理解了理论之后，我们开始结合代码进行实现。由于计算资源限制，部分演示在本地完成，但代码在Colab中同样可以运行。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_25.png)

### 1. 定义超参数与预计算量

![](img/c2c3aeb7b3b3badd1c6059c608068f11_27.png)

首先，我们需要定义总时间步数 `T` 和一些关键的预计算系数。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_29.png)

```python
import torch
import torch.nn as nn

T = 1000  # 总扩散步数
# 定义beta调度表，从1e-4线性增加到0.02
betas = torch.linspace(1e-4, 0.02, T)
# 计算alpha和alpha_bar
alphas = 1. - betas
alphas_bar = torch.cumprod(alphas, dim=0)  # alpha_bar_t = prod_{s=1}^{t} alpha_s
# 计算后续公式中常用的平方根项
sqrt_alphas_bar = torch.sqrt(alphas_bar)
sqrt_one_minus_alphas_bar = torch.sqrt(1. - alphas_bar)
```

![](img/c2c3aeb7b3b3badd1c6059c608068f11_31.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_33.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_35.png)

这些值（`betas`, `alphas`, `alphas_bar`等）在前向过程中是固定的，不需要学习。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_37.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_39.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_41.png)

### 2. 构建U-Net模型

![](img/c2c3aeb7b3b3badd1c6059c608068f11_43.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_44.png)

我们将使用一个简化版的U-Net作为去噪模型。其输入是带噪图像 `xt` 和时间步嵌入，输出是预测的噪声。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_46.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_47.png)

```python
class UNet(nn.Module):
    def __init__(self, in_channels=2):  # 输入通道为2 (xt + 时间嵌入)
        super(UNet, self).__init__()
        # 这里应定义下采样、瓶颈和上采样层
        # 例如：Conv2d, BatchNorm2d, ReLU, MaxPool2d, ConvTranspose2d等
        # 具体架构细节略，可参考标准U-Net实现
        self.down1 = nn.Sequential(nn.Conv2d(in_channels, 64, 3, padding=1), nn.ReLU())
        # ... 更多层定义
        self.up1 = nn.Sequential(nn.ConvTranspose2d(128, 64, 2, stride=2), nn.ReLU())
        # ... 输出层
        self.out = nn.Conv2d(64, 1, 1)  # 输出单通道噪声图

    def forward(self, x, t_emb):
        # t_emb 是时间步嵌入，需要调整形状并与x拼接
        # 前向传播逻辑：下采样 -> 瓶颈 -> 上采样
        # ...
        return predicted_noise
```

![](img/c2c3aeb7b3b3badd1c6059c608068f11_49.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_50.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_52.png)

### 3. 前向扩散过程（加噪）

![](img/c2c3aeb7b3b3badd1c6059c608068f11_54.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_56.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_57.png)

这个函数根据给定的 `x0` 和时间步 `t`，计算加噪后的 `xt`。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_59.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_61.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_63.png)

```python
def forward_diffusion(x0, t, sqrt_alphas_bar, sqrt_one_minus_alphas_bar):
    """
    x0: 原始数据 [batch, channel, height, width]
    t: 时间步索引 [batch]，每个元素在[0, T-1]之间
    """
    # 从标准高斯分布采样噪声
    noise = torch.randn_like(x0)
    # 通过索引获取对应时间步的系数
    sqrt_alpha_bar_t = sqrt_alphas_bar[t].view(-1, 1, 1, 1)
    sqrt_one_minus_alpha_bar_t = sqrt_one_minus_alphas_bar[t].view(-1, 1, 1, 1)
    # 计算 xt
    xt = sqrt_alpha_bar_t * x0 + sqrt_one_minus_alpha_bar_t * noise
    return xt, noise
```

![](img/c2c3aeb7b3b3badd1c6059c608068f11_65.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_67.png)

### 4. 训练步骤

![](img/c2c3aeb7b3b3badd1c6059c608068f11_69.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_71.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_72.png)

在训练循环中，我们执行以下操作：

```python
def train_step(model, x0, optimizer, t, sqrt_alphas_bar, sqrt_one_minus_alphas_bar):
    model.train()
    optimizer.zero_grad()

    # 1. 前向扩散，获取加噪数据xt和真实噪声
    xt, noise = forward_diffusion(x0, t, sqrt_alphas_bar, sqrt_one_minus_alphas_bar)

    # 2. 准备时间步嵌入 (简单通道添加法)
    # 将t扩展为与xt空间维度相同的张量
    t_emb = t.view(-1, 1, 1, 1).expand(-1, 1, xt.shape[2], xt.shape[3]).float()
    # 拼接xt和时间嵌入
    model_input = torch.cat([xt, t_emb], dim=1)

    # 3. 模型预测噪声
    predicted_noise = model(model_input, t_emb)  # 这里t_emb也可能用于其他方式的嵌入

    # 4. 计算损失 (MSE between predicted and true noise)
    loss = nn.MSELoss()(predicted_noise, noise)

    # 5. 反向传播与优化
    loss.backward()
    optimizer.step()

    return loss.item()
```

![](img/c2c3aeb7b3b3badd1c6059c608068f11_74.png)

### 5. 反向采样过程（生成）

![](img/c2c3aeb7b3b3badd1c6059c608068f11_76.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_77.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_79.png)

训练好模型后，我们可以通过反向过程从噪声生成图像。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_81.png)

```python
@torch.no_grad()
def sample(model, sqrt_alphas_bar, sqrt_one_minus_alphas_bar, alphas, betas, img_shape, num_samples=16):
    """
    从纯噪声开始，逐步去噪生成图像。
    """
    model.eval()
    # 1. 从标准高斯分布初始化 xT
    x = torch.randn((num_samples, 1, *img_shape), device=device)

    # 2. 从 T-1 逐步迭代到 0
    for t in reversed(range(T)):
        # 当前时间步
        t_tensor = torch.full((num_samples,), t, device=device, dtype=torch.long)
        # 准备时间嵌入
        t_emb = t_tensor.view(-1, 1, 1, 1).expand(-1, 1, *img_shape).float()
        # 拼接输入
        model_input = torch.cat([x, t_emb], dim=1)

        # 3. 模型预测噪声
        predicted_noise = model(model_input, t_emb)

        # 4. 计算 alpha_t, alpha_bar_t 等系数
        alpha_t = alphas[t].view(-1, 1, 1, 1)
        alpha_bar_t = sqrt_alphas_bar[t].view(-1, 1, 1, 1)
        sqrt_one_minus_alpha_bar_t = sqrt_one_minus_alphas_bar[t].view(-1, 1, 1, 1)

        # 5. 根据公式计算 x_{t-1}
        # 简化公式: x_{t-1} = 1/sqrt(alpha_t) * (x_t - (1-alpha_t)/sqrt(1-alpha_bar_t) * predicted_noise) + sigma_t * z
        # 其中当 t > 0 时，z ~ N(0, I)
        if t > 0:
            z = torch.randn_like(x)
        else:
            z = 0

        x = (1 / torch.sqrt(alpha_t)) * (x - ((1 - alpha_t) / sqrt_one_minus_alpha_bar_t) * predicted_noise) + torch.sqrt(betas[t]) * z

        # 6. 将像素值限制在合理范围
        x = torch.clamp(x, 0.0, 1.0)

    return x
```

## 运行结果与改进方向

![](img/c2c3aeb7b3b3badd1c6059c608068f11_83.png)

运行上述代码进行训练（例如15个周期）后，可以进行采样。初始结果可能不理想，生成图像模糊或噪声较多。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_85.png)

这主要有两个原因：
1.  **训练不充分**：在训练循环中，我们通常需要对一个批次内的数据采样**多个不同的时间步t**，并计算损失的平均值，而不是只用一个随机的t。这能提供更稳定的梯度信号。
2.  **损失函数不完整**：如前所述，我们只实现了损失函数的主要部分（噪声预测损失），而忽略了 `t=1` 时的重建损失项。添加此项有助于改善生成质量。

要获得更好的生成样本，你需要：
*   增加训练周期（Epoch）。
*   确保在训练批次中采样多个时间步。
*   考虑实现完整的损失函数。
*   可能使用更复杂的时间步嵌入（如正弦嵌入）和U-Net架构。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_87.png)

## 总结

![](img/c2c3aeb7b3b3badd1c6059c608068f11_89.png)

本节课中我们一起学习了去噪扩散概率模型（DDPM）的代码实现。我们回顾了扩散模型的前向与反向过程，详细拆解了训练流程，并逐步实现了关键代码模块，包括超参数定义、U-Net构建、前向加噪、训练循环以及反向采样生成。

![](img/c2c3aeb7b3b3badd1c6059c608068f11_91.png)

![](img/c2c3aeb7b3b3badd1c6059c608068f11_92.png)

关键点在于理解扩散模型通过**学习去噪**来生成数据，以及时间步信息在模型中的重要性。虽然初始实现结果可能简单，但它构成了更高级扩散模型（如DDIM、Stable Diffusion）的基础。在下一个教程中，我们将探索DDPM的其他变体或改进实现。