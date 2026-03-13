# 002：模型、提示与输出解析

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_0.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_2.png)

在本节课中，我们将学习LangChain的核心概念：模型、提示和输出解析。我们将了解如何使用LangChain的抽象来更高效地调用语言模型、构建可重用的提示模板，并将模型输出解析为结构化的格式，以便于下游应用使用。

---

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_4.png)

## 🚀 开始之前：环境设置

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_6.png)

要开始使用，我们需要导入必要的库并设置环境。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_8.png)

```python
import os
import openai

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_10.png)

# 加载你的OpenAI API密钥
openai.api_key = os.getenv("OPENAI_API_KEY")
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_12.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_14.png)

如果你在本地运行且尚未安装`openai`库，你需要先运行以下命令：

```bash
pip install openai
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_16.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_18.png)

---

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_20.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_22.png)

## 🤖 直接调用模型

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_24.png)

首先，我们回顾一下直接调用OpenAI API的方式。以下是一个辅助函数，用于调用GPT-3.5 Turbo模型。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_26.png)

```python
def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0,
    )
    return response.choices[0].message["content"]

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_28.png)

# 示例调用
prompt = "1加1是什么？"
response = get_completion(prompt)
print(response)
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_30.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_32.png)

运行上述代码，模型会返回答案“2”。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_34.png)

---

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_36.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_38.png)

## 📝 构建应用：翻译客户邮件

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_40.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_42.png)

假设我们收到一封非英语（例如“海盗英语”）的客户投诉邮件，我们需要将其翻译成礼貌、尊重的美式英语，以便客服人员处理。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_44.png)

客户邮件内容如下：
> “Arrr, I be fuming that me blender lid flew off and splattered me kitchen walls with smoothie! And to make matters worse, the warranty don't cover the cost of cleaning up me kitchen! I need yer help right now, matey!”

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_46.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_48.png)

我们的目标是将其翻译为：“美式英语，语气冷静且尊重”。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_50.png)

我们可以构建一个提示来完成这个任务：

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_52.png)

```python
style = "美式英语，语气冷静且尊重"
text = "Arrr, I be fuming that me blender lid flew off and splattered me kitchen walls with smoothie! And to make matters worse, the warranty don't cover the cost of cleaning up me kitchen! I need yer help right now, matey!"

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_54.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_56.png)

prompt = f"""将以下文本翻译成 {style}：
{text}
"""

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_58.png)

response = get_completion(prompt)
print(response)
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_60.png)

模型返回的翻译结果类似于：
> “我非常沮丧，我的搅拌机盖子飞了出去，把厨房墙壁弄得一团糟！更糟糕的是，保修不包括清理厨房的费用！我现在真的需要你的帮助，朋友。”

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_62.png)

这种方法有效，但如果我们有大量不同语言的邮件需要处理，手动构建每个提示会很繁琐。接下来，我们将看到如何使用LangChain来优化这个过程。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_64.png)

---

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_66.png)

## 🔄 使用LangChain：模型与提示模板

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_68.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_69.png)

LangChain提供了`ChatOpenAI`类来抽象化对ChatGPT API的调用，以及`ChatPromptTemplate`类来创建可重用的提示模板。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_71.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_73.png)

首先，我们导入LangChain的相关模块并设置模型。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_75.png)

```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_77.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_78.png)

# 创建ChatOpenAI实例，设置temperature=0使输出更确定
chat = ChatOpenAI(temperature=0)
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_80.png)

接着，我们定义一个提示模板。这个模板包含两个变量：`style`（目标风格）和`text`（待翻译文本）。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_82.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_84.png)

```python
template_string = """将以下文本翻译成 {style}：
{text}
"""

# 创建提示模板
prompt_template = ChatPromptTemplate.from_template(template_string)
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_86.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_88.png)

现在，我们可以使用这个模板为不同的场景生成具体的提示。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_90.png)

```python
# 场景一：将客户邮件翻译成美式英语
customer_style = "美式英语，语气冷静且尊重"
customer_email = "Arrr, I be fuming that me blender lid flew off and splattered me kitchen walls with smoothie! And to make matters worse, the warranty don't cover the cost of cleaning up me kitchen! I need yer help right now, matey!"

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_92.png)

# 生成消息列表（LangChain的标准格式）
customer_messages = prompt_template.format_messages(
    style=customer_style,
    text=customer_email
)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_94.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_96.png)

# 调用模型
customer_response = chat(customer_messages)
print(customer_response.content)
```

**提示模板的优势在于可重用性**。例如，当客服用英语回复后，我们可以使用同一个模板将回复翻译成“海盗英语”风格返回给客户。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_98.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_100.png)

```python
# 场景二：将客服回复翻译成海盗英语
service_reply = """您好，根据保修条款，因您使用叉子戳搅拌机盖导致损坏，清理费用不在保修范围内。很抱歉带来不便。"""
service_style_pirate = "英语海盗风格，语气礼貌"

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_102.png)

service_messages = prompt_template.format_messages(
    style=service_style_pirate,
    text=service_reply
)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_104.png)

service_response = chat(service_messages)
print(service_response.content)
```

使用提示模板，我们无需重复编写复杂的提示字符串，只需更改变量即可。这对于构建复杂、提示较长的应用程序尤其有用。LangChain还内置了许多常见任务（如摘要、问答、连接数据库）的提示模板，可以进一步加速开发。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_106.png)

---

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_108.png)

## 🧩 输出解析：从文本到结构化数据

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_110.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_112.png)

很多时候，我们希望语言模型的输出是结构化的（例如JSON），而不是纯文本，以便程序能够直接处理。这就是输出解析器的用武之地。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_114.png)

假设我们有一个产品评论，我们希望从中提取特定信息（例如：是否为礼物、送达天数、价格），并以JSON格式输出。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_116.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_118.png)

原始评论如下：
> “这款睡眠风扇效果惊人，有4档风力设置，从轻柔的微风到龙卷风般强劲。两天后就送到了，正好赶上我给妻子的周年纪念礼物。我想我妻子很喜欢它，因为她至今没说话...不过我一直是唯一在使用它的人...”

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_120.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_122.png)

我们期望的输出格式是一个Python字典：
```json
{
  "gift": true,
  "delivery_days": 2,
  "price_value": "相当实惠"
}
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_124.png)

### 不使用解析器的尝试

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_126.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_128.png)

我们可以先尝试用提示让模型直接输出JSON字符串。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_130.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_132.png)

```python
review_template = """\
针对由三个反引号分隔的文本，请提取以下信息：
是否为礼物：如果购买是为了送给别人，则为真；如果是购买者自用，则为假；如果无法确定，则为未知。
送达天数：产品下单后多少天送达。如果未提及送达信息，输出-1。
价格价值：提取评论中关于产品价值的任何描述，例如“物超所值”、“价格合理”、“太贵了”等。

文本: ```{text}```
"""

prompt_template = ChatPromptTemplate.from_template(review_template)
messages = prompt_template.format_messages(text=product_review)
response = chat(messages)
print(response.content)
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_134.png)

模型可能会输出类似这样的字符串：
```
是否为礼物：真
送达天数：2
价格价值：未明确提及
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_136.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_138.png)

虽然内容正确，但`response.content`的类型是字符串，不是Python字典。我们无法直接通过键（如`response[“gift”]`）来访问值。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_140.png)

### 使用LangChain的输出解析器

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_142.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_144.png)

LangChain的`StructuredOutputParser`可以帮我们解决这个问题。它允许我们定义期望的输出模式（schema），并自动将模型输出解析为结构化的Python对象。

首先，我们定义输出模式。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_146.png)

```python
from langchain.output_parsers import ResponseSchema, StructuredOutputParser

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_148.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_150.png)

# 定义每个字段的模式
gift_schema = ResponseSchema(
    name="gift",
    description="购买的商品是送给别人的礼物吗？如果是，则为真；如果是自用，则为假；如果不确定，则为未知。"
)
delivery_days_schema = ResponseSchema(
    name="delivery_days",
    description="产品下单后多少天送达？如果未提及，则为-1。"
)
price_value_schema = ResponseSchema(
    name="price_value",
    description="评论中关于产品价值的描述，例如'物超所值'、'价格合理'、'太贵了'等。"
)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_152.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_154.png)

# 将模式放入列表
response_schemas = [gift_schema, delivery_days_schema, price_value_schema]
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_156.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_157.png)

然后，我们创建输出解析器，并将其与提示模板结合。解析器会自动生成格式指令，并添加到提示中。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_159.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_161.png)

```python
# 创建输出解析器
output_parser = StructuredOutputParser.from_response_schemas(response_schemas)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_163.png)

# 获取解析器提供的格式指令
format_instructions = output_parser.get_format_instructions()

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_165.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_167.png)

# 创建包含格式指令的新提示模板
review_template_with_format = """\
针对由三个反引号分隔的文本，请提取以下信息：
{format_instructions}

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_169.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_171.png)

文本: ```{text}```
"""

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_173.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_175.png)

# 注意：提示中包含了{format_instructions}占位符
prompt = ChatPromptTemplate.from_template(template=review_template_with_format)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_177.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_179.png)

# 生成最终消息
messages = prompt.format_messages(
    text=product_review,
    format_instructions=format_instructions
)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_181.png)

# 调用模型
response = chat(messages)
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_183.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_185.png)

最后，我们使用解析器来解析模型的输出字符串。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_186.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_188.png)

```python
# 解析输出
output_dict = output_parser.parse(response.content)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_190.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_192.png)

# 现在output_dict是一个真正的Python字典
print(type(output_dict))  # <class 'dict'>
print(output_dict)
# 输出可能为：{'gift': True, 'delivery_days': 2, 'price_value': '未明确提及'}

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_194.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_196.png)

# 我们可以像操作普通字典一样访问数据
print(f"是否为礼物: {output_dict['gift']}")
print(f"送达天数: {output_dict['delivery_days']}")
print(f"价格评价: {output_dict['price_value']}")
```

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_198.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_199.png)

通过结合提示模板和输出解析器，我们构建了一个强大的流程：用定义好的模式提示模型，并自动将返回的文本解析为结构化的数据，极大地方便了后续的程序化处理。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_201.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_203.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_205.png)

---

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_207.png)

## 📚 本节总结

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_209.png)

在本节课中，我们一起学习了LangChain的三个核心组件：

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_211.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_213.png)

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_215.png)

1.  **模型**：使用`ChatOpenAI`等类来抽象化对大语言模型的调用，简化参数设置。
2.  **提示**：使用`ChatPromptTemplate`创建可重用的提示模板，提高代码的复用性和可维护性，并能方便地使用LangChain内置的各类提示。
3.  **输出解析**：使用`StructuredOutputParser`定义输出模式，将语言模型返回的非结构化文本自动解析为结构化的Python对象（如字典），便于下游应用程序直接使用。

![](img/f5b781dc791e91ed41bc97e6c7e7e36f_217.png)

通过将这些组件组合使用，你可以更高效、更可靠地构建基于大语言模型的应用程序。在下一节课中，我们将探索如何使用LangChain构建更复杂的聊天机器人或让模型进行更有效的多轮对话。