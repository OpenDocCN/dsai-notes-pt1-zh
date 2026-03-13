# 005：基于文档的问答 📚

![](img/89a5977cf51ecc58c6386d1d630341b1_0.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_2.png)

在本节课中，我们将要学习如何构建一个基于文档的问答系统。这是利用大语言模型（LLM）构建的最常见、最强大的复杂应用之一。我们将学习如何将语言模型与您自己的文档（如PDF、网页或公司内部资料）结合起来，让模型能够回答关于这些文档内容的问题，从而帮助用户深入理解并获取所需信息。

## 环境准备与导入 🛠️

![](img/89a5977cf51ecc58c6386d1d630341b1_4.png)

上一节我们介绍了课程目标，本节中我们来看看如何准备开发环境。首先，我们需要导入必要的库和设置环境变量。

```python
import os
# 假设已设置 OPENAI_API_KEY 环境变量

![](img/89a5977cf51ecc58c6386d1d630341b1_6.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_8.png)

from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI
from langchain.document_loaders import CSVLoader
from langchain.vectorstores import DocArrayInMemorySearch
from IPython.display import display, Markdown
```

![](img/89a5977cf51ecc58c6386d1d630341b1_10.png)

以下是关键导入项的说明：
*   `RetrievalQA`: 用于构建检索式问答链的核心类。
*   `ChatOpenAI`: 我们将使用的聊天语言模型。
*   `CSVLoader`: 用于加载CSV格式的专有数据文档。
*   `DocArrayInMemorySearch`: 一个易于入门的**内存向量存储**，无需连接外部数据库。
*   `display` 和 `Markdown`: 在Jupyter Notebook中显示格式化结果的工具。

![](img/89a5977cf51ecc58c6386d1d630341b1_12.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_14.png)

## 快速开始：一行代码构建问答链 ⚡

![](img/89a5977cf51ecc58c6386d1d630341b1_16.png)

现在，让我们使用LangChain提供的高级接口快速构建一个可运行的问答系统。我们将加载一个关于户外服装的CSV文件作为我们的知识库。

![](img/89a5977cf51ecc58c6386d1d630341b1_18.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_20.png)

```python
file = 'OutdoorClothingCatalog_1000.csv'
loader = CSVLoader(file_path=file)

![](img/89a5977cf51ecc58c6386d1d630341b1_22.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_24.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_26.png)

from langchain.indexes import VectorstoreIndexCreator

![](img/89a5977cf51ecc58c6386d1d630341b1_28.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_30.png)

index = VectorstoreIndexCreator(
    vectorstore_cls=DocArrayInMemorySearch
).from_loaders([loader])
```

![](img/89a5977cf51ecc58c6386d1d630341b1_31.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_33.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_35.png)

代码解释：
1.  使用 `CSVLoader` 加载指定路径的CSV文件。
2.  使用 `VectorstoreIndexCreator` 并指定向量存储类为 `DocArrayInMemorySearch`。
3.  调用 `.from_loaders([loader])` 方法，传入文档加载器列表，自动完成文档加载、分块、创建嵌入和构建向量存储索引的过程。

![](img/89a5977cf51ecc58c6386d1d630341b1_37.png)

索引创建完成后，我们就可以直接进行提问了。

![](img/89a5977cf51ecc58c6386d1d630341b1_39.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_41.png)

```python
query = "Please list all your shirts with sun protection in a table. Summarize each one."
response = index.query(query)
display(Markdown(response))
```

![](img/89a5977cf51ecc58c6386d1d630341b1_43.png)

执行以上代码，语言模型会基于CSV文档中的信息，返回一个包含防晒衬衫名称和描述的Markdown表格，并附上总结。

![](img/89a5977cf51ecc58c6386d1d630341b1_45.png)

## 核心原理：嵌入与向量存储 🧠

![](img/89a5977cf51ecc58c6386d1d630341b1_47.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_49.png)

上一节我们快速体验了文档问答的效果，本节中我们来深入了解一下其背后的核心原理。直接让语言模型处理长文档存在一个关键问题：**语言模型的上下文窗口有限**，一次只能处理几千个单词。

![](img/89a5977cf51ecc58c6386d1d630341b1_51.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_53.png)

那么，如何让语言模型回答超长文档中的所有问题呢？这时，**嵌入（Embeddings）** 和 **向量存储（Vector Stores）** 就开始发挥作用了。

![](img/89a5977cf51ecc58c6386d1d630341b1_55.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_57.png)

### 理解嵌入

![](img/89a5977cf51ecc58c6386d1d630341b1_59.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_61.png)

**嵌入**是为文本片段创建数值表示（即向量）的过程。这种表示能够捕获文本的语义含义。

![](img/89a5977cf51ecc58c6386d1d630341b1_63.png)

```python
# 伪代码示意：文本 -> 向量
embedding_vector = embed("I love programming.")
```

![](img/89a5977cf51ecc58c6386d1d630341b1_65.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_67.png)

具有相似含义的文本，其向量在向量空间中的位置也会相近。例如，关于“宠物”的句子向量会彼此靠近，而与关于“汽车”的句子向量则相距较远。这使我们能够通过计算向量相似度来找出语义相近的文本。

![](img/89a5977cf51ecc58c6386d1d630341b1_69.png)

### 理解向量数据库

![](img/89a5977cf51ecc58c6386d1d630341b1_71.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_73.png)

**向量数据库**是专门用于存储和检索这些向量表示的数据系统。构建向量数据库的典型流程如下：
1.  **文档分块**：将长文档拆分成较小的文本块，以适应语言模型的上下文限制。
2.  **创建嵌入**：为每个文本块生成对应的向量。
3.  **存储向量**：将文本块及其向量存入向量数据库。

![](img/89a5977cf51ecc58c6386d1d630341b1_75.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_77.png)

当用户提出查询时，系统会：
1.  为查询文本生成嵌入向量。
2.  在向量数据库中搜索与查询向量最相似的K个文本块（即**相似性搜索**）。
3.  将这些最相关的文本块作为上下文，与原始问题一起构造提示（Prompt），发送给语言模型以生成最终答案。

![](img/89a5977cf51ecc58c6386d1d630341b1_79.png)

## 逐步实现：拆解问答链的构建 🛠️

![](img/89a5977cf51ecc58c6386d1d630341b1_81.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_83.png)

上一节我们了解了核心概念，本节我们将手动逐步实现一个问答链，以彻底理解每个环节。

![](img/89a5977cf51ecc58c6386d1d630341b1_85.png)

### 第一步：加载文档

![](img/89a5977cf51ecc58c6386d1d630341b1_87.png)

这与快速开始的第一步类似。

![](img/89a5977cf51ecc58c6386d1d630341b1_89.png)

```python
loader = CSVLoader(file_path=file)
docs = loader.load()
# 查看一个文档，对应CSV中的一行（一个产品）
print(docs[0].page_content)
```

![](img/89a5977cf51ecc58c6386d1d630341b1_91.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_93.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_95.png)

### 第二步：创建嵌入并构建向量存储

![](img/89a5977cf51ecc58c6386d1d630341b1_97.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_99.png)

由于我们的文档（每个产品描述）本身已经很小，可以跳过显式的分块步骤，直接创建嵌入。

![](img/89a5977cf51ecc58c6386d1d630341b1_101.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_103.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_105.png)

```python
from langchain.embeddings import OpenAIEmbeddings
embedding = OpenAIEmbeddings()

![](img/89a5977cf51ecc58c6386d1d630341b1_107.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_108.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_110.png)

# 为单个句子创建嵌入示例
sample_embedding = embedding.embed_query("Hi, my name is Harrison")
print(f"嵌入向量维度: {len(sample_embedding)}")

![](img/89a5977cf51ecc58c6386d1d630341b1_112.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_114.png)

# 为所有文档创建嵌入并构建向量存储
db = DocArrayInMemorySearch.from_documents(docs, embedding)
```

![](img/89a5977cf51ecc58c6386d1d630341b1_116.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_118.png)

### 第三步：执行相似性搜索

![](img/89a5977cf51ecc58c6386d1d630341b1_120.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_122.png)

现在我们可以使用向量存储来查找与用户查询最相关的文档。

![](img/89a5977cf51ecc58c6386d1d630341b1_124.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_126.png)

```python
query = "Please recommend a shirt with sunblocking."
docs_result = db.similarity_search(query)
print(docs_result[0].page_content)
```

![](img/89a5977cf51ecc58c6386d1d630341b1_128.png)

### 第四步：创建检索器并组装问答链

![](img/89a5977cf51ecc58c6386d1d630341b1_130.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_132.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_134.png)

检索器（Retriever）是一个通用接口，`db.as_retriever()` 会返回一个基于我们向量存储的检索器对象。

![](img/89a5977cf51ecc58c6386d1d630341b1_136.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_137.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_139.png)

```python
retriever = db.as_retriever()
llm = ChatOpenAI(temperature=0.0)

![](img/89a5977cf51ecc58c6386d1d630341b1_141.png)

# 手动模拟问答过程（仅示意）
# 1. 检索相关文档
relevant_docs = retriever.get_relevant_documents(query)
# 2. 合并文档内容到提示中
combined_context = "\n\n".join([doc.page_content for doc in relevant_docs])
# 3. 构造最终提示并调用LLM（实际链中自动完成）
```

![](img/89a5977cf51ecc58c6386d1d630341b1_143.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_145.png)

LangChain 的 `RetrievalQA` 链封装了以上所有步骤。

![](img/89a5977cf51ecc58c6386d1d630341b1_147.png)

```python
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff", # 使用最简单的“堆叠”方法
    retriever=retriever,
    verbose=True # 显示详细执行过程
)
response = qa_chain.run(query)
display(Markdown(response))
```

![](img/89a5977cf51ecc58c6386d1d630341b1_149.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_151.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_153.png)

`chain_type="stuff"` 表示将所有检索到的文档内容“堆叠”到一个提示中，然后调用一次语言模型。这种方法简单高效，适用于检索到的文档总长度不超过模型上下文限制的场景。

![](img/89a5977cf51ecc58c6386d1d630341b1_155.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_157.png)

## 进阶讨论：不同的链类型与总结 🚀

![](img/89a5977cf51ecc58c6386d1d630341b1_159.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_161.png)

我们使用了 `stuff` 方法，但它并非唯一选择。LangChain 提供了多种处理多文档的链类型，适用于不同场景：

![](img/89a5977cf51ecc58c6386d1d630341b1_163.png)

以下是几种主要的链类型：
*   **Stuff**: 将所有文档内容放入一个提示中。**优点**是简单、成本低、只调用一次LLM。**缺点**是受限于模型的上下文长度。
*   **MapReduce**: 首先为每个文档单独调用LLM生成答案（Map），然后调用另一个LLM来汇总所有独立答案（Reduce）。**优点**是可处理任意数量的文档，且Map步骤可并行。**缺点**是LLM调用次数多，成本较高，且文档间信息不交互。
*   **Refine**: 迭代处理文档，基于前一个文档的答案和当前文档来逐步完善最终答案。**优点**适合需要整合跨文档信息的复杂答案。**缺点**是速度慢，调用不独立，且答案可能冗长。
*   **MapRerank**: 为每个文档调用LLM，要求其返回一个答案及相关性分数，最后选择分数最高的答案。**优点**是速度快，可批量处理。**缺点**是严重依赖LLM评分能力，需要精心设计提示，且调用次数多。

![](img/89a5977cf51ecc58c6386d1d630341b1_165.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_167.png)

对于文档摘要等需要处理超长文本的任务，`MapReduce` 是一种非常常用的模式。

![](img/89a5977cf51ecc58c6386d1d630341b1_169.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_171.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_173.png)

## 课程总结 📝

![](img/89a5977cf51ecc58c6386d1d630341b1_175.png)

![](img/89a5977cf51ecc58c6386d1d630341b1_177.png)

本节课中我们一起学习了基于文档的问答系统的构建。
1.  **目标**：我们学会了如何将大语言模型与外部文档结合，构建能够回答特定领域问题的智能应用。
2.  **核心组件**：我们深入理解了**嵌入**和**向量存储**这两个关键技术，它们通过语义搜索解决了模型上下文有限的瓶颈。
3.  **实践方法**：我们掌握了两种构建方式：使用 `VectorstoreIndexCreator` 快速原型开发，以及手动分步实现以获得更精细的控制。
4.  **链类型**：我们了解了 `stuff`、`map_reduce`、`refine` 和 `map_rerank` 等不同的文档处理策略及其适用场景。

![](img/89a5977cf51ecc58c6386d1d630341b1_179.png)

通过本课，你已经掌握了构建强大文档问答应用的基础。在接下来的课程中，我们将探索LangChain中更多的链和组件。