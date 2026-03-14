# 2：处理文本数据

![](img/d4ac733fd952997f5b89e860e95e4d9a_1.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_2.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_3.png)

在本节课中，我们将学习如何为训练大语言模型准备文本数据。我们将从原始文本开始，逐步将其转换为模型可以理解的数字向量。这个过程包括文本分词、构建词汇表、创建词嵌入以及添加位置信息。

![](img/d4ac733fd952997f5b89e860e95e4d9a_5.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_7.png)

---

![](img/d4ac733fd952997f5b89e860e95e4d9a_8.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_10.png)

## 2.1 概述：数据准备流程

![](img/d4ac733fd952997f5b89e860e95e4d9a_12.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_14.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_16.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_18.png)

构建大语言模型的第一步是准备数据集。在整个LLM构建流程中，数据准备是初始阶段，随后是预训练和微调。我们的目标是将原始文本处理成模型可以处理的格式。

![](img/d4ac733fd952997f5b89e860e95e4d9a_20.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_21.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_22.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_23.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_25.png)

大语言模型本身处理的是文本，但我们必须首先将其转换为模型能够理解的格式，即数学向量。向量是文本的数字表示。我们的目标是将字母和单词转换为数字表示，以便在后续的预训练中用于优化模型的权重参数。

![](img/d4ac733fd952997f5b89e860e95e4d9a_27.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_28.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_29.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_31.png)

## 2.2 文本分词

![](img/d4ac733fd952997f5b89e860e95e4d9a_33.png)

上一节我们介绍了数据准备的整体目标，本节中我们来看看如何将文本分解成更小的单元，这个过程称为分词。

分词是指将输入文本分解成独立的、更小的块。我们将使用一个名为`tiktoken`的库，但首先，让我们通过正则表达式来理解基本概念。

以下是使用正则表达式进行简单分词的方法：

```python
import re

text = "Hello, world. This is a test."
pattern = r"(\s+|[\.,!?;])"
tokens = re.split(pattern, text)
tokens = [token.strip() for token in tokens if token.strip()]
print(tokens)
```

![](img/d4ac733fd952997f5b89e860e95e4d9a_35.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_36.png)

这段代码会将文本按空白字符和标点符号分割成独立的标记。然而，这种方法对于复杂情况（如特殊字符）可能不够健壮。因此，我们需要一个更复杂的正则表达式。

```python
pattern = r"""'s|'t|'re|'ve|'m|'ll|'d| ?\p{L}+| ?\p{N}+| ?[^\s\p{L}\p{N}]+|\s+(?!\S)|\s+"""
tokens = re.findall(pattern, text, re.UNICODE)
tokens = [token.strip() for token in tokens if token.strip()]
print(tokens)
```

这个更复杂的正则表达式能更好地处理单词和特殊标记。我们将在真实文本上测试这个方法。

## 2.3 构建词汇表与生成标记ID

上一节我们学习了如何将文本分解成标记，本节中我们来看看如何将这些标记转换为唯一的整数ID，即标记ID。

第一步是构建一个词汇表。词汇表是每个可能出现的单词与一个唯一整数之间的映射。例如，对于句子“the quick brown fox jumps over the lazy dog”，我们首先将其分词，然后按字母顺序排序并去重，最后为每个唯一的标记分配一个整数。

以下是构建词汇表的Python代码：

```python
# 假设 preprocessed 是分词后的标记列表
preprocessed = ["the", "quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog"]
unique_words = sorted(set(preprocessed))
vocab = {word: idx for idx, word in enumerate(unique_words)}
print(vocab)
```

接下来，我们使用这个词汇表将任何新文本（包括训练文本）转换为标记ID。基本上，我们使用词汇表映射：对于文本中的每个单词，我们在词汇表中查找它并找到对应的整数。

让我们在Python中实现一个简单的分词器类：

```python
import re

class SimpleTokenizer:
    def __init__(self, vocab):
        self.str_to_int = vocab
        self.int_to_str = {i:s for s,i in vocab.items()}
        self.pattern = r"""'s|'t|'re|'ve|'m|'ll|'d| ?\p{L}+| ?\p{N}+| ?[^\s\p{L}\p{N}]+|\s+(?!\S)|\s+"""

    def encode(self, text):
        tokens = re.findall(self.pattern, text, re.UNICODE)
        tokens = [token.strip() for token in tokens if token.strip()]
        ids = [self.str_to_int[token] for token in tokens]
        return ids

    def decode(self, ids):
        text = " ".join([self.int_to_str[i] for i in ids])
        return text

# 使用分词器
tokenizer = SimpleTokenizer(vocab)
text = "the quick brown"
ids = tokenizer.encode(text)
print(ids)
decoded_text = tokenizer.decode(ids)
print(decoded_text)
```

这个简单的分词器演示了如何将文本转换为整数表示，以及如何反向转换。

## 2.4 添加特殊标记

上一节我们创建了基于训练集中所有不同标记的词汇表，并用它来生成标记ID。本节中，我们将讨论如何使用特殊标记来扩展这个词汇表。

例如，我们可能希望为训练集中未出现的未知单词添加一个标记，或者使用一个特殊的“文本结束”标记来指示文本的结尾。我们还需要处理简单分词器的一个缺点：如果遇到一个不在词汇表中的单词，它会引发错误。

以下是扩展词汇表以包含特殊标记的方法：

```python
# 原始唯一标记列表
all_tokens = list(unique_words)
# 添加特殊标记
special_tokens = ["<|endoftext|>", "<|unk|>"]
all_tokens.extend(special_tokens)

# 重新构建词汇表
vocab_v2 = {token: idx for idx, token in enumerate(sorted(set(all_tokens)))}
print(f"词汇表大小: {len(vocab_v2)}")
print(list(vocab_v2.items())[-5:]) # 查看最后五个条目
```

现在，我们可以修改我们的简单分词器来处理这些特殊标记：

```python
class SimpleTokenizerV2(SimpleTokenizer):
    def encode(self, text):
        tokens = re.findall(self.pattern, text, re.UNICODE)
        tokens = [token.strip() for token in tokens if token.strip()]
        ids = []
        for token in tokens:
            if token in self.str_to_int:
                ids.append(self.str_to_int[token])
            else:
                ids.append(self.str_to_int["<|unk|>"]) # 使用未知标记
        return ids

tokenizer_v2 = SimpleTokenizerV2(vocab_v2)
text_with_unknown = "Hello, this is a test with unknownword."
ids_v2 = tokenizer_v2.encode(text_with_unknown)
print(ids_v2)
decoded_v2 = tokenizer_v2.decode(ids_v2)
print(decoded_v2)
```

现在，未知单词被替换为 `<|unk|>` 占位符标记。然而，这种方法的主要缺点是对于LLM来说，所有未知单词看起来都一样，因为它收到的是相同的标记ID。在现实任务中，如果文本中有多个未知标记，LLM将无法知道它们指的是什么。

## 2.5 字节对编码

![](img/d4ac733fd952997f5b89e860e95e4d9a_38.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_39.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_40.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_41.png)

上一节我们讨论了简单分词器的局限性，本节中我们来看看字节对编码算法，它可以将我们的分词器提升到一个新的水平。

BPE是一种已有约30年历史的算法，如今在实现分词器时非常流行。例如，GPT-1、2、3和4都使用BPE算法进行分词。BPE算法帮助我们解决了简单分词器的一个主要缺点：处理未知单词。

BPE算法能够将任何类型的单词分解为子标记。它总是可以将较长的未知单词分解为已知的子词。如果遇到训练数据中完全不存在的字符串，它将回退到使用单个字符作为标记。这样，它永远不会失败，因为它总是可以回退到这些单个字符。

在实践中，我们将使用一个名为 `tiktoken` 的开源库，它来自OpenAI。本书后面的章节将从头开始实现所有内容，但分词器本身并不是LLM的核心部分，因此我们将使用 `tiktoken` 库，因为它高效且实用。

以下是使用 `tiktoken` 库的方法：

```python
import tiktoken

# 实例化GPT-2分词器
tokenizer_gpt2 = tiktoken.get_encoding("gpt2")
# 编码文本
text = "Hello, this is a test with someunknownword."
# 注意：需要处理特殊标记，如 endoftext
ids_gpt2 = tokenizer_gpt2.encode(text, allowed_special={"<|endoftext|>"})
print(ids_gpt2)
# 解码回文本
decoded_gpt2 = tokenizer_gpt2.decode(ids_gpt2)
print(decoded_gpt2)
```

`tiktoken` 库使用BPE算法，可以有效地将任意文本分解为子标记。如果你对BPE的实现细节感兴趣，可以查看OpenAI开源的代码或作者在GitHub上提供的从头实现。

![](img/d4ac733fd952997f5b89e860e95e4d9a_43.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_45.png)

## 2.6 使用滑动窗口进行数据采样

上一节我们讨论了如何从给定文本创建子词标记并将其转换为标记ID，本节中我们来看看如何高效地将这些标记ID以小块的形式提供给LLM。

![](img/d4ac733fd952997f5b89e860e95e4d9a_47.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_48.png)

LLM不能一次接收所有标记作为输入。本节的议题是如何高效地将这些标记ID的小块提供给LLM，以便后续高效地训练模型。

LLM的一个标志性特征是它们一次预测一个标记。如果我们有一个输入文本，LLM学习一次预测一个单词。目标是教LLM一次预测一个单词。在传统机器学习中，我们通常需要人工创建标注数据，但在这里，使用原始文本很容易，因为标签是通过成为下一个标记自动创建的。

我们将为我们的数据集高效地实现这一点。我们将使用一个上下文大小为4的块（为了可视化），但在实践中，这些块通常更大（例如1024个标记）。

以下是准备数据集的代码：

```python
import torch
from torch.utils.data import Dataset, DataLoader

class TextDataset(Dataset):
    def __init__(self, text, tokenizer, max_length=4, stride=4):
        self.tokenizer = tokenizer
        self.input_ids = tokenizer.encode(text, allowed_special={"<|endoftext|>"})
        self.max_length = max_length
        self.stride = stride
        self.inputs = []
        self.targets = []
        # 创建输入和目标块
        for i in range(0, len(self.input_ids) - max_length, stride):
            self.inputs.append(self.input_ids[i:i+max_length])
            self.targets.append(self.input_ids[i+1:i+max_length+1])

    def __len__(self):
        return len(self.inputs)

    def __getitem__(self, idx):
        return torch.tensor(self.inputs[idx]), torch.tensor(self.targets[idx])

def create_dataloader(text, batch_size=8, max_length=4, stride=4, shuffle=True, drop_last=True):
    tokenizer = tiktoken.get_encoding("gpt2")
    dataset = TextDataset(text, tokenizer, max_length, stride)
    dataloader = DataLoader(dataset, batch_size=batch_size, shuffle=shuffle, drop_last=drop_last, num_workers=0)
    return dataloader

# 使用示例
dataloader = create_dataloader(raw_text, batch_size=8, max_length=4, stride=4)
# 获取一个批次
for inputs, targets in dataloader:
    print("输入:", inputs.shape, inputs)
    print("目标:", targets.shape, targets)
    break
```

我们创建了一个数据集，其中目标是输入向右移动一个位置。这有助于我们将下一个标记作为LLM的目标。我们使用PyTorch的`DataLoader`来高效地创建批次、打乱数据并使用多个后台进程。

## 2.7 创建标记嵌入

上一节我们讨论了很多关于创建标记ID的内容，现在让我们更进一步，讨论如何创建标记嵌入。

![](img/d4ac733fd952997f5b89e860e95e4d9a_50.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_51.png)

回顾一下之前的流程：输入文本被转换为分词后的文本，分词后的文本被转换为标记ID。下一步是从标记ID创建这些标记嵌入。本质上，是获取一个整数值并将其转换为嵌入向量。这个嵌入向量包含浮点数，随后可以被优化。

以下是创建标记嵌入的示例：

```python
import torch
import torch.nn as nn

# 示例标记ID
input_ids = torch.tensor([2, 3, 5, 1])
# 创建嵌入层
vocab_size = 50257  # GPT-2分词器的词汇表大小
output_dim = 256     # 嵌入向量维度
embedding_layer = nn.Embedding(vocab_size, output_dim)
# 设置随机种子以便复现
torch.manual_seed(123)
# 获取嵌入
token_embeddings = embedding_layer(input_ids)
print(token_embeddings.shape)  # 应为 torch.Size([4, 256])
print(token_embeddings[0])     # 第一个标记的256维向量
```

嵌入层是LLM本身的一部分。它有一个权重矩阵，其大小为词汇表大小乘以输出维度。当我们调用嵌入层并传入输入ID时，我们是从这个矩阵中提取向量。这些数字最初是随机的，在后续的LLM训练中会被优化。

## 2.8 添加位置信息

![](img/d4ac733fd952997f5b89e860e95e4d9a_53.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_55.png)

现在我们已经理解了词嵌入，让我们更进一步，添加位置信息。

我们使用与之前相同的嵌入类，但现在我们除了词嵌入层之外，还有一个位置嵌入层，用于添加位置信息。目标是让实际的输入嵌入根据其位置而有所不同。

GPT-2使用了一种简单的方法：它使用了第二个嵌入层。以下是实现方法：

```python
class EmbeddingsWithPosition(nn.Module):
    def __init__(self, vocab_size, output_dim, max_length):
        super().__init__()
        self.token_embedding = nn.Embedding(vocab_size, output_dim)
        self.position_embedding = nn.Embedding(max_length, output_dim)
        self.max_length = max_length

    def forward(self, input_ids):
        # input_ids 形状: (batch_size, seq_len)
        batch_size, seq_len = input_ids.shape
        # 创建位置ID: 0, 1, 2, ..., seq_len-1
        position_ids = torch.arange(seq_len, device=input_ids.device).unsqueeze(0) # 形状: (1, seq_len)
        # 获取词嵌入和位置嵌入
        token_embeds = self.token_embedding(input_ids)  # 形状: (batch_size, seq_len, output_dim)
        position_embeds = self.position_embedding(position_ids) # 形状: (1, seq_len, output_dim)
        # 相加 (PyTorch广播机制会将 position_embeds 加到每个批次上)
        input_embeddings = token_embeds + position_embeds
        return input_embeddings

# 使用示例
vocab_size = 50257
output_dim = 256
max_length = 4  # 支持的最大输入标记数
embeddings_layer = EmbeddingsWithPosition(vocab_size, output_dim, max_length)
# 假设有一个批次的数据
batch_input_ids = torch.tensor([[2, 3, 5, 1], [1, 4, 3, 2]])  # 形状: (2, 4)
input_embeddings = embeddings_layer(batch_input_ids)
print(input_embeddings.shape)  # 应为 torch.Size([2, 4, 256])
```

位置嵌入矩阵与词嵌入矩阵类似，其数值在训练开始时也是随机的，并会在LLM训练过程中与词嵌入一起被优化。通过将它们相加，我们得到了包含位置信息的最终输入嵌入。

---

## 总结

在本节课中，我们一起学习了为训练大语言模型准备文本数据的完整流程。

![](img/d4ac733fd952997f5b89e860e95e4d9a_57.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_59.png)

1.  **文本分词**：使用正则表达式或BPE算法将原始文本分解成标记。
2.  **构建词汇表与标记ID**：创建唯一标记到整数的映射，并将文本转换为标记ID序列。
3.  **处理未知词与特殊标记**：扩展词汇表以包含特殊标记（如未知词标记和文本结束标记）。
4.  **字节对编码**：介绍了BPE算法，它是一种能够有效处理未知词、将其分解为子标记的健壮分词方法。
5.  **数据采样**：使用滑动窗口将长的标记ID序列分割成固定长度的上下文块，并构建输入-目标对，用于训练LLM预测下一个标记。
6.  **创建标记嵌入**：通过嵌入层将整数标记ID转换为连续的、可训练的向量表示。
7.  **添加位置嵌入**：为了给模型提供标记在序列中的顺序信息，我们添加了位置嵌入，并将其与标记嵌入相加，形成模型的最终输入。

![](img/d4ac733fd952997f5b89e860e95e4d9a_61.png)

![](img/d4ac733fd952997f5b89e860e95e4d9a_62.png)

至此，我们已经完成了数据准备的全部步骤，将原始文本成功转换成了模型可以接收并处理的数字向量形式。在接下来的章节中，我们将深入探讨GPT模型内部的核心机制——注意力机制。