# 034：2——文档加载 📄

![](img/9ebbe7324ba67e5575b09773683b088a_0.png)

在本节课中，我们将学习 LangChain 中一个基础且关键的环节：**文档加载**。为了创建一个能与你的数据进行交流的应用程序，首先需要将数据加载到可工作的格式中。这就是 LangChain 文档加载器发挥作用的地方。LangChain 提供了超过八十种不同类型的文档加载器，本节课我们将介绍其中最重要的一些，帮助你熟悉这个概念。

## 概述

文档加载器负责处理访问和转换数据的具体细节，将各种不同格式和来源的数据转换为标准化的格式。我们可以从不同地方加载数据，例如网站、数据库、YouTube 视频等。这些数据可能以不同的类型出现，如 PDF、HTML、JSON 等。文档加载器的核心目的就是从这些多样化的数据源中提取信息，并将它们加载到一个标准的文档对象中，该对象由**内容**和相关的**元数据**组成。

![](img/9ebbe7324ba67e5575b09773683b088a_2.png)

## 文档加载器的分类 📂

LangChain 中有很多不同类型的文档加载器。我们没有时间覆盖全部，但以下是对其大致分类的介绍。

![](img/9ebbe7324ba67e5575b09773683b088a_4.png)

以下是主要的文档加载器分类：

![](img/9ebbe7324ba67e5575b09773683b088a_6.png)

*   **处理非结构化数据的加载器（来自公共数据源）**：例如，用于加载来自 YouTube、Twitter、Hacker News 等平台的文本内容。
*   **处理非结构化数据的加载器（来自专有数据源）**：例如，用于加载来自 Figma、Notion 等公司或个人拥有的专有数据。
*   **处理结构化数据的加载器**：用于加载表格格式的数据。即使这些数据主要存在于单元格或行中，你仍然可能希望对其进行问答或语义搜索。这类数据源包括 Airbyte、Stripe、Airtable 等。

## 实际使用文档加载器 🛠️

![](img/9ebbe7324ba67e5575b09773683b088a_8.png)

现在，让我们进入有趣的部分，实际使用文档加载器。首先，我们需要加载一些环境变量，例如 OpenAI API 密钥。

### 1. 加载 PDF 文档

我们首先处理的文档类型是 PDF。让我们从 LangChain 中导入相关的文档加载器 `PyPDFLoader`。我们已经将许多 PDF 文件加载到工作空间的 `documents` 文件夹中。

![](img/9ebbe7324ba67e5575b09773683b088a_10.png)

```python
from langchain.document_loaders import PyPDFLoader
```

因此，让我们选择一份 PDF 并将其放入加载器中。

```python
loader = PyPDFLoader("documents/example.pdf")
```

![](img/9ebbe7324ba67e5575b09773683b088a_12.png)

现在，让我们通过调用 `load` 方法加载文档。

```python
docs = loader.load()
```

让我们看看实际上加载了什么。默认情况下，这将加载一个文档列表。在这个例子中，这个 PDF 有二十二个不同页面，每个页面都是一个独立的文档。让我们查看第一个文档的内容。

![](img/9ebbe7324ba67e5575b09773683b088a_14.png)

```python
print(len(docs))  # 输出文档数量，例如 22
print(docs[0].page_content[:500])  # 打印第一页内容的前500个字符
```

![](img/9ebbe7324ba67e5575b09773683b088a_16.png)

另一个非常重要的信息是与每个文档相关的元数据，这可以通过 `metadata` 属性访问。

```python
print(docs[0].metadata)
```

你可以在这里看到有两个不同的部分：一个是 `source` 信息（PDF 文件名），另一个是 `page` 字段（对应于 PDF 的页码）。

![](img/9ebbe7324ba67e5575b09773683b088a_18.png)

### 2. 加载 YouTube 视频转录

接下来我们将查看的文档加载器，用于从 YouTube 加载内容。YouTube 上有很多有趣的内容，因此很多人使用这种文档加载器来对他们最喜欢的视频、讲座等提问。

我们将在这里导入一些不同的模块。关键部分是 `YoutubeAudioLoader`（从 YouTube 视频加载音频文件）和 `OpenAIWhisperParser`（使用 OpenAI 的 Whisper 语音转文本模型将音频转换为文本）。

![](img/9ebbe7324ba67e5575b09773683b088a_20.png)

![](img/9ebbe7324ba67e5575b09773683b088a_22.png)

```python
from langchain.document_loaders.generic import GenericLoader
from langchain.document_loaders.parsers import OpenAIWhisperParser
from langchain.document_loaders.blob_loaders.youtube_audio import YoutubeAudioLoader
```

我们现在可以指定一个 URL 和一个保存音频文件的目录，然后创建一个结合了 `YoutubeAudioLoader` 和 `OpenAIWhisperParser` 的通用加载器。

```python
url = "https://www.youtube.com/watch?v=example_video_id"
save_dir = "./youtube_audio"
loader = GenericLoader(YoutubeAudioLoader([url], save_dir), OpenAIWhisperParser())
```

![](img/9ebbe7324ba67e5575b09773683b088a_24.png)

然后我们可以调用 `loader.load()` 来加载与这个 YouTube 视频对应的文档。这个过程可能需要几分钟。

```python
docs = loader.load()
```

![](img/9ebbe7324ba67e5575b09773683b088a_26.png)

加载完成后，我们可以查看加载的页面内容。

![](img/9ebbe7324ba67e5575b09773683b088a_28.png)

```python
print(docs[0].page_content[:1000])  # 打印转录文本的前1000个字符
```

![](img/9ebbe7324ba67e5575b09773683b088a_30.png)

这是一个很好的时机，你可以暂停一下，选择你最喜欢的 YouTube 视频，看看这个转录功能是否对你有用。

### 3. 加载网页内容

![](img/9ebbe7324ba67e5575b09773683b088a_32.png)

我们接下来要讨论如何加载来自互联网 URL 的文档。互联网上有很多非常棒的教育内容，如果能与这些内容聊天会很酷。我们将通过导入 LangChain 中的 `WebBaseLoader` 来实现这一点。

![](img/9ebbe7324ba67e5575b09773683b088a_34.png)

```python
from langchain.document_loaders import WebBaseLoader
```

![](img/9ebbe7324ba67e5575b09773683b088a_36.png)

然后我们可以选择任何 URL。这里我们选择一个喜欢的 URL，例如一个 GitHub 页面上的 Markdown 文件，并创建一个加载器。

```python
url = "https://raw.githubusercontent.com/some/repo/main/README.md"
loader = WebBaseLoader(url)
```

![](img/9ebbe7324ba67e5575b09773683b088a_38.png)

接下来我们可以调用 `loader.load()`，然后查看页面的内容。

![](img/9ebbe7324ba67e5575b09773683b088a_40.png)

```python
docs = loader.load()
print(docs[0].page_content[:1000])
```

在这里你会注意到内容中可能有很多空白，紧随其后是一些初始文本。这是一个很好的例子，说明了为什么在实际工作中需要对信息进行后处理，以便将其转换为可用的格式。

### 4. 加载 Notion 数据

![](img/9ebbe7324ba67e5575b09773683b088a_42.png)

最后，我们将介绍如何从 Notion 加载数据。Notion 是一个非常流行的个人和公司数据存储库，很多人已经创建了与 Notion 数据库对话的聊天机器人。

你将看到如何从 Notion 数据库中导出数据的指示。通过这种方式，我们可以将其加载到 LangChain 中。一旦我们以那种格式获得了数据，就可以使用 `NotionDirectoryLoader` 来加载数据并获取我们可以处理的文档。

![](img/9ebbe7324ba67e5575b09773683b088a_44.png)

```python
from langchain.document_loaders import NotionDirectoryLoader
```

![](img/9ebbe7324ba67e5575b09773683b088a_46.png)

假设你已经从 Notion 导出了一个目录 `notion_data`。

```python
loader = NotionDirectoryLoader("notion_data")
docs = loader.load()
```

![](img/9ebbe7324ba67e5575b09773683b088a_48.png)

如果我们查看这个内容，可以看到它是 Markdown 格式。这个 Notion 文档可能来自某个员工手册。相信正在学习的很多人已经使用了 Notion，并且有一些他们想要聊天的数据库。因此，这是一个将数据导出、带到 LangChain 中并开始处理的绝佳机会。

## 总结

本节课中，我们一起学习了 LangChain 的**文档加载**功能。我们介绍了如何从各种来源（如 PDF、YouTube、网页和 Notion）加载数据，并将其转换为标准化的文档接口。然而，这些加载后的文档通常仍然比较大。

![](img/9ebbe7324ba67e5575b09773683b088a_50.png)

因此，在下一节中，我们将讨论如何将它们分割成更小的部分。这一点至关重要，因为在进行检索增强生成（RAG）时，你需要检索出与主题最相关的内容片段，而不是整个文档。这也是一个更好地思考数据来源的机会。虽然我们目前没有覆盖所有的数据源加载器，但你仍然可以自行探索，甚至可以为 LangChain 项目提交 Pull Request 来贡献新的加载器。