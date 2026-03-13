# 035：3——文档分割 📄✂️

![](img/998abcc69f89944611c944f1c443e931_0.png)

![](img/998abcc69f89944611c944f1c443e931_2.png)

![](img/998abcc69f89944611c944f1c443e931_3.png)

## 概述
在本节课中，我们将要学习文档分割。这是将加载好的文档拆分成更小、更易于管理的片段的关键步骤。虽然听起来简单，但其中有许多细节会直接影响后续检索与生成任务的效果。

![](img/998abcc69f89944611c944f1c443e931_5.png)

上一节我们介绍了如何将文档加载为标准格式，本节中我们来看看如何对它们进行有效的分割。

![](img/998abcc69f89944611c944f1c443e931_7.png)

![](img/998abcc69f89944611c944f1c443e931_9.png)

## 为什么文档分割很重要？
文档分割发生在数据加载之后、存入向量数据库之前。一个简单的做法是按固定字符长度分割，但这可能导致语义信息被割裂。

![](img/998abcc69f89944611c944f1c443e931_11.png)

![](img/998abcc69f89944611c944f1c443e931_13.png)

以下是分割不当导致问题的示例：
假设我们有一个关于丰田卡罗拉的句子：“丰田卡罗拉是一款经济型轿车，其油耗为每百公里5升，马力为150匹。”
*   **不当分割**：片段1：“丰田卡罗拉是一款经济型轿车，其油耗为”，片段2：“每百公里5升，马力为150匹。”
*   **下游问题**：当用户提问“卡罗拉的规格是什么？”时，检索系统可能只找到包含不完整信息的片段，从而无法正确回答问题。

因此，分割的目标是**将语义相关的文本组合在一起**，确保信息完整性。

![](img/998abcc69f89944611c944f1c443e931_15.png)

## LangChain中的文本分割器
LangChain中的所有文本分割器都基于两个核心参数进行操作：**片段大小**和**片段重叠**。

![](img/998abcc69f89944611c944f1c443e931_17.png)

*   **片段大小**：指每个文本块的长度。可以用不同方式衡量，例如字符数或标记数。在代码中，我们通过 `length_function` 参数来指定如何测量长度。
    ```python
    # 例如，使用字符数作为长度函数
    length_function = len
    ```
*   **片段重叠**：指相邻两个片段之间共享的文本量。这就像一个滑动的窗口，确保上下文信息在不同片段间有一定的连续性，有助于维持语义连贯性。

![](img/998abcc69f89944611c944f1c443e931_19.png)

![](img/998abcc69f89944611c944f1c443e931_21.png)

文本分割器通常提供两种方法：`create_documents`（处理文本列表）和 `split_documents`（处理文档列表），其内部逻辑相同，只是接口略有不同。

![](img/998abcc69f89944611c944f1c443e931_23.png)

![](img/998abcc69f89944611c944f1c443e931_25.png)

LangChain提供了多种类型的分割器，它们在以下维度上有所不同：
1.  **分割依据**：按哪些字符或规则进行分割。
2.  **长度衡量**：按字符、标记（Token）计数，甚至使用小型模型判断句子边界。
3.  **元数据处理**：如何在分割过程中保留原始文档的元数据，并在适当时为片段添加新的元数据（例如，该片段在原文中的位置）。

![](img/998abcc69f89944611c944f1c443e931_27.png)

![](img/998abcc69f89944611c944f1c443e931_29.png)

## 按文档类型选择分割器
分割策略通常取决于文档类型。这在处理代码等结构化文本时尤为明显。

![](img/998abcc69f89944611c944f1c443e931_31.png)

![](img/998abcc69f89944611c944f1c443e931_33.png)

例如，`RecursiveCharacterTextSplitter` 支持多种编程语言。它会根据语言特性（如Python的缩进、C语言的分号）使用不同的分隔符进行分割，以保持代码逻辑块的完整性。

![](img/998abcc69f89944611c944f1c443e931_35.png)

![](img/998abcc69f89944611c944f1c443e931_37.png)

## 实践：字符分割器
现在，让我们通过代码实践来理解分割过程。首先，我们设置环境并导入必要的库。

![](img/998abcc69f89944611c944f1c443e931_39.png)

![](img/998abcc69f89944611c944f1c443e931_41.png)

以下是两种常见分割器的导入方式：
```python
from langchain.text_splitter import RecursiveCharacterTextSplitter, CharacterTextSplitter
```

![](img/998abcc69f89944611c944f1c443e931_43.png)

我们将先通过一些简单例子了解它们的工作原理。

![](img/998abcc69f89944611c944f1c443e931_45.png)

初始化分割器，设置较小的块大小和重叠以便观察：
```python
chunk_size = 26
chunk_overlap = 4

![](img/998abcc69f89944611c944f1c443e931_47.png)

r_splitter = RecursiveCharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=chunk_overlap)
c_splitter = CharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=chunk_overlap, separator=' ')
```

![](img/998abcc69f89944611c944f1c443e931_49.png)

![](img/998abcc69f89944611c944f1c443e931_51.png)

让我们看看它们如何处理不同的字符串。

![](img/998abcc69f89944611c944f1c443e931_53.png)

**示例1：字母字符串**
*   `RecursiveCharacterTextSplitter` 处理“abcdefghijklmnopqrstuvwxyz”时，由于字符串长度正好是26，所以不会分割。
*   处理更长的字符串时，它会创建有重叠的两个片段。

![](img/998abcc69f89944611c944f1c443e931_55.png)

![](img/998abcc69f89944611c944f1c443e931_57.png)

**示例2：带空格的字符串**
*   处理“a b c d e f g h i j k l m n o p q r s t u v w x y z”时，由于空格占用了长度，会被分割成三个片段，并且重叠部分也包含了空格字符。

![](img/998abcc69f89944611c944f1c443e931_59.png)

![](img/998abcc69f89944611c944f1c443e931_61.png)

**示例3：`CharacterTextSplitter` 的分隔符**
*   默认按换行符 `\n` 分割。如果文本中没有换行符，需要显式设置 `separator` 参数（如设为空格 `‘ ‘`）才能生效。

![](img/998abcc69f89944611c944f1c443e931_63.png)

建议你在此暂停，尝试使用不同的字符串、分隔符、块大小和重叠参数，以加深理解。

![](img/998abcc69f89944611c944f1c443e931_65.png)

## 实践：处理真实文本段落
现在，让我们在更真实的文本段落上尝试。我们有一段约500字符的文本，其中包含段落分隔符（双换行符 `\n\n`）。

![](img/998abcc69f89944611c944f1c443e931_67.png)

![](img/998abcc69f89944611c944f1c443e931_69.png)

![](img/998abcc69f89944611c944f1c443e931_71.png)

定义分割器：
```python
# 字符分割器，按空格分割
c_splitter = CharacterTextSplitter(chunk_size=450, chunk_overlap=0, separator=' ')

![](img/998abcc69f89944611c944f1c443e931_73.png)

# 递归字符分割器，使用默认分隔符列表：["\n\n", "\n", " ", ""]
r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=450,
    chunk_overlap=0,
    separators=["\n\n", "\n", " ", ""] # 显式列出以便理解
)
```

![](img/998abcc69f89944611c944f1c443e931_75.png)

运行分割：
*   `CharacterTextSplitter` 按空格分割，可能导致句子在中间被切断。
*   `RecursiveCharacterTextSplitter` 首先尝试按双换行符 `\n\n` 分割，将文本分成两个完整的段落。由于每个段落都小于450字符，因此不再进一步分割。这通常比在句子中间分割效果更好。

![](img/998abcc69f89944611c944f1c443e931_77.png)

我们还可以添加句号作为分隔符来按句子分割。但需要注意正则表达式的使用，以确保句号被正确保留在句子末尾。
```python
r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=150,
    chunk_overlap=0,
    separators=["\n\n", "\n", "(?<=\. )", " ", ""] # 使用正则表达式在句号后分割
)
```

![](img/998abcc69f89944611c944f1c443e931_79.png)

## 实践：分割PDF文档
让我们将学到的知识应用到一个真实的PDF文档上。我们使用上一节加载的PDF，并用 `RecursiveCharacterTextSplitter` 进行分割。

![](img/998abcc69f89944611c944f1c443e931_81.png)

![](img/998abcc69f89944611c944f1c443e931_83.png)

```python
# 假设 `pages` 是已加载的PDF文档列表
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    length_function=len, # 使用字符数计算长度
)

![](img/998abcc69f89944611c944f1c443e931_85.png)

![](img/998abcc69f89944611c944f1c443e931_87.png)

![](img/998abcc69f89944611c944f1c443e931_89.png)

docs = text_splitter.split_documents(pages) # 输入是Document对象列表
```
分割后，我们会得到比原始页面数量更多的文档片段。每个新片段都继承了原始页面的元数据（如来源和页码）。

![](img/998abcc69f89944611c944f1c443e931_91.png)

## 基于标记的分割
除了按字符分割，另一种重要方式是按**标记**分割。大型语言模型的上下文窗口通常以标记数限定，因此按标记分割有时更精确。

![](img/998abcc69f89944611c944f1c443e931_93.png)

首先导入标记分割器：
```python
from langchain.text_splitter import TokenTextSplitter
```

![](img/998abcc69f89944611c944f1c443e931_95.png)

标记和字符不同。让我们通过一个例子来理解：
```python
token_splitter = TokenTextSplitter(chunk_size=1, chunk_overlap=0)
text = “foo bar bazzy foo”
tokens = token_splitter.split_text(text)
# 结果可能是 [‘foo’, ‘ bar’, ‘ b’, ‘az’, ‘zy’, ‘ foo’]
```
可以看到，一个单词可能被拆分成多个标记，这与简单的字符分割截然不同。

![](img/998abcc69f89944611c944f1c443e931_97.png)

我们可以用同样的方式分割之前加载的文档：
```python
token_splitter = TokenTextSplitter(chunk_size=100, chunk_overlap=10)
split_docs = token_splitter.split_documents(pages)
```
每个分割后的文档片段同样会保留源文档的元数据。

## 添加元数据的分割器
在分割时，有时我们不仅想保留原始元数据，还想为每个片段添加新的元数据，例如该片段在文档中的层级位置。这在回答问题时能提供更多上下文。

![](img/998abcc69f89944611c944f1c443e931_99.png)

`MarkdownHeaderTextSplitter` 就是一个例子，它能根据Markdown标题进行分割，并将标题信息加入片段的元数据中。

![](img/998abcc69f89944611c944f1c443e931_101.png)

**示例：**
```python
from langchain.text_splitter import MarkdownHeaderTextSplitter

![](img/998abcc69f89944611c944f1c443e931_103.png)

markdown_document = “””
# Title
## Chapter 1
Hi, this is Jim.
Hi, this is Joe.
### Section 1.1
Hi, this is Lance.
## Chapter 2
Hi, this is Molly.
“””

![](img/998abcc69f89944611c944f1c443e931_105.png)

headers_to_split_on = [
    (“#”, “Header 1”),
    (“##”, “Header 2”),
    (“###”, “Header 3”),
]

![](img/998abcc69f89944611c944f1c443e931_107.png)

![](img/998abcc69f89944611c944f1c443e931_109.png)

splitter = MarkdownHeaderTextSplitter(headers_to_split_on=headers_to_split_on)
splits = splitter.split_text(markdown_document)
```
分割后，第一个片段的元数据会包含 `{“Header 1”: “Title”, “Header 2”: “Chapter 1”}`，而更深层片段的元数据则会包含所有父级标题信息。

![](img/998abcc69f89944611c944f1c443e931_111.png)

![](img/998abcc69f89944611c944f1c443e931_113.png)

我们可以将此分割器应用于之前加载的Notion Markdown文档，从而获得带有清晰层级结构的文档片段。

![](img/998abcc69f89944611c944f1c443e931_115.png)

## 总结
本节课中我们一起学习了文档分割的关键概念与实践。我们了解到：
1.  文档分割是将大文档拆分为语义连贯的小片段的重要步骤。
2.  **片段大小**和**片段重叠**是两个核心参数。
3.  LangChain提供了多种分割器（如按字符、按标记、按标题），适用于不同类型的文档。
4.  在分割过程中，妥善处理**元数据**的保留与添加，能为下游任务提供宝贵上下文。
5.  选择合适的分割策略，对于保证后续检索增强生成（RAG）流程的效果至关重要。

![](img/998abcc69f89944611c944f1c443e931_117.png)

我们已经学会了如何获取带有恰当元数据的语义相关片段，下一步就是将这些片段存入向量数据库，以便进行高效的检索。