# 39：听力理解应用开发教程

![](img/f725e4b8fde0f83a900447c51f2d804c_1.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_2.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_4.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_6.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_8.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_10.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_12.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_13.png)

## 概述

![](img/f725e4b8fde0f83a900447c51f2d804c_15.png)

在本节课中，我们将学习如何构建一个日语听力理解应用。我们将从整合代码库开始，逐步实现音频转录、数据结构化处理、向量数据库存储以及交互式问题生成等功能。课程将使用 Amazon Bedrock 等工具，并重点关注如何将原始 YouTube 听力测试转录稿处理成结构化的、可用于生成新练习题的数据。

![](img/f725e4b8fde0f83a900447c51f2d804c_17.png)

---

![](img/f725e4b8fde0f83a900447c51f2d804c_19.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_21.png)

## 整合代码库与项目初始化

![](img/f725e4b8fde0f83a900447c51f2d804c_23.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_24.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_25.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_26.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_28.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_29.png)

上一节我们结束了直播。本节中，我们首先需要将分叉的代码库整合到主仓库中，以便统一管理。

![](img/f725e4b8fde0f83a900447c51f2d804c_31.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_33.png)

以下是整合步骤：

![](img/f725e4b8fde0f83a900447c51f2d804c_35.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_37.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_39.png)

1.  打开本地项目目录，找到 `language learning assistant` 文件夹。
2.  删除其中的 `.git` 和 `.gitignore` 文件（或仅复制其内容）。
3.  将文件夹重命名为更具描述性的名称，例如 `Lang_Listening`。
4.  打开主仓库目录，将整理好的文件夹及其内容复制进去。
5.  更新项目结构，确保前端和后端代码位于统一的目录下。

完成整合后，项目结构更加清晰，便于后续开发。

![](img/f725e4b8fde0f83a900447c51f2d804c_41.png)

---

## 配置开发环境与工具选择

现在，我们需要配置开发环境并选择实现核心功能的技术栈。

以下是环境配置与工具选择要点：

*   **后端框架**：继续使用 Amazon Bedrock，因其性能出色，特别是 `Nova Micro` 模型。
*   **转录工具**：从 `Amazon Transcribe` 和 `Polyly` 切换到开源的 `Whisper`，以实现本地化处理。
*   **开发环境**：使用 Conda 创建并激活独立的 Python 环境（例如 `conda activate ll_app`）。
*   **项目运行**：
    *   前端：在项目根目录运行 `streamlit run frontend/main.py`。
    *   后端：确保已安装依赖（`pip install -r requirements.txt`），代码通常与前端集成在同一代码库中运行。

配置好环境后，我们可以开始实现核心的转录功能。

![](img/f725e4b8fde0f83a900447c51f2d804c_43.png)

---

![](img/f725e4b8fde0f83a900447c51f2d804c_45.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_47.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_49.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_51.png)

## 实现音频转录与原始数据处理

![](img/f725e4b8fde0f83a900447c51f2d804c_53.png)

本节我们将实现从 YouTube 视频获取并转录音频的功能，这是应用的数据源头。

![](img/f725e4b8fde0f83a900447c51f2d804c_55.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_57.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_59.png)

我们使用以下流程：

1.  在应用前端输入 YouTube 视频链接（例如 JLPT N5 听力练习视频）。
2.  应用调用后端服务，使用 `Whisper` 下载并转录视频音频。
3.  转录完成后，前端显示原始文本、总字符数、日语字符数等信息。

核心的转录调用代码如下：

```python
# 伪代码示例：调用转录服务
transcript_text = transcribe_youtube_audio(youtube_url)
display_raw_transcript(transcript_text)
```

此步骤完成后，我们获得了未经处理的原始文本数据，接下来需要对其进行结构化解析。

---

## 结构化数据解析与问题提取

原始转录文本是连续的，包含介绍、例题、问题、答案解释等部分。本节目标是从中精准提取出所有听力问题。

我们面临以下挑战：

![](img/f725e4b8fde0f83a900447c51f2d804c_61.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_63.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_65.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_67.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_69.png)

1.  **结构识别**：JLPT 听力测试通常包含多个部分（如 `問題1`、`問題2`），每部分格式可能不同（有图题、无图题、长对话题）。
2.  **信息提取**：需要提取每个问题的“场景介绍”、“对话内容”和“问题本身”。对于有图题，还需处理图像选项描述。
3.  **格式统一**：确保提取出的数据格式一致，便于后续处理。

![](img/f725e4b8fde0f83a900447c51f2d804c_71.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_73.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_75.png)

我们使用 Amazon Bedrock 的 `Nova Light` 模型，通过精心设计的提示词（Prompt）来解析文本。初始提示词可能如下：

![](img/f725e4b8fde0f83a900447c51f2d804c_77.png)

```python
prompt_template = """
你有一个JLPT听力练习测试的转录稿。
请从中提取所有问题。
每个问题按以下结构输出：
- 介绍：
- 对话：
- 问题：
请仅输出提取的问题内容，不要包含章节描述或其他解释性文字。
转录稿内容：{transcript_text}
"""
```

通过多次迭代和优化提示词（例如，指定忽略例题、区分不同题型、要求不翻译日语文本等），我们最终能将转录稿解析为结构化的 JSON 或字典格式，并保存到本地文件（如 `data/questions/section_2_questions.txt`）。

![](img/f725e4b8fde0f83a900447c51f2d804c_79.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_81.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_83.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_85.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_86.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_88.png)

这是构建知识库最关键且最耗时的一步，直接决定了后续生成问题的质量。

![](img/f725e4b8fde0f83a900447c51f2d804c_90.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_92.png)

---

![](img/f725e4b8fde0f83a900447c51f2d804c_94.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_96.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_98.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_99.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_101.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_103.png)

## 构建向量数据库（RAG）

![](img/f725e4b8fde0f83a900447c51f2d804c_105.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_107.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_109.png)

获得结构化问题数据后，我们需要将其存储起来，以便基于语义搜索来生成新的相关练习题。本节我们使用 ChromaDB 构建向量数据库。

![](img/f725e4b8fde0f83a900447c51f2d804c_111.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_113.png)

以下是实现步骤：

1.  **选择嵌入模型**：使用 Amazon Bedrock 的 `Titan Embedding` 模型（支持多语言，包括日语）将文本问题转换为向量。
2.  **创建集合**：根据题型（如 `section_2`, `section_3`）创建不同的集合（Collection），实现问题分类存储。
3.  **数据插入**：读取上一步生成的结构化问题文件，将每个问题及其元数据（如题型、主题）转换为向量并存入对应的集合。
4.  **实现搜索**：提供相似性搜索功能，可根据输入的主题或示例问题，在指定集合中查找最相关的若干条现有问题。

核心的向量存储与搜索代码如下：

![](img/f725e4b8fde0f83a900447c51f2d804c_115.png)

```python
# 伪代码：初始化向量存储并插入数据
from backend.vector_store import VectorStore
vector_store = VectorStore()
vector_store.add_questions(section="section_2", questions=parsed_questions_list)

![](img/f725e4b8fde0f83a900447c51f2d804c_117.png)

# 伪代码：进行相似性搜索
similar_questions = vector_store.search_similar_questions(
    query="关于餐厅点餐的对话",
    section="section_2",
    n_results=3
)
```

![](img/f725e4b8fde0f83a900447c51f2d804c_119.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_121.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_123.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_125.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_127.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_129.png)

将向量数据库与数据解析逻辑解耦，使得系统更灵活。同时，注意将向量数据库文件（如 `*.bin`）添加到 `.gitignore` 中，避免提交大文件。

---

## 实现交互式问题生成与界面

有了向量数据库作为知识库，本节我们将实现核心的交互功能：根据用户选择的主题，生成新的听力练习题。

以下是功能流程：

1.  **前端界面**：使用 Streamlit 构建简洁界面。用户可以选择一个主题（如“餐厅”、“旅行”）。
2.  **生成请求**：用户点击“生成新问题”后，前端将主题发送到后端。
3.  **RAG 生成**：
    *   后端使用向量数据库，根据主题搜索出最相关的几个示例问题。
    *   将这些示例问题作为上下文，连同生成指令，发送给 `Nova Pro` 模型。
    *   模型根据示例的格式和风格，创作一个全新的、但主题相关的问题（包括场景、对话、问题和四个选项）。
4.  **结果显示与交互**：前端显示生成的问题文本和选项。用户选择答案后提交，系统立即给出反馈（正确/错误）并显示正确答案。

![](img/f725e4b8fde0f83a900447c51f2d804c_131.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_133.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_135.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_137.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_139.png)

后端的生成函数核心逻辑如下：

![](img/f725e4b8fde0f83a900447c51f2d804c_141.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_143.png)

```python
def generate_new_question(topic: str, section: str = "section_2") -> dict:
    # 1. 从向量库查找相似问题
    example_questions = vector_store.search_similar_questions(topic, section, n_results=2)

    # 2. 构建生成提示词
    prompt = f"""
    基于以下示例问题，生成一个关于'{topic}'的新听力理解问题。
    请严格遵循示例的格式（介绍、对话、问题、四个选项）。
    确保问题是原创的，且具有清晰的正确答案。

    示例问题：
    {example_questions}

    新问题：
    """

    # 3. 调用LLM生成
    response = bedrock_client.invoke_model(prompt, model_id="nova-pro")
    # 4. 解析并返回结构化的新问题
    return parse_llm_response(response)
```

在前端，我们优化了UI，移除了不必要的侧边栏和其他标签页，专注于交互式学习模块，使得用户体验更集中、流畅。

![](img/f725e4b8fde0f83a900447c51f2d804c_145.png)

---

![](img/f725e4b8fde0f83a900447c51f2d804c_147.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_149.png)

## 挑战与未来方向：音频生成

![](img/f725e4b8fde0f83a900447c51f2d804c_151.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_153.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_155.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_157.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_159.png)

目前，我们的应用还缺少关键一环：将生成的文字问题转换为可播放的音频。本节我们探讨其挑战与可能方案。

![](img/f725e4b8fde0f83a900447c51f2d804c_161.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_163.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_165.png)

实现音频生成面临以下难点：

![](img/f725e4b8fde0f83a900447c51f2d804c_167.png)

1.  **多角色语音**：听力对话涉及不同角色（男、女、旁白），需要能生成不同音色的语音。
2.  **自然度与节奏**：对话间需要有自然的停顿，整体节奏需符合真实听力测试。
3.  **技术集成**：可能需要结合多个TTS服务，并使用如FFmpeg的工具进行音频拼接与后期处理。

![](img/f725e4b8fde0f83a900447c51f2d804c_169.png)

一个潜在的方案是：
*   使用 Amazon Polly 或类似服务，指定不同声音ID来生成各角色语音。
*   将生成的独立音频片段，根据脚本的时间标记，用FFmpeg合成一个完整的音频文件。
*   在前端提供音频播放器。

![](img/f725e4b8fde0f83a900447c51f2d804c_171.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_173.png)

由于实现复杂度较高，且涉及额外服务配置，我们将在后续课程或项目中深入探讨。

![](img/f725e4b8fde0f83a900447c51f2d804c_175.png)

---

![](img/f725e4b8fde0f83a900447c51f2d804c_177.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_179.png)

## 总结

![](img/f725e4b8fde0f83a900447c51f2d804c_181.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_183.png)

本节课中我们一起学习了构建一个日语听力理解应用的完整流程。

![](img/f725e4b8fde0f83a900447c51f2d804c_185.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_187.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_189.png)

我们从**项目初始化与整合**开始，选择了 **Amazon Bedrock** 和 **Whisper** 作为核心技术栈。然后，我们攻克了最复杂的部分：**将杂乱的原始转录稿通过多次迭代的Prompt工程，解析成结构化的JSON数据**。接着，我们利用 **ChromaDB 向量数据库**存储这些问题，实现了基于语义的检索（RAG）。在此基础上，我们开发了**交互式问题生成功能**，能够根据主题自动创作新的练习题。最后，我们探讨了**音频生成**这一高级功能的挑战与可能性。

![](img/f725e4b8fde0f83a900447c51f2d804c_191.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_193.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_194.png)

![](img/f725e4b8fde0f83a900447c51f2d804c_196.png)

通过本项目，我们实践了LLM应用开发中数据处理、知识库构建、内容生成等核心环节，为构建更复杂的生成式AI应用打下了坚实基础。