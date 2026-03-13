# 007：扩展任务与温度参数 🧠

![](img/438e48cb73e816738efc4d8b3de2ff84_0.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_1.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_3.png)

在本节课中，我们将要学习如何使用大型语言模型进行“扩展”任务，即将短文变长，例如根据一组指令或话题列表生成更长的文本。我们还将介绍一个重要的模型参数——**温度**，它控制着模型输出的多样性和随机性。

![](img/438e48cb73e816738efc4d8b3de2ff84_5.png)

---

## 什么是扩展任务？ 📝

![](img/438e48cb73e816738efc4d8b3de2ff84_7.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_9.png)

扩展任务是指让大型语言模型根据简短的输入，生成更长的文本。例如，模型可以根据几条指令或一个话题列表，撰写一封完整的电子邮件或一篇关于某个主题的短文。

![](img/438e48cb73e816738efc4d8b3de2ff84_11.png)

这种能力有很好的用途，例如将大型语言模型作为头脑风暴的伙伴。但需要注意的是，它也可能被滥用，例如用于生成大量垃圾邮件。因此，在使用这些功能时，请务必负责任。

![](img/438e48cb73e816738efc4d8b3de2ff84_13.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_15.png)

---

![](img/438e48cb73e816738efc4d8b3de2ff84_17.png)

## 个性化邮件生成示例 ✉️

![](img/438e48cb73e816738efc4d8b3de2ff84_19.png)

上一节我们介绍了扩展任务的基本概念，本节中我们来看看一个具体的应用示例：生成个性化邮件。

![](img/438e48cb73e816738efc4d8b3de2ff84_21.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_23.png)

假设我们需要以AI客服助手的身份，给一位VIP客户发送邮件回复。邮件需要基于客户评论及其情感倾向来定制。

![](img/438e48cb73e816738efc4d8b3de2ff84_25.png)

以下是实现步骤：

![](img/438e48cb73e816738efc4d8b3de2ff84_27.png)

首先，我们需要设置环境并定义辅助函数。

![](img/438e48cb73e816738efc4d8b3de2ff84_29.png)

```python
import openai

def get_completion(prompt, model="gpt-3.5-turbo", temperature=0):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=temperature,
    )
    return response.choices[0].message["content"]
```

![](img/438e48cb73e816738efc4d8b3de2ff84_31.png)

接下来，我们根据客户评论和已分析出的情感来生成回复。提示词设计如下：

![](img/438e48cb73e816738efc4d8b3de2ff84_33.png)

```
你是一个客户服务AI助手。
你的任务是发送一封电子邮件回复给一位贵宾客户。
给定由三个反引号分隔的客户电子邮件，生成回复。
若情感正面或中性，感谢其评论。
若情感负面，道歉并建议联系客服。
确保使用评论中的具体细节。
以简洁专业语气书写。
签名注明“AI客服助手”。
客户评论：```{review}```
情感：{sentiment}
```

![](img/438e48cb73e816738efc4d8b3de2ff84_35.png)

例如，针对一条关于搅拌机的负面评论，模型会生成一封包含道歉并建议联系客服的邮件。在生成给用户看的文本时，注明由AI生成非常重要，这保持了透明度。

![](img/438e48cb73e816738efc4d8b3de2ff84_37.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_39.png)

---

![](img/438e48cb73e816738efc4d8b3de2ff84_41.png)

## 理解温度参数 🌡️

![](img/438e48cb73e816738efc4d8b3de2ff84_43.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_45.png)

在之前的示例中，我们使用了默认的温度值。现在，我们来深入了解温度这个参数。

**温度** 是一个控制模型输出随机性的参数。你可以将其理解为模型的“探索程度”或“创造性”。

*   当 **温度=0** 时，模型总是选择概率最高的下一个词，输出稳定、可预测。
*   当 **温度>0** 时，模型会考虑概率较低的词，输出更具多样性、更随机，但也可能更富有创意。

公式上，温度调整了模型输出概率的分布。原始概率 $P(w_i)$ 经过温度 $T$ 调整后变为：
$$ P'(w_i) = \frac{\exp(\log(P(w_i)) / T)}{\sum_j \exp(\log(P(w_j)) / T)} $$
当 $T$ 较高时，概率分布更平缓；当 $T$ 较低（接近0）时，概率分布更尖锐。

![](img/438e48cb73e816738efc4d8b3de2ff84_47.png)

---

![](img/438e48cb73e816738efc4d8b3de2ff84_49.png)

## 实践不同温度下的输出 🔬

![](img/438e48cb73e816738efc4d8b3de2ff84_51.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_53.png)

以下是使用不同温度值运行相同提示词的对比。

![](img/438e48cb73e816738efc4d8b3de2ff84_55.png)

对于需要可靠、可预测输出的应用（如客服邮件），建议使用 **温度=0**。

![](img/438e48cb73e816738efc4d8b3de2ff84_57.png)

```python
# 温度 = 0，输出稳定
response_0 = get_completion(prompt, temperature=0)
```

![](img/438e48cb73e816738efc4d8b3de2ff84_59.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_61.png)

若要激发模型的创造性，可以尝试更高的温度，例如 **温度=0.7**。

![](img/438e48cb73e816738efc4d8b3de2ff84_63.png)

```python
# 温度 = 0.7，输出更多样
response_07_1 = get_completion(prompt, temperature=0.7)
response_07_2 = get_completion(prompt, temperature=0.7) # 每次输出可能不同
```

![](img/438e48cb73e816738efc4d8b3de2ff84_65.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_67.png)

在高温下，模型的输出更随机，可以认为它更容易“分心”，但也可能产生更有趣或更出人意料的文本。建议你亲自尝试不同的温度值，观察输出文本的变化。

![](img/438e48cb73e816738efc4d8b3de2ff84_69.png)

---

![](img/438e48cb73e816738efc4d8b3de2ff84_71.png)

![](img/438e48cb73e816738efc4d8b3de2ff84_73.png)

## 总结 📚

![](img/438e48cb73e816738efc4d8b3de2ff84_75.png)

本节课中我们一起学习了：
1.  **扩展任务**：利用大型语言模型将简短输入扩展为更长、更丰富的文本。
2.  **个性化邮件生成**：通过设计提示词，让模型基于特定信息（如客户评论和情感）生成定制化内容。
3.  **温度参数**：这是一个关键参数，用于控制模型输出的随机性与多样性。**温度=0** 确保稳定性，**温度>0** 增加创造性和变化。

![](img/438e48cb73e816738efc4d8b3de2ff84_77.png)

在下一节课中，我们将深入探讨聊天完成端点的更多格式和用法。