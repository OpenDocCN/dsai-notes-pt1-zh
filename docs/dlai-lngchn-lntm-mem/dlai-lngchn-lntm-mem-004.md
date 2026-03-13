# 004：为邮件助手添加语义记忆 📧🧠

![](img/d107fa768e67d52e0c22404bae03a686_0.png)

![](img/d107fa768e67d52e0c22404bae03a686_2.png)

![](img/d107fa768e67d52e0c22404bae03a686_3.png)

![](img/d107fa768e67d52e0c22404bae03a686_5.png)

## 概述
在本节课中，我们将基于之前创建的邮件助手智能体，为其添加工具，使其能够操作语义记忆。语义记忆通常用于存储关于用户的事实信息。我们将学习如何创建工具来学习用户事实，将其存储在长期记忆库中，并添加一个独立的工具来搜索这些事实。

![](img/d107fa768e67d52e0c22404bae03a686_7.png)

![](img/d107fa768e67d52e0c22404bae03a686_8.png)

## 设置与回顾
上一节我们介绍了基础的邮件助手智能体，本节中我们来看看如何为其增强记忆能力。

![](img/d107fa768e67d52e0c22404bae03a686_9.png)

![](img/d107fa768e67d52e0c22404bae03a686_10.png)

![](img/d107fa768e67d52e0c22404bae03a686_12.png)

以下是初始设置代码，与上一课相同：
```python
# 加载环境变量
import os
from dotenv import load_dotenv
load_dotenv()

![](img/d107fa768e67d52e0c22404bae03a686_14.png)

# 定义配置文件和提示词
profile = {...}
prompt_instructions = "..."
example_email = "..."
```

![](img/d107fa768e67d52e0c22404bae03a686_16.png)

## 配置长期记忆存储
首先，我们需要正确设置长期记忆存储。在本课中，我们将使用一个内存中的存储。

以下是导入和初始化内存存储的代码：
```python
from langgraph.checkpoint.memory import MemorySaver

![](img/d107fa768e67d52e0c22404bae03a686_18.png)

![](img/d107fa768e67d52e0c22404bae03a686_20.png)

# 初始化存储，传入嵌入模型用于索引记忆
store = MemorySaver()
```

![](img/d107fa768e67d52e0c22404bae03a686_22.png)

![](img/d107fa768e67d52e0c22404bae03a686_24.png)

![](img/d107fa768e67d52e0c22404bae03a686_26.png)

## 创建记忆管理工具
接下来，我们从 `langmem` 导入两个函数来创建记忆管理工具。`langmem` 是构建在 LangGraph 之上的一个用于处理记忆的封装库。

以下是导入和创建工具的代码：
```python
from langmem import create_manage_memory_tool, create_search_memory_tool

![](img/d107fa768e67d52e0c22404bae03a686_28.png)

![](img/d107fa768e67d52e0c22404bae03a686_30.png)

# 定义工具操作的命名空间
namespace = ["email_assistant", "{langgraph_user_id}", "collection"]

![](img/d107fa768e67d52e0c22404bae03a686_32.png)

![](img/d107fa768e67d52e0c22404bae03a686_34.png)

# 创建管理记忆和搜索记忆的工具
manage_memory_tool = create_manage_memory_tool(namespace)
search_memory_tool = create_search_memory_tool(namespace)
```

![](img/d107fa768e67d52e0c22404bae03a686_36.png)

![](img/d107fa768e67d52e0c22404bae03a686_37.png)

![](img/d107fa768e67d52e0c22404bae03a686_39.png)

当我们调用智能体时，需要传入一个 `langgraph_user_id` 作为运行时配置的一部分，它将用于区分不同用户的记忆命名空间。这样，在处理多个用户的邮件时，我们可以轻松地为不同用户维护不同的记忆集合。

![](img/d107fa768e67d52e0c22404bae03a686_41.png)

## 工具功能详解
让我们详细了解一下这些工具的功能。

![](img/d107fa768e67d52e0c22404bae03a686_43.png)

![](img/d107fa768e67d52e0c22404bae03a686_45.png)

`manage_memory_tool` 用于创建、更新或删除持久化记忆。其参数包括：
*   `content`: 记忆的内容。
*   `action`: 执行的操作，可以是 `create`、`update` 或 `delete`。
*   `id`: 记忆的唯一标识符。

![](img/d107fa768e67d52e0c22404bae03a686_47.png)

`search_memory_tool` 用于根据当前上下文搜索长期记忆中的相关信息。其参数包括：
*   `query`: 搜索查询。
*   `limit`: 返回的记忆数量限制。
*   `offset`: 偏移量。
*   `filter`: 可选的过滤条件。

![](img/d107fa768e67d52e0c22404bae03a686_49.png)

![](img/d107fa768e67d52e0c22404bae03a686_51.png)

![](img/d107fa768e67d52e0c22404bae03a686_53.png)

## 构建增强型智能体
现在，我们来定义集成了记忆工具的智能体。我们需要修改智能体的系统提示词，以包含这两个新工具。

![](img/d107fa768e67d52e0c22404bae03a686_55.png)

![](img/d107fa768e67d52e0c22404bae03a686_56.png)

以下是更新后的智能体构建代码：
```python
# 修改系统提示词以包含新工具
agent_system_prompt = """
你是一个邮件助手。除了处理邮件，你现在还可以管理长期记忆。
你可以使用 `manage_memory` 工具来记住关于用户的重要事实。
你可以使用 `search_memory` 工具来回忆之前存储的信息。
{原有的提示词部分}
"""

![](img/d107fa768e67d52e0c22404bae03a686_58.png)

![](img/d107fa768e67d52e0c22404bae03a686_60.png)

![](img/d107fa768e67d52e0c22404bae03a686_61.png)

# 创建提示词函数
def create_prompt(messages):
    # 将系统提示词添加到消息列表前
    ...

![](img/d107fa768e67d52e0c22404bae03a686_63.png)

# 构建智能体
from langchain.agents import create_react_agent

# 工具列表现在包含记忆工具
tools = [write_email_tool, manage_memory_tool, search_memory_tool, ...]
response_agent = create_react_agent(llm, tools, prompt=create_prompt)
# 注意：需要将 store 传递给智能体以确保其可用
```

![](img/d107fa768e67d52e0c22404bae03a686_65.png)

## 测试智能体记忆功能
让我们测试一下智能体如何使用这些新工具。

![](img/d107fa768e67d52e0c22404bae03a686_67.png)

![](img/d107fa768e67d52e0c22404bae03a686_68.png)

首先，我们需要定义一个包含 `langgraph_user_id` 的运行时配置。
```python
config = {"configurable": {"langgraph_user_id": "Lance"}}
```

![](img/d107fa768e67d52e0c22404bae03a686_70.png)

![](img/d107fa768e67d52e0c22404bae03a686_72.png)

现在，我们用一个包含应记住信息（例如“Jim是我的朋友”）的消息来调用智能体。
```python
result = response_agent.invoke({"messages": [("human", "Jim is my friend.")]}, config)
```
智能体应该会调用 `manage_memory_tool` 来创建一条内容为“Jim is John Doe‘s friend”的记忆。

![](img/d107fa768e67d52e0c22404bae03a686_74.png)

接着，我们询问“Who is Jim?”。
```python
result = response_agent.invoke({"messages": [("human", "Who is Jim?")]}, config)
```
智能体应该会先调用 `search_memory_tool` 来搜索关于“Jim”的记忆，找到之前存储的信息，然后基于此信息进行回答。

![](img/d107fa768e67d52e0c22404bae03a686_76.png)

![](img/d107fa768e67d52e0c22404bae03a686_78.png)

![](img/d107fa768e67d52e0c22404bae03a686_80.png)

## 集成到邮件助手工作流
将增强后的响应智能体集成到完整的邮件助手工作流中，步骤与之前课程基本相同，主要变化是使用了新的 `response_agent` 并确保传入了记忆存储。

![](img/d107fa768e67d52e0c22404bae03a686_82.png)

![](img/d107fa768e67d52e0c22404bae03a686_83.png)

以下是关键集成代码：
```python
# 创建状态和路由节点（与之前相同）
...

![](img/d107fa768e67d52e0c22404bae03a686_85.png)

![](img/d107fa768e67d52e0c22404bae03a686_86.png)

![](img/d107fa768e67d52e0c22404bae03a686_88.png)

# 创建邮件助手，传入存储
email_agent = ... # 使用 response_agent
email_agent.compile(store=store)

![](img/d107fa768e67d52e0c22404bae03a686_90.png)

![](img/d107fa768e67d52e0c22404bae03a686_92.png)

# 调用邮件助手时，记得传入配置
example_email_input = {...}
result = email_agent.invoke(example_email_input, config)
```

![](img/d107fa768e67d52e0c22404bae03a686_94.png)

![](img/d107fa768e67d52e0c22404bae03a686_96.png)

![](img/d107fa768e67d52e0c22404bae03a686_98.png)

## 实战演示
让我们通过一个连续的例子看看记忆如何发挥作用。

![](img/d107fa768e67d52e0c22404bae03a686_100.png)

![](img/d107fa768e67d52e0c22404bae03a686_101.png)

![](img/d107fa768e67d52e0c22404bae03a686_102.png)

**第一封邮件**：Alice 询问缺失的 API 端点。
智能体在回复邮件后，会调用 `manage_memory_tool` 创建一条记忆，内容为“需要跟进：Alice Smith 询问了缺失的 API 端点”。

![](img/d107fa768e67d52e0c22404bae03a686_104.png)

![](img/d107fa768e67d52e0c22404bae03a686_105.png)

![](img/d107fa768e67d52e0c22404bae03a686_107.png)

**第二封邮件**：几天后，Alice 再次发邮件问“关于我之前的请求有更新吗？”。
当智能体处理这封新邮件时，它会首先调用 `search_memory_tool`，查询与“Alice Smith 之前的请求”相关的记忆。找到之前存储的信息后，它就能在回复中引用上下文，例如写道：“感谢您跟进关于 API 端点文档的事宜”。

![](img/d107fa768e67d52e0c22404bae03a686_109.png)

## 总结
本节课中我们一起学习了如何为 LangGraph 邮件助手智能体添加语义记忆功能。我们介绍了如何设置长期记忆存储，创建用于管理和搜索记忆的工具，并将这些工具集成到智能体中。通过实际测试，我们看到了智能体如何利用记忆来跨对话记住用户事实并提供连贯的响应。这为构建具备长期记忆和上下文感知能力的自主智能体奠定了基础。