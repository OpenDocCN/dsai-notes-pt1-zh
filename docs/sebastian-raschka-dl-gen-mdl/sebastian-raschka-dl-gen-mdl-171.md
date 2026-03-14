# 171：使用PyTorch实现DistilBERT电影评论分类器 🎬

在本节课中，我们将学习如何使用Hugging Face的Transformers库，基于预训练的DistilBERT模型，在电影评论数据集上构建一个文本分类器。我们将涵盖从数据加载、模型微调到性能评估的完整流程。

---

![](img/3d9d7ce1bd24c60024253efa02b91d54_1.png)

## 概述

![](img/3d9d7ce1bd24c60024253efa02b91d54_2.png)

上一节我们介绍了Transformer模型及其变体。本节中，我们将通过一个具体的代码示例，展示如何使用预训练的DistilBERT模型进行下游任务的微调，即电影评论情感分类。

![](img/3d9d7ce1bd24c60024253efa02b91d54_4.png)

## 环境与数据准备

![](img/3d9d7ce1bd24c60024253efa02b91d54_5.png)

首先，我们需要安装必要的库并准备数据集。

```python
# 安装Transformers库
!pip install transformers
```

我们使用一个预处理好的电影评论CSV文件，其中包含评论文本和情感标签（正面为1，负面为0）。

![](img/3d9d7ce1bd24c60024253efa02b91d54_7.png)

```python
import pandas as pd

# 加载数据集
df = pd.read_csv(‘movie_reviews.csv’)
print(df.head())
```

数据集包含50，000条评论。我们将其划分为训练集（35，000条）、验证集（5，000条）和测试集（10，000条）。

![](img/3d9d7ce1bd24c60024253efa02b91d54_9.png)

## 模型与分词器

![](img/3d9d7ce1bd24c60024253efa02b91d54_11.png)

接下来，我们加载DistilBERT的分词器和预训练模型。分词器负责将文本转换为模型可理解的输入ID序列。

![](img/3d9d7ce1bd24c60024253efa02b91d54_13.png)

```python
from transformers import DistilBertTokenizer， DistilBertForSequenceClassification

# 加载分词器和模型
tokenizer = DistilBertTokenizer.from_pretrained(‘distilbert-base-uncased’)
model = DistilBertForSequenceClassification.from_pretrained(‘distilbert-base-uncased’)
```

![](img/3d9d7ce1bd24c60024253efa02b91d54_15.png)

**DistilBERT** 是BERT的一个轻量化版本，参数减少约40%，速度提升约60%，同时保留了约95%的原始性能。

## 数据预处理

![](img/3d9d7ce1bd24c60024253efa02b91d54_17.png)

我们需要使用分词器处理文本数据。分词器会将文本转换为ID序列，并自动进行填充或截断，使所有序列长度统一为512。

![](img/3d9d7ce1bd24c60024253efa02b91d54_19.png)

```python
# 对训练集文本进行编码
train_encodings = tokenizer(train_texts.tolist()， truncation=True， padding=True， max_length=512)
```

以下是创建PyTorch数据集的步骤：

![](img/3d9d7ce1bd24c60024253efa02b91d54_21.png)

1.  定义一个自定义数据集类，用于封装编码和标签。
2.  创建数据加载器，设置批处理大小为16（由于模型较大，需避免GPU内存溢出）。

![](img/3d9d7ce1bd24c60024253efa02b91d54_23.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_25.png)

```python
import torch
from torch.utils.data import Dataset， DataLoader

![](img/3d9d7ce1bd24c60024253efa02b91d54_27.png)

class ReviewDataset(Dataset):
    def __init__(self， encodings， labels):
        self.encodings = encodings
        self.labels = labels
    def __getitem__(self， idx):
        item = {key: torch.tensor(val[idx]) for key， val in self.encodings.items()}
        item[‘labels’] = torch.tensor(self.labels[idx])
        return item
    def __len__(self):
        return len(self.labels)

![](img/3d9d7ce1bd24c60024253efa02b91d54_29.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_30.png)

# 创建数据集和数据加载器
train_dataset = ReviewDataset(train_encodings， train_labels)
train_loader = DataLoader(train_dataset， batch_size=16， shuffle=True)
```

![](img/3d9d7ce1bd24c60024253efa02b91d54_32.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_33.png)

## 模型训练

我们使用Adam优化器进行模型微调。DistilBERT模型在正向传播时会返回一个元组，包含损失值和对数几率。

![](img/3d9d7ce1bd24c60024253efa02b91d54_35.png)

以下是训练循环的核心步骤：

1.  将输入ID、注意力掩码和标签送入模型。
2.  从模型输出中获取损失值，并执行反向传播。
3.  根据对数几率计算预测标签，并评估准确率。

![](img/3d9d7ce1bd24c60024253efa02b91d54_37.png)

```python
from transformers import AdamW

![](img/3d9d7ce1bd24c60024253efa02b91d54_39.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_41.png)

optimizer = AdamW(model.parameters()， lr=5e-5)

for epoch in range(3):
    model.train()
    for batch in train_loader:
        optimizer.zero_grad()
        input_ids = batch[‘input_ids’]
        attention_mask = batch[‘attention_mask’]
        labels = batch[‘labels’]
        outputs = model(input_ids， attention_mask=attention_mask， labels=labels)
        loss = outputs.loss
        loss.backward()
        optimizer.step()
```

训练过程较慢（约20分钟/轮次），但最终在测试集上达到了约92%的准确率。

![](img/3d9d7ce1bd24c60024253efa02b91d54_43.png)

## 性能对比与优化

![](img/3d9d7ce1bd24c60024253efa02b91d54_45.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_47.png)

为了对比，我们回顾了之前在Lecture 15中训练的LSTM模型。LSTM训练速度更快（约4-5分钟完成15轮），但测试准确率约为89%，略低于DistilBERT。

为了进一步提升DistilBERT的性能，我们引入了两项改进：

![](img/3d9d7ce1bd24c60024253efa02b91d54_49.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_51.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_52.png)

1.  使用 **AdamW** 优化器，它支持解耦权重衰减，是一种更有效的L2正则化方式。
2.  使用 **线性学习率调度器**，在每次迭代后动态调整学习率。

![](img/3d9d7ce1bd24c60024253efa02b91d54_54.png)

```python
from transformers import get_linear_schedule_with_warmup

![](img/3d9d7ce1bd24c60024253efa02b91d54_56.png)

# 创建优化器和调度器
optimizer = AdamW(model.parameters()， lr=5e-5， weight_decay=0.01)
scheduler = get_linear_schedule_with_warmup(optimizer， num_warmup_steps=0， num_training_steps=len(train_loader)*3)
```

![](img/3d9d7ce1bd24c60024253efa02b91d54_58.png)

应用这些优化后，模型测试准确率提升至约93%。

![](img/3d9d7ce1bd24c60024253efa02b91d54_59.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_60.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_62.png)

## 使用Trainer类简化流程

![](img/3d9d7ce1bd24c60024253efa02b91d54_64.png)

Hugging Face库提供了更高级的 `Trainer` 类，它封装了训练、评估和调度逻辑，使代码更加简洁。

![](img/3d9d7ce1bd24c60024253efa02b91d54_66.png)

以下是使用 `Trainer` 的示例：

```python
from transformers import Trainer， TrainingArguments

![](img/3d9d7ce1bd24c60024253efa02b91d54_68.png)

training_args = TrainingArguments(
    output_dir=‘./results’，
    num_train_epochs=3，
    per_device_train_batch_size=16，
    warmup_steps=500，
    weight_decay=0.01，
    logging_dir=‘./logs’，
)

![](img/3d9d7ce1bd24c60024253efa02b91d54_70.png)

trainer = Trainer(
    model=model，
    args=training_args，
    train_dataset=train_dataset，
    eval_dataset=val_dataset，
)

![](img/3d9d7ce1bd24c60024253efa02b91d54_72.png)

trainer.train()
```

![](img/3d9d7ce1bd24c60024253efa02b91d54_74.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_75.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_77.png)

使用 `Trainer` 类可以更方便地利用多GPU训练，并获得与手动训练循环相当甚至略好的性能。

![](img/3d9d7ce1bd24c60024253efa02b91d54_79.png)

## 总结

![](img/3d9d7ce1bd24c60024253efa02b91d54_81.png)

本节课中我们一起学习了如何使用Hugging Face的Transformers库微调预训练的DistilBERT模型，完成电影评论情感分类任务。我们涵盖了从数据预处理、模型加载、训练循环到性能优化的完整流程。关键点包括：

![](img/3d9d7ce1bd24c60024253efa02b91d54_83.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_84.png)

*   **DistilBERT** 是一个高效的轻量化BERT模型。
*   使用预训练模型进行**下游任务微调**是NLP中的常见做法。
*   **AdamW优化器**和**学习率调度器**可以提升模型性能。
*   Hugging Face的 **`Trainer` 类** 提供了训练过程的强大封装。

![](img/3d9d7ce1bd24c60024253efa02b91d54_85.png)

![](img/3d9d7ce1bd24c60024253efa02b91d54_87.png)

通过本教程，你掌握了使用现代Transformer模型解决实际文本分类任务的基本方法。Hugging Face库中还有更多模型（如GPT系列）和任务（如问答、文本生成）等待你去探索。