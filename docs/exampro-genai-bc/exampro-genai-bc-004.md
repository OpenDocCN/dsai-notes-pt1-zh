# 4：架构图绘制实战教程 🏗️

![](img/a019dd4f18120d1bba780fd4e8073049_1.png)

![](img/a019dd4f18120d1bba780fd4e8073049_2.png)

![](img/a019dd4f18120d1bba780fd4e8073049_3.png)

![](img/a019dd4f18120d1bba780fd4e8073049_4.png)

![](img/a019dd4f18120d1bba780fd4e8073049_5.png)

![](img/a019dd4f18120d1bba780fd4e8073049_6.png)

![](img/a019dd4f18120d1bba780fd4e8073049_7.png)

![](img/a019dd4f18120d1bba780fd4e8073049_8.png)

![](img/a019dd4f18120d1bba780fd4e8073049_9.png)

在本节课中，我们将学习如何绘制一个生成式AI应用的架构图。我们将基于一个“句子构造器”学习活动的简化案例，一步步构建其核心组件和数据流向的示意图。通过这个过程，你将掌握用图表清晰表达技术架构的能力。

![](img/a019dd4f18120d1bba780fd4e8073049_10.png)

![](img/a019dd4f18120d1bba780fd4e8073049_11.png)

![](img/a019dd4f18120d1bba780fd4e8073049_13.png)

![](img/a019dd4f18120d1bba780fd4e8073049_14.png)

![](img/a019dd4f18120d1bba780fd4e8073049_15.png)

## 概述与目标

![](img/a019dd4f18120d1bba780fd4e8073049_16.png)

![](img/a019dd4f18120d1bba780fd4e8073049_17.png)

![](img/a019dd4f18120d1bba780fd4e8073049_19.png)

![](img/a019dd4f18120d1bba780fd4e8073049_20.png)

![](img/a019dd4f18120d1bba780fd4e8073049_22.png)

我们的目标是完成第一周的作业，提交一份架构图。虽然专家们展示的架构非常详尽，但为了入门简单，我们将创建一个简化版本。你可以使用任何工具，如Lucid Charts、PowerPoint、Excel，甚至纸笔来完成。

![](img/a019dd4f18120d1bba780fd4e8073049_24.png)

![](img/a019dd4f18120d1bba780fd4e8073049_26.png)

![](img/a019dd4f18120d1bba780fd4e8073049_28.png)

![](img/a019dd4f18120d1bba780fd4e8073049_30.png)

上一节我们介绍了架构设计的重要性，本节中我们来看看如何动手绘制一个具体的简化架构图。

![](img/a019dd4f18120d1bba780fd4e8073049_32.png)

## 绘制核心数据流

![](img/a019dd4f18120d1bba780fd4e8073049_34.png)

![](img/a019dd4f18120d1bba780fd4e8073049_35.png)

![](img/a019dd4f18120d1bba780fd4e8073049_36.png)

![](img/a019dd4f18120d1bba780fd4e8073049_38.png)

![](img/a019dd4f18120d1bba780fd4e8073049_40.png)

![](img/a019dd4f18120d1bba780fd4e8073049_42.png)

我们将以“句子构造器”学习活动为例进行建模。首先，我们需要确立架构中的核心实体和基本流程。

![](img/a019dd4f18120d1bba780fd4e8073049_44.png)

![](img/a019dd4f18120d1bba780fd4e8073049_46.png)

![](img/a019dd4f18120d1bba780fd4e8073049_48.png)

![](img/a019dd4f18120d1bba780fd4e8073049_49.png)

以下是架构的核心组成部分：
1.  **用户**：与系统交互的终端使用者。
2.  **模型API**：提供生成能力的核心，我们将其简化为一个**大语言模型**。
3.  **查询与响应**：用户发起请求，系统返回结果的基本交互。

![](img/a019dd4f18120d1bba780fd4e8073049_51.png)

![](img/a019dd4f18120d1bba780fd4e8073049_52.png)

![](img/a019dd4f18120d1bba780fd4e8073049_54.png)

![](img/a019dd4f18120d1bba780fd4e8073049_56.png)

![](img/a019dd4f18120d1bba780fd4e8073049_57.png)

![](img/a019dd4f18120d1bba780fd4e8073049_59.png)

其基本流程可以表示为：
```
用户 -> [查询] -> 模型API -> [响应] -> 用户
```

![](img/a019dd4f18120d1bba780fd4e8073049_61.png)

![](img/a019dd4f18120d1bba780fd4e8073049_63.png)

## 引入检索增强生成

![](img/a019dd4f18120d1bba780fd4e8073049_65.png)

![](img/a019dd4f18120d1bba780fd4e8073049_67.png)

为了使模型能基于外部知识生成回复，我们需要引入**检索增强生成**组件。

![](img/a019dd4f18120d1bba780fd4e8073049_69.png)

![](img/a019dd4f18120d1bba780fd4e8073049_70.png)

![](img/a019dd4f18120d1bba780fd4e8073049_71.png)

![](img/a019dd4f18120d1bba780fd4e8073049_72.png)

RAG的工作流程如下：
1.  用户查询首先发送到RAG组件。
2.  RAG从外部数据源（如互联网或数据库）检索相关信息。
3.  将检索到的信息与原始查询结合，再发送给大语言模型进行生成。

![](img/a019dd4f18120d1bba780fd4e8073049_74.png)

![](img/a019dd4f18120d1bba780fd4e8073049_75.png)

![](img/a019dd4f18120d1bba780fd4e8073049_76.png)

![](img/a019dd4f18120d1bba780fd4e8073049_77.png)

![](img/a019dd4f18120d1bba780fd4e8073049_79.png)

![](img/a019dd4f18120d1bba780fd4e8073049_80.png)

![](img/a019dd4f18120d1bba780fd4e8073049_82.png)

我们可以用以下伪代码描述其思想：
```python
# 简化的RAG流程概念
def rag_pipeline(user_query):
    retrieved_info = retrieve_from_external_source(user_query) # 检索
    augmented_prompt = combine(user_query, retrieved_info) # 增强
    response = llm_generate(augmented_prompt) # 生成
    return response
```

![](img/a019dd4f18120d1bba780fd4e8073049_84.png)

![](img/a019dd4f18120d1bba780fd4e8073049_85.png)

![](img/a019dd4f18120d1bba780fd4e8073049_87.png)

![](img/a019dd4f18120d1bba780fd4e8073049_89.png)

![](img/a019dd4f18120d1bba780fd4e8073049_90.png)

![](img/a019dd4f18120d1bba780fd4e8073049_92.png)

![](img/a019dd4f18120d1bba780fd4e8073049_94.png)

![](img/a019dd4f18120d1bba780fd4e8073049_96.png)

## 添加输入输出护栏

![](img/a019dd4f18120d1bba780fd4e8073049_97.png)

![](img/a019dd4f18120d1bba780fd4e8073049_99.png)

![](img/a019dd4f18120d1bba780fd4e8073049_100.png)

![](img/a019dd4f18120d1bba780fd4e8073049_101.png)

![](img/a019dd4f18120d1bba780fd4e8073049_103.png)

![](img/a019dd4f18120d1bba780fd4e8073049_104.png)

为了确保系统的安全性和可靠性，我们需要在数据流入模型前和流出模型后设置检查点。

![](img/a019dd4f18120d1bba780fd4e8073049_106.png)

![](img/a019dd4f18120d1bba780fd4e8073049_108.png)

以下是需要添加的护栏：
*   **输入护栏**：在查询进入RAG或模型前，检查用户输入是否安全、合规。
*   **输出护栏**：在模型生成响应后、返回给用户前，检查输出内容是否恰当、无危害。

![](img/a019dd4f18120d1bba780fd4e8073049_110.png)

![](img/a019dd4f18120d1bba780fd4e8073049_112.png)

![](img/a019dd4f18120d1bba780fd4e8073049_113.png)

此时，数据流更新为：
```
用户 -> [输入护栏] -> RAG -> 模型API -> [输出护栏] -> 用户
```

![](img/a019dd4f18120d1bba780fd4e8073049_115.png)

![](img/a019dd4f18120d1bba780fd4e8073049_116.png)

![](img/a019dd4f18120d1bba780fd4e8073049_118.png)

![](img/a019dd4f18120d1bba780fd4e8073049_120.png)

![](img/a019dd4f18120d1bba780fd4e8073049_121.png)

![](img/a019dd4f18120d1bba780fd4e8073049_123.png)

## 考虑性能优化：缓存

![](img/a019dd4f18120d1bba780fd4e8073049_125.png)

![](img/a019dd4f18120d1bba780fd4e8073049_126.png)

![](img/a019dd4f18120d1bba780fd4e8073049_128.png)

在架构中引入缓存可以显著提升响应速度并降低计算成本。缓存可以放置在多个位置。

![](img/a019dd4f18120d1bba780fd4e8073049_129.png)

![](img/a019dd4f18120d1bba780fd4e8073049_130.png)

![](img/a019dd4f18120d1bba780fd4e8073049_132.png)

以下是几个可选的缓存位置：
1.  **提示词缓存**：缓存用户频繁使用的查询或提示模板。
2.  **RAG结果缓存**：缓存RAG组件从外部源检索到的结果。
3.  **模型输出缓存**：缓存模型对相同或相似输入的生成结果。

![](img/a019dd4f18120d1bba780fd4e8073049_134.png)

![](img/a019dd4f18120d1bba780fd4e8073049_136.png)

![](img/a019dd4f18120d1bba780fd4e8073049_137.png)

![](img/a019dd4f18120d1bba780fd4e8073049_139.png)

![](img/a019dd4f18120d1bba780fd4e8073049_141.png)

对于我们的简化架构，可以在用户输入后、进入主要处理流程前添加一个**提示缓存**。

![](img/a019dd4f18120d1bba780fd4e8073049_143.png)

## 明确组件细节与业务沟通

![](img/a019dd4f18120d1bba780fd4e8073049_145.png)

![](img/a019dd4f18120d1bba780fd4e8073049_146.png)

![](img/a019dd4f18120d1bba780fd4e8073049_147.png)

![](img/a019dd4f18120d1bba780fd4e8073049_148.png)

![](img/a019dd4f18120d1bba780fd4e8073049_150.png)

![](img/a019dd4f18120d1bba780fd4e8073049_152.png)

架构图不仅是技术文档，也是与业务方沟通的工具。因此，我们需要为关键组件添加有业务意义的说明。

![](img/a019dd4f18120d1bba780fd4e8073049_154.png)

![](img/a019dd4f18120d1bba780fd4e8073049_156.png)

![](img/a019dd4f18120d1bba780fd4e8073049_158.png)

![](img/a019dd4f18120d1bba780fd4e8073049_160.png)

![](img/a019dd4f18120d1bba780fd4e8073049_162.png)

以下是为组件添加说明的示例：
*   **大语言模型**：可注明参数规模（如`700亿参数`），以说明其能力与成本。
*   **数据库**：可注明类型（如`向量数据库`），以说明其专门用于存储和检索AI所需的嵌入向量。
*   **护栏**：强调其用于保障内容安全与合规。
*   **缓存**：说明其用于提升系统性能与降低成本。

![](img/a019dd4f18120d1bba780fd4e8073049_164.png)

![](img/a019dd4f18120d1bba780fd4e8073049_165.png)

![](img/a019dd4f18120d1bba780fd4e8073049_167.png)

![](img/a019dd4f18120d1bba780fd4e8073049_168.png)

![](img/a019dd4f18120d1bba780fd4e8073049_170.png)

## 作业提交与扩展思考

完成架构图绘制后，需要将其保存为图片（如PNG或JPG格式），并放入GitHub仓库的指定文件夹中。

![](img/a019dd4f18120d1bba780fd4e8073049_172.png)

![](img/a019dd4f18120d1bba780fd4e8073049_173.png)

![](img/a019dd4f18120d1bba780fd4e8073049_174.png)

![](img/a019dd4f18120d1bba780fd4e8073049_175.png)

![](img/a019dd4f18120d1bba780fd4e8073049_176.png)

![](img/a019dd4f18120d1bba780fd4e8073049_177.png)

![](img/a019dd4f18120d1bba780fd4e8073049_178.png)

除了架构图，你也可以尝试回答一些扩展问题来深化思考：

![](img/a019dd4f18120d1bba780fd4e8073049_180.png)

![](img/a019dd4f18120d1bba780fd4e8073049_181.png)

![](img/a019dd4f18120d1bba780fd4e8073049_182.png)

![](img/a019dd4f18120d1bba780fd4e8073049_183.png)

![](img/a019dd4f18120d1bba780fd4e8073049_185.png)

![](img/a019dd4f18120d1bba780fd4e8073049_187.png)

![](img/a019dd4f18120d1bba780fd4e8073049_188.png)

![](img/a019dd4f18120d1bba780fd4e8073049_189.png)

以下是几个可以思考的扩展方向：
*   **功能需求**：系统必须具备的具体能力是什么？
*   **非功能需求**：系统在性能、安全、成本等方面的约束是什么？
*   **假设**：我们在规划和开发时认定的、无需证明的前提条件是什么？（例如：“我们假设所选开源模型能在1-1.5万美元的硬件投资上有效运行。”）
*   **数据策略**：如何收集、准备数据？如何保证质量、多样性和隐私？
*   **技术考量**：选择特定模型或技术栈的原因是什么？（例如：“考虑使用IBM Granite模型，因为它是真正的开源模型，训练数据可追溯，有助于避免版权问题。”）

![](img/a019dd4f18120d1bba780fd4e8073049_191.png)

![](img/a019dd4f18120d1bba780fd4e8073049_193.png)

![](img/a019dd4f18120d1bba780fd4e8073049_195.png)

![](img/a019dd4f18120d1bba780fd4e8073049_197.png)

![](img/a019dd4f18120d1bba780fd4e8073049_198.png)

![](img/a019dd4f18120d1bba780fd4e8073049_200.png)

## 总结

![](img/a019dd4f18120d1bba780fd4e8073049_202.png)

![](img/a019dd4f18120d1bba780fd4e8073049_204.png)

![](img/a019dd4f18120d1bba780fd4e8073049_206.png)

本节课中我们一起学习了如何为一个生成式AI应用绘制简化架构图。我们从核心的用户-模型交互开始，逐步引入了**检索增强生成**、**输入输出护栏**和**缓存层**，并讨论了如何为组件添加业务说明以便沟通。记住，架构图的目标是清晰传达系统设计，为后续开发和讨论奠定基础。现在，请动手创建你自己的架构图吧！