# 002：01_概述 🎯

![](img/54bcd02b89e9efb5f56167fe92fbf95a_1.png)

![](img/54bcd02b89e9efb5f56167fe92fbf95a_2.png)

在本节课中，我们将介绍一个贯穿整个课程使用的LLM提示词和响应数据集。我们将学习如何检测数据泄露、提示词注入和幻觉等问题，这些技术将在后续课程中详细探讨。

## 课程设置与数据导入

首先，我们需要进行一些准备工作。我们将导入一个辅助模块，它提供了一些可视化和数据探索工具，以及评估我们指标的功能。

接下来，我们导入pandas库。我创建了一个包含用户提示词和LLM响应的数据集，并将它们标记为“正常”或存在特定问题，例如拒绝、越狱、幻觉、毒性或数据泄露。

以下是数据集的前几行预览：

```python
import pandas as pd
chats = pd.read_csv('../chats.csv')
print(chats.head())
```

数据集包含`prompt`和`response`两列。这些提示词和响应收集自一个LLM，特别是OpenAI的GPT-3.5 Turbo。我们将使用它们进行评估。请注意，这个数据集并不具有普遍代表性，它包含了许多我们特意寻找的特殊案例。

## 数据记录与初步分析

我们将使用`whylogs`，一个用于捕获机器学习数据的开源Python日志库。为了计算针对文本和LLM的特定指标，我们使用了构建在`whylogs`之上的开源`langkit`包。

![](img/54bcd02b89e9efb5f56167fe92fbf95a_4.png)

![](img/54bcd02b89e9efb5f56167fe92fbf95a_6.png)

```python
import whylogs as why
from langkit import llm_schema

![](img/54bcd02b89e9efb5f56167fe92fbf95a_8.png)

# 使用LLM模式记录数据
profile = why.log(chats, name="LLM_chats_dataset", schema=llm_schema).profile()
```

![](img/54bcd02b89e9efb5f56167fe92fbf95a_10.png)

数据记录完成后，我们可以查看可视化报告，确认数据集中有68行数据。在“洞察与概况”页面，我们可以看到LLM指标设置自动收集的一系列指标。

## 识别幻觉问题

从高层次看，**幻觉**是指LLM产生的**不准确**或**不相关**的响应。即使答案本身正确，但如果与问题无关，也属于幻觉。幻觉的一个特点是它们通常看起来**真实可信**——一段可读、连贯、看起来像是有效回应的文本。

目前，让我们先看看**提示词-响应相关性**。从业者衡量相关性的一种常见方法是，查看LLM的响应与其收到的提示词之间的相似度。我们使用句子嵌入的**余弦相似度**来实现。

```python
from langkit import input_output
# 使用辅助函数可视化“响应与提示词的相关性”指标
input_output.visualize_metric(chats, metric_name='response_relevance_to_prompt')
```

得分接近0的低分更可能是幻觉。然而，语义相似性与相关性相关，但并非等同。有时一个好的答案可能不使用相同的语言（例如，问“牛怎么叫？”，答“哞”），而一个使用大量相关词汇的答案可能并未直接回答问题。

提示词-响应相关性并非衡量幻觉的唯一指标。在后续课程中，我们将探讨更先进、更新的方法，例如**响应自相似性**（如SelfCheckGPT），即比较LLM对同一提示词多次响应的相似度。

## 检测数据泄露与毒性

接下来，我们看看数据泄露和毒性问题。

检测数据泄露的常见方法仍然是使用**正则表达式**进行字符串模式匹配。电话号码、电子邮件地址等个人可识别信息具有很好的结构性，适合用正则表达式检测。

```python
from langkit import data_leakage
# 可视化数据泄露指标
data_leakage.visualize_metric(chats, metric_name='data_leakage')
```

在我们的数据集中，可以看到电子邮件地址、电话号码、邮寄地址和社会安全号码。同样，在响应中也能看到信用卡号。

**毒性**可以包含多种内容，主要指明确的毒性语言，如涉及种族、性别、脏话或恶意词汇。

```python
from langkit import toxicity
# 可视化毒性指标
toxicity.visualize_metric(chats, metric_name='toxicity')
```

我们可以看到，提示词和响应的毒性分布都呈现长尾特征，大多数值为0，只有少数具有较高值。

## 识别拒绝与提示词注入

有时你会看到LLM回应“抱歉，我无法回答这个问题”或“我无法帮助处理这类请求”。这是一种**拒绝**，即LLM检测到提示词可能要求它做其未被设定去做的事情。

黑客可能会尝试通过巧妙的提示词绕过这些拒绝，诱使LLM提供其通常应拒绝的信息。这种尝试被称为**越狱**。越狱是**提示词注入**的一种特定类型。提示词注入泛指任何试图让LLM执行其设计者未预期行为的提示词。

```python
from langkit import injections
# 可视化注入（越狱）指标
injections.visualize_metric(chats, metric_name='jail_break')
```

在分布中，可以看到大量接近1或0的值，这是因为模型对许多示例的判断非常确信。这个数据集为了学习目的，过度代表了越狱案例，在真实世界数据集中它们会非常罕见。

## 评估指标性能

在构建指标的过程中，我们需要检查检测问题示例的效果。为此，我使用`whylogs`创建了一个仪表板来评估性能。

我们可以先尝试一个简单的指标，例如在响应中查找特定词汇（如“sorry”），看看能发现哪些示例，并将其传递给评估器。

```python
# 筛选包含“sorry”的响应
sorry_chats = chats[chats['response'].str.contains('sorry', case=False, na=False)]
# 将筛选结果传递给评估器
evaluator.evaluate(sorry_chats)
```

通过这个简单方法，我们找到了所有简单的拒绝示例，但仍有更复杂的案例需要使用更高级的方法来发现。我鼓励你尝试新的筛选条件，例如筛选提示词长度超过250个字符的示例，看看能发现哪些不同类型的问题。

![](img/54bcd02b89e9efb5f56167fe92fbf95a_12.png)

![](img/54bcd02b89e9efb5f56167fe92fbf95a_13.png)

## 总结

![](img/54bcd02b89e9efb5f56167fe92fbf95a_15.png)

本节课中，我们一起学习了LLM应用程序中常见的问题类型：**幻觉**、**数据泄露**、**毒性**、**拒绝**和**提示词注入**。我们介绍了用于检测这些问题的初步指标，并利用`langkit`和`whylogs`工具对示例数据集进行了初步分析和可视化。接下来的课程将深入探讨如何发现和创建新的、更有效的指标来精准识别这些问题，并通过所有测试。