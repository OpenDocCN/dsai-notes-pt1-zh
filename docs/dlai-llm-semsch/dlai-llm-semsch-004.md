# 004：稠密检索 🧠

在本节课中，我们将学习如何利用嵌入向量进行语义搜索，即根据含义进行搜索。上一节我们介绍了嵌入向量的概念，本节中我们来看看如何将其应用于搜索任务。

![](img/db00d5bef4906711cee3eb00bb5bc65e_1.png)

本节课分为两部分。第一部分，我们将连接到第一课中使用过的数据库，但不再使用关键词搜索，而是利用嵌入向量进行向量搜索以实现语义搜索。第二部分，在熟悉了如何查询一个已准备好的向量数据库后，我们将从头开始处理文本，学习如何从零构建一个向量索引。

## 第一部分：使用向量数据库进行语义搜索

首先，我们加载API密钥并设置环境，这与之前的操作一致。

![](img/db00d5bef4906711cee3eb00bb5bc65e_3.png)

```python
import cohere
co = cohere.Client('YOUR_API_KEY')
```

![](img/db00d5bef4906711cee3eb00bb5bc65e_5.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_7.png)

接下来，我们设置Weaviate客户端并连接到同一个数据库。到目前为止，这些步骤都是熟悉的。

![](img/db00d5bef4906711cee3eb00bb5bc65e_9.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_11.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_13.png)

在深入搜索代码之前，让我们基于嵌入课程的知识，理解其背后的原理。假设查询是“what is the capital of Canada”，而我们的档案库中有五个可能的句子。通过嵌入模型，我们可以将这些句子和查询投射到同一个向量空间中。语义相似的句子在空间中的位置会更接近。一个为搜索优化的嵌入模型会使查询的向量最接近其答案的向量。这就是我们利用嵌入的相似性和距离属性进行搜索的方式，也就是**稠密检索**。

![](img/db00d5bef4906711cee3eb00bb5bc65e_15.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_16.png)

以下是使用Weaviate进行稠密检索的代码。它与第一课的关键词搜索代码很相似，主要区别在于我们使用 `near_text` 参数进行向量搜索，而不是 `BM25`。

![](img/db00d5bef4906711cee3eb00bb5bc65e_18.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_19.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_21.png)

```python
response = (
    client.query
    .get("Articles", ["title", "url", "text"])
    .with_near_text({"concepts": [query]})
    .with_limit(3)
    .do()
)
```

![](img/db00d5bef4906711cee3eb00bb5bc65e_23.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_24.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_25.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_26.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_28.png)

执行此代码后，我们可以发送查询并查看结果。

![](img/db00d5bef4906711cee3eb00bb5bc65e_29.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_31.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_33.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_35.png)

让我们运行几个查询来比较稠密检索和关键词搜索的效果。

![](img/db00d5bef4906711cee3eb00bb5bc65e_37.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_39.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_40.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_42.png)

**查询示例 1: “who wrote Hamlet”**
*   **稠密检索结果**：返回的文本提到了莎士比亚创作《哈姆雷特》，这是最相关的结果。
*   **关键词搜索对比**：我们导入第一课的函数进行对比。

![](img/db00d5bef4906711cee3eb00bb5bc65e_44.png)

**查询示例 2: “what is the capital of Canada”**
*   **稠密检索结果**：返回关于渥太华（加拿大首都）的维基百科页面。
*   **关键词搜索结果**：返回的结果涉及加拿大君主制、早期现代时期等，与“首都”这一核心意图的相关性较弱。

**查询示例 3: “tallest person in history”**
*   **稠密检索结果**：正确返回了历史上最高的人“Robert Wadlow”的相关记录。
*   **关键词搜索结果**：返回了关于日本历史、“7 summits”等不相关的内容。

![](img/db00d5bef4906711cee3eb00bb5bc65e_46.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_48.png)

此外，稠密检索还支持**多语言搜索**，因为使用的嵌入模型是多语言的。例如，用阿拉伯语输入“历史上最高的人”，依然能返回正确的结果。

![](img/db00d5bef4906711cee3eb00bb5bc65e_50.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_51.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_52.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_53.png)

另一个强大的应用是**探索性搜索**。例如，查询“film about a time travel paradox”，可以返回《About Time》、《The Time Machine》等相关电影，帮助你探索数据集。

以上是第一部分的结束，我们演示了如何消费一个现成的向量数据库。接下来，我们将进入第二部分，学习如何从头构建自己的向量搜索数据库。

![](img/db00d5bef4906711cee3eb00bb5bc65e_55.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_57.png)

## 第二部分：从头构建向量搜索索引

![](img/db00d5bef4906711cee3eb00bb5bc65e_59.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_60.png)

首先，我们导入必要的库，包括近似最近邻库 `Annoy`。

```python
import cohere
from annoy import AnnoyIndex
import numpy as np
import textwrap
```

![](img/db00d5bef4906711cee3eb00bb5bc65e_62.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_63.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_64.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_66.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_67.png)

我们将使用电影《星际穿越》的维基百科页面文本作为示例数据。

![](img/db00d5bef4906711cee3eb00bb5bc65e_69.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_71.png)

构建语义搜索系统的一个关键步骤是**文本分块**。如何分块取决于具体任务。常见方法包括按句号分割句子或按段落分割。对于维基百科这类结构清晰的文本，按句号分割是一个简单选择。但需要注意，单独的句子可能缺乏上下文。一个改进方法是**为每个块添加标题作为上下文**，例如在每个句子前加上“Interstellar (film): ”。

![](img/db00d5bef4906711cee3eb00bb5bc65e_73.png)

以下是分块和添加上下文的示例代码：

![](img/db00d5bef4906711cee3eb00bb5bc65e_75.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_77.png)

```python
# 假设 text 包含维基百科全文
sentences = text.split('. ')
title = "Interstellar (film)"
chunks = [f"{title}: {sentence}." for sentence in sentences]
```

![](img/db00d5bef4906711cee3eb00bb5bc65e_79.png)

![](img/db00d5bef4906711cee3eb00bb5bc65e_81.png)

完成分块后，下一步是**为每个文本块生成嵌入向量**。

![](img/db00d5bef4906711cee3eb00bb5bc65e_83.png)

```python
embeds = co.embed(texts=chunks).embeddings
embeds = np.array(embeds) # 转换为NumPy数组以便处理
# embeds 的维度为 (句子数量, 嵌入向量维度)
```

![](img/db00d5bef4906711cee3eb00bb5bc65e_85.png)

现在，我们将这些嵌入向量存入一个搜索索引。这里使用 `Annoy` 库来构建一个近似最近邻索引。

```python
# 初始化索引，嵌入向量维度为4096，使用欧氏距离
search_index = AnnoyIndex(4096, 'angular')
for i, embed in enumerate(embeds):
    search_index.add_item(i, embed)
search_index.build(10) # 构建索引，10表示树的数量
search_index.save('test.ann') # 保存索引到文件
```

索引构建完成后，我们可以定义一个搜索函数。该函数会嵌入查询文本，然后在索引中查找最近的邻居。

```python
def search(query):
    # 嵌入查询
    query_embed = co.embed(texts=[query]).embeddings
    query_embed = np.array(query_embed[0])
    # 在索引中搜索最近邻
    nearest_ids = search_index.get_nns_by_vector(query_embed, 3, include_distances=True)
    # 返回结果
    for _id, dist in zip(nearest_ids[0], nearest_ids[1]):
        print(f"Distance: {dist:.2f}")
        print(textwrap.fill(chunks[_id], width=60))
        print("\n")
```

![](img/db00d5bef4906711cee3eb00bb5bc65e_87.png)

让我们测试一下。查询“how much did the film make”，即使句子中没有直接出现“make”这个词，系统也能返回关于电影全球票房（gross）的正确句子。

![](img/db00d5bef4906711cee3eb00bb5bc65e_88.png)

## 稠密检索、向量搜索库与向量数据库

我们已经看到了如何使用 `Annoy` 这样的库进行稠密检索。现在我们来讨论一下**近似最近邻向量搜索库**（如 Annoy, Faiss, ScaNN）和**向量数据库**（如 Weaviate, Pinecone, Chroma）之间的区别。

以下是两者的主要对比：

*   **设置与复杂度**：向量搜索库通常更轻量、更简单，易于设置。向量数据库功能更丰富，但可能更复杂。
*   **数据管理**：向量搜索库通常只存储向量。向量数据库除了存储向量，还能存储并管理原始文本、元数据等。
*   **更新操作**：向向量搜索库中添加或修改记录通常需要重建整个索引。向量数据库则支持动态增删改查，更容易更新。
*   **查询功能**：向量数据库支持更复杂的查询，例如结合元数据过滤（如我们之前按语言过滤）。

在实际应用中，你无需用向量搜索完全取代关键词搜索。它们可以互补，形成**混合搜索**管道。即对一个查询同时执行关键词搜索和向量搜索，然后对两者的结果评分进行聚合，以呈现最佳结果。你还可以引入其他信号（如网页的权威性评分）来进一步优化排序。

在下一课中，我们将学习语义搜索的第二种主要方式：**重排序**，它能在搜索步骤之后显著提升结果的相关性。

## 总结

本节课中我们一起学习了：
1.  **稠密检索的原理**：利用嵌入向量将查询和文档映射到同一空间，通过计算距离来寻找语义上最相似的文档。
2.  **使用 Weaviate 进行向量搜索**：实践了如何对现有向量数据库执行语义搜索查询，并对比了其与关键词搜索的效果差异，特别是在处理语义意图、多语言查询和探索性搜索时的优势。
3.  **从头构建向量索引**：学习了文本分块、添加上下文、生成嵌入以及使用 `Annoy` 库构建和查询近似最近邻索引的完整流程。
4.  **技术选型对比**：了解了近似最近邻库与全功能向量数据库在复杂度、数据管理和更新操作上的区别，并认识了混合搜索的基本概念。

![](img/db00d5bef4906711cee3eb00bb5bc65e_90.png)

通过本课，你已经掌握了实现语义搜索的核心技能之一，并能够根据需求选择合适的技术工具来构建搜索系统。