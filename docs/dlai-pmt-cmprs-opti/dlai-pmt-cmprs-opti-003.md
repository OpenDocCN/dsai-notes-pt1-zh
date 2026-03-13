# 003：使用元数据进行过滤 🎯

![](img/30d35882e289d43893c899049d3014fe_1.png)

在本节课中，我们将学习如何使用元数据来优化MongoDB的向量搜索查询。我们将构建一个多阶段的聚合管道，通过后过滤和前过滤技术，利用元数据来细化和限制搜索结果，从而提高检索的效率和相关性。

![](img/30d35882e289d43893c899049d3014fe_3.png)

## 概述

![](img/30d35882e289d43893c899049d3014fe_5.png)

元数据是关于数据的额外信息，它为关键数据点提供了更多上下文。在大型语言模型应用和检索增强生成（RAG）模式中，元数据可以增强嵌入数据的上下文，并通过过滤和排序来提高向量搜索查询的相关性。

![](img/30d35882e289d43893c899049d3014fe_7.png)

## 什么是元数据？

元数据旨在补充一个关键数据点并提供更多上下文。例如，蒙娜丽莎的图像是关键数据，其向量嵌入可以代表图像本身。而标题、艺术家姓名、位置等信息则可以作为元数据添加到图像嵌入中。

![](img/30d35882e289d43893c899049d3014fe_9.png)

**关键数据 + 元数据 = 单个MongoDB文档**

![](img/30d35882e289d43893c899049d3014fe_11.png)

通过将向量嵌入与元数据配对，整体数据变得更具信息量和有用性。

![](img/30d35882e289d43893c899049d3014fe_13.png)

## 元数据如何提升搜索？

在RAG设计模式中，元数据主要有两个作用：
1.  为嵌入数据添加额外上下文，以提高模型对内容的理解和相关性判断。
2.  通过基于数据属性（即元数据）进行过滤和排序，来缩小向量搜索的范围或优化其结果。

为了简化操作并提高结果的相关性，我们将在MongoDB的聚合管道中，将过滤阶段与向量搜索阶段组合使用。聚合管道使得创建和实现复杂的可组合查询变得更加容易。

![](img/30d35882e289d43893c899049d3014fe_15.png)

## 过滤技术：后过滤与前过滤

![](img/30d35882e289d43893c899049d3014fe_17.png)

上一节我们介绍了元数据的概念，本节中我们来看看两种核心的过滤技术。

以下是两种主要的过滤方法：

### 后过滤
在后过滤中，我们先对完整数据集执行向量搜索，以获得与用户查询语义相似的结果。然后，在向量搜索阶段**之后**应用一个过滤阶段，根据指定的元数据标准（如位置、容量）进一步减少返回的结果。

**流程**：完整数据集 → 向量搜索 → 过滤 → 最终结果

![](img/30d35882e289d43893c899049d3014fe_19.png)

### 前过滤
在前过滤中，流程则相反。我们首先对数据集应用过滤操作，去除不符合元数据标准的结果。然后，仅对这个过滤后的数据子集进行向量搜索。

**流程**：完整数据集 → 过滤 → 向量搜索 → 最终结果

**关键区别**：前过滤在进行向量搜索**之前**就缩小了数据集，这可能会提高搜索效率，但也存在风险：一些在语义上与查询相似但不符合过滤条件的文档，将完全不会被纳入搜索范围，从而导致潜在的相关信息丢失。

![](img/30d35882e289d43893c899049d3014fe_21.png)

## 实践：构建带过滤的RAG管道

![](img/30d35882e289d43893c899049d3014fe_23.png)

![](img/30d35882e289d43893c899049d3014fe_25.png)

了解了理论之后，现在让我们动手编写代码，实际构建一个结合过滤功能的RAG管道。

![](img/30d35882e289d43893c899049d3014fe_27.png)

### 1. 准备环境与数据
首先，我们需要导入必要的模块并加载数据。我们使用一个自定义工具模块来简化流程。

![](img/30d35882e289d43893c899049d3014fe_29.png)

![](img/30d35882e289d43893c899049d3014fe_31.png)

```python
# 导入自定义工具模块
from utils import process_records, connectToDatabase, setupVectorSearchIndex

![](img/30d35882e289d43893c899049d3014fe_33.png)

# 加载并处理数据集
data_points = process_records(loaded_dataset)  # 返回符合数据模型的Python列表

![](img/30d35882e289d43893c899049d3014fe_35.png)

![](img/30d35882e289d43893c899049d3014fe_37.png)

# 连接数据库
db, collection = connectToDatabase()

![](img/30d35882e289d43893c899049d3014fe_39.png)

# 清空集合（为实验做准备）
delete_result = collection.delete_many({})
print(f"已删除 {delete_result.deleted_count} 个文档。")

![](img/30d35882e289d43893c899049d3014fe_41.png)

![](img/30d35882e289d43893c899049d3014fe_43.png)

# 将数据插入集合
collection.insert_many(data_points)
print("数据摄入完成。")
```

![](img/30d35882e289d43893c899049d3014fe_45.png)

### 2. 创建向量搜索索引
接下来，我们需要为集合创建向量搜索索引，这是进行语义搜索的基础。

```python
# 设置向量搜索索引
index_name = setupVectorSearchIndex(collection)
```

### 3. 基础向量搜索查询
我们定义一个基础的向量搜索函数，它将用户查询转换为向量，并在数据库中执行搜索。

![](img/30d35882e289d43893c899049d3014fe_47.png)

```python
def vector_search(user_query, db, collection, index_name):
    # 将用户查询转换为嵌入向量
    query_embedding = get_embedding(user_query)
    
    # 构建聚合管道
    pipeline = [
        {
            "$vectorSearch": {
                "index": index_name,
                "path": "embedding_field",
                "queryVector": query_embedding,
                "numCandidates": 150,
                "limit": 20
            }
        }
    ]
    
    # 执行搜索并计时
    start_time = time.time()
    results = list(collection.aggregate(pipeline))
    elapsed_time = time.time() - start_time
    
    print(f"向量搜索耗时: {elapsed_time:.4f} 秒")
    return results
```

![](img/30d35882e289d43893c899049d3014fe_49.png)

### 4. 处理后过滤查询
现在，我们来实现后过滤。在向量搜索之后，添加一个`$match`阶段来过滤结果。

以下是后过滤的实现步骤：

![](img/30d35882e289d43893c899049d3014fe_51.png)

1.  **定义过滤路径**：指定要基于哪个元数据字段进行过滤（例如，`address.country`）。
2.  **创建匹配阶段**：使用`$match`操作符定义过滤条件（例如，国家为“美国”，容纳人数在1到5人之间）。
3.  **修改管道**：在向量搜索阶段后添加这个`$match`阶段。

![](img/30d35882e289d43893c899049d3014fe_53.png)

![](img/30d35882e289d43893c899049d3014fe_55.png)

```python
# 定义过滤路径和条件
search_path = "address.country"
filter_criteria = {
    search_path: "United States",
    "accommodates": {"$gt": 1, "$lt": 5}
}

![](img/30d35882e289d43893c899049d3014fe_57.png)

# 创建额外的匹配阶段（后过滤）
additional_stage = {"$match": filter_criteria}

# 修改向量搜索函数以支持后过滤
def vector_search_with_postfilter(user_query, db, collection, index_name, extra_stage=None):
    query_embedding = get_embedding(user_query)
    pipeline = [
        {
            "$vectorSearch": {
                "index": index_name,
                "path": "embedding_field",
                "queryVector": query_embedding,
                "numCandidates": 150,
                "limit": 20
            }
        }
    ]
    if extra_stage:
        pipeline.append(extra_stage)  # 添加后过滤阶段
    
    results = list(collection.aggregate(pipeline))
    return results

# 执行带后过滤的查询
user_query = “寻找一个温暖、友好且离餐厅不远的地方。”
postfilter_results = vector_search_with_postfilter(user_query, db, collection, index_name, additional_stage)
```
执行后，返回的文档将仅限于来自美国且容纳人数在1-5之间的房源。

![](img/30d35882e289d43893c899049d3014fe_59.png)

### 5. 实现前过滤查询
最后，我们来实现前过滤。这需要在向量搜索阶段内部定义过滤条件，以便在搜索执行前就应用过滤。

以下是前过滤的实现步骤：

![](img/30d35882e289d43893c899049d3014fe_61.png)

1.  **创建支持过滤的索引**：为了高效执行，可以为过滤字段创建复合索引（可选，但推荐）。
2.  **修改向量搜索阶段**：在`$vectorSearch`参数中直接加入`filter`条件。
3.  **定义新的搜索函数**：该函数在搜索前就通过`filter`字段限制数据集。

![](img/30d35882e289d43893c899049d3014fe_63.png)

```python
# 首先，创建一个新的支持过滤的向量搜索索引（可选）
filtered_index_name = “vector_index_with_filter”
# ... 创建索引的代码 ...

![](img/30d35882e289d43893c899049d3014fe_65.png)

# 定义带前过滤的向量搜索函数
def vector_search_with_prefilter(user_query, db, collection, index_name):
    query_embedding = get_embedding(user_query)
    pipeline = [
        {
            "$vectorSearch": {
                "index": index_name,
                "path": "embedding_field",
                "queryVector": query_embedding,
                "numCandidates": 150,
                "limit": 20,
                "filter": {  # 前过滤在此发生
                    "$and": [
                        {"accommodates": {"$gt": 1, "$lt": 5}},
                        {"bedrooms": {"$lte": 7}}
                    ]
                }
            }
        }
    ]
    results = list(collection.aggregate(pipeline))
    return results

![](img/30d35882e289d43893c899049d3014fe_67.png)

# 执行带前过滤的查询
prefilter_results = vector_search_with_prefilter(user_query, db, collection, filtered_index_name)
```
使用前过滤后，向量搜索仅处理那些满足容纳人数和卧室数量条件的文档子集，返回的结果也会严格符合这些条件。

![](img/30d35882e289d43893c899049d3014fe_69.png)

## 结果对比与总结

![](img/30d35882e289d43893c899049d3014fe_71.png)

本节课中，我们一起学习了如何利用元数据对向量搜索进行过滤优化。

![](img/30d35882e289d43893c899049d3014fe_73.png)

![](img/30d35882e289d43893c899049d3014fe_75.png)

*   **后过滤**：先进行广泛的语义搜索，再过滤结果。可能丢失那些语义相关但不符合过滤条件的文档。在我们的例子中，请求20个文档，但过滤后只返回了4个。
*   **前过滤**：先根据元数据严格筛选数据集，再进行语义搜索。搜索效率更高，结果始终符合过滤条件，但可能无法发现过滤条件之外的潜在相关项。它返回了我们请求的20个文档。

![](img/30d35882e289d43893c899049d3014fe_77.png)

**总结**：选择后过滤还是前过滤，取决于你的应用场景。如果确保结果完全符合硬性约束（如法规、价格范围）至关重要，则前过滤更合适。如果你想先最大化语义相关性，再根据次要属性进行筛选，则后过滤是更好的选择。通过将元数据与MongoDB强大的聚合管道结合，你可以构建出既高效又精准的智能搜索系统。

![](img/30d35882e289d43893c899049d3014fe_79.png)

在下一节课中，我们将探讨如何进一步减少从数据库返回的数据量，以优化性能。