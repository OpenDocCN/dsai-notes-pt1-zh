#  021：使用 LLaMA3 构建可靠、完全本地的 RAG 智能体 🚀

![](img/442408c9b7f45e8c326836ae513300da_0.png)

![](img/442408c9b7f45e8c326836ae513300da_2.png)

![](img/442408c9b7f45e8c326836ae513300da_3.png)

在本节课中，我们将学习如何利用最新发布的 LLaMA3 模型，构建一个能够完全在本地笔记本电脑上运行的、可靠的检索增强生成智能体。我们将结合多篇 RAG 论文的核心思想，设计一个复杂的控制流，确保智能体的稳定性和准确性。

---

![](img/442408c9b7f45e8c326836ae513300da_5.png)

![](img/442408c9b7f45e8c326836ae513300da_6.png)

![](img/442408c9b7f45e8c326836ae513300da_7.png)

## 智能体与 RAG 概述 🤖

![](img/442408c9b7f45e8c326836ae513300da_8.png)

![](img/442408c9b7f45e8c326836ae513300da_10.png)

![](img/442408c9b7f45e8c326836ae513300da_11.png)

上一节我们介绍了课程目标，本节中我们来看看智能体的基本概念以及我们如何将其与 RAG 流程结合。

![](img/442408c9b7f45e8c326836ae513300da_13.png)

智能体的定义存在一些争议，但一个被广泛接受的观点来自 Lilian Wang 的博客。一个智能体通常具备以下三个核心能力：
*   **规划**：能够将复杂任务分解为更小的子目标或子任务。
*   **记忆**：拥有记忆能力，包括对话历史和存储在向量数据库中的长期记忆。
*   **工具使用**：能够调用外部工具来完成任务。

我们的目标是构建一个实现“纠正式 RAG”流程的智能体。通常，人们会想到使用 **ReAct** 框架来构建智能体。ReAct 代理的工作流程是一个循环：语言模型选择动作、观察结果、进行思考，然后选择下一个动作。它同样可以利用记忆和工具。

如果使用 ReAct 代理来实现我们想要的 RAG 流程，其轨迹可能如下：智能体首先执行动作（如从向量库检索文档），观察结果，思考是否需要评估文档相关性，然后选择评估工具，如此循环，直到完成整个流程。

![](img/442408c9b7f45e8c326836ae513300da_15.png)

---

![](img/442408c9b7f45e8c326836ae513300da_16.png)

![](img/442408c9b7f45e8c326836ae513300da_17.png)

![](img/442408c9b7f45e8c326836ae513300da_19.png)

## 从 ReAct 到 LangGraph 控制流 🔄

![](img/442408c9b7f45e8c326836ae513300da_21.png)

![](img/442408c9b7f45e8c326836ae513300da_22.png)

上一节我们介绍了 ReAct 代理的工作方式，本节中我们来看看另一种更可靠的方法：使用 **LangGraph** 定义明确的控制流。

我们不依赖智能体在每一步自主决策，而是作为工程师预先设计好整个控制流程。这样，语言模型只需在每个预设步骤中完成特定任务，而无需决定下一步该做什么。

![](img/442408c9b7f45e8c326836ae513300da_24.png)

以下是这种方法的优势对比：
*   **规划**：我们预先定义好控制流，而非由 LLM 实时规划。
*   **记忆**：我们可以使用 **图状态** 在整个控制流中持久化信息，例如与 RAG 相关的文档和问题。
*   **工具使用**：图中的每个节点都可以使用不同的工具（如检索工具、评估工具、网络搜索工具）。

![](img/442408c9b7f45e8c326836ae513300da_25.png)

![](img/442408c9b7f45e8c326836ae513300da_26.png)

![](img/442408c9b7f45e8c326836ae513300da_27.png)

**ReAct 代理** 的优势在于灵活性高，可以自由选择动作序列。但其主要挑战是**可靠性较低**，智能体必须在每个决策点都做出正确选择，这对于较小的本地模型来说容易出错。

**LangGraph 控制流** 的优势在于**可靠性高**。智能体每次都遵循相同的预设路径执行，LLM 无需在节点间进行非约束性选择。虽然灵活性受限，但正是这种约束性使得它能够与本地、较小的 LLM 模型稳定兼容，这也是本节课要传达的核心益处。

![](img/442408c9b7f45e8c326836ae513300da_29.png)

![](img/442408c9b7f45e8c326836ae513300da_30.png)

![](img/442408c9b7f45e8c326836ae513300da_31.png)

---

## 动手实践：构建本地 RAG 组件 🛠️

![](img/442408c9b7f45e8c326836ae513300da_33.png)

![](img/442408c9b7f45e8c326836ae513300da_34.png)

理论介绍完毕，现在让我们开始编写代码。我们将首先构建“纠正式 RAG”的核心部分，并单独测试每个组件。

![](img/442408c9b7f45e8c326836ae513300da_35.png)

![](img/442408c9b7f45e8c326836ae513300da_36.png)

![](img/442408c9b7f45e8c326836ae513300da_37.png)

![](img/442408c9b7f45e8c326836ae513300da_38.png)

首先进行环境准备。对于本地模型，我们使用 **Ollama** 来运行 LLaMA3。对于文本嵌入，我们使用 LangChain 合作伙伴 **Nomic** 提供的 `nomic-embed-text` 模型，效果很好。

```python
# 拉取 LLaMA3 模型
ollama pull llama3
```

### 步骤一：构建索引

RAG 流程的关键组件是索引。我们将为三篇我喜欢的博客文章构建一个本地向量索引。

![](img/442408c9b7f45e8c326836ae513300da_40.png)

![](img/442408c9b7f45e8c326836ae513300da_41.png)

![](img/442408c9b7f45e8c326836ae513300da_42.png)

```python
# 设置块大小，使用 Chroma 作为本地向量数据库
chunk_size = 512
vector_store = Chroma.from_documents(documents, embedding_model, persist_directory="./chroma_db")
```
索引构建完成，这是我们 RAG 流程的基础。

![](img/442408c9b7f45e8c326836ae513300da_43.png)

![](img/442408c9b7f45e8c326836ae513300da_44.png)

![](img/442408c9b7f45e8c326836ae513300da_45.png)

### 步骤二：创建检索评估器

![](img/442408c9b7f45e8c326836ae513300da_47.png)

![](img/442408c9b7f45e8c326836ae513300da_48.png)

![](img/442408c9b7f45e8c326836ae513300da_49.png)

接下来，我们构建检索评估器。它的功能是检索文档，并根据问题评估其相关性。这里我们将首次使用 LLaMA3。

![](img/442408c9b7f45e8c326836ae513300da_51.png)

Ollama 支持 **JSON 模式**，可以确保 LLM 的输出是格式正确的 JSON。我们的提示词非常简单：“评估文档。返回一个包含‘score’字段（值为‘yes’或‘no’）的 JSON。”

![](img/442408c9b7f45e8c326836ae513300da_52.png)

```python
# 定义评估提示词和链
grader_prompt = PromptTemplate.from_template("Grade the documents. Return a JSON with a ‘score‘ key of ‘yes‘ or ‘no‘.")
grader_chain = grader_prompt | llm | JsonOutputParser()
```

我们进行一次模拟检索并调用评估链。通过集成 LangSmith 追踪，我们可以实时查看运行细节。调用完成后，我们得到了完美的 JSON 输出，这正是我们想要的。

![](img/442408c9b7f45e8c326836ae513300da_54.png)

![](img/442408c9b7f45e8c326836ae513300da_55.png)

![](img/442408c9b7f45e8c326836ae513300da_56.png)

![](img/442408c9b7f45e8c326836ae513300da_57.png)

![](img/442408c9b7f45e8c326836ae513300da_58.png)

### 步骤三：生成答案

![](img/442408c9b7f45e8c326836ae513300da_60.png)

现在进行标准的 RAG 生成步骤。我们使用一个自定义的 RAG 提示词模板，将检索到的文档和问题输入给 LLaMA3 来生成答案。

![](img/442408c9b7f45e8c326836ae513300da_62.png)

![](img/442408c9b7f45e8c326836ae513300da_63.png)

```python
# 定义 RAG 提示词和生成链
rag_prompt = PromptTemplate.from_template("Context: {documents}\n\nQuestion: {question}\n\nAnswer:")
rag_chain = rag_prompt | llm
```
运行速度很快（大约4秒），我们可以检查包含文档的提示词和生成的输出，一切运行良好。

### 步骤四：集成网络搜索工具

![](img/442408c9b7f45e8c326836ae513300da_65.png)

![](img/442408c9b7f45e8c326836ae513300da_66.png)

![](img/442408c9b7f45e8c326836ae513300da_67.png)

![](img/442408c9b7f45e8c326836ae513300da_68.png)

![](img/442408c9b7f45e8c326836ae513300da_69.png)

我们还需要一个网络搜索工具作为备用方案。这里使用 **Tavily** 作为快速搜索工具。

![](img/442408c9b7f45e8c326836ae513300da_71.png)

```python
# 初始化 Tavily 搜索工具
search_tool = TavilySearchResults(max_results=2)
```

![](img/442408c9b7f45e8c326836ae513300da_73.png)

![](img/442408c9b7f45e8c326836ae513300da_74.png)

---

## 使用 LangGraph 组装智能体 🧩

上一节我们准备好了所有组件，本节中我们将使用 LangGraph 将它们组装成一个完整的智能体。

![](img/442408c9b7f45e8c326836ae513300da_76.png)

![](img/442408c9b7f45e8c326836ae513300da_77.png)

![](img/442408c9b7f45e8c326836ae513300da_78.png)

![](img/442408c9b7f45e8c326836ae513300da_79.png)

![](img/442408c9b7f45e8c326836ae513300da_80.png)

图中的每个绿色节点（如“检索文档”、“生成答案”、“评估文档”）都对应一个函数。每个函数接收并修改 **图状态**。

![](img/442408c9b7f45e8c326836ae513300da_81.png)

**图状态** 是一个字典，它是在智能体整个生命周期中持久化的短期记忆，包含了控制流中需要感知的所有信息（如问题、生成结果、检索到的文档、网络搜索结果等）。

![](img/442408c9b7f45e8c326836ae513300da_83.png)

![](img/442408c9b7f45e8c326836ae513300da_84.png)

![](img/442408c9b7f45e8c326836ae513300da_85.png)

以下是关键节点的函数示例：

![](img/442408c9b7f45e8c326836ae513300da_86.png)

![](img/442408c9b7f45e8c326836ae513300da_88.png)

```python
# 定义图状态结构
class GraphState(TypedDict):
    question: str
    documents: List[str]
    generation: str
    web_search_needed: bool

# 检索节点函数
def retrieve_documents(state: GraphState):
    question = state[“question“]
    documents = vector_store.invoke(question) # 从向量库检索
    return {“documents“: documents}

# 生成节点函数
def generate_answer(state: GraphState):
    question = state[“question“]
    documents = state[“documents“]
    generation = rag_chain.invoke({“question“: question, “documents“: documents})
    return {“generation“: generation}

![](img/442408c9b7f45e8c326836ae513300da_90.png)

![](img/442408c9b7f45e8c326836ae513300da_91.png)

![](img/442408c9b7f45e8c326836ae513300da_92.png)

![](img/442408c9b7f45e8c326836ae513300da_93.png)

![](img/442408c9b7f45e8c326836ae513300da_94.png)

![](img/442408c9b7f45e8c326836ae513300da_95.png)

# 评估节点函数
def grade_documents(state: GraphState):
    question = state[“question“]
    documents = state[“documents“]
    score = grader_chain.invoke({“question“: question, “documents“: documents})
    return {“documents_are_relevant“: score == “yes“}
```

![](img/442408c9b7f45e8c326836ae513300da_96.png)

![](img/442408c9b7f45e8c326836ae513300da_97.png)

![](img/442408c9b7f45e8c326836ae513300da_99.png)

通过 LangGraph，我们可以定义节点之间的边和条件逻辑。例如，在“评估文档”节点之后，我们可以设置：
*   如果文档相关，则流向“生成答案”节点。
*   如果文档不相关，则流向“网络搜索”节点。
*   在生成答案后，可以再添加一个“检查幻觉”的节点，如果答案不相关或存在幻觉，则回退到网络搜索。

![](img/442408c9b7f45e8c326836ae513300da_101.png)

这样，我们就将一个复杂的、包含路由、回退和检查机制的 RAG 流程，编码成了一个稳定、可重复执行的控制图。由于流程是预设的，即使使用 LLaMA3-8B 这样的本地模型，也能可靠地运行。

![](img/442408c9b7f45e8c326836ae513300da_102.png)

---

## 总结 📝

![](img/442408c9b7f45e8c326836ae513300da_104.png)

![](img/442408c9b7f45e8c326836ae513300da_105.png)

本节课中，我们一起学习了如何利用 LLaMA3 构建完全本地的可靠 RAG 智能体。

![](img/442408c9b7f45e8c326836ae513300da_106.png)

![](img/442408c9b7f45e8c326836ae513300da_107.png)

![](img/442408c9b7f45e8c326836ae513300da_108.png)

我们首先对比了 **ReAct** 和 **LangGraph** 两种构建智能体的范式，指出了在追求可靠性时，使用 LangGraph 预设控制流的优势。接着，我们逐步实践了构建本地 RAG 的核心组件：建立索引、创建评估器、生成答案以及集成搜索工具。最后，我们使用 LangGraph 将这些组件组装成一个具备复杂逻辑（路由、评估、回退）的智能体工作流。

![](img/442408c9b7f45e8c326836ae513300da_110.png)

![](img/442408c9b7f45e8c326836ae513300da_111.png)

这种方法的核心在于，通过工程师精心设计的控制流程来弥补小型本地语言模型在自主规划和决策上的不确定性，从而在有限的硬件资源上实现稳定、实用的智能体应用。