# 005：准备数据 📊

![](img/f3f068b1d071ba200429dfe4484c2710_0.png)

在本节课中，我们将学习如何为大型语言模型的微调准备数据。我们将探讨数据准备的最佳实践、具体步骤，并通过代码示例展示如何对文本进行标记化、填充和截断处理。

---

## 概述

数据准备是微调过程中的关键步骤。高质量、多样化的数据能帮助模型更好地学习特定任务。本节将介绍数据准备的核心原则和具体操作流程。

---

## 数据准备的最佳实践

![](img/f3f068b1d071ba200429dfe4484c2710_2.png)

上一节我们介绍了微调的基本概念，本节中我们来看看准备数据时应遵循哪些最佳实践。

以下是数据准备的四个关键原则：

1.  **高质量数据**：提供高质量的数据是微调的首要任务。低质量的输入会导致模型产生低质量的输出。
2.  **数据多样性**：数据应涵盖用例的多个方面。如果所有输入和输出都相同，模型可能会开始记忆数据，而非学习泛化模式。
3.  **真实数据优先**：虽然可以生成数据，但使用真实数据通常更有效。生成的数据可能包含固定模式，用其训练可能无法让模型学习新的表达方式。
4.  **数据量**：在大多数机器学习应用中，更多数据通常更好。但由于大模型已经过预训练，拥有海量基础知识，因此对于微调而言，数据质量、多样性和真实性比单纯的数据量更重要。

![](img/f3f068b1d071ba200429dfe4484c2710_4.png)

---

## 数据准备的步骤

了解了核心原则后，我们来看看将原始数据转化为模型可接受格式的具体步骤。

![](img/f3f068b1d071ba200429dfe4484c2710_6.png)

以下是数据准备的四个标准步骤：

1.  **收集指令-响应对**：首先收集任务相关的问答对或指令-响应对。
2.  **构建提示**：将这些对连接起来，或添加统一的提示模板。
3.  **标记化**：使用标记器将文本数据转换为数字序列。
4.  **数据集划分**：将处理好的数据划分为训练集和测试集。

![](img/f3f068b1d071ba200429dfe4484c2710_8.png)

---

## 理解标记化

![](img/f3f068b1d071ba200429dfe4484c2710_10.png)

标记化是准备数据的关键环节。它指的是将文本转换成模型能够理解的数字序列的过程。

![](img/f3f068b1d071ba200429dfe4484c2710_12.png)

标记器基于字符或子词的常见出现频率进行转换。例如，单词“fine-tuning”可能被拆分为多个标记，如 `['fine', '-', 'tuning']`，每个标记对应一个唯一的数字ID。

![](img/f3f068b1d071ba200429dfe4484c2710_14.png)

![](img/f3f068b1d071ba200429dfe4484c2710_16.png)

**核心概念**：每个模型都有其专用的标记器。使用错误的标记器会导致模型混淆，因为相同的数字可能代表不同的文本含义。标记化过程可以表示为以下伪代码：

```python
# 伪代码：标记化过程
token_ids = tokenizer.encode(text="你的文本")
decoded_text = tokenizer.decode(token_ids=token_ids)
```

---

## 动手实践：使用标记器

现在，让我们通过代码来具体看看如何使用标记器。我们将使用Hugging Face库提供的`AutoTokenizer`类。

![](img/f3f068b1d071ba200429dfe4484c2710_18.png)

首先导入必要的库并加载标记器。`AutoTokenizer`能根据模型名称自动选择正确的标记器。

![](img/f3f068b1d071ba200429dfe4484c2710_20.png)

```python
from transformers import AutoTokenizer

# 指定模型名称，加载对应的标记器
model_name = "your_model_name_here"
tokenizer = AutoTokenizer.from_pretrained(model_name)
```

![](img/f3f068b1d071ba200429dfe4484c2710_22.png)

接下来，我们对一段文本进行编码和解码，以验证标记化过程。

![](img/f3f068b1d071ba200429dfe4484c2710_24.png)

```python
# 对单条文本进行标记化
text = "Hi, how are you?"
encoded_text = tokenizer(text)
print("编码后的ID:", encoded_text['input_ids'])

![](img/f3f068b1d071ba200429dfe4484c2710_26.png)

![](img/f3f068b1d071ba200429dfe4484c2710_28.png)

# 将标记ID解码回文本
decoded_text = tokenizer.decode(encoded_text['input_ids'])
print("解码后的文本:", decoded_text)
```

处理批量文本时，我们需要确保每个序列的长度一致，以便模型处理。

![](img/f3f068b1d071ba200429dfe4484c2710_30.png)

![](img/f3f068b1d071ba200429dfe4484c2710_32.png)

```python
# 对批量文本进行标记化
text_list = ["Hi, how are you?", "I'm fine.", "Yes"]
batch_encoded = tokenizer(text_list)
print("批量编码结果:", batch_encoded)
```

![](img/f3f068b1d071ba200429dfe4484c2710_34.png)

由于批量中文本长度可能不同，我们需要进行**填充**和**截断**。

![](img/f3f068b1d071ba200429dfe4484c2710_36.png)

```python
# 应用填充和截断
processed_batch = tokenizer(
    text_list,
    padding=True,        # 启用填充
    truncation=True,     # 启用截断
    max_length=10,       # 设置最大长度
    return_tensors="pt"  # 返回PyTorch张量
)
print("处理后的批量数据:", processed_batch)
```

![](img/f3f068b1d071ba200429dfe4484c2710_38.png)

**填充**通过在序列末尾添加特定的“填充标记”（如0）使所有序列达到相同长度。**截断**则通过裁剪序列使其不超过最大长度。可以指定从左侧或右侧截断，这取决于任务需求。

![](img/f3f068b1d071ba200429dfe4484c2710_40.png)

---

![](img/f3f068b1d071ba200429dfe4484c2710_42.png)

## 准备完整的数据集

![](img/f3f068b1d071ba200429dfe4484c2710_44.png)

![](img/f3f068b1d071ba200429dfe4484c2710_45.png)

我们将上述步骤整合，创建一个函数来处理整个数据集。

![](img/f3f068b1d071ba200429dfe4484c2710_47.png)

首先，加载包含问答对的数据集，并构建提示。

![](img/f3f068b1d071ba200429dfe4484c2710_49.png)

```python
def prepare_prompt(example):
    """将问答对组合成提示格式。"""
    prompt = f"### Question: {example['question']}\n ### Answer: {example['answer']}"
    return {"prompt": prompt}

![](img/f3f068b1d071ba200429dfe4484c2710_51.png)

# 假设 `dataset` 是加载好的数据集
dataset = dataset.map(prepare_prompt)
```

然后，定义标记化函数并应用于数据集。

![](img/f3f068b1d071ba200429dfe4484c2710_53.png)

![](img/f3f068b1d071ba200429dfe4484c2710_55.png)

```python
def tokenize_function(examples):
    """对数据集进行标记化。"""
    # 设置最大长度，并进行填充与截断
    tokenized_inputs = tokenizer(
        examples["prompt"],
        truncation=True,
        padding="max_length",
        max_length=512
    )
    return tokenized_inputs

![](img/f3f068b1d071ba200429dfe4484c2710_57.png)

# 应用标记化函数
tokenized_dataset = dataset.map(tokenize_function, batched=True)
```

最后，将数据集划分为训练集和测试集。

![](img/f3f068b1d071ba200429dfe4484c2710_59.png)

```python
# 划分数据集
split_dataset = tokenized_dataset.train_test_split(test_size=0.1, shuffle=True)
train_dataset = split_dataset["train"]
test_dataset = split_dataset["test"]

![](img/f3f068b1d071ba200429dfe4484c2710_61.png)

print(f"训练集大小: {len(train_dataset)}")
print(f"测试集大小: {len(test_dataset)}")
```

Hugging Face提供了许多有趣的数据集可供练习，例如关于泰勒·斯威夫特或开源大模型的数据集，你可以下载并尝试用自己的数据微调模型。

![](img/f3f068b1d071ba200429dfe4484c2710_63.png)

---

![](img/f3f068b1d071ba200429dfe4484c2710_65.png)

## 总结

本节课中我们一起学习了为大型语言模型微调准备数据的全过程。我们首先明确了高质量、多样化、真实数据优先的原则，然后逐步演练了从收集数据、构建提示、进行标记化（包括填充与截断）到最终划分数据集的完整流程。正确的数据准备是模型能否成功学习特定任务的基础。在接下来的实践中，你将应用这些知识来准备自己的微调数据。