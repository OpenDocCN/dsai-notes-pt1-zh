# 012：构建响应器链 🛠️

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_1.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_3.png)

在本节课中，我们将学习如何构建 Reflexion 代理系统中的第一个核心组件——响应器代理。我们将从定义提示模板开始，并重点学习如何将大语言模型的无结构输出，强制转换为结构化的 JSON 数据，以便后续处理。

## 概述：响应器代理的目标

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_5.png)

上一节我们介绍了 Reflexion 代理系统的整体架构。本节中，我们来看看如何构建其中的响应器代理。响应器代理是“执行者”代理的一个子组件，其核心目标是接收用户指令，生成一个包含博客内容、自我批判和搜索关键词的结构化响应。

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_7.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_8.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_9.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_10.png)

最终，我们希望得到一个格式固定的 Python 字典，其结构如下：
```python
{
    "response": "生成的博客内容",
    "critique": "对内容的批判",
    "search": ["搜索关键词1", "搜索关键词2"]
}
```

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_12.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_13.png)

## 构建基础提示模板

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_15.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_16.png)

首先，我们需要为“执行者”代理创建一个基础的聊天提示模板。这个模板将被响应器代理和后续的修订器代理复用。

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_18.png)

以下是创建该模板的代码：
```python
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder

actor_agent_prompt = ChatPromptTemplate.from_messages([
    ("system", "You are an expert AI researcher. Current time: {current_time}"),
    ("system", """
    1. {first_instruction}
    2. Reflect and critique your answer. Be severe to maximize improvement.
    3. After the reflection, list 1 to 3 search queries separately for researching improvements. Do not include them inside the reflection.
    Answer the user's question above using the required format.
    """),
    MessagesPlaceholder(variable_name="messages"),
    ("system", "Answer the user's question above using the required format.")
])
```
这个模板包含几个关键部分：系统角色定义、动态指令占位符、消息历史记录占位符以及用于强调输出格式的尾部指令。

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_20.png)

## 结构化输出处理系统

大语言模型的原始输出是自由文本，我们需要将其转换为结构化的数据。为此，我们引入一个“LLM 响应处理系统”，它通过一系列步骤确保数据验证和格式一致。

以下是该系统的三个核心组件：
1.  **聊天提示模板**：我们刚刚已经完成。
2.  **带有 Pydantic 模式的函数调用**：用于强制 LLM 按照预定模式输出 JSON。
3.  **Pydantic 解析器**：用于验证 LLM 的输出并创建 Python 对象。

### 步骤一：定义 Pydantic 数据模式

为了定义我们期望的输出结构，我们使用 Pydantic 库创建一个数据模型（Schema）。

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_22.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_23.png)

以下是定义 `AnswerQuestion` 数据模型的代码：
```python
from pydantic import BaseModel, Field
from typing import List

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_25.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_26.png)

class Reflection(BaseModel):
    missing: str = Field(description="Critique of what is missing or could be improved.")
    superfluous: str = Field(description="Critique of what is excessive, redundant, or not needed.")

class AnswerQuestion(BaseModel):
    answer: str = Field(description="A 250-word detailed answer to the question.")
    search_queries: List[str] = Field(description="1 to 3 search queries for researching information to address the critique.")
    reflection: Reflection
```
这个模型精确规定了 `answer`、`search_queries` 和 `reflection` 的字段名称、类型和描述，为 LLM 的输出提供了明确的蓝图。

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_28.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_29.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_31.png)

### 步骤二：组装响应器链

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_32.png)

现在，我们将各个组件组合起来，构建完整的响应器处理链。

以下是构建响应器链的完整代码：
```python
from datetime import datetime
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage
from langchain_core.output_parsers.openai_tools import PydanticToolsParser

# 1. 实例化LLM
llm = ChatOpenAI(model="gpt-4")

# 2. 创建响应器专用的提示模板（填充第一条指令）
responder_prompt_template = actor_agent_prompt.partial(
    current_time=datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
    first_instruction="Provide me a detailed 250-word answer."
)

# 3. 创建Pydantic解析器
pydantic_parser = PydanticToolsParser(tools=[AnswerQuestion])

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_34.png)

# 4. 构建处理链
first_responder_chain = (
    responder_prompt_template
    | llm.bind_tools(tools=[AnswerQuestion], tool_choice="AnswerQuestion")
    | pydantic_parser
)
```
这段代码完成了以下工作：预填充了时间和指令，将提示模板、绑定了特定工具的 LLM 以及输出解析器连接成一个可顺序执行的处理链。

### 步骤三：调用链并获取结果

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_36.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_37.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_39.png)

最后，我们传入用户消息来调用这个处理链，并查看结构化的输出结果。

以下是调用链并打印结果的代码：
```python
# 调用链
result = first_responder_chain.invoke({
    "messages": [HumanMessage(content="Write me a blog post on how small businesses can leverage AI to grow.")]
})

# 打印结果
print(result)
# 输出将是 AnswerQuestion 类的一个实例，我们可以方便地访问其属性：
# result[0].answer  # 获取生成的博客内容
# result[0].search_queries  # 获取搜索关键词列表
# result[0].reflection.missing  # 获取“缺失内容”的批判
```
运行后，我们将得到一个 `AnswerQuestion` 对象，其中包含了结构化的回答、自我批判和搜索建议，完全符合我们预先定义的模式。

## 总结

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_41.png)

![](img/bd656c5a557f7bbc8d2da4be0b7a9140_43.png)

本节课中我们一起学习了构建 Reflexion 代理系统中响应器链的完整过程。我们从创建基础提示模板开始，然后定义了严格的 Pydantic 数据模式来规范输出结构，最后通过绑定工具和解析器，组装了一个能将 LLM 自由文本输出可靠地转换为结构化数据的处理流水线。这个方法不仅适用于当前项目，也是你未来控制任何 LLM 输出格式的强大工具。在下一节，我们将运用相同的原理，来构建负责修订内容的“修订器代理”。