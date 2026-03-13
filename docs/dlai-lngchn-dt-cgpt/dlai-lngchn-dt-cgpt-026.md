# 026：模型、提示与输出解析 🧠

![](img/33b34f27f891ee6bcb7f740a22b7da9c_0.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_2.png)

在本节课中，我们将学习LangChain的三个核心组件：**模型**、**提示**和**输出解析器**。模型指的是支撑应用的语言模型；提示是传递给模型的输入指令；而输出解析器则负责将模型的输出转换为结构化的格式，以便于下游处理。通过LangChain的抽象，我们可以更方便地构建和复用这些组件。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_4.png)

## 1. 基础设置与环境准备 ⚙️

![](img/33b34f27f891ee6bcb7f740a22b7da9c_6.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_8.png)

首先，我们需要设置环境并导入必要的库。以下是初始代码，用于加载OpenAI API密钥并定义一个调用模型的辅助函数。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_10.png)

```python
import os
import openai

![](img/33b34f27f891ee6bcb7f740a22b7da9c_12.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_14.png)

# 加载OpenAI API密钥
openai.api_key = os.getenv("OPENAI_API_KEY")

![](img/33b34f27f891ee6bcb7f740a22b7da9c_16.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_18.png)

# 辅助函数：调用GPT-3.5模型
def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0,
    )
    return response.choices[0].message["content"]
```

![](img/33b34f27f891ee6bcb7f740a22b7da9c_20.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_22.png)

如果你在本地运行此代码，并且尚未安装`openai`库，可能需要先执行`pip install openai`。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_24.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_26.png)

## 2. 直接使用提示模板示例 📝

![](img/33b34f27f891ee6bcb7f740a22b7da9c_28.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_30.png)

上一节我们完成了基础设置，本节中我们来看看一个直接使用提示模板的示例。假设我们收到一封非英语客户的电子邮件，需要将其翻译成美式英语，并以礼貌尊重的语气呈现。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_32.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_34.png)

以下是客户邮件的示例内容（使用海盗英语风格）：
> “Arrr, I be fuming that me blender lid flew off and splattered me kitchen walls with smoothie! And things be gettin' worse: warranty not be coverin' the cost o' cleanin' me kitchen. I needs yer help right now, matey!”

![](img/33b34f27f891ee6bcb7f740a22b7da9c_36.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_38.png)

我们的目标是将此文本翻译成美式英语，并保持语气平静、尊重。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_40.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_42.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_44.png)

```python
customer_email = """
Arrr, I be fuming that me blender lid flew off and splattered me kitchen walls with smoothie! And things be gettin' worse: warranty not be coverin' the cost o' cleanin' me kitchen. I needs yer help right now, matey!
"""

![](img/33b34f27f891ee6bcb7f740a22b7da9c_46.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_48.png)

style = "American English in a calm and respectful tone"

![](img/33b34f27f891ee6bcb7f740a22b7da9c_50.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_52.png)

prompt = f"""Translate the text that is delimited by triple backticks into a style that is {style}.
text: ```{customer_email}```
"""

![](img/33b34f27f891ee6bcb7f740a22b7da9c_54.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_56.png)

response = get_completion(prompt)
print(response)
```

![](img/33b34f27f891ee6bcb7f740a22b7da9c_58.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_60.png)

运行上述代码，模型将返回翻译后的文本，例如：
> “I am really frustrated that my blender lid flew off and made a mess on my kitchen walls with smoothie! And to make matters worse, the warranty does not cover the cost of cleaning my kitchen. I really need your help right now.”

![](img/33b34f27f891ee6bcb7f740a22b7da9c_62.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_64.png)

你可以尝试修改提示中的`style`变量，观察模型输出的变化。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_66.png)

## 3. 使用LangChain的提示模板 🔄

![](img/33b34f27f891ee6bcb7f740a22b7da9c_68.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_69.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_71.png)

直接使用f字符串构建提示虽然简单，但在构建复杂应用时，提示可能非常长且详细。LangChain提供了**提示模板**这一有用的抽象，帮助我们复用和管理提示。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_73.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_75.png)

首先，我们导入LangChain的相关模块并设置模型。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_77.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_78.png)

```python
from langchain.chat_models import ChatOpenAI
from langchain import PromptTemplate

![](img/33b34f27f891ee6bcb7f740a22b7da9c_80.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_82.png)

# 初始化ChatOpenAI模型，设置temperature=0使输出更确定
chat = ChatOpenAI(temperature=0.0)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_84.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_86.png)

# 定义可复用的提示模板字符串
template_string = """Translate the text that is delimited by triple backticks into a style that is {style}.
text: ```{text}```
"""

![](img/33b34f27f891ee6bcb7f740a22b7da9c_88.png)

# 创建提示模板对象
prompt_template = PromptTemplate.from_template(template_string)
```

![](img/33b34f27f891ee6bcb7f740a22b7da9c_90.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_92.png)

现在，我们可以使用这个模板为不同的输入生成提示。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_94.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_96.png)

```python
# 定义翻译风格和客户邮件
customer_style = "American English in a calm and respectful tone"
customer_email = """
Arrr, I be fuming that me blender lid flew off and splattered me kitchen walls with smoothie! And things be gettin' worse: warranty not be coverin' the cost o' cleanin' me kitchen. I needs yer help right now, matey!
"""

![](img/33b34f27f891ee6bcb7f740a22b7da9c_98.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_100.png)

# 使用模板生成具体的提示消息
customer_messages = prompt_template.format_messages(
    style=customer_style,
    text=customer_email
)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_102.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_104.png)

# 将提示传递给模型并获取响应
customer_response = chat(customer_messages)
print(customer_response.content)
```

![](img/33b34f27f891ee6bcb7f740a22b7da9c_106.png)

提示模板的优势在于其可复用性。例如，如果客服代表需要用海盗英语风格回复客户，我们可以轻松地复用同一个模板。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_108.png)

```python
service_reply = """Hey there customer, the warranty does not cover cleaning expenses for your kitchen because it's your fault that you misused the blender by using a fork to remove the lid. Sorry about that!"""

![](img/33b34f27f891ee6bcb7f740a22b7da9c_110.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_112.png)

service_style_pirate = "English Pirate in a calm and respectful tone"

![](img/33b34f27f891ee6bcb7f740a22b7da9c_114.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_116.png)

service_messages = prompt_template.format_messages(
    style=service_style_pirate,
    text=service_reply
)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_118.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_120.png)

service_response = chat(service_messages)
print(service_response.content)
```

![](img/33b34f27f891ee6bcb7f740a22b7da9c_122.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_124.png)

## 4. 使用输出解析器处理结构化输出 🗂️

![](img/33b34f27f891ee6bcb7f740a22b7da9c_126.png)

上一节我们介绍了如何使用提示模板，本节中我们来看看如何利用**输出解析器**将模型的输出解析为结构化的格式（如Python字典），以便于下游程序处理。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_128.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_130.png)

假设我们需要从产品评论中提取特定信息，并以JSON格式输出。以下是期望的输出格式示例：

![](img/33b34f27f891ee6bcb7f740a22b7da9c_132.png)

```python
{
    "gift": False,
    "delivery_days": 5,
    "price_value": "pretty affordable!"
}
```

以及一条客户评论：

```python
review = """This leaf blower is pretty amazing. It has four settings: candle blower, gentle breeze, windy city, and tornado. It arrived in two days, just in time for my wife's anniversary present. I think my wife liked it so far. She has been using it non-stop and says it's better than the old one. The only complaint is that it's a bit loud, but for the price, it's a great deal!"""
```

![](img/33b34f27f891ee6bcb7f740a22b7da9c_134.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_136.png)

首先，我们尝试直接使用提示模板让模型输出JSON。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_138.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_140.png)

```python
template = """For the following text, extract the following information:

![](img/33b34f27f891ee6bcb7f740a22b7da9c_142.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_144.png)

gift: Was the item purchased as a gift for someone else? Answer True if yes, False if not or unknown.

delivery_days: How many days did it take for the product to arrive? If this information is not found, output -1.

![](img/33b34f27f891ee6bcb7f740a22b7da9c_146.png)

price_value: Extract any sentences about the value or price, and output them as a comma separated Python list.

![](img/33b34f27f891ee6bcb7f740a22b7da9c_148.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_150.png)

Format the output as a JSON object with "gift", "delivery_days" and "price_value" as the keys.

![](img/33b34f27f891ee6bcb7f740a22b7da9c_152.png)

text: {text}
"""

![](img/33b34f27f891ee6bcb7f740a22b7da9c_154.png)

prompt_template = PromptTemplate.from_template(template)
messages = prompt_template.format_messages(text=review)
response = chat(messages)
print(response.content)
```

![](img/33b34f27f891ee6bcb7f740a22b7da9c_156.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_157.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_159.png)

模型可能会返回一个看起来像JSON的字符串，但其类型实际上是`str`，而不是Python字典。为了将其转换为字典，我们需要使用LangChain的输出解析器。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_161.png)

以下是使用输出解析器的完整步骤：

![](img/33b34f27f891ee6bcb7f740a22b7da9c_163.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_165.png)

```python
from langchain.output_parsers import ResponseSchema, StructuredOutputParser

![](img/33b34f27f891ee6bcb7f740a22b7da9c_167.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_169.png)

# 1. 定义期望输出的模式（Schema）
gift_schema = ResponseSchema(
    name="gift",
    description="Was the item purchased as a gift for someone else? Answer True if yes, False if not or unknown."
)
delivery_days_schema = ResponseSchema(
    name="delivery_days",
    description="How many days did it take for the product to arrive? If this information is not found, output -1."
)
price_value_schema = ResponseSchema(
    name="price_value",
    description="Extract any sentences about the value or price, and output them as a comma separated Python list."
)
response_schemas = [gift_schema, delivery_days_schema, price_value_schema]

![](img/33b34f27f891ee6bcb7f740a22b7da9c_171.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_173.png)

# 2. 创建输出解析器
output_parser = StructuredOutputParser.from_response_schemas(response_schemas)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_175.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_177.png)

# 3. 获取解析器生成的格式指令，并将其加入到提示模板中
format_instructions = output_parser.get_format_instructions()

![](img/33b34f27f891ee6bcb7f740a22b7da9c_179.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_181.png)

# 4. 更新提示模板，包含格式指令
template_with_format = """For the following text, extract the following information:

![](img/33b34f27f891ee6bcb7f740a22b7da9c_183.png)

{format_instructions}

![](img/33b34f27f891ee6bcb7f740a22b7da9c_185.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_186.png)

text: {text}
"""

![](img/33b34f27f891ee6bcb7f740a22b7da9c_188.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_190.png)

prompt = PromptTemplate(
    template=template_with_format,
    input_variables=["text"],
    partial_variables={"format_instructions": format_instructions}
)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_192.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_194.png)

# 5. 生成提示并调用模型
messages = prompt.format_messages(text=review)
response = chat(messages)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_196.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_198.png)

# 6. 使用解析器将模型输出转换为字典
output_dict = output_parser.parse(response.content)
print(output_dict)
print(type(output_dict))  # 输出应为 <class 'dict'>

![](img/33b34f27f891ee6bcb7f740a22b7da9c_199.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_201.png)

# 7. 现在可以方便地访问字典中的值
print(output_dict.get('gift'))  # 例如：True
print(output_dict.get('delivery_days'))  # 例如：2
print(output_dict.get('price_value'))  # 例如：["it's a great deal!"]
```

![](img/33b34f27f891ee6bcb7f740a22b7da9c_203.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_205.png)

通过这种方式，我们成功地将语言模型的非结构化文本输出，转换为了结构化的Python字典，极大地方便了后续的数据处理。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_207.png)

## 总结 📚

![](img/33b34f27f891ee6bcb7f740a22b7da9c_209.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_211.png)

在本节课中，我们一起学习了LangChain的三个核心概念：**模型**、**提示**和**输出解析器**。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_213.png)

![](img/33b34f27f891ee6bcb7f740a22b7da9c_215.png)

*   **模型**是应用的基础，我们使用`ChatOpenAI`等类来与大型语言模型交互。
*   **提示模板**提供了一种强大的抽象，允许我们设计、复用和共享复杂的提示，使应用构建更加模块化和高效。
*   **输出解析器**能够将模型返回的文本解析成结构化的数据格式（如字典），使得模型的输出可以直接被下游的Python代码使用。

![](img/33b34f27f891ee6bcb7f740a22b7da9c_217.png)

通过结合使用提示模板和输出解析器，我们可以构建出更健壮、更易维护的大型语言模型应用。在接下来的课程中，我们将学习如何利用LangChain构建更复杂的应用，例如聊天机器人和智能代理。