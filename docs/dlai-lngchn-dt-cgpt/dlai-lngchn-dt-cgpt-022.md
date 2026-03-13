# 022：4. 第三篇 - 句子窗口检索 (Sentence Window Retrieval) 🧩

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_1.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_3.png)

在本节课中，我们将深入研究一种高级的RAG技术——句子窗口检索方法。我们将学习如何通过检索较小的句子片段来更好地匹配相关上下文，然后基于扩展的上下文窗口来合成答案。课程将涵盖从设置索引、构建查询引擎到使用TruLens进行性能评估的完整流程。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_5.png)

## 概述

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_7.png)

句子窗口检索方法的核心思想是将嵌入检索与LLM合成所需的上下文大小进行解耦。标准的RAG管道使用相同的文本块同时进行嵌入和合成，但嵌入检索通常在小片段上效果更好，而LLM则需要更大的上下文来生成优质答案。句子窗口检索通过先检索小句子，再扩展其上下文来解决这个问题。

## 设置环境与文档

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_9.png)

上一节我们介绍了课程目标，本节中我们来看看如何准备环境和数据。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_11.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_12.png)

首先，确保安装了必要的包，例如 `llama-index` 和 `trulens-eval`。你还需要一个OpenAI API密钥，用于嵌入模型、LLM以及评估部分。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_14.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_16.png)

```python
# 示例：导入必要库
from llama_index import VectorStoreIndex, ServiceContext
from llama_index.node_parser import SentenceWindowNodeParser
import openai
```

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_17.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_19.png)

接下来，我们加载用于实验的文档。本课程使用《如何在人工智能中构建职业生涯》的电子书作为示例文档。该文档包含41页，我们将它们合并为一个文档对象，这有助于在使用高级检索器时提高文本分割的准确性。

```python
# 示例：加载并合并文档
documents = load_your_pdf("career_in_ai.pdf")
combined_document = "\n".join([doc.text for doc in documents])
```

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_21.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_22.png)

## 理解句子窗口检索

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_24.png)

在深入设置之前，我们先来理解句子窗口检索的工作原理。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_26.png)

标准的RAG管道使用相同的文本块进行嵌入和合成。问题是，基于嵌入的检索通常与较小的片段配合良好，而LLM需要更多的上下文和较大的片段来合成一个好的答案。句子窗口检索将这两个过程稍微分离。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_28.png)

我们首先对较小的片段或句子进行嵌入，并将其存储在向量数据库中。同时，我们为每个片段添加上下文（即其前后出现的句子）。在检索时，我们使用相似性搜索找到与问题最相关的句子，然后将该句子替换为完整的周围上下文。这允许我们扩大实际输入到LLM的上下文范围。

## 设置句子窗口检索器

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_30.png)

理解了原理后，本节我们将动手设置句子窗口检索方法。我们将从窗口大小为3、top k值为6开始。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_32.png)

首先，导入 `SentenceWindowNodeParser` 对象。这个解析器会将文档分割成单独的句子，并为每个句子片段添加上下文增强。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_34.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_36.png)

以下是节点解析器的工作示例：

```python
from llama_index.node_parser import SentenceWindowNodeParser

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_38.png)

node_parser = SentenceWindowNodeParser.from_defaults(
    window_size=3,
    window_metadata_key="window",
    original_text_metadata_key="original_sentence",
)
# 对示例文本进行解析
sample_text = "Hello world. This is a test. Goodbye."
nodes = node_parser.get_nodes_from_documents([Document(text=sample_text)])
# 查看第二个节点的元数据
print(nodes[1].metadata)
```
元数据将包含原始句子及其前后相邻的句子。

## 构建索引

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_40.png)

设置好解析器后，下一步是构建索引。

首先，设置LLM。在本例中，我们使用 `gpt-3.5-turbo`，温度设置为0.1。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_42.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_44.png)

```python
llm = OpenAI(model="gpt-3.5-turbo", temperature=0.1)
```

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_46.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_48.png)

接着，设置 `ServiceContext` 对象。这是一个包装对象，包含索引所需的所有必要组件，包括LLM、嵌入模型和节点解析器。

```python
embed_model = "local:BAAI/bge-small-en" # 使用本地运行的BGE-small嵌入模型
service_context = ServiceContext.from_defaults(
    llm=llm,
    embed_model=embed_model,
    node_parser=node_parser,
)
```

然后，使用源文档和服务上下文来设置向量存储索引。这将把源文档转换为一组附加了上下文的句子，进行嵌入，并加载到向量存储中。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_50.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_52.png)

```python
index = VectorStoreIndex.from_documents(
    documents, service_context=service_context
)
# 可选：将索引保存到磁盘
index.storage_context.persist(persist_dir="./sentence_index")
```

如果索引已构建并保存，你可以从现有文件加载它，避免重复构建。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_54.png)

## 配置查询引擎

索引构建完成后，我们需要配置查询引擎来使用句子窗口检索。

首先，定义 `MetadataReplacementPostProcessor`。这个后处理器会获取存储在元数据中的上下文值，并用它替换节点的文本内容。

```python
from llama_index.postprocessor import MetadataReplacementPostProcessor

postproc = MetadataReplacementPostProcessor(
    target_metadata_key="window"
)
```

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_56.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_58.png)

接下来，添加句子变换重排序器。它获取查询和检索到的节点，并使用专门为重新排序任务设计的模型（如 `BAAI/bge-reranker-base`）按相关性对节点进行重新排序。通常，你会设置一个较大的初始相似度 `top_k`，然后让重排序器筛选出较小的 `top_n` 结果集。

```python
from llama_index.postprocessor import SentenceTransformerRerank

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_60.png)

rerank = SentenceTransformerRerank(
    model="BAAI/bge-reranker-base", top_n=2
)
```

现在，将所有组件组合到查询引擎中。我们设置相似度 `top_k=6`，然后使用重排序器过滤出最相关的2个结果。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_62.png)

```python
query_engine = index.as_query_engine(
    similarity_top_k=6,
    node_postprocessors=[postproc, rerank]
)
```

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_64.png)

让我们运行一个示例查询：

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_66.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_68.png)

```python
response = query_engine.query("在AI领域建立职业生涯的关键是什么？")
print(response)
```
响应可能是：“在AI领域建立职业生涯的关键是学习基础的技术技能，参与项目并找到实习机会。”

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_70.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_72.png)

## 整合与评估

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_74.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_76.png)

我们已经成功设置了句子窗口查询引擎。现在，我们将所有代码整合到工具函数中，并学习如何使用TruLens进行评估。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_78.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_80.png)

以下是将构建索引和获取查询引擎的功能整合在一起的示例函数：

```python
def build_sentence_window_index(documents, llm, embed_model, save_dir):
    # ... 构建索引的代码 ...
    return index

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_82.png)

def get_sentence_window_query_engine(index, similarity_top_k=6, rerank_top_n=2):
    # ... 配置查询引擎的代码 ...
    return query_engine
```

现在，你已经准备好尝试句子窗口检索了。在下一部分，我们将展示如何运行评估，比较不同句子窗口大小对性能的影响。

## 使用TruLens评估性能

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_84.png)

本节我们将学习如何使用TruLens评估句子窗口检索器的性能，并与基础RAG进行比较。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_86.png)

随着句子窗口大小的增加，标记使用量（成本）会增加，但上下文相关性通常也会提高。我们预期，在开始增加窗口大小时，上下文相关性和答案的“接地性”（即答案基于检索上下文的程度）会提高。然而，窗口过大可能导致LLM被过多信息淹没，反而降低接地性。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_88.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_90.png)

我们将逐步增加句子窗口大小（例如1, 3, 5），并使用TruLens的RAG三元组（答案相关性、上下文相关性、接地性）来评估每个版本。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_92.png)

首先，加载一组预定义的评估问题。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_94.png)

```python
eval_questions = load_eval_questions() # 你的问题列表
```

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_96.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_98.png)

然后，为每个句子窗口大小设置评估流程：

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_100.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_102.png)

```python
from trulens_eval import Tru, Feedback, TruLlama
from trulens_eval.feedback import Groundedness, Relevance

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_104.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_106.png)

tru = Tru()
# 定义反馈函数
f_context_relevance = Feedback(Relevance.context_relevance).on_input_output()
# ... 其他反馈函数定义

for window_size in [1, 3, 5]:
    # 1. 用指定的 window_size 重建索引和查询引擎
    index = build_sentence_window_index(..., window_size=window_size)
    query_engine = get_sentence_window_query_engine(index, ...)

    # 2. 用TruLlama包装查询引擎进行记录
    tru_query_engine = TruLlama(query_engine,
        app_id=f'Sentence Window Retrieval (size={window_size})',
        feedbacks=[f_context_relevance, ...])
    
    # 3. 对每个评估问题运行并记录
    for question in eval_questions:
        with tru_query_engine as recording:
            response = query_engine.query(question)
    # 记录会被自动发送到TruLens数据库
```

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_108.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_109.png)

运行评估后，你可以在TruLens仪表板中查看聚合指标和每个记录的详细分析。通过比较不同窗口大小的指标（如平均延迟、总成本、上下文相关性得分、接地性得分），你可以找到最佳的参数平衡点。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_111.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_113.png)

例如，实验可能发现：
*   窗口大小从1增加到3时，上下文相关性和接地性显著提升。
*   窗口大小从3增加到5时，成本增加，但接地性可能因信息过载而下降。
*   对于你的特定应用，窗口大小为3可能是最佳选择。

## 总结

在本节课中，我们一起学习了句子窗口检索这一高级RAG技术。

我们首先了解了其核心思想：将用于检索的小句子片段与用于LLM合成的扩展上下文分离开来。接着，我们逐步设置了句子窗口节点解析器、构建了向量索引，并配置了包含元数据替换和重排序功能的查询引擎。最后，我们学习了如何使用TruLens框架，通过系统性地调整句子窗口大小等参数，并评估RAG三元组指标，来迭代和优化我们的RAG应用性能。

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_115.png)

![](img/4a1fc26221d3b2f04bf8ca6e27e0a318_117.png)

通过这种方法，你可以在检索精度、答案质量和系统成本之间找到适合你应用场景的最佳平衡点。在接下来的课程中，我们将探索其他高级RAG技术，如自动合并检索，以解决可能遇到的其他挑战。