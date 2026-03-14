# 133：PyTorch中的RNN情感分类器 🧠

![](img/a7a047c14a6293beebbf971ebf029967_1.png)

![](img/a7a047c14a6293beebbf971ebf029967_2.png)

![](img/a7a047c14a6293beebbf971ebf029967_3.png)

![](img/a7a047c14a6293beebbf971ebf029967_5.png)

在本节课中，我们将学习如何使用PyTorch和TorchText库，实现一个基于循环神经网络（RNN）的情感分类器。我们将处理IMDB电影评论数据集，将文本数据转换为模型可以理解的格式，并构建一个“多对一”的LSTM模型来预测评论的情感是正面还是负面。

![](img/a7a047c14a6293beebbf971ebf029967_7.png)

![](img/a7a047c14a6293beebbf971ebf029967_9.png)

![](img/a7a047c14a6293beebbf971ebf029967_11.png)

---

## 概述

![](img/a7a047c14a6293beebbf971ebf029967_13.png)

![](img/a7a047c14a6293beebbf971ebf029967_15.png)

上一节我们介绍了“多对一”RNN的基本概念。本节中，我们将看看如何在PyTorch中具体实现它。整个过程包括：从原始文本开始，构建词汇表，将其转换为整数表示，然后通过嵌入层（Embedding Layer）输入到RNN中。最终，RNN会输出一个类别标签。

![](img/a7a047c14a6293beebbf971ebf029967_17.png)

![](img/a7a047c14a6293beebbf971ebf029967_1.png)

![](img/a7a047c14a6293beebbf971ebf029967_19.png)

![](img/a7a047c14a6293beebbf971ebf029967_21.png)

![](img/a7a047c14a6293beebbf971ebf029967_23.png)

我们准备了两个版本的代码：一个是常规LSTM实现，另一个是使用了填充打包（Packed LSTM）的更高效实现。我们先从常规方法开始讲解。

![](img/a7a047c14a6293beebbf971ebf029967_25.png)

---

## 环境设置与数据准备

![](img/a7a047c14a6293beebbf971ebf029967_27.png)

![](img/a7a047c14a6293beebbf971ebf029967_28.png)

![](img/a7a047c14a6293beebbf971ebf029967_29.png)

![](img/a7a047c14a6293beebbf971ebf029967_31.png)

首先，我们需要导入必要的库并设置一些通用参数。

```python
import torch
import torchtext
# 确保使用torchtext 0.9或更高版本
```

以下是模型的关键参数设置：
*   **词汇表大小（VOCAB_SIZE）**：`20000`
*   **学习率（LEARNING_RATE）**：`0.005`
*   **批大小（BATCH_SIZE）**：`128`
*   **训练轮数（NUM_EPOCHS）**：`25`
*   **嵌入维度（EMBEDDING_DIM）**：`128`
*   **隐藏层维度（HIDDEN_DIM）**：`256`
*   **类别数（NUM_CLASSES）**：`2`

![](img/a7a047c14a6293beebbf971ebf029967_33.png)

![](img/a7a047c14a6293beebbf971ebf029967_35.png)

![](img/a7a047c14a6293beebbf971ebf029967_37.png)

### 下载与加载数据集

![](img/a7a047c14a6293beebbf971ebf029967_39.png)

![](img/a7a047c14a6293beebbf971ebf029967_40.png)

![](img/a7a047c14a6293beebbf971ebf029967_42.png)

我们将使用IMDB电影评论数据集。为了简化预处理步骤，我们从特定来源下载一个已处理好的CSV格式文件。

```python
import pandas as pd
# 下载并解压数据集
# ... (数据下载和解压代码)
df = pd.read_csv(‘imdb.csv’)
```

![](img/a7a047c14a6293beebbf971ebf029967_44.png)

数据框包含`review`（评论文本）和`sentiment`（情感标签，0为负面，1为正面）两列。为了避免在后续代码中因列名不同而频繁修改，我们将其重命名为通用名称。

![](img/a7a047c14a6293beebbf971ebf029967_46.png)

```python
TEXT_COLUMN_NAME = ‘TEXT_COLUMN_NAME’
LABEL_COLUMN_NAME = ‘LABEL_COLUMN_NAME’
df = df.rename(columns={‘review’: TEXT_COLUMN_NAME, ‘sentiment’: LABEL_COLUMN_NAME})
df.to_csv(‘imdb_renamed.csv’, index=False)
```

![](img/a7a047c14a6293beebbf971ebf029967_48.png)

![](img/a7a047c14a6293beebbf971ebf029967_50.png)

![](img/a7a047c14a6293beebbf971ebf029967_52.png)

---

## 使用TorchText处理文本

![](img/a7a047c14a6293beebbf971ebf029967_54.png)

![](img/a7a047c14a6293beebbf971ebf029967_56.png)

![](img/a7a047c14a6293beebbf971ebf029967_58.png)

现在，我们使用TorchText库将数据准备成适合RNN输入的格式。

![](img/a7a047c14a6293beebbf971ebf029967_60.png)

![](img/a7a047c14a6293beebbf971ebf029967_62.png)

### 定义字段与分词器

![](img/a7a047c14a6293beebbf971ebf029967_64.png)

![](img/a7a047c14a6293beebbf971ebf029967_65.png)

![](img/a7a047c14a6293beebbf971ebf029967_67.png)

![](img/a7a047c14a6293beebbf971ebf029967_69.png)

首先，我们需要一个分词器（Tokenizer）将句子拆分成单词（标记）。这里我们使用SpaCy库，它比简单的空格分割更健壮，能处理特殊字符和HTML标签。

![](img/a7a047c14a6293beebbf971ebf029967_71.png)

![](img/a7a047c14a6293beebbf971ebf029967_73.png)

```python
import spacy
spacy_en = spacy.load(‘en_core_web_sm’) # 需要先运行 ‘python -m spacy download en_core_web_sm’ 下载模型

![](img/a7a047c14a6293beebbf971ebf029967_75.png)

![](img/a7a047c14a6293beebbf971ebf029967_77.png)

![](img/a7a047c14a6293beebbf971ebf029967_79.png)

![](img/a7a047c14a6293beebbf971ebf029967_81.png)

![](img/a7a047c14a6293beebbf971ebf029967_82.png)

def tokenizer(text):
    return [token.text for token in spacy_en.tokenizer(text)]
```

![](img/a7a047c14a6293beebbf971ebf029967_84.png)

![](img/a7a047c14a6293beebbf971ebf029967_85.png)

接下来，我们为文本和标签定义TorchText的`Field`。注意，我们使用的是TorchText的旧版API（`legacy`）。

![](img/a7a047c14a6293beebbf971ebf029967_87.png)

![](img/a7a047c14a6293beebbf971ebf029967_89.png)

```python
from torchtext.legacy import data

![](img/a7a047c14a6293beebbf971ebf029967_91.png)

![](img/a7a047c14a6293beebbf971ebf029967_92.png)

![](img/a7a047c14a6293beebbf971ebf029967_94.png)

![](img/a7a047c14a6293beebbf971ebf029967_95.png)

TEXT = data.Field(tokenize=tokenizer, batch_first=True)
LABEL = data.LabelField(dtype=torch.long, batch_first=True)
```

![](img/a7a047c14a6293beebbf971ebf029967_97.png)

![](img/a7a047c14a6293beebbf971ebf029967_99.png)

然后，我们使用`TabularDataset`来读取CSV文件，并指定字段映射。

![](img/a7a047c14a6293beebbf971ebf029967_101.png)

![](img/a7a047c14a6293beebbf971ebf029967_103.png)

![](img/a7a047c14a6293beebbf971ebf029967_104.png)

![](img/a7a047c14a6293beebbf971ebf029967_106.png)

```python
fields = [(TEXT_COLUMN_NAME, TEXT), (LABEL_COLUMN_NAME, LABEL)]
dataset = data.TabularDataset(path=‘imdb_renamed.csv’, format=‘csv’, fields=fields, skip_header=True)
```

### 划分数据集并构建词汇表

将数据集划分为训练集、验证集和测试集。

```python
train_data, test_data = dataset.split(split_ratio=0.8)
train_data, valid_data = train_data.split(split_ratio=0.85)
# 最终得到：34000个训练样本，6000个验证样本，10000个测试样本
```

![](img/a7a047c14a6293beebbf971ebf029967_108.png)

**重要**：词汇表应仅基于训练数据构建，以模拟真实场景中验证集和测试集是未知数据的情况。

![](img/a7a047c14a6293beebbf971ebf029967_110.png)

![](img/a7a047c14a6293beebbf971ebf029967_112.png)

![](img/a7a047c14a6293beebbf971ebf029967_114.png)

```python
TEXT.build_vocab(train_data, max_size=VOCAB_SIZE)
LABEL.build_vocab(train_data)
print(f‘词汇表大小: {len(TEXT.vocab)}’) # 应为20002（20000个常用词 + <unk> + <pad>）
print(f‘类别: {LABEL.vocab.stoi}’) # 查看标签映射
```

![](img/a7a047c14a6293beebbf971ebf029967_116.png)

![](img/a7a047c14a6293beebbf971ebf029967_118.png)

词汇表构建后，每个单词都有一个对应的整数索引。例如，单词`“the”`可能对应索引2。

![](img/a7a047c14a6293beebbf971ebf029967_120.png)

![](img/a7a047c14a6293beebbf971ebf029967_122.png)

### 创建数据加载器

![](img/a7a047c14a6293beebbf971ebf029967_124.png)

我们使用`BucketIterator`，它能够将长度相近的句子分到同一个批次中，从而减少填充（padding）的数量，提高效率。

![](img/a7a047c14a6293beebbf971ebf029967_126.png)

![](img/a7a047c14a6293beebbf971ebf029967_128.png)

![](img/a7a047c14a6293beebbf971ebf029967_130.png)

![](img/a7a047c14a6293beebbf971ebf029967_131.png)

```python
train_loader, valid_loader, test_loader = data.BucketIterator.splits(
    (train_data, valid_data, test_data),
    batch_size=BATCH_SIZE,
    sort_key=lambda x: len(x.TEXT_COLUMN_NAME), # 按句子长度排序
    sort_within_batch=True,
    device=device # 指定设备（CPU/GPU）
)
```

![](img/a7a047c14a6293beebbf971ebf029967_133.png)

![](img/a7a047c14a6293beebbf971ebf029967_134.png)

检查一个批次的数据，其形状为`(句子长度, 批次大小)`，这与我们之前在卷积网络中常见的`(批次大小, …)`格式不同。

---

![](img/a7a047c14a6293beebbf971ebf029967_136.png)

![](img/a7a047c14a6293beebbf971ebf029967_138.png)

![](img/a7a047c14a6293beebbf971ebf029967_139.png)

## 构建LSTM模型 🏗️

![](img/a7a047c14a6293beebbf971ebf029967_141.png)

![](img/a7a047c14a6293beebbf971ebf029967_143.png)

![](img/a7a047c14a6293beebbf971ebf029967_145.png)

![](img/a7a047c14a6293beebbf971ebf029967_147.png)

现在，我们来构建核心的RNN模型。我们使用的是LSTM单元。

![](img/a7a047c14a6293beebbf971ebf029967_149.png)

![](img/a7a047c14a6293beebbf971ebf029967_151.png)

![](img/a7a047c14a6293beebbf971ebf029967_153.png)

![](img/a7a047c14a6293beebbf971ebf029967_155.png)

### 模型结构

![](img/a7a047c14a6293beebbf971ebf029967_157.png)

![](img/a7a047c14a6293beebbf971ebf029967_159.png)

模型主要包含三层：
1.  **嵌入层（Embedding Layer）**：将单词索引转换为密集向量表示。公式为：`embedding = Embedding(vocab_size, embedding_dim)`。
2.  **LSTM层**：处理序列数据，捕获上下文信息。公式为：`output, (hidden, cell) = LSTM(embedding)`。
3.  **全连接输出层（Fully Connected Layer）**：将LSTM最后一个时间步的隐藏状态映射到类别输出。公式为：`logits = Linear(hidden_dim, num_classes)`。

![](img/a7a047c14a6293beebbf971ebf029967_161.png)

![](img/a7a047c14a6293beebbf971ebf029967_162.png)

![](img/a7a047c14a6293beebbf971ebf029967_164.png)

![](img/a7a047c14a6293beebbf971ebf029967_165.png)

在“多对一”结构中，我们只利用LSTM最后一个时间步的隐藏状态（`hidden`）来进行分类。

![](img/a7a047c14a6293beebbf971ebf029967_167.png)

![](img/a7a047c14a6293beebbf971ebf029967_169.png)

![](img/a7a047c14a6293beebbf971ebf029967_171.png)

![](img/a7a047c14a6293beebbf971ebf029967_173.png)

```python
import torch.nn as nn

![](img/a7a047c14a6293beebbf971ebf029967_175.png)

![](img/a7a047c14a6293beebbf971ebf029967_177.png)

class RNN(nn.Module):
    def __init__(self, input_dim, embedding_dim, hidden_dim, output_dim):
        super().__init__()
        self.embedding = nn.Embedding(input_dim, embedding_dim)
        self.rnn = nn.LSTM(embedding_dim, hidden_dim, batch_first=True)
        self.fc = nn.Linear(hidden_dim, output_dim)

    def forward(self, text):
        # text shape: [batch size, sentence length]
        embedded = self.embedding(text) # shape: [batch size, sent len, emb dim]
        output, (hidden, cell) = self.rnn(embedded)
        # hidden shape: [1, batch size, hid dim]
        hidden = hidden.squeeze(0) # 移除第一维，shape: [batch size, hid dim]
        logits = self.fc(hidden)
        return logits
```

![](img/a7a047c14a6293beebbf971ebf029967_179.png)

![](img/a7a047c14a6293beebbf971ebf029967_181.png)

![](img/a7a047c14a6293beebbf971ebf029967_182.png)

### 初始化模型与优化器

![](img/a7a047c14a6293beebbf971ebf029967_184.png)

![](img/a7a047c14a6293beebbf971ebf029967_185.png)

![](img/a7a047c14a6293beebbf971ebf029967_187.png)

使用定义好的参数初始化模型，并选择Adam优化器。

![](img/a7a047c14a6293beebbf971ebf029967_188.png)

```python
INPUT_DIM = len(TEXT.vocab) # 20002
model = RNN(INPUT_DIM, EMBEDDING_DIM, HIDDEN_DIM, NUM_CLASSES)
optimizer = torch.optim.Adam(model.parameters(), lr=LEARNING_RATE)
criterion = nn.CrossEntropyLoss()
```

---

![](img/a7a047c14a6293beebbf971ebf029967_190.png)

## 训练与评估模型 🔄

![](img/a7a047c14a6293beebbf971ebf029967_192.png)

以下是训练循环的核心步骤。

![](img/a7a047c14a6293beebbf971ebf029967_194.png)

```python
def train(model, iterator, optimizer, criterion):
    model.train()
    for batch in iterator:
        text = getattr(batch, TEXT_COLUMN_NAME)
        labels = getattr(batch, LABEL_COLUMN_NAME)
        optimizer.zero_grad()
        logits = model(text)
        loss = criterion(logits, labels)
        loss.backward()
        optimizer.step()
        # … 计算并记录精度
```

![](img/a7a047c14a6293beebbf971ebf029967_196.png)

在训练过程中，模型在验证集上的准确率逐渐提升，最终在测试集上可以达到约84%的准确率。

![](img/a7a047c14a6293beebbf971ebf029967_198.png)

---

![](img/a7a047c14a6293beebbf971ebf029967_200.png)

![](img/a7a047c14a6293beebbf971ebf029967_202.png)

## 使用模型进行预测

![](img/a7a047c14a6293beebbf971ebf029967_204.png)

![](img/a7a047c14a6293beebbf971ebf029967_206.png)

训练完成后，我们可以用模型对新文本进行情感预测。

![](img/a7a047c14a6293beebbf971ebf029967_208.png)

以下是预测单个文本情感的步骤：
1.  将模型设置为评估模式（`model.eval()`）。
2.  使用相同的分词器处理新文本。
3.  将单词序列转换为对应的索引序列。
4.  将索引张量输入模型，得到逻辑值（logits）。
5.  应用Softmax函数得到概率分布。

```python
def predict_sentiment(model, sentence):
    model.eval()
    tokenized = [token.text for token in spacy_en.tokenizer(sentence)]
    indexed = [TEXT.vocab.stoi[token] for token in tokenized]
    tensor = torch.LongTensor(indexed).unsqueeze(0).to(device)
    logits = model(tensor)
    probabilities = torch.softmax(logits, dim=1)
    return probabilities[0][1].item() # 返回正面情感的概率

![](img/a7a047c14a6293beebbf971ebf029967_210.png)

![](img/a7a047c14a6293beebbf971ebf029967_212.png)

# 示例
positive_prob = predict_sentiment(model, “This is such an awesome movie, I really love it!”)
print(f“正面概率: {positive_prob:.2%}”) # 可能输出：98%
```

![](img/a7a047c14a6293beebbf971ebf029967_214.png)

![](img/a7a047c14a6293beebbf971ebf029967_216.png)

![](img/a7a047c14a6293beebbf971ebf029967_218.png)

---

![](img/a7a047c14a6293beebbf971ebf029967_220.png)

![](img/a7a047c14a6293beebbf971ebf029967_222.png)

## 更高效的实现：Packed LSTM

![](img/a7a047c14a6293beebbf971ebf029967_224.png)

![](img/a7a047c14a6293beebbf971ebf029967_225.png)

![](img/a7a047c14a6293beebbf971ebf029967_227.png)

![](img/a7a047c14a6293beebbf971ebf029967_229.png)

为了提高效率，我们可以使用“Packed LSTM”来进一步减少填充带来的计算浪费。主要改动包括：
*   在分词和`Field`定义时需要包含句子长度信息。
*   在模型前向传播时，使用`pack_padded_sequence`和`pad_packed_sequence`来处理变长序列。

![](img/a7a047c14a6293beebbf971ebf029967_231.png)

![](img/a7a047c14a6293beebbf971ebf029967_232.png)

![](img/a7a047c14a6293beebbf971ebf029967_233.png)

这些改动使得训练速度更快（例如，每个epoch从约0.7分钟减少到0.33分钟），并且在这个数据集上获得了更高的测试准确率（约99%）。

![](img/a7a047c14a6293beebbf971ebf029967_235.png)

![](img/a7a047c14a6293beebbf971ebf029967_237.png)

---

![](img/a7a047c14a6293beebbf971ebf029967_239.png)

![](img/a7a047c14a6293beebbf971ebf029967_240.png)

## 总结

![](img/a7a047c14a6293beebbf971ebf029967_242.png)

本节课中，我们一起学习了如何在PyTorch中实现一个用于情感分类的RNN（LSTM）模型。我们涵盖了从文本数据预处理（使用TorchText和SpaCy）、构建词汇表、创建数据加载器，到定义、训练和评估“多对一”LSTM模型的完整流程。虽然RNN的实现比卷积网络更为复杂，涉及许多步骤和概念，但它是处理序列数据的基础。希望本教程能为你提供一个实用的起点。在接下来的课程中，我们将进入生成式模型的世界，探索自编码器、变分自编码器和生成对抗网络等主题。