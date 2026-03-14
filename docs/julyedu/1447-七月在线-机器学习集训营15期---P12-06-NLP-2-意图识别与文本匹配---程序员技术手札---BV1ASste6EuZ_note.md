# 课程12：NLP模型构建与意图识别 🧠

![](img/68efa51e068fee303461772d91f58c31_0.png)

![](img/68efa51e068fee303461772d91f58c31_2.png)

在本节课中，我们将学习如何从零开始构建一个NLP模型，并重点探讨用于意图识别的文本分类模型。课程内容分为三个主要部分：NLP模型的基础构建流程、经典的文本分类模型（特别是TextCNN），以及Transformer模型的核心概念——自注意力机制。

***

## 第一部分：如何从零搭建NLP模型 🏗️

上一节我们介绍了智能问答系统的整体框架。本节中，我们来看看构建一个NLP深度学习模型需要哪些基本组件和步骤。

一个NLP模型的输入通常需要经过精心设计，主要包括以下几个方面：

### 1. 输入形式：字向量 vs. 词向量

![](img/68efa51e068fee303461772d91f58c31_4.png)

模型的输入需要将文本转换为数值化的向量表示，即嵌入（Embedding）。这主要分为两种形式：

![](img/68efa51e068fee303461772d91f58c31_6.png)

*   **字向量（Char Embedding）**：将句子按字进行划分。
    *   **优势**：词典规模小（例如，中文常用字约2万个），内存占用少，不易出现未登录词（OOV）问题。
    *   **劣势**：语义表达能力通常弱于词向量。
*   **词向量（Word Embedding）**：将句子按词进行划分。
    *   **优势**：语义表达能力更强，因为词通常由多个字组成，承载了更丰富的语义信息。
    *   **劣势**：受分词工具效果影响大；词典规模庞大（可能达10万甚至20万词），易导致OOV问题，且One-Hot编码会消耗大量显存。

![](img/68efa51e068fee303461772d91f58c31_8.png)

![](img/68efa51e068fee303461772d91f58c31_10.png)

![](img/68efa51e068fee303461772d91f58c31_12.png)

在实际应用中，可以同时使用字向量和词向量，例如通过两个独立的嵌入层分别处理，然后将它们的输出进行融合（如相加或拼接）。

![](img/68efa51e068fee303461772d91f58c31_14.png)

### 2. 嵌入向量的初始化方式

在构建模型的嵌入层时，有三种常见的初始化方式：

*   **动态字/词向量**：使用预训练好的向量（如Word2Vec）初始化嵌入矩阵，并在模型反向传播（BP）过程中更新该矩阵。
*   **静态字/词向量**：同样使用预训练向量初始化嵌入矩阵，但在BP过程中**不更新**该矩阵，只更新模型后续层的参数。
*   **随机字/词向量**：不进行预训练，直接随机初始化嵌入矩阵，并在BP过程中更新它。

### 3. 引入先验特征

![](img/68efa51e068fee303461772d91f58c31_16.png)

![](img/68efa51e068fee303461772d91f58c31_18.png)

除了文本本身的向量表示，我们还可以加入人工提取的特征，这类似于机器学习中的特征工程。

![](img/68efa51e068fee303461772d91f58c31_20.png)

![](img/68efa51e068fee303461772d91f58c31_22.png)

以下是几个例子：
*   在**文本匹配**任务中，可以加入两个句子的共有词数量、文本长度差、是否包含特定关键词等特征。
*   在**情感分析**任务中，可以加入是否出现强烈情感倾向词等特征。

![](img/68efa51e068fee303461772d91f58c31_24.png)

![](img/68efa51e068fee303461772d91f58c31_26.png)

这些特征可以作为额外的输入，与文本向量拼接后一同输入模型。

### 4. 构建词典

![](img/68efa51e068fee303461772d91f58c31_28.png)

![](img/68efa51e068fee303461772d91f58c31_30.png)

构建模型的第一步是创建词典，它将字或词映射到唯一的索引ID。流程如下：
1.  **确定输入形式**：明确使用分词还是分字，并确保训练和预测时使用相同的分词器。
2.  **读取语料与分词**：加载所有训练文本，并进行分词/分字处理。
3.  **统计词频与排序**：统计每个词的出现频率，并按照词频从高到低排序。
4.  **添加特殊标记**：在词典开头加入必要的特殊标记符。
5.  **生成词典文件**：将高频词（例如，词频大于5）及其索引写入文件。

词典中通常包含以下四个关键的特殊标记位：
*   **`[PAD]`**：填充标记。用于将不同长度的句子补齐到同一固定长度，以便批量处理。
*   **`[UNK]`**：未知词标记。用于替换那些未出现在词典中的词。
*   **`[CLS]`**：起始标记。在如BERT等模型中用于表示序列的开始。
*   **`[SEP]`**：分隔/终止标记。用于分隔句子或表示序列结束。

### 5. 统一输入长度

为了进行批量训练，需要将所有句子处理成相同长度。假设最大序列长度设为 `max_len`。
*   **短句填充**：对于长度小于 `max_len` 的句子，在其末尾（或开头）添加 `[PAD]` 标记直至达到 `max_len`。
*   **长句截断**：对于长度超过 `max_len` 的句子，进行截断。可以截取前 `max_len` 个词、后 `max_len` 个词，或前后各取一部分进行拼接。

### 6. 使用数据生成器

当语料数据量很大时，一次性加载所有数据可能耗尽内存。因此，我们使用数据生成器（Data Generator）进行小批量（batch）加载。

在PyTorch中，这通过组合 `torch.utils.data.Dataset` 和 `torch.utils.data.DataLoader` 来实现。`Dataset` 负责封装数据和标签，`DataLoader` 负责按指定批量大小生成数据批次。

### 7. 使用PyTorch构建模型

在PyTorch中构建深度学习模型非常直观，主要分为两步：
1.  **自定义模型类**：创建一个继承自 `nn.Module` 的类。
2.  **重写两个方法**：
    *   **`__init__` 方法（构造方法）**：在此初始化模型参数，并定义模型所需要的所有网络层（如嵌入层、卷积层、全连接层等）。
    *   **`forward` 方法（前向传播方法）**：在此定义数据从输入到输出的完整计算流程，即如何将 `__init__` 中定义的层连接起来。

**模型构建公式示例**：
```python
import torch.nn as nn

class MyModel(nn.Module):
    def __init__(self, vocab_size, embed_size):
        super(MyModel, self).__init__()
        self.embedding = nn.Embedding(vocab_size, embed_size)
        self.fc = nn.Linear(embed_size, num_classes)

    def forward(self, x):
        x = self.embedding(x) # 将词索引转换为向量
        x = x.mean(dim=1)     # 简单的池化：沿序列维度取平均
        output = self.fc(x)   # 分类层
        return output
```

***

## 第二部分：常见的文本分类模型 📚

![](img/68efa51e068fee303461772d91f58c31_32.png)

![](img/68efa51e068fee303461772d91f58c31_34.png)

![](img/68efa51e068fee303461772d91f58c31_36.png)

![](img/68efa51e068fee303461772d91f58c31_38.png)

上一节我们介绍了NLP模型的基础构建模块。本节中，我们来看看如何将这些模块应用于具体的任务——意图识别，这本质上是一个文本分类问题。

![](img/68efa51e068fee303461772d91f58c31_40.png)

![](img/68efa51e068fee303461772d91f58c31_42.png)

![](img/68efa51e068fee303461772d91f58c31_44.png)

我们将介绍五种常见的文本分类模型，它们各有适用场景。

![](img/68efa51e068fee303461772d91f58c31_46.png)

![](img/68efa51e068fee303461772d91f58c31_48.png)

![](img/68efa51e068fee303461772d91f58c31_50.png)

![](img/68efa51e068fee303461772d91f58c31_52.png)

### 模型概览

![](img/68efa51e068fee303461772d91f58c31_54.png)

![](img/68efa51e068fee303461772d91f58c31_56.png)

![](img/68efa51e068fee303461772d91f58c31_58.png)

![](img/68efa51e068fee303461772d91f58c31_60.png)

![](img/68efa51e068fee303461772d91f58c31_62.png)

![](img/68efa51e068fee303461772d91f58c31_64.png)

以下是五种文本分类模型的简要对比：
1.  **FastText**：结构极简，适合**类别区分明显**的**短文本**分类。
2.  **TextCNN**：利用多尺寸卷积核捕捉不同粒度的特征，适合**短文本**分类，计算效率高。
3.  **HAN (Hierarchical Attention Network)**：采用层次化注意力机制，特别适合处理**长文本**（如文档）。
4.  **BiLSTM + Attention**：结合双向LSTM和注意力机制，是一个**通用性较强**的模型，对长短文本都有效。
5.  **BERT微调**：使用强大的预训练语言模型进行微调，通常能取得**最佳效果**，但计算成本较高。

![](img/68efa51e068fee303461772d91f58c31_66.png)

![](img/68efa51e068fee303461772d91f58c31_68.png)

![](img/68efa51e068fee303461772d91f58c31_70.png)

![](img/68efa51e068fee303461772d91f58c31_72.png)

在实际工作中，建议先从简单的模型（如TextCNN）开始尝试，若效果不满足要求再考虑使用更复杂的模型（如BERT）。

![](img/68efa51e068fee303461772d91f58c31_74.png)

![](img/68efa51e068fee303461772d91f58c31_76.png)

![](img/68efa51e068fee303461772d91f58c31_78.png)

### 重点模型：TextCNN详解

![](img/68efa51e068fee303461772d91f58c31_80.png)

![](img/68efa51e068fee303461772d91f58c31_82.png)

TextCNN是处理短文本分类的高效模型。其核心思想是使用**多个不同宽度（即不同词数）的卷积核**，来并行地提取句子中不同局部范围的关联特征，这类似于N-gram模型从不同粒度（如字、词、短语）捕捉信息。

**模型结构拆解**：
1.  **嵌入层**：输入句子被转换为词向量矩阵，形状为 `[序列长度, 嵌入维度]`，可视为一张“特征图”。
2.  **卷积层**：使用多个不同尺寸的卷积核（例如，宽度分别为2, 3, 4，高度等于嵌入维度）在词向量矩阵上进行一维卷积操作。每个尺寸的卷积核可以有多个，以提取更丰富的特征。
    *   **卷积操作**：卷积核在矩阵上滑动，每次计算其覆盖区域内向量的点积之和，生成一个特征值。
3.  **池化层**：对每个卷积核产生的特征序列进行**最大池化（Max Pooling）**，即只保留最重要的一个特征值。
4.  **拼接与分类**：将所有池化后的特征值拼接成一个长向量，然后通过全连接层（+ Softmax/Sigmoid）输出分类结果。

**TextCNN前向传播流程公式化描述**：
```
输入: [batch_size, seq_len]
嵌入层: -> [batch_size, seq_len, embed_dim]
卷积层（多个核）: -> 多个 [batch_size, out_channels, new_seq_len]
池化层（最大池化）: -> 多个 [batch_size, out_channels]
拼接: -> [batch_size, total_out_channels]
全连接层: -> [batch_size, num_classes]
```

![](img/68efa51e068fee303461772d91f58c31_84.png)

![](img/68efa51e068fee303461772d91f58c31_86.png)

![](img/68efa51e068fee303461772d91f58c31_88.png)

![](img/68efa51e068fee303461772d91f58c31_90.png)

### 代码实践：构建TextCNN模型

![](img/68efa51e068fee303461772d91f58c31_92.png)

![](img/68efa51e068fee303461772d91f58c31_94.png)

以下是使用PyTorch实现TextCNN的关键代码片段：

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

![](img/68efa51e068fee303461772d91f58c31_96.png)

class TextCNN(nn.Module):
    def __init__(self, vocab_size, embed_size, num_classes, kernel_sizes=[3,4,5], num_filters=100):
        super(TextCNN, self).__init__()
        self.embedding = nn.Embedding(vocab_size, embed_size)
        # 多个并行的一维卷积层
        self.convs = nn.ModuleList([
            nn.Conv2d(1, num_filters, (k, embed_size)) for k in kernel_sizes
        ])
        self.dropout = nn.Dropout(0.5)
        self.fc = nn.Linear(len(kernel_sizes) * num_filters, num_classes)

    def forward(self, x):
        x = self.embedding(x)  # [batch, seq_len, embed_size]
        x = x.unsqueeze(1)     # 增加通道维 [batch, 1, seq_len, embed_size]
        # 对每个卷积核进行卷积和池化
        conv_outputs = []
        for conv in self.convs:
            conv_out = F.relu(conv(x)).squeeze(3) # [batch, num_filters, seq_len-k+1]
            pool_out = F.max_pool1d(conv_out, conv_out.size(2)).squeeze(2) # [batch, num_filters]
            conv_outputs.append(pool_out)
        # 拼接所有特征
        x = torch.cat(conv_outputs, dim=1) # [batch, len(kernel_sizes)*num_filters]
        x = self.dropout(x)
        logits = self.fc(x) # [batch, num_classes]
        return logits
```

### 模型训练与集成

![](img/68efa51e068fee303461772d91f58c31_98.png)

![](img/68efa51e068fee303461772d91f58c31_100.png)

训练过程包括：加载数据、定义损失函数和优化器、进行前向/反向传播、模型验证与保存。训练完成后，将预测函数集成到智能问答的主流程中，即可实现意图识别功能。

![](img/68efa51e068fee303461772d91f58c31_102.png)

```python
# 意图识别集成示例
def intent_recognition(user_input):
    # 使用训练好的TextCNN模型进行预测
    prob = textcnn_model.predict(user_input)
    if prob > 0.5:
        return "闲聊", prob
    else:
        return "专业问题", prob
```

![](img/68efa51e068fee303461772d91f58c31_104.png)

***

## 第三部分：Transformer模型核心——自注意力机制 ⚡

上一节我们介绍了用于分类的CNN模型。本节中，我们来看看NLP领域里程碑式的模型——Transformer，并深入理解其核心组件：自注意力机制。

Transformer模型同样遵循编码器-解码器（Encoder-Decoder）架构，但其完全基于注意力机制，摒弃了循环神经网络（RNN）。

### 自注意力机制详解

自注意力（Self-Attention）的核心思想是：让句子中的**每个词**都与该句子中的**所有词**进行交互，从而动态地计算每个词相对于其他所有词的“重要性”权重，并基于这些权重对词向量进行加权求和，得到新的、蕴含了全局上下文信息的词表示。

**计算步骤**：
1.  **生成Q, K, V**：对于输入序列的每个词向量，通过三个不同的权重矩阵（`W_Q`, `W_K`, `W_V`）线性变换，得到对应的查询向量（Query）、键向量（Key）和值向量（Value）。通常，初始时Q, K, V就是输入词向量本身。
2.  **计算注意力分数**：计算Query与所有Key的点积，得到每个词对其他词的原始注意力分数。这衡量了词与词之间的相关性。
3.  **缩放与归一化**：将注意力分数除以 `sqrt(d_k)`（`d_k`是Key向量的维度），以防止点积结果过大导致Softmax梯度消失。然后应用Softmax函数，将分数归一化为概率分布（总和为1），即得到最终的注意力权重。
4.  **加权求和**：将注意力权重与对应的Value向量相乘并求和，得到该词新的向量表示。

**自注意力公式**：
```
Attention(Q, K, V) = softmax( (Q * K^T) / sqrt(d_k) ) * V
```

![](img/68efa51e068fee303461772d91f58c31_106.png)

![](img/68efa51e068fee303461772d91f58c31_108.png)

![](img/68efa51e068fee303461772d91f58c31_110.png)

### 多头注意力机制

单一的注意力头可能只能捕捉到一种模式的依赖关系。为了增强模型的能力，Transformer使用了**多头注意力（Multi-Head Attention）**。

其做法是：
1.  将Q, K, V通过多组不同的权重矩阵投影到多个子空间（即多个“头”）。
2.  在每个头上独立地执行自注意力计算。
3.  将所有头的输出拼接起来，再通过一个线性层进行融合和降维。

这样，模型可以并行地从不同子空间（例如，语法、语义、指代等不同方面）学习信息。

**多头注意力公式**：
```
MultiHead(Q, K, V) = Concat(head_1, ..., head_h) * W_O
其中 head_i = Attention(Q * W_Q_i, K * W_K_i, V * W_V_i)
```

### 在模型中的应用

在Transformer的编码器中，多头自注意力层是其核心。它使模型能够无视距离地捕捉序列中任意两个位置之间的依赖关系，解决了RNN长距离依赖捕捉困难的问题。这种机制同样可以替换之前提到的HAN或BiLSTM+Attention模型中的注意力层，往往能取得更好的效果。

***

## 总结 📝

本节课中我们一起学习了构建NLP深度学习模型的完整流程，从输入处理（字/词向量、词典构建、长度统一）到使用PyTorch搭建模型。我们重点剖析了TextCNN这一高效的短文本分类模型，并通过代码实践将其应用于意图识别任务。最后，我们探讨了Transformer模型的核心——自注意力机制的原理，理解了其如何通过动态加权聚合全局信息。

![](img/68efa51e068fee303461772d91f58c31_112.png)

![](img/68efa51e068fee303461772d91f58c31_114.png)

![](img/68efa51e068fee303461772d91f58c31_116.png)

![](img/68efa51e068fee303461772d91f58c31_117.png)

通过本课的学习，你应该能够独立实现一个基础的文本分类模型，并理解现代NLP模型中最重要的注意力机制的基本思想。请务必动手完成代码实践，这是掌握这些知识的关键。