# 32：多模态应用开发 🎯

![](img/a9b3147697529d2d2c58f32f90520322_1.png)

## 概述
在本节课中，我们将学习如何构建一个结合多种模态（如文本、语音）的生成式AI应用。我们将创建一个语言听力理解学习应用，它能够从YouTube获取转录文本，利用大型语言模型进行分析和生成，并整合语音合成功能。我们将使用Python、Streamlit、ChromaDB向量数据库以及Amazon Bedrock等工具。

---

![](img/a9b3147697529d2d2c58f32f90520322_3.png)

## 课程开场与提醒

大家好，我是Andrew Brown，欢迎来到免费生成式AI训练营。我们开始第二周（或第三周，取决于是否从零开始计数）的学习。

在介绍本周讲师之前，我想提醒大家：如果您有听力障碍，在LinkedIn上观看直播流是最佳选择，因为YouTube并不总是提供实时字幕，而LinkedIn可以。链接已放在描述中。

现在，请允许我介绍我们本周的客座讲师。

![](img/a9b3147697529d2d2c58f32f90520322_5.png)

大家好，Andrew，我很好。大家好，感谢邀请我。

---

## 赞助商与课程资源

![](img/a9b3147697529d2d2c58f32f90520322_7.png)

我们首先要感谢本次训练营的赞助商Intel。他们有三个基于社区的产品，我鼓励大家更多地参与和使用。

上周我们使用了OPA。我想了解一下，训练营的学员们使用OPA的体验如何？是简单还是困难？请在聊天中分享。

（观看评论片刻）看来有些人进展顺利，有些人遇到了一些问题，比如Mega Service应用无法完全工作，或者容器崩溃。这很正常，所以本周的课程安排会相对轻松一些，以便大家有时间赶上进度。

我们还有赞助商Torque，他们是一个帮助开发者连接工作机会的平台。我与Torque的Ken Collins进行了一次非常精彩的职业分享，讨论了“AI应用工程师”的定义，以及公司在匹配这些技能时面临的挑战。基于那次反馈，我可能会录制一系列视频，包括“GenAI Essentials”课程。

![](img/a9b3147697529d2d2c58f32f90520322_9.png)

![](img/a9b3147697529d2d2c58f32f90520322_10.png)

此外，我们还有Code Rabbit。在开发者周期间，我提到想涵盖代码审查，而Code Rabbit正好是这方面的工具。随着我们的项目越来越复杂，使用AI进行代码审查将非常有用。

---

## 常见问题解答 (FAQ)

*   **何时可以提交作业？** 本周我发布表单稍晚，因为我需要确定作业要求，并提供一个评分视频，让大家了解我从我的角度如何评分，以便你们能提交最好的作业。
*   **如何运行打字导师游戏？** 如果你已经让前端和后端运行起来，但游戏没有出现，那是因为我当时没有将其包含在代码库中。现在它已经在ExamPro的代码仓库 (`exampro-co/free-genai-bootcamp-2025`) 中，你可以下载并放入你的项目。
*   **如果无法完成后端/前端，如何继续？** 在“开发工具周”，你们的任务是从头构建后端或重新实现我们移除的代码。如果因故无法完成，没关系，ExamPro代码仓库中有完整的代码。但请注意，你需要下载一个特定的提交版本，我故意设置了一个“代码陷阱”，以防止简单的复制粘贴。
*   **我甚至没时间构建前端，落后了怎么办？** 别担心，连我也落后于自己的预期。即使落后，也要尽力提交。
*   **何时能看到评分？** 如果你坚持到最后一周并提交，无论如何都会得到评分。如果你提交了所有周次的作业，会得到我个人的评分。但由于提交量很大，评分会分散在不同周次进行，有些学员可能要到训练营结束时才看到所有评分。我们会尽力而为。

---

## 本周重点：多模态

本周我们专注于**多模态**。这意味着我们将处理除纯文本大语言模型之外的其他生成式AI模型。

我们将涉及：
*   **语音转文本 (ASR)**：例如Open Whisper。
*   **计算机视觉**：例如Robs的ASL手指拼写。
*   **文本转语音 (TTS)**：例如AWS Polly等托管服务。
*   **视觉编码器-解码器**：处理图像并生成结果。

这是非常有趣的一周，但任务相对较轻，请大家利用这个机会赶上之前的进度。

---

## 时间管理与学习建议

对于同时处理多项事务的学员，这里有一些建议：
1.  **对自己宽容一些**。你不可能完成所有事情。
2.  **优先排序**。列出需要完成的事项，先处理最重要的。
3.  **时间盒**。例如使用番茄工作法，专注25分钟在一件事上，然后切换。
4.  **保持一致性**。即使每周只提交一小段代码，坚持到底也能获得徽章。从本次训练营中学到的最重要技能之一就是时间管理。

---

![](img/a9b3147697529d2d2c58f32f90520322_12.png)

![](img/a9b3147697529d2d2c58f32f90520322_13.png)

## 项目介绍：语言听力理解学习应用

![](img/a9b3147697529d2d2c58f32f90520322_15.png)

![](img/a9b3147697529d2d2c58f32f90520322_17.png)

![](img/a9b3147697529d2d2c58f32f90520322_19.png)

现在，让我们开始构建本周的项目。

![](img/a9b3147697529d2d2c58f32f90520322_21.png)

### 业务目标
你是一名AI应用工程师，任务是构建一个**语言听力理解学习应用**。

![](img/a9b3147697529d2d2c58f32f90520322_23.png)

### 应用功能
该应用将：
1.  从YouTube获取语言学习测试（如JLPT N5）的听力理解示例内容。
2.  利用这些内容生成类似风格的、无限的听力理解练习题。

**为什么需要这个？** 传统的听力练习，一旦做过就知道答案。我们需要一个能持续生成新练习的系统。

![](img/a9b3147697529d2d2c58f32f90520322_25.png)

![](img/a9b3147697529d2d2c58f32f90520322_27.png)

![](img/a9b3147697529d2d2c58f32f90520322_29.png)

### 技术需求
我们需要实现以下功能：
1.  **下载YouTube转录文本**：使用 `youtube-transcript-api` 直接从YouTube获取视频字幕。
2.  **语音转文本 (ASR)**：可选。如果视频没有现成字幕，则使用此功能（如Amazon Transcribe或Open Whisper）。
3.  **智能体与工具使用**：让LLM能够调用工具（如获取转录文本的API）。
4.  **文本转语音 (TTS)**：用于生成听力音频（如Amazon Polly）。
5.  **向量数据库**：用于存储和处理转录文本，以便进行检索增强生成。
6.  **前端界面**：使用Streamlit构建。
7.  **代码助手**：使用Amazon Q Developer辅助开发。
8.  **防护机制**：防止不当内容生成。

![](img/a9b3147697529d2d2c58f32f90520322_31.png)

### 技术不确定性
在项目中，我们可能会遇到以下挑战：
*   **目标语言知识**：如果不熟悉目标语言（如日语），如何评估输出质量？
*   **向量数据库选择**：最初考虑SQLite，但可能选择ChromaDB或PG Vector。
*   **TTS/ASR支持**：目标语言可能没有高质量的TTS或ASR服务。
*   **转录获取**：某些视频可能无法通过API直接获取转录。

**应对策略**：如果因技术限制无法继续，可以转而深入练习OPA，或投入时间完善你的自定义学习应用（对于追求红色徽章的学员）。

---

## 动手开发：环境设置与向量数据库

### 1. 项目初始化与代码获取
我们首先克隆项目仓库并设置开发环境。

![](img/a9b3147697529d2d2c58f32f90520322_33.png)

![](img/a9b3147697529d2d2c58f32f90520322_35.png)

```bash
git clone <repository-url>
cd language-learning-assistant
```

### 2. 使用Conda创建虚拟环境
为了避免依赖冲突，我们使用Conda创建独立的Python环境。

![](img/a9b3147697529d2d2c58f32f90520322_37.png)

![](img/a9b3147697529d2d2c58f32f90520322_39.png)

```bash
conda create -n listening-learning-app python=3.10
conda activate listening-learning-app
```

![](img/a9b3147697529d2d2c58f32f90520322_41.png)

### 3. 安装依赖
安装项目所需的Python包。

```bash
pip install -r backend/requirements.txt
```

![](img/a9b3147697529d2d2c58f32f90520322_43.png)

### 4. 向量数据库选择：ChromaDB
最初我们考虑使用SQLite作为向量存储，但为了更简单，我们选择**ChromaDB**，一个轻量级、内存式的向量数据库。

![](img/a9b3147697529d2d2c58f32f90520322_45.png)

![](img/a9b3147697529d2d2c58f32f90520322_46.png)

**将ChromaDB添加到依赖：**
在 `backend/requirements.txt` 中添加：
```
chromadb
boto3
```

![](img/a9b3147697529d2d2c58f32f90520322_48.png)

![](img/a9b3147697529d2d2c58f32f90520322_50.png)

![](img/a9b3147697529d2d2c58f32f90520322_52.png)

**ChromaDB基本用法：**
```python
import chromadb

![](img/a9b3147697529d2d2c58f32f90520322_54.png)

![](img/a9b3147697529d2d2c58f32f90520322_56.png)

# 初始化客户端
client = chromadb.Client()

![](img/a9b3147697529d2d2c58f32f90520322_58.png)

![](img/a9b3147697529d2d2c58f32f90520322_59.png)

![](img/a9b3147697529d2d2c58f32f90520322_61.png)

# 创建或获取一个集合（类似于表）
collection = client.create_collection(name="transcripts")

![](img/a9b3147697529d2d2c58f32f90520322_63.png)

# 添加文档（文本块）及其元数据
collection.add(
    documents=["这是一个文档内容", "这是另一个文档"],
    metadatas=[{"source": "video1"}, {"source": "video2"}],
    ids=["id1", "id2"]
)

![](img/a9b3147697529d2d2c58f32f90520322_65.png)

# 进行相似性搜索
results = collection.query(
    query_texts=["搜索查询"],
    n_results=2
)
```

我们遇到了一个环境问题：ChromaDB依赖的SQLite版本过低。解决方案是创建干净的Python虚拟环境，这通常能解决系统级库的冲突。

---

## 集成Amazon Q Developer

为了辅助开发，我们在VS Code中集成Amazon Q Developer扩展。
1.  在VS Code扩展商店搜索并安装“Amazon Q Developer”。
2.  登录你的AWS Builder ID进行认证。
3.  Q Developer可以提供代码补全、解释和问题解答，特别是在使用AWS相关服务时很有帮助。

---

![](img/a9b3147697529d2d2c58f32f90520322_67.png)

![](img/a9b3147697529d2d2c58f32f90520322_69.png)

![](img/a9b3147697529d2d2c58f32f90520322_70.png)

![](img/a9b3147697529d2d2c58f32f90520322_72.png)

## 构建应用模块

### 1. 基础聊天功能
我们首先实现一个与LLM（这里使用Amazon Nova Micro模型）对话的简单聊天后端。

![](img/a9b3147697529d2d2c58f32f90520322_74.png)

![](img/a9b3147697529d2d2c58f32f90520322_76.png)

**后端代码 (`backend/chat.py`) 核心：**
```python
import boto3
from botocore.config import Config

class BedrockChat:
    def __init__(self, model_id="amazon.nova-micro-v1:0"):
        self.bedrock = boto3.client("bedrock-runtime", region_name="us-east-1")
        self.model_id = model_id
        self.messages = []  # 存储对话历史

    def chat(self, user_message):
        # 将用户消息添加到历史
        self.messages.append({"role": "user", "content": user_message})

        # 调用Bedrock Converse API
        response = self.bedrock.converse(
            modelId=self.model_id,
            messages=self.messages
        )

        # 提取助手回复
        assistant_message = response['output']['message']['content'][0]['text']
        self.messages.append({"role": "assistant", "content": assistant_message})

        return assistant_message
```

**测试：**
运行 `python backend/chat.py`，可以快速测试与Nova Micro模型的对话。例如，让其生成一个JLPT N5风格的听力理解问题，响应速度非常快。

### 2. 前端集成 (Streamlit)
我们使用Streamlit构建一个多步骤的前端界面。

**前端结构 (`frontend/main.py`)：**
```python
import streamlit as st
from backend.chat import BedrockChat

![](img/a9b3147697529d2d2c58f32f90520322_78.png)

# 初始化聊天会话
if "chat_session" not in st.session_state:
    st.session_state.chat_session = BedrockChat()

# 侧边栏导航
stage = st.sidebar.radio(
    "选择阶段",
    ["聊天", "原始转录", "结构化数据", "RAG实现", "交互学习"]
)

if stage == "聊天":
    st.title("与Nova聊天")
    user_input = st.text_input("输入你的问题")
    if user_input:
        response = st.session_state.chat_session.chat(user_input)
        st.write(response)
# ... 其他阶段
```

![](img/a9b3147697529d2d2c58f32f90520322_80.png)

![](img/a9b3147697529d2d2c58f32f90520322_82.png)

**解决导入问题：**
在Streamlit中运行前端时，可能会遇到模块导入错误。这是因为Python路径问题。解决方案是在 `frontend/main.py` 开头添加路径设置：

![](img/a9b3147697529d2d2c58f32f90520322_84.png)

```python
import sys
import os
sys.path.append(os.path.join(os.path.dirname(__file__), '..'))
```

### 3. 获取YouTube转录文本
我们实现一个类，使用 `youtube-transcript-api` 下载指定视频的转录。

![](img/a9b3147697529d2d2c58f32f90520322_86.png)

**后端代码 (`backend/get_transcript.py`) 核心：**
```python
from youtube_transcript_api import YouTubeTranscriptApi

![](img/a9b3147697529d2d2c58f32f90520322_88.png)

class YouTubeTranscriptDownloader:
    def get_transcript(self, video_url):
        # 从URL提取视频ID
        video_id = video_url.split("v=")[-1]
        transcript_list = YouTubeTranscriptApi.get_transcript(video_id)
        # 将转录片段合并为完整文本
        full_text = " ".join([item['text'] for item in transcript_list])
        return full_text

    def save_transcript(self, transcript_text, video_id):
        filename = f"transcripts/{video_id}.txt"
        with open(filename, 'w', encoding='utf-8') as f:
            f.write(transcript_text)
```

**前端集成：**
在Streamlit的“原始转录”阶段，添加一个文本框用于输入YouTube URL，并调用上述类来下载和保存转录。

![](img/a9b3147697529d2d2c58f32f90520322_90.png)

### 4. 分析转录并生成练习
获取转录后，我们可以让LLM分析其结构，并基于此生成新的练习。

**示例提示词：**
```
你是一名日语语言测试专家。以下是JLPT N5听力理解测试的一个真实转录文本：
[粘贴转录文本 here]

请分析这个听力问题的格式和结构。然后，基于相同的格式，创建一个关于“男人和女人讨论周末计划”的新听力理解问题（包括介绍、对话片段、问题及四个选项）。全部使用日语。
```

![](img/a9b3147697529d2d2c58f32f90520322_92.png)

通过这种方式，我们利用“上下文学习”，让LLM在提示中学习示例的格式，并生成符合要求的新内容。

---

## 总结与后续

本节课中，我们一起学习了：
1.  **多模态AI应用**的概念与组成部分。
2.  如何规划一个结合**语音、文本、向量检索**的完整项目。
3.  使用 **ChromaDB** 作为向量数据库存储和检索文本。
4.  集成 **Amazon Bedrock** 和 **Nova Micro** 模型进行对话与内容生成。
5.  利用 **Streamlit** 快速构建交互式前端。
6.  使用 **YouTube Transcript API** 获取学习材料。
7.  通过 **上下文学习** 提示技术，引导LLM生成特定格式的内容。

![](img/a9b3147697529d2d2c58f32f90520322_94.png)

虽然我们在直播时间内没有完成所有功能（如RAG集成、TTS生成），但我们已经搭建了核心框架，并解决了环境配置、模块集成等常见难题。**在真实项目开发中，这种探索和解决问题过程是非常典型的。**

**后续步骤：**
*   完善RAG部分：将转录文本切块存入ChromaDB，并在聊天时进行检索增强。
*   集成文本转语音服务（如Amazon Polly），为生成的听力问题创建音频。
*   添加防护机制，过滤不当内容。
*   部署应用到云端。

![](img/a9b3147697529d2d2c58f32f90520322_96.png)

请记住，一致性比一次性完成所有功能更重要。即使每周只进步一点点，坚持到底就是胜利。祝大家编码愉快！