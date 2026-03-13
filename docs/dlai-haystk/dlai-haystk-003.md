# 003：构建自定义RAG管道 🛠️

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_0.png)

在本节课中，我们将学习如何构建一个简单的检索增强生成（RAG）管道，并探索如何定制其行为。我们将从创建一个基础的问答RAG管道开始，然后对其进行修改，使其能够引用答案的来源。

## 概述：什么是RAG？

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_2.png)

检索增强生成（RAG）主要包含两个核心步骤：
1.  **检索**：接收一个查询，并从数据库中检索与该查询最相关的文档。
2.  **生成**：利用检索到的文档内容来增强提示，并生成最终答案。

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_4.png)

这个过程可以概括为以下流程：
**查询 -> 检索相关文档 -> 构建增强提示 -> 生成答案**

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_6.png)

最常见的检索方式是基于嵌入的语义搜索。其核心思想是：
**为查询和所有文档创建向量表示（嵌入），然后通过计算余弦相似度等方法来找出语义上最相似的文档。**

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_8.png)

在Haystack框架中，一个典型的RAG管道包含以下组件：查询嵌入器、嵌入检索器、提示构建器和生成器。

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_10.png)

---

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_12.png)

## 构建索引管道 📥

上一节我们介绍了RAG的基本概念，本节我们将动手构建管道。首先，我们需要一个索引管道来准备和存储文档数据。

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_14.png)

在本实验中，我们将改变索引文档的方式。之前是将文本文件索引到内存中，这次我们将使用网络内容。具体来说，我们会使用`LinkContentFetcher`组件来获取URL的内容，并使用Cohere的模型来生成文档嵌入。

以下是构建索引管道所需的步骤和代码：

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_16.png)

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_18.png)

首先，我们初始化文档存储和各个组件。
```python
# 初始化内存文档存储
document_store = InMemoryDocumentStore()

# 初始化用于获取URL内容的组件
fetcher = LinkContentFetcher()

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_20.png)

# 初始化将HTML转换为Haystack文档格式的组件
converter = HTMLToDocument()

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_22.png)

# 初始化Cohere文档嵌入器
embedder = CohereDocumentEmbedder(model=“embed-english-v3.0”)

# 初始化文档写入器
writer = DocumentWriter(document_store)
```

接下来，我们将这些组件添加到管道中并建立连接。
```python
# 创建索引管道并添加组件
indexing_pipeline = Pipeline()
indexing_pipeline.add_component(“fetcher”, fetcher)
indexing_pipeline.add_component(“converter”, converter)
indexing_pipeline.add_component(“embedder”, embedder)
indexing_pipeline.add_component(“writer”, writer)

# 连接组件：fetcher -> converter -> embedder -> writer
indexing_pipeline.connect(“fetcher.streams”, “converter.sources”)
indexing_pipeline.connect(“converter”, “embedder”)
indexing_pipeline.connect(“embedder”, “writer”)
```

现在，我们可以运行这个索引管道，将指定URL的内容获取、转换、嵌入并存入文档库。
```python
# 定义要索引的URL列表
urls = [
    “https://haystack.deepset.ai/integrations/anthropic”,
    “https://haystack.deepset.ai/integrations/cohere”,
    “https://haystack.deepset.ai/integrations/jina”,
    “https://haystack.deepset.ai/integrations/nvidia”
]

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_24.png)

# 运行管道，输入是URLs
indexing_pipeline.run({“fetcher”: {“urls”: urls}})
```
运行后，四个文档及其嵌入向量已被写入文档存储。每个文档不仅包含网页内容，还在元数据中保存了原始URL，这在后续定制中会很有用。

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_26.png)

---

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_28.png)

## 构建基础RAG管道 ❓

现在，我们已经有了一个包含文档的数据库，接下来可以构建检索增强生成（RAG）管道了。首先，我们需要设计一个提示模板。

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_30.png)

Haystack使用Jinja2模板语言来构建提示，这非常灵活，允许我们使用循环、条件等逻辑。以下是一个基础的问答提示模板：
```jinja2
根据提供的上下文回答问题。
上下文：
{% for document in documents %}
{{ document.content }}
{% endfor %}
问题：{{ query }}
答案：
```

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_32.png)

这个模板表明，提示构建器期望接收`documents`和`query`作为输入，并将所有文档的内容循环填入“上下文”部分。

以下是构建RAG管道的代码：

首先，初始化管道所需的各个组件。
```python
# 初始化查询嵌入器（使用与文档嵌入相同的Cohere模型）
query_embedder = CohereTextEmbedder(model=“embed-english-v3.0”)

# 初始化基于嵌入的检索器
retriever = InMemoryEmbeddingRetriever(document_store)

# 初始化提示构建器，并传入我们定义的模板
prompt_builder = PromptBuilder(template=“””根据提供的上下文回答问题。
上下文：
{% for document in documents %}
{{ document.content }}
{% endfor %}
问题：{{ query }}
答案：”””)

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_34.png)

# 初始化生成器（这里使用OpenAI的GPT-3.5）
generator = OpenAIGenerator(model=“gpt-3.5-turbo”)
```

接着，创建管道，添加组件并连接它们。
```python
# 创建RAG管道
rag_pipeline = Pipeline()

# 添加组件
rag_pipeline.add_component(“query_embedder”, query_embedder)
rag_pipeline.add_component(“retriever”, retriever)
rag_pipeline.add_component(“prompt_builder”, prompt_builder)
rag_pipeline.add_component(“generator”, generator)

# 连接组件：query_embedder -> retriever -> prompt_builder -> generator
rag_pipeline.connect(“query_embedder.embedding”, “retriever.query_embedding”)
rag_pipeline.connect(“retriever”, “prompt_builder.documents”)
rag_pipeline.connect(“prompt_builder”, “generator”)
```

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_36.png)

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_38.png)

现在，我们可以运行这个RAG管道进行问答了。
```python
# 定义问题
question = “我如何使用Cohere与Haystack一起？”

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_40.png)

# 运行管道
result = rag_pipeline.run({
    “query_embedder”: {“text”: question},
    “prompt_builder”: {“query”: question},
    “retriever”: {“top_k”: 1}  # 仅检索最相关的1个文档
})

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_42.png)

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_44.png)

# 打印生成的答案
print(result[“generator”][“replies”][0])
```
管道会检索与问题最相关的文档，将其内容填入提示，并调用GPT-3.5生成答案。

---

## 定制RAG管道行为 🎨

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_46.png)

上一节我们构建了一个基础的问答管道，本节我们来看看如何通过修改提示来定制它的行为。例如，我们可以让模型在回答时引用来源URL，并指定回答的语言。

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_48.png)

回想一下，在索引时，文档的元数据中包含了URL。我们可以在提示模板中访问这个元数据。以下是一个定制后的提示模板：
```jinja2
你是一个有帮助的助手。请根据以下上下文回答问题。
每个上下文片段都来自一个网页。
你的答案应该使用{{ language }}。

{% for document in documents %}
内容 {{ loop.index }}: {{ document.content }}
URL {{ loop.index }}: {{ document.meta[‘url’] }}
{% endfor %}

问题：{{ query }}
答案：
```

这个新模板增加了两个功能：
1.  它期望一个额外的输入变量 `language`，用于指定回答语言。
2.  它在循环中不仅输出文档内容，还输出了文档元数据中的URL。

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_50.png)

使用新模板构建和运行管道的代码与之前类似，只需更新`PromptBuilder`的模板，并在运行时提供额外的`language`参数。
```python
# 使用新模板初始化提示构建器
custom_prompt_builder = PromptBuilder(template=“””你是一个有帮助的助手... [上述模板内容] ...”””)

# ... (将custom_prompt_builder加入管道并连接，过程同上)

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_52.png)

# 运行定制后的管道
result = rag_pipeline.run({
    “query_embedder”: {“text”: question},
    “prompt_builder”: {“query”: question, “language”: “法语”}, # 指定法语回答
    “retriever”: {“top_k”: 1}
})
```
这样，模型就会用法语生成答案，并且在答案的上下文中明确指出了所引用信息的来源URL。你可以通过修改模板来尝试更多定制，例如改变指令、调整检索文档的数量（`top_k`）等。

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_54.png)

---

## 总结 📝

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_56.png)

在本节课中，我们一起学习了：
1.  **RAG的核心原理**：即检索与生成两阶段流程，通过语义搜索找到相关文档来增强提示。
2.  **构建索引管道**：使用Haystack组件获取网页内容、转换为文档、生成嵌入并存储。
3.  **构建基础RAG管道**：组合查询嵌入器、检索器、提示构建器和生成器，实现问答功能。
4.  **定制管道行为**：通过灵活修改Jinja2提示模板，我们可以轻松地让RAG管道支持引用来源、指定输出语言等高级功能。

![](img/3f9a062d201ae3d8166c2e3d753f7e8b_58.png)

你已掌握了使用Haystack搭建和定制RAG管道的基本方法。在下一节课中，我们将探索如何创建自己的Haystack组件，以获得更大的灵活性。