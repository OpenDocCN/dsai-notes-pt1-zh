# 007：L6 多模态推荐系统 🎬

![](img/c6ad1ac84c9117fdb1467145b6182a48_0.png)

在本节课中，我们将学习如何构建一个多模态推荐系统。我们将使用 Weaviate 来创建一个多向量推荐引擎，该系统能够通过搜索项目的不同模态表示来推荐相关项目。

![](img/c6ad1ac84c9117fdb1467145b6182a48_2.png)

## 搜索与推荐的区别 🔍

首先，我们来思考搜索与推荐有何不同。搜索是**客观**的。它不依赖于搜索者是谁。只要结果在语义上与查询匹配，搜索就可以被认为是成功的，无需为用户个性化结果。例如，如果用户搜索“篮球球衣”，返回以下任何一件球衣都是正确的。



![](img/c6ad1ac84c9117fdb1467145b6182a48_4.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_5.png)

推荐则相反，它是**主观**的。它非常依赖于提问者是谁。假设你的商店销售篮球球衣，一位顾客来询问球衣。你需要了解这位顾客的更多信息才能提供正确的回应。例如，如果来的是湖人队球迷，那么你能推荐的最佳球衣就是这些湖人队球衣。另一方面，如果来的是波士顿凯尔特人队球迷，那么最佳推荐就是凯尔特人队球衣。



## 多模态数据的优势 📊

多模态数据为个性化推荐系统提供了一个绝佳的解决方案，它允许你利用所有可用的模态来捕捉用户的兴趣。让我们看一个例子。如果我让你描述你心目中的完美汉堡，你可以用文字写下描述，也可以给我看一张图片，或者告诉我关于这个汉堡的营养信息。



![](img/c6ad1ac84c9117fdb1467145b6182a48_7.png)

我可以利用关于你完美汉堡的所有这些信息来识别你的好恶。我还可以利用这些独立的模态来个性化推荐。为此，你可以将所有信息捕获到一个向量，甚至是多向量表示中，其中每个向量都来自一个专门的模型，并允许你搜索该模态。这很重要，因为有些人可能因为产品的外观而购买，另一些人可能因为详细的描述而购买，而其他人则可能基于营养信息来选择。



## 实践环节：构建多模态电影推荐系统 🛠️

现在，让我们在实践中看看这一切。在本实验中，你将加载一个嵌入了多模态电影数据的数据集，使用两个独立的机器学习模型，然后在不同的向量空间中运行文本和多模态查询。

### 环境设置

![](img/c6ad1ac84c9117fdb1467145b6182a48_9.png)

与之前的所有课程一样，你将从忽略所有不必要的警告开始，并加载必要的 API 密钥。这次我们将使用两个不同的密钥：一个用于多模态嵌入（与我们之前使用的模型相同），另一个用于 OpenAI 的文本嵌入。然后像这样加载它们。

```python
import weaviate
import openai
# 加载 API 密钥
openai.api_key = "your_openai_api_key"
weaviate_api_key = "your_weaviate_api_key"
```

接下来，连接到 Weaviate 的嵌入式实例。这次我们将使用两个不同的模块：一个用于文本，一个用于多模态。我们必须将这两个 API 密钥作为请求头的一部分传递。

```python
client = weaviate.Client(
    embedded_options=weaviate.embedded.EmbeddedOptions(),
    additional_headers={
        "X-OpenAI-Api-Key": openai.api_key,
        "X-MultiModal-Api-Key": weaviate_api_key
    }
)
```

运行此代码后，你将连接到一个嵌入式的 Weaviate 实例。

![](img/c6ad1ac84c9117fdb1467145b6182a48_11.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_13.png)

### 创建集合

现在你已连接到 Weaviate，接下来将创建一个名为 `Movie` 的新集合。为了帮助你，这里已经预定义了一些属性，我们可以预期有 `title`、`overview`、`vote_average`、`release_year` 等。还有一个非常重要的属性：`poster_path`，用于存放图片路径。

因为这是一个多向量集合，首先需要为你的文本嵌入配置一个命名向量。你将这个向量空间称为 `text_vector`。具体来说，对于这个命名向量，你希望对 `title` 和 `overview` 进行向量化，并忽略所有其他属性。

对于第二个向量空间，你将添加第二个命名向量，这将是多模态向量。我们希望在 `poster` 字段内的图像字段上运行它，同时指定我们需要运行的特定模型和所有附加信息。

为了澄清，这个命名向量空间将被称为 `poster_vector`。

![](img/c6ad1ac84c9117fdb1467145b6182a48_15.png)

```python
# 定义集合模式
schema = {
    "classes": [{
        "class": "Movie",
        "properties": [
            {"name": "title", "dataType": ["string"]},
            {"name": "overview", "dataType": ["text"]},
            {"name": "vote_average", "dataType": ["number"]},
            {"name": "release_year", "dataType": ["int"]},
            {"name": "poster_path", "dataType": ["string"]}
        ],
        "vectorizers": [
            {
                "name": "text_vector",
                "vectorizer": "text2vec-openai",
                "properties": ["title", "overview"]
            },
            {
                "name": "poster_vector",
                "vectorizer": "multi2vec-clip",
                "properties": ["poster_path"]
            }
        ]
    }]
}
client.schema.create(schema)
```

![](img/c6ad1ac84c9117fdb1467145b6182a48_17.png)

执行此操作将为你创建一个新集合。

如果需要重新运行并再次创建此集合，可以取消注释下面这行代码，它将为你删除集合，然后你可以重新创建它。请注意，如果运行此代码，你也会丢失当时该集合中的所有数据。

```python
# client.schema.delete_class("Movie") # 谨慎使用
```

### 导入数据

现在你有了一个准备好的集合，可以开始导入数据了。在这个项目中，有一个 `movies_data.json` 文件，其中列出了 20 部不同的电影及其海报。让我们看看里面有什么。你可以看到有一个指向海报的链接，在这里称为 `poster_path`。如果我滚动浏览列，你可以看到你已经拥有了所有必要的数据，如原始标题、概述、流行度，以及非常重要的海报路径等。

接下来，你需要加载一个辅助函数，该函数将获取文件路径并将其转换为 base64 表示，这你在之前的课程中已经做过了。

```python
import base64
def image_to_base64(file_path):
    with open(file_path, "rb") as image_file:
        return base64.b64encode(image_file.read()).decode('utf-8')
```

现在你拥有了所有构建模块，是时候导入数据了。首先需要做的事情之一是获取 `movies` 集合，然后在该集合上创建一个批处理对象，速率限制为 20，这基本上意味着你希望每分钟只上传 20 张图片，这有助于我们应对服务的速率限制。然后，使用数据框，我们可以遍历预加载数据集中的所有电影（只有 20 部，这也符合我们设定的速率限制）。

这里的这个语句非常有用，因为使用在顶部导入的 `generate_uuid`，我们可以提供一个电影 ID，如果该电影 ID 已经存在于我们的数据集中，那么我们可以跳过它。因此，即使你重新运行这个单元格，也不会导入同一部电影两次，这非常有用。

接下来，当你遍历数据框中的所有对象时，你需要解析海报文件的路径（该路径位于 `poster` 文件夹内），然后逐一将其转换为 base64 表示。

然后，利用这些信息构建一个电影对象，该对象将包含标题、概述、平均评分、ID、海报路径和海报的 base64 编码。

最后，你现在需要做的就是调用批处理对象的 `add` 方法，传递电影对象作为属性，然后从电影 ID 生成一个 UUID（该 ID 在早期阶段用于验证我们没有重复导入相同的内容）。运行此代码，导入我们所有的图片。

![](img/c6ad1ac84c9117fdb1467145b6182a48_19.png)

```python
import pandas as pd
import uuid

# 加载数据
df = pd.read_json('movies_data.json')

# 获取集合
movies = client.collections.get("Movie")

# 创建批处理对象
with movies.batch.dynamic() as batch:
    batch.batch_size = 20  # 速率限制
    for index, row in df.iterrows():
        # 检查是否已存在
        existing = movies.query.fetch_object_by_id(str(row['id']))
        if existing:
            continue

        # 转换图片为 base64
        poster_b64 = image_to_base64(row['poster_path'])

        # 构建数据对象
        movie_object = {
            "title": row['title'],
            "overview": row['overview'],
            "vote_average": row['vote_average'],
            "release_year": row['release_year'],
            "poster_path": row['poster_path'],
            "poster_b64": poster_b64
        }

        # 添加到批处理
        batch.add_object(
            properties=movie_object,
            uuid=uuid.uuid5(uuid.NAMESPACE_DNS, str(row['id']))
        )
```

现在正在发生的是，这 20 部电影已被发送到两个不同的向量化器。在这里，我们将海报发送到多模态模型进行向量化，同时标题和描述也被发送到 OpenAI 的文本模型，对标题和描述进行向量化。这大约需要一分钟。

之后，你应该可以开始查询这个数据集了。作为最后一步，你需要检查导入是否顺利。你可以通过进入 `movies` 批处理对象并查找失败的对象（如果有的话）来做到这一点。因为这是一个列表，你可以打印它，或者打印第一个。如果没有错误，我们应该会得到一条“导入完成，无错误”的快乐消息，这很好。

### 执行查询

![](img/c6ad1ac84c9117fdb1467145b6182a48_21.png)

现在，是时候进入有趣的部分了，你可以在数据上运行一些查询。让我们搜索一些关于可爱宠物的电影。这个查询与你之前运行的非常相似，但是，因为我们使用的是多向量空间，集合配置了两个不同的向量，你需要指定要搜索哪个向量空间。对于这个查询，你希望跨 `text_vector` 空间进行搜索。

```python
# 在文本向量空间搜索
response = movies.query.near_text(
    query="lovable cute pets",
    limit=3,
    target_vector="text_vector"
)

![](img/c6ad1ac84c9117fdb1467145b6182a48_23.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_24.png)

for obj in response.objects:
    print(f"Title: {obj.properties['title']}")
    print(f"Overview: {obj.properties['overview']}")
    # 显示海报（假设有方法显示 base64 图片）
    display_poster(obj.properties['poster_b64'])
```

请注意，这个特定的查询只会尝试根据标题和概述中的内容进行匹配，因为文本向量就是基于这些构建的。运行这个查询。

![](img/c6ad1ac84c9117fdb1467145b6182a48_26.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_27.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_28.png)

你可以看到，我的第一个结果是《101 忠狗》，以及描述和电影海报。我还得到了《精灵鼠小弟》的海报及其标题和描述，第三个回应是《虫虫特工队》，同样附有其描述和电影海报。



![](img/c6ad1ac84c9117fdb1467145b6182a48_30.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_32.png)

现在，让我们在同一个向量空间上尝试另一个查询，但这次让我们寻找“史诗级超级英雄”。

```python
response = movies.query.near_text(
    query="epic superheroes",
    limit=3,
    target_vector="text_vector"
)
```

你可以看到，我们得到了《钢铁侠》、《超人总动员 2》等结果。



![](img/c6ad1ac84c9117fdb1467145b6182a48_34.png)

你现在可能在想，这个查询是基于文本向量嵌入的，但我们仍然在这里得到了海报。是的，确实如此。查询仍然只在标题和概述上运行。然而，我们返回的对象包含有关海报的信息。这就是我们能够显示它们的原因，但海报尚未用于查询。这将在下一步中实现。

![](img/c6ad1ac84c9117fdb1467145b6182a48_36.png)

### 跨海报搜索

要实际跨海报进行搜索，你需要运行类似的查询，但在 `poster_vector` 向量空间上运行。通过运行这个，我们将尝试匹配那些海报反映了“可爱宠物”这一查询的电影。

![](img/c6ad1ac84c9117fdb1467145b6182a48_38.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_39.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_40.png)

```python
# 在多模态向量空间搜索
response = movies.query.near_text(
    query="lovable cute pets",
    limit=3,
    target_vector="poster_vector"
)
```

你可以看到，结果非常相似，但并不完全相同。然而，它们确实与原始查询非常匹配，所以“可爱宠物”的主题在我们返回的这三张海报中都清晰可见。



![](img/c6ad1ac84c9117fdb1467145b6182a48_42.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_43.png)

让我们在这里尝试另一个查询，再次在 `poster_vector` 空间搜索，但这次你想寻找“史诗级超级英雄”，以便与在文本向量空间中搜索时得到的结果进行比较。



同样，结果非常相似，但又不完全相同，因为我们在海报中得到的信息与电影标题和描述中的信息略有不同。如果你有更多的数据，两者之间的差异可能会更大。但在我们拥有的这个只有 20 张不同图片的数据集中，很可能会有重复。但如果你有成千上万张图片，那么两者可能都匹配得很好，但不一定会返回完全相同的对象。

### 使用图片进行搜索

你可以对数据做的另一件事是使用图片在 `poster_vector` 空间上跨海报进行搜索。让我们从这张“恐怖场景”的图片开始。



![](img/c6ad1ac84c9117fdb1467145b6182a48_45.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_46.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_47.png)

要使用这张恐怖图片进行搜索，你需要调用 `near_image` 方法，将这张恐怖图片转换为 base64，并希望在 `poster_vector` 空间上进行搜索。完成后，让我们只显示标题和海报，使输出更清晰，然后执行此操作。



你得到的是恐怖电影的海报，比如《圣诞夜惊魂》、《女巫也疯狂》和《惊声尖叫》。



![](img/c6ad1ac84c9117fdb1467145b6182a48_49.png)

作为最后一个例子，让我们尝试根据这张图片搜索一些超级英雄。



![](img/c6ad1ac84c9117fdb1467145b6182a48_51.png)

搜索非常相似，但这次只是使用不同的图片。我们在这里得到的是《蝙蝠侠大战超人：正义黎明》，还有《钢铁侠》。



![](img/c6ad1ac84c9117fdb1467145b6182a48_53.png)

好吧，我们得承认，这只追逐坚果的松鼠绝对是我心目中的超级英雄。



![](img/c6ad1ac84c9117fdb1467145b6182a48_54.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_55.png)

![](img/c6ad1ac84c9117fdb1467145b6182a48_57.png)

### 清理工作

![](img/c6ad1ac84c9117fdb1467145b6182a48_59.png)

现在你已经完成了所有查询，可以关闭客户端并完成本课。

```python
client.close()
```

## 总结 📝

![](img/c6ad1ac84c9117fdb1467145b6182a48_61.png)

本节课到此结束，本课程也告一段落。在本节课中，你学习了如何使用多向量空间，然后跨两个不同的向量空间进行搜索：一个是文本向量空间，另一个是多模态空间，并从不同的角度理解数据。正如你所看到的，不同向量空间之间的一些结果可能是相似的，但每个向量空间和每个模型都可能给你不同类型的结果，这可能非常强大，也非常有用。我相信你将会有很多想法和用例，来利用这一点并充分发挥其潜力。