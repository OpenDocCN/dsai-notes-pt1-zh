# 006：提取表格 📊

在本节课中，我们将要学习如何从非结构化文档（如PDF和图像）中提取表格内容，并将其转换为适合LLM处理的格式。表格是许多文档中承载结构化信息的关键部分，掌握其提取技术对于构建高质量的RAG应用程序至关重要。

## 概述：表格提取的重要性

上一节我们介绍了如何使用视觉线索从PDF和图像中提取信息。本节中，我们来看看更高级的预处理技术——提取表格内容。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_1.png)

大多数RAG应用侧重于文档中的纯文本内容。然而，在一些行业中，非结构化文档中经常包含结构化信息，例如表格。这在金融和保险等行业尤为常见。表格提取使得RAG应用程序能够从非结构化文档中提取包含在表格中的关键信息。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_1.png)

## 表格提取的挑战与方法

一些文档类型，如HTML和Word文档，其本身包含了表格的结构信息（例如HTML中的`<table>`标签）。对于这些文档，可以使用基于规则的解析器来提取表格信息。

然而，对于PDF和图像这类文档，需要使用视觉线索来识别文档中的表格区域，然后处理它以提取信息。主要有三种技术可以帮助完成这个任务：

1.  **表格转换器**
2.  **视觉转换器**
3.  **OCR后处理**

一旦从表格中提取了内容，通常需要将其转换为HTML输出，以便在将表格传递给LLM时保留其结构信息。

## 方法一：表格转换器

首先，我们来了解表格转换器。表格转换器是一个模型，它识别表格单元的边界框，然后将输出转换为HTML。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_3.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_3.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_5.png)

这个过程分为两个步骤：
1.  使用文档布局检测模型识别文档中的表格区域。
2.  将识别出的表格区域传递给表格转换器模型进行处理。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_5.png)

以下是表格转换器方法的优缺点：

**优点：**
*   能够追溯到原始文档中每个单元格的边界框信息。

**缺点：**
*   涉及多次昂贵的模型调用，包括文档布局检测和OCR调用。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_7.png)

表格转换器模型的架构如下图所示。如果您感兴趣，可以查看相关论文。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_7.png)

## 方法二：视觉转换器

您还可以使用视觉转换器从PDF和图像中提取表格内容，就像在上一课中提取JSON信息一样。但与此前不同的是，本节的目标输出是HTML。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_9.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_9.png)

以下是视觉转换器方法的优缺点：

**优点：**
*   提示更加灵活。
*   只涉及单一模型调用。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_11.png)

**缺点：**
*   属于生成式方法，容易产生“幻觉”（输出不准确的内容）。
*   不会保留单元格的边界框信息。

这种方法的过程示例如下：从文档图像开始，通过视觉转换器运行，最终得到表格的HTML文本表示。

## 方法三：OCR后处理

您可以用于预处理包含表格的文档的最后一种技术，是对表格进行光学字符识别（OCR）后处理。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_11.png)

在这种技术中：
1.  使用文档布局检测模型识别表格。
2.  对表格区域进行OCR，获取文本。
3.  使用基于规则或统计的方法处理OCR文本，以重建表格结构。

以下是OCR后处理方法的优缺点：

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_13.png)

**优点：**
*   处理速度快。
*   对于格式规范、简单的表格，可以非常准确。

**缺点：**
*   依赖于统计或基于规则的解析，比其他方法更脆弱、不够灵活。
*   对于复杂的表格，效果可能不理想。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_15.png)

下图展示了使用文档布局检测模型识别表格，并对OCR输出进行后处理，最终构造出HTML表示的示例。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_13.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_17.png)

## 实践：使用非结构化API提取表格

现在，您已经学会了从非结构化文档中提取表格的一些技术，接下来可以将其付诸实践。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_19.png)

首先，导入在之前课程中使用过的一些辅助函数。由于表格提取是基于模型的，我们将再次使用非结构化API，因为它已经为您设置好了所有需要的模型。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_20.png)

```python
# 导入必要的库和辅助函数
from unstructured.partition.pdf import partition_pdf
```

在上一课中，我们处理了一些简单的PDF示例。现在，我们将预处理一个包含表格和图像的更复杂文档。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_22.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_19.png)
![](img/9a48b00729d5f8e3427f3e9da2eb75ef_20.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_23.png)

您可以在下面看到这个文档的样子。我们的目标是从该文档的特定页面中提取“表1”的内容。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_22.png)
![](img/9a48b00729d5f8e3427f3e9da2eb75ef_23.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_25.png)

现在，开始进行预处理。要将文档传递给非结构化API，需要注意以下参数：

```python
# 调用API提取元素，包括表格
elements = partition_pdf(
    filename="complex_document.pdf",
    infer_table_structure=True,  # 关键参数：告诉API提取表格结构
    skip_infer_table_types=[],   # 不对任何文档类型跳过表格推断
    strategy="hi_res",           # 使用高分辨率策略
    model_name="yolox"           # 指定布局检测模型
)
```

*   `infer_table_structure=True`：这个参数告诉API我们想要提取表格的结构信息。
*   `skip_infer_table_types=[]`：这个参数指示API不要跳过任何文档类型的表格提取。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_27.png)

现在，可以运行API调用了。请注意，此过程需要多个模型调用，可能需要几分钟时间。

API调用完成后，可以通过过滤操作，将文档中的内容缩小到仅包含表格元素。我们将查找类别（category）为“Table”的元素。

```python
# 过滤出所有表格元素
tables = [el for el in elements if el.category == "Table"]
```

## 查看与使用提取结果

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_29.png)

现在您有了表格元素，可以使用元素的`.text`属性查看表格中的文本内容。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_31.png)

```python
# 查看第一个表格的文本内容
print(tables[0].text)
```

然而，正如之前所学，将表格信息以HTML格式传递给LLM有助于保持表格结构。这些信息存储在元素的元数据中，作为一个名为`“html”`的字段。

```python
# 查看表格的HTML表示
table_html = tables[0].metadata.html
print(table_html)
```

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_33.png)

您可以使用以下代码块查看元数据字段中的HTML是什么样子。如您所见，这是文档中表格的HTML表示。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_35.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_33.png)
![](img/9a48b00729d5f8e3427f3e9da2eb75ef_35.png)

如果您有兴趣在Notebook中显示此表格，也可以使用IPython的`HTML`显示功能。

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_37.png)

```python
from IPython.display import HTML, display
display(HTML(table_html))
```

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_38.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_40.png)
![](img/9a48b00729d5f8e3427f3e9da2eb75ef_42.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_40.png)

## 为表格生成摘要

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_42.png)

一旦从文档中提取了表格内容并将其转换为HTML，为这些表格生成摘要也很有帮助。这样，在RAG架构中执行相似性搜索时，就可以搜索这些摘要。

为此，我们将使用LangChain中的一些实用工具。LangChain是一个用于编排LLM应用的流行框架。

```python
from langchain.chains.summarize import load_summarize_chain
from langchain.schema import Document
from langchain_openai import ChatOpenAI

# 实例化LLM
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)

# 创建摘要链
chain = load_summarize_chain(llm, chain_type="stuff")

# 将表格HTML内容包装成LangChain文档对象
table_doc = Document(page_content=table_html)

# 对表格进行摘要
summary = chain.run([table_doc])
print(summary)
```

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_44.png)

![](img/9a48b00729d5f8e3427f3e9da2eb75ef_44.png)

正如您所看到的，模型成功地对表格进行了摘要。现在，我们既有了表格的HTML表示，也有了一个文本摘要，可以在RAG系统内进行相似性搜索时使用。

## 总结与练习

本节课中，我们一起学习了如何从PDF和图像中提取表格内容。我们介绍了三种主要方法：表格转换器、视觉转换器和OCR后处理，并重点实践了使用非结构化API进行表格提取和摘要的完整流程。

现在您知道如何提取表格内容了，请尝试在您自己的文件上实践一下。如果您有兴趣尝试不同的参数，还可以将`high_res`策略下的`model_name`参数更改为`“chipper”`，以便比较视觉转换器与表格转换器的输出效果。

在下一课中，我们将把所有技能结合起来，为您自己构建一个完整的文档预处理流程。