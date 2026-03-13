# 023：5. 第四篇 - 自动合并检索(Auto-merging Retrieval) 📚

![](img/cad3624e9a8b61b04aa6255157bba696_1.png)

在本节课中，我们将深入研究另一个高级RAG技术：自动合并检索。我们将探讨标准RAG管道在处理碎片化上下文时遇到的问题，并学习如何通过构建层次化的文档块结构来合并相关片段，从而为语言模型提供更连贯的上下文信息。

## 概述：标准RAG的挑战与自动合并的解决方案

![](img/cad3624e9a8b61b04aa6255157bba696_3.png)

上一节我们介绍了句子窗口检索技术。本节中，我们来看看另一种提升检索质量的方法：自动合并检索。

![](img/cad3624e9a8b61b04aa6255157bba696_5.png)

标准RAG管道的一个核心问题是，它检索的是一系列碎片化的上下文块，以便放入语言模型的上下文窗口中。当使用的块尺寸越小时，这种碎片化问题就越严重。例如，你可能会检索到两个或多个覆盖文档中大致相同部分的块，但系统无法保证这些块的顺序，这可能会阻碍语言模型合成连贯答案的能力。

![](img/cad3624e9a8b61b04aa6255157bba696_7.png)

自动合并检索通过以下方式解决这个问题：
1.  首先，它建立一个由较小块（子块）和较大块（父块）组成的层次结构。
2.  在检索时，如果一个父块下的大部分子块都被检索出来（超过某个阈值），系统就会将这些子块合并为它们所属的更大的父块。
3.  最终，检索并传递给语言模型的是更大的、上下文更连贯的父块。

![](img/cad3624e9a8b61b04aa6255157bba696_9.png)

![](img/cad3624e9a8b61b04aa6255157bba696_11.png)

## 实践：设置自动合并检索

现在让我们看看如何具体设置自动合并检索。本部分将介绍构建自动合并检索器所需的各个组件，并最终展示如何通过实验和评估来优化其性能。

![](img/cad3624e9a8b61b04aa6255157bba696_13.png)

### 环境与数据准备

首先，我们需要加载必要的API密钥和数据。与之前的课程类似，我们将使用“如何构建人工智能职业生涯”的PDF文档作为示例。

![](img/cad3624e9a8b61b04aa6255157bba696_15.png)

```python
# 加载OpenAI API密钥
from utils import load_openai_key
load_openai_key()

# 加载文档
from llama_index import SimpleDirectoryReader
documents = SimpleDirectoryReader(input_dir="./data").load_data()
# 将多个文档对象合并为一个
full_document = "\n\n".join([doc.text for doc in documents])
```

![](img/cad3624e9a8b61b04aa6255157bba696_17.png)

### 构建层次化节点解析器

为了使用自动合并检索器，我们需要以层次化的方式解析文档节点。这意味着节点被按尺寸递减的顺序解析，并包含对其父节点的引用。

![](img/cad3624e9a8b61b04aa6255157bba696_19.png)

以下是定义层次节点解析器的步骤：

```python
from llama_index.node_parser import HierarchicalNodeParser

![](img/cad3624e9a8b61b04aa6255157bba696_21.png)

# 定义块大小序列（尺寸递减）
chunk_sizes = [2048, 512, 128]  # 父块 -> 中间块 -> 叶节点块
node_parser = HierarchicalNodeParser.from_defaults(chunk_sizes=chunk_sizes)

![](img/cad3624e9a8b61b04aa6255157bba696_23.png)

# 从文档中获取所有节点（包括叶节点、中间节点和父节点）
all_nodes = node_parser.get_nodes_from_documents([full_document])

# 如果只想查看最小的叶节点
leaf_nodes = node_parser.get_leaf_nodes(all_nodes)
# 查看一个叶节点的示例
print(leaf_nodes[30].text)  # 文本内容较短，块大小为128个标记
```

![](img/cad3624e9a8b61b04aa6255157bba696_25.png)

通过上述代码，我们建立了文档的层次结构。每个父节点（2048标记）包含多个子节点（512标记），而每个子节点又包含更小的叶节点（128标记）。

![](img/cad3624e9a8b61b04aa6255157bba696_27.png)

![](img/cad3624e9a8b61b04aa6255157bba696_29.png)

### 创建索引与存储

![](img/cad3624e9a8b61b04aa6255157bba696_31.png)

接下来，我们需要构建索引。关键点在于，我们只在叶节点上构建向量索引，而所有节点（包括父节点和中间节点）都存储在一个文档存储中。

```python
from llama_index import VectorStoreIndex, ServiceContext, StorageContext
from llama_index.llms import OpenAI
from llama_index.embeddings import OpenAIEmbedding

![](img/cad3624e9a8b61b04aa6255157bba696_33.png)

# 定义LLM和嵌入模型
llm = OpenAI(model="gpt-3.5-turbo")
embed_model = OpenAIEmbedding(model="text-embedding-ada-002")
service_context = ServiceContext.from_defaults(llm=llm, embed_model=embed_model, node_parser=node_parser)

![](img/cad3624e9a8b61b04aa6255157bba696_35.png)

# 创建存储上下文，用于存放所有节点
storage_context = StorageContext.from_defaults()
storage_context.docstore.add_documents(all_nodes)

![](img/cad3624e9a8b61b04aa6255157bba696_37.png)

# 创建向量索引，但只对叶节点进行嵌入和索引
auto_merge_index = VectorStoreIndex(
    nodes=leaf_nodes,  # 仅传入叶节点
    service_context=service_context,
    storage_context=storage_context  # 索引知道底层存储包含所有节点
)

# 持久化索引以便后续加载
auto_merge_index.storage_context.persist(persist_dir="./auto_merge_index")
```

### 配置检索器与查询引擎

最后一步是设置自动合并检索器和查询引擎。检索器负责在检索时动态地将相关的叶节点合并回其父节点。

![](img/cad3624e9a8b61b04aa6255157bba696_39.png)

```python
from llama_index.retrievers import AutoMergingRetriever
from llama_index.postprocessor import SentenceTransformerRerank
from llama_index.query_engine import RetrieverQueryEngine

![](img/cad3624e9a8b61b04aa6255157bba696_41.png)

![](img/cad3624e9a8b61b04aa6255157bba696_43.png)

# 创建自动合并检索器
base_retriever = auto_merge_index.as_retriever(similarity_top_k=12)  # 为叶节点设置较大的top_k
auto_merging_retriever = AutoMergingRetriever(
    base_retriever,
    auto_merge_index.storage_context,
    verbose=True
)

![](img/cad3624e9a8b61b04aa6255157bba696_45.png)

# 创建重排序器，对合并后的结果进行精炼
rerank = SentenceTransformerRerank(top_n=6)  # 将结果重新排序并限制为前6个

![](img/cad3624e9a8b61b04aa6255157bba696_47.png)

# 组合成最终的查询引擎
query_engine = RetrieverQueryEngine.from_args(
    auto_merging_retriever,
    node_postprocessors=[rerank]
)

![](img/cad3624e9a8b61b04aa6255157bba696_49.png)

# 进行查询测试
response = query_engine.query(“网络在AI职业生涯中有多重要？”)
print(response)
```

![](img/cad3624e9a8b61b04aa6255157bba696_51.png)

![](img/cad3624e9a8b61b04aa6255157bba696_53.png)

## 评估与参数迭代 🧪

![](img/cad3624e9a8b61b04aa6255157bba696_55.png)

![](img/cad3624e9a8b61b04aa6255157bba696_57.png)

![](img/cad3624e9a8b61b04aa6255157bba696_59.png)

我们已经设置好了自动合并检索器。现在，让我们看看如何使用RAG评估框架（如RAG Triad）来评估其性能，并与基础RAG进行对比实验。

![](img/cad3624e9a8b61b04aa6255157bba696_61.png)

### 设置评估实验

![](img/cad3624e9a8b61b04aa6255157bba696_63.png)

我们可以创建不同层次结构的索引（例如，两层 vs 三层）来比较性能。

![](img/cad3624e9a8b61b04aa6255157bba696_65.png)

![](img/cad3624e9a8b61b04aa6255157bba696_67.png)

```python
# 创建两层结构的索引（叶节点512标记，父节点2048标记）
chunk_sizes_2_layer = [2048, 512]
# 创建三层结构的索引（叶节点128标记，父节点512标记，祖父节点2048标记）
chunk_sizes_3_layer = [2048, 512, 128]

![](img/cad3624e9a8b61b04aa6255157bba696_69.png)

# 为每种结构构建索引和查询引擎（代码类似上文，此处省略）
# ...

![](img/cad3624e9a8b61b04aa6255157bba696_71.png)

![](img/cad3624e9a8b61b04aa6255157bba696_73.png)

# 使用TruLens进行评估
from trulens_eval import Tru, Feedback, Select
from trulens_eval.feedback import Groundedness
tru = Tru()
# 定义评估指标（如相关性、准确性、上下文相关性）
# ...
# 运行评估并记录结果
tru.run_dashboard()
```

![](img/cad3624e9a8b61b04aa6255157bba696_75.png)

### 分析评估结果

![](img/cad3624e9a8b61b04aa6255157bba696_77.png)

通过评估仪表板，我们可以：
*   在聚合级别比较不同应用（如`app_0`两层结构 vs `app_1`三层结构）的指标得分。
*   深入查看单个查询记录，了解模型在哪些问题上表现出色或失败。
*   分析不同块大小和层次结构对**上下文相关性**、**答案准确性**和**信息锚定性**的影响。

![](img/cad3624e9a8b61b04aa6255157bba696_79.png)

关键发现可能包括：
*   更深的层次结构（更小的叶节点）可能降低token使用量和成本。
*   合适的合并阈值能提高上下文的连贯性。
*   文档类型（如法律合同 vs 技术报告）可能最适合不同的分块策略。

## 总结

在本节课中，我们一起学习了自动合并检索这一高级RAG技术。

我们首先了解了标准RAG中上下文碎片化的问题。接着，我们逐步实践了如何设置层次化节点解析器、构建索引以及配置自动合并检索器和查询引擎。最后，我们探讨了如何使用评估框架来量化不同配置（如层次数量和块大小）的性能，并通过迭代实验找到最适合特定用例和文档类型的参数。

![](img/cad3624e9a8b61b04aa6255157bba696_81.png)

自动合并检索与句子窗口检索是互补的技术。自动合并擅长合并文本中连续且相关的片段，而句子窗口检索则专注于精确的上下文扩展。结合使用这些高级检索技术，并辅以系统的评估和实验跟踪，可以显著提升你的RAG应用质量。

![](img/cad3624e9a8b61b04aa6255157bba696_83.png)

---
**请注意**：本教程主要聚焦于自动合并检索技术及其评估。为了确保LLM应用的有用性、诚实性和无害性，你还可以探索其他评估维度，我们鼓励你深入研究相关评估框架和笔记本。