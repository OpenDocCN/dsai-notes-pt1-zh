# 深度学习基础到稳定扩散模型：18：从零实现扩散模型 🧠

![](img/3246ceb323b7589c410e1a8ed89ee30c_1.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_3.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_5.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_7.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_8.png)

在本节课中，我们将学习如何从零开始构建一个无条件和有条件的扩散模型。我们将从构建一个基础的U-Net架构开始，逐步加入时间步嵌入和注意力机制，最终实现一个能够根据类别标签生成图像的模型。本节课的核心是理解扩散模型的核心组件及其实现细节。

---

![](img/3246ceb323b7589c410e1a8ed89ee30c_10.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_11.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_13.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_15.png)

## 从零构建无条件的扩散模型 🏗️

![](img/3246ceb323b7589c410e1a8ed89ee30c_17.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_18.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_20.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_22.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_24.png)

上一节我们学习了扩散模型的基本原理和噪声调度。本节中，我们将动手构建一个无条件的扩散模型，其核心是一个U-Net架构。

### 基础组件：预激活卷积与残差块

首先，我们定义模型的基础构建块。我们将使用预激活卷积（Pre-activation Convolution），即先进行归一化和激活，再进行卷积操作。

![](img/3246ceb323b7589c410e1a8ed89ee30c_26.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_28.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_30.png)

**预激活卷积的代码表示：**
```python
class PreActConv(nn.Module):
    def __init__(self, ni, nf, ks=3, stride=1):
        super().__init__()
        self.norm = nn.GroupNorm(1, ni)  # 使用组归一化
        self.act = nn.SiLU()  # SiLU激活函数，也称为Swish
        self.conv = nn.Conv2d(ni, nf, ks, stride=stride, padding=ks//2)
    def forward(self, x):
        return self.conv(self.act(self.norm(x)))
```

接下来是基础的残差块（ResNet Block）。与之前不同，这个残差块不包含下采样选项，下采样将在后续的“下采样块”中单独处理。

![](img/3246ceb323b7589c410e1a8ed89ee30c_32.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_34.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_36.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_38.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_40.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_42.png)

**基础残差块的代码表示：**
```python
class UnitResBlock(nn.Module):
    def __init__(self, ni, nf):
        super().__init__()
        self.conv1 = PreActConv(ni, nf)
        self.conv2 = PreActConv(nf, nf)
        # 如果输入输出通道数不同，需要一个1x1卷积来调整维度
        self.idconv = nn.Identity() if ni==nf else nn.Conv2d(ni, nf, 1)
    def forward(self, x):
        return self.conv2(self.conv1(x)) + self.idconv(x)
```

![](img/3246ceb323b7589c410e1a8ed89ee30c_44.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_46.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_48.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_50.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_52.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_54.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_55.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_57.png)

### 实现U-Net的跳跃连接：保存模块

![](img/3246ceb323b7589c410e1a8ed89ee30c_59.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_60.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_62.png)

U-Net的关键特性是编码器（下采样）和解码器（上采样）之间的跳跃连接。为了优雅地实现这一点，我们创建一个“保存模块”（Save Module）的混入类（Mixin），它能在前向传播时自动保存激活值。

![](img/3246ceb323b7589c410e1a8ed89ee30c_64.png)

**保存模块混入类的代码表示：**
```python
class SaveModule:
    def forward(self, *args, **kwargs):
        self.saved = super().forward(*args, **kwargs)
        return self.saved
```

通过多重继承，我们可以创建能自动保存输出的卷积层和残差块。

**带保存功能的卷积和残差块：**
```python
class SavedConv(SaveModule, nn.Conv2d): pass
class SavedResBlock(SaveModule, UnitResBlock): pass
```

![](img/3246ceb323b7589c410e1a8ed89ee30c_66.png)

### 构建下采样块与上采样块

![](img/3246ceb323b7589c410e1a8ed89ee30c_68.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_70.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_72.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_73.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_74.png)

下采样块（DownBlock）由一系列保存残差块组成，最后可选地添加一个步长为2的卷积进行下采样。

![](img/3246ceb323b7589c410e1a8ed89ee30c_76.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_78.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_80.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_82.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_84.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_86.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_88.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_89.png)

上采样块（UpBlock）与下采样块结构类似，但方向相反。它接收来自下采样路径的保存激活列表，并在每个残差块前通过拼接（concatenation）将其与当前激活结合。

![](img/3246ceb323b7589c410e1a8ed89ee30c_91.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_93.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_94.png)

**下采样块与上采样块的核心逻辑：**
```python
# 下采样块：多个SavedResBlock + 可选的Stride=2卷积
# 上采样块：接收保存的激活列表，与当前激活拼接后输入普通残差块 + 可选的最近邻上采样层
```

![](img/3246ceb323b7589c410e1a8ed89ee30c_96.png)

### 组装完整的U-Net模型

现在，我们可以将下采样块、中间块和上采样块组合成完整的U-Net。模型首先通过一个卷积增加通道数，然后经过一系列下采样块，一个中间残差块，再经过一系列上采样块，最后通过一个卷积将通道数映射回图像通道（例如3）。

![](img/3246ceb323b7589c410e1a8ed89ee30c_98.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_100.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_102.png)

**U-Net前向传播流程：**
1.  初始卷积。
2.  遍历下采样块序列，保存每一层的激活。
3.  通过中间残差块。
4.  遍历上采样块序列，每次从保存的激活列表中取出对应的激活进行拼接。
5.  最终卷积输出。

![](img/3246ceb323b7589c410e1a8ed89ee30c_104.png)

---

![](img/3246ceb323b7589c410e1a8ed89ee30c_106.png)

## 引入时间步嵌入 ⏱️

![](img/3246ceb323b7589c410e1a8ed89ee30c_108.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_109.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_111.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_113.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_115.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_117.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_118.png)

上一节我们构建了基础的U-Net，但扩散模型需要知道当前处于去噪过程的哪个时间步。本节中，我们来看看如何将时间信息嵌入到网络中。

![](img/3246ceb323b7589c410e1a8ed89ee30c_120.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_122.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_123.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_125.png)

### 正弦时间步嵌入

![](img/3246ceb323b7589c410e1a8ed89ee30c_127.png)

我们使用正弦嵌入（Sinusoidal Embedding）将连续的时间步（或噪声水平σ）转换为向量表示。其核心思想是生成一组频率不同的正弦和余弦波，确保相近的时间步有相似的嵌入，而不同的时间步嵌入也不同。

**正弦嵌入的公式：**
给定时间步 `t`，嵌入维度 `d`，最大周期 `max_period`：
对于 `i` 在 `[0, 1, ..., d/2-1]` 范围内：
*   频率 = `exp( -log(max_period) * i / (d/2) )`
*   嵌入向量的第 `2i` 个元素 = `sin(t * 频率)`
*   嵌入向量的第 `2i+1` 个元素 = `cos(t * 频率)`

**代码实现：**
```python
def sinusoidal_embedding(t, dim, max_period=10000):
    half = dim // 2
    freqs = torch.exp(-math.log(max_period) * torch.arange(half, dtype=torch.float32) / half).to(t.device)
    args = t[:, None].float() * freqs[None]
    return torch.cat([torch.sin(args), torch.cos(args)], dim=-1)
```

### 将时间嵌入整合到残差块中

我们创建一个新的残差块（EmbResBlock），它在两个卷积之间融入时间步信息。具体做法是：将时间嵌入通过一个小型MLP投影，然后分割为缩放（scale）和偏移（shift）两部分，分别作用于第一个卷积后的激活值。

**带时间嵌入的残差块前向传播步骤：**
1.  对输入进行第一个卷积。
2.  将时间嵌入投影并分割为 `scale` 和 `shift`。
3.  对卷积后的激活值进行缩放和偏移：`x = x * (1 + scale) + shift`。
4.  进行第二个卷积。
5.  与快捷连接（shortcut）相加。

![](img/3246ceb323b7589c410e1a8ed89ee30c_129.png)

---

## 加入注意力机制 🔍

上一节我们为模型添加了时间感知能力。本节中，我们将引入注意力机制，使模型能够在图像的远距离区域之间建立联系，这对于生成结构一致的图像很重要。

### 自注意力机制原理

在卷积网络中，一个像素的感受野有限。自注意力允许每个像素与图像中所有其他像素进行交互，通过加权平均的方式聚合全局信息。

![](img/3246ceb323b7589c410e1a8ed89ee30c_131.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_133.png)

**单头自注意力的计算步骤（稳定扩散风格）：**
1.  将输入 `x`（形状 `[batch, channels, height, width]`）展平为 `[batch, height*width, channels]`。
2.  使用三个独立的线性投影层，得到查询（Query，Q）、键（Key，K）、值（Value，V）。
3.  计算注意力权重：`attn = softmax( (Q @ K.transpose(-2, -1)) / sqrt(d) )`，其中 `d` 是通道数。
4.  使用注意力权重对V进行加权求和：`out = attn @ V`。
5.  将输出投影回原始通道数，并重塑为原始空间维度。
6.  最终输出为 `x + out`，形成一个残差连接。

![](img/3246ceb323b7589c410e1a8ed89ee30c_135.png)

### 多头注意力

![](img/3246ceb323b7589c410e1a8ed89ee30c_137.png)

单头注意力可能只关注一种模式。多头注意力将通道分成多组（头），每组独立进行注意力计算，从而让模型同时关注不同方面的信息。

![](img/3246ceb323b7589c410e1a8ed89ee30c_139.png)

**实现多头注意力的关键技巧：**
使用 `rearrange` 操作（来自 `einops` 库）将“批次数”维度与“头数”维度合并，从而将多头计算转换为批处理计算，简化了实现。

**使用 `einops.rearrange` 实现多头：**
```python
from einops import rearrange
# 将输入从 [batch, seq, channels] 重排为 [batch*heads, seq, channels_per_head]
q = rearrange(q, ‘b s (h d) -> (b h) s d’, h=n_heads)
# 注意力计算...
# 计算完成后，再重排回来
out = rearrange(out, ‘(b h) s d -> b s (h d)’, h=n_heads)
```

### 将注意力整合到U-Net中

由于注意力计算复杂度与序列长度（即像素数量）的平方成正比，在图像分辨率很高时（如早期层）计算开销巨大。因此，我们通常只在U-Net的较低分辨率层（例如，在几次下采样之后）添加注意力模块。

![](img/3246ceb323b7589c410e1a8ed89ee30c_141.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_143.png)

在我们的实现中，可以为 `EmbResBlock` 添加一个可选的注意力层，并在构建U-Net时指定从第几个下采样/上采样块开始使用注意力。

---

![](img/3246ceb323b7589c410e1a8ed89ee30c_145.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_147.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_149.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_151.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_152.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_154.png)

## 实现条件扩散模型 🏷️

![](img/3246ceb323b7589c410e1a8ed89ee30c_156.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_158.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_160.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_162.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_164.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_165.png)

到目前为止，我们构建的模型都是无条件的。本节中，我们将扩展模型，使其能够根据类别标签生成特定类型的图像（例如，“生成一件T恤”）。

![](img/3246ceb323b7589c410e1a8ed89ee30c_167.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_169.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_171.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_173.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_175.png)

### 添加条件嵌入

![](img/3246ceb323b7589c410e1a8ed89ee30c_176.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_178.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_179.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_180.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_181.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_182.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_183.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_185.png)

条件信息（如类别标签）也需要被嵌入到网络中。处理方式与时间步嵌入非常相似：
1.  为类别标签创建一个嵌入层（`nn.Embedding`），将类别索引映射为向量。
2.  将类别嵌入向量与时间步嵌入向量**相加**，形成一个联合条件向量。
3.  将这个联合条件向量输入到每个 `EmbResBlock` 中，方式与之前的时间嵌入完全相同。

![](img/3246ceb323b7589c410e1a8ed89ee30c_186.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_188.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_190.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_191.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_193.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_195.png)

**在U-Net前向传播中整合条件：**
```python
def forward(self, x, t, y):
    # t: 时间步， y: 类别标签
    t_emb = self.time_embedding(t)
    y_emb = self.label_embedding(y)
    cond = t_emb + y_emb # 联合条件
    # 后续将 cond 传入各个 EmbResBlock
```

![](img/3246ceb323b7589c410e1a8ed89ee30c_197.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_198.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_200.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_202.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_204.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_206.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_208.png)

### 训练与条件采样

**训练：** 数据加载器现在需要提供三元组 `(噪声图像, 时间步, 类别标签)`，损失函数保持不变。

**采样：** 在推理时，我们需要指定想要生成的类别。只需将目标类别标签转换为嵌入，并与时间步嵌入结合，然后输入给训练好的模型，模型就会朝着生成该类别图像的方向进行去噪。

![](img/3246ceb323b7589c410e1a8ed89ee30c_210.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_212.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_214.png)

---

![](img/3246ceb323b7589c410e1a8ed89ee30c_216.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_218.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_220.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_222.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_224.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_226.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_228.png)

## 总结 📚

![](img/3246ceb323b7589c410e1a8ed89ee30c_230.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_232.png)

本节课中，我们一起学习了如何从零开始构建一个完整的扩散模型。

![](img/3246ceb323b7589c410e1a8ed89ee30c_234.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_236.png)

1.  **我们首先构建了无条件的U-Net**，使用了预激活卷积、残差块，并通过巧妙的“保存模块”设计实现了跳跃连接。
2.  **接着引入了时间步嵌入**，使用正弦函数将连续时间编码为向量，并通过缩放和偏移操作将其融入残差块，使模型感知去噪进度。
3.  **然后加入了注意力机制**，解释了自注意力和多头注意力的原理与实现，并将其整合到U-Net的深层，以捕获图像中的长程依赖关系。
4.  **最后实现了条件扩散模型**，通过将类别嵌入与时间嵌入相加，使模型能够根据文本或类别标签生成特定内容的图像。

![](img/3246ceb323b7589c410e1a8ed89ee30c_238.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_240.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_242.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_244.png)

![](img/3246ceb323b7589c410e1a8ed89ee30c_246.png)

至此，我们已经复现了稳定扩散模型的核心架构（除了CLIP文本编码器和潜在扩散部分）。我们拥有了一个功能完备的、可以无条件或有条件生成图像的扩散模型。