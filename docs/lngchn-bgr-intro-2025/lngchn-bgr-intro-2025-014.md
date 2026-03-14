# 014：链基础 🧩

在本节课中，我们将要学习Langchain中“链”的基础概念。链允许我们将多个任务（如创建提示词、调用大语言模型、解析输出）串联成一个流畅的工作流程，从而简化代码并提高可读性。

## 概述

上一节我们介绍了提示词模板的创建。本节中，我们来看看如何利用“链”将这些独立的步骤高效地组合起来。

## 初始化模型与提示词模板

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_1.png)

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_2.png)

首先，我们需要初始化大语言模型并创建一个提示词模板。这与之前章节的步骤类似。

```python
# 初始化OpenAI聊天模型
from langchain_openai import ChatOpenAI
model = ChatOpenAI(model="gpt-3.5-turbo")

# 使用消息列表创建提示词模板
from langchain_core.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_messages([
    ("system", "你是一位动物专家，了解任何动物的知识，例如猫、狗、大象等。"),
    ("human", "告诉我关于{animal}的{count}个事实。")
])
```

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_4.png)

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_5.png)

我们采用这种方式创建模板，是为了在模板中包含更多信息，例如系统消息和人类用户消息。系统消息定义了AI的角色，而人类消息则包含了需要填充的占位符，如 `{animal}` 和 `{count}`。

## 传统分步调用方法

在之前的章节中，我们采用分步调用的方式：

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_7.png)

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_8.png)

1.  调用提示词模板填充占位符。
2.  将填充后的提示词传递给大语言模型。
3.  处理模型的响应。

虽然这是正确的流程，但我们需要重复调用 `.invoke()` 方法，代码显得较为繁琐。

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_10.png)

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_12.png)

## 使用链简化流程

链的核心思想是使用管道操作符 `|` 将不同的任务连接起来。这使代码更加简洁和易读。

以下是创建和调用链的步骤：

1.  **构建链**：使用 `|` 操作符将提示词模板、模型和一个输出解析器串联起来。
2.  **调用链**：使用 `chain.invoke()` 方法，并传入一个包含所有占位符值的字典。
3.  **传递数据**：传入的字典数据在整个链的所有任务中都是可用的。

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_14.png)

以下是具体代码示例：

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_15.png)

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_16.png)

```python
from langchain_core.output_parsers import StrOutputParser

# 创建链：提示词模板 -> 模型 -> 输出解析器
chain = prompt | model | StrOutputParser()

# 调用链，传入占位符的值
response = chain.invoke({"animal": "猫", "count": "2"})
print(response)
```

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_18.png)

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_19.png)

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_20.png)

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_21.png)

在这个链中：
*   第一个任务 `prompt` 接收输入并生成完整的提示词。
*   第二个任务 `model` 接收上一步的提示词并生成AI响应。
*   第三个任务 `StrOutputParser()` 是一个内置函数，它从模型返回的复杂响应对象中提取出纯文本内容。

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_23.png)

运行上述代码，我们将得到关于猫的两个事实。如果我们改变输入，例如 `{"animal": "大象", "count": "1"}`，链将相应地输出关于大象的一个事实。

## 链的优势：代码对比

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_25.png)

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_26.png)

通过链，我们用几行代码就完成了提示词生成、模型调用和输出解析的整个流程。

相比之下，传统的分步调用方法需要编写更多的中间步骤和变量赋值代码。链显著简化了复杂工作流的构建，使代码意图更清晰，更易于维护。

## 总结与下节预告

本节课中我们一起学习了Langchain中“链”的基础用法。我们了解了如何将提示词模板、大语言模型和输出解析器连接成一个连贯的工作流，并看到了链如何使代码变得更加简洁高效。

链的核心优势在于其**可组合性**和**简洁性**，公式可以概括为：`chain = task1 | task2 | task3 ...`

![](img/0091c8e5d860bb356abcd0bf84aa2e0f_28.png)

在下一节中，我们将探索链的内部工作原理。这将帮助我们理解如何创建自定义任务。例如，本节我们使用了预置的 `StrOutputParser` 来提取文本内容，但有时我们可能需要编写自己的解析函数来实现更特定的逻辑。为了做到这一点，我们需要理解链在底层是如何运作的。