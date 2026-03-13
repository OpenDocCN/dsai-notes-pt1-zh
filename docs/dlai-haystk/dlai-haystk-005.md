# 005：使用分支管道实现回退机制 🛠️

在本节课中，我们将学习如何构建一个能够分支的管道，并实现一个回退到网络搜索的机制。当基于数据库的RAG管道无法回答用户查询时，系统将自动切换到网络搜索来寻找答案。

## 概述

![](img/df2d29d845a86b3065b7116302337cd7_1.png)

我们之前提到过管道可以分支，本节课将实际构建一个分支管道。回退机制是分支管道的一个典型应用场景。我们将使用Haystack中的**条件路由器**组件，根据RAG管道的输出结果来决定是直接给出答案，还是转向网络搜索分支。

## 构建基础RAG管道

![](img/df2d29d845a86b3065b7116302337cd7_3.png)

首先，我们需要构建一个基础的检索增强生成管道。以下是构建步骤：

![](img/df2d29d845a86b3065b7116302337cd7_5.png)

以下是导入依赖项和初始化环境的代码：

```python
import warnings
warnings.filterwarnings("ignore")
import os
from haystack import Pipeline
from haystack.document_stores import InMemoryDocumentStore
from haystack.nodes import BM25Retriever, PromptNode, PromptTemplate, ConditionalRouter
```

![](img/df2d29d845a86b3065b7116302337cd7_7.png)

接下来，我们将一些虚拟文档写入内存文档存储。这些文档描述了不同的Haystack组件及其使用方法。

```python
document_store = InMemoryDocumentStore()
documents = [
    Document(content="检索器用于为用户查询检索相关文档。"),
    Document(content="生成器基于检索到的文档和查询生成自然语言回答。"),
    Document(content="提示构建器负责组装发送给大型语言模型的提示。"),
    Document(content="文档存储用于保存和索引文档。")
]
document_store.write_documents(documents)
```

![](img/df2d29d845a86b3065b7116302337cd7_9.png)

现在，我们开始创建RAG管道。首先定义提示模板，其中包含一个特殊指令：如果答案不在文档中，则回复“无答案”。

![](img/df2d29d845a86b3065b7116302337cd7_11.png)

```python
rag_prompt_template = PromptTemplate(
    prompt="""基于以下文档回答问题。如果答案不在文档中，请回复“无答案”。
文档：{documents}
问题：{query}
答案："""
)
```

然后，我们初始化管道并添加组件：基于关键字的BM25检索器、提示构建器以及使用GPT-3的生成器。

```python
rag_pipeline = Pipeline()
retriever = BM25Retriever(document_store=document_store)
prompt_builder = PromptNode(model_name_or_path="gpt-3.5-turbo", default_prompt_template=rag_prompt_template)
generator = PromptNode(model_name_or_path="gpt-3.5-turbo")

![](img/df2d29d845a86b3065b7116302337cd7_13.png)

rag_pipeline.add_node(component=retriever, name="Retriever", inputs=["Query"])
rag_pipeline.add_node(component=prompt_builder, name="PromptBuilder", inputs=["Retriever"])
rag_pipeline.add_node(component=generator, name="Generator", inputs=["PromptBuilder"])
```

让我们测试一下这个基础管道。询问一个已知的问题：

```python
result = rag_pipeline.run(query="检索器是用来干什么的？")
print(result)
```
输出应为：`检索器是用于为用户查询检索相关文档的。`

再询问一个未知的问题：

```python
result = rag_pipeline.run(query="米斯特拉尔有哪些组件？")
print(result)
```
由于文档中没有相关信息，模型应回复：`无答案`。

![](img/df2d29d845a86b3065b7116302337cd7_15.png)

## 引入条件路由器实现分支

![](img/df2d29d845a86b3065b7116302337cd7_17.png)

上一节我们构建了基础的RAG管道，本节中我们来看看如何通过**条件路由器**为其添加分支逻辑，实现回退机制。

![](img/df2d29d845a86b3065b7116302337cd7_19.png)

![](img/df2d29d845a86b3065b7116302337cd7_21.png)

条件路由器是一个特殊组件，它根据预定义的条件和路线来决定管道的执行流向。其核心逻辑可以用以下伪代码描述：

```
if 条件1满足:
    激活分支1
elif 条件2满足:
    激活分支2
else:
    激活默认分支
```

在我们的用例中，条件是基于生成器的回复是否包含“无答案”。我们将创建两条路线：

![](img/df2d29d845a86b3065b7116302337cd7_23.png)

1.  **路线1（网络搜索）**：如果回复中包含“无答案”（不区分大小写），则将查询路由到网络搜索分支。
2.  **路线2（答案）**：如果回复中不包含“无答案”，则直接将回复作为最终答案输出。

以下是定义路线的代码：

![](img/df2d29d845a86b3065b7116302337cd7_25.png)

```python
from haystack.nodes import RouteCondition

def contains_no_answer(reply):
    # 检查回复中是否包含“无答案”，忽略大小写
    return "无答案" in reply.lower()

![](img/df2d29d845a86b3065b7116302337cd7_27.png)

route_web = RouteCondition(
    condition=contains_no_answer,
    output="query",
    output_name="go_to_web_search"
)
route_answer = RouteCondition(
    condition=lambda reply: not contains_no_answer(reply),
    output="reply",
    output_name="answer"
)

router = ConditionalRouter(routes=[route_web, route_answer])
```

现在，我们将路由器添加到现有的RAG管道中，并将生成器的输出连接到路由器。

```python
rag_pipeline.add_node(component=router, name="Router", inputs=["Generator"])
```

![](img/df2d29d845a86b3065b7116302337cd7_29.png)

此时，管道的结构变为：`Query -> Retriever -> PromptBuilder -> Generator -> Router`。路由器将根据生成器的回复，输出到`go_to_web_search`或`answer`。

让我们测试一下分支逻辑。再次询问未知问题：

![](img/df2d29d845a86b3065b7116302337cd7_31.png)

```python
result = rag_pipeline.run(query="海斯塔克有哪些米斯特拉尔组件？")
print(result)
```
输出应为：`{'go_to_web_search': {'query': '海斯塔克有哪些米斯特拉尔组件？'}}`。这表明路由器已成功将查询导向网络搜索分支。

![](img/df2d29d845a86b3065b7116302337cd7_33.png)

## 构建网络搜索分支

![](img/df2d29d845a86b3065b7116302337cd7_35.png)

上一节我们成功将查询路由到了网络搜索分支，本节中我们来看看如何构建这个分支，使其能够从互联网获取信息并生成回答。

![](img/df2d29d845a86b3065b7116302337cd7_37.png)

我们将使用 `SerperDevWebSearch` 组件进行网络搜索。该组件接收查询，在网络上搜索，并将结果作为Haystack文档返回。这样，我们就可以构建一个与之前类似的RAG管道，但文档来源是网络。

![](img/df2d29d845a86b3065b7116302337cd7_39.png)

首先，定义用于网络搜索的提示模板，要求模型在答案中注明信息来源于网络搜索。

![](img/df2d29d845a86b3065b7116302337cd7_41.png)

```python
web_prompt_template = PromptTemplate(
    prompt="""基于以下从网络搜索获得的文档回答问题。你的答案应该表明信息来源于网络搜索。
网络搜索结果：{documents}
问题：{query}
基于网络搜索的答案："""
)
```

![](img/df2d29d845a86b3065b7116302337cd7_43.png)

现在，我们构建完整的、包含回退机制的分支管道。我们将创建一个新管道，它整合了基础的RAG管道和网络搜索分支。

```python
from haystack.nodes import WebRetriever # 假设使用WebRetriever，实际可能是SerperDevWebSearch

![](img/df2d29d845a86b3065b7116302337cd7_45.png)

full_pipeline = Pipeline()

![](img/df2d29d845a86b3065b7116302337cd7_47.png)

# 添加基础RAG组件
full_pipeline.add_node(component=retriever, name="Retriever", inputs=["Query"])
full_pipeline.add_node(component=prompt_builder, name="PromptBuilder", inputs=["Retriever"])
full_pipeline.add_node(component=generator, name="Generator", inputs=["PromptBuilder"])
full_pipeline.add_node(component=router, name="Router", inputs=["Generator"])

# 添加网络搜索分支组件
web_retriever = WebRetriever(api_key=os.getenv("SERPERDEV_API_KEY"))
web_prompt_builder = PromptNode(model_name_or_path="gpt-3.5-turbo", default_prompt_template=web_prompt_template)
web_generator = PromptNode(model_name_or_path="gpt-3.5-turbo")

full_pipeline.add_node(component=web_retriever, name="WebRetriever", inputs=["Router.go_to_web_search"])
full_pipeline.add_node(component=web_prompt_builder, name="WebPromptBuilder", inputs=["WebRetriever", "Router.go_to_web_search"])
full_pipeline.add_node(component=web_generator, name="WebGenerator", inputs=["WebPromptBuilder"])
```

最后，我们需要连接这些节点。关键点在于，路由器输出的查询（`go_to_web_search`）将同时作为 `WebRetriever` 的查询输入和 `WebPromptBuilder` 的查询输入。

![](img/df2d29d845a86b3065b7116302337cd7_49.png)

![](img/df2d29d845a86b3065b7116302337cd7_51.png)

```python
# 连接网络搜索分支
full_pipeline.connect(source="Router.go_to_web_search", target="WebRetriever.query")
full_pipeline.connect(source="Router.go_to_web_search", target="WebPromptBuilder.query")
full_pipeline.connect(source="WebRetriever", target="WebPromptBuilder.documents")
```

![](img/df2d29d845a86b3065b7116302337cd7_53.png)

现在，完整的管道已经构建完毕。其逻辑流程如下图所示（用文字描述）：
1.  用户查询进入基础RAG管道。
2.  生成器产生回复。
3.  条件路由器检查回复。
    *   如果**不包含**“无答案”，则从`answer`端口直接输出回复。
    *   如果**包含**“无答案”，则将原始查询从`go_to_web_search`端口输出。
4.  `go_to_web_search`的查询触发网络搜索。
5.  网络搜索结果和原始查询被送入新的提示构建器和生成器，产生最终答案。

![](img/df2d29d845a86b3065b7116302337cd7_55.png)

## 测试完整管道

![](img/df2d29d845a86b3065b7116302337cd7_57.png)

让我们用几个问题来测试完整的管道。

![](img/df2d29d845a86b3065b7116302337cd7_58.png)

首先，测试一个RAG管道能够回答的问题：

```python
result = full_pipeline.run(query="生成器的用途是什么？")
print(result['answer'])
```
输出应为来自基础RAG管道的答案。

![](img/df2d29d845a86b3065b7116302337cd7_60.png)

其次，测试一个RAG管道无法回答、需要回退到网络搜索的问题：

![](img/df2d29d845a86b3065b7116302337cd7_62.png)

```python
result = full_pipeline.run(query="法国的首都是什么？")
# 结果将来自 WebGenerator
print(result.get('WebGenerator', {}).get('replies', ['No reply'])[0])
```
输出应是一个基于网络搜索生成的答案，例如：“根据网络搜索结果，法国的首都是巴黎。”

你可以尝试更多问题，例如“海斯塔克有什么卡希尔组件？”（应触发网络搜索）和“提示构建器是做什么的？”（应由基础RAG回答）。

![](img/df2d29d845a86b3065b7116302337cd7_64.png)

## 总结

![](img/df2d29d845a86b3065b7116302337cd7_66.png)

在本节课中，我们一起学习了如何构建一个具有分支能力的智能管道。我们首先回顾了基础RAG管道的构建，然后引入了**条件路由器**组件来实现核心的分支逻辑。最后，我们构建了网络搜索分支，完成了完整的回退机制。

通过本节课，你掌握了：
1.  **条件路由器**的使用方法，它通过`RouteCondition`对象定义分支逻辑。
2.  构建复杂管道的能力，能够将不同的处理流程（如本地RAG和网络搜索RAG）动态连接起来。
3.  实现一个健壮问答系统的思路：优先使用可靠的本体知识（数据库），当其不足时，智能地回退到外部知识源（互联网）。

这种模式极大地增强了AI应用的鲁棒性和实用性，使其能够应对更广泛、更开放领域的问题。