# 012：向量和嵌入 📚

![](img/351fb93d60a6130df964b235d2de5e90_0.png)

![](img/351fb93d60a6130df964b235d2de5e90_2.png)

在本节课中，我们将学习如何将文本数据转化为数字表示（即嵌入），并将其存储在向量数据库中，以便后续高效地检索与问题相关的信息。这是构建基于文档的问答系统的核心步骤。

![](img/351fb93d60a6130df964b235d2de5e90_4.png)

---

![](img/351fb93d60a6130df964b235d2de5e90_6.png)

![](img/351fb93d60a6130df964b235d2de5e90_8.png)

## 从文本拆分到向量存储 🔄

![](img/351fb93d60a6130df964b235d2de5e90_10.png)

![](img/351fb93d60a6130df964b235d2de5e90_12.png)

上一节我们介绍了如何将文件分割成有意义的文本块。本节中，我们来看看如何将这些文本块转化为数字形式并建立索引。

![](img/351fb93d60a6130df964b235d2de5e90_14.png)

![](img/351fb93d60a6130df964b235d2de5e90_16.png)

嵌入技术能将文本转换为数字向量。语义相近的文本，其向量在数字空间中也更为接近。这使得我们可以通过比较向量来快速找到相似的文本片段。

![](img/351fb93d60a6130df964b235d2de5e90_18.png)

以下是构建索引的基本流程：
1.  从原始文件开始。
2.  将文档拆分为较小的、语义完整的块。
3.  为每个文本块创建嵌入向量。
4.  将所有向量存储到向量数据库中。

![](img/351fb93d60a6130df964b235d2de5e90_20.png)

![](img/351fb93d60a6130df964b235d2de5e90_22.png)

当用户提出问题时，系统会：
1.  为问题本身创建嵌入向量。
2.  在向量数据库中搜索与该问题向量最相似的文本块向量。
3.  将找到的最相关文本块与原始问题一同传递给大语言模型（LLM），最终生成答案。

![](img/351fb93d60a6130df964b235d2de5e90_24.png)

---

![](img/351fb93d60a6130df964b235d2de5e90_26.png)

![](img/351fb93d60a6130df964b235d2de5e90_28.png)

## 环境设置与数据准备 ⚙️

![](img/351fb93d60a6130df964b235d2de5e90_30.png)

![](img/351fb93d60a6130df964b235d2de5e90_32.png)

我们将使用一套课程讲义（CS229）作为示例数据。为了模拟现实世界中可能存在的“脏数据”，我们特意复制了第一讲的内容。

![](img/351fb93d60a6130df964b235d2de5e90_34.png)

首先，加载文档并使用递归字符文本分割器创建文本块。

![](img/351fb93d60a6130df964b235d2de5e90_36.png)

```python
# 示例：加载文档并分割
from langchain.text_splitter import RecursiveCharacterTextSplitter

![](img/351fb93d60a6130df964b235d2de5e90_38.png)

![](img/351fb93d60a6130df964b235d2de5e90_40.png)

# ... 加载 documents ...
text_splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
splits = text_splitter.split_documents(documents)
print(f"创建了 {len(splits)} 个文本块。")
```

![](img/351fb93d60a6130df964b235d2de5e90_42.png)

![](img/351fb93d60a6130df964b235d2de5e90_44.png)

完成上述步骤后，我们得到了200多个不同的文本块。接下来，进入核心环节：为这些块创建嵌入。

![](img/351fb93d60a6130df964b235d2de5e90_46.png)

---

![](img/351fb93d60a6130df964b235d2de5e90_48.png)

![](img/351fb93d60a6130df964b235d2de5e90_50.png)

## 理解嵌入：概念与示例 🧠

![](img/351fb93d60a6130df964b235d2de5e90_52.png)

![](img/351fb93d60a6130df964b235d2de5e90_54.png)

在进入实际应用前，我们先通过几个简单的例子来理解嵌入的工作原理。

![](img/351fb93d60a6130df964b235d2de5e90_56.png)

![](img/351fb93d60a6130df964b235d2de5e90_58.png)

我们准备三个句子：
- 句子1: “我爱我的宠物狗。”
- 句子2: “我有一只可爱的宠物狗。”
- 句子3: “今天天气晴朗，我开车去上班。”

![](img/351fb93d60a6130df964b235d2de5e90_60.png)

![](img/351fb93d60a6130df964b235d2de5e90_62.png)

前两个句子语义相似（都关于宠物狗），第三个句子则无关。我们将使用OpenAI的嵌入模型为每个句子生成向量。

![](img/351fb93d60a6130df964b235d2de5e90_64.png)

```python
# 示例：创建和比较嵌入
from langchain.embeddings import OpenAIEmbeddings
import numpy as np

![](img/351fb93d60a6130df964b235d2de5e90_66.png)

![](img/351fb93d60a6130df964b235d2de5e90_68.png)

embeddings = OpenAIEmbeddings()
sentences = [
    “我爱我的宠物狗。”,
    “我有一只可爱的宠物狗。”,
    “今天天气晴朗，我开车去上班。”
]
vecs = [embeddings.embed_query(s) for s in sentences]

![](img/351fb93d60a6130df964b235d2de5e90_70.png)

![](img/351fb93d60a6130df964b235d2de5e90_72.png)

# 使用点积计算相似度
def dot_product(vec_a, vec_b):
    return np.dot(vec_a, vec_b)

![](img/351fb93d60a6130df964b235d2de5e90_74.png)

# 比较相似度
print(f"句子1与句子2的相似度: {dot_product(vecs[0], vecs[1]):.2f}")
print(f"句子1与句子3的相似度: {dot_product(vecs[0], vecs[2]):.2f}")
```

![](img/351fb93d60a6130df964b235d2de5e90_76.png)

![](img/351fb93d60a6130df964b235d2de5e90_78.png)

点积值越高，表示两个向量的相似度越高。运行后，我们会发现句子1和句子2的点积值（例如0.96）远高于句子1和句子3的点积值（例如0.70），这与我们的语义判断一致。

![](img/351fb93d60a6130df964b235d2de5e90_80.png)

---

![](img/351fb93d60a6130df964b235d2de5e90_82.png)

![](img/351fb93d60a6130df964b235d2de5e90_84.png)

## 构建向量数据库 💾

![](img/351fb93d60a6130df964b235d2de5e90_86.png)

![](img/351fb93d60a6130df964b235d2de5e90_88.png)

现在，让我们回到实际案例，为所有PDF文本块创建嵌入并存入向量数据库。本节课我们使用轻量级的Chroma数据库。

![](img/351fb93d60a6130df964b235d2de5e90_90.png)

以下是创建并持久化向量数据库的步骤：

![](img/351fb93d60a6130df964b235d2de5e90_92.png)

```python
# 示例：创建并保存向量数据库
from langchain.vectorstores import Chroma

![](img/351fb93d60a6130df964b235d2de5e90_94.png)

![](img/351fb93d60a6130df964b235d2de5e90_95.png)

persist_directory = ‘./docs/chroma/‘
# 确保目录为空
import shutil
shutil.rmtree(persist_directory, ignore_errors=True)

![](img/351fb93d60a6130df964b235d2de5e90_97.png)

![](img/351fb93d60a6130df964b235d2de5e90_99.png)

# 创建向量存储
vectordb = Chroma.from_documents(
    documents=splits,
    embedding=OpenAIEmbeddings(),
    persist_directory=persist_directory
)
print(f"向量库中的文档数量: {vectordb._collection.count()}")
```

![](img/351fb93d60a6130df964b235d2de5e90_101.png)

创建完成后，我们可以立即使用它进行语义搜索。

![](img/351fb93d60a6130df964b235d2de5e90_103.png)

![](img/351fb93d60a6130df964b235d2de5e90_105.png)

---

![](img/351fb93d60a6130df964b235d2de5e90_107.png)

## 进行语义搜索 🔍

![](img/351fb93d60a6130df964b235d2de5e90_109.png)

假设我们的课程资料中提到了寻求帮助的邮箱，我们可以通过提问来检索该信息。

![](img/351fb93d60a6130df964b235d2de5e90_111.png)

```python
# 示例：在向量库中进行相似性搜索
question = “如果有问题，可以通过什么邮箱寻求帮助？”
docs = vectordb.similarity_search(question, k=3)
print(f"检索到 {len(docs)} 个相关文档。")
print(“最相关文档的内容:”, docs[0].page_content)
```

![](img/351fb93d60a6130df964b235d2de5e90_113.png)

![](img/351fb93d60a6130df964b235d2de5e90_115.png)

运行后，系统应能返回包含帮助邮箱（如 `cs229-qa@cs.stanford.edu`）的文本块。最后，别忘了持久化保存数据库以供后续使用：`vectordb.persist()`。

---

![](img/351fb93d60a6130df964b235d2de5e90_117.png)

## 面临的挑战与边缘情况 ⚠️

![](img/351fb93d60a6130df964b235d2de5e90_119.png)

![](img/351fb93d60a6130df964b235d2de5e90_121.png)

基于嵌入的语义搜索虽然强大，但并不完美。以下是两种常见的失效情况：

![](img/351fb93d60a6130df964b235d2de5e90_123.png)

![](img/351fb93d60a6130df964b235d2de5e90_125.png)

**1. 重复信息导致冗余**
由于我们加载数据时故意复制了第一讲，导致完全相同的文本块出现在数据库中。当进行搜索时，这些重复块可能被同时检索出来，挤占了其他有价值信息的位置，降低了检索结果的多样性。

![](img/351fb93d60a6130df964b235d2de5e90_127.png)

**2. 结构化信息检索失败**
考虑这个问题：“他们在第三堂课上对回归说了什么？” 我们期望的答案应仅来自第三讲的资料。然而，语义搜索可能返回所有提及“回归”的文本块，包括来自第一讲或第二讲的内容。这是因为嵌入模型更关注“回归”这个主题词，而未能完美捕捉“第三堂课”这个结构化的过滤条件。

![](img/351fb93d60a6130df964b235d2de5e90_129.png)

![](img/351fb93d60a6130df964b235d2de5e90_131.png)

你可以尝试调整返回文档的数量（`k`值），观察其对结果相关性和多样性的影响。

![](img/351fb93d60a6130df964b235d2de5e90_133.png)

![](img/351fb93d60a6130df964b235d2de5e90_135.png)

---

![](img/351fb93d60a6130df964b235d2de5e90_137.png)

## 课程总结 📝

本节课中，我们一起学习了：
1.  **嵌入的概念**：将文本转化为数字向量，使语义相似的文本在向量空间中靠近。
2.  **向量数据库的构建**：使用Chroma存储文本块的嵌入，以便快速进行相似性检索。
3.  **语义搜索实践**：通过提问，从向量库中检索出最相关的文档片段。
4.  **当前方法的局限性**：认识到基于纯语义相似度的检索可能面临信息冗余和结构化查询失效等挑战。

在下一节课中，我们将探讨如何改进检索策略，例如同时考虑相关性和多样性，以应对这些边缘情况。