# 007：L3 智能销售管道 🚀

在本节课中，我们将学习如何使用 crewAI 的新功能 **Flows** 来构建一个智能销售管道。我们将从加载潜在客户信息开始，对其进行丰富和评分，最后为高评分客户撰写邮件。整个过程将展示如何将多个 AI 智能体（Crew）与常规 Python 代码结合，实现复杂的自动化工作流。

## 概述：销售管道流程

在开始构建之前，我们先了解整个销售管道的设计思路。

我们将从加载潜在客户数据开始，这可以是常规的 Python 代码，例如从数据库读取。然后，数据将被发送给一个我们即将构建的“潜在客户评分智能体组”。该智能体组将搜索关于该客户的信息，评估其与我们产品和业务的匹配度，并给出评分。

接下来，评分结果将被保存回数据库（同样可以是常规 Python 代码）。我们需要过滤掉评分不高的客户，只保留高评分客户，以便发送给下一个智能体组。

第二个智能体组将利用我们找到的客户信息，撰写一封旨在提高参与度的邮件。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_1.png)

我们将把所有这些步骤封装到一个 **Flow** 中。Flow 通过事件来确保我们按顺序执行所有步骤，并且它拥有一个 **状态（State）**，用于在流程执行期间或之后存储和访问信息。

现在，让我们深入了解这两个智能体组。

## 智能体组设计

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_3.png)

第一个是“潜在客户评分智能体组”。它将执行以下任务：
1.  分析客户数据。
2.  研究该公司的文化背景。
3.  进行最终的客户评分。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_4.png)

如果评分足够高，第二个“邮件撰写智能体组”将接手，执行以下任务：
1.  起草初始邮件。
2.  优化邮件以提高参与度。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_1.png)

需要指出的是，潜在客户评分智能体组将输出**结构化数据**。我们将利用这些数据，并将其传递给邮件撰写智能体组。这展示了如何从智能体和智能体组中获取复杂的结构化输出，非常实用。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_6.png)

## 什么是智能销售管道？

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_8.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_10.png)

智能销售管道本质上会：
1.  研究潜在客户。
2.  根据个人、公司及所有能找到的信息对其进行评分。
3.  如果销售线索合格，则撰写一封合适的初始联系邮件。

这非常令人兴奋，因为你可以使用完全相同的模式，为公司自动化整个销售管道，然后针对你的具体用例训练和微调智能体。

现在，让我们动手构建它。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_12.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_3.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_4.png)

## 代码实现：构建智能体组

首先，导入我们需要的类。这包括常规的 `Agent`、`Task` 和 `Crew` 类。

```python
from crewai import Agent, Task, Crew
```

接下来，设置我们将要使用的 OpenAI 模型。我们将使用 `gpt-4o-mini`。请注意，这已经是 crewAI 的默认设置，但为了明确起见，我们在这里进行设置。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_14.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_15.png)

```python
from langchain_openai import ChatOpenAI
llm = ChatOpenAI(model="gpt-4o-mini")
```

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_16.png)

然后，我们需要加载所有智能体和任务的 YAML 配置文件，方式与过去类似。不同之处在于，我们现在使用两个智能体组，因此需要加载两组 YAML 文件：一组用于专注于客户评分和研究的“潜在客户智能体”，另一组用于专注于撰写邮件的“邮件智能体”。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_6.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_18.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_19.png)

加载方式很直接，我们将它们分别存入四个变量中以便后续使用。

```python
import yaml

with open('lead_agents.yaml') as file:
    lead_agents_config = yaml.safe_load(file)

with open('lead_tasks.yaml') as file:
    lead_tasks_config = yaml.safe_load(file)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_21.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_22.png)

with open('email_agents.yaml') as file:
    email_agents_config = yaml.safe_load(file)

with open('email_tasks.yaml') as file:
    email_tasks_config = yaml.safe_load(file)
```

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_24.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_25.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_8.png)

## 创建结构化数据模型

现在，创建我们将要使用的 Pydantic 模型，用于将客户信息和数据构造成可用的形式。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_10.png)

我们将使用四个不同的模型：
1.  `LeadPersonalInfo`：保存客户个人信息。
2.  `CompanyInfo`：保存公司信息，如公司名称、行业、收入等。
3.  `LeadScore`：存储智能体将给出的具体评分信息。
4.  `LeadScoringResult`：将上述所有信息整合到一个模型对象中。

```python
from pydantic import BaseModel
from typing import Optional

class LeadPersonalInfo(BaseModel):
    name: str
    job_title: str
    role_relevance: str
    professional_background: Optional[str] = None

class CompanyInfo(BaseModel):
    company_name: str
    industry: str
    revenue: Optional[str] = None
    market_presence: Optional[str] = None

class LeadScore(BaseModel):
    score: int
    scoring_criteria: str
    validation_notes: Optional[str] = None

class LeadScoringResult(BaseModel):
    personal_info: LeadPersonalInfo
    company_info: CompanyInfo
    lead_score: LeadScore
```

## 导入工具

创建好模型后，导入智能体组将使用的工具。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_27.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_12.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_29.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_30.png)

对于这些智能体组，我们将只使用两个工具：`SerperDevTool` 和 `ScrapeWebsiteTool`。这些工具允许我们的智能体使用谷歌搜索互联网，并抓取遇到的任何 URL，以便了解更多关于特定版本或网站的信息。

```python
from crewai_tools import SerperDevTool, ScrapeWebsiteTool

search_tool = SerperDevTool()
scrape_tool = ScrapeWebsiteTool()
```

## 构建第一个智能体组：潜在客户评分

现在，让我们实际加载 YAML 配置来创建我们的第一个智能体组：潜在客户资格评定智能体组。

这个智能体组非常明确。我们将有三个智能体和三个任务：`LeadDataAgent`、`CulturalFitAgent`、`ScoringValidationAgent`。每个都专注于获取客户的特定信息。
*   `LeadDataAgent`：尝试丰富客户信息，评估其在公司的角色，判断其是否是我们产品的潜在优质买家。
*   `CulturalFitAgent`：确保该公司符合我们的理想客户画像。
*   `ScoringValidationAgent`：整合所有信息，给出最终评分，并说明评分依据。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_32.png)

我们稍后可以在 YAML 中查看这些任务，但现在，我们将它们全部整合到这个单一的智能体组中。

```python
# 从YAML配置创建智能体
lead_data_agent = Agent(**lead_agents_config['lead_data_agent'], llm=llm, tools=[search_tool, scrape_tool])
cultural_fit_agent = Agent(**lead_agents_config['cultural_fit_agent'], llm=llm, tools=[search_tool, scrape_tool])
scoring_agent = Agent(**lead_agents_config['scoring_validation_agent'], llm=llm)

# 从YAML配置创建任务
analyze_task = Task(**lead_tasks_config['analyze_lead_data'])
research_task = Task(**lead_tasks_config['research_cultural_fit'])
score_task = Task(**lead_tasks_config['final_lead_scoring'])

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_34.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_35.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_36.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_37.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_38.png)

# 创建智能体组
lead_qualification_crew = Crew(
    agents=[lead_data_agent, cultural_fit_agent, scoring_agent],
    tasks=[analyze_task, research_task, score_task],
    verbose=True
)
```

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_40.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_14.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_15.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_16.png)

现在，让我们快速看一下智能体的定义。你可以看到它们的角色、目标和背景故事。这些智能体的定义非常直接，其核心目标是找到关于该客户的信息，以便就是否为优质客户做出明智决策。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_42.png)

如果我们查看任务，可以看到我们试图为每个客户提取的信息类型。包括姓名、职位、角色、专业背景等个人信息，以及行业、公司规模、收入、市场存在感等公司信息。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_44.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_45.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_18.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_19.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_46.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_47.png)

你可以看到，我们在任务定义中预设了要销售的公司和产品（本例中是 crewAI，一个多智能体编排平台）。我们告诉智能体我们的理想客户画像以及如何推销这个平台，然后将客户数据插入到任务定义中。当然，我们也可以将公司和产品信息作为变量插入，但为了示例简单，我们直接将其定义在任务中。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_49.png)

其他任务则继续寻找关于文化、价值观、战略契合度以及客户质量的定性评分等信息。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_51.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_52.png)

了解了智能体和任务后，让我们回到代码。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_21.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_22.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_54.png)

## 构建第二个智能体组：邮件撰写

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_56.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_57.png)

现在创建我们的第二个智能体组：邮件参与度智能体组。该智能体组将在客户被评分和分类后，为其撰写邮件。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_59.png)

这个智能体组非常直接。我们有两个智能体：`EmailContentSpecialist` 和 `EngagementStrategist`。它们将执行起草初始邮件和优化邮件以确保其吸引力的步骤。该智能体组只关注我们获得的高评分客户，以确保能与他们有效互动。

```python
# 从YAML配置创建邮件智能体
email_content_agent = Agent(**email_agents_config['email_content_specialist'], llm=llm)
engagement_agent = Agent(**email_agents_config['engagement_strategist'], llm=llm)

# 从YAML配置创建邮件任务
draft_task = Task(**email_tasks_config['draft_initial_email'])
optimize_task = Task(**email_tasks_config['optimize_for_engagement'])

# 创建邮件智能体组
email_engagement_crew = Crew(
    agents=[email_content_agent, engagement_agent],
    tasks=[draft_task, optimize_task],
    verbose=True
)
```

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_61.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_62.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_64.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_24.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_25.png)

## 创建 Flow

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_66.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_67.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_68.png)

现在两个智能体组都已创建，我们准备开始创建 Flow。Flow 是 crewAI 的一个全新功能，它允许你在智能体组执行之前、期间或之后执行常规 Python 代码。

这让你拥有更多控制权，能够构建更完整、更复杂的自动化流程，既利用了智能体带来的自主性，又保留了在需要时执行常规 Python 代码的能力。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_70.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_72.png)

首先，导入我们将要使用的类和函数。你可以看到我导入了 `Flow` 类以及两个函数装饰器：`listen` 和 `start`。这将是一个非常直接的 Flow，但能让我们理解其强大之处。

创建 Flow 的基本方法是创建一个继承自 crewAI `Flow` 类的类。你可以使用 `@start` 装饰器来定义将要执行的初始函数。

在本例中，`fetch_leads` 函数将是我们的初始函数，因为我们用 `@start` 装饰了它。这个函数可以从数据库获取客户，或执行其他操作。本例中，我使用一些关于我自己和我之前在一家名为 Clibit 公司的职位的模拟数据。

获取客户后，我们需要对这些客户进行评分。为此，我们可以创建一个名为 `score_leads` 的新函数。你可以看到我们在这个函数上使用了不同的装饰器 `@listen`，并引用了 `fetch_leads` 函数。这意味着 `score_leads` 函数将在 `fetch_leads` 完成后执行。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_74.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_76.png)

`fetch_leads` 函数返回的内容（本例中是一个客户数组）将被传递给 `score_leads` 函数，我们可以用它来为每个客户启动评分智能体组。

需要指出的另一点是，每个 Flow 都有一个状态，可以通过 `self.state` 访问。在本例中，我们将评分存入 `self.state`，以便稍后对收集的信息、成本等进行分析。

评分完成后，我们想做两件事：将结果存储到数据库，并筛选出评分非常高（例如高于70分）的客户。为此，我们创建两个新函数：`store_leads_score` 和 `filter_leads`。这两个函数都监听 `score_leads`。因此，一旦 `score_leads` 执行完成，这两个函数将**并行执行**。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_78.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_79.png)

`store_leads` 函数可以将信息存回数据库。`filter_leads` 函数则过滤掉评分低于70的客户。现在，`filter_leads` 函数的返回值将只包含我们获得的高评分客户。

对于这些高评分客户，我们可以添加一个新函数，使用第二个智能体组来撰写邮件。我们添加一个名为 `write_email` 的新函数，它监听 `filter_leads`。因此，一旦 `filter_leads` 完成，`write_email` 将只获得评分高于70的客户，然后为每一个调用邮件撰写智能体组。这将返回一个邮件列表，这些邮件就可以准备发送了。

如果我们想立即发送，可以添加一个监听 `write_email` 的新函数 `send_email` 来发送所有邮件。这是 Flow 的最终函数，它的返回值将是 Flow 本身的最终返回值。为了简单起见，我们只返回邮件。

这就是一个 Flow，设置起来非常直接。要执行它，你可以先创建一个 Flow 实例。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_81.png)

```python
from crewai.flow import Flow, start, listen

class SalesPipelineFlow(Flow):
    @start
    def fetch_leads(self):
        # 模拟从数据库获取数据
        leads = [
            {"name": "Andrew Ng", "company": "Clibit", "title": "CTO"},
            # ... 更多模拟数据
        ]
        return leads

    @listen(fetch_leads)
    def score_leads(self, leads):
        scored_leads = []
        for lead in leads:
            # 调用潜在客户评分智能体组
            result = lead_qualification_crew.kickoff(inputs=lead)
            # 将结果转换为 Pydantic 模型并存储到状态
            scoring_result = LeadScoringResult(**result)
            self.state.scored_leads.append(scoring_result)
            scored_leads.append(scoring_result)
        return scored_leads

    @listen(score_leads)
    def store_leads_score(self, scored_leads):
        # 模拟存储到数据库
        print(f"Storing {len(scored_leads)} leads to database.")
        # 实际数据库操作代码...
        return True

    @listen(score_leads)
    def filter_leads(self, scored_leads):
        high_score_leads = [lead for lead in scored_leads if lead.lead_score.score > 70]
        return high_score_leads

    @listen(filter_leads)
    def write_email(self, high_score_leads):
        emails = []
        for lead in high_score_leads:
            # 调用邮件撰写智能体组
            email_result = email_engagement_crew.kickoff(inputs=lead.dict())
            emails.append(email_result)
        return emails

    # 可选：发送邮件函数
    # @listen(write_email)
    # def send_email(self, emails):
    #     for email in emails:
    #         # 发送邮件代码...
    #         pass
    #     return "Emails sent."

# 创建 Flow 实例
flow = SalesPipelineFlow()
```

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_83.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_27.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_29.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_30.png)

## 可视化 Flow

在执行 Flow 之前，我们可以将其绘制出来，以便查看其结构。绘制 Flow 非常简单，只需在 Flow 实例上调用 `plot` 函数，它会自动生成一个包含整个 Flow 的 HTML 文件。

```python
flow.plot('sales_pipeline_flow.html')
```

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_85.png)

现在，我们加载这个 HTML 文件来看看它的样子。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_32.png)

在这里，你可以看到整个 Flow。它从 `fetch_leads` 函数开始，然后调用 `score_leads`。在 `score_leads` 之后，它并行执行两件事：将评分存储到数据库，以及筛选评分高于70的客户。之后，它撰写邮件并发送。图中还有一个图例，教你如何阅读整个图表，理解 Flow 中发生的一切。这对于导出流程图、附加到 GitHub 仓库或向他人解释流程执行情况非常有用。

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_34.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_35.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_36.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_37.png)
![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_38.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_87.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_88.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_89.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_90.png)

## 运行 Flow 并查看指标

现在，让我们看看实际运行这个 Flow，并检查其后的一些指标。

启动 Flow 与启动智能体组非常相似。只需在 Flow 变量上调用 `kickoff` 函数。我们使用 `await` 来确保等待 Flow 执行完成，因为许多函数可以异步执行。我们将最终结果赋值给 `emails` 变量，以便检查一些运行数据。

```python
emails = await flow.kickoff()
```

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_92.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_93.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_94.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_95.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_96.png)

![](img/d4e05605ddc4eb0f5b18ffc1cb17a648_