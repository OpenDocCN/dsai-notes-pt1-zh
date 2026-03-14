# 7：日语句子构造器 - 技术规范 📋

![](img/cd2fdb3bd0e57fc1885afd1551069000_0.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_2.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_3.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_4.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_5.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_6.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_7.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_8.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_9.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_10.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_11.png)

在本节课中，我们将学习“日语句子构造器”项目的技术规范。这是一个面向初学者的项目，我们将探讨其目标、技术不确定性以及如何为后续的实际实施做好准备。

## 项目概述 🎯

![](img/cd2fdb3bd0e57fc1885afd1551069000_13.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_14.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_15.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_16.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_17.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_18.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_19.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_20.png)

我们正在查看JennyAI训练营Wiki左侧“项目”下的“日语句子构造器”项目。这是一个**100级**的初学者项目。这意味着它不涉及编程，我们将**100%使用AI辅助工具**，通过自然语言指令来让智能体完成我们想要的任务。

![](img/cd2fdb3bd0e57fc1885afd1551069000_21.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_22.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_23.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_24.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_26.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_27.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_28.png)

该项目的目标是构建一个聊天智能体，它扮演教学助手的角色，指导学生将目标英语句子翻译成日语。这个教学助手**不直接提供答案**，而是只提供指导，以帮助学生自己完成翻译。

![](img/cd2fdb3bd0e57fc1885afd1551069000_30.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_31.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_32.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_33.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_34.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_35.png)

## 技术不确定性探讨 🤔

在开展任何项目时，我们都应考虑其技术不确定性。在项目结束时，我们应该能够解释这些不确定性。这是一个研发过程，你在开发过程中进行研究，并记录下答案。你的记录会与他人不同，因为你可能使用不同的技术、硬件或方法。

![](img/cd2fdb3bd0e57fc1885afd1551069000_37.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_38.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_39.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_40.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_41.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_42.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_43.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_44.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_45.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_46.png)

以下是几个需要你思考和扩展的技术不确定性问题：

1.  **AI辅助工具执行广泛任务的能力**：AI辅助工具几乎可以处理所有事情。但问题是，它在什么情况下会失效？它的能力边界在哪里？
2.  **广泛任务是否更适合由多个专业智能体协作完成**：我们知道，对于大型语言模型来说，任务越庞大，执行起来可能越困难。因此，我们可能会发现某些部分需要拆分成更小的任务，或者将一个智能体拆分成三四个专门的智能体。
3.  **使用AI辅助工具是否适合快速原型化智能体**：这个问题探讨的是利用现有AI平台快速构建和测试智能体概念的效率。
4.  **如何将AI系统中构建的智能体重新实现在允许直接集成的技术栈中**：请注意，AI系统不仅仅是单一的大型语言模型，其底层包含许多编排逻辑。如果你构建了一个智能体，当组织希望进行直接集成时，能否将其迁移到其他平台？
5.  **从一个AI辅助工具迁移到另一个时，需要多大程度上重写提示文档**：例如，如果我们最初使用Anthropic Claude，但后来公司觉得太贵，想换到OpenAI，我们该如何迁移我们的工作？
6.  **在AI辅助工具的局限内，我们能自然地发现哪些提示技巧**：提示工程有很多技巧，但有些技巧只有在你能将LLM与其他工具编排时才能使用。而我们目前仅限于在文本框中输入。通过自然使用，你可能会发现一些对你有用的元方法。
7.  **对于我们的业务目标，特定的AI辅助工具有哪些独特的创新功能**：不同的AI助手都在构建自己的“护城河”。例如，OpenAI的语音功能非常出色，Anthropic Claude的项目功能便于上传知识库。评估这些功能是否能被我们的解决方案所利用。
8.  **基于AI系统的选择和我们的硬件或预算限制，我们能够实现什么**：观看本课程的每个人使用的工具可能不同。你可能只能使用免费服务、开源模型，或者拥有强大的本地机器。**关键不在于你是否取得了完美的结果，而在于你带回了什么信息**。即使你只能使用性能有限的免费模型，你得到的反馈仍然非常有价值。

![](img/cd2fdb3bd0e57fc1885afd1551069000_48.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_49.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_50.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_51.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_52.png)

## 可用工具说明 🛠️

![](img/cd2fdb3bd0e57fc1885afd1551069000_54.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_55.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_56.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_57.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_58.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_59.png)

以下是你可以考虑使用的AI辅助工具示例（必须是在线的、无需编程配置的AI助手平台）：

![](img/cd2fdb3bd0e57fc1885afd1551069000_60.png)

*   OpenAI ChatGPT
*   Anthropic Claude
*   Google Gemini
*   Microsoft Copilot
*   Perplexity
*   Grok

![](img/cd2fdb3bd0e57fc1885afd1551069000_62.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_63.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_64.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_65.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_66.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_67.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_69.png)

此外，还有一些技术上的替代方案（标注*号），它们并非严格意义上的“AI辅助工具”，但如果你因各种原因选择使用它们，也是可以接受的：

*   **Ollama + Web UI***：这为你提供了可下载的单一模型的Web界面。
*   **Libra Chat***：这是一个在线平台，允许你接入多种不同的模型，可以用来同时评估多个模型。
*   **Leonardo AI***：这是一个承诺提供开源个人助理的平台，可以接入许多功能。

![](img/cd2fdb3bd0e57fc1885afd1551069000_71.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_72.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_73.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_74.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_75.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_76.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_77.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_78.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_79.png)

**AI辅助工具**指的是在线平台，你无需调整`top_p`、`top_k`等参数，也无需进行任何编程操作，只需使用它们提供的文本框和任何可视化功能。

![](img/cd2fdb3bd0e57fc1885afd1551069000_81.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_82.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_83.png)

## 项目文档与后续步骤 📄

通常，我会提供一个架构图和更详细的构建信息。但本项目非常简单，无需架构图。此外，直接在这里编写提示文档并不理想，因为我希望带领大家以探索的方式，像第一次构建一样来完成它。

![](img/cd2fdb3bd0e57fc1885afd1551069000_85.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_86.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_87.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_88.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_89.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_90.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_91.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_92.png)

当然，我之前已经花了好几个月构建过这个项目，并发现了一些非常有趣的事情。但请理解，本文档相对精简。我们将在构建过程中逐步探讨具体要实现什么，并且考虑到你可能不懂日语，我也会在构建时解释相关要点。

![](img/cd2fdb3bd0e57fc1885afd1551069000_94.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_95.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_96.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_97.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_98.png)

希望这是一个很好的介绍。在下一个视频中，我们将开始具体的实施。

![](img/cd2fdb3bd0e57fc1885afd1551069000_100.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_101.png)

## 总结 📝

![](img/cd2fdb3bd0e57fc1885afd1551069000_103.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_104.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_105.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_106.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_107.png)

![](img/cd2fdb3bd0e57fc1885afd1551069000_108.png)

本节课我们一起学习了“日语句子构造器”项目的技术规范。我们明确了这是一个为日语学习者构建指导性教学助手的初级项目，完全依赖自然语言与AI交互。我们重点探讨了项目实施中可能遇到的**技术不确定性**，并列出了一系列需要你在实践中探索和回答的问题。最后，我们概述了可用的工具类型，并预告了接下来的实践环节。记住，本项目的核心价值在于探索和记录过程，为后续更复杂的智能体开发积累经验。