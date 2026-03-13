# 037：高级检索技术 🔍

![](img/1251636ef68b09989c270ce30b4b171d_0.png)

![](img/1251636ef68b09989c270ce30b4b171d_2.png)

在本节课中，我们将深入学习检索（Retrieval）技术。检索是RAG（检索增强生成）模型应用中的关键环节，它决定了我们能为大语言模型提供哪些相关信息。上一节我们介绍了基础的语义搜索，本节我们将探讨几种更先进的方法，以克服基础检索可能遇到的边缘情况，例如信息重复或缺乏多样性。

![](img/1251636ef68b09989c270ce30b4b171d_4.png)

![](img/1251636ef68b09989c270ce30b4b171d_6.png)

## 概述 📋

![](img/1251636ef68b09989c270ce30b4b171d_8.png)

![](img/1251636ef68b09989c270ce30b4b171d_10.png)

![](img/1251636ef68b09989c270ce30b4b171d_12.png)

本节课我们将覆盖三种高级检索技术：
1.  **最大边际相关性（MMR）**：用于确保检索结果的多样性。
2.  **自我查询（Self-query）**：用于处理同时包含语义内容和元数据过滤条件的问题。
3.  **上下文压缩（Contextual Compression）**：用于从检索到的文档中提取最相关的部分。

![](img/1251636ef68b09989c270ce30b4b171d_14.png)

![](img/1251636ef68b09989c270ce30b4b171d_16.png)

这些技术能显著提升检索质量，帮助我们构建更强大的RAG应用。

![](img/1251636ef68b09989c270ce30b4b171d_18.png)

![](img/1251636ef68b09989c270ce30b4b171d_20.png)

---

![](img/1251636ef68b09989c270ce30b4b171d_22.png)

![](img/1251636ef68b09989c270ce30b4b171d_24.png)

## 1. 最大边际相关性（MMR）🔄

![](img/1251636ef68b09989c270ce30b4b171d_26.png)

![](img/1251636ef68b09989c270ce30b4b171d_28.png)

在上一节中，我们讨论了语义相似性搜索。但有时，仅仅返回与查询最相似的文档可能会导致信息冗余或缺乏多样性。MMR技术就是为了解决这个问题。

![](img/1251636ef68b09989c270ce30b4b171d_30.png)

![](img/1251636ef68b09989c270ce30b4b171d_32.png)

MMR的核心思想是：在保证与查询相关性的同时，最大化返回文档集之间的差异性。其工作流程通常分为两步：
1.  首先，基于语义相似性获取一个较大的初始文档集（数量由 `fetch_k` 参数控制）。
2.  然后，从这个初始集中，根据相关性和多样性进行优化，最终返回指定数量（`k`）的文档。

![](img/1251636ef68b09989c270ce30b4b171d_34.png)

![](img/1251636ef68b09989c270ce30b4b171d_36.png)

**公式/概念描述**：MMR 在排序时不仅考虑文档与查询的相似度 `Sim(Q, Di)`，还考虑文档与已选文档集 `S` 的相似度，通过一个权衡参数 `λ` 来平衡相关性和多样性。
`MMR = argmax [ λ * Sim(Q, Di) - (1-λ) * max Sim(Di, Dj) ]`，其中 `Dj` 属于已选文档集 `S`。

![](img/1251636ef68b09989c270ce30b4b171d_38.png)

![](img/1251636ef68b09989c270ce30b4b171d_40.png)

以下是一个使用LangChain实现MMR的示例：

![](img/1251636ef68b09989c270ce30b4b171d_42.png)

![](img/1251636ef68b09989c270ce30b4b171d_44.png)

```python
# 假设 `vectordb` 是一个已初始化的向量数据库
# 基础相似性搜索（可能返回重复信息）
docs_similarity = vectordb.similarity_search(“关于MATLAB的内容”, k=2)

![](img/1251636ef68b09989c270ce30b4b171d_46.png)

![](img/1251636ef68b09989c270ce30b4b171d_48.png)

![](img/1251636ef68b09989c270ce30b4b171d_50.png)

# 使用MMR进行检索
docs_mmr = vectordb.max_marginal_relevance_search(“关于MATLAB的内容”, k=2, fetch_k=3)
```
通过设置 `fetch_k=3`，检索器会先获取3个最相似的文档，然后从中选出2个既相关又多样化的文档返回。

![](img/1251636ef68b09989c270ce30b4b171d_52.png)

![](img/1251636ef68b09989c270ce30b4b171d_54.png)

---

![](img/1251636ef68b09989c270ce30b4b171d_56.png)

![](img/1251636ef68b09989c270ce30b4b171d_58.png)

![](img/1251636ef68b09989c270ce30b4b171d_60.png)

## 2. 自我查询（Self-query）🤖

![](img/1251636ef68b09989c270ce30b4b171d_62.png)

![](img/1251636ef68b09989c270ce30b4b171d_64.png)

当用户的问题不仅包含需要查找的语义内容，还包含对元数据（如日期、作者、类别）的过滤条件时，自我查询检索器就非常有用。

![](img/1251636ef68b09989c270ce30b4b171d_66.png)

![](img/1251636ef68b09989c270ce30b4b171d_68.png)

它的工作原理是：利用一个大语言模型（LLM）自动将自然语言问题拆解为两部分：
*   **搜索词（Search Term）**：问题的语义核心。
*   **过滤器（Filter）**：对元数据字段的约束条件。

![](img/1251636ef68b09989c270ce30b4b171d_70.png)

![](img/1251636ef68b09989c270ce30b4b171d_72.png)

以下是如何在LangChain中配置和使用自我查询检索器：

![](img/1251636ef68b09989c270ce30b4b171d_74.png)

![](img/1251636ef68b09989c270ce30b4b171d_76.png)

```python
from langchain.llms import OpenAI
from langchain.retrievers.self_query.base import SelfQueryRetriever
from langchain.chains.query_constructor.base import AttributeInfo

![](img/1251636ef68b09989c270ce30b4b171d_78.png)

![](img/1251636ef68b09989c270ce30b4b171d_80.png)

# 1. 定义元数据字段信息
metadata_field_info = [
    AttributeInfo(
        name=“source”, # 元数据字段名
        description=“讲座笔记的来源，例如 `docs/MachineLearning-Lecture03.pdf`”， # 字段描述
        type=“string”， # 字段类型
    ),
    AttributeInfo(
        name=“page”，
        description=“文档所在的页码”，
        type=“integer”，
    ),
]
document_content_description = “机器学习讲座的笔记” # 对整个文档内容的描述

![](img/1251636ef68b09989c270ce30b4b171d_82.png)

![](img/1251636ef68b09989c270ce30b4b171d_84.png)

![](img/1251636ef68b09989c270ce30b4b171d_86.png)

# 2. 初始化LLM和检索器
llm = OpenAI(temperature=0)
retriever = SelfQueryRetriever.from_llm(
    llm,
    vectordb, # 基础向量数据库
    document_content_description,
    metadata_field_info,
    verbose=True # 设为True以查看LLM的推理过程
)

![](img/1251636ef68b09989c270ce30b4b171d_88.png)

![](img/1251636ef68b09989c270ce30b4b171d_90.png)

# 3. 进行查询
docs = retriever.get_relevant_documents(“在第三讲中他们说了什么关于回归的内容？”)
```
执行上述查询时，LLM会推断出搜索词应为“回归”，同时生成一个过滤器：`source == “docs/MachineLearning-Lecture03.pdf”`。这样，返回的结果将同时满足语义相关性和元数据条件。

![](img/1251636ef68b09989c270ce30b4b171d_92.png)

![](img/1251636ef68b09989c270ce30b4b171d_94.png)

---

![](img/1251636ef68b09989c270ce30b4b171d_96.png)

![](img/1251636ef68b09989c270ce30b4b171d_98.png)

## 3. 上下文压缩（Contextual Compression）✂️

![](img/1251636ef68b09989c270ce30b4b171d_100.png)

![](img/1251636ef68b09989c270ce30b4b171d_102.png)

![](img/1251636ef68b09989c270ce30b4b171d_104.png)

有时，检索到的整个文档可能只有一小部分与问题真正相关。上下文压缩技术可以在将文档传递给最终的大语言模型生成答案前，先提取出每个文档中最相关的片段。

![](img/1251636ef68b09989c270ce30b4b171d_106.png)

![](img/1251636ef68b09989c270ce30b4b171d_108.png)

这种方法以额外调用一次LLM为代价，来换取最终答案更高的聚焦度和准确性。

![](img/1251636ef68b09989c270ce30b4b171d_110.png)

![](img/1251636ef68b09989c270ce30b4b171d_112.png)

以下是实现步骤：

![](img/1251636ef68b09989c270ce30b4b171d_114.png)

![](img/1251636ef68b09989c270ce30b4b171d_116.png)

```python
from langchain.retrievers import ContextualCompressionRetriever
from langchain.retrievers.document_compressors import LLMChainExtractor
from langchain.llms import OpenAI

![](img/1251636ef68b09989c270ce30b4b171d_118.png)

![](img/1251636ef68b09989c270ce30b4b171d_120.png)

# 1. 初始化一个用于提取的LLM和压缩器
llm = OpenAI(temperature=0)
compressor = LLMChainExtractor.from_llm(llm)

![](img/1251636ef68b09989c270ce30b4b171d_122.png)

![](img/1251636ef68b09989c270ce30b4b171d_124.png)

# 2. 创建上下文压缩检索器，它包装了基础的向量数据库检索器
base_retriever = vectordb.as_retriever(search_type=“mmr”) # 可以结合MMR使用
compression_retriever = ContextualCompressionRetriever(
    base_compressor=compressor,
    base_retriever=base_retriever
)

![](img/1251636ef68b09989c270ce30b4b171d_126.png)

![](img/1251636ef68b09989c270ce30b4b171d_128.png)

# 3. 进行查询
compressed_docs = compression_retriever.get_relevant_documents(“他们对MATLAB说了什么？”)
```
返回的 `compressed_docs` 将不再是完整文档，而是经过LLM提炼后的、只包含相关信息的文本片段。

![](img/1251636ef68b09989c270ce30b4b171d_130.png)

![](img/1251636ef68b09989c270ce30b4b171d_132.png)

---

![](img/1251636ef68b09989c270ce30b4b171d_134.png)

![](img/1251636ef68b09989c270ce30b4b171d_136.png)

![](img/1251636ef68b09989c270ce30b4b171d_138.png)

## 其他检索方法补充 💡

![](img/1251636ef68b09989c270ce30b4b171d_140.png)

![](img/1251636ef68b09989c270ce30b4b171d_142.png)

值得注意的是，除了基于向量数据库的语义检索，还存在其他基于传统NLP技术的检索方法，例如：
*   **SVM检索器**：基于支持向量机算法。
*   **TF-IDF检索器**：基于词频-逆文档频率算法。

这些方法可以作为语义检索的补充或替代，特别是在特定领域或数据集上可能表现良好。LangChain也提供了这些检索器的接口，方便我们进行实验和比较。

![](img/1251636ef68b09989c270ce30b4b171d_144.png)

![](img/1251636ef68b09989c270ce30b4b171d_146.png)

```python
from langchain.retrievers import SVMRetriever, TFIDFRetriever

![](img/1251636ef68b09989c270ce30b4b171d_148.png)

![](img/1251636ef68b09989c270ce30b4b171d_150.png)

# 创建SVM检索器（需要传入文本嵌入模型）
svm_retriever = SVMRetriever.from_texts(texts, embeddings)
# 创建TF-IDF检索器
tfidf_retriever = TFIDFRetriever.from_texts(texts)
```

![](img/1251636ef68b09989c270ce30b4b171d_152.png)

![](img/1251636ef68b09989c270ce30b4b171d_154.png)

![](img/1251636ef68b09989c270ce30b4b171d_156.png)

---

![](img/1251636ef68b09989c270ce30b4b171d_158.png)

![](img/1251636ef68b09989c270ce30b4b171d_160.png)

## 总结 🎯

本节课我们一起深入学习了三种高级检索技术：
1.  **MMR**：通过权衡相关性与多样性，避免返回重复或单一的信息。
2.  **自我查询**：利用LLM解析复杂查询，实现对元数据的自动过滤，使检索更加精准。
3.  **上下文压缩**：在检索后对文档内容进行提炼，只保留最相关的部分，提升后续生成步骤的效率和质量。

这些技术可以单独使用，也可以组合使用（例如，使用MMR作为基础检索器，再对其进行上下文压缩），以构建更加强大和灵活的RAG系统。建议你尝试在不同的数据集和问题上应用这些技术，观察它们的效果，特别是自我查询检索器，对于处理复杂结构化查询非常有效。