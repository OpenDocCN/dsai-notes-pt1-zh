# 009：客户拓展活动的工具 🛠️

在本节课中，我们将学习如何在 crewAI 框架中为多智能体系统配置和使用工具。我们将创建一个客户拓展活动案例，涉及两个智能体，并使用预置工具以及自定义工具来完成从潜在客户分析到生成个性化沟通邮件的完整流程。

---

## 导入基础类与设置模型

首先，我们需要导入 crewAI 的核心类并设置我们将要使用的语言模型。

```python
from crewai import Agent, Task, Crew
```

上一节我们导入了必要的类，本节中我们来看看如何设置模型。我们将使用 GPT-4 模型，因为它能处理更多的上下文数据。

![](img/ba9f9b6091e89492008fdc13584866d0_1.png)

```python
import os
os.environ["OPENAI_API_KEY"] = "你的OpenAI API密钥"
os.environ["OPENAI_MODEL_NAME"] = "gpt-4"
```

此外，我们还需要设置 Serper API 的密钥，这是一个允许我们进行谷歌搜索的工具。

```python
os.environ["SERPER_API_KEY"] = "你的Serper API密钥"
```

---

![](img/ba9f9b6091e89492008fdc13584866d0_3.png)

## 创建智能体

我们将创建两个智能体来协作完成客户拓展任务。

以下是第一个智能体：销售代表。它的目标是识别符合我们理想客户画像的高价值潜在客户。

```python
sales_rep_agent = Agent(
    role='销售代表',
    goal='识别符合理想客户画像的高价值潜在客户',
    backstory='你在一家专注于AI代理解决方案的尖端AI初创公司工作。你的使命是识别出能够从我们先进的多AI代理系统中受益最多的企业。',
    allow_delegation=False,
    verbose=True
)
```

![](img/ba9f9b6091e89492008fdc13584866d0_5.png)

接下来，我们创建第二个智能体：潜在客户销售代表。它的目标是为已识别的潜在客户寻找个性化内容并建立沟通。

```python
lead_sales_rep_agent = Agent(
    role='潜在客户销售代表',
    goal='为潜在客户寻找个性化内容并建立沟通',
    backstory='你是一位经验丰富的销售代表，擅长通过个性化沟通与潜在客户建立联系。你利用研究来定制你的沟通方式。',
    allow_delegation=False,
    verbose=True
)
```

---

## 配置工具

![](img/ba9f9b6091e89492008fdc13584866d0_7.png)

![](img/ba9f9b6091e89492008fdc13584866d0_8.png)

![](img/ba9f9b6091e89492008fdc13584866d0_9.png)

![](img/ba9f9b6091e89492008fdc13584866d0_10.png)

![](img/ba9f9b6091e89492008fdc13584866d0_11.png)

现在，让我们看看如何为智能体配置工具。我们将使用 crewAI 工具包中的预置工具，并创建一个自定义工具。

首先，导入我们将要使用的预置工具。

```python
from crewai_tools import DirectoryReadTool, FileReadTool, SerperDevTool
```

以下是我们要实例化的三个工具：
1.  **目录读取工具**：允许智能体读取特定目录。
2.  **文件读取工具**：允许智能体读取文件。
3.  **搜索工具**：允许智能体使用 Serper API 搜索互联网。

```python
# 实例化工具，并限制目录读取工具只能访问 `instructions` 文件夹
directory_read_tool = DirectoryReadTool(directory='./instructions')
file_read_tool = FileReadTool()
search_tool = SerperDevTool()
```

`instructions` 目录包含针对不同规模公司（大、中、小型）的处理指南 Markdown 文件。通过配置此工具，智能体可以读取这些文件以获取指导。

---

## 创建自定义工具

除了预置工具，crewAI 也允许我们创建自定义工具。这通过继承 `BaseTool` 类来实现。

以下是创建一个简单的情感分析工具的示例：

![](img/ba9f9b6091e89492008fdc13584866d0_13.png)

![](img/ba9f9b6091e89492008fdc13584866d0_14.png)

```python
from crewai.tools import BaseTool

class SentimentAnalysisTool(BaseTool):
    name: str = "情感分析工具"
    description: str = "分析一段文本的情感倾向。"

    def _run(self, text: str) -> str:
        # 此处可以调用真实的情感分析API
        # 本例中，我们简单地返回“积极”作为示例
        return "积极"

# 实例化自定义工具
sentiment_analysis_tool = SentimentAnalysisTool()
```

![](img/ba9f9b6091e89492008fdc13584866d0_16.png)

自定义工具需要定义 `name`、`description` 和实现 `_run` 方法。`_run` 方法是工具执行具体操作的入口。

![](img/ba9f9b6091e89492008fdc13584866d0_17.png)

---

## 创建任务并分配工具

有了智能体和工具，我们现在可以创建具体的任务，并为每个任务分配合适的工具。

第一个任务是**潜在客户画像分析**。该任务将使用目录读取、文件读取和搜索工具来收集和分析潜在客户信息。

```python
lead_profiling_task = Task(
    description=(
        "对名为 {lead_name} 的潜在客户进行深入分析。"
        "该公司属于 {industry} 行业。"
        "关键决策者是 {key_decision_maker}。"
        "分析其业务规模、所在领域，并找出可能对我们产品感兴趣的相关信息。"
    ),
    expected_output="一份详细的潜在客户分析报告。",
    tools=[directory_read_tool, file_read_tool, search_tool],
    agent=sales_rep_agent,
)
```

第二个任务是**创建个性化沟通**。该任务将使用我们自定义的情感分析工具和搜索工具，基于之前的分析结果来生成个性化的沟通邮件。

![](img/ba9f9b6091e89492008fdc13584866d0_19.png)

![](img/ba9f9b6091e89492008fdc13584866d0_20.png)

![](img/ba9f9b6091e89492008fdc13584866d0_21.png)

```python
personalized_outreach_task = Task(
    description=(
        "基于对 {lead_name} 的分析，创建一封个性化的沟通邮件。"
        "邮件应针对 {key_decision_maker}，并提及他们的里程碑：{product_launch}。"
        "确保邮件内容吸引人且相关。"
    ),
    expected_output="一封准备发送的、完整的个性化邮件草稿。",
    tools=[sentiment_analysis_tool, search_tool],
    agent=lead_sales_rep_agent,
)
```

![](img/ba9f9b6091e89492008fdc13584866d0_22.png)

---

## 组建并运行智能体团队

![](img/ba9f9b6091e89492008fdc13584866d0_24.png)

![](img/ba9f9b6091e89492008fdc13584866d0_25.png)

现在，我们将智能体和任务组合成一个团队（Crew），并运行它。

![](img/ba9f9b6091e89492008fdc13584866d0_26.png)

![](img/ba9f9b6091e89492008fdc13584866d0_27.png)

![](img/ba9f9b6091e89492008fdc13584866d0_28.png)

```python
crew = Crew(
    agents=[sales_rep_agent, lead_sales_rep_agent],
    tasks=[lead_profiling_task, personalized_outreach_task],
    verbose=2,
    memory=True  # 启用记忆功能，有助于工具调用的缓存
)
```

运行团队时，我们需要传入任务描述中定义的变量。

```python
inputs = {
    "lead_name": "Deep Learning AI",
    "industry": "在线学习平台",
    "key_decision_maker": "Andrew (CEO)",
    "product_launch": "新产品发布"
}

result = crew.kickoff(inputs=inputs)
print(result)
```

![](img/ba9f9b6091e89492008fdc13584866d0_30.png)

---

## 执行过程与工具使用观察

当团队开始执行时，销售代表智能体会首先启动。它会使用搜索工具在互联网上查找关于“Deep Learning AI”的信息。

![](img/ba9f9b6091e89492008fdc13584866d0_32.png)

![](img/ba9f9b6091e89492008fdc13584866d0_33.png)

![](img/ba9f9b6091e89492008fdc13584866d0_34.png)

在执行过程中，你可能会观察到智能体尝试用“文件读取工具”去读取一个网页链接，这会导致错误。但 crewAI 框架会优雅地处理这个异常，将错误信息反馈给智能体，而不会中断整个执行流程。智能体随后会调整策略，继续使用搜索工具。

![](img/ba9f9b6091e89492008fdc13584866d0_35.png)

![](img/ba9f9b6091e89492008fdc13584866d0_36.png)

![](img/ba9f9b6091e89492008fdc13584866d0_37.png)

![](img/ba9f9b6091e89492008fdc13584866d0_38.png)

**关于工具异常处理的要点**：
不同的框架对工具异常的处理方式不同。crewAI 选择让异常“优雅失败”，不中断执行，智能体可以从错误中学习。而其他框架或自定义实现可能选择让异常直接停止执行。这是设计多智能体系统时需要考虑的一个重要方面。

当第一个任务完成后，潜在客户销售代表智能体会开始工作。它会利用第一个智能体的研究成果（得益于启用了 `memory=True`，相同的搜索可能会被缓存，从而提高效率并节省资源），并结合情感分析工具，生成一封个性化的沟通邮件。

最终输出将包含针对潜在客户的详细分析报告以及几封不同侧重点的邮件草稿供你选择。

---

![](img/ba9f9b6091e89492008fdc13584866d0_40.png)

![](img/ba9f9b6091e89492008fdc13584866d0_41.png)

![](img/ba9f9b6091e89492008fdc13584866d0_42.png)

![](img/ba9f9b6091e89492008fdc13584866d0_43.png)

![](img/ba9f9b6091e89492008fdc13584866d0_44.png)

![](img/ba9f9b6091e89492008fdc13584866d0_45.png)

## 总结

![](img/ba9f9b6091e89492008fdc13584866d0_46.png)

本节课中我们一起学习了如何在 crewAI 中为智能体配置和使用工具。我们涵盖了：
1.  导入预置工具（`DirectoryReadTool`, `FileReadTool`, `SerperDevTool`）。
2.  创建自定义工具（通过继承 `BaseTool` 类）。
3.  将特定工具分配给不同的任务。
4.  组建并运行一个包含两个智能体的团队，完成从客户分析到生成个性化邮件的销售拓展流程。
5.  观察了工具在实际运行中的交互，并理解了框架对工具异常的处理机制。

![](img/ba9f9b6091e89492008fdc13584866d0_48.png)

销售是 multi-agent 系统一个非常常见且强大的应用场景，因为它天然涉及研究、分析和沟通等多个可自动化的环节。掌握工具的使用，是构建高效智能体工作流的关键一步。