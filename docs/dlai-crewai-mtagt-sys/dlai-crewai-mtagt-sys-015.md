# 015：多智能体协作进行金融分析 📈

在本节课中，我们将学习如何使用 crewAI 框架构建一个用于金融分析的多智能体协作系统。我们将创建四个具有不同职责的智能体，并通过分层流程让它们协同工作，完成从市场分析到风险评估的完整股票投资分析流程。

---

## 概述与准备工作 🛠️

首先，我们需要导入必要的类并设置环境。以下是核心步骤：

![](img/fdddacce10a1834d4ab590dc79b82ca3_1.png)

1.  **导入核心类**：我们需要从 crewAI 导入 `Agent`、`Task` 和 `Crew` 这三个核心类。
2.  **配置语言模型与工具**：我们将使用 GPT-4 Turbo 模型，并配置两个工具：`SerperDevTool`（用于网络搜索）和 `ScrapeWebsiteTool`（用于抓取特定网页内容）。你需要提供相应的 API 密钥。
3.  **定义智能体**：我们将创建四个智能体，分别负责数据分析、交易策略、交易执行和风险管理。

![](img/fdddacce10a1834d4ab590dc79b82ca3_2.png)

以下是初始设置的代码框架：

```python
from crewai import Agent, Task, Crew
from crewai.process import Process

![](img/fdddacce10a1834d4ab590dc79b82ca3_4.png)

# 配置 LLM 和工具（此处为示例，需替换为你的 API 密钥）
from langchain_openai import ChatOpenAI
from crewai_tools import SerperDevTool, ScrapeWebsiteTool

llm = ChatOpenAI(model="gpt-4-turbo", api_key="你的-OpenAI-API-密钥")
search_tool = SerperDevTool(api_key="你的-Serper-API-密钥")
scrape_tool = ScrapeWebsiteTool()

# 注意：实际代码中需要正确安装和导入相关库。
```

![](img/fdddacce10a1834d4ab590dc79b82ca3_6.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_7.png)

---

## 创建智能体团队 👥

上一节我们完成了环境设置，本节中我们来定义执行金融分析任务的四个智能体。每个智能体都有明确的角色、目标和工具。

以下是四个智能体的定义：

*   **数据分析师**：负责分析市场，寻找潜在的投资机会。
    *   **角色**：资深金融数据分析师
    *   **目标**：找出最具增长潜力的股票
    *   **工具**：`search_tool`, `scrape_tool`
*   **交易策略师**：负责根据找到的股票和用户风险偏好，制定交易策略。
    *   **角色**：股票交易策略专家
    *   **目标**：制定最优的买入/卖出策略
    *   **工具**：`search_tool`, `scrape_tool`
*   **交易执行员**：负责确定具体的交易时机、价格和入场点。
    *   **角色**：交易执行专家
    *   **目标**：确定最佳交易执行参数
    *   **工具**：`search_tool`, `scrape_tool`
*   **风险管理师**：负责评估投资组合的整体风险，确保风险可控。
    *   **角色**：金融风险管理师
    *   **目标**：全面评估并报告潜在风险
    *   **工具**：`search_tool`, `scrape_tool`

![](img/fdddacce10a1834d4ab590dc79b82ca3_9.png)

**重要提示**：本教程仅为展示多智能体系统能力的示例，**不构成任何投资建议**。在实际应用中，已有大型企业使用类似技术进行金融文档分析。

---

![](img/fdddacce10a1834d4ab590dc79b82ca3_11.png)

## 定义任务流程 📋

![](img/fdddacce10a1834d4ab590dc79b82ca3_12.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_13.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_14.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_15.png)

智能体创建好后，我们需要为它们分配具体的工作任务。任务定义了智能体需要完成的具体工作内容。

以下是分配给四个智能体的任务列表：

*   **市场分析任务**：由数据分析师执行。监控市场，寻找有潜力的股票。
*   **策略制定任务**：由交易策略师执行。基于选定的股票和用户风险承受能力，决定如何操作。
*   **交易执行计划任务**：由交易执行员执行。确定具体的价格、入场时机和退出点。
*   **风险评估任务**：由风险管理师执行。在给出最终建议前，综合评估所有风险因素。

![](img/fdddacce10a1834d4ab590dc79b82ca3_17.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_18.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_19.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_20.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_21.png)

每个任务都会指定负责的智能体、任务描述和期望输出。

![](img/fdddacce10a1834d4ab590dc79b82ca3_22.png)

---

## 配置分层协作流程 🏗️

现在，我们有了智能体和任务，需要将它们组织成一个团队（Crew）。这里我们将使用一种新的协作模式：**分层流程**。

![](img/fdddacce10a1834d4ab590dc79b82ca3_24.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_25.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_26.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_27.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_28.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_29.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_30.png)

与之前按顺序执行的“顺序流程”不同，分层流程会**自动创建一个管理者智能体**。这个管理者的工作是协调和委派工作给其他智能体。

![](img/fdddacce10a1834d4ab590dc79b82ca3_31.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_32.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_33.png)

关键代码差异如下：
```python
# 创建 Crew 并指定流程为分层模式
crew = Crew(
    agents=[data_analyst_agent, strategy_agent, execution_agent, risk_agent],
    tasks=[analysis_task, strategy_task, execution_task, risk_task],
    process=Process.hierarchical, # 使用分层流程
    manager_llm=llm, # 指定管理者使用的LLM
    verbose=True
)
```
通过设置 `process=Process.hierarchical`，CrewAI 会在内部自动创建一个管理者，负责工作的分配与调度。在未来的版本中，你将可以自定义这个管理者智能体。

---

## 运行与分析 🚀

![](img/fdddacce10a1834d4ab590dc79b82ca3_35.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_36.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_37.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_38.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_39.png)

一切就绪后，我们可以为任务提供输入并启动整个分析流程。输入参数可以包括股票名称、初始资金、风险承受能力等。

启动命令很简单：
```python
# 定义输入
inputs = {
    'stock_selection': 'AAPL',
    'initial_capital': '100000',
    'risk_tolerance': '中等'
}
# 启动 Crew
result = crew.kickoff(inputs=inputs)
print(result)
```
**建议**：你可以暂停并尝试修改这些输入参数（如换成你关心的股票、调整资金量等），观察智能体们的行为和输出如何随之变化。

当 Crew 启动后，你可以从日志中清晰地看到分层协作的过程：
1.  **管理者**首先将工作委派给**数据分析师**。
2.  数据分析师使用搜索工具查找苹果公司当前股价等信息，并利用网页抓取工具从雅虎财经等网站提取详细内容。
3.  数据分析师将分析结果汇报给管理者。
4.  管理者根据结果，将下一步工作（如制定策略）委派给**交易策略师**，并将已获得的信息传递给它。
5.  此模式不断重复，直到所有智能体都完成了自己的任务，最终由管理者汇总生成一份完整的报告。

![](img/fdddacce10a1834d4ab590dc79b82ca3_41.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_42.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_43.png)

在整个执行过程中，智能体们会不断地搜索、阅读、分析信息，并将学习到的内容在团队内传递，由管理者智能体进行全局协调。

![](img/fdddacce10a1834d4ab590dc79b82ca3_44.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_45.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_46.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_47.png)

---

## 结果解读与总结 📊

经过一系列的分析与协作，Crew 最终输出了一份关于苹果公司股票交易的**综合风险分析报告**。

![](img/fdddacce10a1834d4ab590dc79b82ca3_49.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_50.png)

![](img/fdddacce10a1834d4ab590dc79b82ca3_52.png)

报告内容通常包括：
*   **概述**：对股票和策略的总体看法。
*   **技术指标建议**：例如使用 **20日与200日移动平均线（SMA）**、**相对强弱指数（RSI）** 等来识别市场时机。
*   **风险管理措施**：建议设置**止盈（stop win）和止损（stop loss）** 点位。
*   **潜在风险提示**：列出需要关注的市场波动、运营挑战等风险因素。
*   **结论与建议**：总结分析结果，给出是否投资以及如何谨慎部署资金的最终建议。

**本节课中我们一起学习了**如何使用 crewAI 构建一个复杂的、用于金融分析的多智能体系统。我们从简单的文章写作 Crew 开始，现在已经能够构建具备完整工作流、能进行网络研究、风险分析并行工作的复杂系统。这为自动化处理复杂问题打开了巨大的可能性。

**鼓励你进行以下探索**：
1.  将流程从 `hierarchical` 改回 `sequential`，观察工作流有何不同。
2.  尝试移除或修改某个智能体或任务，看看结果如何变化。
3.  用你感兴趣的股票和参数运行整个流程，构建对你有价值的分析工具。

![](img/fdddacce10a1834d4ab590dc79b82ca3_54.png)

通过本课程，你已经掌握了多智能体系统（特别是 crewAI 框架）的核心组件：**Crew（团队）**、**Agent（智能体）**、**Task（任务）** 和 **Process（流程）**。你应该已经准备好构建更复杂的智能体系统了。