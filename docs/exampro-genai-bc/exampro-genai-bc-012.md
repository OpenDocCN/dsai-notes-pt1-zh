# 12：使用Perplexity构建英语到西班牙语句子翻译助手 🎯

在本节课中，我们将学习如何利用Perplexity AI平台，构建一个能够辅助英语到西班牙语翻译学习的智能助手。我们将通过一个精心设计的提示词（Prompt），让AI扮演西班牙语教师的角色，引导用户逐步完成句子翻译练习。

![](img/f82563bd7e9e4bd0222fca6df5c2851b_1.png)

---

![](img/f82563bd7e9e4bd0222fca6df5c2851b_3.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_5.png)

## 概述

![](img/f82563bd7e9e4bd0222fca6df5c2851b_7.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_9.png)

本节课的核心目标是创建一个交互式语言学习工具。我们将使用Perplexity AI，通过一个结构化的提示词，使其化身为一位严格的西班牙语教师。这位“教师”不会直接给出答案，而是通过提供词汇表、句子结构线索和反馈，引导学习者自己完成从英语到西班牙语的翻译。

## 构建AI教师提示词

![](img/f82563bd7e9e4bd0222fca6df5c2851b_11.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_13.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_14.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_16.png)

上一节我们明确了目标，本节中我们来看看如何构建这个核心提示词。提示词定义了AI的角色、行为规则和交互流程。

**角色与指令设定：**
我们首先需要为AI设定明确的角色和教学目标。

```
角色：你是一位西班牙语教师。
语言水平：B1（中级）。
指令：学生将提供一个英语句子。你需要帮助学生将其转录成西班牙语。不要直接给出转录结果，让学生通过线索自己完成。如果学生直接索要答案，告诉他们你不能提供，但可以提供线索。
```

![](img/f82563bd7e9e4bd0222fca6df5c2851b_18.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_20.png)

**输出格式要求：**
为了有效辅助学习，我们需要AI以特定的格式提供信息。

以下是AI需要提供的具体内容：
*   **词汇表**：提供句子中关键单词的字典形式（原形）。
*   **句子结构**：分析并展示可能的西班牙语句子结构。
*   **线索与考量**：给出翻译时需要思考的语法点或注意事项。
*   **反馈与推进**：当学生尝试翻译后，用英语解释他们实际表达了什么（进行解读），而不是直接评判对错。不要过于鼓励或提供额外提示。一旦学生成功翻译句子，立即提供一个新句子让他们继续练习。

这个提示词的设计灵感来源于Andrew Brown训练营中用于日语的句子构建器，但根据西班牙语的特点（如没有独立的书面和口语体系）进行了简化。

## 在Perplexity中配置与测试

现在我们已经准备好了提示词，接下来将其投入实践，在Perplexity平台中进行配置和初步测试。

首先，在Perplexity中，我们将焦点模式（Focus）设置为“写作”（Writing）。这可以确保AI专注于与我们对话，而不会去联网搜索信息，从而获得更可控、更符合教学目标的交互体验。

接着，我们将完整的提示词复制到Perplexity的输入框中。AI接收指令后，会确认它已理解并准备好以B1水平协助进行英译西练习，然后提示我们输入第一个待翻译的英语句子。

![](img/f82563bd7e9e4bd0222fca6df5c2851b_22.png)

我们输入第一个句子：**“Let‘s go to the store tomorrow.”**（我们明天去商店吧。）

![](img/f82563bd7e9e4bd0222fca6df5c2851b_24.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_25.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_27.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_29.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_31.png)

AI根据提示词作出了如下响应：
*   **词汇表**：`ir` (to go), `tienda` (store), `mañana` (tomorrow)
*   **句子结构**：`[Let‘s form of ir] + [a la] + [tienda] + [mañana].`
*   **线索与考量**：注意西班牙语中表示“Let‘s”的特殊形式（通常使用第一人称复数虚拟式，如 `vamos`）；考虑指示目的地时与动词 `ir` 连用的正确介词。

![](img/f82563bd7e9e4bd0222fca6df5c2851b_32.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_34.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_35.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_37.png)

然后，我尝试翻译：`Nosotros iremos a la tienda mañana.`
AI反馈道：“我来解读你写的内容：‘We will go to the store tomorrow.‘（我们明天将去商店。）这很接近，但不是我们想要的目标句。我们需要的是‘Let‘s go‘的形式，这与‘we will go‘的形式不同。”

![](img/f82563bd7e9e4bd0222fca6df5c2851b_39.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_40.png)

经过几轮询问线索和尝试修正（例如，学习到“Let‘s go”可以使用 `vayamos` 或 `vamos`），最终以 `Vamos a la tienda mañana.` 通过了验证。成功后，AI按照指令自动给出了下一个句子：**“The cat sleeps on the couch every day.”**

## 探索不同AI模型的差异

在同一个提示词下，不同的AI模型可能会展现出不同的推理风格和响应方式。本节中，我们来看看切换模型会带来什么变化。

我们将Perplexity的模型从默认的“专业搜索”切换到 **DeepSeek-R1** 模型。一个有趣的现象出现了：DeepSeek模型会进行“链式思考”（Chain-of-Thought）。它在生成最终答案前，会将其推理过程以文字形式展示出来，例如：“用户提供了这个详细的查询……主要任务是……我需要彻底解析指令……示例格式显示每次交互后我应该提供一个新句子……” 这让使用者能够窥见AI是如何一步步处理复杂指令的，非常有助于理解和调试提示词。

接下来，我们切换到 **OpenAI o3-mini** 模型。它的响应速度非常快，并且没有展示中间的思考过程。它直接理解了指令，并开始请求第一个英语句子。我们输入 **“I like my dog.”**，它同样提供了词汇表、句子结构，并在我们正确翻译为 `Me gusta mi perro.` 后给出了肯定反馈和下一个句子。

![](img/f82563bd7e9e4bd0222fca6df5c2851b_42.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_43.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_45.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_47.png)

最后，我们尝试调整提示词中的语言水平，将“B1”改为“C1”（高级），并让AI自己生成符合该水平的句子。AI生成了一个更复杂的句子：**“The new environmental policies could have a significant impact on biodiversity.”**（新的环境政策可能对生物多样性产生重大影响。）这证实了我们的提示词具有良好的可扩展性，能够适应不同学习阶段的需求。

![](img/f82563bd7e9e4bd0222fca6df5c2851b_49.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_51.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_53.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_55.png)

## 总结与延伸

![](img/f82563bd7e9e4bd0222fca6df5c2851b_57.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_58.png)

本节课中，我们一起学习了如何利用一个结构化的提示词，在Perplexity AI上构建一个个性化的西班牙语学习助手。我们看到了如何通过定义角色、设定规则和输出格式，来引导AI的行为，使其成为一位有耐心、不直接给答案的“老师”。

![](img/f82563bd7e9e4bd0222fca6df5c2851b_60.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_61.png)

我们探索了不同AI模型（如DeepSeek-R1和OpenAI o3-mini）对同一提示词的反应差异，这有助于我们根据需求选择最合适的模型。同时，我们也验证了通过简单修改提示词（如调整语言等级），可以轻松适配不同难度的学习内容。

![](img/f82563bd7e9e4bd0222fca6df5c2851b_63.png)

![](img/f82563bd7e9e4bd0222fca6df5c2851b_64.png)

这个练习的核心在于提示词工程。你可以借鉴这个方法，尝试将其改编用于学习任何其他语言。Andrew Brown的GitHub仓库中提供了更多语言的提示词示例，其他客座讲师也会分享不同语言的实践，这些都是宝贵的学习资源。