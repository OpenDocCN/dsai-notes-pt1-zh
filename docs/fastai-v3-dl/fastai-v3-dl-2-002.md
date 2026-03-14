# 深度学习基础到稳定扩散模型：2：稳定扩散模型深度解析 🧠

![](img/0629c20054834ad43432bab705e8969b_1.png)

![](img/0629c20054834ad43432bab705e8969b_2.png)

在本节课中，我们将深入探索稳定扩散模型的代码实现。我们将拆解流行的API和库背后的生成过程，了解每个独立组件的工作原理，并学习如何修改它们。

![](img/0629c20054834ad43432bab705e8969b_4.png)

## 环境准备与基础生成

![](img/0629c20054834ad43432bab705e8969b_6.png)

![](img/0629c20054834ad43432bab705e8969b_7.png)

![](img/0629c20054834ad43432bab705e8969b_8.png)

![](img/0629c20054834ad43432bab705e8969b_9.png)

![](img/0629c20054834ad43432bab705e8969b_10.png)

首先，我们通过复制Hugging Face默认稳定扩散流水线的核心代码，来重现图像生成过程。以下是基础设置和生成循环的代码框架：

![](img/0629c20054834ad43432bab705e8969b_12.png)

![](img/0629c20054834ad43432bab705e8969b_13.png)

![](img/0629c20054834ad43432bab705e8969b_14.png)

![](img/0629c20054834ad43432bab705e8969b_15.png)

![](img/0629c20054834ad43432bab705e8969b_17.png)

```python
# 基础设置代码
import torch
from diffusers import StableDiffusionPipeline

![](img/0629c20054834ad43432bab705e8969b_19.png)

![](img/0629c20054834ad43432bab705e8969b_20.png)

![](img/0629c20054834ad43432bab705e8969b_21.png)

![](img/0629c20054834ad43432bab705e8969b_22.png)

# 加载预训练模型
pipe = StableDiffusionPipeline.from_pretrained("CompVis/stable-diffusion-v1-4")
pipe = pipe.to("cuda")

![](img/0629c20054834ad43432bab705e8969b_23.png)

![](img/0629c20054834ad43432bab705e8969b_24.png)

![](img/0629c20054834ad43432bab705e8969b_25.png)

![](img/0629c20054834ad43432bab705e8969b_27.png)

![](img/0629c20054834ad43432bab705e8969b_28.png)

![](img/0629c20054834ad43432bab705e8969b_30.png)

# 生成图像
prompt = "A watercolor picture of an otter"
image = pipe(prompt).images[0]
image.save("otter.png")
```

这段代码会生成一张水獭的水彩画。但我们的目标是理解这背后的机制。

![](img/0629c20054834ad43432bab705e8969b_32.png)

![](img/0629c20054834ad43432bab705e8969b_34.png)

## 组件一：自动编码器 (VAE) 🎨

![](img/0629c20054834ad43432bab705e8969b_36.png)

![](img/0629c20054834ad43432bab705e8969b_37.png)

![](img/0629c20054834ad43432bab705e8969b_38.png)

![](img/0629c20054834ad43432bab705e8969b_39.png)

![](img/0629c20054834ad43432bab705e8969b_40.png)

稳定扩散是一个**潜在扩散模型**。这意味着它不在像素空间操作，而是在一个经过训练的变分自动编码器的**潜在空间**中操作。

![](img/0629c20054834ad43432bab705e8969b_42.png)

![](img/0629c20054834ad43432bab705e8969b_43.png)

![](img/0629c20054834ad43432bab705e8969b_45.png)

![](img/0629c20054834ad43432bab705e8969b_46.png)

上一节我们介绍了基础生成流程，本节中我们来看看第一个核心组件：自动编码器。它的作用是将高分辨率图像压缩成一个信息丰富的低维潜在表示。

![](img/0629c20054834ad43432bab705e8969b_48.png)

![](img/0629c20054834ad43432bab705e8969b_50.png)

以下是编码和解码过程的代码示例：

![](img/0629c20054834ad43432bab705e8969b_52.png)

![](img/0629c20054834ad43432bab705e8969b_53.png)

```python
def encode_image(vae, image_tensor):
    # 编码图像到潜在分布
    with torch.no_grad():
        latent_dist = vae.encode(image_tensor).latent_dist
        latent_sample = latent_dist.sample()
    # 按原始论文缩放因子进行缩放
    latent_sample = latent_sample * 0.18215
    return latent_sample

![](img/0629c20054834ad43432bab705e8969b_55.png)

![](img/0629c20054834ad43432bab705e8969b_56.png)

def decode_latent(vae, latent_sample):
    # 缩放回原始范围
    latent_sample = latent_sample / 0.18215
    # 解码回图像空间
    with torch.no_grad():
        image = vae.decode(latent_sample).sample
    return image
```

![](img/0629c20054834ad43432bab705e8969b_57.png)

![](img/0629c20054834ad43432bab705e8969b_58.png)

![](img/0629c20054834ad43432bab705e8969b_59.png)

![](img/0629c20054834ad43432bab705e8969b_60.png)

![](img/0629c20054834ad43432bab705e8969b_62.png)

![](img/0629c20054834ad43432bab705e8969b_64.png)

这个过程实现了**8倍**的尺寸缩减（例如从512x512到64x64），数据量减少了约64倍，但依然能保留图像的大部分视觉信息。输入图像的尺寸必须是8的倍数。

## 组件二：调度器 (Scheduler) ⏱️

调度器负责控制扩散过程中噪声的添加与移除。在训练时，我们向图像添加噪声，然后让模型尝试预测所添加的噪声。

以下是使用DDPM调度器添加噪声的示例：

```python
from diffusers import DDPMScheduler

# 初始化调度器
scheduler = DDPMScheduler(
    beta_start=0.00085,
    beta_end=0.012,
    beta_schedule="scaled_linear",
    num_train_timesteps=1000
)

# 设置推理步数（通常远小于训练步数）
scheduler.set_timesteps(50)

# 查看对应的时间步和噪声水平（sigma）
timesteps = scheduler.timesteps  # 例如 [999, 957, ... , 0]
sigmas = scheduler.sigmas        # 对应每个时间步的噪声水平
```

![](img/0629c20054834ad43432bab705e8969b_66.png)

![](img/0629c20054834ad43432bab705e8969b_68.png)

噪声添加的数学原理很简单，核心公式是：
`noisy_latents = original_latents + noise * sigma`

![](img/0629c20054834ad43432bab705e8969b_69.png)

![](img/0629c20054834ad43432bab705e8969b_70.png)

![](img/0629c20054834ad43432bab705e8969b_72.png)

![](img/0629c20054834ad43432bab705e8969b_73.png)

![](img/0629c20054834ad43432bab705e8969b_74.png)

我们可以从任意时间步开始生成，这构成了“图生图”功能的基础。例如，从一张已有图像的加噪版本开始去噪，可以保留原图的部分结构和色彩，同时根据新提示词生成新内容。

![](img/0629c20054834ad43432bab705e8969b_76.png)

![](img/0629c20054834ad43432bab705e8969b_77.png)

## 组件三：文本编码器 (Text Encoder) 🔤

![](img/0629c20054834ad43432bab705e8969b_79.png)

![](img/0629c20054834ad43432bab705e8969b_80.png)

![](img/0629c20054834ad43432bab705e8969b_81.png)

![](img/0629c20054834ad43432bab705e8969b_82.png)

![](img/0629c20054834ad43432bab705e8969b_83.png)

![](img/0629c20054834ad43432bab705e8969b_84.png)

![](img/0629c20054834ad43432bab705e8969b_85.png)

文本编码器负责将文本提示转换为模型可以理解的数值表示（嵌入向量）。这个过程分为几个步骤。

![](img/0629c20054834ad43432bab705e8969b_87.png)

![](img/0629c20054834ad43432bab705e8969b_88.png)

![](img/0629c20054834ad43432bab705e8969b_90.png)

上一节我们介绍了噪声调度，本节中我们来看看文本是如何被转换成模型条件的。以下是文本编码的完整流程：

![](img/0629c20054834ad43432bab705e8969b_92.png)

![](img/0629c20054834ad43432bab705e8969b_93.png)

![](img/0629c20054834ad43432bab705e8969b_94.png)

![](img/0629c20054834ad43432bab705e8969b_95.png)

![](img/0629c20054834ad43432bab705e8969b_96.png)

![](img/0629c20054834ad43432bab705e8969b_97.png)

1.  **分词**：将提示词转换为离散的令牌ID序列（例如77个令牌，不足则用填充令牌补足）。
2.  **令牌嵌入**：每个令牌ID通过一个查找表映射为一个768维的向量。
3.  **位置嵌入**：为序列中的每个位置（1到77）添加一个额外的768维位置编码向量，以提供顺序信息。令牌嵌入和位置嵌入直接相加。
4.  **Transformer编码**：将组合后的嵌入向量输入一个Transformer编码器（由多个块堆叠而成），经过自注意力等机制处理，得到最终的**输出嵌入**（即编码器隐藏状态）。

![](img/0629c20054834ad43432bab705e8969b_99.png)

![](img/0629c20054834ad43432bab705e8969b_100.png)

![](img/0629c20054834ad43432bab705e8969b_101.png)

![](img/0629c20054834ad43432bab705e8969b_102.png)

![](img/0629c20054834ad43432bab705e8969b_103.png)

以下是手动模拟这一过程的代码：

![](img/0629c20054834ad43432bab705e8969b_105.png)

![](img/0629c20054834ad43432bab705e8969b_106.png)

```python
# 1. 分词
tokenizer = pipe.tokenizer
text_input = tokenizer(
    prompt,
    padding="max_length",
    max_length=tokenizer.model_max_length,
    truncation=True,
    return_tensors="pt"
)
input_ids = text_input.input_ids.to(device)  # 形状: [1, 77]

![](img/0629c20054834ad43432bab705e8969b_108.png)

![](img/0629c20054834ad43432bab705e8969b_109.png)

![](img/0629c20054834ad43432bab705e8969b_111.png)

# 2. 获取令牌嵌入
token_embeddings = text_encoder.text_model.embeddings.token_embedding(input_ids)

# 3. 获取位置嵌入并相加
position_ids = torch.arange(tokenizer.model_max_length).unsqueeze(0).to(device)
position_embeddings = text_encoder.text_model.embeddings.position_embedding(position_ids)
input_embeddings = token_embeddings + position_embeddings

# 4. 通过Transformer编码器
encoder_outputs = text_encoder.text_model.encoder(
    inputs_embeds=input_embeddings,
    output_hidden_states=True
)
# 获取最终的输出嵌入（通常是最后一层的隐藏状态）
prompt_embeddings = encoder_outputs.hidden_states[-1]  # 形状: [1, 77, 768]
```

![](img/0629c20054834ad43432bab705e8969b_113.png)

![](img/0629c20054834ad43432bab705e8969b_115.png)

理解这个流程的妙处在于，我们可以在不同阶段干预嵌入向量，实现有趣的控制。

![](img/0629c20054834ad43432bab705e8969b_117.png)

![](img/0629c20054834ad43432bab705e8969b_119.png)

### 嵌入操控技巧

![](img/0629c20054834ad43432bab705e8969b_121.png)

![](img/0629c20054834ad43432bab705e8969b_122.png)

![](img/0629c20054834ad43432bab705e8969b_124.png)

![](img/0629c20054834ad43432bab705e8969b_125.png)

![](img/0629c20054834ad43432bab705e8969b_126.png)

![](img/0629c20054834ad43432bab705e8969b_127.png)

以下是几种通过修改嵌入向量来控制生成的方法：

![](img/0629c20054834ad43432bab705e8969b_129.png)

![](img/0629c20054834ad43432bab705e8969b_130.png)

*   **令牌替换**：直接替换特定单词（如“puppy”）的令牌嵌入为另一个单词（如“cat”）的嵌入，生成的图像内容会随之改变。
*   **令牌混合**：将两个令牌的嵌入进行线性插值（如`embedding = α * embed_puppy + (1-α) * embed_skunk`），可以生成混合概念的图像。
*   **文本反转**：这是最强大的应用之一。我们可以为模型训练一个全新的“概念”（如一种特定绘画风格）对应的嵌入向量，并将其作为一个特殊令牌使用。社区已经贡献了成千上万个这样的概念嵌入。
*   **提示词嵌入混合**：在文本编码器的最终输出阶段，将两个不同提示词（如“a mouse”和“a leopard”）的输出嵌入进行混合，可以生成融合两者特征的图像。

## 组件四：UNet扩散模型 🧬

![](img/0629c20054834ad43432bab705e8969b_132.png)

![](img/0629c20054834ad43432bab705e8969b_133.png)

![](img/0629c20054834ad43432bab705e8969b_134.png)

![](img/0629c20054834ad43432bab705e8969b_135.png)

UNet是扩散模型的核心，它负责预测添加到潜在表示中的噪声。

上一节我们探讨了文本编码，本节中我们聚焦于执行去噪任务的UNet模型。UNet的调用签名如下，它接收噪声潜在、时间步和文本嵌入作为输入：

```python
# 模型预测噪声
noise_pred = unet(
    noisy_latents,          # 当前噪声潜在表示
    timestep,               # 当前时间步
    encoder_hidden_states=prompt_embeddings  # 文本条件
).sample

# 预测的“去噪”潜在可以通过以下公式计算：
pred_original_sample = noisy_latents - sigma * noise_pred
```

在采样循环中，我们不会一步就减去所有预测的噪声，而是逐步进行。可视化每一步的`noisy_latents`和`pred_original_sample`，可以清晰地看到图像从模糊噪声逐渐变得清晰的过程。

### 无分类器引导

为了生成更贴合文本提示的图像，稳定扩散使用了**无分类器引导**技术。其核心思想是同时计算有条件（带提示词）和无条件（空提示）的噪声预测，然后按以下公式进行引导：

`final_noise_pred = noise_pred_uncond + guidance_scale * (noise_pred_cond - noise_pred_uncond)`

`guidance_scale`（引导尺度）控制着向有条件预测方向推进的强度。较高的引导尺度通常能生成更符合提示词但多样性可能降低的图像。

## 采样算法原理 📈

为什么我们需要迭代采样而不是一步到位？我们可以从两个角度理解：

1.  **ODE求解视角**：扩散过程可以表述为一个随机微分方程（SDE），而去噪则是求解对应的逆向常微分方程（ODE）。由于这个ODE无法一步精确求解，我们需要使用数值方法（如欧拉法）进行多步近似。更高级的采样器（如二阶求解器、线性多步法）通过估计梯度变化或利用历史信息，可以用更少的步数获得更好的结果。
2.  **优化视角**：将生成过程视为一个优化问题。我们的目标是找到一个潜在点，使得模型预测的噪声尽可能小（即看起来像一张真实图像）。我们从随机噪声点开始，将模型预测的噪声视为“梯度”，使用优化器（带有学习率、动量等）逐步更新潜在表示，直至收敛到一个低“噪声损失”的点。

两种视角都解释了迭代采样的必要性：单步预测会得到模糊、平均的结果，而多步迭代能逐步细化，得到清晰、高质量的图像。

## 高级控制：引导生成 🎯

除了文本控制，我们还可以通过额外损失函数来引导生成过程，例如控制颜色、风格或形状。

以下是实现颜色引导（使图像偏蓝）的示例步骤：

1.  在采样循环中，定期（如每5步）启用潜在张量的梯度计算。
2.  计算当前步预测的去噪潜在，并通过VAE解码为图像。
3.  在图像空间计算自定义损失（例如，蓝色通道值与目标值的均方误差）。
4.  计算该损失对噪声潜在张量的梯度。
5.  按照梯度方向更新潜在张量，以减小损失。

```python
# 简单蓝色损失示例
def blue_loss(images):
    # 计算图像蓝色通道与目标值0.9的差异
    return (images[:,2] - 0.9).mean().abs() # images shape: [B, C, H, W]

# 在采样循环中
if i % 5 == 0:
    latents.requires_grad_(True)
    # 获取预测的原始样本并解码
    pred_original_sample = latents - sigma * noise_pred
    decoded_image = decode_latent(vae, pred_original_sample)
    loss = blue_loss(decoded_image) * guidance_scale_extra
    # 计算梯度并更新latents
    grad = torch.autograd.grad(loss, latents)[0]
    latents = latents - grad * (sigma**2)
    latents.requires_grad_(False)
```

这种方法非常强大，可以结合CLIP模型、分类器或任何可微分的损失函数，为生成过程注入丰富的控制信号。

## 总结 🎉

![](img/0629c20054834ad43432bab705e8969b_137.png)

![](img/0629c20054834ad43432bab705e8969b_138.png)

![](img/0629c20054834ad43432bab705e8969b_139.png)

本节课中我们一起深入学习了稳定扩散模型的内部机制：

![](img/0629c20054834ad43432bab705e8969b_141.png)

1.  **VAE**：在潜在空间进行高效操作，实现图像压缩与重建。
2.  **调度器**：管理噪声添加与移除的节奏，是“图生图”等功能的基础。
3.  **文本编码器**：将文本转换为条件嵌入，并可通过嵌入操控实现文本反转、概念混合等高级控制。
4.  **UNet**：核心去噪模型，预测噪声以逐步净化潜在表示。
5.  **无分类器引导**：通过结合有条件与无条件预测，增强文本跟随能力。
6.  **采样原理**：从ODE求解或优化视角理解迭代采样的必要性。
7.  **引导生成**：通过外部损失函数对生成过程进行细粒度控制。

![](img/0629c20054834ad43432bab705e8969b_143.png)

![](img/0629c20054834ad43432bab705e8969b_144.png)

![](img/0629c20054834ad43432bab705e8969b_145.png)

![](img/0629c20054834ad43432bab705e8969b_147.png)

![](img/0629c20054834ad43432bab705e8969b_148.png)

![](img/0629c20054834ad43432bab705e8969b_150.png)

希望本教程能帮助你不仅学会使用稳定扩散，更能理解其原理，并激发你创造更多有趣的应用。