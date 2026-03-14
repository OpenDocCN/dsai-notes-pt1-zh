# 013：LangGraph 规划智能体 🧠

![](img/0bf4459b1e63df6a638c19c6f9ca232a_1.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_3.png)

在本节课中，我们将学习如何使用 LangGraph 框架创建“规划与执行”风格的智能体。这种智能体设计模式通过将任务分解为规划和执行两个阶段，旨在实现更快的执行速度、更低的令牌成本以及比传统智能体（如 ReAct）更高的可靠性。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_5.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_6.png)

## 背景：从 ReAct 智能体说起

上一节我们提到了规划智能体的优势，为了更好地理解其进步，本节我们先来看看经典的 ReAct 智能体。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_8.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_10.png)

ReAct（推理与行动）是一种提示语言模型的方法，使其能够推理需要执行的操作，然后输出一个可被解析并用于在真实世界（如浏览器）中执行的动作。其工作流程如下：
1.  模型接收包含历史观察、可用工具和用户问题的提示。
2.  模型输出一个动作（例如，`search(NFL top scorer)`）。
3.  系统执行该动作，并将结果作为新的观察反馈给模型。
4.  重复此过程，直到模型认为已获得足够信息来生成最终答案。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_12.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_14.png)

**局限性**：
*   **串行执行**：每个工具调用都需要一次 LLM 调用，速度慢且成本高。
*   **短视决策**：一次只规划一步，可能缺乏整体战略。
*   **无法并行**：工具调用通常是顺序进行的。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_16.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_17.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_18.png)

## 规划与执行智能体模式 🗺️

为了解决 ReAct 的局限性，业界提出了“规划与执行”智能体模式。该模式将智能体分解为几个核心模块：

以下是其主要组成部分：
1.  **规划器**：接收用户输入和环境信息，生成一个需要执行的步骤计划。
2.  **执行器**：负责执行计划中的具体任务。
3.  **重新规划器（可选）**：根据执行结果，决定是否需要调整原计划。这个循环逻辑是智能体的典型特征。

本教程中的示例主要基于 **Plan-and-Solve** 论文，并参考了 **BAGI** 项目的思想。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_20.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_22.png)

### 基础实现：Plan-and-Solve 示例

让我们开始构建第一个规划智能体。首先，我们需要设置环境、工具并创建智能体。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_24.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_25.png)

**步骤 1：环境与工具配置**
我们需要配置 API 密钥（如 OpenAI）和工具（如 Tavily 搜索引擎），并设置 LangSmith 用于追踪和调试。

```python
# 示例：配置工具
from langchain_community.tools.tavily_search import TavilySearchResults
search_tool = TavilySearchResults()
```

![](img/0bf4459b1e63df6a638c19c6f9ca232a_27.png)

**步骤 2：创建执行代理**
规划器生成任务列表后，执行代理负责处理每个具体任务。

```python
# 示例：创建执行代理（简化概念）
execution_agent = create_execution_agent(tools=[search_tool])
```

![](img/0bf4459b1e63df6a638c19c6f9ca232a_29.png)

**步骤 3：定义图状态与节点**
在 LangGraph 中，我们通过定义状态图和节点来构建工作流。状态包含输入、计划、历史步骤和最终响应等信息。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_31.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_32.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_34.png)

```python
# 示例：定义状态结构（使用 Pydantic）
from typing import List
from pydantic import BaseModel

class PlanExecuteState(BaseModel):
    input: str
    plan: List[str]
    past_steps: List[dict]
    response: str = None
```

![](img/0bf4459b1e63df6a638c19c6f9ca232a_36.png)

**步骤 4：组装智能体图**
我们将规划器、执行器和重新规划器定义为图的节点，并通过条件边控制流程。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_38.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_40.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_42.png)

```python
# 示例：构建图（概念代码）
workflow = StateGraph(PlanExecuteState)
workflow.add_node(“planner”, planner_node)
workflow.add_node(“agent”, execution_agent_node)
workflow.add_node(“replan”, replanner_node)
workflow.add_conditional_edges(“replan”, should_end)
workflow.compile()
```

![](img/0bf4459b1e63df6a638c19c6f9ca232a_44.png)

通过调用这个图，并设置递归限制（防止无限循环），智能体就可以开始处理查询（例如，“谁是美网冠军？”）。在 LangSmith 中，我们可以清晰地追踪每个步骤的决策和执行过程。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_46.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_47.png)

## 进阶：支持变量替换的 ReWOO 智能体 🔄

![](img/0bf4459b1e63df6a638c19c6f9ca232a_49.png)

上一节的基础规划器将计划输出为字符串列表，执行时缺乏灵活性。ReWOO（无需观察的推理）论文对此进行了改进。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_51.png)

ReWOO 的核心创新是允许在计划中进行**变量替换**。这意味着初始计划可以引用早期步骤的结果，而无需再次咨询规划器，从而节省了大量时间和令牌。

**工作流程**：
1.  **规划器**：接收输入，生成一个包含变量占位符（如 `#E1`）的计划和推理步骤。
2.  **执行器**：顺序执行计划中的工具调用。当一个工具的结果产生后，系统会用该结果替换后续步骤中对应的变量。
3.  **求解器**：所有步骤执行完毕后，根据结果生成最终答案。

**优势**：
*   减少了与规划器 LLM 的交互次数。
*   通过变量替换实现了步骤间的信息传递。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_53.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_55.png)

**局限性**：
*   任务仍然是**顺序执行**的，无法并行。
*   需要等待整个计划生成完毕后才开始执行工具。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_56.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_57.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_59.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_61.png)

## 高效并行：LLM 编译器智能体 ⚡

为了进一步提升性能，LLM Compiler 论文旨在解决 ReWOO 的剩余瓶颈。

LLM Compiler 通过两种主要方式加速：
1.  **流式任务生成（DAG）**：将任务输出为有向无环图格式，每个任务明确声明其依赖项。这样，没有依赖关系的任务可以**并行执行**。
2.  **任务流式传输**：在 LLM 输出令牌流的过程中，一旦解析出一个完整的任务，就立即将其提交给任务调度单元执行，无需等待整个计划生成完毕。

**核心组件：任务调度单元**
这是 LLM Compiler 最有趣的部分。它接收一个任务字典流，每个字典包含要调用的工具及其依赖项列表。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_63.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_65.png)

```python
# 概念性任务格式
task = {
    “tool”: “search”,
    “args”: {“query”: “GDP of New York”},
    “dependencies”: [] # 此任务没有依赖，可立即执行
}
task_with_dep = {
    “tool”: “calculate”,
    “args”: {“expression”: “#1 + #2”}, # 依赖任务1和2的结果
    “dependencies”: [1, 2]
}
```

![](img/0bf4459b1e63df6a638c19c6f9ca232a_67.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_68.png)

![](img/0bf4459b1e63df6a638c19c6f9ca232a_70.png)

调度器使用多线程，一旦某个任务的依赖项全部满足，就立即执行它。同时，它支持变量替换（如 `#1` 引用第一个任务的结果），使得复杂的依赖关系得以实现。

**工作流程**：
1.  **规划/调度节点**：流式生成任务 DAG。
2.  **任务调度单元**：并行执行满足依赖条件的任务。
3.  **合并器**：所有任务完成后，决定是输出最终答案还是重新规划。
4.  **循环**：通过条件边在“重新规划”和“结束”之间选择。

通过处理“纽约的 GDP 是多少？”或“现存最老的鹦鹉年龄是多少？”等多步骤问题，可以看到 LLM Compiler 能够有效地并行搜索和计算，显著减少总体响应时间。

## 总结 🎯

本节课我们一起学习了三种基于 LangGraph 的规划智能体：

![](img/0bf4459b1e63df6a638c19c6f9ca232a_72.png)

1.  **基础 Plan-and-Solve 智能体**：引入了规划与执行分离的概念，通过重新规划实现循环，比 ReAct 更具结构性。
2.  **ReWOO 智能体**：在规划中引入变量替换，减少了与 LLM 的冗余交互，提升了效率。
3.  **LLM Compiler 智能体**：通过流式生成任务 DAG 和并行执行，实现了性能的极大优化，是迈向生产级高效智能体的重要一步。

![](img/0bf4459b1e63df6a638c19c6f9ca232a_74.png)

这些智能体设计模式都朝着更鲁棒、更有效的方向迈进。使用 LangGraph 实现它们非常直观。我们鼓励你尝试实现这些模式，甚至可以组合它们的思想（例如，将 ReWOO 风格的规划器与 LLM Compiler 风格的流式并行执行器结合），以构建适合自己需求的高性能智能体。