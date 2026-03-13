# 015：构建完整功能的聊天机器人 🤖

![](img/50b76bf8c477b208a834e72d4141c649_0.png)

在本节课中，我们将学习如何构建一个功能完整的聊天机器人。我们将整合之前学到的知识——加载文档、分割文本、创建向量存储和检索信息——并加入“聊天历史”的概念，使机器人能够处理对话中的后续问题。

---

## 环境与数据准备

![](img/50b76bf8c477b208a834e72d4141c649_2.png)

首先，我们需要设置环境并加载数据。

![](img/50b76bf8c477b208a834e72d4141c649_4.png)

我们将加载环境变量，并启动LangSmith平台以便观察内部运行过程。

![](img/50b76bf8c477b208a834e72d4141c649_6.png)

```python
# 加载环境变量
from dotenv import load_dotenv
load_dotenv()

![](img/50b76bf8c477b208a834e72d4141c649_8.png)

# 可选：打开LangSmith平台以观察内部过程
import os
os.environ["LANGCHAIN_TRACING_V2"] = "true"
```

![](img/50b76bf8c477b208a834e72d4141c649_10.png)

接下来，加载我们之前创建的向量存储，其中包含了所有课程材料的嵌入。

![](img/50b76bf8c477b208a834e72d4141c649_12.png)

```python
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings

![](img/50b76bf8c477b208a834e72d4141c649_14.png)

![](img/50b76bf8c477b208a834e72d4141c649_16.png)

# 加载向量存储
embeddings = OpenAIEmbeddings()
vectorstore = Chroma(persist_directory="./chroma_db", embedding_function=embeddings)
```

![](img/50b76bf8c477b208a834e72d4141c649_18.png)

我们可以对这个向量存储进行基础的相似性搜索。

```python
# 进行相似性搜索示例
docs = vectorstore.similarity_search("什么是机器学习？", k=3)
```

然后，初始化我们将要使用的语言模型。

![](img/50b76bf8c477b208a834e72d4141c649_20.png)

```python
from langchain.chat_models import ChatOpenAI

# 初始化语言模型
llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)
```

以上步骤与之前课程的内容一致。现在，让我们进入核心部分：为聊天机器人添加记忆能力。

![](img/50b76bf8c477b208a834e72d4141c649_22.png)

---

![](img/50b76bf8c477b208a834e72d4141c649_24.png)

## 添加记忆与对话能力

![](img/50b76bf8c477b208a834e72d4141c649_26.png)

上一节我们介绍了基础的检索问答链。本节中，我们来看看如何通过引入“记忆”来让机器人理解对话上下文。

![](img/50b76bf8c477b208a834e72d4141c649_28.png)

我们将使用“会话缓冲内存”。它的作用是保存一个聊天消息的历史记录列表，并在每次提问时，将这段历史与当前问题一起传递给模型。

![](img/50b76bf8c477b208a834e72d4141c649_30.png)

以下是初始化内存的代码：

![](img/50b76bf8c477b208a834e72d4141c649_32.png)

```python
from langchain.memory import ConversationBufferMemory

![](img/50b76bf8c477b208a834e72d4141c649_34.png)

# 初始化会话缓冲内存
memory = ConversationBufferMemory(
    memory_key="chat_history",  # 与提示模板中的输入变量对齐
    return_messages=True        # 以消息列表形式返回历史，而非单个字符串
)
```

![](img/50b76bf8c477b208a834e72d4141c649_36.png)

这是最简单的一种内存类型。关于内存更深入的内容，可以参考本系列的第一门课程。

![](img/50b76bf8c477b208a834e72d4141c649_38.png)

---

![](img/50b76bf8c477b208a834e72d4141c649_40.png)

## 创建会话检索链

现在，让我们创建一种新型的链——会话检索链。它在基础的检索问答链之上，增加了一个关键步骤：**将聊天历史和新问题浓缩成一个独立的、可检索的问题**。

![](img/50b76bf8c477b208a834e72d4141c649_42.png)

以下是创建该链的代码：

![](img/50b76bf8c477b208a834e72d4141c649_44.png)

```python
from langchain.chains import ConversationalRetrievalChain

![](img/50b76bf8c477b208a834e72d4141c649_46.png)

# 创建会话检索链
qa_chain = ConversationalRetrievalChain.from_llm(
    llm=llm,
    retriever=vectorstore.as_retriever(),
    memory=memory
)
```

![](img/50b76bf8c477b208a834e72d4141c649_48.png)

![](img/50b76bf8c477b208a834e72d4141c649_50.png)

这个链的工作流程是：
1.  接收用户的新问题和聊天历史。
2.  使用一个语言模型，将二者重写为一个独立的、完整的问题。
3.  将这个独立的问题发送到向量存储中进行检索。
4.  利用检索到的文档和原始问题，生成最终答案。

---

![](img/50b76bf8c477b208a834e72d4141c649_52.png)

## 测试对话功能

![](img/50b76bf8c477b208a834e72d4141c649_54.png)

让我们来测试一下这个具有对话能力的机器人。

![](img/50b76bf8c477b208a834e72d4141c649_56.png)

首先，问一个没有历史背景的问题：

```python
# 第一个问题
question = "这门课的主题是什么？"
result = qa_chain({"question": question})
print(result["answer"])
```

![](img/50b76bf8c477b208a834e72d4141c649_58.png)

![](img/50b76bf8c477b208a834e72d4141c649_60.png)

假设得到的答案是：“教师假设学生对概率和统计学有基本的了解”。

现在，问一个后续问题：

![](img/50b76bf8c477b208a834e72d4141c649_62.png)

```python
# 后续问题
follow_up = "为什么需要这些先决条件？"
result = qa_chain({"question": follow_up})
print(result["answer"])
```

![](img/50b76bf8c477b208a834e72d4141c649_64.png)

此时，机器人能够理解“这些先决条件”指代的是上一个答案中提到的“概率和统计学”，并给出相关的解释。

![](img/50b76bf8c477b208a834e72d4141c649_66.png)

---

![](img/50b76bf8c477b208a834e72d4141c649_68.png)

## 内部机制解析

![](img/50b76bf8c477b208a834e72d4141c649_70.png)

我们可以通过LangSmith观察内部发生的过程。链的输入现在不仅包含问题，还包含了从内存中获取的聊天历史。

![](img/50b76bf8c477b208a834e72d4141c649_72.png)

在追踪日志中，你会看到两个主要步骤：
1.  **生成独立问题**：调用一个LLM，根据对话历史将后续问题重写为独立问题。提示模板类似于：
    > 给定以下对话和后续问题，请将后续问题重写为一个独立的问题。
2.  **检索并回答**：将独立问题送入检索器，获取相关文档，最后利用这些文档回答用户的原始问题。

![](img/50b76bf8c477b208a834e72d4141c649_74.png)

![](img/50b76bf8c477b208a834e72d4141c649_76.png)

你可以尝试调整这个链的各个部分，例如：
*   修改用于回答问题或重写问题的提示模板。
*   尝试其他类型的内存（如 `ConversationSummaryMemory`）。

![](img/50b76bf8c477b208a834e72d4141c649_78.png)

---

![](img/50b76bf8c477b208a834e72d4141c649_80.png)

## 整合为完整应用

![](img/50b76bf8c477b208a834e72d4141c649_82.png)

最后，我们将所有组件整合到一个具有图形用户界面的完整应用中。虽然UI代码较多，但核心逻辑与我们上面构建的链一致。

![](img/50b76bf8c477b208a834e72d4141c649_84.png)

以下是核心流程的总结：

1.  **加载与处理文档**：使用PDF加载器读取文件，分割成块。
2.  **创建知识库**：为文本块生成嵌入，存入向量数据库。
3.  **设置检索器**：基于向量存储创建检索器，使用相似性搜索。
4.  **构建对话链**：创建 `ConversationalRetrievalChain`，但为了便于GUI管理，我们从外部传入聊天历史，而非绑定内部内存。
5.  **构建交互界面**：创建一个界面，用于输入问题、显示答案、查看检索到的源文档和完整的对话历史。

运行这个应用后，你将获得一个功能完整的问答聊天机器人。你可以：
*   上传自己的文档。
*   进行多轮对话。
*   点击查看机器人检索到的源文档块。
*   在配置区调整参数。

---

## 课程总结 🎉

在本节课中，我们一起学习了如何构建一个完整功能的聊天机器人。我们回顾并整合了整个课程的知识点：

1.  **数据加载**：使用LangChain丰富的文档加载器从各种源加载数据。
2.  **文本分割**：将文档分割成适合处理的文本块。
3.  **向量化与存储**：为文本块创建嵌入，并存入向量数据库以实现语义搜索。
4.  **高级检索**：探讨了多种检索算法来克服简单语义搜索的不足。
5.  **问答生成**：结合检索到的文档和大型语言模型来生成答案。
6.  **对话能力**：通过引入“聊天历史”和“会话检索链”，赋予机器人处理多轮对话的能力。

![](img/50b76bf8c477b208a834e72d4141c649_86.png)

至此，你已经掌握了使用LangChain基于自有数据构建端到端聊天机器人的完整流程。这是一个快速发展的领域，鼓励你继续探索、分享所学，甚至为开源社区做出贡献。祝你构建愉快！