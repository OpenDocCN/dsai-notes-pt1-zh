# 012：自动化活动策划 🎯

在本节课中，我们将学习如何使用 crewAI 框架构建一个能够自动化策划活动的多智能体系统。我们将创建三个各司其职的智能体，并利用 Pydantic 模型将模糊的 AI 输出转换为结构化的数据。

## 概述

我们将构建一个能够为“科技创新大会”寻找场地、协调后勤并制定营销策略的智能体团队。通过本教程，你将学会如何设置环境、创建智能体和任务，并利用 `output_json` 等高级特性将 AI 输出集成到常规代码中。

---

## 环境与工具设置

首先，我们需要导入必要的类并设置环境变量。

![](img/9838fae71b260e6a118ce9b1f1bfa653_1.png)

```python
import warnings
warnings.filterwarnings('ignore')

from crewai import Agent, Task, Crew
```

接下来，设置环境变量。本示例将使用 Serper 工具进行网络搜索，因此你需要提供 Serper API 密钥。

```python
import os
os.environ["OPENAI_API_KEY"] = "你的 OpenAI API 密钥"
os.environ["SERPER_API_KEY"] = "你的 Serper API 密钥"
os.environ["OPENAI_MODEL_NAME"] = "gpt-4"  # 或你选择的模型
```

![](img/9838fae71b260e6a118ce9b1f1bfa653_3.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_4.png)

现在，导入我们将要使用的两个工具：`ScrapeWebsiteTool` 和 `SerperDevTool`。

```python
from crewai_tools import ScrapeWebsiteTool, SerperDevTool

search_tool = SerperDevTool()
scrape_tool = ScrapeWebsiteTool()
```

这两个工具将协同工作：`SerperDevTool` 用于在互联网上搜索信息，`ScrapeWebsiteTool` 则允许智能体访问搜索到的网站并获取其内容以供分析。合理规划工具的使用方式，能极大地影响智能体的行为。

---

## 创建智能体 🧠

上一节我们完成了环境设置，本节中我们来看看如何创建负责不同职责的智能体。

以下是我们将要创建的三个智能体：

1.  **场地协调员**：负责根据活动要求和条件，识别并预订合适的场地。
2.  **后勤经理**：负责思考活动的后勤细节，确保场地适合我们的会议或聚会。
3.  **营销专员**：在场地和后勤信息的基础上，构思如何营销此次活动，以吸引尽可能多的参与者。

让我们依次创建它们。

```python
# 1. 场地协调员智能体
venue_coordinator = Agent(
    role='场地协调员',
    goal='根据活动条件和要求，识别并预订合适的场地。',
    backstory='你是一位经验丰富的活动策划者，擅长在城市中寻找完美的活动场所。',
    tools=[search_tool, scrape_tool],
    verbose=True
)

# 2. 后勤经理智能体
logistics_manager = Agent(
    role='后勤经理',
    goal='确保活动的后勤安排周全，包括餐饮和设备，并确认场地适用性。',
    backstory='你是一位注重细节的后勤专家，擅长将活动愿景转化为可执行的计划。',
    tools=[search_tool, scrape_tool],
    verbose=True
)

# 3. 营销专员智能体
marketing_agent = Agent(
    role='营销专员',
    goal='基于活动场地和主题，制定有创意的营销方案以最大化参与度。',
    backstory='你是一位富有创造力的营销策略师，擅长为新活动制造话题和吸引力。',
    tools=[search_tool, scrape_tool],
    verbose=True
)
```

现在，我们拥有了一个不仅能寻找活动信息，还能统筹后勤和营销策略的系统。这在传统的非AI应用程序中是很难实现的。

![](img/9838fae71b260e6a118ce9b1f1bfa653_6.png)

---

## 使用 Pydantic 定义结构化输出 📦

在开始创建任务之前，我们先花点时间了解 Pydantic。这是一个非常流行的库，它允许你以简单的方式创建模型并处理强类型变量。

我们创建一个 Pydantic 模型来保存场地详情。这样做的好处是，可以将智能体模糊的输出转换为强类型的、可在常规代码中使用的对象。

```python
from pydantic import BaseModel

class VenueDetails(BaseModel):
    name: str
    address: str
    capacity: int
    booking_status: str
```

与经典 Python 类需要定义 `__init__` 方法不同，Pydantic 提供了一种语法糖，让你能更简洁地定义模型及其属性。Pydantic 的功能远不止于此，但本课中我们主要利用它将智能体输出转化为结构化数据。

![](img/9838fae71b260e6a118ce9b1f1bfa653_8.png)

---

## 创建任务 📝

上一节我们介绍了如何使用 Pydantic 模型，本节我们来创建智能体需要执行的具体任务。

以下是三个核心任务：

![](img/9838fae71b260e6a118ce9b1f1bfa653_10.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_11.png)

1.  **寻找场地任务**：此任务负责为活动寻找场地。我们将使用 `output_json` 属性，将智能体的输出通过 `VenueDetails` 模型转换为 JSON 格式并保存到文件。
    ```python
    find_venue_task = Task(
        description=(
            "为在{city}举办的关于{topic}的活动寻找并预订一个合适的场地。"
            "该场地需要能容纳{participants}人，并符合{budget}的预算。"
            "优先考虑会议厅。"
        ),
        expected_output="一个包含场地名称、地址、容量和预订状态的 JSON 对象。",
        agent=venue_coordinator,
        output_json=VenueDetails,  # 使用 Pydantic 模型定义输出格式
        output_file='venue_details.json'  # 输出到 JSON 文件
    )
    ```
    这个特性非常强大，尤其当你希望将智能体系统集成到现有代码中时。它允许你获得可以直接在函数或其他逻辑中使用的结构化输出，而无需调用完整的 API。

2.  **协调后勤任务**：此任务负责协调活动的餐饮和设备。我们使用了 `human_input=True` 属性，这意味着任务完成前，智能体会暂停并询问操作员（我们）是否满意结果或需要进行修改。同时，我们设置 `async_execution=True`，允许此任务与后续任务并行执行。
    ```python
    logistics_task = Task(
        description=(
            "为在{city}举办的{topic}活动协调后勤，预计{participants}人参与，暂定日期为{date}。"
            "基于选定的场地，规划餐饮服务、视听设备、座椅布置和网络需求。"
        ),
        expected_output="一份详细的后勤协调计划，包括供应商推荐和时间安排。",
        agent=logistics_manager,
        human_input=True,  # 执行中请求人工输入
        async_execution=True  # 允许异步执行
    )
    ```
    虽然后勤任务依赖于场地任务的结果（因为地点会影响餐饮和设备协调），但它与营销任务无关，因此可以并行执行。

3.  **制定营销任务**：此任务负责创建营销活动以推广本次事件。同样，我们设置其可以异步执行。我们要求输出为 Markdown 格式，并保存到文件中。
    ```python
    marketing_task = Task(
        description=(
            "为在{city}举办的{topic}活动制定全面的营销活动方案，目标吸引{participants}名参与者。"
            "利用场地特色和活动主题，设计数字营销、内容策略和合作伙伴推广计划。"
        ),
        expected_output="一份格式清晰的 Markdown 文档，概述完整的营销活动策略。",
        agent=marketing_agent,
        async_execution=True,
        output_file='marketing_campaign.md'
    )
    ```

我们利用了 crewAI 任务类的多个新属性来展示它们在真实场景中的用途。你现在可能已经在思考可以利用这些属性支持哪些用例了。

![](img/9838fae71b260e6a118ce9b1f1bfa653_13.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_14.png)

---

## 组装团队并执行 🚀

现在我们已经有了智能体和任务，接下来创建 Crew 并启动执行过程。

![](img/9838fae71b260e6a118ce9b1f1bfa653_16.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_17.png)

创建 Crew 的过程很直接，与我们之前所做的一致：定义一组智能体、一组任务，并设置 `verbose` 模式。

![](img/9838fae71b260e6a118ce9b1f1bfa653_18.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_19.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_20.png)

```python
# 定义活动参数
event_inputs = {
    "topic": "科技与创新",
    "city": "旧金山",
    "date": "2024-06-15",
    "participants": 500,
    "budget": "$20,000"
}

![](img/9838fae71b260e6a118ce9b1f1bfa653_22.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_24.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_25.png)

# 创建 Crew
event_planning_crew = Crew(
    agents=[venue_coordinator, logistics_manager, marketing_agent],
    tasks=[find_venue_task, logistics_task, marketing_task],
    verbose=True
)

# 执行任务
results = event_planning_crew.kickoff(inputs=event_inputs)
```

执行开始后，场地协调员会首先启动，根据我们指定的城市“旧金山”开始寻找符合“科技创新大会”要求的场地。它会使用搜索工具查找信息，并可能使用爬取工具获取具体网站内容来分析选项。

当后勤任务需要人工输入时，程序会暂停等待你的反馈。之后，后勤任务和营销任务将并行执行，因为它们互不依赖。

![](img/9838fae71b260e6a118ce9b1f1bfa653_27.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_28.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_29.png)

---

![](img/9838fae71b260e6a118ce9b1f1bfa653_30.png)

## 查看输出结果 📄

![](img/9838fae71b260e6a118ce9b1f1bfa653_32.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_33.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_34.png)

执行完成后，我们可以查看生成的文件。

1.  **场地详情 JSON 文件** (`venue_details.json`)：这是由 `find_venue_task` 生成的强类型 JSON 输出，内容结构清晰，可以直接被其他程序读取或存入数据库。
    ```json
    {
      "name": "Main Hall at Yerba Buena Center",
      "address": "701 Mission St, San Francisco, CA",
      "capacity": 600,
      "booking_status": "Available for rental"
    }
    ```

2.  **营销活动 Markdown 文件** (`marketing_campaign.md`)：这是一份完整的营销活动计划，涵盖了数字营销、内容营销、合作伙伴关系、网络建设、现场活动策划、工具推荐甚至预算分配。
    ```markdown
    # 科技与创新大会营销活动计划

    ## 1. 数字营销
    - 社交媒体广告投放...
    - 邮件营销系列...

    ## 2. 内容营销
    - 发布行业领袖访谈...
    ...

    ## 3. 预算分配
    - 社交媒体广告: $5,000
    - 内容制作: $3,000
    ...
    ```

这个系统不仅能够找到合适的场地、规划后勤，还能构思出一套可执行的营销策略。这仅仅是多智能体系统能力的冰山一角，展示了如何将 AI 代理应用于工程领域之外的真实世界场景。

![](img/9838fae71b260e6a118ce9b1f1bfa653_36.png)

---

![](img/9838fae71b260e6a118ce9b1f1bfa653_37.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_38.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_39.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_40.png)

## 总结

本节课中，我们一起学习了如何使用 crewAI 构建一个自动化活动策划的多智能体系统。我们涵盖了以下核心内容：

*   **设置环境与工具**：导入必要的库，配置 API 密钥，并集成网络搜索与内容爬取工具。
*   **创建专业化智能体**：定义了场地协调员、后勤经理和营销专员三个角色，并为其分配合适的工具。
*   **利用 Pydantic 实现结构化输出**：通过定义 `VenueDetails` 模型，将 AI 的模糊输出转换为强类型的 JSON 数据，便于后续集成和使用。
*   **配置高级任务属性**：使用了 `output_json`、`human_input` 和 `async_execution` 等任务属性，增强了系统的交互性、集成能力和执行效率。
*   **组装与执行**：将智能体和任务组合成 `Crew`，并注入活动参数启动自动化流程。
*   **结果分析**：最终系统输出了结构化的场地信息 JSON 文件和详细的营销计划 Markdown 文档。

![](img/9838fae71b260e6a118ce9b1f1bfa653_42.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_43.png)

![](img/9838fae71b260e6a118ce9b1f1bfa653_44.png)

通过这个实例，你看到了如何将多个 AI 智能体组织起来，协同完成一个复杂的、多步骤的现实世界任务，并产生可直接利用的成果。这为你在其他领域应用多智能体系统提供了坚实的基础和广阔的想象空间。