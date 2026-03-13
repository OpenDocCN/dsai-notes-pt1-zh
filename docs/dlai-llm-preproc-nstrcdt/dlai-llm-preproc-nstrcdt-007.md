# 007：构建RAG机器人 🚀

在本节课中，我们将综合运用之前学到的所有技巧，构建一个RAG机器人。这个机器人将使用一个包含PDF、PowerPoint和Markdown文档的语料库。我们将编写代码，整合预处理、向量数据库查询和LLM提示构建等步骤，最终实现一个能与文档对话的智能应用。

---

## 准备工作

![](img/c1c5ba90202b449c5953f0251c01e288_1.png)

上一节我们介绍了多种文档预处理技巧，本节中我们来看看如何将它们整合到一个完整的应用程序中。首先，你需要准备好语料库和必要的工具。

![](img/c1c5ba90202b449c5953f0251c01e288_3.png)

以下是构建RAG机器人前需要完成的准备工作：

1.  **导入辅助函数**：导入在整个课程中已见过的函数，并针对新文档类型（如Markdown）添加新的处理函数（例如 `partition_markdown`）。
2.  **设置非结构化客户端**：因为语料库包含PDF文档，除了基于规则的解析外，还需要使用非结构化API。
3.  **准备语料库**：本教程的语料库包含三份关于“甜甜圈模型”的文档：
    *   一份包含复杂表格的PDF论文。
    *   一份包含相关信息的PowerPoint演示文稿。
    *   一份来自GitHub仓库的Markdown格式的README文件。

---

![](img/c1c5ba90202b449c5953f0251c01e288_5.png)

## 预处理PDF文档

![](img/c1c5ba90202b449c5953f0251c01e288_7.png)

现在，我们开始对PDF文档进行预处理。我们将应用在课程中学到的知识，使用非结构化API调用YOLOx模型进行文档布局检测。

![](img/c1c5ba90202b449c5953f0251c01e288_9.png)

```python
# 示例：使用非结构化API处理PDF
processed_pdf_elements = unstructured_client.process_pdf("donut_paper.pdf", strategy="hi_res")
```

![](img/c1c5ba90202b449c5953f0251c01e288_11.png)

这是一个基于昂贵模型的工作负载，如果处理需要几分钟，属于正常现象。预处理完成后，可以查看结果。例如，以下是一个被识别为“标题”的元素：

```python
print(processed_pdf_elements[0].text)  # 输出标题文本
```

![](img/c1c5ba90202b449c5953f0251c01e288_13.png)

PDF中的表格信息被保存在元素的元数据中，以HTML格式存储。你可以在组装RAG应用程序后查询这些表格。

![](img/c1c5ba90202b449c5953f0251c01e288_15.png)

---

## 过滤无关内容

![](img/c1c5ba90202b449c5953f0251c01e288_17.png)

预处理后的文档可能包含我们不希望被检索到的内容，例如参考文献和页眉。我们可以利用提取的元数据进行过滤。

### 过滤参考文献

![](img/c1c5ba90202b449c5953f0251c01e288_19.png)

![](img/c1c5ba90202b449c5953f0251c01e288_20.png)

首先，我们过滤掉参考文献部分。参考文献不包含叙述性内容，对问答应用价值不大。

![](img/c1c5ba90202b449c5953f0251c01e288_22.png)

1.  **定位参考文献标题**：通过检查元素的类别和文本内容，找到“References”或“参考文献”标题元素。
2.  **获取其元素ID**：将该标题元素的ID保存下来。
3.  **过滤子元素**：查找所有“父ID”等于该标题元素ID的元素，这些就是参考文献条目。
4.  **执行过滤**：从元素集合中移除这些条目。

```python
# 伪代码：过滤参考文献
reference_title_id = find_element_id_by_text(elements, "References")
elements = [el for el in elements if el.metadata.parent_id != reference_title_id]
```

### 过滤页眉信息

![](img/c1c5ba90202b449c5953f0251c01e288_24.png)

接下来，我们过滤掉每页顶部的页眉信息（如文档标题和页码），这些信息会破坏文档的叙事结构。

1.  **识别页眉元素**：页眉元素的元数据中，`category` 字段通常为 `Header`。
2.  **执行过滤**：直接从元素集合中移除类别为 `Header` 的元素。

![](img/c1c5ba90202b449c5953f0251c01e288_26.png)

```python
# 伪代码：过滤页眉
elements = [el for el in elements if el.metadata.category != "Header"]
```

![](img/c1c5ba90202b449c5953f0251c01e288_28.png)

完成这两步过滤后，我们的元素集就只包含核心的叙述性内容了。

![](img/c1c5ba90202b449c5953f0251c01e288_30.png)

---

![](img/c1c5ba90202b449c5953f0251c01e288_32.png)

## 预处理其他文档类型

处理完PDF后，我们继续预处理PowerPoint和Markdown文档。方法与我们早期课程中学到的一致。

![](img/c1c5ba90202b449c5953f0251c01e288_34.png)

*   **预处理PowerPoint**：使用非结构化开源库中的 `partition_pptx` 函数。
*   **预处理Markdown**：使用非结构化开源库中的 `partition_md` 函数。

```python
from unstructured.partition.pptx import partition_pptx
from unstructured.partition.md import partition_md

![](img/c1c5ba90202b449c5953f0251c01e288_36.png)

ppt_elements = partition_pptx("donut_presentation.pptx")
md_elements = partition_md("README.md")
```

![](img/c1c5ba90202b449c5953f0251c01e288_38.png)

---

## 分块与加载到向量数据库

![](img/c1c5ba90202b449c5953f0251c01e288_40.png)

现在我们已经预处理了所有文档，下一步是将它们合并为单个语料库，并进行分块处理。

1.  **合并与分块**：使用在元数据和分块课程中学到的 `chunk_by_title` 函数对所有文档元素进行分块。
2.  **准备加载**：将分块后的文档转换为适合加载的格式，并包含元数据（如来源文件名），以便后续进行混合搜索。
3.  **嵌入与加载**：使用OpenAI Embeddings为文档块生成向量，然后利用LangChain的工具将其加载到Chroma向量数据库中。

![](img/c1c5ba90202b449c5953f0251c01e288_42.png)

```python
from langchain.vectorstores import Chroma
from langchain.embeddings import OpenAIEmbeddings

# 假设 `all_chunks` 是分块后的文档列表，包含文本和元数据
vectorstore = Chroma.from_documents(
    documents=all_chunks,
    embedding=OpenAIEmbeddings(),
    persist_directory="./chroma_db"
)
```

---

![](img/c1c5ba90202b449c5953f0251c01e288_44.png)

## 设置检索器与提示模板

向量数据库准备就绪后，我们需要设置检索器和与大模型对话的提示模板。

![](img/c1c5ba90202b449c5953f0251c01e288_46.png)

1.  **设置检索器**：创建一个基于向量相似度的检索器，用于从数据库中查找与问题相关的文档块。

    ```python
    retriever = vectorstore.as_retriever(search_kwargs={"k": 6})
    ```

2.  **创建提示模板**：使用LangChain定义提示模板，指导LLM如何基于检索到的上下文回答问题，并声明在不知道答案时应如实回答。

    ```python
    from langchain.prompts import PromptTemplate

    template = """你是一个AI助手，请根据以下上下文回答问题。
    如果你不知道答案，就说你不知道。不要编造答案。

    上下文：{context}

    问题：{question}
    答案："""
    PROMPT = PromptTemplate(template=template, input_variables=["context", "question"])
    ```

![](img/c1c5ba90202b449c5953f0251c01e288_48.png)

---

## 构建对话链并进行查询

最后，我们将所有组件连接起来，构建一个完整的对话链。

1.  **构建对话链**：使用LangChain的 `ConversationalRetrievalChain`，它集成了检索、历史管理和提示填充功能。

    ```python
    from langchain.chains import ConversationalRetrievalChain
    from langchain.chat_models import ChatOpenAI

    qa_chain = ConversationalRetrievalChain.from_llm(
        llm=ChatOpenAI(temperature=0),
        retriever=retriever,
        combine_docs_chain_kwargs={"prompt": PROMPT}
    )
    ```

![](img/c1c5ba90202b449c5953f0251c01e288_50.png)

2.  **进行查询**：现在可以向你的RAG机器人提问了。

    *   **示例问题1**：“Donut模型与其他文档理解模型有何不同？”
        *   **机器人回答**：正确指出Donut是一个不依赖OCR的文档理解模型，并引用了其来源（论文和幻灯片）。
    *   **示例问题2（混合搜索）**：如果你只想从README文件中查找信息，可以设置一个能过滤元数据（如文件名）的检索器。

        ```python
        # 创建过滤特定来源的检索器
        filtered_retriever = vectorstore.as_retriever(
            search_kwargs={"k": 6, "filter": {"source": "README.md"}}
        )
        # 用新的检索器创建链并提问
        ```
        提问：“如何使用Donut对文档进行分类？” 机器人将只用README文件中的内容来回答。

---

## 总结与拓展

本节课中我们一起学习了如何构建一个完整的RAG机器人。我们从预处理混合格式的文档开始，经历了过滤无关内容、分块、嵌入并加载到向量数据库，最后设置了检索器和提示模板，通过LangChain链实现了智能问答。

![](img/c1c5ba90202b449c5953f0251c01e288_52.png)

你现在已经拥有了构建自己RAG应用的基础。可以尝试以下方式进行改进：

*   加入你自己的文档（如Word文档）。
*   尝试提出更复杂的问题。
*   在混合搜索中利用其他元数据字段（如作者、章节）进行过滤。

![](img/c1c5ba90202b449c5953f0251c01e288_54.png)

现在，你已经准备好基于对你自己或你的组织重要的各种文档信息，创建专属的智能问答机器人了。