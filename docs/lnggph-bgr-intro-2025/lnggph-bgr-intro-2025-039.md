# 039：高级多步推理 RAG 系统 🚀

![](img/6b3084f1f6e267d7e6580fd84e9afae1_1.png)

在本节课中，我们将构建一个可用于生产环境的高级多步推理 RAG（检索增强生成）系统。这个系统能够处理复杂的用户查询，包括上下文不完整的后续问题，并能智能地判断问题是否相关、检索到的信息是否足够，从而在各种边缘情况下都能给出合适的响应。

## 系统流程图概览 📊

![](img/6b3084f1f6e267d7e6580fd84e9afae1_3.png)

在深入代码之前，我们先通过流程图来了解整个系统的运作流程。虽然它看起来有些复杂，但原理其实很简单。



上图展示了系统的核心流程。我们从 `Start` 节点开始，然后进入一个名为 `Question Rewriter` 的新节点。

## 为什么需要问题重写节点？ 🤔

让我们以一个健身房（Peak Performance Gym）的客服系统为例。假设用户首次提问：“Peak Performance Gym 的营业时间是几点？”



在这种情况下，我们通常的做法是：将用户查询发送给检索器（Retriever），检索器从向量数据库中获取相关文档片段，然后将用户问题和这些上下文一起交给大语言模型（LLM）来生成答案。这个过程工作得很好。

但是，如果用户接着问了一个后续问题，比如：“那周末呢？”



![](img/6b3084f1f6e267d7e6580fd84e9afae1_5.png)

此时，我们不能单独使用“那周末呢？”这个字符串去检索，因为它本身缺乏上下文，单独看没有意义。它必须结合整个对话历史才能被理解。因此，我们需要对这个问题进行重写，使其包含所有必要的上下文信息，这样检索器才有足够的信息去获取相关的文档片段。

简单来说，如果不重写后续查询，这个查询本身缺乏上下文，如果直接发送给检索系统，很可能会返回不相关的结果。重写节点的作用就是将“那周末呢？”转化为“Peak Performance Gym 的周末营业时间是几点？”。这就是我们在流程最开始时设置这个节点的原因。

## 问题分类器节点 🏷️

在重写问题之后，我们遇到了分类器节点。这个节点我们之前已经见过，它的作用很简单：判断用户的问题是**相关**还是**不相关**。



我们不希望系统回答用户提出的与业务完全无关的随机问题。如果问题被判定为不相关，流程将转向 `Off Topic` 节点，然后结束。我们可以在这里提供一个标准的回复，例如“我无法回答这个问题”。

但如果问题是相关的，系统将自信地继续前进，从向量存储中获取文档片段。

## 检索与相关性评分节点 🔍

这就是 `Retrieve` 节点要做的事情。例如，我们可以设置 `k=4`，让它获取四个不同的文档片段。

检索完成后，我们进入 `Retrieval Grader` 节点。这个节点会遍历获取到的每一个文档片段，并检查该片段是否与用户的问题相关。因为我们设置了 `k=4`，可能其中一两个片段根本不相关。这个节点的工作就是为每个片段评分：相关还是不相关。

![](img/6b3084f1f6e267d7e6580fd84e9afae1_7.png)

这里可能会出现几种不同的边缘情况：
1.  **至少有一个片段相关**：LLM 可以利用它来回答用户的问题。在这种情况下，流程将转向 `Generate Answer` 节点，然后结束。
2.  **没有一个片段相关**：流程将转向 `Refine Question` 节点。这个节点会为问题添加更多信息或调整措辞，使其更完善，从而为检索器提供更好的信息，希望从向量数据库中检索到更相关的片段。

我们可能会遇到这样的情况：无论我们如何修改问题，都无法检索到相关的片段。为了避免无限循环消耗大量 tokens，我们设置了一个最大循环限制（例如三次）。如果在循环中成功获取到了有用信息，就转向 `Generate Answer` 节点；如果循环达到三次上限，则意味着这是一个“无解”的问题，我们无法回答。此时，流程将转向 `Cannot Answer` 节点，回复“我目前没有这个信息”之类的内容。由于问题本身仍是相关的，我们也可以在这里将其路由给人工客服处理。

`Cannot Answer`、`Generate Answer` 和 `Refine Question` 节点共同处理了我们在生产环境中可能遇到的所有潜在边缘情况。

希望这个介绍能让你对整个系统有一个清晰的理解。现在，让我们开始深入研究代码。

## 代码实现 💻

我已经创建了一个名为 `advanced_multistep_reasoning.py` 的文件。第一个代码块与之前类似，我复用了相同的示例数据（关于 Peak Performance Gym 的五个文档），并初始化了数据库和检索器。

接下来，我们初始化模型（使用 GPT-4）并设置提示词模板。这次，我们会在最终的 LLM 调用中提供**整个对话历史**、检索器获取的**上下文**以及**问题**（可能是重写后的问题）。

![](img/6b3084f1f6e267d7e6580fd84e9afae1_9.png)

```python
# 提示词模板示例
template = """请基于以下上下文和聊天历史回答问题，尤其要考虑最新的问题：

![](img/6b3084f1f6e267d7e6580fd84e9afae1_11.png)

聊天历史：
{history}

上下文：
{context}

问题：{question}
"""
```

然后，我们创建 RAG 链。

### 定义智能体状态 🧠

![](img/6b3084f1f6e267d7e6580fd84e9afae1_13.png)

为了实现这个系统，我们需要在智能体状态中维护更多属性。

```python
from typing import TypedDict, List, Optional, Literal
from langgraph.graph import StateGraph

class AgentState(TypedDict):
    messages: List  # 消息列表，用于维护对话历史
    documents: List  # 检索器获取的所有文档
    on_topic: Optional[Literal[“yes”, “no”]]  # 用户问题是否相关
    rephrased_question: str  # LLM 重写后的问题（如果上下文不完整）
    proceed_to_generate: bool  # 是否继续生成答案
    rephrase_count: int  # 问题重写的循环次数
    question: str  # 用户原始的最新问题
```

*   `messages`：维护所有消息，以保持对话历史。
*   `documents`：检索器获取的所有文档。
*   `on_topic`：用户问题是相关（on-topic）还是不相关（off-topic）。
*   `rephrased_question`：如果用户问题缺乏完整上下文，LLM 将重写它并存储在这里。
*   `proceed_to_generate`：这个标志决定流程是走向生成答案，还是需要重写问题，或者在达到重试次数后结束。
*   `rephrase_count`：用于控制重写循环的次数。
*   `question`：保存用户原始的最新问题。

![](img/6b3084f1f6e267d7e6580fd84e9afae1_15.png)

### 使用 Pydantic 模型进行结构化输出 🏗️

我们使用 Pydantic 模型来从 LLM 获取结构化的数据输出。

```python
from pydantic import BaseModel, Field

class GradeQuestion(BaseModel):
    score: str = Field(description=”问题是否关于指定主题。如果是则返回 ‘yes’，否则返回 ‘no’。”)
```

这个 Pydantic 模型将强制 LLM 输出一个包含 `score` 字段的结构，其值只能是 “yes” 或 “no”。我们稍后会在分类器节点中使用它。

### 问题重写节点 🔄

![](img/6b3084f1f6e267d7e6580fd84e9afae1_17.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_18.png)

现在让我们看看第一个节点：`question_rewriter`。

```python
def question_rewriter(state: AgentState):
    print(“进入问题重写节点，当前状态：”, state)
    # 每次运行都重置状态变量（除了 messages 和 question）
    state[“documents”] = []
    state[“on_topic”] = None
    state[“rephrased_question”] = “”
    state[“proceed_to_generate”] = False
    state[“rephrase_count”] = 0

    # 确保 messages 列表存在
    if “messages” not in state or not state[“messages”]:
        state[“messages”] = []

    # 如果 question 不在 messages 中，则添加进去
    if state[“question”] not in [msg.content for msg in state[“messages”] if hasattr(msg, ‘content’)]:
        # 添加用户消息到历史
        pass # 具体添加逻辑

    # 检查是否需要重写问题（如果不是第一次对话）
    if len(state[“messages”]) > 1:
        # 提取对话历史（最后一条消息除外）
        history = state[“messages”][:-1]
        current_question = state[“messages”][-1].content

        # 构建系统消息，要求 LLM 重写问题以优化检索
        system_msg = “你是一个乐于助人的助手，负责将用户的问题重新表述为独立的、适合检索的问题。”
        # 调用 LLM 进行重写
        # response = llm.invoke(…)
        rephrased = response.content.strip()
        state[“rephrased_question”] = rephrased
        print(f”重写后的问题：{rephrased}”)
    else:
        # 如果是第一次对话，直接使用原始问题
        state[“rephrased_question”] = state[“question”]

    return state
```

这个节点在每次运行时，都会重置大部分状态变量（除了 `messages` 和 `question`），以确保每次查询都从一个干净的状态开始（但保留对话历史）。然后，它检查对话历史长度。如果大于1，说明不是首次对话，它就会提取历史记录和当前问题，请求 LLM 将其重写为一个包含完整上下文的独立问题，并将结果存入 `rephrased_question`。

### 问题分类器节点 🏷️

![](img/6b3084f1f6e267d7e6580fd84e9afae1_20.png)

重写问题后，我们需要检查它是否仍然与主题相关。

```python
def classifier(state: AgentState):
    print(“进入分类器节点”)
    # 定义系统消息，说明需要判断的主题
    system_message = “””你是一个分类器，用于判断用户的问题是否关于以下主题：
    - 健身房历史、创始人、营业时间、会员计划、健身课程、私人教练。
    如果问题是关于以上任何主题，请回答 ‘yes’，否则回答 ‘no’。”””

    user_question = state[“rephrased_question”] or state[“question”]
    human_message = {“role”: “user”, “content”: user_question}

    # 创建提示
    prompt = [system_message, human_message]

    # 使用带有结构化输出的 LLM
    structured_llm = llm.with_structured_output(GradeQuestion)
    result = structured_llm.invoke(prompt)
    # 清理输出并存入状态
    state[“on_topic”] = result.score.strip().lower()
    print(f”分类结果：{state[‘on_topic’]}”)
    return state
```

这个节点使用带有 Pydantic 工具的结构化 LLM，强制输出一个包含 `score`（“yes” 或 “no”）的对象，并将其存入 `on_topic` 状态。

### 路由节点：相关 vs 不相关 ➡️

![](img/6b3084f1f6e267d7e6580fd84e9afae1_22.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_23.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_24.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_25.png)

根据分类结果，我们需要一个路由节点来决定流程的走向。

![](img/6b3084f1f6e267d7e6580fd84e9afae1_27.png)

```python
def on_off_topic_router(state: AgentState):
    print(“进入相关/不相关路由节点”)
    if state[“on_topic”] == “yes”:
        return “retrieve” # 前往检索节点
    else:
        return “off_topic_response” # 前往不相关回复节点
```

### 检索节点 📥

如果问题相关，就进入检索节点。

```python
def retrieve(state: AgentState):
    print(“进入检索节点”)
    question = state[“rephrased_question”] or state[“question”]
    # 调用检索器
    docs = retriever.invoke(question)
    state[“documents”] = docs
    print(f”检索到 {len(docs)} 个文档片段”)
    return state
```

### 检索结果评分节点 📊

检索完成后，我们需要对每个结果进行评分。

![](img/6b3084f1f6e267d7e6580fd84e9afae1_29.png)

```python
def retrieval_grader(state: AgentState):
    print(“进入检索评分节点”)
    relevant_docs = []
    question = state[“rephrased_question”] or state[“question”]

    # 定义评分用的 Pydantic 模型
    class GradeDocument(BaseModel):
        score: str = Field(description=”文档是否与问题相关。如果是则返回 ‘yes’，否则返回 ‘no’。”)

    grader_llm = llm.with_structured_output(GradeDocument)

    for doc in state[“documents”]:
        # 构建提示，包含问题和文档内容
        human_msg = f”问题：{question}\n\n文档：{doc.page_content}”
        result = grader_llm.invoke(human_msg)
        if result.score.strip().lower() == “yes”:
            relevant_docs.append(doc)

    # 更新状态，只保留相关文档
    state[“documents”] = relevant_docs
    # 根据是否有相关文档决定是否继续生成
    state[“proceed_to_generate”] = len(relevant_docs) > 0
    print(f”相关文档数量：{len(relevant_docs)}，是否继续生成：{state[‘proceed_to_generate’]}”)
    return state
```

这个节点遍历所有检索到的文档，使用另一个结构化 LLM 判断每个文档是否与问题相关，并只保留相关的文档。然后根据是否有相关文档来设置 `proceed_to_generate` 标志。

### 生成决策路由节点 🤔

现在我们需要另一个路由节点，根据当前状态决定下一步：生成答案、重写问题还是放弃回答。

![](img/6b3084f1f6e267d7e6580fd84e9afae1_31.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_32.png)

```python
def proceed_router(state: AgentState):
    print(“进入生成决策路由节点”)
    if state[“proceed_to_generate”]:
        return “generate_answer”
    elif state[“rephrase_count”] >= 3: # 假设最大重试3次
        return “cannot_answer”
    else:
        return “refine_question”
```

### 问题精炼节点 ✨

如果决定精炼问题，则进入此节点。

![](img/6b3084f1f6e267d7e6580fd84e9afae1_34.png)

```python
def refine_question(state: AgentState):
    print(“进入问题精炼节点”)
    old_question = state[“rephrased_question”] or state[“question”]
    # 请求 LLM 稍微调整问题以改善检索结果
    system_msg = “你是一个乐于助人的助手，负责微调用户的问题以改进检索结果。请提供一个略微调整后的问题版本。”
    human_msg = f”原始问题：{old_question}”
    # 调用 LLM
    # response = llm.invoke(…)
    refined_q = response.content.strip()
    state[“rephrased_question”] = refined_q
    state[“rephrase_count”] += 1 # 增加重试计数
    print(f”精炼后的问题：{refined_q}，重试次数：{state[‘rephrase_count’]}”)
    return state
```

![](img/6b3084f1f6e267d7e6580fd84e9afae1_36.png)

### 生成答案节点 🤖

如果一切顺利，且有相关文档，则进入此节点生成最终答案。

```python
def generate_answer(state: AgentState):
    print(“进入生成答案节点”)
    # 准备输入：历史、上下文、问题
    history = state[“messages”]
    context = “\n”.join([doc.page_content for doc in state[“documents”]])
    question = state[“rephrased_question”] or state[“question”]

    # 调用之前定义好的 RAG 链
    answer = rag_chain.invoke({“history”: history, “context”: context, “question”: question})
    # 将 AI 的回答添加到消息历史中
    state[“messages”].append(AIMessage(content=answer))
    print(f”生成的答案：{answer}”)
    return state
```

![](img/6b3084f1f6e267d7e6580fd84e9afae1_38.png)

### 无法回答节点 ❌

如果重试多次仍无相关文档，则进入此节点。

```python
def cannot_answer(state: AgentState):
    print(“进入无法回答节点”)
    response = “抱歉，我找不到您要查找的信息。”
    state[“messages”].append(AIMessage(content=response))
    print(response)
    return state
```

![](img/6b3084f1f6e267d7e6580fd84e9afae1_40.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_41.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_42.png)

### 不相关回复节点 🚫

如果问题被分类为不相关，则进入此节点。

```python
def off_topic_response(state: AgentState):
    print(“进入不相关回复节点”)
    response = “抱歉，我无法回答这个问题，因为它与主题无关。”
    state[“messages”].append(AIMessage(content=response))
    print(response)
    return state
```

### 组装图并添加检查点 💾

![](img/6b3084f1f6e267d7e6580fd84e9afae1_44.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_45.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_46.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_47.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_48.png)

我们已经定义了所有节点，现在需要将它们组装成图，并添加检查点（Checkpointer）以支持多轮对话的记忆功能。

```python
from langgraph.checkpoint.sqlite import SqliteSaver

![](img/6b3084f1f6e267d7e6580fd84e9afae1_50.png)

# 初始化检查点存储器
memory = SqliteSaver.from_conn_string(“:memory:”) # 示例中使用内存数据库

![](img/6b3084f1f6e267d7e6580fd84e9afae1_51.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_52.png)

# 创建状态图
workflow = StateGraph(AgentState)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_54.png)

# 添加所有节点
workflow.add_node(“question_rewriter”, question_rewriter)
workflow.add_node(“classifier”, classifier)
workflow.add_node(“retrieve”, retrieve)
workflow.add_node(“retrieval_grader”, retrieval_grader)
workflow.add_node(“refine_question”, refine_question)
workflow.add_node(“generate_answer”, generate_answer)
workflow.add_node(“cannot_answer”, cannot_answer)
workflow.add_node(“off_topic_response”, off_topic_response)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_55.png)

# 设置入口点
workflow.set_entry_point(“question_rewriter”)

# 添加边（定义流程）
workflow.add_edge(“question_rewriter”, “classifier”)
workflow.add_conditional_edges(
    “classifier”,
    on_off_topic_router, # 根据这个函数的返回值决定路由
    {
        “retrieve”: “retrieve”,
        “off_topic_response”: “off_topic_response”
    }
)
workflow.add_edge(“retrieve”, “retrieval_grader”)
workflow.add_conditional_edges(
    “retrieval_grader”,
    proceed_router, # 根据这个函数的返回值决定路由
    {
        “generate_answer”: “generate_answer”,
        “refine_question”: “refine_question”,
        “cannot_answer”: “cannot_answer”
    }
)
workflow.add_edge(“refine_question”, “retrieve”) # 精炼问题后回到检索
workflow.add_edge(“generate_answer”, END)
workflow.add_edge(“cannot_answer”, END)
workflow.add_edge(“off_topic_response”, END)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_57.png)

# 编译图，并传入检查点存储器
app = workflow.compile(checkpointer=memory)
```

![](img/6b3084f1f6e267d7e6580fd84e9afae1_59.png)

## 测试系统 🧪

现在让我们测试几个边缘情况，看看系统如何响应。

### 测试 1：不相关的问题

用户提问：“苹果公司是做什么的？”（与健身房业务完全无关）

![](img/6b3084f1f6e267d7e6580fd84e9afae1_61.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_62.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_63.png)

系统流程：
1.  进入 `question_rewriter`，由于是第一个问题，无需重写。
2.  进入 `classifier`，`on_topic` 被判定为 “no”。
3.  路由到 `off_topic_response`。
4.  输出：“抱歉，我无法回答这个问题，因为它与主题无关。”

### 测试 2：相关但无答案的问题

用户提问：“Peak Performance Gym 会员的取消政策是什么？”（文档中未包含此信息）

![](img/6b3084f1f6e267d7e6580fd84e9afae1_65.png)

系统流程：
1.  问题被重写和分类为相关 (`on_topic: yes`)。
2.  `retrieve` 获取4个文档。
3.  `retrieval_grader` 判断所有文档都不相关 (`proceed_to_generate: false`)。
4.  由于 `rephrase_count` 未超限，路由到 `refine_question`，问题被精炼（例如改为“Peak Performance Gym 的会员资格取消政策是什么？”）。
5.  流程跳回 `retrieve`，再次检索和评分，仍然不相关。
6.  重复精炼和检索过程。
7.  当 `rephrase_count` 达到3次上限后，路由到 `cannot_answer`。
8.  输出：“抱歉，我找不到您要查找的信息。”

### 测试 3：简单问题及后续问题

**第一轮：**
用户提问：“谁创立了 Peak Performance Gym？”
系统流程：检索到相关文档，生成答案：“Peak Performance Gym 由前奥运运动员 Marcus Chen 创立。”

**第二轮：**
用户提问：“他什么时候创立的？”（“他”指代不明）
系统流程：
1.  `question_rewriter` 将问题重写为“Marcus Chen 什么时候创立了 Peak Performance Gym？”。
2.  后续流程检索到相关文档（例如包含“成立于2015年”的文档）。
3.  生成最终答案：“它成立于2015年。”

![](img/6b3084f1f6e267d7e6580fd84e9afae1_67.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_68.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_69.png)

![](img/6b3084f1f6e267d7e6580fd84e9afae1_70.png)

## 总结 📝

在本节课中，我们一起学习并构建了一个高级的多步推理 RAG 系统。这个系统通过引入**问题重写**、**主题分类**、**检索结果评分**、**问题精炼循环**和**多种决策路由**，显著增强了传统 RAG 的鲁棒性。它能够：
*   处理上下文不完整的后续问题。
*   过滤掉与业务无关的查询。
*   确保生成答案所依据的文档是高度相关的。
*   在无法找到答案时，通过有限次数的尝试精炼查询，并在最终失败时优雅地告知用户。
*   通过检查点机制支持连贯的多轮对话。

![](img/6b3084f1f6e267d7e6580fd84e9afae1_72.png)

这个架构为构建可用于生产环境的、可靠的问答系统提供了一个强大的蓝图。建议你下载代码并亲自运行，以加深理解。我们下个章节再见！