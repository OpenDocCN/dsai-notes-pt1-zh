# 【七月在线】NLP高端就业训练营10期 - P4：4.文本相似度计算与文本匹配模型 📚

![](img/398942c983e0a079b7cef9fb3e214f65_0.png)

![](img/398942c983e0a079b7cef9fb3e214f65_2.png)

![](img/398942c983e0a079b7cef9fb3e214f65_4.png)

![](img/398942c983e0a079b7cef9fb3e214f65_6.png)

在本节课中，我们将要学习文本相似度计算与文本匹配模型。我们将从文本相似度的基本概念和应用场景入手，然后深入探讨基于表示学习和基于匹配模型的两种主要计算方法，并重点讲解ESIM模型的原理与实现。

## 第一部分：什么是文本相似度？ 🤔

上一节我们介绍了文本分类，本节中我们来看看文本相似度。文本相似度，顾名思义，就是计算两条文本之间的相似程度。它与文本分类最大的区别在于：文本分类针对单条文本，而文本相似度计算需要同时处理两条文本。

![](img/398942c983e0a079b7cef9fb3e214f65_8.png)

![](img/398942c983e0a079b7cef9fb3e214f65_10.png)

文本相似度在自然语言处理中有着广泛的应用场景。

以下是其主要应用场景：
*   **信息检索**：例如搜索引擎，用户输入一个查询（query），系统需要从海量文档（document）中找出最相关的结果并排序。文本相似度是排序的核心依据之一。
*   **问答系统**：系统有一个预先构建的问答对（QA）数据库。当用户提出一个新问题时，系统计算该问题与库中所有问题的相似度，将最相似问题对应的答案返回给用户。
*   **文本聚类**：将大量文本中语义相近的句子聚集在一起，通常也需要计算两两文本间的相似度。

在计算相似度时，我们需要一个度量标准。对于文本向量，最常用的是**余弦相似度**。

**公式**：`sim(A, B) = (A · B) / (||A|| * ||B||)`

选择余弦相似度的主要原因是其值域在[-1, 1]之间，对于文本向量，其值通常落在[0, 1]区间，便于理解和比较。

## 第二部分：基于表示学习的文本相似度计算 🧠

上一节我们了解了文本相似度的概念，本节中我们来看看第一种计算方法——基于表示学习。这种方法的核心思想是：通过一个模型将句子转换为一个固定维度的向量（即句子的embedding），然后计算这两个向量之间的余弦相似度。

整个流程可以概括为：
1.  输入句子。
2.  将句子分词，得到词语序列。
3.  将每个词语转换为词向量（word embedding）。
4.  将所有词向量聚合（如加权平均），生成句向量（sentence embedding）。
5.  计算两个句向量之间的余弦相似度。

其中，第3步使用的模型是关键。我们可以使用简单的Word2Vec，也可以使用更强大的预训练语言模型，如ELMo。

### ELMo模型介绍

ELMo（Embeddings from Language Models）是一个基于双层双向LSTM的预训练语言模型。

![](img/398942c983e0a079b7cef9fb3e214f65_12.png)

**模型结构**：
*   输入是词的embedding。
*   经过一个前向的LSTM层和一个后向的LSTM层（均为两层）。
*   将每个位置上前向和后向LSTM的输出拼接起来，作为该词的上下文相关表示。

![](img/398942c983e0a079b7cef9fb3e214f65_14.png)

![](img/398942c983e0a079b7cef9fb3e214f65_16.png)

![](img/398942c983e0a079b7cef9fb3e214f65_18.png)

![](img/398942c983e0a079b7cef9fb3e214f65_20.png)

**预训练任务**：ELMo在海量文本上进行无监督训练，任务是预测句子的下一个词。训练完成后，模型权重已经包含了丰富的语言知识。

![](img/398942c983e0a079b7cef9fb3e214f65_22.png)

![](img/398942c983e0a079b7cef9fb3e214f65_24.png)

![](img/398942c983e0a079b7cef9fb3e214f65_26.png)

![](img/398942c983e0a079b7cef9fb3e214f65_28.png)

![](img/398942c983e0a079b7cef9fb3e214f65_30.png)

**如何使用ELMo生成句向量**：
1.  加载预训练的ELMo权重。
2.  输入句子，模型输出每个词的上下文相关向量。
3.  对这些词向量进行平均（或加权平均）操作，得到句子的向量表示。

![](img/398942c983e0a079b7cef9fb3e214f65_32.png)

![](img/398942c983e0a079b7cef9fb3e214f65_34.png)

![](img/398942c983e0a079b7cef9fb3e214f65_36.png)

![](img/398942c983e0a079b7cef9fb3e214f65_38.png)

![](img/398942c983e0a079b7cef9fb3e214f65_40.png)

以下是使用ELMo生成句子向量的代码示例：
```python
import torch
import jieba
from elmoformanylangs import Embedder

# 初始化模型（需提前下载权重文件）
e = Embedder(‘path/to/your/elmo/model‘)

# 句子分词
sentence = “这是一个例句”
seg_list = jieba.lcut(sentence)

# 生成ELMo向量
# 输出是每个词的向量，形状为 (词数, 向量维度)
vectors = e.sents2elmo([seg_list])

# 对词向量求平均，得到句向量
sentence_embedding = torch.mean(torch.tensor(vectors[0]), dim=0)
```

通过这种方式，我们就能得到高质量的句子表示，进而计算相似度。

## 第三部分：基于匹配模型的文本相似度计算 ⚔️

上一节我们介绍了基于表示学习的方法，本节中我们来看看第二种主流方法——基于匹配模型。这种方法不单独生成句向量，而是构建一个模型，直接接收两条文本作为输入，并输出一个相似度分数或分类结果（如是否相似）。

这类模型通常采用“孪生网络”或“双塔结构”，即两个结构相同（通常共享权重）的子网络分别处理两条文本，然后在高层进行交互和判断。

根据交互发生的位置，匹配模型可以分为：
*   **外交互**：两个子网络独立处理文本，生成各自的向量表示后，再计算相似度或进行分类。
*   **内交互**：在模型处理过程中，两条文本的表示就进行交互（如通过注意力机制），最后再汇总输出。

### 经典模型：DSSM 与 ESIM

**DSSM（Deep Structured Semantic Model）** 是双塔结构的经典代表。每个“塔”由全连接层构成，分别将查询（Query）和文档（Document）映射到同一个语义空间，然后计算其余弦相似度。

**ESIM（Enhanced Sequential Inference Model）** 是一个效果和效率都不错的模型，它基于LSTM并引入了注意力机制进行内交互。接下来我们将重点解析ESIM模型的结构与实现。

ESIM模型主要包含四层：
1.  **Input Encoding**：使用双向LSTM对输入的两条文本分别进行编码。
2.  **Local Inference Modeling**：使用注意力机制（Attention）让两条文本的表示进行交互，并生成增强的局部推理信息。
3.  **Inference Composition**：再次使用双向LSTM对增强后的信息进行组合编码。
4.  **Output**：使用池化（最大池化与平均池化）将变长序列转换为定长向量，然后通过全连接层进行分类。

**注意力机制（Attention）核心**：
这是ESIM模型的关键。它计算文本A中每个词与文本B中所有词的相似度（通过点积得到权重矩阵），然后对文本B的词向量进行加权求和，得到一个能代表文本A中某个词的“上下文向量”。这个过程让模型能够关注两条文本间相关的部分。

![](img/398942c983e0a079b7cef9fb3e214f65_42.png)

**公式（简化）**：`权重矩阵 E = A · B^T`， 对E按行做softmax得到归一化权重，然后用该权重对B加权得到A的注意力表示 `A_hat = softmax(E) · B`。对B也进行类似操作。

![](img/398942c983e0a079b7cef9fb3e214f65_44.png)

![](img/398942c983e0a079b7cef9fb3e214f65_46.png)

### ESIM模型代码实现要点

![](img/398942c983e0a079b7cef9fb3e214f65_48.png)

![](img/398942c983e0a079b7cef9fb3e214f65_50.png)

以下是使用PyTorch构建ESIM模型的核心结构代码：

![](img/398942c983e0a079b7cef9fb3e214f65_52.png)

![](img/398942c983e0a079b7cef9fb3e214f65_54.png)

```python
import torch
import torch.nn as nn

![](img/398942c983e0a079b7cef9fb3e214f65_56.png)

![](img/398942c983e0a079b7cef9fb3e214f65_58.png)

class ESIM(nn.Module):
    def __init__(self, vocab_size, embedding_size, hidden_size):
        super(ESIM, self).__init__()
        self.embedding = nn.Embedding(vocab_size, embedding_size)
        # Input Encoding 层 (双向LSTM)
        self.lstm1 = nn.LSTM(embedding_size, hidden_size, batch_first=True, bidirectional=True)
        # Inference Composition 层 (双向LSTM)
        # 注意输入维度：经过Local Inference后，向量维度变为 hidden_size*2 * 4
        self.lstm2 = nn.LSTM(hidden_size * 8, hidden_size, batch_first=True, bidirectional=True)
        # 分类层
        self.fc = nn.Sequential(
            nn.Linear(hidden_size * 8, hidden_size),
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(hidden_size, 2) # 二分类输出
        )
        self.dropout = nn.Dropout(0.2)

    def forward(self, p, q):
        # 1. Embedding
        p_emb = self.embedding(p)
        q_emb = self.embedding(q)
        # 2. Input Encoding
        p_enc, _ = self.lstm1(p_emb)
        q_enc, _ = self.lstm1(q_emb)
        p_enc = self.dropout(p_enc)
        q_enc = self.dropout(q_enc)
        # 3. Local Inference Modeling (Attention)
        e = torch.matmul(p_enc, q_enc.transpose(1, 2)) # 相似度矩阵
        p_att = torch.matmul(torch.softmax(e, dim=-1), q_enc) # p基于q的注意力表示
        q_att = torch.matmul(torch.softmax(e.transpose(1, 2), dim=-1), p_enc) # q基于p的注意力表示
        # 组合局部推理信息
        m_p = torch.cat([p_enc, p_att, p_enc - p_att, p_enc * p_att], dim=-1)
        m_q = torch.cat([q_enc, q_att, q_enc - q_att, q_enc * q_att], dim=-1)
        # 4. Inference Composition
        p_compose, _ = self.lstm2(m_p)
        q_compose, _ = self.lstm2(m_q)
        # 5. Output (Pooling + Classification)
        p_avg = torch.mean(p_compose, dim=1)
        q_avg = torch.mean(q_compose, dim=1)
        p_max, _ = torch.max(p_compose, dim=1)
        q_max, _ = torch.max(q_compose, dim=1)
        v = torch.cat([p_avg, q_avg, p_max, q_max], dim=-1)
        v = self.dropout(v)
        out = self.fc(v)
        return out
```

![](img/398942c983e0a079b7cef9fb3e214f65_60.png)

![](img/398942c983e0a079b7cef9fb3e214f65_62.png)

![](img/398942c983e0a079b7cef9fb3e214f65_64.png)

![](img/398942c983e0a079b7cef9fb3e214f65_66.png)

**模型训练**：训练流程与常规文本分类模型一致，包括数据加载、模型初始化、定义损失函数（如交叉熵损失）和优化器、进行前向/反向传播等步骤。

![](img/398942c983e0a079b7cef9fb3e214f65_68.png)

![](img/398942c983e0a079b7cef9fb3e214f65_70.png)

### 两种方法的对比与应用

![](img/398942c983e0a079b7cef9fb3e214f65_72.png)

![](img/398942c983e0a079b7cef9fb3e214f65_74.png)

![](img/398942c983e0a079b7cef9fb3e214f65_76.png)

![](img/398942c983e0a079b7cef9fb3e214f65_78.png)

![](img/398942c983e0a079b7cef9fb3e214f65_80.png)

![](img/398942c983e0a079b7cef9fb3e214f65_82.png)

在实际系统中，表示学习方法和匹配模型方法常常结合使用，形成“召回-排序”流水线：
*   **召回阶段**：使用表示学习方法（如ELMo句向量+余弦相似度）。这种方法速度快，可以快速从海量候选文本中筛选出Top-K个最相关的候选项。
*   **排序阶段**：使用复杂的匹配模型（如ESIM）对召回阶段得到的少量候选项进行精细排序，得到最终结果。这种方法精度高，但计算量较大。

![](img/398942c983e0a079b7cef9fb3e214f65_84.png)

![](img/398942c983e0a079b7cef9fb3e214f65_86.png)

这种架构兼顾了效率与效果。

![](img/398942c983e0a079b7cef9fb3e214f65_88.png)

![](img/398942c983e0a079b7cef9fb3e214f65_90.png)

![](img/398942c983e0a079b7cef9fb3e214f65_92.png)

## 总结 🎯

本节课中我们一起学习了文本相似度计算与文本匹配模型。

![](img/398942c983e0a079b7cef9fb3e214f65_94.png)

![](img/398942c983e0a079b7cef9fb3e214f65_96.png)

我们首先明确了文本相似度的定义及其在检索、问答、聚类等场景下的重要作用。接着，我们深入探讨了两种主流的计算方法：基于表示学习的方法（如使用ELMo生成句向量）和基于匹配模型的方法（如ESIM模型）。我们重点剖析了ESIM模型的结构，特别是其核心的注意力机制，并给出了模型实现的关键代码。

![](img/398942c983e0a079b7cef9fb3e214f65_98.png)

![](img/398942c983e0a079b7cef9fb3e214f65_100.png)

![](img/398942c983e0a079b7cef9fb3e214f65_102.png)

![](img/398942c983e0a079b7cef9fb3e214f65_104.png)

![](img/398942c983e0a079b7cef9fb3e214f65_106.png)

![](img/398942c983e0a079b7cef9fb3e214f65_107.png)

理解文本相似度计算是构建许多高级NLP应用（如搜索引擎、智能客服）的基础。希望大家通过本节课的学习，能够掌握文本相似度的核心思想与主流方法，并能够动手实现一个基本的匹配模型。