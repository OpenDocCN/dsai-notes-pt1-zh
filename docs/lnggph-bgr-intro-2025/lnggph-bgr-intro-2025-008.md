# 008：构建反思代理图

## 概述
在本节课中，我们将学习如何构建自己的第一个 LangGraph 图。我们将利用上一节创建的链，构建一个反思代理。这个代理会生成内容，然后进行自我反思和优化，循环数次后输出最终结果。

![](img/dbc4426934a26bcd889bde19b7260a76_1.png)

## 准备工作
在开始编写代码之前，需要确保已安装必要的包。使用以下命令安装 LangGraph：
```bash
pip install langgraph
```

接下来，我们导入所需的模块和类。

```python
from typing import List, Sequence
from dotenv import load_dotenv
from langchain_core.messages import BaseMessage, HumanMessage
from langgraph.graph import END, MessageGraph
from chains import generation_chain, reflection_chain
```

以下是导入内容的说明：
*   `List` 和 `Sequence`：用于类型提示，帮助检查代码，类似于 TypeScript。
*   `load_dotenv`：用于加载环境变量。
*   `BaseMessage` 和 `HumanMessage`：LangChain 中处理消息的类。
*   `END` 和 `MessageGraph`：来自 LangGraph 的核心类，用于构建图。
*   `generation_chain` 和 `reflection_chain`：我们在上一节创建的两个链。

![](img/dbc4426934a26bcd889bde19b7260a76_3.png)

![](img/dbc4426934a26bcd889bde19b7260a76_4.png)

![](img/dbc4426934a26bcd889bde19b7260a76_5.png)

## 理解 MessageGraph
在开始构建之前，我们需要理解什么是 MessageGraph。

MessageGraph 是 LangGraph 提供的一个类，用于协调不同节点之间的消息流。它维护一个消息列表，并决定这些消息如何在节点间流动。

**MessageGraph 的核心机制**：
*   图中的每个节点都将收到**完整的、之前所有消息的列表**作为输入。
*   每个节点可以生成新的消息，并将其**追加**到这个列表中。
*   更新后的消息列表会被传递给下一个节点。

简单来说，每个节点都能看到整个对话历史，并在此基础上添加自己的回复，然后将更新后的历史传递给下一个节点。这种“生成 -> 反思 -> 再生成”的循环工作流非常适合使用 MessageGraph 来实现。

如果你的应用需要管理更复杂的状态（而不仅仅是消息列表），则可以考虑使用 `StateGraph`。但就本节课的用例而言，`MessageGraph` 完全够用。

![](img/dbc4426934a26bcd889bde19b7260a76_7.png)

## 构建图结构
理解了 MessageGraph 后，我们现在开始构建自己的图。整个过程类似于绘制流程图并控制节点间的连接。

![](img/dbc4426934a26bcd889bde19b7260a76_9.png)

![](img/dbc4426934a26bcd889bde19b7260a76_10.png)

![](img/dbc4426934a26bcd889bde19b7260a76_11.png)

首先，我们初始化一个 MessageGraph 实例。
```python
graph = MessageGraph()
```

现在图是空的。我们需要创建节点，将它们添加到图中，然后连接它们。为了清晰，我们先定义节点名称常量。
```python
REFLECT = "reflect"
GENERATE = "generate"
```

![](img/dbc4426934a26bcd889bde19b7260a76_13.png)

![](img/dbc4426934a26bcd889bde19b7260a76_14.png)

### 创建生成节点
生成节点是一个函数，它接收当前状态（即消息列表），调用生成链，并将链的响应追加到历史中。
```python
def generate_node(state: List[BaseMessage]):
    # 调用生成链，传入当前所有消息
    response = generation_chain.invoke({"messages": state})
    # 将AI的响应作为新消息追加到状态列表中
    return [response]
```
这个函数的作用是：基于已有的对话历史，生成一条新的推文。

![](img/dbc4426934a26bcd889bde19b7260a76_16.png)

![](img/dbc4426934a26bcd889bde19b7260a76_17.png)

![](img/dbc4426934a26bcd889bde19b7260a76_18.png)

![](img/dbc4426934a26bcd889bde19b7260a76_19.png)

![](img/dbc4426934a26bcd889bde19b7260a76_20.png)

### 创建反思节点
反思节点的结构与生成节点类似，但它调用的是反思链来批判上一步生成的内容。
```python
def reflect_node(state: List[BaseMessage]):
    # 调用反思链，传入当前所有消息
    response = reflection_chain.invoke({"messages": state})
    # 将反思内容作为新消息追加到状态列表中
    return [response]
```
**一个可选技巧**：在现实中，反思通常由人类完成。为了让日志跟踪更清晰，我们可以将反思节点的输出标记为来自“人类”。这可以通过返回一个 `HumanMessage` 对象来实现，但这并非必需。简单的 `return [response]` 同样有效。

![](img/dbc4426934a26bcd889bde19b7260a76_22.png)

### 将节点添加到图中
创建好节点函数后，我们将它们添加到图中。
```python
graph.add_node(GENERATE, generate_node)
graph.add_node(REFLECT, reflect_node)
```

![](img/dbc4426934a26bcd889bde19b7260a76_24.png)

![](img/dbc4426934a26bcd889bde19b7260a76_25.png)

![](img/dbc4426934a26bcd889bde19b7260a76_26.png)

![](img/dbc4426934a26bcd889bde19b7260a76_27.png)

### 设置入口点
每个图都需要一个起始节点。在我们的流程中，入口点是 `GENERATE` 节点。
```python
graph.set_entry_point(GENERATE)
```

## 控制循环流程
我们的目标是让“生成”和“反思”循环进行数次（例如4次），然后结束。这需要一个条件判断函数。

以下是 `should_continue` 函数，它检查消息列表的长度。如果循环次数未达到上限，就前往反思节点；否则，结束流程。
```python
def should_continue(state: List[BaseMessage]):
    # 如果消息数量超过设定值（例如4条），则结束
    if len(state) > 4:
        return END
    # 否则，继续前往反思节点
    return REFLECT
```

![](img/dbc4426934a26bcd889bde19b7260a76_29.png)

![](img/dbc4426934a26bcd889bde19b7260a76_30.png)

![](img/dbc4426934a26bcd889bde19b7260a76_31.png)

![](img/dbc4426934a26bcd889bde19b7260a76_32.png)

## 连接节点
现在我们需要用“边”将节点连接起来，定义消息的流动路径。

首先，在生成节点之后，我们需要根据 `should_continue` 函数的决定，选择是前往反思节点还是结束。这是一个**条件边**。
```python
graph.add_conditional_edges(
    GENERATE, # 源节点
    should_continue, # 条件函数
    {
        REFLECT: REFLECT, # 如果返回“reflect”，则前往反思节点
        END: END # 如果返回“end”，则结束
    }
)
```

其次，从反思节点回到生成节点的连接是固定的，这是一个普通的边。
```python
graph.add_edge(REFLECT, GENERATE) # 从反思节点连接到生成节点
```

## 编译与可视化
完成所有节点和边的配置后，我们编译这个图以创建可执行的应用。
```python
app = graph.compile()
```

在正式运行前，最好先可视化图的结构，确保它与设计一致。以下代码可以生成 Mermaid 和 ASCII 格式的图表。
```python
# 生成Mermaid图（用于图表渲染）
print(graph.get_graph().draw_mermaid())
# 生成ASCII图（在终端中查看）
print(graph.get_graph().draw_ascii())
```
如果运行这些可视化代码时提示缺少 `grandalf` 包，请使用 `pip install grandalf` 安装。

## 运行反思代理
图构建并编译完成后，就可以运行它了。我们通过传入一个初始的人类消息来启动整个流程。
```python
# 定义初始提示
initial_message = HumanMessage(content="I want there to be a tweet written on AI agents taking over content creation")
# 调用图应用
final_state = app.invoke(initial_message)
# 打印最终结果
print(final_state)
```
**运行过程解释**：
1.  入口点 `GENERATE` 节点收到包含初始消息的状态列表。
2.  生成链基于此生成第一条推文（AI消息），并将其追加到列表中。
3.  `should_continue` 函数判断循环次数，由于未达到上限，流程前往 `REFLECT` 节点。
4.  反思链对刚生成的推文进行批判（可标记为人类消息），并将批判内容追加到列表。
5.  通过普通边，流程再次回到 `GENERATE` 节点。
6.  生成链现在看到的是包含初始提示、第一版推文和批判的完整历史，并据此生成改进版的第二版推文。
7.  此循环持续进行，直到 `should_continue` 函数判断次数已够，流程走向 `END`，并返回最终的消息列表。

**注意**：整个过程中只有一个共享的、不断增长的消息历史列表。每个链（生成链和反思链）有自己独立的系统提示，但它们都读取和写入这个共享的历史。

## 故障排除与模型选择
在运行过程中，你可能会遇到 API 调用频率限制的问题（尤其是使用免费的 Gemini 模型时）。如果发生这种情况，可以考虑切换到付费 API，如 OpenAI。

只需在 `chains.py` 文件中将模型从 `ChatGoogleGenerativeAI` 替换为 `ChatOpenAI`，并设置相应的环境变量（如 `OPENAI_API_KEY`）即可。
```python
# 在chains.py中替换模型
from langchain_openai import ChatOpenAI
model = ChatOpenAI(model="gpt-4")
```

![](img/dbc4426934a26bcd889bde19b7260a76_34.png)

![](img/dbc4426934a26bcd889bde19b7260a76_35.png)

## 总结
本节课我们一起学习了如何从零开始构建一个 LangGraph 图。我们完成了以下步骤：
1.  **理解 MessageGraph** 的工作原理：它管理消息列表并在节点间传递。
2.  **创建节点**：定义了 `generate_node` 和 `reflect_node` 两个函数。
3.  **组装图**：将节点添加到图中，设置入口点。
4.  **控制流程**：使用 `should_continue` 函数和条件边来实现循环逻辑。
5.  **连接节点**：使用 `add_conditional_edges` 和 `add_edge` 方法定义消息流向。
6.  **编译与运行**：编译图并传入初始消息来执行反思代理工作流。

![](img/dbc4426934a26bcd889bde19b7260a76_37.png)

通过这个实践，你已经掌握了构建基础 LangGraph 的核心技能。在下一节中，我们将使用 LangSmith 来追踪和可视化这个图的详细执行过程，让每一步都更加清晰。