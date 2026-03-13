# 039：7——完整功能的聊天机器人 🚀

![](img/92ccfc59e7465ca7b35eec7bf4601f06_0.png)

在本节课中，我们将学习如何构建一个功能完整的聊天机器人。我们将整合之前学过的所有组件——文档加载、分割、向量存储和检索——并引入“聊天历史”的概念，使机器人能够处理对话中的后续问题，实现真正的交互式对话。

---

## 加载环境与数据 📂

![](img/92ccfc59e7465ca7b35eec7bf4601f06_2.png)

首先，我们需要设置环境并加载数据。这与之前的步骤类似，是构建聊天机器人的基础。

我们将加载环境变量。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_4.png)

```python
# 示例代码：加载环境变量
import os
from dotenv import load_dotenv
load_dotenv()
```

![](img/92ccfc59e7465ca7b35eec7bf4601f06_6.png)

如果平台已经设置好，可以一并打开，以便观察内部运行情况。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_8.png)

接着，我们加载包含所有课程材料嵌入向量的向量存储。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_10.png)

```python
# 示例代码：加载向量存储
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings

![](img/92ccfc59e7465ca7b35eec7bf4601f06_12.png)

embeddings = OpenAIEmbeddings()
vectorstore = Chroma(persist_directory="./chroma_db", embedding_function=embeddings)
```

![](img/92ccfc59e7465ca7b35eec7bf4601f06_14.png)

我们可以在向量存储上执行基本的相似性搜索来验证数据。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_16.png)

```python
# 示例代码：执行相似性搜索
docs = vectorstore.similarity_search("概率")
print(docs[0].page_content)
```

---

## 初始化模型与基础链 🤖

![](img/92ccfc59e7465ca7b35eec7bf4601f06_18.png)

上一节我们准备好了数据，本节中我们来看看如何初始化语言模型并创建基础的问答链。

我们初始化将用作聊天机器人的语言模型。

```python
# 示例代码：初始化语言模型
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(temperature=0, model_name="gpt-3.5-turbo")
```

![](img/92ccfc59e7465ca7b35eec7bf4601f06_20.png)

然后，我们可以初始化提示模板，创建基础的检索问答链，并测试其回答问题的能力。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_22.png)

```python
# 示例代码：创建基础检索QA链
from langchain.chains import RetrievalQA
from langchain.prompts import PromptTemplate

prompt_template = """使用以下上下文来回答最后的问题。
{context}
问题：{question}
"""
PROMPT = PromptTemplate(template=prompt_template, input_variables=["context", "question"])
qa_chain = RetrievalQA.from_chain_type(llm=llm, chain_type="stuff", retriever=vectorstore.as_retriever(), chain_type_kwargs={"prompt": PROMPT})

![](img/92ccfc59e7465ca7b35eec7bf4601f06_24.png)

result = qa_chain.run("这门课的先修要求是什么？")
print(result)
```

![](img/92ccfc59e7465ca7b35eec7bf4601f06_26.png)

然而，这个链无法处理后续问题，因为它没有“记忆”对话历史。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_28.png)

---

![](img/92ccfc59e7465ca7b35eec7bf4601f06_30.png)

## 引入记忆与对话能力 💬

我们已经有了能回答单次问题的机器人，但真正的对话需要记忆。本节我们将为链添加记忆功能。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_32.png)

我们将使用 `ConversationBufferMemory`。它的作用是维护一个历史记录缓冲区，并将这些信息与每次的新问题一起传递给聊天机器人。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_34.png)

```python
# 示例代码：添加对话记忆
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory(memory_key="chat_history", return_messages=True)
```

![](img/92ccfc59e7465ca7b35eec7bf4601f06_36.png)

这里，`memory_key="chat_history"` 与提示模板中的输入变量对齐。`return_messages=True` 确保聊天历史以消息列表的形式返回，而不是单个字符串。这是最简单的一种记忆类型。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_38.png)

---

![](img/92ccfc59e7465ca7b35eec7bf4601f06_40.png)

## 创建对话检索链 🔄

![](img/92ccfc59e7465ca7b35eec7bf4601f06_42.png)

现在，让我们创建一个新的链类型——`ConversationRetrievalChain`。它在基础的检索问答链之上增加了一个关键步骤：将聊天历史和新问题合并成一个独立的、完整的问题，再传递给检索器查找相关文档。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_44.png)

```python
# 示例代码：创建对话检索链
from langchain.chains import ConversationalRetrievalChain

![](img/92ccfc59e7465ca7b35eec7bf4601f06_46.png)

qa = ConversationalRetrievalChain.from_llm(llm, vectorstore.as_retriever(), memory=memory)
```

![](img/92ccfc59e7465ca7b35eec7bf4601f06_48.png)

让我们测试一下它的对话能力。首先问一个初始问题。

```python
# 示例代码：测试对话能力
result = qa({"question": "这门课关于概率的主题有哪些？"})
print(result["answer"])
```

![](img/92ccfc59e7465ca7b35eec7bf4601f06_50.png)

然后，基于上一个答案问一个后续问题。

```python
# 示例代码：问后续问题
result = qa({"question": "为什么需要这些先修知识？"})
print(result["answer"])
```

![](img/92ccfc59e7465ca7b35eec7bf4601f06_52.png)

![](img/92ccfc59e7465ca7b35eec7bf4601f06_54.png)

现在，机器人能够理解“这些先修知识”指的是上一轮对话中提到的“概率与统计”，并给出连贯的回答。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_56.png)

---

## 理解链的内部运作 ⚙️

![](img/92ccfc59e7465ca7b35eec7bf4601f06_58.png)

为了更深入地理解，我们可以查看链的内部运行轨迹。这有助于我们调试和优化提示。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_60.png)

当我们运行对话链时，会发生两件事：
1.  **生成独立问题**：链首先调用一个子链，将聊天历史和新问题重新表述为一个独立的、无需上下文也能理解的问题。
2.  **检索并回答**：将这个独立问题传递给检索器获取相关文档，然后将文档和原始问题一起传递给语言模型生成最终答案。

在UI或日志中，我们可以看到对应的提示模板和中间结果。例如，重新表述问题的提示可能如下：

![](img/92ccfc59e7465ca7b35eec7bf4601f06_62.png)

![](img/92ccfc59e7465ca7b35eec7bf4601f06_64.png)

```
给定以下对话和一个后续问题，请将后续问题重新表述为一个独立的问题。

聊天历史：
人类：这门课关于概率的主题有哪些？
AI：教师假设学生对概率和统计有基本的理解...

![](img/92ccfc59e7465ca7b35eec7bf4601f06_66.png)

![](img/92ccfc59e7465ca7b35eec7bf4601f06_68.png)

后续问题：为什么需要这些先修知识？

![](img/92ccfc59e7465ca7b35eec7bf4601f06_70.png)

独立问题：
```

![](img/92ccfc59e7465ca7b35eec7bf4601f06_72.png)

这个“独立问题”会被用于检索，从而找到最相关的文档来回答原始的后续问题。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_74.png)

---

![](img/92ccfc59e7465ca7b35eec7bf4601f06_76.png)

## 构建图形用户界面 🎨

将所有功能整合到一个美观的GUI中，可以提供更佳的用户体验。虽然构建UI需要较多前端代码，但其核心逻辑与我们上面构建的链是一致的。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_78.png)

以下是构建GUI的核心步骤：

1.  **加载与处理文档**：使用PDF加载器读取文件，分割文档，创建嵌入并存入向量存储。
    ```python
    # 伪代码：文档处理流程
    loader = PyPDFLoader("materials.pdf")
    documents = loader.load()
    text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
    docs = text_splitter.split_documents(documents)
    vectorstore = Chroma.from_documents(docs, embeddings)
    retriever = vectorstore.as_retriever(search_kwargs={"k": 4})
    ```
2.  **创建对话链（外部管理记忆）**：为了方便GUI管理对话轮次，我们不在链内部绑定记忆，而是在调用链时显式传入聊天历史。
    ```python
    qa = ConversationalRetrievalChain.from_llm(llm, retriever)
    # 在GUI中，我们需要自己维护一个 chat_history 列表
    ```
3.  **设计交互逻辑**：在GUI中，用户输入问题后，程序将当前的 `chat_history` 和 `question` 一起传递给对话链，获得答案后，再将这一轮问答更新到 `chat_history` 中。

一个完整的GUI可能包含以下标签页：
*   **聊天界面**：主对话区域。
*   **数据库查询**：显示最近向向量数据库提出的问题及其检索到的源文档。
*   **配置**：允许用户上传新文件、调整检索参数（如 `k` 值）。

通过这个界面，用户可以轻松地进行多轮对话，例如：
*   用户：`TAs是谁？`
*   机器人：`TAs是Paul B.和Katie C.。`
*   用户：`他们的专业是什么？` （后续问题）
*   机器人：`Paul正在研究机器学习与计算机视觉，Katie是一名神经科学家。`

---

## 总结 📝

本节课中，我们一起学习了如何构建一个完整功能的聊天机器人。我们回顾了从文档加载、分割到创建向量存储的整个流程，并重点引入了**对话记忆**和**对话检索链**这两个关键概念，使机器人能够理解上下文并处理后续问题。

我们构建的端到端系统涵盖了以下核心步骤：
1.  **数据处理**：`加载文档 -> 分割文本 -> 创建向量存储`。
2.  **检索增强**：利用向量存储进行语义搜索，并可以集成更高级的检索算法（如自查询、压缩等）。
3.  **对话管理**：使用 `ConversationBufferMemory` 记录历史，通过 `ConversationalRetrievalChain` 将历史与新问题结合，实现连贯对话。
4.  **集成展示**：将所有功能封装，并可通过GUI进行交互。

![](img/92ccfc59e7465ca7b35eec7bf4601f06_80.png)

至此，我们已经完成了“使用LangChain与你的数据对话”系列的核心内容。你现在已经拥有一个可以基于自定义知识库进行智能对话的强大工具了。