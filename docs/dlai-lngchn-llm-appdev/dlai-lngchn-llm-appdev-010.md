# 010：文档加载

![](img/45fbcc5f08d4b53978152b089a1da51e_0.png)

在本节课中，我们将学习LangChain中一个核心且基础的概念：文档加载。为了创建一个能与您的数据对话的应用程序，首先必须将数据加载到可用的格式中。LangChain提供了80多种不同类型的文档加载器来处理这一任务。本节课将介绍其中最重要的一些加载器，帮助您理解其基本概念和工作原理。

## 概述：什么是文档加载器？

文档加载器负责处理访问和转换数据的细节，将各种不同格式和来源的数据转换为标准化的格式。这些数据来源广泛，包括网站、数据库、YouTube视频等。数据格式也多种多样，例如PDF、HTML、JSON等。

文档加载器的核心目的是获取各种数据源，并将它们加载到标准的**文档对象**中。每个文档对象由**内容**和相关的**元数据**组成。

![](img/45fbcc5f08d4b53978152b089a1da51e_2.png)

## 文档加载器的分类

LangChain中有许多不同类型的文档加载器。虽然没有时间全部介绍，但我们可以将其大致分为几类。

![](img/45fbcc5f08d4b53978152b089a1da51e_4.png)

以下是主要的分类：

![](img/45fbcc5f08d4b53978152b089a1da51e_6.png)

*   **处理非结构化公共数据**：用于加载来自公共数据源的文本文件，例如YouTube、Twitter、Hacker News等。
*   **处理非结构化专有数据**：用于加载您或您的公司拥有的专有数据源，例如Figma、Notion等。
*   **处理结构化数据**：用于加载表格格式的数据，这些数据可能在单元格或行中包含文本信息，适用于问答或语义搜索任务。相关来源包括Airbyte、Stripe、Airtable等。

![](img/45fbcc5f08d4b53978152b089a1da51e_8.png)

## 实践：使用文档加载器

上一节我们介绍了文档加载器的概念和分类，本节中我们来看看如何实际使用它们。首先，我们需要加载一些环境变量，例如OpenAI API密钥。

### 1. 加载PDF文档

![](img/45fbcc5f08d4b53978152b089a1da51e_10.png)

我们要处理的第一类文档是PDF。让我们从LangChain导入相关的文档加载器。

```python
from langchain.document_loaders import PyPDFLoader
```

我们已将一些PDF文件放入工作区的`documents`文件夹中。现在，让我们选择一个PDF文件，并使用加载器加载它。

![](img/45fbcc5f08d4b53978152b089a1da51e_12.png)

```python
loader = PyPDFLoader("documents/example.pdf")
```

接下来，我们通过调用`load`方法来加载文档，并查看加载的内容。

```python
docs = loader.load()
print(len(docs))  # 查看加载了多少个文档（PDF页面）
```

默认情况下，这将加载一个文档列表。对于PDF，每个页面通常是一个独立的文档。让我们查看第一个文档的组成。

![](img/45fbcc5f08d4b53978152b089a1da51e_14.png)

```python
first_doc = docs[0]
print(first_doc.page_content[:500])  # 打印前500个字符的内容
```

![](img/45fbcc5f08d4b53978152b089a1da51e_16.png)

另一个重要的信息是与每个文档关联的元数据，可以通过`.metadata`属性访问。

```python
print(first_doc.metadata)
```

您会看到元数据中包含`source`（来源文件）和`page`（对应的PDF页码）等信息。

![](img/45fbcc5f08d4b53978152b089a1da51e_18.png)

### 2. 从YouTube加载音频并转录

我们将要看到的下一种文档加载器可以从YouTube加载内容。YouTube上有大量有趣的内容，许多人使用这个加载器来向他们喜欢的视频或讲座提问。

以下是实现步骤：

![](img/45fbcc5f08d4b53978152b089a1da51e_20.png)

```python
# 导入必要的组件
from langchain.document_loaders.generic import GenericLoader
from langchain.document_loaders.parsers import OpenAIWhisperParser
from langchain.document_loaders.blob_loaders.youtube_audio import YoutubeAudioLoader
```

![](img/45fbcc5f08d4b53978152b089a1da51e_22.png)

关键部分是`YoutubeAudioLoader`（用于从YouTube视频加载音频文件）和`OpenAIWhisperParser`（使用OpenAI的Whisper语音转文本模型，将音频转换为文本）。

现在，我们可以指定一个YouTube视频的URL，并创建加载器。

![](img/45fbcc5f08d4b53978152b089a1da51e_24.png)

```python
url = "https://www.youtube.com/watch?v=example_video_id"
save_dir = "tmp/youtube_audio"  # 指定保存音频文件的目录

![](img/45fbcc5f08d4b53978152b089a1da51e_26.png)

loader = GenericLoader(
    YoutubeAudioLoader([url], save_dir),
    OpenAIWhisperParser()
)
docs = loader.load()  # 加载并转录，此过程可能需要几分钟
```

![](img/45fbcc5f08d4b53978152b089a1da51e_28.png)

加载完成后，我们可以查看转录的文本内容。

```python
print(docs[0].page_content[:1000])  # 打印视频第一部分的内容
```

![](img/45fbcc5f08d4b53978152b089a1da51e_30.png)

### 3. 从网页URL加载内容

互联网上有很多优秀的教育内容。如果能与这些内容“对话”，将会非常酷。接下来，我们学习如何从网页URL加载数据。

![](img/45fbcc5f08d4b53978152b089a1da51e_32.png)

我们将通过从LangChain导入基于Web的加载器来实现。

![](img/45fbcc5f08d4b53978152b089a1da51e_34.png)

```python
from langchain.document_loaders import WebBaseLoader
```

然后，我们可以选择任何URL（例如一个GitHub页面的Markdown文件），并为它创建一个加载器。

![](img/45fbcc5f08d4b53978152b089a1da51e_36.png)

```python
url = "https://raw.githubusercontent.com/langchain-ai/langchain/master/README.md"
loader = WebBaseLoader(url)
docs = loader.load()
```

![](img/45fbcc5f08d4b53978152b089a1da51e_38.png)

现在，我们可以查看加载的页面内容。

![](img/45fbcc5f08d4b53978152b089a1da51e_40.png)

```python
print(docs[0].page_content[:1500])
```

您可能会注意到内容中包含很多空白和原始文本。这是一个很好的例子，说明了为什么通常需要对加载的信息进行后处理，以使其成为更易于处理的格式。

![](img/45fbcc5f08d4b53978152b089a1da51e_42.png)

### 4. 从Notion加载数据

![](img/45fbcc5f08d4b53978152b089a1da51e_44.png)

Notion是一个非常流行的个人和公司数据存储工具。许多人希望创建能与自己的Notion数据库对话的聊天机器人。

您需要先将数据从Notion数据库导出为特定的格式（如Markdown），然后才能将其加载到LangChain中。

一旦有了导出文件，就可以使用`NotionDirectoryLoader`来加载数据。

![](img/45fbcc5f08d4b53978152b089a1da51e_46.png)

```python
from langchain.document_loaders import NotionDirectoryLoader

![](img/45fbcc5f08d4b53978152b089a1da51e_48.png)

loader = NotionDirectoryLoader("path/to/notion/export/directory")
docs = loader.load()
```

查看加载的内容，您会发现它是Markdown格式的。

![](img/45fbcc5f08d4b53978152b089a1da51e_50.png)

```python
print(docs[0].page_content[:1000])
```

![](img/45fbcc5f08d4b53978152b089a1da51e_52.png)

这是一个很好的机会，您可以导出自己的Notion数据，并将其带入LangChain中开始工作。

## 总结与展望

本节课中，我们一起学习了如何使用LangChain的文档加载器从各种来源（PDF、YouTube、网页、Notion）加载数据，并将其转换为标准化的文档对象。

然而，这些加载后的文档通常仍然比较大。在下一节中，我们将学习如何将这些大文档分割成更小的“块”。这一步至关重要，因为在执行检索增强生成时，我们通常只需要检索与问题最相关的内容片段，而不是整个文档。这有助于提高检索的效率和准确性。

![](img/45fbcc5f08d4b53978152b089a1da51e_54.png)

同时，这也是一个思考数据来源的好机会。LangChain目前可能还没有覆盖您需要的所有数据源，但社区在不断扩展，也许您可以探索甚至贡献新的加载器。