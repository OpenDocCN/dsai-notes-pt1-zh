# 028：为聊天机器人添加工具调用能力

在本节课中，我们将学习如何为基于 LangGraph 构建的聊天机器人添加调用外部工具的能力。通过本教程，你将掌握如何让大语言模型（LLM）在需要时，自主决定并调用我们提供的工具来回答问题。

## 概述与准备工作

上一节我们构建了一个基础的聊天机器人。本节中，我们将在其基础上集成工具调用功能。

首先，我们进行必要的导入和初始化。代码结构与上一节类似，但增加了工具相关的部分。

```python
# 导入必要的库
from langgraph.prebuilt import ToolNode
# ... 其他导入（如模型、状态图等）
```

我们使用 `TavilySearch` 作为示例工具，并将其放入一个列表中以便后续绑定。

```python
search_tool = TavilySearchResults(max_results=2)
tools = [search_tool]
```

接下来，我们初始化大语言模型，并使用 `bind_tools` 方法将工具列表绑定到模型上。

```python
llm_with_tools = ChatOpenAI(model="gpt-4o-mini").bind_tools(tools)
```

`bind_tools` 方法的作用是：当我们向 LLM 提问时，模型会评估自身能否直接回答。如果问题超出其知识范围（例如查询实时天气），模型会“建议”调用我们提供的合适工具，而不是强行生成一个可能错误的答案。

## 理解工具调用机制

![](img/7013576fa2a5e986d25b24ae1a53158a_1.png)

为了理解 `bind_tools` 的工作原理，我们先进行一个简单的测试。

如果我们询问一个普通问题：
```python
response = llm_with_tools.invoke("hi, my name is Harsh")
print(response.content)
# 输出: Nice to meet you, Harsh! Is there anything I can help you with?
```
此时，模型直接回应，与未绑定工具时无异。

但如果我们询问一个需要工具才能回答的问题：
```python
response = llm_with_tools.invoke("What's the weather in Bangalore?")
print(response.content)
# 输出: (空字符串)
print(response.tool_calls)
# 输出: [{'name': 'tavily_search_results_json', 'args': {'query': 'weather Bangalore'}, ...}]
```
可以看到，`content` 字段为空，而 `tool_calls` 字段包含了一个对象。这个对象指示了模型希望调用的工具名称（`tavily_search_results_json`）以及调用时所需的参数（查询词 `weather Bangalore`）。这就是工具调用的核心：模型输出的是一个“行动指令”，而非最终答案。

## 构建带工具调用的图

![](img/7013576fa2a5e986d25b24ae1a53158a_3.png)

理解了机制后，我们来构建完整的图。其工作流程如下图所示：

![](img/7013576fa2a5e986d25b24ae1a53158a_1.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_5.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_6.png)

流程解析：
1.  用户输入进入 `chatbot` 节点（即绑定了工具的 LLM）。
2.  LLM 判断是否需要调用工具。
    *   如果不需要（`tool_calls` 为空），则直接生成回答，流程走向 `END`。
    *   如果需要（`tool_calls` 不为空），则流程走向 `tools` 节点。
3.  `tools` 节点执行 LLM 指定的工具调用。
4.  工具执行的结果会作为一条新的“工具消息”添加到对话历史中。
5.  包含工具执行结果的完整对话历史再次被送入 `chatbot` 节点。
6.  此时 LLM 拥有了回答问题所需的全部信息，生成最终回答并流向 `END`。

![](img/7013576fa2a5e986d25b24ae1a53158a_7.png)

### 第一步：创建 Chatbot 节点

`chatbot` 节点与之前类似，但这里我们使用绑定了工具的 `llm_with_tools`。

![](img/7013576fa2a5e986d25b24ae1a53158a_9.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_10.png)

```python
def chatbot_node(state: State):
    messages = state[‘messages’]
    response = llm_with_tools.invoke(messages)
    return {“messages”: [response]}
```

### 第二步：创建条件路由函数

我们需要一个函数来判断 LLM 的输出是否包含工具调用，从而决定下一步是去 `tools` 节点还是直接结束。

以下是该函数的实现：
```python
def tools_router(state: State):
    # 获取最后一条消息（即刚生成的 AI 消息）
    last_message = state[‘messages’][-1]
    # 检查该消息是否包含 tool_calls 且列表不为空
    if hasattr(last_message, ‘tool_calls’) and last_message.tool_calls:
        return “tools”  # 需要调用工具
    return “end”  # 可以直接结束
```

### 第三步：创建工具执行节点

LangGraph 提供了便捷的 `ToolNode` 类来处理工具调用。它会自动查找最后一条 AI 消息中的 `tool_calls`，并执行其中指定的所有工具。

```python
tool_node = ToolNode(tools)
```
默认情况下，`ToolNode` 会在状态（State）的 `messages` 键中查找消息列表。如果你的消息列表使用不同的键名，可以通过 `messages_key` 参数指定，例如 `ToolNode(tools, messages_key=“chat_history”)`。

![](img/7013576fa2a5e986d25b24ae1a53158a_12.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_13.png)

### 第四步：组装图

![](img/7013576fa2a5e986d25b24ae1a53158a_14.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_15.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_17.png)

现在，我们将所有节点和边组合起来，构建完整的图。

![](img/7013576fa2a5e986d25b24ae1a53158a_19.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_20.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_22.png)

以下是构建图的步骤：
1.  创建状态图，指定状态结构。
2.  添加节点：`chatbot` 和 `tools`。
3.  设置入口点为 `chatbot` 节点。
4.  添加边：
    *   从 `chatbot` 到 `tools_router` 的条件边。
    *   从 `tools` 节点回到 `chatbot` 节点的普通边（以便将工具结果返回给 LLM 进行总结）。

![](img/7013576fa2a5e986d25b24ae1a53158a_23.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_25.png)

```python
# 1. 创建图
workflow = StateGraph(State)

# 2. 添加节点
workflow.add_node(“chatbot”, chatbot_node)
workflow.add_node(“tools”, tool_node)

![](img/7013576fa2a5e986d25b24ae1a53158a_27.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_28.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_29.png)

# 3. 设置入口点
workflow.set_entry_point(“chatbot”)

# 4. 添加边
workflow.add_conditional_edges(
    “chatbot”,
    tools_router,
    {
        “tools”: “tools”,  # 如果 tools_router 返回 “tools”，则前往 tools 节点
        “end”: END  # 如果返回 “end”，则结束
    }
)
workflow.add_edge(“tools”, “chatbot”)  # 工具执行完后，回到 chatbot 节点

# 5. 编译图
app = workflow.compile()
```

## 运行与测试

![](img/7013576fa2a5e986d25b24ae1a53158a_31.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_32.png)

![](img/7013576fa2a5e986d25b24ae1a53158a_34.png)

让我们编译并运行这个图进行测试。

首先测试一个无需工具的问题：
```python
inputs = {“messages”: [(“user”, “Hi, I‘m Harsh.”)]}
result = app.invoke(inputs)
print(result[“messages”][-1].content)
# 输出可能为：Hello Harsh! Nice to meet you.
```

接着测试需要工具调用的问题：
```python
inputs = {“messages”: [(“user”, “What is the current weather in Bangalore?”)]}
result = app.invoke(inputs)
```

![](img/7013576fa2a5e986d25b24ae1a53158a_36.png)

执行流程如下：
1.  **用户消息**：`“What is the current weather in Bangalore?”`
2.  **AI 消息（第一次）**：`content` 为空，`tool_calls` 包含调用 `tavily_search` 工具的指令及查询参数。
3.  **工具消息**：`ToolNode` 执行搜索，返回包含天气信息的工具消息。
4.  **AI 消息（第二次）**：LLM 收到包含天气信息的完整对话历史，生成最终答案，例如：`“The current weather in Bangalore is partly cloudy with a temperature of 32.3 degrees Celsius.”`

通过检查最终的 `result[“messages”]`，你可以清晰地看到这四条消息的完整链条，验证了工具调用的整个过程。

## 总结

本节课中，我们一起学习了如何为 LangGraph 聊天机器人添加工具调用能力。核心要点包括：
1.  使用 **`model.bind_tools(tools)`** 将工具绑定到 LLM，使其具备调用外部功能的能力。
2.  理解 LLM 的输出结构：当需要工具时，**`content` 为空**，而 **`tool_calls` 字段**包含了调用的具体指令。
3.  利用 **`ToolNode`** 这个预构建节点来简化工具的执行过程。
4.  设计 **条件路由逻辑**（`tools_router`），根据 `tool_calls` 是否存在来决定图的工作流走向。
5.  构建了一个完整的循环图：`chatbot -> (判断) -> tools -> chatbot -> end`，使得工具调用结果能反馈给 LLM 并生成最终回答。

目前，我们的机器人还无法记住对话历史（每次提问都是独立的）。在下一节中，我们将为机器人添加记忆功能，使其能够进行连贯的多轮对话。