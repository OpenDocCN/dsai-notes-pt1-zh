# 031：代理 (Agents) 🤖

![](img/b3647b51d77e6b78045ac95d56632965_0.png)

在本节课中，我们将学习 LangChain 框架中一个非常强大且令人兴奋的部分——代理。代理允许我们将大型语言模型视为一个**推理引擎**，而不是一个静态的知识库。通过为代理配备不同的工具，它可以与外部数据源、API 或计算功能交互，从而完成更复杂的任务。

---

![](img/b3647b51d77e6b78045ac95d56632965_2.png)

## 环境设置与初始化 ⚙️

![](img/b3647b51d77e6b78045ac95d56632965_4.png)

首先，我们需要设置环境并导入必要的库。我们将初始化一个语言模型，并加载一些工具供代理使用。

![](img/b3647b51d77e6b78045ac95d56632965_6.png)

![](img/b3647b51d77e6b78045ac95d56632965_8.png)

```python
# 导入必要的库并设置环境变量
import os
# ... (假设已设置OPENAI_API_KEY等环境变量)

![](img/b3647b51d77e6b78045ac95d56632965_10.png)

# 初始化语言模型
from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(temperature=0)
```

![](img/b3647b51d77e6b78045ac95d56632965_12.png)

我们将 `temperature` 设置为 **0**，这很重要，因为我们希望作为推理引擎的语言模型尽可能精确和可靠。

![](img/b3647b51d77e6b78045ac95d56632965_14.png)

![](img/b3647b51d77e6b78045ac95d56632965_16.png)

---

![](img/b3647b51d77e6b78045ac95d56632965_18.png)

![](img/b3647b51d77e6b78045ac95d56632965_20.png)

## 加载工具 🛠️

![](img/b3647b51d77e6b78045ac95d56632965_22.png)

接下来，我们加载两个内置工具：一个用于数学计算，另一个用于查询维基百科。

![](img/b3647b51d77e6b78045ac95d56632965_24.png)

```python
# 加载工具
from langchain.agents import load_tools
tools = load_tools(["llm-math", "wikipedia"], llm=llm)
```

![](img/b3647b51d77e6b78045ac95d56632965_26.png)

![](img/b3647b51d77e6b78045ac95d56632965_28.png)

*   **`llm-math` 工具**：这是一个结合了语言模型和计算器的链，专门用于解决数学问题。
*   **`wikipedia` 工具**：这是一个连接到维基百科 API 的工具，允许代理执行搜索查询并获取结果。

![](img/b3647b51d77e6b78045ac95d56632965_30.png)

---

![](img/b3647b51d77e6b78045ac95d56632965_32.png)

## 初始化代理 🤖

![](img/b3647b51d77e6b78045ac95d56632965_34.png)

![](img/b3647b51d77e6b78045ac95d56632965_36.png)

现在，我们使用加载的工具和语言模型来初始化一个代理。

![](img/b3647b51d77e6b78045ac95d56632965_38.png)

![](img/b3647b51d77e6b78045ac95d56632965_40.png)

```python
# 初始化代理
from langchain.agents import initialize_agent
from langchain.agents import AgentType

![](img/b3647b51d77e6b78045ac95d56632965_42.png)

agent = initialize_agent(
    tools,
    llm,
    agent=AgentType.CHAT_ZERO_SHOT_REACT_DESCRIPTION,
    handle_parsing_errors=True,
    verbose=True
)
```

![](img/b3647b51d77e6b78045ac95d56632965_44.png)

![](img/b3647b51d77e6b78045ac95d56632965_46.png)

以下是关键参数的解释：
*   **`agent=AgentType.CHAT_ZERO_SHOT_REACT_DESCRIPTION`**：
    *   `CHAT` 表示此代理针对聊天模型进行了优化。
    *   `ZERO_SHOT_REACT_DESCRIPTION` 是一种提示技术，旨在从语言模型中获取最佳的推理性能。
*   **`handle_parsing_errors=True`**：当语言模型的输出格式无法被解析为有效的“动作”和“动作输入”时，此设置会将错误信息传回给语言模型，让它自我纠正。
*   **`verbose=True`**：此设置会打印出代理执行过程中的详细步骤，便于我们理解其内部运作。

![](img/b3647b51d77e6b78045ac95d56632965_48.png)

![](img/b3647b51d77e6b78045ac95d56632965_50.png)

---

## 使用代理解决问题 🧩

![](img/b3647b51d77e6b78045ac95d56632965_52.png)

![](img/b3647b51d77e6b78045ac95d56632965_54.png)

上一节我们介绍了如何初始化一个配备了工具的代理。本节中，我们来看看代理如何利用这些工具解决实际问题。

![](img/b3647b51d77e6b78045ac95d56632965_56.png)

![](img/b3647b51d77e6b78045ac95d56632965_58.png)

### 示例1：解决数学问题 ➗

我们首先问代理一个简单的数学问题。

```python
# 向代理提问
result = agent.run("三百的百分之二十五是多少？")
print(result) # 输出：75.0
```

当 `verbose=True` 时，我们可以看到代理的思考过程：
1.  **思考**：代理分析问题，认为需要使用计算器。
2.  **动作**：它决定调用 `Calculator` 工具。
3.  **动作输入**：它将问题转化为计算器能理解的输入 `300 * 0.25`。
4.  **观察**：计算器工具返回结果 `75.0`。
5.  **最终答案**：代理接收观察结果，并最终输出答案 `75.0`。

![](img/b3647b51d77e6b78045ac95d56632965_60.png)

![](img/b3647b51d77e6b78045ac95d56632965_62.png)

这个流程展示了代理如何将自然语言问题分解，选择合适的工具执行，并整合结果。

---

![](img/b3647b51d77e6b78045ac95d56632965_64.png)

### 示例2：查询维基百科 📚

![](img/b3647b51d77e6b78045ac95d56632965_66.png)

![](img/b3647b51d77e6b78045ac95d56632965_68.png)

接下来，我们让代理查询一个需要外部知识的问题。

![](img/b3647b51d77e6b78045ac95d56632965_70.png)

![](img/b3647b51d77e6b78045ac95d56632965_72.png)

```python
result = agent.run("汤姆·米切尔写了哪本书？")
print(result) # 输出：机器学习 (Machine Learning)
```

![](img/b3647b51d77e6b78045ac95d56632965_74.png)

观察代理的步骤：
1.  **思考**：代理意识到需要查找汤姆·米切尔的信息。
2.  **动作**：它决定使用 `Wikipedia` 工具。
3.  **动作输入**：搜索关键词 `汤姆·米切尔`。
4.  **观察**：维基百科返回了包含计算机科学家汤姆·米切尔信息的摘要，其中提到了他写的书《机器学习》。
5.  **最终答案**：代理从摘要中提取信息并给出答案。

![](img/b3647b51d77e6b78045ac95d56632965_76.png)

![](img/b3647b51d77e6b78045ac95d56632965_78.png)

这个例子也说明了代理并不总是完全可靠。有时它可能会执行多余的步骤（例如，额外搜索“机器学习书籍”），但最终仍能基于获取的信息得出正确答案。

![](img/b3647b51d77e6b78045ac95d56632965_80.png)

![](img/b3647b51d77e6b78045ac95d56632965_82.png)

---

![](img/b3647b51d77e6b78045ac95d56632965_84.png)

![](img/b3647b51d77e6b78045ac95d56632965_85.png)

## 创建Python代码执行代理 💻

代理的强大之处在于它可以与各种功能交互。现在，我们创建一个可以编写并执行Python代码的代理。

![](img/b3647b51d77e6b78045ac95d56632965_87.png)

![](img/b3647b51d77e6b78045ac95d56632965_89.png)

首先，我们加载一个特殊的工具——`Python REPL`。REPL（读取-求值-输出循环）可以理解为一个交互式的代码执行环境，类似于Jupyter Notebook。

![](img/b3647b51d77e6b78045ac95d56632965_91.png)

```python
# 创建Python代理
from langchain.agents import load_tools
python_tools = load_tools(["python_repl"])
python_agent = initialize_agent(
    python_tools,
    llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)
```

![](img/b3647b51d77e6b78045ac95d56632965_93.png)

然后，我们让这个代理解决一个编程问题：对一个名字列表进行排序。

![](img/b3647b51d77e6b78045ac95d56632965_95.png)

![](img/b3647b51d77e6b78045ac95d56632965_97.png)

```python
# 向Python代理提问
question = """
请按以下顺序处理这个客户列表：['哈里森', '蔡斯', 'LangChain LLC', '杰夫', 'Fusion Transformer', '生成式AI']。
首先，按姓氏排序（如果没有姓氏，则按全名排序）。
然后，按名字排序。
最后，请打印出排序后的列表。
"""
result = python_agent.run(question)
print(result)
```

代理的思考过程如下：
1.  **思考**：代理认识到这是一个列表排序任务，需要编写Python代码。
2.  **动作**：它调用 `Python REPL` 工具。
3.  **动作输入**：它生成了一段Python代码。这段代码会：
    *   创建客户列表变量。
    *   定义一个函数来提取姓氏（或全名）和名字。
    *   使用 `sorted` 函数和自定义的键（key）进行排序。
    *   打印排序后的结果。
4.  **观察**：`Python REPL` 执行代码并返回打印的输出。
5.  **最终答案**：代理将代码执行的结果作为最终答案返回。

![](img/b3647b51d77e6b78045ac95d56632965_99.png)

通过启用LangChain的调试模式（`langchain.debug = True`），我们可以更深入地看到每一步的详细输入和输出，包括传递给语言模型的完整提示、工具的确切调用参数等，这对于理解和调试代理行为非常有帮助。

---

![](img/b3647b51d77e6b78045ac95d56632965_101.png)

## 创建自定义工具 🔧

到目前为止，我们使用的都是LangChain内置的工具。但代理真正的威力在于能够连接到你自己的数据源和API。本节中，我们来看看如何创建自定义工具。

我们将创建一个简单的工具，用于返回当前日期。

![](img/b3647b51d77e6b78045ac95d56632965_103.png)

```python
# 导入工具装饰器
from langchain.tools import tool
from datetime import datetime

![](img/b3647b51d77e6b78045ac95d56632965_105.png)

# 使用@tool装饰器将函数转换为工具
@tool
def time(text: str) -> str:
    """
    当需要知道当前日期时，使用此工具。
    输入应始终为空字符串。
    """
    # 此函数忽略输入文本，直接返回当前日期
    return datetime.now().strftime("%Y-%m-%d")
```

![](img/b3647b51d77e6b78045ac95d56632965_107.png)

![](img/b3647b51d77e6b78045ac95d56632965_109.png)

**关键点**：
*   `@tool` 装饰器将任何函数标记为LangChain代理可用的工具。
*   函数的**文档字符串 (docstring)** 至关重要。代理通过阅读它来了解：
    *   何时应该调用此工具（“当需要知道当前日期时”）。
    *   如何调用此工具（“输入应始终为空字符串”）。

![](img/b3647b51d77e6b78045ac95d56632965_111.png)

现在，我们将这个自定义工具与其他工具一起加载，并创建一个新的代理。

![](img/b3647b51d77e6b78045ac95d56632965_113.png)

![](img/b3647b51d77e6b78045ac95d56632965_115.png)

```python
# 创建包含自定义工具的代理
custom_tools = load_tools(["llm-math", "wikipedia"], llm=llm)
custom_tools.append(time) # 添加自定义的时间工具

![](img/b3647b51d77e6b78045ac95d56632965_117.png)

custom_agent = initialize_agent(
    custom_tools,
    llm,
    agent=AgentType.CHAT_ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)

![](img/b3647b51d77e6b78045ac95d56632965_119.png)

![](img/b3647b51d77e6b78045ac95d56632965_121.png)

# 使用代理
result = custom_agent.run("今天的日期是什么？")
print(result) # 输出类似：今天的日期是2023-05-21。
```

![](img/b3647b51d77e6b78045ac95d56632965_123.png)

代理会识别出问题与日期相关，查阅工具描述，然后调用我们的 `time` 工具，并按照文档字符串的指示传入空字符串，最后将工具返回的日期信息回答给用户。

![](img/b3647b51d77e6b78045ac95d56632965_125.png)

你可以遵循这个模式，创建连接数据库、调用内部API或处理特定文件格式的自定义工具，从而极大地扩展代理的能力边界。

![](img/b3647b51d77e6b78045ac95d56632965_127.png)

![](img/b3647b51d77e6b78045ac95d56632965_129.png)

---

![](img/b3647b51d77e6b78045ac95d56632965_131.png)

## 总结 📝

![](img/b3647b51d77e6b78045ac95d56632965_133.png)

![](img/b3647b51d77e6b78045ac95d56632965_135.png)

在本节课中，我们一起学习了LangChain框架中的代理。

1.  **代理的核心思想**：我们将大型语言模型视为一个**推理引擎**，它能够理解目标、规划步骤、调用工具并综合结果，而不仅仅是回答记忆中的知识。
2.  **使用内置工具**：我们实践了如何使用 `llm-math` 和 `wikipedia` 等内置工具，让代理解决数学问题和进行事实查询。
3.  **代码执行代理**：我们创建了能够编写和执行Python代码的代理，使其可以完成数据处理等编程任务。
4.  **创建自定义工具**：我们学习了如何使用 `@tool` 装饰器创建自定义工具，这是将代理连接到专有数据、API或业务逻辑的关键。

![](img/b3647b51d77e6b78045ac95d56632965_137.png)

代理是LangChain中较新且功能强大的部分，它开启了将语言模型与外部世界连接起来的无限可能。希望本教程能帮助你开始构建自己的智能代理应用。