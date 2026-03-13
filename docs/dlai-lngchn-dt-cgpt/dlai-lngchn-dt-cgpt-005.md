# 005：5-准备数据 📊

![](img/04be87adea703493c4bb301f9c54bbfa_0.png)

在本节课中，我们将学习如何为微调大型语言模型准备数据。数据准备是模型训练过程中的关键步骤，直接影响到最终模型的质量和性能。

上一节我们探索了数据，本节中我们来看看如何具体地准备数据。

## 数据准备的最佳实践

准备高质量的训练数据是微调成功的第一要素。以下是几个核心的最佳实践：

![](img/04be87adea703493c4bb301f9c54bbfa_2.png)

1.  **高质量数据**：模型会模仿输入数据的模式。如果输入是低质量的“垃圾”，模型将尝试生成低质量的输出。因此，提供高质量的数据至关重要。
2.  **数据多样性**：数据应覆盖用例的各个方面。如果所有输入和输出都过于相似，模型可能会开始记忆这些模式，导致其只能重复相同的内容，缺乏泛化能力。
3.  **真实数据与生成数据**：虽然可以使用大型语言模型生成数据，但使用真实数据通常更有效，尤其是在写作类任务中。生成数据可能包含某些可被检测的模式，过多依赖生成数据可能限制模型学习新的表达方式。
4.  **数据量**：在大多数机器学习任务中，数据越多越好。但对于预训练过的大型语言模型来说，它们已经从海量互联网数据中获得了基础理解能力。因此，数据量虽然仍有帮助，但其重要性通常低于数据质量、多样性和真实性。

## 数据准备步骤

![](img/04be87adea703493c4bb301f9c54bbfa_4.png)

以下是准备数据的标准流程：

1.  **收集指令-响应对**：首先，需要收集成对的指令（或问题）和期望的响应（或答案）。
2.  **拼接与格式化**：将这些对连接起来，或按照特定格式（如前几节提到的提示模板）进行组织。
3.  **分词与标准化**：将文本数据转换为模型能够理解的数字标记（Token），并处理长度不一致的问题。
4.  **划分数据集**：将处理好的数据分割为训练集和测试集，用于模型训练和评估。

## 理解分词

![](img/04be87adea703493c4bb301f9c54bbfa_6.png)

分词是将文本转换为一系列数字标记的过程。这些标记不一定对应完整的单词，而是基于字符或子词的常见频率进行划分。

例如，单词 “fine-tuning” 可能被分词器拆分为 `[“fine”, “-“, “tuning”]`，并分别映射为特定的数字ID（如 `[278, ...]`）。使用相同的分词器解码这些ID，可以还原出原始文本。

![](img/04be87adea703493c4bb301f9c54bbfa_8.png)

**核心概念**：必须使用与目标模型相匹配的分词器。因为每个模型都在其特定的标记词汇表上进行了预训练，使用错误的分词器会导致模型无法正确理解输入。

## 动手实践：使用代码进行数据准备

![](img/04be87adea703493c4bb301f9c54bbfa_10.png)

让我们通过代码示例来具体了解如何实现数据准备。

![](img/04be87adea703493c4bb301f9c54bbfa_12.png)

![](img/04be87adea703493c4bb301f9c54bbfa_14.png)

首先，导入必要的库，特别是Hugging Face Transformers库中的 `AutoTokenizer` 类，它能根据模型名称自动加载正确的分词器。

![](img/04be87adea703493c4bb301f9c54bbfa_16.png)

```python
from transformers import AutoTokenizer

model_name = "your-model-name-here"  # 例如：一个70亿参数的模型
tokenizer = AutoTokenizer.from_pretrained(model_name)
```

### 基础分词

对一段文本进行分词和还原。

![](img/04be87adea703493c4bb301f9c54bbfa_18.png)

```python
text = "Hi, how are you?"
encoded_text = tokenizer(text)
print("编码后的ID:", encoded_text['input_ids'])

![](img/04be87adea703493c4bb301f9c54bbfa_20.png)

decoded_text = tokenizer.decode(encoded_text['input_ids'])
print("解码后的文本:", decoded_text)
```

![](img/04be87adea703493c4bb301f9c54bbfa_22.png)

### 批处理与填充

![](img/04be87adea703493c4bb301f9c54bbfa_24.png)

模型处理需要固定长度的输入。当批量处理不同长度的文本时，需要使用“填充”策略使它们长度一致。

![](img/04be87adea703493c4bb301f9c54bbfa_26.png)

![](img/04be87adea703493c4bb301f9c54bbfa_28.png)

```python
texts = ["Hi, how are you?", "I'm fine.", "Yes"]
batch_encodings = tokenizer(texts, padding=True)
print("填充后的批次编码:", batch_encodings['input_ids'])
```

![](img/04be87adea703493c4bb301f9c54bbfa_30.png)

### 截断处理

![](img/04be87adea703493c4bb301f9c54bbfa_32.png)

![](img/04be87adea703493c4bb301f9c54bbfa_34.png)

模型有最大输入长度限制。对于过长的文本，需要使用“截断”策略。

![](img/04be87adea703493c4bb301f9c54bbfa_36.png)

```python
# 从右侧截断到最大长度3
truncated_encoding = tokenizer(texts, truncation=True, max_length=3)
print("截断后的编码:", truncated_encoding['input_ids'])

![](img/04be87adea703493c4bb301f9c54bbfa_38.png)

# 可以指定从左侧截断
truncated_left_encoding = tokenizer(texts, truncation=True, max_length=3, truncation_side='left')
```

![](img/04be87adea703493c4bb301f9c54bbfa_40.png)

通常，我们会同时启用填充和截断。

![](img/04be87adea703493c4bb301f9c54bbfa_42.png)

```python
full_encoding = tokenizer(texts, padding=True, truncation=True, max_length=10)
```

![](img/04be87adea703493c4bb301f9c54bbfa_44.png)

![](img/04be87adea703493c4bb301f9c54bbfa_45.png)

### 处理真实数据集

![](img/04be87adea703493c4bb301f9c54bbfa_47.png)

假设我们有一个包含问答对的数据集。

![](img/04be87adea703493c4bb301f9c54bbfa_49.png)

```python
# 加载数据集 (示例)
from datasets import load_dataset
dataset = load_dataset('your-dataset-name')

![](img/04be87adea703493c4bb301f9c54bbfa_51.png)

# 定义一个格式化函数，将问答对拼接成提示
def format_instruction(example):
    example['text'] = f"### Question:\n{example['question']}\n\n### Answer:\n{example['answer']}"
    return example

![](img/04be87adea703493c4bb301f9c54bbfa_53.png)

dataset = dataset.map(format_instruction)

![](img/04be87adea703493c4bb301f9c54bbfa_55.png)

![](img/04be87adea703493c4bb301f9c54bbfa_57.png)

# 定义一个分词函数
def tokenize_function(examples):
    # 确定批次中的最大长度或模型最大长度
    model_max_length = tokenizer.model_max_length
    # 对‘text’字段进行分词，启用填充和截断
    tokenized_inputs = tokenizer(
        examples["text"],
        truncation=True,
        padding="max_length",
        max_length=min(model_max_length, 512), # 设置一个上限
    )
    # 对于因果语言模型，标签就是输入ID本身（用于计算下一个词的损失）
    tokenized_inputs["labels"] = tokenized_inputs["input_ids"].copy()
    return tokenized_inputs

# 对整个数据集应用分词函数
tokenized_dataset = dataset.map(tokenize_function, batched=True, remove_columns=dataset["train"].column_names)

![](img/04be87adea703493c4bb301f9c54bbfa_59.png)

# 分割数据集
split_dataset = tokenized_dataset["train"].train_test_split(test_size=0.1, shuffle=True, seed=42)
train_dataset = split_dataset["train"]
eval_dataset = split_dataset["test"]

![](img/04be87adea703493c4bb301f9c54bbfa_61.png)

print(f"训练集大小: {len(train_dataset)}")
print(f"评估集大小: {len(eval_dataset)}")
```

## 可选的数据集

![](img/04be87adea703493c4bb301f9c54bbfa_63.png)

除了商业问答数据集，你也可以尝试一些有趣的主题数据集进行练习，例如关于明星泰勒·斯威夫特或流行乐队BTS的数据集，这能让学习过程更有趣味性。

![](img/04be87adea703493c4bb301f9c54bbfa_65.png)

---

本节课中我们一起学习了为微调准备数据的关键步骤和最佳实践。我们了解了高质量、多样化数据的重要性，掌握了从收集指令对、格式化、分词到最终划分数据集的完整流程，并通过代码示例进行了实践。准备好数据后，我们就可以进入下一阶段——实际训练模型了。