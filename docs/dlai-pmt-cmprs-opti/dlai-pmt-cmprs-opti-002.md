# 002：基础向量搜索 🔍

在本节课中，我们将学习向量搜索的基础知识，并动手构建一个完整的检索增强生成（RAG）系统。我们将使用MongoDB作为向量数据库，Pydantic进行数据验证，并基于Airbnb房源数据集创建一个推荐系统。

![](img/7123cecf9341a47c17accb2610c5b124_1.png)

## 概述：从关键词匹配到语义搜索

互联网上存在海量数据。比较数据点之间相似性或接近程度有多种方法。一种常见方法是文本搜索，即将查询关键词与数据点内容的部分进行匹配以计算匹配度。

![](img/7123cecf9341a47c17accb2610c5b124_3.png)

这本质上是信息检索：输入一个关键词或搜索词，与多个数据点进行匹配，并检测关键词是否在内容中。现在，我们将学习如何根据数据的上下文或意义来检索数据。

## 向量搜索基础

![](img/7123cecf9341a47c17accb2610c5b124_5.png)

### 数据向量化

第一步是收集数据。数据可以是结构化数据，如在具有定义列的表格或电子表格中组织的数据；也可以是非结构化数据，如音频和图像数据。

![](img/7123cecf9341a47c17accb2610c5b124_7.png)

下一步涉及将数据作为输入传递给嵌入模型。嵌入模型的输出是一个向量。此时，原始数据已经被向量化，留下了捕捉数据上下文和语义的数字表示。这被称为向量嵌入。

在一个高维空间（称为向量空间）中，可以比较两个或更多嵌入向量之间的距离，以获得它们在语义或上下文方面的接近程度的指示。

![](img/7123cecf9341a47c17accb2610c5b124_9.png)

### 向量搜索与传统搜索

到目前为止，我们理解了向量搜索是一种使用数据的数字表示（称为向量嵌入）来搜索和检索信息的信息检索技术。我们也明白，传统上，信息检索依赖于关键词匹配，即搜索查询文本与数据集中文本之间的直接匹配。

然而，向量搜索利用这些嵌入来实现高级功能，例如：
*   **语义搜索**：理解查询的上下文。
*   **推荐系统**：预测用户偏好。
*   **检索增强生成（RAG）**：为大型语言模型输入提供额外上下文。

![](img/7123cecf9341a47c17accb2610c5b124_11.png)

这些能力使向量搜索在各种人工智能应用中成为强大的工具。

## 向量数据库与索引

![](img/7123cecf9341a47c17accb2610c5b124_13.png)

一旦收集了结构化和非结构化的数据并将其编码为向量嵌入，就需要将向量化的数据存储到一个专门的数据存储中，称为向量数据库。

![](img/7123cecf9341a47c17accb2610c5b124_15.png)

在向量数据库中，为了确保基于向量搜索查询高效检索向量数据，最佳实践是对向量数据进行索引。向量搜索索引是一种专门的结构，它优化了向量嵌入的存储和检索，允许进行高效的相似性搜索。

因此，当执行向量搜索操作时，该索引有助于查询向量与数据集的高效匹配，减少找到最相似向量所需的时间。这就引出了检索增强生成系统中的向量搜索。

## 检索增强生成（RAG）简介

![](img/7123cecf9341a47c17accb2610c5b124_17.png)

检索增强生成（RAG）是一种系统设计模式，它利用信息检索技术（包括向量搜索）和基础模型，为用户查询提供准确和相关的响应。

RAG通过检索语义相似的数据来为用户查询补充额外上下文，然后将检索到的信息与原始查询结合，作为输入传递给大型语言模型来实现这一点。

例如，使用聊天界面的一个典型过程是：输入聊天内容，然后从大型语言模型获得回应。这不是理想的过程，因为这没有使用任何相关数据。

![](img/7123cecf9341a47c17accb2610c5b124_19.png)

理想的过程应该是：在向大型语言模型输入时，添加相关的特定领域数据，这样大型语言模型就可以为查询提供相关且有上下文感知的回应。

![](img/7123cecf9341a47c17accb2610c5b124_21.png)

### RAG的优势

现在我们已经了解了检索增强生成，让我们概述一下RAG设计模式对大型语言模型应用的关键好处。

构建利用RAG系统设计模式的人工智能应用提供了许多好处：
*   **减少幻觉**：将大型语言模型的回应建立在相关和最新的信息基础上，减少模型提供错误或不相关信息的机会。
*   **减少输入量**：通过RAG，可以减少作为输入传递给大型语言模型的信息量，从而减少传递到上下文窗口的上下文。
*   **避免微调**：在某些情况下，不再需要微调大型语言模型。
*   **利用私有数据**：使用RAG，可以利用自己的私有数据或特定领域数据来确保大型语言模型的回应满足特定的要求和需求。

![](img/7123cecf9341a47c17accb2610c5b124_23.png)

## MongoDB：作为向量数据库

![](img/7123cecf9341a47c17accb2610c5b124_25.png)

现在我们知道，当补充相关上下文时，大型语言模型会给出更好的答案。你可能会想知道在哪里以及如何存储这些数据，以及如何实现信息检索的向量搜索。这就是MongoDB发挥作用的地方。

![](img/7123cecf9341a47c17accb2610c5b124_27.png)

MongoDB是一个开发者数据平台，提供具有向量搜索功能的NoSQL数据库。在人工智能应用中，MongoDB可以作为向量数据的存储解决方案，充当向量数据库。MongoDB还提供了更多功能，可作为操作和事务数据的数据存储，使其成为LLM和AI应用（包括RAG和智能系统）的强大内存提供者。

### 文档数据模型

![](img/7123cecf9341a47c17accb2610c5b124_29.png)

你可能熟悉传统的关系型数据库。让我们用房子的数据存储来举例说明关系型数据库是如何工作的。在典型的关系型数据库中，可能会在一个表上存储房子的房间数和浴室数等信息，而在另一个表上存储房子的地址信息。

![](img/7123cecf9341a47c17accb2610c5b124_31.png)

![](img/7123cecf9341a47c17accb2610c5b124_33.png)

在文档模型中，根据应用程序上发生的交互来建模数据，而不是相反。

![](img/7123cecf9341a47c17accb2610c5b124_35.png)

**什么是文档？**
在MongoDB中，文档是类似于JSON的基本数据单元。每个文档是一组键值对，这在MongoDB中相当于关系型数据库中的一行。

![](img/7123cecf9341a47c17accb2610c5b124_37.png)

以房子为例，我们把房子的所有属性都分配在一个文档中，包括它的地址。这是MongoDB中的一个文档示例。文档是动态的，这意味着它们可以在同一集合中包含不同的字段和结构。非关系型数据库中的集合类似于关系型数据库中的表。

![](img/7123cecf9341a47c17accb2610c5b124_39.png)

![](img/7123cecf9341a47c17accb2610c5b124_41.png)

文档模型使用JSON模式，这是技术栈各层的核心数据模型。例如，JSON有助于在应用层的网站部分和REST API之间传输数据，并在实现智能体时用于模型层的函数调用和工具定义。MongoDB能够实现灵活的字段和值数据存储，并能够存储不同的数据类型。

![](img/7123cecf9341a47c17accb2610c5b124_43.png)

![](img/7123cecf9341a47c17accb2610c5b124_45.png)

### 数据建模与聚合管道

为确保文档结构合理，应考虑数据建模。数据建模涉及设计文档和集合的结构，以有效表示和组织数据。这关乎规划如何在文档间存储和链接数据，以优化性能、可扩展性以及应用程序的特定数据访问模式。

![](img/7123cecf9341a47c17accb2610c5b124_47.png)

![](img/7123cecf9341a47c17accb2610c5b124_49.png)

有时，应用程序的组件布局由数据库中数据的结构和格式决定。这表示根据数据层中的信息实现应用层。但理想情况下，应首先从应用程序需求开始，而不是数据本身。必须问：“我将如何访问我的数据？”而这应该决定将如何建模数据结构。

![](img/7123cecf9341a47c17accb2610c5b124_51.png)

MongoDB能够使用对“管道”的熟悉理解，这在数据处理或机器学习概念中存在。可以将管道概念应用于数据库层内的想法。使用MongoDB进行查询时，构建一个聚合管道。

可以将聚合管道视为一系列数据处理阶段，其中每个阶段在数据通过时对其进行转换。这个过程允许在MongoDB中进行复杂的查询组合，因为在管道内有各种数据转换阶段。

![](img/7123cecf9341a47c17accb2610c5b124_53.png)

![](img/7123cecf9341a47c17accb2610c5b124_55.png)

**聚合管道示例：**
假设正在管理来自社交媒体应用程序的数据，其中有一系列用户帖子。想找到2021年1月根据点赞数定义的最热门帖子。也许对按类别汇总每个帖子的平均评论数和点赞数感兴趣。

![](img/7123cecf9341a47c17accb2610c5b124_57.png)

这个聚合管道会：
1.  过滤出2021年1月的帖子。
2.  按类别进行分组。
3.  计算平均点赞数和评论数。
4.  按平均点赞数降序排列结果。

![](img/7123cecf9341a47c17accb2610c5b124_59.png)

![](img/7123cecf9341a47c17accb2610c5b124_61.png)

通过使用聚合管道，可以利用对机器学习和人工智能管道中顺序操作的理解，并将类似逻辑应用于在MongoDB中管理和分析数据，使复杂查询变得相当容易理解和管理。

## Pydantic：数据验证与管理

![](img/7123cecf9341a47c17accb2610c5b124_63.png)

![](img/7123cecf9341a47c17accb2610c5b124_65.png)

在人工智能应用中，需要进行数据验证并确保数据符合特定模型。这降低了生产系统中出现错误的可能性。Pydantic是一个用于数据验证、建模和管理的Python库。

![](img/7123cecf9341a47c17accb2610c5b124_67.png)

Pydantic提供的功能允许创建包含对象及其属性定义的数据模式。Pydantic还确保数据符合定义的模式、数据类型、格式和约束。如果数据模式不符合验证标准，Pydantic通过引发详细说明具体验证问题的异常来处理错误。

![](img/7123cecf9341a47c17accb2610c5b124_69.png)

## 实战：构建Airbnb RAG推荐系统

![](img/7123cecf9341a47c17accb2610c5b124_71.png)

![](img/7123cecf9341a47c17accb2610c5b124_72.png)

在我们深入编码之前，让我们回顾一下数据集。它由托管在Hugging Face上的5000个Airbnb房源组成，包含地址、描述、交通、评论和意见等细节。

![](img/7123cecf9341a47c17accb2610c5b124_74.png)

对于本课程，将使用它基于RAG技术构建一个Airbnb房源推荐系统。每条记录或数据点包括列表照片的图像嵌入和来自空间属性内容的文本嵌入。空间属性中的信息已由OpenAI `text-embedding-ada-002` 模型处理。

![](img/7123cecf9341a47c17accb2610c5b124_76.png)

### 项目步骤概述

![](img/7123cecf9341a47c17accb2610c5b124_78.png)

以下是我们在本节课的编码部分要采取的步骤：
1.  从Hugging Face加载数据。
2.  建立数据库连接以访问数据库和集合。
3.  向集合插入数据（数据摄取）。
4.  使用查询嵌入和集合内的嵌入进行向量搜索查询。
5.  处理用户查询并可视化响应。

![](img/7123cecf9341a47c17accb2610c5b124_80.png)

### 系统预期输出

![](img/7123cecf9341a47c17accb2610c5b124_82.png)

在本节课中，将构建一个使用向量搜索从向量数据库中提取相关结果，并将其作为额外上下文添加到大型语言模型的RAG推荐系统。

系统将展示：
*   向量搜索查询的执行时间。
*   用户问题和系统响应（来自LLM的响应，包含推荐列表及原因）。
*   一个属性表格，显示作为额外上下文传递给LLM的数据（包括名称、可容纳人数、地址等），针对所有从向量搜索查询中检索到的信息。

![](img/7123cecf9341a47c17accb2610c5b124_84.png)

### 代码实现

![](img/7123cecf9341a47c17accb2610c5b124_86.png)

#### 1. 环境设置与库导入

![](img/7123cecf9341a47c17accb2610c5b124_88.png)

```python
import os
from datasets import load_dataset
import pandas as pd
from pydantic import BaseModel, Field
from datetime import datetime
from pymongo import MongoClient
from pymongo.operations import SearchIndexModel
from openai import OpenAI
import time
```

#### 2. 加载数据集

![](img/7123cecf9341a47c17accb2610c5b124_90.png)

```python
# 从Hugging Face加载Airbnb嵌入数据集
dataset = load_dataset("abidlabs/airbnb-embeddings", split="train", streaming=True)
# 取5000个数据点并转换为Pandas DataFrame
dataset = dataset.take(5000)
df = pd.DataFrame(dataset)
# 查看前5个数据点
print(df.head())
```

![](img/7123cecf9341a47c17accb2610c5b124_92.png)

#### 3. 使用Pydantic进行数据建模

![](img/7123cecf9341a47c17accb2610c5b124_94.png)

```python
# 定义数据模型
class Host(BaseModel):
    host_id: str
    host_name: str
    host_location: str | None
    host_response_time: str | None

class Location(BaseModel):
    type: str = "Point"
    coordinates: list[float]

![](img/7123cecf9341a47c17accb2610c5b124_96.png)

class Address(BaseModel):
    street: str
    suburb: str
    government_area: str
    market: str
    country: str
    country_code: str
    location: Location

class Review(BaseModel):
    _id: str
    date: datetime
    listing_id: str
    reviewer_id: str
    reviewer_name: str
    comments: str | None

class Listing(BaseModel):
    _id: str
    listing_url: str
    name: str
    summary: str | None
    space: str | None
    description: str | None
    neighborhood_overview: str | None
    notes: str | None
    transit: str | None
    access: str | None
    interaction: str | None
    house_rules: str | None
    property_type: str
    room_type: str
    bed_type: str
    minimum_nights: str
    maximum_nights: str
    cancellation_policy: str
    last_scraped: datetime
    calendar_last_scraped: datetime
    first_review: datetime | None
    last_review: datetime | None
    accommodates: int
    bedrooms: int | None
    beds: int | None
    number_of_reviews: int
    bathrooms: float | None
    amenities: list[str]
    price: float
    security_deposit: float | None
    cleaning_fee: float | None
    extra_people: float
    guests_included: int
    images: dict
    host: Host
    address: Address
    availability: dict
    review_scores: dict
    reviews: list[Review]
    text_embeddings: list[float]

![](img/7123cecf9341a47c17accb2610c5b124_98.png)

# 将数据转换为Listing对象列表
records = df.to_dict('records')
listings = [Listing(**record).dict() for record in records]
print(listings[0])  # 查看第一个房源
```

![](img/7123cecf9341a47c17accb2610c5b124_100.png)

#### 4. 连接MongoDB数据库

![](img/7123cecf9341a47c17accb2610c5b124_102.png)

![](img/7123cecf9341a47c17accb2610c5b124_104.png)

```python
# 设置数据库和集合名称
DATABASE_NAME = "airbnb_dataset"
COLLECTION_NAME = "listings_reviews"

![](img/7123cecf9341a47c17accb2610c5b124_106.png)

def get_mongo_client(mongo_uri):
    """创建并返回MongoDB客户端"""
    return MongoClient(mongo_uri, appname="rag-course")

![](img/7123cecf9341a47c17accb2610c5b124_108.png)

# 从环境变量获取URI并连接
MONGO_URI = os.getenv("MONGODB_URI")
client = get_mongo_client(MONGO_URI)
db = client[DATABASE_NAME]
collection = db[COLLECTION_NAME]

# 清理现有集合（首次运行可跳过）
collection.delete_many({})
```

![](img/7123cecf9341a47c17accb2610c5b124_110.png)

#### 5. 数据摄取

![](img/7123cecf9341a47c17accb2610c5b124_112.png)

```python
# 将数据插入MongoDB集合
result = collection.insert_many(listings)
print(f"插入了 {len(result.inserted_ids)} 个文档")
```

#### 6. 创建向量搜索索引

![](img/7123cecf9341a47c17accb2610c5b124_114.png)

```python
# 定义向量搜索索引
text_embedding_field_name = "text_embeddings"
vector_search_index_name_text = "vector_index_text"

vector_search_model = SearchIndexModel(
    definition={
        "fields": [
            {
                "type": "vector",
                "path": text_embedding_field_name,
                "numDimensions": 1536,  # OpenAI ada-002的维度
                "similarity": "cosine",
            }
        ]
    },
    name=vector_search_index_name_text,
)

![](img/7123cecf9341a47c17accb2610c5b124_116.png)

# 创建索引
collection.create_search_index(vector_search_model)
print("向量搜索索引创建成功。等待索引初始化...")
time.sleep(60)  # 等待索引就绪
```

![](img/7123cecf9341a47c17accb2610c5b124_118.png)

![](img/7123cecf9341a47c17accb2610c5b124_119.png)

#### 7. 定义嵌入生成函数

![](img/7123cecf9341a47c17accb2610c5b124_121.png)

![](img/7123cecf9341a47c17accb2610c5b124_123.png)

```python
# 初始化OpenAI客户端
openai_client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

![](img/7123cecf9341a47c17accb2610c5b124_125.png)

def get_embedding(text):
    """使用OpenAI生成文本嵌入"""
    if not isinstance(text, str):
        raise ValueError("输入必须是字符串")
    
    response = openai_client.embeddings.create(
        model="text-embedding-ada-002",
        input=text
    )
    return response.data[0].embedding
```

![](img/7123cecf9341a47c17accb2610c5b124_127.png)

#### 8. 构建向量搜索函数

```python
def vector_search(query, db, collection, vector_index_name="vector_index_text"):
    """执行向量搜索"""
    # 将查询转换为嵌入
    query_embedding = get_embedding(query)
    
    # 定义向量搜索阶段
    vector_search_stage = {
        "$vectorSearch": {
            "index": vector_index_name,
            "queryVector": query_embedding,
            "path": text_embedding_field_name,
            "numCandidates": 100,
            "limit": 20
        }
    }
    
    # 构建聚合管道
    pipeline = [vector_search_stage]
    
    # 执行搜索
    start_time = time.time()
    results = list(collection.aggregate(pipeline))
    search_time = (time.time() - start_time) * 1000  # 转换为毫秒
    
    print(f"向量搜索耗时: {search_time:.2f} 毫秒")
    return results
```

![](img/7123cecf9341a47c17accb2610c5b124_129.png)

#### 9. 处理用户查询与RAG集成

```python
# 定义搜索结果模型
class SearchResultItem(BaseModel):
    name: str
    accommodates: int
    address: dict
    summary: str | None
    price: float
    property_type: str

![](img/7123cecf9341a47c17accb2610c5b124_131.png)

def handle_user_query(query, db, collection):
    """处理用户查询：RAG全流程"""
    # 1. 向量搜索获取相关知识
    knowledge = vector_search(query, db, collection)
    
    if not knowledge:
        return "未找到相关房源信息。"
    
    # 2. 格式化搜索结果
    search_results = [SearchResultItem(**doc) for doc in knowledge]
    results_df = pd.DataFrame([item.dict() for item in search_results])
    
    # 3. 构建LLM提示词
    context = results_df.to_string()
    prompt = f"""
    你是一个Airbnb房源推荐系统。根据以下房源信息，回答用户的问题。
    
    用户查询: {query}
    
    相关房源信息:
    {context}
    
    请推荐最符合用户需求的房源，并说明推荐理由。
    如果信息不足，请如实告知。
    """
    
    # 4. 调用LLM生成回复
    response = openai_client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "你是一个专业的Airbnb旅行顾问。"},
            {"role": "user", "content": prompt}
        ],
        temperature=0.7
    )
    
    system_response = response.choices[0].message.content
    
    # 5. 打印结果
    print("="*50)
    print(f"用户查询: {query}")
    print(f"系统响应: {system_response}")
    print("="*50)
    print("\n用于生成回复的房源信息:")
    print(results_df[['name', 'accommodates', 'property_type', 'price']].to_string())
    
    return system_response
```

![](img/7123cecf9341a47c17accb2610c5b124_133.png)

#### 10. 运行系统

```python
# 测试查询
query = "推荐一个温暖友好、离餐馆不太远的Airbnb房源"
response = handle_user_query(query, db, collection)
```

![](img/7123cecf9341a47c17accb2610c5b124_135.png)

## 总结

![](img/7123cecf9341a47c17accb2610c5b124_137.png)

在本节课中，我们一起学习了：
1.  **向量搜索原理**：理解了如何将数据转换为向量嵌入，并通过计算向量间距离实现语义搜索。
2.  **RAG系统架构**：掌握了检索增强生成如何结合向量搜索与大型语言模型，提供基于上下文的精准回答。
3.  **MongoDB向量数据库**：学会了使用MongoDB存储和索引向量数据，并执行高效的向量搜索查询。
4.  **Pydantic数据验证**：使用Pydantic库确保数据质量，为AI应用提供可靠的数据基础。
5.  **实战项目**：构建了一个完整的Airbnb房源RAG推荐系统，实现了从数据加载、向量搜索到LLM集成的全流程。

![](img/7123cecf9341a47c17accb2610c5b124_139.png)

通过本节课，你已经掌握了构建基础RAG系统的核心技能。在下一节课中，我们将探索如何在向量搜索操作中添加过滤（包括前置过滤和后置过滤），使推荐系统更加精准和高效。