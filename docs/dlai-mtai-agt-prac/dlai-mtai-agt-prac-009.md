# 009：支持数据分析洞察 📊

![](img/85ab78bbb9dcdb62faab450c1c540798_0.png)

在本节课中，我们将学习如何利用AI智能体来分析支持数据，生成洞察、建议和可视化图表。我们将看到智能体如何加载数据、理解客户问题、分析团队表现，并最终生成一份包含表格、图表和建议的综合报告。

---

## 概述

支持数据分析用例将执行一系列操作：处理支持数据、生成改进建议、将数据组织成有意义的表格、绘制可视化图表以展示趋势，并最终将所有内容整合成一份完整的分析报告。这个用例特别之处在于，我们将首次使用能够编写并执行代码来生成图表的智能体。

![](img/85ab78bbb9dcdb62faab450c1c540798_2.png)

---

## 构建用例

上一节我们介绍了用例的目标，本节中我们来看看如何具体构建它。

首先，我们需要导入必要的库和类。这个用例的初始设置与其他用例类似。

```python
# 导入必要的库和CrewAI类
import ...
from crewai import Agent, Task, Crew
```

为了处理复杂的代码生成任务，我们将为智能体使用GPT-4模型。接下来，我们通过加载YAML配置文件来创建智能体和任务。

以下是本用例涉及的三个智能体：
*   **建议引擎智能体**：分析问题报告并提出改进建议。
*   **报告生成器智能体**：将数据组织成表格视图，总结关键指标和趋势。
*   **图表专家智能体**：编写并执行代码，绘制关于问题分布、优先级、解决时间和满意度的图表。

以下是本用例涉及的四个任务：
1.  **建议生成任务**：分析所有问题类型、历史数据、客户认证和反馈。
2.  **表格生成任务**：总结关键指标和趋势，查看问题分类结果和代理性能。
3.  **图表生成任务**：绘制关于问题分布、优先级、解决时间和满意度的图表。
4.  **最终报告汇编任务**：将前三个任务的所有结果整合成一份Markdown格式的最终报告。

---

## 创建智能体与任务

![](img/85ab78bbb9dcdb62faab450c1c540798_4.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_5.png)

现在我们已经了解了智能体和任务，让我们深入代码，实际构建这个Crew。

![](img/85ab78bbb9dcdb62faab450c1c540798_7.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_8.png)

这个Crew将从本地的CSV文件读取支持数据。我们使用CrewAI工具包中的文件读取工具来加载`support_tickets_data.csv`文件。

![](img/85ab78bbb9dcdb62faab450c1c540798_10.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_12.png)

```python
# 使用工具读取CSV文件
from crewai_tools import FileReadTool
csv_tool = FileReadTool(file_path='./support_tickets_data.csv')
```

![](img/85ab78bbb9dcdb62faab450c1c540798_14.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_15.png)

数据文件包含票证ID、客户ID、问题类型等信息。我们的智能体将解析这些信息。

![](img/85ab78bbb9dcdb62faab450c1c540798_16.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_18.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_20.png)

在创建智能体时，我们为建议生成和报告生成智能体提供了访问CSV数据的工具。对于图表生成智能体，我们设置了一个新属性`allow_code_execution=True`，这允许它在受Docker保护的沙箱环境中编写和执行代码，确保与本地机器隔离。

![](img/85ab78bbb9dcdb62faab450c1c540798_22.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_23.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_24.png)

```python
# 创建智能体
suggestion_agent = Agent(
    role='建议引擎',
    goal='分析支持数据并提出改进建议',
    tools=[csv_tool],
    ...
)
chart_agent = Agent(
    role='图表专家',
    goal='创建数据可视化图表',
    allow_code_execution=True, # 允许执行代码
    ...
)
```

![](img/85ab78bbb9dcdb62faab450c1c540798_26.png)

接下来，我们创建四个任务。注意，在最终报告任务中，我们使用了`context`属性，以便它能获取其他三个任务的结果来生成最终报告。

```python
# 创建任务
final_report_task = Task(
    description='汇编包含所有洞察、图表和建议的最终报告',
    agent=report_agent,
    context=[suggestion_task, table_task, chart_task], # 依赖其他任务的结果
    ...
)
```

最后，我们将所有智能体和任务组合成一个Crew。

![](img/85ab78bbb9dcdb62faab450c1c540798_28.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_29.png)

```python
# 创建Crew
analysis_crew = Crew(
    agents=[suggestion_agent, report_agent, chart_agent],
    tasks=[suggestion_task, table_task, chart_task, final_report_task],
    verbose=2
)
```

![](img/85ab78bbb9dcdb62faab450c1c540798_31.png)

---

## 测试与训练智能体

在正式运行Crew之前，我们可以先对其进行测试，以评估智能体的表现。我们使用`crew.test()`方法，并可以指定测试迭代次数和作为评判的LLM模型。

```python
# 测试Crew
test_results = analysis_crew.test(
    n_iterations=3,
    llm_as_judge='gpt-4'
)
```

测试完成后，我们可以训练智能体以提升其表现。训练过程允许我们为每个任务的结果提供反馈，智能体会根据反馈调整其输出并学习，以便在未来做得更好。

```python
# 训练Crew
analysis_crew.train(
    n_iterations=2,
    save_file_path='./trained_crew.pkl'
)
```

![](img/85ab78bbb9dcdb62faab450c1c540798_33.png)

在训练过程中，每个任务完成后，系统会请求反馈。例如，我们可以要求建议更深入，或要求报告包含特定的比较表格。智能体将根据这些反馈重新执行任务并更新其输出逻辑。

训练完成后，再次运行测试，可以比较训练前后的性能得分，以验证训练效果。

---

## 运行与分析结果

经过测试和训练后，我们正式运行Crew来生成最终的报告。

```python
# 运行Crew
final_result = analysis_crew.kickoff()
```

运行完成后，我们将得到一份Markdown格式的综合报告。报告内容可能包括：
*   **问题类型与频率表格**：列出各种问题及其出现次数。
*   **可视化图表**：例如问题分布图、优先级水平图、解决时间趋势图、客户满意度月度趋势图以及代理绩效对比图。
*   **代理绩效分析**：通过表格和图表展示不同支持人员处理的票证数量、解决时间和客户满意度评分。
*   ** actionable 改进建议**：基于数据分析，提出具体的产品功能或支持流程优化建议。

![](img/85ab78bbb9dcdb62faab450c1c540798_35.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_36.png)

![](img/85ab78bbb9dcdb62faab450c1c540798_37.png)

这份报告生动地展示了如何利用多智能体协作，将原始支持数据转化为易于理解、可指导行动的深度业务洞察。

![](img/85ab78bbb9dcdb62faab450c1c540798_39.png)

---

![](img/85ab78bbb9dcdb62faab450c1c540798_41.png)

## 总结

![](img/85ab78bbb9dcdb62faab450c1c540798_43.png)

本节课中，我们一起学习了如何构建一个用于支持数据分析的多AI智能体系统。我们了解了如何配置能执行代码的智能体来生成图表，如何通过测试评估智能体性能，以及如何通过提供反馈来训练智能体，使其输出更符合我们的要求。这个用例展示了AI智能体在自动化数据分析、生成可视化报告方面的强大潜力，此模式可扩展应用于人力资源、代码审查等多种公司内部场景。