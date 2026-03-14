# Hugging Face速成指南！P4：L4- 基于PyTorch的分类实现 🧠

在本节课中，我们将学习如何手动调用Hugging Face的预训练模型进行文本分类推理。我们将使用PyTorch框架，逐步完成从加载模型、处理输入到获取预测结果的完整流程。

![](img/6f843910cc38c033063518b8167635a3_1.png)

---

![](img/6f843910cc38c033063518b8167635a3_2.png)

![](img/6f843910cc38c033063518b8167635a3_3.png)

## 手动模型推理

![](img/6f843910cc38c033063518b8167635a3_4.png)

![](img/6f843910cc38c033063518b8167635a3_5.png)

上一节我们介绍了如何使用`pipeline`快速进行预测。本节中，我们来看看如何手动调用模型和分词器，以便更深入地理解其工作原理，并为后续的模型微调打下基础。

进行推理时，我们需要禁用梯度跟踪以节省内存和计算资源。在PyTorch中，这通过`torch.no_grad()`上下文管理器实现。

```python
import torch

![](img/6f843910cc38c033063518b8167635a3_7.png)

![](img/6f843910cc38c033063518b8167635a3_8.png)

with torch.no_grad():
    # 模型推理代码将放在这里
    pass
```

![](img/6f843910cc38c033063518b8167635a3_9.png)

![](img/6f843910cc38c033063518b8167635a3_10.png)

在`torch.no_grad()`环境下，我们可以调用模型。模型的输入通常是一个由分词器返回的字典。对于PyTorch模型，我们需要使用`**`操作符来解包这个字典。

![](img/6f843910cc38c033063518b8167635a3_11.png)

![](img/6f843910cc38c033063518b8167635a3_12.png)

```python
outputs = model(**batch)
```

以下是手动进行模型推理并处理输出的完整步骤：

1.  **获取模型原始输出**：调用模型后，我们得到包含`logits`等信息的输出对象。
2.  **计算预测概率**：对`logits`应用`softmax`函数，将其转换为概率分布。
3.  **获取预测标签**：使用`argmax`函数找到概率最高的类别索引。
4.  **转换标签ID**：将数字标签ID映射回可读的类别名称（如“正面”、“负面”）。

![](img/6f843910cc38c033063518b8167635a3_14.png)

![](img/6f843910cc38c033063518b8167635a3_15.png)

```python
# 应用softmax获取概率
import torch.nn.functional as F
predictions = F.softmax(outputs.logits, dim=1)

![](img/6f843910cc38c033063518b8167635a3_16.png)

![](img/6f843910cc38c033063518b8167635a3_17.png)

# 获取预测的标签ID
predicted_label_ids = torch.argmax(predictions, dim=1)

![](img/6f843910cc38c033063518b8167635a3_18.png)

# 将标签ID转换为可读名称
labels = [model.config.id2label[label_id] for label_id in predicted_label_ids.tolist()]
```

![](img/6f843910cc38c033063518b8167635a3_20.png)

## 计算损失

![](img/6f843910cc38c033063518b8167635a3_21.png)

如果你想在推理时也计算损失（例如，用于验证集评估），可以为模型提供真实的标签。`AutoModelForSequenceClassification`这类任务特定模型通常接受`labels`参数。

```python
# 假设真实标签为 [1, 0]
true_labels = torch.tensor([1, 0])
outputs_with_loss = model(**batch, labels=true_labels)
loss = outputs_with_loss.loss
```
**注意**：`labels`参数和自动计算损失的功能通常是`AutoModelForSequenceClassification`这类针对特定任务封装的模型才提供的，通用的`AutoModel`可能不包含此功能。

![](img/6f843910cc38c033063518b8167635a3_23.png)

## 与Pipeline对比

![](img/6f843910cc38c033063518b8167635a3_24.png)

![](img/6f843910cc38c033063518b8167635a3_25.png)

现在，让我们对比一下手动调用和`pipeline`的差异。

![](img/6f843910cc38c033063518b8167635a3_26.png)

使用`pipeline`仅需两行代码，即可直接获得整理好的预测结果（标签和置信度分数）。

```python
from transformers import pipeline
classifier = pipeline("sentiment-analysis")
result = classifier("I love this tutorial!")
```

![](img/6f843910cc38c033063518b8167635a3_28.png)

![](img/6f843910cc38c033063518b8167635a3_29.png)

而手动调用虽然步骤更多，但提供了更大的灵活性和对流程的完全控制。这在需要对模型进行**微调**或**自定义推理逻辑**时至关重要。

---

## 总结

本节课中我们一起学习了如何基于PyTorch手动实现Hugging Face模型的分类推理流程。

![](img/6f843910cc38c033063518b8167635a3_31.png)

我们掌握了以下核心操作：
*   使用`torch.no_grad()`禁用梯度。
*   使用`model(**batch)`调用模型并解包输入。
*   使用`F.softmax(outputs.logits, dim=1)`将原始输出转换为概率。
*   使用`torch.argmax()`获取预测标签ID，并通过`model.config.id2label`映射为可读名称。
*   了解了通过提供`labels`参数可以计算损失。
*   对比了手动调用与`pipeline`的便捷性与灵活性差异。

![](img/6f843910cc38c033063518b8167635a3_32.png)

![](img/6f843910cc38c033063518b8167635a3_33.png)

理解这些手动步骤是进行模型微调和高级应用的基础。在接下来的课程中，我们将利用这些知识，开始学习如何训练和微调属于自己的模型。