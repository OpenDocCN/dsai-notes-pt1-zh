# 011：文档分割 📄✂️

![](img/ebe398bb25bacc9df35d89337a0c9403_0.png)

## 概述
在本节课中，我们将要学习如何将加载好的文档分割成更小的“块”。文档分割是构建高效检索系统（RAG）的关键步骤，它直接影响后续信息检索的质量。我们将探讨分割的重要性、不同分割策略的原理，并通过代码示例学习如何使用LangChain中的各种文本分割器。

![](img/ebe398bb25bacc9df35d89337a0c9403_2.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_3.png)

---

![](img/ebe398bb25bacc9df35d89337a0c9403_5.png)

## 为什么文档分割既重要又棘手？
上一节我们介绍了如何将文档加载为标准格式。本节中我们来看看如何将它们分割成更小的块。这听起来可能很简单，但其中有很多微妙之处，对未来步骤有巨大影响。

![](img/ebe398bb25bacc9df35d89337a0c9403_7.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_9.png)

将数据加载为文档格式后，在它进入向量存储之前，需要进行文档分割。一个简单的想法是根据固定字符长度来分割块。

![](img/ebe398bb25bacc9df35d89337a0c9403_11.png)

但为什么这既棘手又非常重要呢？请看以下例子：
> 我们有一个关于丰田凯美瑞的句子和一些规格。

![](img/ebe398bb25bacc9df35d89337a0c9403_13.png)

如果我们进行简单的分割，可能会将句子的一部分放在一个块中，另一部分放在另一个块中。当我们试图回答“凯美瑞的规格是什么？”这个问题时，实际上在这两个块中都没有完整的正确信息。因此，如何分割块，使得语义上相关的内容能保持在一起，具有很多细微差别和重要性。

---

## LangChain文本分割器基础
LangChain中所有文本分割器的基础都涉及几个核心概念：**块大小** 和 **块重叠**。

![](img/ebe398bb25bacc9df35d89337a0c9403_15.png)

*   **块大小**：指每个块的大小，可以用几种不同的方法来测量（例如字符数或令牌数）。我们通过 `length_function` 参数来指定测量方式。
*   **块重叠**：指相邻两个块之间保留的重叠部分，就像一个滑动窗口。这允许相同的上下文出现在一个块的末尾和下一个块的开头，有助于保持语义的连贯性。

文本分割器通常有 `create_documents`（接收文本列表）和 `split_documents`（接收文档列表）两种方法，它们底层的分割逻辑相同，只是接口略有不同。

![](img/ebe398bb25bacc9df35d89337a0c9403_17.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_19.png)

LangChain中有多种类型的文本分割器，它们在以下几个维度上有所不同：
1.  **分割依据**：根据什么字符或规则进行分割。
2.  **长度测量**：是按字符、令牌计数，还是使用其他模型（如判断句子结尾的小模型）来测量。
3.  **元数据处理**：如何在所有块中维护原始元数据，并在适当时添加新的元数据（例如块在文档中的位置）。
4.  **文档类型特异性**：分割方式通常可以针对特定文档类型进行优化，这在代码分割上尤为明显。例如，有专门的 `Language` 文本分割器，针对Python、Ruby、C等不同编程语言使用不同的分隔符。

![](img/ebe398bb25bacc9df35d89337a0c9403_21.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_23.png)

---

![](img/ebe398bb25bacc9df35d89337a0c9403_25.png)

## 实践：两种基础分割器
现在，让我们通过代码来了解具体如何使用这些分割器。首先，我们设置环境并导入两种最常见的文本分割器。

![](img/ebe398bb25bacc9df35d89337a0c9403_27.png)

```python
# 设置环境（例如OpenAI API密钥）
import os
os.environ[‘OPENAI_API_KEY‘] = ‘your-api-key-here‘

![](img/ebe398bb25bacc9df35d89337a0c9403_29.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_31.png)

# 导入分割器
from langchain.text_splitter import RecursiveCharacterTextSplitter, CharacterTextSplitter
```

![](img/ebe398bb25bacc9df35d89337a0c9403_33.png)

我们将先讨论一些简单的用例，以了解这些分割器的工作原理。设置一个较小的块大小（26）和块重叠（4）。

![](img/ebe398bb25bacc9df35d89337a0c9403_35.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_37.png)

```python
# 初始化分割器
chunk_size = 26
chunk_overlap = 4

![](img/ebe398bb25bacc9df35d89337a0c9403_39.png)

r_splitter = RecursiveCharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=chunk_overlap)
c_splitter = CharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=chunk_overlap)
```

![](img/ebe398bb25bacc9df35d89337a0c9403_41.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_43.png)

以下是几个不同的用例：

**用例1：长度等于块大小的字符串**
```python
text1 = “abcdefghijklmnopqrstuvwxyz“
print(r_splitter.split_text(text1))
# 输出: [‘abcdefghijklmnopqrstuvwxyz‘]
```
字符串长度为26个字符，与指定的块大小一致，因此无需分割。

![](img/ebe398bb25bacc9df35d89337a0c9403_45.png)

**用例2：长度超过块大小的字符串**
```python
text2 = “abcdefghijklmnopqrstuvwxyzabcdefg“
print(r_splitter.split_text(text2))
# 输出: [‘abcdefghijklmnopqrstuvwxyz‘, ‘wxyzabcdefg‘]
```
这里创建了两个块。第一个块以“z”结尾（26个字符）。第二个块以“wxyz”开头，这体现了4个字符的重叠，然后继续字符串的其余部分。

**用例3：包含空格的字符串**
```python
text3 = “a b c d e f g h i j k l m n o p q r s t u v w x y z“
print(r_splitter.split_text(text3))
# 输出: [‘a b c d e f g h i j k l‘, ‘i j k l m n o p q r s t u v‘, ‘s t u v w x y z‘]
```
由于空格也占用字符位置，现在分成了三个块。注意重叠部分：“i j k l”同时出现在第一和第二个块的末尾/开头。

![](img/ebe398bb25bacc9df35d89337a0c9403_47.png)

现在尝试使用 `CharacterTextSplitter`。默认情况下，它在换行符(`\n`)上分割。
```python
print(c_splitter.split_text(text1))
# 输出: [‘abcdefghijklmnopqrstuvwxyz‘]
```
它没有分割，因为字符串中没有换行符。如果我们将分隔符设置为空格：
```python
c_splitter = CharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=chunk_overlap, separator=“ “)
print(c_splitter.split_text(text3))
# 输出与r_splitter类似
```

![](img/ebe398bb25bacc9df35d89337a0c9403_49.png)

**建议**：在此处暂停视频，尝试自己编造不同的字符串，并交换分割器、调整块大小和块重叠，观察会发生什么。通过几个简单例子建立直觉，有助于理解后续更真实的场景。

![](img/ebe398bb25bacc9df35d89337a0c9403_51.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_53.png)

---

## 处理真实文本
现在，让我们用一些更真实的例子来尝试。这里有一段较长的文本，其中包含典型的段落分隔符——双换行符(`\n\n`)。

![](img/ebe398bb25bacc9df35d89337a0c9403_55.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_57.png)

```python
some_text = “““
... (此处是一段约500字符的文本，包含多个段落) ...
“““

![](img/ebe398bb25bacc9df35d89337a0c9403_59.png)

print(len(some_text)) # 大约500
```

![](img/ebe398bb25bacc9df35d89337a0c9403_61.png)

定义两个文本分割器：
```python
# 1. 字符文本分割器，以空格为分隔符
c_splitter = CharacterTextSplitter(chunk_size=450, chunk_overlap=0, separator=“ “)
# 2. 递归字符文本分割器，使用默认分隔符列表
from langchain.text_splitter import RecursiveCharacterTextSplitter
r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=450,
    chunk_overlap=0,
    separators=[“\n\n“, “\n“, “ “, ““] # 这是默认列表，此处显式写出以便理解
)
```

![](img/ebe398bb25bacc9df35d89337a0c9403_63.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_65.png)

观察它们对上述文本的处理：
*   `c_splitter` 在空格上分割，可能导致句子从中间被切断。
*   `r_splitter` 首先尝试在双换行符(`\n\n`)上分割。即使第一个段落比指定的450字符短，它也会将其作为一个独立的块分割出来。这通常更好，因为每个段落保持了完整性。

为了在句子级别分割，我们可以添加句号作为分隔符。但需要注意正则表达式的使用，以确保句号在正确的位置（例如，避免将“Mr.”中的点号误判为句子结束）。
```python
r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=150,
    chunk_overlap=0,
    separators=[“\n\n“, “\n“, “(?<=\. )“, “ “, ““] # “(?<=\. )“ 是一个“向后查找”正则，匹配后面跟着空格的句号
)
# 运行后，文本将在句子结尾处被正确分割。
```

![](img/ebe398bb25bacc9df35d89337a0c9403_67.png)

---

![](img/ebe398bb25bacc9df35d89337a0c9403_69.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_71.png)

## 应用于真实文档
让我们使用在文档加载课程中用过的PDF文件进行实践。

![](img/ebe398bb25bacc9df35d89337a0c9403_73.png)

```python
from langchain.document_loaders import PyPDFLoader
loader = PyPDFLoader(“docs/cs229_lectures/MachineLearning-Lecture01.pdf“)
pages = loader.load()

![](img/ebe398bb25bacc9df35d89337a0c9403_75.png)

# 定义文本分割器
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=150,
    length_function=len, # 默认就是len，此处显式说明是按字符数计算
)

![](img/ebe398bb25bacc9df35d89337a0c9403_77.png)

# 分割文档
docs = text_splitter.split_documents(pages) # 传入文档列表

![](img/ebe398bb25bacc9df35d89337a0c9403_79.png)

print(f“原始页面数: {len(pages)}“)
print(f“分割后的文档数: {len(docs)}“)
# 可以看到创建了更多的文档，因为进行了分割。
```

我们也可以像第一节课那样，查看分割前后文档的内容长度对比。
```python
print(pages[0].page_content[:500]) # 查看原始第一页的前500字符
print(“---“)
print(docs[0].page_content) # 查看分割后第一个块的内容
print(“---“)
print(docs[1].page_content) # 查看分割后第二个块的内容
```

![](img/ebe398bb25bacc9df35d89337a0c9403_81.png)

**建议**：这是一个很好的暂停点，可以尝试用不同的块大小和重叠值进行实验。

![](img/ebe398bb25bacc9df35d89337a0c9403_83.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_85.png)

---

![](img/ebe398bb25bacc9df35d89337a0c9403_87.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_89.png)

## 基于令牌的分割
到目前为止，我们都是基于字符进行分割。但还有另一种重要方法：基于**令牌**进行分割。LLM通常有由令牌数定义的上下文窗口，因此基于令牌分割能更准确地反映模型对文本的“看法”。

![](img/ebe398bb25bacc9df35d89337a0c9403_91.png)

```python
from langchain.text_splitter import TokenTextSplitter

![](img/ebe398bb25bacc9df35d89337a0c9403_93.png)

# 初始化令牌文本分割器，块大小为1，重叠为0，这会将文本拆分为单独的令牌列表。
token_splitter = TokenTextSplitter(chunk_size=1, chunk_overlap=0)

![](img/ebe398bb25bacc9df35d89337a0c9403_95.png)

text = “foo bar bazzy foo“
print(token_splitter.split_text(text))
# 输出可能是: [‘foo‘, ‘ bar‘, ‘ baz‘, ‘zy‘, ‘ foo‘]
```
这个例子展示了在字符上拆分和在令牌上拆分的区别。令牌“bazzy”被拆分成了“baz”和“zy”。

![](img/ebe398bb25bacc9df35d89337a0c9403_97.png)

让我们将其应用到之前加载的PDF文档上：
```python
token_splitter = TokenTextSplitter(chunk_size=100, chunk_overlap=10)
token_docs = token_splitter.split_documents(pages)

print(token_docs[0].page_content[:100]) # 查看第一个令牌块的内容
print(token_docs[0].metadata) # 查看元数据
```
可以看到，`metadata`（如`source`和`page`）被正确地传递到了每个分割后的块中。

![](img/ebe398bb25bacc9df35d89337a0c9403_99.png)

---

## 增强元数据的分割
有些情况下，你希望在分割时向块添加更多元数据，例如块在文档中的位置信息。这可以在回答问题时提供更多上下文。

一个具体的例子是 `MarkdownHeaderTextSplitter`。它会根据标题（如`# Header1`, `## Header2`）来分割Markdown文件，并将这些标题作为元数据添加到每个块中。

![](img/ebe398bb25bacc9df35d89337a0c9403_101.png)

首先看一个玩具示例：
```python
from langchain.text_splitter import MarkdownHeaderTextSplitter

![](img/ebe398bb25bacc9df35d89337a0c9403_103.png)

markdown_document = “““
# Title
## Chapter 1
Hi, I‘m Jim. Hi, I‘m Joe.
## Chapter 2
Hi, I‘m Lance.
### Section 2.1
Hi, I‘m Harry.
“““

![](img/ebe398bb25bacc9df35d89337a0c9403_105.png)

headers_to_split_on = [
    (“#“, “Header 1“),
    (“##“, “Header 2“),
    (“###“, “Header 3“),
]

markdown_splitter = MarkdownHeaderTextSplitter(headers_to_split_on=headers_to_split_on)
md_header_splits = markdown_splitter.split_text(markdown_document)

![](img/ebe398bb25bacc9df35d89337a0c9403_107.png)

print(md_header_splits[0])
# 输出内容包含 “Hi, I‘m Jim. Hi, I‘m Joe.“，其元数据为 {‘Header 1‘: ‘Title‘, ‘Header 2‘: ‘Chapter 1‘}
print(md_header_splits[2])
# 输出内容包含 “Hi, I‘m Harry.“，其元数据为 {‘Header 1‘: ‘Title‘, ‘Header 2‘: ‘Chapter 2‘, ‘Header 3‘: ‘Section 2.1‘}
```

![](img/ebe398bb25bacc9df35d89337a0c9403_109.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_111.png)

现在，让我们在一个真实世界的Markdown文档上尝试（使用之前加载Confluence目录的示例）：
```python
from langchain.document_loaders import ConfluenceLoader
# ... 加载Confluence文档 ...

markdown_splitter = MarkdownHeaderTextSplitter(headers_to_split_on=[(“#“, “Header 1“), (“##“, “Header 2“)])
md_header_splits = markdown_splitter.split_documents(confluence_docs)

![](img/ebe398bb25bacc9df35d89337a0c9403_113.png)

# 查看分割后的块及其丰富的元数据
for split in md_header_splits[:2]:
    print(split.page_content[:100])
    print(split.metadata)
    print(“---“)
```

![](img/ebe398bb25bacc9df35d89337a0c9403_115.png)

---

![](img/ebe398bb25bacc9df35d89337a0c9403_117.png)

![](img/ebe398bb25bacc9df35d89337a0c9403_119.png)

## 总结
本节课中，我们一起学习了文档分割的核心概念与实践：
1.  **分割的重要性**：不恰当的分割会破坏语义，影响后续检索效果。
2.  **核心参数**：**块大小** (`chunk_size`) 和 **块重叠** (`chunk_overlap`) 是控制分割粒度和连贯性的关键。
3.  **多种分割器**：我们实践了 `RecursiveCharacterTextSplitter`、`CharacterTextSplitter`、`TokenTextSplitter` 和 `MarkdownHeaderTextSplitter`，它们分别适用于不同场景和需求。
4.  **长度测量**：分割可以基于**字符**或**令牌**，后者更贴近LLM的运作方式。
5.  **元数据保留与增强**：分割时需确保原始元数据传递到每个块，并可以利用特定分割器（如 `MarkdownHeaderTextSplitter`）添加结构性元数据，为检索提供更丰富的上下文。

我们已经讨论了如何获得带有合适元数据的、语义相关的文档块。下一步，就是将这些数据块存入向量数据库，以便进行高效的相似性检索。