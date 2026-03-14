# Hugging Face速成指南！P3：L3- 模型和分词器 🧠🔧

在本节课中，我们将学习如何直接使用Hugging Face的模型和分词器，并手动执行文本处理的各个步骤。这将为你提供更大的灵活性，让你更深入地理解其工作原理。

## 概述
我们将从调用分词器的基本函数开始，逐步探索如何将文本转换为模型可以理解的数值形式，并最终将这些数据输入模型进行预测。

![](img/4e0ddcb38791825272bb7f961349fc05_1.png)

![](img/4e0ddcb38791825272bb7f961349fc05_2.png)

---

![](img/4e0ddcb38791825272bb7f961349fc05_3.png)

![](img/4e0ddcb38791825272bb7f961349fc05_4.png)

## 分词器的基本操作

![](img/4e0ddcb38791825272bb7f961349fc05_5.png)

上一节我们介绍了如何加载预训练模型和分词器。本节中，我们来看看如何手动使用分词器处理文本。

![](img/4e0ddcb38791825272bb7f961349fc05_6.png)

![](img/4e0ddcb38791825272bb7f961349fc05_7.png)

首先，我们调用分词器的 `tokenize` 方法。这个方法接收一个字符串，并将其分割成更小的单元，我们称之为“令牌”。

![](img/4e0ddcb38791825272bb7f961349fc05_8.png)

```python
tokens = tokenizer.tokenize("这是一个示例句子。")
```

一旦我们得到了令牌列表，就可以使用 `convert_tokens_to_ids` 函数将这些令牌转换为对应的ID。这些ID是模型能够理解的数学表示。

```python
token_ids = tokenizer.convert_tokens_to_ids(tokens)
```

![](img/4e0ddcb38791825272bb7f961349fc05_10.png)

或者，我们可以直接调用分词器对象本身，它会返回一个包含多个键的字典，例如 `input_ids` 和 `attention_mask`。

![](img/4e0ddcb38791825272bb7f961349fc05_11.png)

![](img/4e0ddcb38791825272bb7f961349fc05_12.png)

```python
inputs = tokenizer("这是一个示例句子。")
```

![](img/4e0ddcb38791825272bb7f961349fc05_13.png)

![](img/4e0ddcb38791825272bb7f961349fc05_14.png)

现在，让我们打印这三个变量，观察它们之间的区别。

![](img/4e0ddcb38791825272bb7f961349fc05_15.png)

```python
print(“令牌列表:”, tokens)
print(“令牌ID列表:”, token_ids)
print(“分词器直接输出:”, inputs)
```

![](img/4e0ddcb38791825272bb7f961349fc05_16.png)

![](img/4e0ddcb38791825272bb7f961349fc05_17.png)

运行代码后，我们可以看到：
*   `tokenizer.tokenize()` 返回一个字符串列表，每个元素是一个令牌。
*   `tokenizer.convert_tokens_to_ids()` 返回一个整数列表，每个整数对应一个令牌的唯一ID。
*   直接调用 `tokenizer()` 返回一个字典，其中 `input_ids` 键对应的值与 `token_ids` 基本相同，但通常会在序列的开头和结尾添加特殊的起始（如101）和结束（如102）令牌。

这些 `input_ids` 就是我们稍后可以传递给模型进行预测的数值输入。

---

![](img/4e0ddcb38791825272bb7f961349fc05_19.png)

## 处理批量数据

在实际应用中，我们通常需要处理多个句子。以下是处理批量数据的方法。

我们可以创建一个包含多个句子的列表作为训练数据。

```python
X_train = [“这是第一个句子。”, “这是第二个稍长一些的句子。”]
```

![](img/4e0ddcb38791825272bb7f961349fc05_21.png)

![](img/4e0ddcb38791825272bb7f961349fc05_22.png)

然后，我们将这个列表传递给分词器。为了确保批次中的所有样本长度一致，我们需要设置一些关键参数。

![](img/4e0ddcb38791825272bb7f961349fc05_23.png)

```python
batch = tokenizer(
    X_train,
    padding=True,      # 对较短的序列进行填充
    truncation=True,   # 对过长的序列进行截断
    max_length=512,    # 设置最大序列长度
    return_tensors=“pt” # 返回PyTorch张量
)
```

![](img/4e0ddcb38791825272bb7f961349fc05_24.png)

以下是这些参数的作用：
*   `padding=True`： 确保所有序列长度相同，较短的序列会被填充。
*   `truncation=True`： 确保序列不会超过最大长度，过长的部分会被截断。
*   `max_length=512`： 定义序列的最大长度。
*   `return_tensors=“pt”`： 指定返回的数据类型为PyTorch张量。

现在，打印这个批次，看看它的结构。

```python
print(batch)
```

输出将是一个字典，包含 `input_ids` 和 `attention_mask` 两个键，每个键对应一个PyTorch张量。`input_ids` 张量包含了两个句子的令牌ID，`attention_mask` 则用于指示哪些位置是真实令牌（1），哪些是填充令牌（0）。

![](img/4e0ddcb38791825272bb7f961349fc05_26.png)

![](img/4e0ddcb38791825272bb7f961349fc05_27.png)

---

![](img/4e0ddcb38791825272bb7f961349fc05_28.png)

![](img/4e0ddcb38791825272bb7f961349fc05_29.png)

## 将数据输入模型

![](img/4e0ddcb38791825272bb7f961349fc05_30.png)

![](img/4e0ddcb38791825272bb7f961349fc05_31.png)

现在，我们已经准备好了模型可以理解的输入数据。接下来，就可以将这个批次数据传递给模型进行预测。

```python
outputs = model(**batch)
```

模型将处理这些输入ID，并返回相应的输出，例如分类任务的logits或序列标注任务的预测结果。

---

![](img/4e0ddcb38791825272bb7f961349fc05_33.png)

## 总结

![](img/4e0ddcb38791825272bb7f961349fc05_34.png)

![](img/4e0ddcb38791825272bb7f961349fc05_35.png)

本节课中，我们一起学习了如何手动使用Hugging Face的分词器和模型：
1.  我们使用 `tokenizer.tokenize()` 将文本分割成令牌。
2.  我们使用 `tokenizer.convert_tokens_to_ids()` 将令牌转换为数值ID。
3.  我们了解了直接调用 `tokenizer()` 会返回包含 `input_ids` 和 `attention_mask` 的字典。
4.  我们掌握了如何使用 `padding`, `truncation`, `max_length` 和 `return_tensors` 等参数来处理批量文本数据，使其符合模型的输入要求。
5.  最后，我们学会了如何将处理好的批次数据输入模型。

![](img/4e0ddcb38791825272bb7f961349fc05_36.png)

通过手动执行这些步骤，你能够更灵活地控制文本预处理流程，并为后续更复杂的自然语言处理任务打下坚实基础。