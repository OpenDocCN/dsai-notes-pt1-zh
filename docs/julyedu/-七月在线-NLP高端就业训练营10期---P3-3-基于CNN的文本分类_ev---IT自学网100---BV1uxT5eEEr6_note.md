# 【七月在线】NLP高端就业训练营10期 - P3：3. 基于CNN的文本分类教程 📚

![](img/30cc1fd08333411df1f429aca431b8df_0.png)

![](img/30cc1fd08333411df1f429aca431b8df_2.png)

![](img/30cc1fd08333411df1f429aca431b8df_4.png)

在本节课中，我们将学习如何使用卷积神经网络来处理自然语言处理任务，特别是文本分类。我们将从Transformer模型的补充知识开始，然后介绍如何阅读学术论文，最后深入探讨CNN的原理及其在文本处理中的应用。

---

## 第一部分：Transformer模型补充 🧠

上一节我们介绍了Transformer模型的核心组件Self-Attention。本节中，我们来看看Transformer编码器中的其他重要结构。

### 残差连接与层归一化

在Self-Attention层之后，Transformer编码器会进行残差连接和层归一化操作。

**残差连接** 的核心思想是让训练变得更加简单。它通过添加一条“捷径”，将层的输入直接加到输出上。这有助于缓解深层网络中的梯度消失问题。

**公式表示**：
`输出 = 层输入 + 层输出`

**层归一化** 的目的是将数据转化为均值为0、方差为1的分布，有助于稳定训练过程，防止梯度消失或爆炸。

**公式表示**：
`LayerNorm(x) = α * ( (x - μ) / √(σ² + ε) ) + β`
其中，`μ`是均值，`σ²`是方差，`ε`是一个极小值防止除零，`α`和`β`是可学习的参数。

![](img/30cc1fd08333411df1f429aca431b8df_6.png)

![](img/30cc1fd08333411df1f429aca431b8df_8.png)

![](img/30cc1fd08333411df1f429aca431b8df_10.png)

### 位置编码

![](img/30cc1fd08333411df1f429aca431b8df_12.png)

![](img/30cc1fd08333411df1f429aca431b8df_14.png)

由于Transformer是并行处理序列的，它本身不具备序列的顺序信息。因此，需要引入**位置编码**来为每个词的位置提供信息。

在原始Transformer中，位置编码使用正弦和余弦函数计算：
`PE(pos, 2i) = sin(pos / 10000^(2i/d_model))`
`PE(pos, 2i+1) = cos(pos / 10000^(2i/d_model))`
其中，`pos`是位置，`i`是维度索引。

在BERT等模型中，位置编码通常通过学习一个嵌入矩阵来获得。

### 解码器中的掩码机制

在解码器部分，为了防止模型在预测当前词时“看到”未来的词信息（即标签泄漏），引入了**掩码机制**。

具体做法是，在计算注意力权重矩阵后，将其与一个下三角矩阵（未来位置为0）相乘，从而将未来位置的权重置零。

---

## 第二部分：如何阅读学术论文 📄

![](img/30cc1fd08333411df1f429aca431b8df_16.png)

![](img/30cc1fd08333411df1f429aca431b8df_18.png)

![](img/30cc1fd08333411df1f429aca431b8df_20.png)

在深入技术细节之前，掌握高效阅读论文的方法至关重要。以下是阅读NLP领域论文的一些建议。

![](img/30cc1fd08333411df1f429aca431b8df_22.png)

### 论文查找与筛选

![](img/30cc1fd08333411df1f429aca431b8df_24.png)

*   **关注经典**：初学者应优先阅读领域内的经典论文（如Transformer、TextCNN），打好基础。
*   **利用资源**：可以使用如 **arXiv**、**Papers with Code**、**PaperWeekly** 等网站或社区来发现和跟踪论文。
*   **管理工具**：推荐使用 **ReadPaper** 等在线工具来管理和阅读论文，它能高亮重要内容并提取图表。

![](img/30cc1fd08333411df1f429aca431b8df_26.png)

![](img/30cc1fd08333411df1f429aca431b8df_28.png)

![](img/30cc1fd08333411df1f429aca431b8df_30.png)

![](img/30cc1fd08333411df1f429aca431b8df_32.png)

![](img/30cc1fd08333411df1f429aca431b8df_34.png)

![](img/30cc1fd08333411df1f429aca431b8df_36.png)

### 阅读顺序与重点

![](img/30cc1fd08333411df1f429aca431b8df_38.png)

![](img/30cc1fd08333411df1f429aca431b8df_40.png)

![](img/30cc1fd08333411df1f429aca431b8df_42.png)

![](img/30cc1fd08333411df1f429aca431b8df_44.png)

![](img/30cc1fd08333411df1f429aca431b8df_46.png)

阅读一篇论文时，建议遵循以下顺序：

![](img/30cc1fd08333411df1f429aca431b8df_48.png)

![](img/30cc1fd08333411df1f429aca431b8df_50.png)

1.  **标题与摘要**：快速了解论文的核心贡献。
2.  **引言**：理解研究背景和动机。
3.  **模型部分**：这是论文的核心，需仔细理解其创新点与结构。
4.  **实验部分**：关注实验设置、数据集和结果分析，特别是与基线模型的对比。
5.  **结论**：总结工作并展望未来。

![](img/30cc1fd08333411df1f429aca431b8df_52.png)

![](img/30cc1fd08333411df1f429aca431b8df_54.png)

![](img/30cc1fd08333411df1f429aca431b8df_56.png)

![](img/30cc1fd08333411df1f429aca431b8df_58.png)

对于复现，首先查看作者是否开源了代码。复现时要注意论文描述与代码实现可能存在的差异。

![](img/30cc1fd08333411df1f429aca431b8df_60.png)

---

![](img/30cc1fd08333411df1f429aca431b8df_62.png)

## 第三部分：卷积神经网络基础 🖼️

![](img/30cc1fd08333411df1f429aca431b8df_64.png)

上一节我们介绍了处理序列的RNN和Transformer。本节中，我们来看看如何利用CNN来捕捉文本中的局部特征（如短语）。

### 什么是卷积？

卷积是一种数学运算，在图像处理中常用来提取特征（如边缘）。在离散情况下，可以理解为对局部区域进行加权求和。

**文本中的卷积**：在文本上应用卷积，意味着使用一个滑动窗口（卷积核）在词向量序列上移动，每次计算窗口内几个连续词的组合特征。

### CNN在文本上的应用

以下是CNN处理文本的关键步骤：

1.  **输入表示**：句子被表示为词向量矩阵，形状为 `[序列长度, 词向量维度]`。
2.  **卷积操作**：使用多个不同宽度的卷积核进行扫描。**卷积核的宽度**（如2, 3, 4）决定了每次考虑几个词，**高度**必须与词向量维度相同。
3.  **填充**：为了保持卷积后序列长度不变，通常在序列首尾进行补零操作。
4.  **多通道**：使用多个卷积核可以提取不同方面的特征。
5.  **池化**：常用**最大池化**，从每个卷积核产生的特征图中提取最重要的值（如全局最大值）。
6.  **拼接与分类**：将所有池化后的特征拼接成一个向量，输入全连接层进行分类。

**维度变化公式**（假设输入序列长度为 `W`，卷积核大小为 `F`，步长为 `S`，填充为 `P`）：
`输出长度 = (W + 2P - F) / S + 1`

### CNN处理文本的优缺点

*   **优点**：能有效提取N-gram（短语）特征；模型结构简单，参数较少，训练速度快。
*   **缺点**：**感受野有限**，难以捕获长距离依赖关系；本身不具备序列顺序信息，通常需要结合位置编码。

---

## 第四部分：CNN文本分类模型实战 ⚙️

理论之后，我们通过一个经典的TextCNN模型来实践文本分类。

### TextCNN模型结构

TextCNN是用于文本分类的经典CNN模型，其结构非常简单：

1.  词嵌入层将单词转换为向量。
2.  使用多个不同尺寸（如2,3,4）的卷积核在嵌入矩阵上进行卷积，提取不同粒度的特征。
3.  对每个卷积核的输出进行全局最大池化，得到一个标量。
4.  将所有池化结果拼接成一个特征向量。
5.  将特征向量输入全连接层，最后通过Softmax得到分类概率。

### 使用PyTorch实现TextCNN

以下是使用PyTorch框架构建TextCNN模型的核心代码：

![](img/30cc1fd08333411df1f429aca431b8df_66.png)

![](img/30cc1fd08333411df1f429aca431b8df_68.png)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class TextCNN(nn.Module):
    def __init__(self, vocab_size, embed_size, num_classes=2):
        super(TextCNN, self).__init__()
        self.embedding = nn.Embedding(vocab_size, embed_size)
        
        # 定义三种不同宽度的卷积核，每种2个
        self.conv1 = nn.Conv2d(1, 2, (2, embed_size)) # 窗口大小为2
        self.conv2 = nn.Conv2d(1, 2, (3, embed_size)) # 窗口大小为3
        self.conv3 = nn.Conv2d(1, 2, (4, embed_size)) # 窗口大小为4
        
        self.dropout = nn.Dropout(0.5)
        # 全连接层：输入特征数 = 卷积核种类数 * 每种核的数量
        self.fc = nn.Linear(2*3, num_classes) 

    def forward(self, x):
        # x: [batch_size, seq_len]
        x = self.embedding(x) # [batch_size, seq_len, embed_size]
        x = x.unsqueeze(1) # 增加通道维 [batch_size, 1, seq_len, embed_size]
        
        # 卷积并激活
        x1 = F.relu(self.conv1(x)).squeeze(3) # [batch_size, 2, seq_len-1]
        x2 = F.relu(self.conv2(x)).squeeze(3) # [batch_size, 2, seq_len-2]
        x3 = F.relu(self.conv3(x)).squeeze(3) # [batch_size, 2, seq_len-3]
        
        # 池化
        x1 = F.max_pool1d(x1, x1.size(2)).squeeze(2) # [batch_size, 2]
        x2 = F.max_pool1d(x2, x2.size(2)).squeeze(2)
        x3 = F.max_pool1d(x3, x3.size(2)).squeeze(2)
        
        # 拼接
        x = torch.cat((x1, x2, x3), dim=1) # [batch_size, 6]
        x = self.dropout(x)
        out = self.fc(x) # [batch_size, num_classes]
        return out
```

**代码说明**：
*   `nn.Embedding`：将单词索引映射为稠密向量。
*   `nn.Conv2d`：定义二维卷积层。在文本中，我们将其视为一维卷积，因此卷积核高度与词向量维度相同。
*   `F.max_pool1d`：进行一维最大池化，提取每个特征图的最显著特征。
*   `torch.cat`：将不同卷积核提取的特征拼接起来。
*   `nn.Linear`：最后的全连接分类层。

![](img/30cc1fd08333411df1f429aca431b8df_70.png)

![](img/30cc1fd08333411df1f429aca431b8df_71.png)

在实际训练中，你需要准备数据集（如IMDB影评）、构建词汇表、设置数据加载器，并定义损失函数和优化器进行训练。

![](img/30cc1fd08333411df1f429aca431b8df_73.png)

![](img/30cc1fd08333411df1f429aca431b8df_75.png)

![](img/30cc1fd08333411df1f429aca431b8df_77.png)

![](img/30cc1fd08333411df1f429aca431b8df_79.png)

---

## 总结 🎯

本节课中我们一起学习了：
1.  **Transformer的补充知识**：包括残差连接、层归一化、位置编码以及解码器的掩码机制。
2.  **阅读学术论文的方法**：从查找、筛选到重点阅读模型与实验部分。
3.  **卷积神经网络的基础**：理解了卷积的概念及其在文本处理中的应用方式。
4.  **TextCNN模型实战**：掌握了使用PyTorch构建一个用于文本分类的简单CNN模型。

![](img/30cc1fd08333411df1f429aca431b8df_81.png)

![](img/30cc1fd08333411df1f429aca431b8df_83.png)

![](img/30cc1fd08333411df1f429aca431b8df_85.png)

CNN为文本处理提供了一种捕捉局部特征的高效方式，特别适合短文本分类任务。理解其原理并能够动手实现，是构建更复杂NLP模型的重要基础。