# 007：构建自定义聊天机器人 🤖

![](img/c108a1a802fa35f3869e421ada174fb8_0.png)

在本节课中，我们将学习如何利用大型语言模型构建一个自定义的聊天机器人。我们将深入理解OpenAI聊天补全格式的组成部分，并动手创建一个披萨餐厅的订单机器人。

---

![](img/c108a1a802fa35f3869e421ada174fb8_2.png)

## 概述

大型语言模型的一个令人兴奋之处在于，你可以用它来构建一个自定义的聊天机器人，而无需付出太多努力。ChatGPT的网页界面就是一种与大型语言模型进行对话的方式。但更酷的是，你也可以使用大型语言模型来构建你自己的聊天机器人，例如扮演AI客服代理或餐厅AI点餐员的角色。在本节中，你将学习如何自己实现这一点。

---

## 聊天模型的工作原理

上一节我们介绍了提示工程的基础概念，本节中我们来看看如何利用聊天格式进行多轮对话。

![](img/c108a1a802fa35f3869e421ada174fb8_4.png)

像ChatGPT这样的聊天模型，其训练目标是接收一系列消息作为输入，并返回一个模型生成的消息作为输出。虽然聊天格式是为多轮对话设计的，但我们在之前的视频中也看到，它同样适用于没有任何对话的单轮任务。

以下是OpenAI聊天补全API中消息的基本结构：

![](img/c108a1a802fa35f3869e421ada174fb8_6.png)

```python
messages = [
    {"role": "system", "content": "系统指令..."},
    {"role": "user", "content": "用户消息1..."},
    {"role": "assistant", "content": "助手回复1..."},
    {"role": "user", "content": "用户消息2..."}
]
```

---

## 消息角色详解

接下来，我们来详细了解一下消息中不同角色的作用。

以下是消息中可能出现的角色及其含义：

![](img/c108a1a802fa35f3869e421ada174fb8_8.png)

*   **系统（system）**：用于设定助手的行为和角色，为整个对话提供高级指令。可以将其理解为在助手耳边低语，引导其回应，而用户通常不会察觉到系统消息的存在。
*   **用户（user）**：代表与聊天机器人交互的用户所发送的消息。
*   **助手（assistant）**：代表聊天机器人（模型）所生成的消息。

![](img/c108a1a802fa35f3869e421ada174fb8_10.png)

![](img/c108a1a802fa35f3869e421ada174fb8_12.png)

---

## 实践：莎士比亚风格的聊天机器人

![](img/c108a1a802fa35f3869e421ada174fb8_14.png)

现在，让我们通过一个例子来实践如何使用这些消息进行对话。

![](img/c108a1a802fa35f3869e421ada174fb8_16.png)

![](img/c108a1a802fa35f3869e421ada174fb8_17.png)

我们将使用一个新的辅助函数，它接收一个消息列表，而不仅仅是单个提示。系统消息指示助手“像莎士比亚一样说话”。

```python
messages = [
    {"role": "system", "content": "你是一个说话像莎士比亚的助手。"},
    {"role": "user", "content": "给我讲个笑话。"},
    {"role": "assistant", "content": "为什么鸡要过马路？"},
    {"role": "user", "content": "我不知道。"}
]
response = get_completion_from_messages(messages, temperature=1)
print(response)
```

![](img/c108a1a802fa35f3869e421ada174fb8_19.png)

![](img/c108a1a802fa35f3869e421ada174fb8_21.png)

运行后，助手会以莎士比亚的风格回答：“为了到达另一边，亲爱的夫人，这是个永恒的经典，从未失手。”

![](img/c108a1a802fa35f3869e421ada174fb8_23.png)

![](img/c108a1a802fa35f3869e421ada174fb8_24.png)

---

## 上下文的重要性

![](img/c108a1a802fa35f3869e421ada174fb8_26.png)

![](img/c108a1a802fa35f3869e421ada174fb8_27.png)

每个与语言模型的对话都是独立的交互。这意味着，为了让模型能够参考或“记住”对话的早期部分，你必须将早期的交流内容作为输入提供给模型。我们将其称为“上下文”。

例如，如果我们直接问模型“我的名字是什么？”，它无法知道。但如果我们提供了包含用户自我介绍的上文，模型就能正确回答。

![](img/c108a1a802fa35f3869e421ada174fb8_29.png)

```python
# 没有上下文
messages = [{"role": "user", "content": "我的名字是什么？"}]
# 模型无法回答

![](img/c108a1a802fa35f3869e421ada174fb8_31.png)

# 有上下文
messages = [
    {"role": "system", "content": "你是一个友好的聊天机器人。"},
    {"role": "user", "content": "你好，我的名字是伊萨。"},
    {"role": "assistant", "content": "你好伊萨，很高兴见到你！"},
    {"role": "user", "content": "我的名字是什么？"}
]
# 模型可以回答：“你的名字是伊萨。”
```

![](img/c108a1a802fa35f3869e421ada174fb8_33.png)

---

![](img/c108a1a802fa35f3869e421ada174fb8_35.png)

![](img/c108a1a802fa35f3869e421ada174fb8_36.png)

## 构建订单机器人：OrderBot 🍕

现在，你将构建自己的聊天机器人。这个机器人将被称为OrderBot，它是一个披萨餐厅的自动点餐机器人。

首先，我们需要定义一个辅助函数来自动收集用户提示和助手回复，从而构建对话上下文。

```python
def collect_messages(_):
    prompt = inp.value_input
    inp.value = ''
    context.append({'role':'user', 'content':f"{prompt}"})
    response = get_completion_from_messages(context)
    context.append({'role':'assistant', 'content':f"{response}"})
    # 更新聊天界面...
```

![](img/c108a1a802fa35f3869e421ada174fb8_38.png)

这个函数将用户输入添加到上下文列表中，调用模型获取回复，再将助手回复也添加到上下文中。这样，上下文会随着对话不断增长，模型始终拥有决定下一步行动所需的信息。

---

![](img/c108a1a802fa35f3869e421ada174fb8_40.png)

## 设置系统指令与菜单

![](img/c108a1a802fa35f3869e421ada174fb8_42.png)

![](img/c108a1a802fa35f3869e421ada174fb8_43.png)

为了让OrderBot正常工作，我们需要在系统消息中给出清晰的指令和菜单信息。

![](img/c108a1a802fa35f3869e421ada174fb8_45.png)

以下是OrderBot的系统消息示例：

![](img/c108a1a802fa35f3869e421ada174fb8_47.png)

```
你是一个OrderBot，一个为披萨餐厅收集订单的自动化服务。
你首先问候顾客，然后收集订单，接着询问是自取还是外卖。
你等待收集整个订单，然后进行总结并最后确认一次顾客是否还要添加其他东西。
如果是外卖，你需要询问地址。最后，你收取付款。
确保澄清所有选项、配料和尺寸，以便从菜单中唯一确定商品。
你以简短、非常口语化、友好的风格回应。
菜单包括：
...
（此处列出披萨、配料、饮料、小吃的详细菜单和价格）
```

![](img/c108a1a802fa35f3869e421ada174fb8_48.png)

![](img/c108a1a802fa35f3869e421ada174fb8_50.png)

---

## 运行与测试OrderBot

设置完成后，我们就可以运行OrderBot并与它对话了。

![](img/c108a1a802fa35f3869e421ada174fb8_52.png)

例如：
*   用户：“你好，我想点个披萨。”
*   OrderBot：“太好了！您想点哪种披萨？我们有意大利香肠披萨、芝士披萨和茄子披萨。”
*   用户：“中号的茄子披萨多少钱？”
*   OrderBot：“中号茄子披萨是15美元。”
*   （对话继续，询问配料、饮料、配送方式等）

![](img/c108a1a802fa35f3869e421ada174fb8_53.png)

![](img/c108a1a802fa35f3869e421ada174fb8_55.png)

你可以尝试暂停视频，在自己的笔记本中运行这段代码，并与OrderBot进行完整的点餐对话。

![](img/c108a1a802fa35f3869e421ada174fb8_57.png)

![](img/c108a1a802fa35f3869e421ada174fb8_58.png)

---

![](img/c108a1a802fa35f3869e421ada174fb8_60.png)

## 生成订单摘要

![](img/c108a1a802fa35f3869e421ada174fb8_62.png)

![](img/c108a1a802fa35f3869e421ada174fb8_63.png)

![](img/c108a1a802fa35f3869e421ada174fb8_65.png)

对话结束后，我们可以指示模型根据对话内容生成一个JSON格式的订单摘要，以便发送给订单系统。

我们通过追加一个系统消息（或用户消息）来实现：

```python
# 在对话上下文后追加指令
context.append({
    'role':'system',
    'content':'创建之前食品订单的JSON摘要。逐项列出每个商品的价格。字段应包括：1. 披萨，2. 配料列表，3. 饮料列表，4. 小吃列表，5. 总价。'
})
response = get_completion_from_messages(context, temperature=0)
print(response)
```

对于此类结构化输出任务，我们通常使用较低的温度值（如`temperature=0`），以确保输出更加可预测和稳定。

生成的JSON摘要可能如下所示：
```json
{
  "pizza": {"name": "medium eggplant pizza", "price": 15},
  "toppings": [],
  "drinks": [{"name": "water", "price": 2}],
  "sides": [{"name": "large fries", "price": 4}],
  "total_price": 21
}
```

---

![](img/c108a1a802fa35f3869e421ada174fb8_67.png)

![](img/c108a1a802fa35f3869e421ada174fb8_68.png)

![](img/c108a1a802fa35f3869e421ada174fb8_69.png)

## 总结

![](img/c108a1a802fa35f3869e421ada174fb8_71.png)

本节课中，我们一起学习了如何利用OpenAI的聊天补全API构建自定义聊天机器人。我们了解了系统消息、用户消息和助手消息的不同角色，掌握了通过维护“上下文”来实现多轮对话的关键。最后，我们动手创建了一个功能完整的披萨餐厅订单机器人OrderBot，并学会了如何从对话中提取结构化的订单信息。

![](img/c108a1a802fa35f3869e421ada174fb8_73.png)

你可以自由定制系统消息，改变聊天机器人的行为，赋予它不同的角色和知识，从而构建出适用于各种场景的对话助手。