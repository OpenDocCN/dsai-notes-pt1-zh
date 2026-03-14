# 53：重新实现歌曲转词汇代理

![](img/4a233499b334cdc6af379b7bbc057a00_0.png)

![](img/4a233499b334cdc6af379b7bbc057a00_2.png)

![](img/4a233499b334cdc6af379b7bbc057a00_4.png)

![](img/4a233499b334cdc6af379b7bbc057a00_0.png)

![](img/4a233499b334cdc6af379b7bbc057a00_6.png)

![](img/4a233499b334cdc6af379b7bbc057a00_8.png)

![](img/4a233499b334cdc6af379b7bbc057a00_10.png)

在本节课中，我们将学习如何实现一个能够从互联网上查找歌曲歌词并提取词汇的AI代理。我们将使用本地运行的LLM（如Mistral）和FastAPI框架来构建这个系统。

![](img/4a233499b334cdc6af379b7bbc057a00_12.png)

![](img/4a233499b334cdc6af379b7bbc057a00_14.png)

## 概述

![](img/4a233499b334cdc6af379b7bbc057a00_16.png)

![](img/4a233499b334cdc6af379b7bbc057a00_18.png)

![](img/4a233499b334cdc6af379b7bbc057a00_20.png)

我们将创建一个AI代理，其核心功能是：接收一首歌曲的名称（和可选的艺术家信息），自动从互联网上搜索并获取其歌词，然后分析歌词内容，提取出对语言学习者有价值的词汇列表。整个系统将遵循ReAct（推理-行动）框架，让LLM自主决定使用哪些工具（如网页搜索、内容提取）来完成目标。

上一节我们介绍了代理的基本概念，本节中我们来看看如何具体实现一个功能完整的代理。

![](img/4a233499b334cdc6af379b7bbc057a00_22.png)

![](img/4a233499b334cdc6af379b7bbc057a00_24.png)

## 项目初始化与规划

![](img/4a233499b334cdc6af379b7bbc057a00_26.png)

![](img/4a233499b334cdc6af379b7bbc057a00_28.png)

![](img/4a233499b334cdc6af379b7bbc057a00_30.png)

![](img/4a233499b334cdc6af379b7bbc057a00_32.png)

![](img/4a233499b334cdc6af379b7bbc057a00_34.png)

首先，我们创建一个新的项目目录并规划技术方案。

![](img/4a233499b334cdc6af379b7bbc057a00_36.png)

![](img/4a233499b334cdc6af379b7bbc057a00_38.png)

![](img/4a233499b334cdc6af379b7bbc057a00_40.png)

![](img/4a233499b334cdc6af379b7bbc057a00_42.png)

![](img/4a233499b334cdc6af379b7bbc057a00_44.png)

```bash
mkdir song_vocab
cd song_vocab
```

我们创建一个文本规范文件来描述项目目标和技术要求。

**业务目标**：创建一个程序，能够从互联网上查找特定语言的歌曲歌词，并生成可导入数据库的词汇表。

![](img/4a233499b334cdc6af379b7bbc057a00_46.png)

![](img/4a233499b334cdc6af379b7bbc057a00_48.png)

**技术要求**：
*   使用Python和FastAPI构建API。
*   使用Ollama API本地运行LLM（如Mistral）作为代理的核心。
*   使用SQLite存储结果（可选）。
*   使用Instructor库来结构化LLM的JSON输出。
*   工具包括：网页搜索、获取页面内容、提取词汇。

## 选择与配置本地模型

![](img/4a233499b334cdc6af379b7bbc057a00_50.png)

![](img/4a233499b334cdc6af379b7bbc057a00_52.png)

为了实现本地计算，我们选择使用Ollama来运行Mistral 7B模型。

```bash
sudo snap install ollama
ollama run mistral
```

![](img/4a233499b334cdc6af379b7bbc057a00_54.png)

![](img/4a233499b334cdc6af379b7bbc057a00_56.png)

这个模型大小适中，能够在本地机器上运行，并且支持我们所需的多步推理任务。虽然对于某些语言（如日语）的支持可能不是最优，但我们可以通过提示词工程来引导它。

![](img/4a233499b334cdc6af379b7bbc057a00_58.png)

![](img/4a233499b334cdc6af379b7bbc057a00_60.png)

![](img/4a233499b334cdc6af379b7bbc057a00_62.png)

## 设计系统架构

![](img/4a233499b334cdc6af379b7bbc057a00_64.png)

![](img/4a233499b334cdc6af379b7bbc057a00_66.png)

我们的系统主要包含一个API端点和若干工具函数。

![](img/4a233499b334cdc6af379b7bbc057a00_68.png)

![](img/4a233499b334cdc6af379b7bbc057a00_70.png)

![](img/4a233499b334cdc6af379b7bbc057a00_71.png)

**核心API端点**：`POST /api/agent`
*   **请求**：一个包含歌曲名（和可选艺术家名）的字符串消息。
*   **响应**：一个唯一的歌曲ID，用于查找存储的歌词和词汇文件。

![](img/4a233499b334cdc6af379b7bbc057a00_73.png)

**工具函数**：
1.  `search_web(query: str)`: 使用DuckDuckGo（或SerpAPI）搜索互联网。
2.  `get_page_content(url: str)`: 获取并解析网页内容，提取文本。
3.  `extract_vocabulary(text: str)`: 使用LLM从文本中提取所有词汇，并输出结构化JSON。
4.  `save_results(song_id: str, lyrics: str, vocabulary: list)`: 将歌词和词汇结果保存到文件。

![](img/4a233499b334cdc6af379b7bbc057a00_75.png)

![](img/4a233499b334cdc6af379b7bbc057a00_77.png)

代理的工作流程是：接收用户请求 -> LLM思考并选择工具 -> 执行工具 -> 观察结果 -> 重复直到任务完成（找到歌词并提取词汇）-> 保存结果并返回歌曲ID。

## 实现提示词与代理逻辑

![](img/4a233499b334cdc6af379b7bbc057a00_79.png)

代理的提示词存储在 `prompts/lyrics_agent.md` 中。它定义了代理的角色、可用工具、目标语言（例如日语）以及需要遵循的ReAct框架。

以下是提示词的核心部分：
```
你是一个帮助寻找歌曲歌词并从中提取词汇的AI助手。
目标语言：日语。
你有权使用以下工具：search_web, get_page_content, extract_vocabulary, save_results。
请遵循ReAct框架：思考你需要做什么，从可用工具中选择一个行动，观察结果，然后思考下一步。
当你获得最终结果（歌词和词汇）时，使用save_results工具保存它们，并仅返回歌曲ID。
```

![](img/4a233499b334cdc6af379b7bbc057a00_81.png)

在代码中，我们使用Ollama客户端调用Mistral模型，并将提示词和对话历史发送给它。我们解析模型的响应，提取出它想要执行的动作（工具名和参数），然后调用相应的工具函数。

![](img/4a233499b334cdc6af379b7bbc057a00_83.png)

## 实现工具函数

![](img/4a233499b334cdc6af379b7bbc057a00_85.png)

![](img/4a233499b334cdc6af379b7bbc057a00_87.png)

![](img/4a233499b334cdc6af379b7bbc057a00_89.png)

![](img/4a233499b334cdc6af379b7bbc057a00_91.png)

以下是核心工具的实现要点：

**网页搜索 (`tools/search_web.py`)**:
最初使用DuckDuckGo，但遇到了速率限制问题。因此我们添加了备用方案SerpAPI。代码中实现了重试逻辑和回退机制。

![](img/4a233499b334cdc6af379b7bbc057a00_93.png)

![](img/4a233499b334cdc6af379b7bbc057a00_95.png)

![](img/4a233499b334cdc6af379b7bbc057a00_97.png)

![](img/4a233499b334cdc6af379b7bbc057a00_99.png)

![](img/4a233499b334cdc6af379b7bbc057a00_101.png)

![](img/4a233499b334cdc6af379b7bbc057a00_103.png)

![](img/4a233499b334cdc6af379b7bbc057a00_105.png)

**获取页面内容 (`tools/get_page_content.py`)**:
使用`requests`库获取HTML，然后使用`BeautifulSoup`解析页面，移除脚本、样式等标签，提取纯文本内容。

![](img/4a233499b334cdc6af379b7bbc057a00_107.png)

![](img/4a233499b334cdc6af379b7bbc057a00_109.png)

**提取词汇 (`tools/extract_vocabulary.py`)**:
这是核心的LLM调用。我们使用`instructor`库与Ollama结合，定义一个Pydantic模型（如`VocabularyList`），其中包含词汇项列表，每个项有`word`、`reading`、`definition`等字段。这确保了输出的结构化。

![](img/4a233499b334cdc6af379b7bbc057a00_111.png)

**保存结果 (`tools/save_results.py`)**:
将歌词保存为`.txt`文件，将词汇列表保存为`.json`文件，存储在`outputs/`目录下，文件名基于生成的歌曲ID。

![](img/4a233499b334cdc6af379b7bbc057a00_113.png)

![](img/4a233499b334cdc6af379b7bbc057a00_115.png)

![](img/4a233499b334cdc6af379b7bbc057a00_117.png)

## 运行与测试

![](img/4a233499b334cdc6af379b7bbc057a00_119.png)

![](img/4a233499b334cdc6af379b7bbc057a00_121.png)

![](img/4a233499b334cdc6af379b7bbc057a00_123.png)

![](img/4a233499b334cdc6af379b7bbc057a00_125.png)

![](img/4a233499b334cdc6af379b7bbc057a00_127.png)

我们创建一个启动脚本 `bin/post` 来测试API。

```bash
#!/bin/bash
curl -X POST http://localhost:8000/api/agent \
  -H "Content-Type: application/json" \
  -d '{"message": "find lyrics for Yosobi Idol"}'
```

运行FastAPI服务器：
```bash
uvicorn main:app --reload
```

然后执行测试脚本。代理会开始工作，我们可以在日志中看到它的思考过程、工具调用和结果。

![](img/4a233499b334cdc6af379b7bbc057a00_129.png)

## 调试与优化

![](img/4a233499b334cdc6af379b7bbc057a00_131.png)

![](img/4a233499b334cdc6af379b7bbc057a00_133.png)

在开发过程中，我们遇到了几个问题：
1.  **DuckDuckGo速率限制**：通过添加SerpAPI作为备用搜索提供商解决。
2.  **LLM响应解析**：需要稳健地解析模型输出，以提取工具调用指令。我们实现了`parse_lm_action`函数来处理。
3.  **循环控制**：代理可能陷入无限循环。我们设置了最大迭代次数（例如10次）来防止这种情况。
4.  **日志记录**：添加详细的日志记录对于理解代理的决策过程和调试至关重要。

![](img/4a233499b334cdc6af379b7bbc057a00_135.png)

![](img/4a233499b334cdc6af379b7bbc057a00_137.png)

## 总结

![](img/4a233499b334cdc6af379b7bbc057a00_139.png)

本节课中我们一起学习了如何从零开始构建一个功能性的AI代理。我们涵盖了项目规划、本地LLM配置、系统架构设计、ReAct模式实现、工具函数开发以及调试优化全过程。关键点在于将复杂任务分解为可由LLM协调的多个工具步骤，并通过结构化的提示词和输出来引导模型行为。虽然最终实现可能需要进一步的调试和优化，但核心框架已经建立，展示了构建自主AI代理的基本方法。