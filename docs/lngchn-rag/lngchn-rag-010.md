# 010：路由（Routing）🚦

![](img/19ae6bf2da43f76684260f6335e6439b_1.png)

在本节课中，我们将要学习 RAG 系统中的“路由”环节。路由的核心任务是将用户的问题引导至最合适的数据源或处理流程。我们将探讨两种主要的路由方法：逻辑路由和语义路由，并通过代码示例展示如何实现它们。

上一节我们介绍了查询转换，即通过改写问题来优化检索效果。本节中我们来看看如何将转换后的问题路由到正确的数据源。

## 概述：什么是路由？🧭

路由是指根据用户问题的内容，将其引导至最合适的数据源或处理链的过程。在许多情况下，系统可能拥有多个数据源，例如向量数据库、关系型数据库和图数据库。路由机制能够智能地判断哪个数据源最有可能包含答案。

![](img/19ae6bf2da43f76684260f6335e6439b_3.png)

![](img/19ae6bf2da43f76684260f6335e6439b_4.png)

![](img/19ae6bf2da43f76684260f6335e6439b_5.png)

![](img/19ae6bf2da43f76684260f6335e6439b_6.png)

路由的应用不仅限于选择数据库，它还可以用于选择不同的提示词模板，或者将问题发送到不同的处理模块。

![](img/19ae6bf2da43f76684260f6335e6439b_8.png)

![](img/19ae6bf2da43f76684260f6335e6439b_10.png)

## 逻辑路由 🤖

![](img/19ae6bf2da43f76684260f6335e6439b_12.png)

逻辑路由的核心思想是让大语言模型（LLM）根据对可用数据源的了解，通过逻辑推理来决定将问题路由到哪里。这类似于一个分类任务，结合函数调用功能，让 LLM 输出一个结构化的对象，该对象被约束在几个预定义的选项之中。

![](img/19ae6bf2da43f76684260f6335e6439b_14.png)

以下是实现逻辑路由的关键步骤：

![](img/19ae6bf2da43f76684260f6335e6439b_16.png)

![](img/19ae6bf2da43f76684260f6335e6439b_18.png)

![](img/19ae6bf2da43f76684260f6335e6439b_20.png)

1.  **定义数据结构**：首先，我们需要定义一个数据模型，用于约束 LLM 的输出。例如，我们定义三种可能的数据源：`python_docs`、`js_docs` 和 `golang_docs`。
    ```python
    from pydantic import BaseModel, Field
    from typing import Literal

    class RouteQuery(BaseModel):
        """Route a user query to the most relevant datasource."""
        datasource: Literal["python_docs", "js_docs", "golang_docs"] = Field(
            ...,
            description="Given a user question, choose which datasource would be most relevant for answering their question",
        )
    ```

2.  **绑定到 LLM**：将这个数据模型转换为 OpenAI 的函数调用模式，并绑定到 LLM 上。这样，当我们向 LLM 提问时，它会调用这个“函数”来生成一个符合我们定义的结构化输出。
    ```python
    from langchain_openai import ChatOpenAI

    llm = ChatOpenAI(model="gpt-4o-mini", temperature=0)
    structured_llm = llm.with_structured_output(RouteQuery)
    ```

3.  **创建路由链**：构建一个提示词模板，指导 LLM 根据问题内容进行路由判断。
    ```python
    from langchain_core.prompts import ChatPromptTemplate

    system = """You are an expert at routing a user question to the appropriate data source based on the programming language the user is asking about."""
    prompt = ChatPromptTemplate.from_messages(
        [
            ("system", system),
            ("human", "{question}"),
        ]
    )
    router = prompt | structured_llm
    ```

![](img/19ae6bf2da43f76684260f6335e6439b_22.png)

![](img/19ae6bf2da43f76684260f6335e6439b_23.png)

![](img/19ae6bf2da43f76684260f6335e6439b_25.png)

4.  **执行与后续处理**：向路由链提问，它会返回一个 `RouteQuery` 对象。我们可以根据这个对象中的 `datasource` 字段值，将原始问题发送到对应的处理链（例如，连接到 Python 文档的检索链）。
    ```python
    question = “How do I use list comprehensions in Python?”
    result = router.invoke({“question”: question})
    # result.datasource 的值可能是 “python_docs”
    # 接下来，可以根据这个值选择对应的检索器或处理链
    ```

![](img/19ae6bf2da43f76684260f6335e6439b_26.png)

![](img/19ae6bf2da43f76684260f6335e6439b_27.png)

![](img/19ae6bf2da43f76684260f6335e6439b_29.png)

通过这种方式，我们利用 LLM 的逻辑推理能力和结构化输出，实现了清晰、可控的路由决策。

![](img/19ae6bf2da43f76684260f6335e6439b_30.png)

![](img/19ae6bf2da43f76684260f6335e6439b_32.png)

## 语义路由 🔍

![](img/19ae6bf2da43f76684260f6335e6439b_34.png)

![](img/19ae6bf2da43f76684260f6335e6439b_35.png)

语义路由不依赖于 LLM 的复杂推理，而是利用嵌入向量和相似度计算来实现路由。其基本思路是：预先定义好不同场景下的提示词模板，将它们转换为向量。当用户提问时，同样将问题转换为向量，然后计算它与各个提示词向量的相似度，最后选择相似度最高的提示词模板来处理问题。

以下是实现语义路由的步骤：

1.  **定义提示词模板**：为不同的主题或任务定义专属的提示词。
    ```python
    physics_template = “””You are a very smart physics professor.
    You are great at answering questions about physics in a concise and easy to understand manner.
    When you don’t know the answer to a question you admit that you don’t know.

    Here is a question:
    {input}”””

    math_template = “””You are a very good mathematician. You are great at answering math questions.
    You are so good because you are able to break down hard problems into their component parts,
    answer the component parts, and then put them together to answer the broader question.

    Here is a question:
    {input}”””
    ```

![](img/19ae6bf2da43f76684260f6335e6439b_37.png)

![](img/19ae6bf2da43f76684260f6335e6439b_38.png)

![](img/19ae6bf2da43f76684260f6335e6439b_39.png)

![](img/19ae6bf2da43f76684260f6335e6439b_40.png)

![](img/19ae6bf2da43f76684260f6335e6439b_41.png)

![](img/19ae6bf2da43f76684260f6335e6439b_42.png)

2.  **嵌入提示词**：使用嵌入模型将这些提示词模板转换为向量。
    ```python
    from langchain_openai import OpenAIEmbeddings

    embeddings = OpenAIEmbeddings()
    prompt_infos = [
        (“physics”, physics_template),
        (“math”, math_template),
    ]
    prompt_vectors = [embeddings.embed_query(template) for _, template in prompt_infos]
    ```

![](img/19ae6bf2da43f76684260f6335e6439b_44.png)

![](img/19ae6bf2da43f76684260f6335e6439b_46.png)

![](img/19ae6bf2da43f76684260f6335e6439b_48.png)

3.  **构建路由函数**：创建一个函数，它接收用户问题，计算其嵌入向量与所有提示词向量的相似度，并返回最相似的提示词名称。
    ```python
    from langchain_core.runnables import RunnableLambda

    def semantic_router(input_dict):
        question = input_dict[“question”]
        question_embedding = embeddings.embed_query(question)
        # 计算余弦相似度（示例，实际需实现）
        similarities = [cosine_similarity(question_embedding, vec) for vec in prompt_vectors]
        most_similar_idx = similarities.index(max(similarities))
        return {“prompt_name”: prompt_infos[most_similar_idx][0]}
    ```

4.  **应用路由**：将路由函数集成到链中，根据返回的提示词名称选择对应的模板来最终回答用户问题。
    ```python
    route_chain = RunnableLambda(semantic_router)
    # 假设我们有一个根据 prompt_name 选择模板并调用 LLM 的后续链
    # full_chain = route_chain | select_prompt_chain | llm
    ```

![](img/19ae6bf2da43f76684260f6335e6439b_50.png)

语义路由方法更加直接，依赖于向量空间的几何特性，适合在提示词模板主题区分明显、且对推理过程要求不高的场景下使用。

![](img/19ae6bf2da43f76684260f6335e6439b_52.png)

![](img/19ae6bf2da43f76684260f6335e6439b_53.png)

![](img/19ae6bf2da43f76684260f6335e6439b_54.png)

![](img/19ae6bf2da43f76684260f6335e6439b_55.png)

## 总结 📝

本节课中我们一起学习了 RAG 系统中的路由机制。

*   **逻辑路由**：利用 LLM 的逻辑推理和结构化输出能力，将问题分类并路由到预定义的数据源或处理链。这种方法灵活、可解释性强，适合处理需要复杂判断的路由场景。
*   **语义路由**：基于嵌入向量的相似度计算，将问题匹配到最相关的提示词模板。这种方法实现简单、效率高，适合基于主题或风格进行路由的场景。

![](img/19ae6bf2da43f76684260f6335e6439b_57.png)

![](img/19ae6bf2da43f76684260f6335e6439b_58.png)

![](img/19ae6bf2da43f76684260f6335e6439b_59.png)

![](img/19ae6bf2da43f76684260f6335e6439b_60.png)

![](img/19ae6bf2da43f76684260f6335e6439b_61.png)

![](img/19ae6bf2da43f76684260f6335e6439b_62.png)

这两种工具都非常有用，你可以根据实际应用场景的需求选择或结合使用它们。建议你动手实验，将它们集成到你自己的 RAG 系统中，以构建更智能、更高效的信息检索流程。