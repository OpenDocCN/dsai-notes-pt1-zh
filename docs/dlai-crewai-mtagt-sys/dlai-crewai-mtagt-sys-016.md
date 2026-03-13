# 016：L7 构建定制求职申请的智能体团队

![](img/21e20c9e15fb7f73e3acf247dbc2600b_0.png)

## 概述

在本节课中，我们将构建一个完整的智能体团队，其目标是帮助用户提高获得特定职位面试的机会。我们将通过分析职位描述、匹配个人技能、优化简历，并准备面试要点来实现这一目标。这个案例将综合运用我们之前学到的所有关于智能体、任务和工具的知识。

![](img/21e20c9e15fb7f73e3acf247dbc2600b_2.png)

---

## 构建智能体团队的目标与流程

上一节我们深入探讨了智能体的各种特性。本节中，我们将回到第一课提到的用例，构建一个能帮助你求职的智能体团队。

这个团队的目标是**最大化你获得特定职位面试的机会**。为实现此目标，我们将遵循以下流程：
1.  学习职位要求。
2.  将职位要求与你的特定技能和经验进行交叉比对。
3.  重塑你的简历以突出相关领域。
4.  用合适的语言重写简历。
5.  （额外步骤）让团队准备初步面试的谈话要点，帮助你在面试中更好地展示技能。

---

## 初始化环境与导入工具

首先，我们需要设置基础代码，导入必要的类并配置环境变量。

```python
# 导入核心类
from crewai import Agent, Task, Crew

# 设置环境变量（例如API密钥和模型选择）
# 本示例使用 GPT-4 Turbo 和 Serper API
import os
os.environ["OPENAI_API_KEY"] = "your_openai_api_key"
os.environ["SERPER_API_KEY"] = "your_serper_api_key"
os.environ["OPENAI_MODEL_NAME"] = "gpt-4-turbo"
```

接下来，我们关注本项目中要使用的工具。所有工具都来自 `crewai_tools` 包。

以下是我们要使用的四个工具：
*   `SerperDevTool`：用于在互联网上搜索信息。
*   `ScrapeWebsiteTool`：用于抓取网站内容。
*   `FileReadTool`：限制智能体只能读取特定文件（本例中是一个伪造的简历Markdown文件）。
*   `MDXSearchTool`：允许对我们的伪造简历进行语义搜索（RAG）。

让我们创建这些工具并查看简历文件。

```python
from crewai_tools import (
    SerperDevTool,
    ScrapeWebsiteTool,
    FileReadTool,
    MDXSearchTool
)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_4.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_5.png)

# 实例化工具
search_tool = SerperDevTool()
scrape_tool = ScrapeWebsiteTool()
# 指定只能读取 ‘fake_resume.md‘ 文件
read_resume_tool = FileReadTool(file_path=‘./fake_resume.md‘)
# 对简历文件建立语义搜索
resume_search_tool = MDXSearchTool(mdx=‘./fake_resume.md‘)
```

![](img/21e20c9e15fb7f73e3acf247dbc2600b_6.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_7.png)

---

## 分析简历文件

在继续之前，我们先看一下 `fake_resume.md` 文件的内容，了解智能体将要处理的信息。

![](img/21e20c9e15fb7f73e3acf247dbc2600b_9.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_10.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_11.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_12.png)

这份伪造简历属于一个名叫 **Noah Williams** 的人。简历显示：
*   **个人简介**：他拥有MBA学位，擅长领导力，自称软件工程领导者，在领导远程和现场工程团队方面表现出色。这更像是一个申请管理职位的简历。
*   **技术能力**：深入查看工作经历会发现，Noah 也是一名优秀的工程师，具备 Ruby、Python、JavaScript、TypeScript、Elixir 等经验，并完成过许多出色的技术项目。
*   **核心问题**：根据他申请职位的不同，这些技术能力可能没有被以正确的方式突出显示。

我们的智能体团队需要根据目标职位，智能地调整简历的重点。

---

## 创建四个智能体

现在开始创建智能体。本团队需要四个不同的智能体。

**1. 研究员智能体**
这个智能体负责分析职位发布信息，理解公司希望通过该职位实现的目标，以及他们试图招聘什么样的人。

```python
researcher_agent = Agent(
    role=‘技术职位研究员‘,
    goal=‘分析职位描述，理解其背后的需求、公司目标以及理想的候选人画像‘,
    backstory=‘你是一名擅长深度分析职位描述和市场需求的专家。‘,
    tools=[search_tool, scrape_tool],
    verbose=True
)
```

**2. 档案分析员智能体**
这个智能体负责研究求职者（Noah），通过交叉比对简历和职位描述，找出使此人成为该职位有力竞争者的突出亮点。

```python
profiler_agent = Agent(
    role=‘工程师个人档案分析员‘,
    goal=‘交叉分析求职者简历与职位要求，找出最匹配的优势和经历‘,
    backstory=‘你是一名善于从个人资料中提取关键信息并与外部需求进行匹配的专家。‘,
    tools=[search_tool, scrape_tool, read_resume_tool, resume_search_tool],
    verbose=True
)
```

**3. 简历策略师智能体**
这个智能体将实际负责更新简历。它会学习职位描述和求职者的技能，并围绕两者的交集制定策略。它功能强大，可以访问互联网、读取简历并进行语义搜索。

```python
resume_strategist_agent = Agent(
    role=‘简历策略师‘,
    goal=‘基于职位需求和求职者技能的交集，优化和重写简历‘,
    backstory=‘你是一名专业的简历写手，擅长根据特定职位定制化突出候选人的优势。‘,
    tools=[search_tool, read_resume_tool, resume_search_tool],
    verbose=True
)
```

**4. 工程面试准备员智能体**
这个智能体将帮助我们为潜在的面试做准备，整理相关问题、答案和谈话要点，确保我们能自信地展示自己的技能。

```python
interview_preparer_agent = Agent(
    role=‘工程面试准备员‘,
    goal=‘准备与职位相关的面试问题、答案和谈话要点‘,
    backstory=‘你是一名经验丰富的面试教练，擅长帮助候选人展示其与职位最相关的技能。‘,
    tools=[search_tool, read_resume_tool],
    verbose=True
)
```

---

## 为每个智能体创建任务

智能体创建完毕后，我们需要为每个智能体分配相应的任务。总共四个任务，每个任务对应一个智能体。

以下是各个任务的描述：

**1. 研究工作**
此任务由研究员智能体执行，负责分析输入的职位发布URL。

```python
research_task = Task(
    description=“””
    分析位于以下URL的职位发布：{job_posting_url}
    找出：
    1. 必需的技能和资格。
    2. 期望的经验。
    3. 公司文化和团队动态。
    4. 该职位试图解决的核心问题。
    “””,
    agent=researcher_agent,
    expected_output=“一份关于职位需求、公司目标和理想候选人特征的详细报告。”,
    async_execution=True # 允许并行执行
)
```

![](img/21e20c9e15fb7f73e3acf247dbc2600b_14.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_15.png)

**2. 档案分析工作**
此任务由档案分析员智能体执行，负责分析求职者的GitHub、个人简介等信息。

```python
profiling_task = Task(
    description=“””
    研究求职者以了解其背景。
    使用以下资源：
    1. 简历文件。
    2. GitHub 资料: {github_url}
    3. 个人简述: {personal_writeup}
    找出：
    1. 核心技术栈和熟练程度。
    2. 相关项目经验。
    3. 与目标职位可能匹配的软技能和成就。
    “””,
    agent=profiler_agent,
    expected_output=“一份关于求职者技能、经验及其与目标职位匹配点的综合分析。”,
    async_execution=True # 允许并行执行
)
```

![](img/21e20c9e15fb7f73e3acf247dbc2600b_17.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_18.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_19.png)

**3. 简历策略工作**
此任务由简历策略师智能体执行，负责生成定制化的新简历文件。**注意**：此任务需要等待前两个任务完成，以获取它们的输出作为上下文。

![](img/21e20c9e15fb7f73e3acf247dbc2600b_21.png)

```python
resume_strategy_task = Task(
    description=“””
    使用之前关于职位需求（来自研究任务）和求职者资料（来自档案分析任务）的分析结果。
    重写并优化原始简历，创建一个名为 ‘tailored_resume.md‘ 的新文件。
    重点：
    1. 突出显示与职位最相关的技能和经验。
    2. 使用职位描述中的关键词。
    3. 调整整体叙述，使其与职位要求保持一致。
    4. 保持真实，但进行战略性强调。
    “””,
    agent=resume_strategist_agent,
    expected_output=“一个名为 ‘tailored_resume.md‘ 的新文件，其中包含为特定职位定制化的简历。”,
    output_file=‘./tailored_resume.md‘, # 指定输出文件
    context=[research_task, profiling_task] # 依赖前两个任务
)
```

![](img/21e20c9e15fb7f73e3acf247dbc2600b_23.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_24.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_25.png)

**4. 面试准备工作**
此任务由面试准备员智能体执行，负责生成面试准备材料文件。

```python
interview_prep_task = Task(
    description=“””
    基于职位描述和定制化后的简历，准备面试材料。
    创建一个名为 ‘interview_materials.md‘ 的文件，包含：
    1. 可能被问到的技术问题及建议答案。
    2. 行为面试问题及展示相关经验的要点。
    3. 用于展示与职位相关技能的谈话要点。
    4. 向面试官提问的建议问题。
    “””,
    agent=interview_preparer_agent,
    expected_output=“一个名为 ‘interview_materials.md‘ 的文件，包含全面的面试准备指南。”,
    output_file=‘./interview_materials.md‘
)
```

---

![](img/21e20c9e15fb7f73e3acf247dbc2600b_27.png)

## 组装团队并设置执行流程

![](img/21e20c9e15fb7f73e3acf247dbc2600b_29.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_30.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_31.png)

现在，我们将所有智能体和任务组装成一个团队，并理解其执行流程。

![](img/21e20c9e15fb7f73e3acf247dbc2600b_33.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_34.png)

```python
# 创建团队
job_application_crew = Crew(
    agents=[researcher_agent, profiler_agent, resume_strategist_agent, interview_preparer_agent],
    tasks=[research_task, profiling_task, resume_strategy_task, interview_prep_task],
    verbose=True
)
```

在启动团队之前，我们需要提供所有任务中定义的输入变量。

![](img/21e20c9e15fb7f73e3acf247dbc2600b_36.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_37.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_38.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_39.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_40.png)

```python
# 定义输入
inputs = {
    “job_posting_url”: “https://example.com/job-posting-senior-fullstack”,
    “github_url”: “https://github.com/fake-noah”,
    “personal_writeup”: “我是一名全栈开发者，对构建可扩展的Web应用充满热情，在敏捷团队领导和跨职能协作方面有丰富经验。”
}
```

**执行流程分析**：
1.  `research_task` 和 `profiling_task` 都设置了 `async_execution=True`，因此它们可以**并行执行**。研究员分析职位，档案员分析求职者，节省时间。
2.  `resume_strategy_task` 通过 `context` 参数依赖于前两个任务。即使档案任务先完成，它也会等待研究任务完成，然后才使用两者的结果来优化简历。
3.  `interview_prep_task` 在简历策略任务完成后执行，生成最终的面试材料。

---

![](img/21e20c9e15fb7f73e3acf247dbc2600b_42.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_43.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_44.png)

## 运行团队并查看结果

![](img/21e20c9e15fb7f73e3acf247dbc2600b_45.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_46.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_47.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_48.png)

![](img/21e20c9e15fb7f73e3acf247dbc2600b_49.png)

启动团队并观察其执行过程。

```python
# 运行团队
result = job_application_crew.kickoff(inputs=inputs)
```

执行时，你会看到：
1.  研究员和档案员同时开始工作，一个抓取职位描述，另一个分析GitHub和个人简介。
2.  两者完成后，简历策略师开始工作，读取原始简历，并生成一份新的 `tailored_resume.md`。
3.  最后，面试准备员生成 `interview_materials.md` 文件。

![](img/21e20c9e15fb7f73e3acf247dbc2600b_51.png)

让我们查看生成的结果文件。

**定制化简历 (`tailored_resume.md`)**：
*   **重点转变**：原始简历强调“软件工程领导者”，新简历则突出“拥有软件工程经验的**全栈开发者**”。
*   **技能显化**：原本埋没在文中的Ruby、Python等技能，在新简历中被清晰列出并强调。
*   **内容增补**：新增了MySQL和MongoDB等数据库知识，这些在原始简历中未提及。
*   **经历筛选**：工作经历部分被精简，只保留与目标职位最相关、最能展示匹配度的项目。

**面试材料 (`interview_materials.md`)**：
*   包含模拟的技术问题和行为问题。
*   提供了展示特定框架、语言或数据库管理经验的谈话要点。
*   给出了向面试官提问的建议。
*   这份材料能帮助求职者充满信心、准备充分地参加面试。

---

## 总结

本节课中，我们一起构建了一个非常实用的智能体团队。这个团队能够分析职位需求，交叉比对个人技能，生成定制化的简历，并准备面试要点，从而直接帮助你提高获得面试的机会。

![](img/21e20c9e15fb7f73e3acf247dbc2600b_53.png)

通过这个完整的项目，你综合运用了关于多智能体系统的知识，包括智能体创建、任务定义、工具使用以及任务间的依赖和并行执行控制。你现在已经掌握了使用crewAI构建能够解决实际问题的多AI代理系统的基础能力。