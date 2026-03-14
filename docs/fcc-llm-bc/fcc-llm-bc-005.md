# 5：项目实战演练 - AskFSDL Discord Bot

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_1.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_3.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_5.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_7.png)

在本节课中，我们将一起深入分析“Ask FSDL” Discord问答机器人的完整代码库。这个项目是上午演示的问答系统的成熟版本，它基于向量存储检索技术，在一个信息语料库上进行问答。我们将从项目结构、数据处理、部署工具到性能优化等多个方面进行拆解。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_9.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_10.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_11.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_12.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_13.png)

## 项目概览与工程化实践

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_15.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_17.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_19.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_20.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_22.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_24.png)

上一节我们介绍了项目的背景。本节中，我们来看看如何将一个实验性代码转化为一个可维护的工程项目。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_26.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_27.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_28.png)

首先，一个专业的Python项目通常包含良好的工程管理工具。在这个项目中，我们使用了一个`Makefile`来统一管理各种任务。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_30.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_31.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_32.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_33.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_35.png)

以下是`Makefile`中包含的主要命令：
*   `setup`: 设置项目环境并进行工具认证。
*   `deploy-backend`: 部署后端服务。
*   `setup-vector-index`: 设置向量索引。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_37.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_39.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_40.png)

除了`Makefile`，项目中还集成了一系列代码质量工具，以确保团队协作时代码的整洁和一致性。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_42.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_43.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_44.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_45.png)

以下是项目中使用的核心代码质量工具：
*   **pre-commit**: 在代码提交到GitHub前自动运行检查，防止常见错误（如多余空格、语法错误、误提交大文件等）。
*   **Black**: Python代码自动格式化工具，确保所有Python文件的格式统一。
*   **Ruff**: 一个新兴的、基于Rust的Python代码linter和格式化工具，用于捕捉代码风格问题，速度很快。
*   **shellcheck**: 如果你需要编写很多Bash脚本，这个工具可以帮你检查脚本中的常见错误。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_47.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_49.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_50.png)

## 数据处理：提升结果质量的关键

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_52.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_53.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_54.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_56.png)

上一节我们介绍了基础的工程化设置。本节中我们来看看如何通过精心处理数据来显著提升问答系统的效果。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_58.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_60.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_62.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_64.png)

我们意识到，最初简单的数据抓取和分块方法（类似于上午演示的版本）效果并不理想。最大的质量提升来自于对数据的深入理解和处理。项目中的Jupyter笔记本详细记录了数据改进步骤，并设置了文档数据库。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_66.png)

仅仅将文本当作纯文本来解析会丢失大量有价值的结构信息。例如，从Full Stack Deep Learning网站抓取的Markdown笔记包含章节标题、段落等结构，这些结构通常与页面中的特定部分相关联。我们的目标是提供能够帮助用户快速定位信息的来源，因此在导入文档时保留这些结构至关重要。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_68.png)

这需要对数据源有一定的了解。虽然处理互联网规模的数据更具挑战性，但如果是针对公司内部知识库构建问答系统，则可以有更多上下文来进行优化。

数据分割的目标是获得一种基础的文档格式，既能保留链接所需的信息，又能尽可能保留原始结构。这与**LangChain**等工具中常见的“分块”概念不同：后者是为了将内容切成适合语言模型上下文窗口的小块以便放入向量存储；而前者是在存储文档时解析并保留其结构。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_70.png)

除了PDF和网页，我们还探索了从其他媒介提取文本信息的方法。例如，我们的线上视频自带YouTube自动生成的字幕，这些字幕可以作为问答机器人的知识来源。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_71.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_72.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_74.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_76.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_78.png)

寻找创造性的方法从各种知识载体中提取数据，并通过嵌入或其他方式使其能够被LLM访问，是构建更有用语言模型应用的一个重要方向。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_80.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_81.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_82.png)

即使能方便地获取数据（例如通过`youtube-transcript-api`库获取字幕），原始数据也可能不符合特定需求。例如，YouTube字幕带有精确到秒的时间标签，这虽然便于链接到视频的特定时刻，但作为检索单元过于细碎。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_84.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_86.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_88.png)

一个简单的解决方案是将其重新分块，合并成包含几百到几千个token的、信息量更实用的片段。这类优化需要结合具体数据源和待解决的问题来思考。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_89.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_91.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_92.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_94.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_96.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_97.png)

在数据上投入前期工作，编写看似“枯燥”的代码来妥善管理数据，往往能带来巨大的回报。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_99.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_100.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_102.png)

## 部署与基础设施：使用Modal

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_104.png)

上一节我们探讨了数据处理。本节中，我们来了解如何使用Modal来部署和管理这个应用。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_106.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_107.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_108.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_110.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_112.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_113.png)

Modal的核心优势之一是能够为不同的任务创建独立的容器镜像。你可以基于某个版本定制镜像，通过`pip install`安装依赖，或者执行安装Linux包、运行Shell命令等操作。Modal的容器构建和启动速度非常快，这大大减少了云原生开发中常见的摩擦，保持了快速的开发周期。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_115.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_116.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_117.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_118.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_120.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_122.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_123.png)

Modal提供了一个便捷的交互式调试功能。你可以编写一个函数来启动一个运行在Modal云基础设施上的IPython内核，其体验几乎和本地调试一样方便，但包含了云端的完整运行环境。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_125.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_126.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_127.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_129.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_130.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_132.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_134.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_135.png)

除了数据处理，Modal也旨在支持应用部署。它内置了**异步服务器网关接口**抽象，可以轻松地将你的代码包装成Web应用。所有计算资源都是**无服务器**的，按需分配和释放，这对于用户访问量波动大的应用（如演示期间流量高，其他时间流量低）非常经济高效。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_137.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_138.png)

## 用户界面与集成：Gradio与Discord Bot

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_140.png)

上一节我们介绍了后端部署。本节中，我们来看看如何为应用添加用户界面和集成。

**Gradio**是一个强大的工具，允许你**用纯Python描述用户界面**，并快速获得一个简单的单页或多页应用。它由Hugging Face支持，能快速集成机器学习领域所需的各种功能。Gradio界面非常灵活，可以嵌入到Jupyter笔记本或作为iframe使用，并且始终提供OpenAPI规范，便于与其他工具（如ChatGPT插件）集成。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_142.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_143.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_144.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_145.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_146.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_148.png)

对于Discord机器人的集成，我们使用了`discord.py`库。设置过程包括定义机器人事件和斜杠命令。当用户在Discord中输入`/ask`命令时，机器人会异步调用部署在Modal上的后端API（基于**FastAPI**构建）来获取答案并返回。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_150.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_152.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_154.png)

## 核心组件与未来优化

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_155.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_156.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_157.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_159.png)

上一节我们介绍了前端集成。本节中，我们来剖析核心的问答逻辑并探讨优化方向。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_161.png)

我们使用**OpenAI的`text-embedding-ada-002`模型**来生成文本的向量嵌入，该模型成本低廉且效果不错。检索目前主要依赖于向量索引，但结合元数据过滤可能会更有效。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_163.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_164.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_166.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_167.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_169.png)

问答链的核心是一个**提示词模板**，它是一个f-string，将检索到的来源（URL和内容）插入到预设的提示词中，然后发送给语言模型生成答案。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_171.png)

如果要进一步提升这个机器人，主要面临三大挑战：
1.  **改进检索效果**：可以借鉴传统信息检索领域的成熟思路。
2.  **提升模型输出质量**：需要更精细的提示工程和模型选择。
3.  **建立稳定的用户群**：这是产品化和市场层面的挑战。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_173.png)

为了监控和优化应用，我们集成了**Sentry**来记录所有问答事件。这允许我们追踪输入输出，并在后期通过“投影”功能对已记录的数据进行丰富和分析，例如检测问题是否具有毒性、分析文本熵值等。更进一步，我们可以利用**语言模型来评估语言模型**的输出质量，自动识别回答不合理的情况，并结合Sentry的观测数据持续改进系统。

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_175.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_177.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_178.png)

---

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_180.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_182.png)

![](img/e7e22aaa8cc6d46d8673de82a33dc5b1_184.png)

本节课中我们一起学习了“Ask FSDL”问答机器人项目的完整架构。我们从项目工程化、数据处理的重要性讲起，深入了解了如何使用Modal进行高效的云部署和调试，探讨了利用Gradio快速构建界面以及集成Discord机器人的方法，最后分析了核心的检索-生成流程以及通过Sentry实现可观测性和持续优化的路径。这个项目展示了构建一个实用LLM应用所需的全栈考量和工具链。