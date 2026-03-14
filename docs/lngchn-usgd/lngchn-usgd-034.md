#  034：使用 Llama 3.1 构建完全本地的 RAG 智能体 🚀

在本节课中，我们将学习如何本地运行 Meta 最新发布的 Llama 3.1 模型，并利用它构建一个具备自我纠正能力的检索增强生成智能体。我们还将简要了解如何将其与其他模型进行性能对比。

![](img/b4a524dad6217cb8930d33cfb95458ad_1.png)

## 概述：Llama 3.1 模型发布

Meta 发布了备受期待的 Llama 3.1 模型。该系列包含多个版本：8B、70B 和 405B 参数模型，并支持 128K 的上下文长度。

![](img/b4a524dad6217cb8930d33cfb95458ad_1.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_3.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_4.png)

其中，405B 参数模型性能卓越，可与 GPT-4 Omni、Claude 3.5 Sonnet 等大型闭源模型相媲美。它将被托管在 Grok、Fireworks 等多个平台上。

![](img/b4a524dad6217cb8930d33cfb95458ad_5.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_3.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_4.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_5.png)

对于本地运行而言，8B 参数模型是一个“甜点”尺寸。例如，在一台配备 32GB 内存的 M2 MacBook Pro 上，它可以流畅运行，且性能优于之前的 Llama 3、Gemma 2 和 Mistral 7B Instruct 模型。

![](img/b4a524dad6217cb8930d33cfb4a524dad6217cb8930d33cfb95458ad_7.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_8.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_9.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_10.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_7.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_8.png)

## 第一步：环境设置与模型准备 🔧

![](img/b4a524dad6217cb8930d33cfb95458ad_9.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_10.png)

首先，我们需要设置运行环境并拉取 Llama 3.1 模型。

![](img/b4a524dad6217cb8930d33cfb95458ad_12.png)

### 拉取模型

使用 `ollama` 命令行工具拉取模型：
```bash
ollama pull llama3.1
```

![](img/b4a524dad6217cb8930d33cfb95458ad_14.png)

拉取完成后，你可以在 `ollama` 的模型库中看到不同尺寸的模型。8B 模型仅需约 4.7GB 空间，非常适合本地开发。

![](img/b4a524dad6217cb8930d33cfb95458ad_12.png)

### 安装必要的 Python 库

接下来，在 Jupyter Notebook 或 Python 环境中安装所需的库：
```python
# 安装核心库
!pip install tavily-python langgraph scikit-learn langchain-llama
```
以下是各库的用途：
*   `tavily-python`: 用于网络搜索的工具。
*   `langgraph`: 用于构建智能体工作流。
*   `scikit-learn`: 用于构建向量数据库。
*   `langchain-llama`: 用于接入 Llama 模型。

### 配置 API 密钥（可选）

根据你的需求，可能需要设置一些 API 密钥。这些对于 LangSmith 评估等功能是可选的。
```python
import os
# 例如：设置 OpenAI Embeddings 的 API 密钥（如果你使用它）
os.environ[“OPENAI_API_KEY”] = “your-openai-api-key”
# 设置 Tavily 搜索的 API 密钥
os.environ[“TAVILY_API_KEY”] = “your-tavily-api-key”
```

![](img/b4a524dad6217cb8930d33cfb95458ad_16.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_17.png)

## 第二步：构建自我纠正的 RAG 智能体 🤖

![](img/b4a524dad6217cb8930d33cfb95458ad_18.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_19.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_20.png)

我们将构建一个“自我纠正检索增强生成”智能体。其核心思想是：智能体能够评估检索到的文档相关性，如果文档不相关，则触发网络搜索来补充信息。这为智能体提供了灵活性，使其在问题超出向量数据库知识范围时，仍能通过网络搜索回答问题。

![](img/b4a524dad6217cb8930d33cfb95458ad_21.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_16.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_17.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_18.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_19.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_20.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_21.png)

### 2.1 创建向量数据库

![](img/b4a524dad6217cb8930d33cfb95458ad_23.png)

首先，我们需要一个包含知识文档的向量数据库作为检索源。

![](img/b4a524dad6217cb8930d33cfb95458ad_24.png)

我们将索引几篇来自 LangChain 的博客文章（主题包括智能体、提示工程、对抗性测试）。以下是创建向量数据库的步骤：

```python
from langchain_community.document_loaders import WebBaseLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.vectorstores import SKLearnVectorStore
from langchain_openai import OpenAIEmbeddings

![](img/b4a524dad6217cb8930d33cfb95458ad_26.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_27.png)

# 1. 加载文档
urls = [
    “https://blog.langchain.com/agents-overview/”,
    “https://blog.langchain.com/prompt-engineering-guide/”,
    “https://blog.langchain.com/adversarial-testing/”
]
loader = WebBaseLoader(urls)
docs = loader.load()

# 2. 分割文档
text_splitter = RecursiveCharacterTextSplitter(chunk_size=250, chunk_overlap=50)
splits = text_splitter.split_documents(docs)

# 3. 创建向量存储（使用 OpenAI Embeddings）
# 如需完全本地化，可考虑使用 Nomic Embeddings
vectorstore = SKLearnVectorStore.from_documents(
    documents=splits,
    embedding=OpenAIEmbeddings(),
)
retriever = vectorstore.as_retriever()
```

![](img/b4a524dad6217cb8930d33cfb95458ad_26.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_27.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_29.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_30.png)

创建完成后，可以测试检索器是否正常工作：
```python
# 测试检索器
question = “What is agent memory?”
retrieved_docs = retriever.invoke(question)
print(f”检索到 {len(retrieved_docs)} 个文档片段。”)
```

![](img/b4a524dad6217cb8930d33cfb95458ad_31.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_32.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_33.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_36.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_34.png)

### 2.2 配置工具与模型

![](img/b4a524dad6217cb8930d33cfb95458ad_36.png)

我们需要一个网络搜索工具和我们的核心模型——Llama 3.1。

**配置网络搜索工具：**
```python
from langchain_community.tools.tavily_search import TavilySearchResults
search_tool = TavilySearchResults(max_results=2)
```

![](img/b4a524dad6217cb8930d33cfb95458ad_38.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_39.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_38.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_39.png)

**配置 Llama 3.1 模型：**
确保已通过 `ollama pull llama3.1` 拉取模型。
```python
from langchain_llama import ChatLlama
# 初始化 Llama 3.1 聊天模型
llm = ChatLlama(model=“llama3.1”)
```

![](img/b4a524dad6217cb8930d33cfb95458ad_41.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_42.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_43.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_44.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_41.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_42.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_43.png)

### 2.3 构建 RAG 链与评估器

![](img/b4a524dad6217cb8930d33cfb95458ad_44.png)

**构建基础的 RAG 链：**
这是一个简单的问答链，基于检索到的文档生成答案。
```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

# 定义 RAG 提示模板
rag_prompt = ChatPromptTemplate.from_template(“””
你是一个用于问答任务的助手。
使用以下检索到的上下文来回答问题。如果你不知道答案，就说你不知道。
上下文：{context}
问题：{question}
答案：
“””)

![](img/b4a524dad6217cb8930d33cfb95458ad_46.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_47.png)

# 构建 RAG 链
rag_chain = rag_prompt | llm | StrOutputParser()
```

![](img/b4a524dad6217cb8930d33cfb95458ad_46.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_47.png)

**构建文档相关性评估器：**
这是实现“自我纠正”的关键组件。它将判断检索到的文档是否与问题相关。
```python
from langchain_core.pydantic_v1 import BaseModel, Field

# 定义评估器输出格式
class GradeDocuments(BaseModel):
    binary_score: str = Field(description=“文档是否相关，输出 ‘yes’ 或 ‘no'”)

# 定义评估提示模板
grader_prompt = ChatPromptTemplate.from_template(“””
你是一个评估员，负责评估检索到的文档与用户问题的相关性。
文档内容：{document}
用户问题：{question}
如果文档**包含**与问题相关的关键词或语义内容，则评估为相关。
你的评估不必非常严格，主要目的是过滤掉明显错误的检索结果。
请给出二进制评分 ‘yes’ 或 ‘no’，并以 JSON 格式返回。
“””)

# 构建评估链（使用 JSON 模式输出）
grader = grader_prompt | llm.with_structured_output(GradeDocuments)
```

![](img/b4a524dad6217cb8930d33cfb95458ad_49.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_50.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_51.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_49.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_50.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_51.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_52.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_53.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_54.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_55.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_52.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_53.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_54.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_55.png)

现在，我们拥有了智能体所需的所有核心组件：向量检索器、RAG 链、评估器和网络搜索工具。

![](img/b4a524dad6217cb8930d33cfb95458ad_57.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_57.png)

## 第三步：使用 LangGraph 编排智能体工作流 ⚙️

我们将使用 `LangGraph` 来定义智能体的控制流。`LangGraph` 允许我们以图的形式清晰地表达复杂的智能体推理步骤。

![](img/b4a524dad6217cb8930d33cfb95458ad_59.png)

首先，定义智能体运行过程中需要维护的状态：
```python
from typing import TypedDict, List, Annotated
import operator

![](img/b4a524dad6217cb8930d33cfb95458ad_60.png)

class GraphState(TypedDict):
    question: str # 用户问题
    generation: str # 最终生成的答案
    search_required: str # 是否需要网络搜索，’yes‘ 或 ’no‘
    documents: List[str] # 检索到或搜索到的文档
    steps: List[str] # 记录执行步骤，用于调试或追踪
```

![](img/b4a524dad6217cb8930d33cfb95458ad_59.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_60.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_62.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_63.png)

接下来，我们将智能体的每个步骤定义为一个“节点”函数。

**1. 检索节点：**
这个节点从向量数据库中检索初始文档。
```python
def retrieve(state: GraphState):
    print(“—–检索文档—–“)
    question = state[“question”]
    # 调用检索器
    retrieved_docs = retriever.invoke(question)
    # 更新状态
    return {
        “documents”: retrieved_docs,
        “steps”: state[“steps”] + [“检索文档”]
    }
```

![](img/b4a524dad6217cb8930d33cfb95458ad_65.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_66.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_62.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_63.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_65.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_66.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_67.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_67.png)

**2. 评估文档节点：**
这个节点评估每个检索到的文档的相关性。如果任何文档被评估为“不相关”，则触发网络搜索标志。
```python
def grade_documents(state: GraphState):
    print(“—–评估文档相关性—–“)
    question = state[“question”]
    documents = state[“documents”]
    search_required = “no” # 默认不搜索
    filtered_docs = []

    for doc in documents:
        # 对每个文档进行评估
        score = grader.invoke({“document”: doc.page_content, “question”: question})
        if score.binary_score == “yes”:
            # 文档相关，保留
            filtered_docs.append(doc)
        else:
            # 文档不相关，标记需要搜索
            search_required = “yes”
            print(f”发现不相关文档，将触发网络搜索。”)
    # 更新状态
    return {
        “documents”: filtered_docs,
        “search_required”: search_required,
        “steps”: state[“steps”] + [“评估文档”]
    }
```

![](img/b4a524dad6217cb8930d33cfb95458ad_69.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_71.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_77.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_78.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_79.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_80.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_81.png)

**3. 网络搜索节点：**
当评估节点判定需要搜索时，执行此节点。
```python
def web_search(state: GraphState):
    print(“—–执行网络搜索—–“)
    question = state[“question”]
    # 调用搜索工具
    search_results = search_tool.invoke({“query”: question})
    # 将搜索结果添加到文档列表中
    existing_docs = state.get(“documents”, [])
    all_docs = existing_docs + search_results
    return {
        “documents”: all_docs,
        “steps”: state[“steps”] + [“网络搜索”]
    }
```

![](img/b4a524dad6217cb8930d33cfb95458ad_73.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_74.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_75.png)

**4. 生成答案节点：**
使用所有可用的文档（检索到的或搜索到的）来生成最终答案。
```python
def generate(state: GraphState):
    print(“—–生成最终答案—–“)
    question = state[“question”]
    documents = state[“documents”]
    # 将所有文档内容合并为上下文
    context = “\n\n”.join([doc.page_content for doc in documents])
    # 调用 RAG 链生成答案
    generation = rag_chain.invoke({“context”: context, “question”: question})
    return {
        “generation”: generation,
        “steps”: state[“steps”] + [“生成答案”]
    }
```

![](img/b4a524dad6217cb8930d33cfb95458ad_73.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_74.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_75.png)

**5. 定义控制流图：**
最后，我们将所有节点连接起来，定义完整的工作流。
```python
from langgraph.graph import StateGraph, END

# 创建图
workflow = StateGraph(GraphState)

# 添加节点
workflow.add_node(“retrieve”, retrieve)
workflow.add_node(“grade_documents”, grade_documents)
workflow.add_node(“web_search”, web_search)
workflow.add_node(“generate”, generate)

# 设置入口点
workflow.set_entry_point(“retrieve”)

![](img/b4a524dad6217cb8930d33cfb95458ad_77.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_78.png)

# 添加边（定义执行路径）
workflow.add_edge(“retrieve”, “grade_documents”)
# 根据评估结果决定路径
workflow.add_conditional_edges(
    “grade_documents”,
    # 判断函数：检查是否需要搜索
    lambda state: state[“search_required”],
    {
        “yes”: “web_search”, # 需要搜索，则去搜索
        “no”: “generate” # 不需要搜索，则直接生成
    }
)
workflow.add_edge(“web_search”, “generate”) # 搜索完后生成
workflow.add_edge(“generate”, END) # 生成后结束

![](img/b4a524dad6217cb8930d33cfb95458ad_79.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_80.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_81.png)

# 编译图
app = workflow.compile()
```

![](img/b4a524dad6217cb8930d33cfb95458ad_83.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_84.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_85.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_83.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_84.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_85.png)

至此，我们的自我纠正 RAG 智能体就构建完成了。其工作流清晰对应了最初的设计图。

![](img/b4a524dad6217cb8930d33cfb95458ad_87.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_88.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_89.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_90.png)
![](img/b4a524dad6217cb8930d33cfb95458ad_91.png)

## 第四步：测试智能体 🧪

现在，让我们测试这个完全在本地运行的智能体。

我们可以创建一个简单的函数来调用它：
```python
def run_agent(question: str):
    # 初始化状态
    initial_state = {“question”: question, “steps”: []}
    # 执行图
    final_state = app.invoke(initial_state)
    # 输出结果
    print(“\n=== 最终答案 ===“)
    print(final_state[“generation”])
    print(“\n=== 执行步骤 ===")
    for step in final_state[“steps”]:
        print(f”- {step}”)
```

运行测试：
```python
# 测试一个向量数据库内有答案的问题
run_agent(“What is agent memory?”)

# 测试一个可能超出向量数据库知识范围，需要网络搜索的问题
run_agent(“What is the latest version of PyTorch?”)
```

![](img/b4a524dad6217cb8930d33cfb95458ad_87.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_93.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_88.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_89.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_90.png)

![](img/b4a524dad6217cb8930d33cfb95458ad_91.png)

## 总结与回顾 📚

在本节课中，我们一起学习了：

1.  **模型获取**：如何使用 `ollama` 工具本地拉取和运行最新的 Llama 3.1 8B 模型。
2.  **核心概念**：理解了“自我纠正 RAG”智能体的工作原理，即通过评估检索结果的相关性，动态决定是否借助外部工具（如网络搜索）来获取更准确的信息。
3.  **工具链搭建**：安装了构建智能体所需的库，并配置了向量数据库、检索器、评估器和网络搜索工具。
4.  **工作流编排**：使用 **`LangGraph`** 框架，以图的形式定义并实现了智能体的多步骤推理控制流，包括检索、评估、条件分支（搜索/生成）等关键节点。

![](img/b4a524dad6217cb8930d33cfb95458ad_93.png)

你成功构建了一个可以完全在本地环境（如配备 Apple Silicon 的笔记本电脑）中运行的、具备一定自我纠正能力的 RAG 智能体原型。这个项目是评估不同大语言模型在复杂任务中表现的一个优秀测试平台。你可以尝试更换不同的模型（如 Llama 3.1 70B，或其他本地模型），观察其性能变化。