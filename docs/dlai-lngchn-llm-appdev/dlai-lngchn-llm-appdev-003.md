# 003：记忆 🧠

![](img/6611f57afa341027c62f44f8188df02e_0.png)

![](img/6611f57afa341027c62f44f8188df02e_1.png)

在本节课中，我们将要学习LangChain中的“记忆”功能。大型语言模型本身是无状态的，不会记住之前的对话。为了构建能够进行连贯对话的聊天机器人等应用，我们需要一种机制来存储和利用对话历史。本节课将介绍LangChain提供的多种记忆管理方式。

![](img/6611f57afa341027c62f44f8188df02e_3.png)

## 概述

![](img/6611f57afa341027c62f44f8188df02e_5.png)

![](img/6611f57afa341027c62f44f8188df02e_7.png)

![](img/6611f57afa341027c62f44f8188df02e_9.png)

与大型语言模型互动时，它们默认不会记住之前的对话内容。这是一个问题，因为当你构建如聊天机器人等应用时，你希望对话能够连贯进行。因此，在这一节中我们将涵盖记忆功能，即如何记住对话的前一部分并将其输入语言模型。这样，当你与模型互动时，它们可以有这种对话流程。LangChain提供了多种高级选项来管理这些记忆。

![](img/6611f57afa341027c62f44f8188df02e_11.png)

![](img/6611f57afa341027c62f44f8188df02e_13.png)

## 环境设置与基础记忆

![](img/6611f57afa341027c62f44f8188df02e_15.png)

首先，我们需要导入必要的API密钥和工具。

![](img/6611f57afa341027c62f44f8188df02e_17.png)

```python
# 导入API密钥
import os
os.environ['OPENAI_API_KEY'] = 'your-api-key-here'

![](img/6611f57afa341027c62f44f8188df02e_19.png)

# 导入所需工具
from langchain.chains import ConversationChain
from langchain.chat_models import ChatOpenAI
from langchain.memory import ConversationBufferMemory
```

![](img/6611f57afa341027c62f44f8188df02e_21.png)

![](img/6611f57afa341027c62f44f8188df02e_23.png)

上一节我们介绍了环境设置，本节中我们来看看如何使用基础记忆功能。

![](img/6611f57afa341027c62f44f8188df02e_25.png)

我们将使用LangChain来管理聊天对话。为此，需要设置语言模型和记忆组件。

![](img/6611f57afa341027c62f44f8188df02e_27.png)

![](img/6611f57afa341027c62f44f8188df02e_29.png)

```python
# 设置语言模型和记忆
llm = ChatOpenAI(temperature=0)
memory = ConversationBufferMemory()
conversation = ConversationChain(llm=llm, memory=memory, verbose=False)
```

![](img/6611f57afa341027c62f44f8188df02e_31.png)

![](img/6611f57afa341027c62f44f8188df02e_33.png)

在这门短课中，我们不会深入探讨链的本质和LangChain的所有细节，现在不必太担心语法的细节。但这构建了一个可以对话的LLM应用。

![](img/6611f57afa341027c62f44f8188df02e_35.png)

![](img/6611f57afa341027c62f44f8188df02e_37.png)

现在，让我们开始一段对话。

![](img/6611f57afa341027c62f44f8188df02e_39.png)

![](img/6611f57afa341027c62f44f8188df02e_41.png)

```python
# 开始对话
response = conversation.predict(input="Hi, my name is Andrew.")
print(response)  # 输出可能是：你好，很高兴见到你，Andrew。

![](img/6611f57afa341027c62f44f8188df02e_43.png)

![](img/6611f57afa341027c62f44f8188df02e_45.png)

response = conversation.predict(input="What is 1+1?")
print(response)  # 输出：1加1等于2。

![](img/6611f57afa341027c62f44f8188df02e_47.png)

response = conversation.predict(input="What is my name?")
print(response)  # 输出：你的名字是Andrew，如你之前所说。
```

![](img/6611f57afa341027c62f44f8188df02e_49.png)

![](img/6611f57afa341027c62f44f8188df02e_51.png)

通过设置 `verbose=True`，你可以查看LangChain生成的实际提示，了解记忆是如何被整合到每次对话中的。

![](img/6611f57afa341027c62f44f8188df02e_53.png)

![](img/6611f57afa341027c62f44f8188df02e_55.png)

## 记忆的工作原理

![](img/6611f57afa341027c62f44f8188df02e_57.png)

![](img/6611f57afa341027c62f44f8188df02e_59.png)

上一节我们介绍了基础对话，本节中我们来看看记忆内部是如何存储的。

![](img/6611f57afa341027c62f44f8188df02e_61.png)

![](img/6611f57afa341027c62f44f8188df02e_63.png)

LangChain存储对话的方式是使用“对话缓冲内存”。我们可以打印出当前记忆的内容。

![](img/6611f57afa341027c62f44f8188df02e_65.png)

![](img/6611f57afa341027c62f44f8188df02e_67.png)

```python
# 查看记忆缓冲区
print(memory.buffer)
# 或者使用 load_memory_variables 方法
print(memory.load_memory_variables({}))
```

![](img/6611f57afa341027c62f44f8188df02e_69.png)

![](img/6611f57afa341027c62f44f8188df02e_71.png)

记忆缓冲区存储了到目前为止的所有对话。它只是AI或人类所说的一切内容的记录。

![](img/6611f57afa341027c62f44f8188df02e_73.png)

如果你想明确地向记忆中添加内容，可以手动保存上下文。

![](img/6611f57afa341027c62f44f8188df02e_75.png)

![](img/6611f57afa341027c62f44f8188df02e_77.png)

```python
# 手动保存上下文到记忆
memory.save_context({"input": "Hello"}, {"output": "What's up?"})
memory.save_context({"input": "Not much, just hanging"}, {"output": "Cool"})
print(memory.load_memory_variables({}))
```

![](img/6611f57afa341027c62f44f8188df02e_79.png)

![](img/6611f57afa341027c62f44f8188df02e_81.png)

![](img/6611f57afa341027c62f44f8188df02e_83.png)

当你使用大型语言模型进行聊天对话时，语言模型本身实际上是无状态的。每次API调用都是独立的。聊天机器人之所以能记住对话，是因为有代码提供了迄今为止的完整对话作为上下文。因此，记忆功能可以明确存储对话历史，并将其作为输入或附加上下文提供给模型，以便它们可以生成知道之前说了什么的回复。

![](img/6611f57afa341027c62f44f8188df02e_85.png)

## 不同类型的记忆

![](img/6611f57afa341027c62f44f8188df02e_87.png)

![](img/6611f57afa341027c62f44f8188df02e_89.png)

![](img/6611f57afa341027c62f44f8188df02e_91.png)

随着对话变长，所需内存量会变得非常大，发送大量令牌到语言模型的成本也会增加。因此，LangChain提供了几种方便的内存类型来存储和累积对话。

![](img/6611f57afa341027c62f44f8188df02e_93.png)

![](img/6611f57afa341027c62f44f8188df02e_95.png)

我们一直在看“对话缓冲内存”。让我们看看另一种类型的内存。

![](img/6611f57afa341027c62f44f8188df02e_97.png)

![](img/6611f57afa341027c62f44f8188df02e_99.png)

### 对话缓冲窗口内存

![](img/6611f57afa341027c62f44f8188df02e_101.png)

![](img/6611f57afa341027c62f44f8188df02e_103.png)

这种内存只保留最近的一段对话。

![](img/6611f57afa341027c62f44f8188df02e_105.png)

![](img/6611f57afa341027c62f44f8188df02e_107.png)

```python
from langchain.memory import ConversationBufferWindowMemory

![](img/6611f57afa341027c62f44f8188df02e_109.png)

![](img/6611f57afa341027c62f44f8188df02e_111.png)

# 只记住最近一次对话交换（k=1）
memory = ConversationBufferWindowMemory(k=1)
conversation = ConversationChain(llm=llm, memory=memory, verbose=False)

![](img/6611f57afa341027c62f44f8188df02e_113.png)

![](img/6611f57afa341027c62f44f8188df02e_115.png)

# 进行对话
conversation.predict(input="Hi, my name is Andrew.")
conversation.predict(input="What is 1+1?")
response = conversation.predict(input="What is my name?")
print(response)  # 输出可能：抱歉，我没有访问这些信息。
```

![](img/6611f57afa341027c62f44f8188df02e_117.png)

![](img/6611f57afa341027c62f44f8188df02e_118.png)

因为 `k=1`，它只记得最近一次交流（“1+1=2”），而忘记了更早的交流（“我的名字是Andrew”）。这是一个很好的功能，因为它让你可以跟踪最近的几次对话，防止内存无限制增长。在实践中，你可能会将 `k` 设置为一个较大的数字。

![](img/6611f57afa341027c62f44f8188df02e_120.png)

![](img/6611f57afa341027c62f44f8188df02e_122.png)

![](img/6611f57afa341027c62f44f8188df02e_124.png)

### 对话令牌缓冲记忆

![](img/6611f57afa341027c62f44f8188df02e_126.png)

![](img/6611f57afa341027c62f44f8188df02e_128.png)

这种记忆将限制保存的令牌数，这更直接地映射到语言模型调用的成本。

![](img/6611f57afa341027c62f44f8188df02e_130.png)

![](img/6611f57afa341027c62f44f8188df02e_132.png)

![](img/6611f57afa341027c62f44f8188df02e_134.png)

```python
from langchain.memory import ConversationTokenBufferMemory

![](img/6611f57afa341027c62f44f8188df02e_136.png)

![](img/6611f57afa341027c62f44f8188df02e_138.png)

# 设置最大令牌限制
memory = ConversationTokenBufferMemory(llm=llm, max_token_limit=50)
conversation = ConversationChain(llm=llm, memory=memory, verbose=False)

![](img/6611f57afa341027c62f44f8188df02e_140.png)

![](img/6611f57afa341027c62f44f8188df02e_142.png)

# 进行一段较长的对话
inputs = ["AI is amazing.", "Backpropagation is beautiful.", "Chatbots are cool."]
for inp in inputs:
    _ = conversation.predict(input=inp)

![](img/6611f57afa341027c62f44f8188df02e_144.png)

print(memory.load_memory_variables({}))
```

![](img/6611f57afa341027c62f44f8188df02e_146.png)

如果你减少令牌限制，那么它会切掉对话的早期部分，以保留对应最近交流的令牌数，但受限于不超过令牌限制。需要指定 `llm` 参数是因为不同的语言模型使用不同的计数令牌方式。

![](img/6611f57afa341027c62f44f8188df02e_148.png)

![](img/6611f57afa341027c62f44f8188df02e_150.png)

![](img/6611f57afa341027c62f44f8188df02e_152.png)

### 对话摘要缓冲记忆

![](img/6611f57afa341027c62f44f8188df02e_154.png)

![](img/6611f57afa341027c62f44f8188df02e_156.png)

这种记忆的想法不是限制记忆到固定数量的令牌或基于最近的陈述，而是使用语言模型为对话生成摘要，让摘要成为记忆。

![](img/6611f57afa341027c62f44f8188df02e_158.png)

![](img/6611f57afa341027c62f44f8188df02e_160.png)

```python
from langchain.memory import ConversationSummaryBufferMemory

![](img/6611f57afa341027c62f44f8188df02e_162.png)

![](img/6611f57afa341027c62f44f8188df02e_164.png)

![](img/6611f57afa341027c62f44f8188df02e_166.png)

# 创建摘要记忆
memory = ConversationSummaryBufferMemory(llm=llm, max_token_limit=100)
conversation = ConversationChain(llm=llm, memory=memory, verbose=True)

![](img/6611f57afa341027c62f44f8188df02e_168.png)

![](img/6611f57afa341027c62f44f8188df02e_170.png)

# 输入一段长文本（例如日程）
schedule = "早上9点与产品团队会议，需要准备PPT。中午12点在意大利餐厅与客户共进午餐，记得带上笔记本电脑展示最新的产品演示。下午3点进行代码审查。"
conversation.predict(input=f"Today's schedule is: {schedule}")

![](img/6611f57afa341027c62f44f8188df02e_172.png)

# 询问关于日程的问题
response = conversation.predict(input="What should I present to the client?")
print(response)
```

它试图做的是保持消息的显式存储，直到达到我们指定的标记数为止。任何超出部分它将使用语言模型生成摘要来保存。

## 其他记忆类型与总结

![](img/6611f57afa341027c62f44f8188df02e_174.png)

尽管我们已用聊天示例说明了这些不同记忆，但这些记忆对其他应用也有用。例如，若系统反复在线搜索事实，但你想保持总记忆量，不让列表任意增长。

![](img/6611f57afa341027c62f44f8188df02e_176.png)

![](img/6611f57afa341027c62f44f8188df02e_178.png)

实际上，LangChain还支持其他类型的内存。

![](img/6611f57afa341027c62f44f8188df02e_180.png)

![](img/6611f57afa341027c62f44f8188df02e_182.png)

*   **向量数据库记忆**：如果你熟悉词嵌入和文本嵌入，向量数据库可以存储这样的嵌入，并检索最相关的文本块作为记忆。这是一种强大的记忆方式。
*   **实体记忆**：适用于你想记住特定人或特定实体的细节时。比如谈论特定朋友，可以让LangChain记住关于那个朋友的事实。

![](img/6611f57afa341027c62f44f8188df02e_184.png)

![](img/6611f57afa341027c62f44f8188df02e_186.png)

在用LangChain实现应用时，也可以组合使用多种类型记忆。例如，使用一种对话记忆来记住对话流程，同时使用实体记忆来回忆对话中重要人物的具体信息。

![](img/6611f57afa341027c62f44f8188df02e_188.png)

![](img/6611f57afa341027c62f44f8188df02e_190.png)

![](img/6611f57afa341027c62f44f8188df02e_192.png)

当然，除了使用这些记忆类型，开发人员将整个对话存储在传统数据库（如键值存储或SQL数据库）中也很常见。

![](img/6611f57afa341027c62f44f8188df02e_194.png)

![](img/6611f57afa341027c62f44f8188df02e_196.png)

## 总结

![](img/6611f57afa341027c62f44f8188df02e_197.png)

![](img/6611f57afa341027c62f44f8188df02e_198.png)

![](img/6611f57afa341027c62f44f8188df02e_199.png)

本节课中我们一起学习了LangChain中的记忆功能。我们了解到大型语言模型本身是无状态的，需要外部机制来管理对话历史。我们介绍了**对话缓冲内存**、**对话缓冲窗口内存**、**对话令牌缓冲内存**和**对话摘要缓冲内存**这几种核心记忆类型，并了解了它们各自的适用场景和配置方法。最后，我们还简要提到了更高级的**向量数据库记忆**和**实体记忆**。合理利用记忆功能，是构建流畅、智能对话应用的关键。