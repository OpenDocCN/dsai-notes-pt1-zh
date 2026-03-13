# 007：代理 🧠

![](img/3ff7beb031a24989fc152622b2092ecc_0.png)

在本节课中，我们将要学习LangChain中的**代理**框架。代理允许我们将大语言模型视为一个**推理引擎**，而非仅仅是知识库。通过代理，模型可以调用外部工具（如计算器、搜索引擎或自定义API）来获取信息、进行计算并决定下一步行动，从而完成更复杂的任务。

---

## 环境与模型初始化 ⚙️

上一节我们介绍了代理的基本概念，本节中我们来看看如何设置环境并初始化模型。

首先，我们需要设置环境变量并导入必要的库。然后，我们将初始化语言模型。这里我们使用`ChatOpenAI`，并将温度设置为`0`，以确保模型作为推理引擎时输出稳定且精确。

![](img/3ff7beb031a24989fc152622b2092ecc_2.png)

![](img/3ff7beb031a24989fc152622b2092ecc_4.png)

```python
from langchain.chat_models import ChatOpenAI

![](img/3ff7beb031a24989fc152622b2092ecc_6.png)

llm = ChatOpenAI(temperature=0)
```

![](img/3ff7beb031a24989fc152622b2092ecc_8.png)

---

![](img/3ff7beb031a24989fc152622b2092ecc_10.png)

## 加载与使用内置工具 🛠️

![](img/3ff7beb031a24989fc152622b2092ecc_12.png)

![](img/3ff7beb031a24989fc152622b2092ecc_14.png)

初始化模型后，我们需要为代理配备工具。LangChain提供了一些内置工具，例如用于数学计算的`LLMMathTool`和用于查询的`Wikipedia`工具。

![](img/3ff7beb031a24989fc152622b2092ecc_16.png)

以下是加载这两个工具的代码：

![](img/3ff7beb031a24989fc152622b2092ecc_18.png)

![](img/3ff7beb031a24989fc152622b2092ecc_20.png)

```python
from langchain.agents import load_tools

![](img/3ff7beb031a24989fc152622b2092ecc_22.png)

![](img/3ff7beb031a24989fc152622b2092ecc_24.png)

tools = load_tools(["llm-math", "wikipedia"], llm=llm)
```

![](img/3ff7beb031a24989fc152622b2092ecc_26.png)

*   **`llm-math`工具**：本身是一个链，结合了语言模型和计算器来解决数学问题。
*   **`wikipedia`工具**：一个API包装器，允许代理对维基百科执行搜索查询并获取结果。

![](img/3ff7beb031a24989fc152622b2092ecc_28.png)

![](img/3ff7beb031a24989fc152622b2092ecc_30.png)

---

## 创建并运行代理 🤖

![](img/3ff7beb031a24989fc152622b2092ecc_32.png)

现在，我们可以使用加载的工具和语言模型来初始化一个代理。我们将使用`initialize_agent`函数，并指定代理类型为`"chat-zero-shot-react-description"`。这个类型专为与聊天模型配合使用而优化，并采用了“ReAct”提示技术以获得更好的推理性能。

![](img/3ff7beb031a24989fc152622b2092ecc_34.png)

![](img/3ff7beb031a24989fc152622b2092ecc_36.png)

```python
from langchain.agents import initialize_agent

![](img/3ff7beb031a24989fc152622b2092ecc_38.png)

![](img/3ff7beb031a24989fc152622b2092ecc_40.png)

agent = initialize_agent(
    tools,
    llm,
    agent="chat-zero-shot-react-description",
    handle_parsing_errors=True, # 处理模型输出解析错误
    verbose=True # 打印详细执行步骤
)
```

![](img/3ff7beb031a24989fc152622b2092ecc_42.png)

代理创建完成后，我们就可以向它提问了。让我们先问一个简单的数学问题。

![](img/3ff7beb031a24989fc152622b2092ecc_44.png)

```python
agent.run("三百的百分之二十五是多少？")
```

![](img/3ff7beb031a24989fc152622b2092ecc_46.png)

执行时，`verbose=True`会让我们看到代理的思考过程：
1.  **思考**：模型分析问题，决定需要做什么。
2.  **动作**：模型输出一个JSON结构，指定要使用的工具（如`Calculator`）和输入（如`"300*0.25"`）。
3.  **观察**：工具执行并返回结果（如`75.0`）。
4.  **最终答案**：模型根据观察结果，生成最终答案返回给用户。

![](img/3ff7beb031a24989fc152622b2092ecc_48.png)

![](img/3ff7beb031a24989fc152622b2092ecc_50.png)

---

## 探索更复杂的查询：维基百科示例 🔍

![](img/3ff7beb031a24989fc152622b2092ecc_52.png)

![](img/3ff7beb031a24989fc152622b2092ecc_54.png)

接下来，我们看看代理如何处理需要外部知识的问题。我们将询问关于“汤姆·米切尔”的信息。

![](img/3ff7beb031a24989fc152622b2092ecc_56.png)

```python
agent.run("汤姆·米切尔写了哪本书？")
```

![](img/3ff7beb031a24989fc152622b2092ecc_58.png)

代理会识别出需要使用`Wikipedia`工具，并执行搜索。它可能得到多个结果（例如，同名的计算机科学家和足球运动员）。通过阅读返回的摘要，代理能够推理出正确的答案（《机器学习》），并最终给出回应。这个过程展示了代理如何串联使用工具和推理能力。

---

## 创建代码执行代理 💻

代理的一个强大功能是能够编写并执行代码。这类似于ChatGPT的代码解释器插件。我们将创建一个使用`PythonREPLTool`的代理，它可以在一个安全的沙箱环境中运行Python代码。

以下是创建Python代理的步骤：

![](img/3ff7beb031a24989fc152622b2092ecc_60.png)

![](img/3ff7beb031a24989fc152622b2092ecc_62.png)

```python
from langchain.agents import load_tools

python_tools = load_tools(["python_repl"])
python_agent = initialize_agent(
    python_tools,
    llm,
    agent="chat-zero-shot-react-description",
    verbose=True
)
```

![](img/3ff7beb031a24989fc152622b2092ecc_64.png)

现在，我们可以让这个代理解决一个编程问题，例如对一个名字列表进行排序。

![](img/3ff7beb031a24989fc152622b2092ecc_66.png)

```python
question = """
请按姓氏对以下名字列表进行排序，然后按名字排序，并打印结果：
[‘哈里森·蔡斯’， ‘LangChain LLM’， ‘杰夫·融合’， ‘变压器·生成AI’]
"""
python_agent.run(question)
```

![](img/3ff7beb031a24989fc152622b2092ecc_68.png)

![](img/3ff7beb031a24989fc152622b2092ecc_70.png)

代理会思考如何用代码解决这个问题，然后生成并执行相应的Python代码（如使用`sorted`函数和`lambda`表达式），最后将代码的输出作为观察结果返回，并整理出最终答案。

![](img/3ff7beb031a24989fc152622b2092ecc_72.png)

---

![](img/3ff7beb031a24989fc152622b2092ecc_74.png)

## 深入调试与理解内部机制 🔬

![](img/3ff7beb031a24989fc152622b2092ecc_76.png)

![](img/3ff7beb031a24989fc152622b2092ecc_78.png)

为了更深入地理解代理的工作流程，我们可以开启LangChain的调试模式。这将打印出链中所有层级的详细输入和输出，帮助我们看清模型接收的完整提示、工具调用的细节以及中间状态的变化。

![](img/3ff7beb031a24989fc152622b2092ecc_80.png)

![](img/3ff7beb031a24989fc152622b2092ecc_82.png)

![](img/3ff7beb031a24989fc152622b2092ecc_84.png)

```python
import langchain
langchain.debug = True

![](img/3ff7beb031a24989fc152622b2092ecc_85.png)

# 再次运行代理
agent.run("一个简单的问题")
```

![](img/3ff7beb031a24989fc152622b2092ecc_87.png)

![](img/3ff7beb031a24989fc152622b2092ecc_89.png)

通过调试输出，你可以看到：
*   传递给语言模型的完整**提示模板**，其中包含了工具说明和输出格式要求。
*   语言模型生成的**原始响应**。
*   传递给工具的**精确输入**和工具返回的**输出**。
*   代理如何将历史**上下文（动作、观察）** 组合起来，形成下一步推理的输入。

![](img/3ff7beb031a24989fc152622b2092ecc_91.png)

这对于诊断代理出错的原因或优化提示工程非常有帮助。

![](img/3ff7beb031a24989fc152622b2092ecc_93.png)

![](img/3ff7beb031a24989fc152622b2092ecc_95.png)

---

![](img/3ff7beb031a24989fc152622b2092ecc_97.png)

## 构建自定义工具 🔗

![](img/3ff7beb031a24989fc152622b2092ecc_99.png)

目前我们使用了LangChain的内置工具，但代理的真正优势在于能够连接到任何自定义的数据源或API。接下来，我们将介绍如何创建自定义工具。

我们将创建一个简单的工具，用于返回当前日期。

![](img/3ff7beb031a24989fc152622b2092ecc_101.png)

首先，导入工具装饰器并定义函数：

```python
from langchain.tools import tool
from datetime import datetime

![](img/3ff7beb031a24989fc152622b2092ecc_103.png)

@tool
def time(text: str) -> str:
    """
    当需要知道当前日期时使用此工具。
    输入应始终为空字符串。
    """
    return datetime.now().strftime("%Y-%m-%d")
```

![](img/3ff7beb031a24989fc152622b2092ecc_105.png)

![](img/3ff7beb031a24989fc152622b2092ecc_107.png)

**关键点**：函数的**文档字符串**至关重要。代理通过阅读它来决定何时以及如何调用这个工具。我们需要清晰说明工具的用途和输入格式。

![](img/3ff7beb031a24989fc152622b2092ecc_109.png)

![](img/3ff7beb031a24989fc152622b2092ecc_111.png)

然后，将这个新工具加入到工具列表中，并创建一个新的代理：

![](img/3ff7beb031a24989fc152622b2092ecc_113.png)

![](img/3ff7beb031a24989fc152622b2092ecc_115.png)

```python
custom_tools = [time] + tools # 将自定义工具与之前加载的工具合并
custom_agent = initialize_agent(
    custom_tools,
    llm,
    agent="chat-zero-shot-react-description",
    verbose=True
)

![](img/3ff7beb031a24989fc152622b2092ecc_117.png)

![](img/3ff7beb031a24989fc152622b2092ecc_119.png)

# 现在可以询问日期了
custom_agent.run("今天的日期是什么？")
```

![](img/3ff7beb031a24989fc152622b2092ecc_121.png)

![](img/3ff7beb031a24989fc152622b2092ecc_123.png)

代理会识别出需要使用`time`工具，调用它获取当前日期，并最终给出回答。

![](img/3ff7beb031a24989fc152622b2092ecc_125.png)

![](img/3ff7beb031a24989fc152622b2092ecc_127.png)

---

![](img/3ff7beb031a24989fc152622b2092ecc_129.png)

## 总结 📝

![](img/3ff7beb031a24989fc152622b2092ecc_131.png)

![](img/3ff7beb031a24989fc152622b2092ecc_133.png)

本节课中我们一起学习了LangChain代理框架的核心内容。我们了解到代理如何将大语言模型转变为**推理引擎**，通过调用外部工具来扩展其能力。我们实践了：

![](img/3ff7beb031a24989fc152622b2092ecc_135.png)

1.  如何**初始化代理**并为其加载内置工具（如数学计算和维基百科查询）。
2.  如何观察代理的**思考-行动-观察**循环，理解其问题解决流程。
3.  如何创建**代码执行代理**，让模型能够编写和运行Python代码。
4.  如何利用**调试模式**深入探查代理的内部工作机制。
5.  如何**构建自定义工具**，将代理连接到任意的数据源或API。

![](img/3ff7beb031a24989fc152622b2092ecc_137.png)

代理是LangChain中强大且前沿的部分，它为实现复杂、动态的AI应用打开了大门。希望本教程能帮助你开始构建自己的智能代理应用。