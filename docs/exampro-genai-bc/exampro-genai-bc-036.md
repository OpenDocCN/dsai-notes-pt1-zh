# 36：Kana书写应用 - Andrews的重构实现

![](img/8bf79ee38a3d52c6191c4671faccca1d_1.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_3.png)

## 概述

在本节课中，我们将学习如何将Dara的Kana书写应用整合到我们的语言学习门户系统中。我们将重构一个现有的Streamlit应用，使其能够接收来自门户系统的单词组数据，并构建一个完整的句子书写练习应用。课程将涵盖应用设计、状态管理、API集成以及调试技巧。

## 应用设计与技术栈

上一节我们介绍了整合现有应用的目标，本节中我们来看看具体的技术实现方案。

我们将使用Streamlit作为前端框架，因为它能快速构建交互式应用。后端数据来自我们已有的语言学习门户API。核心功能将依赖大型语言模型来生成练习句子，并使用视觉模型或OCR技术来评估用户的手写输入。

以下是应用的核心技术组件：
*   **前端框架**：`Streamlit`
*   **后端API**：语言学习门户的`/api/groups/<id>/words/raw`端点
*   **AI服务**：用于生成句子的`LLM API`（如OpenAI）和用于图像识别的`视觉模型`或`Manga OCR`

## 应用流程与状态设计

理解了技术栈后，我们需要规划用户与应用的交互流程。我们将应用划分为三个主要状态，确保用户体验清晰流畅。

应用包含三个核心状态：
1.  **初始化状态**：应用启动，从URL参数获取`group_id`，并调用API加载对应的单词数据。
2.  **练习状态**：展示一个英文句子，用户需要书写对应的目标语言句子并上传图片。
3.  **复习状态**：展示系统对用户上传图片的识别结果、翻译和评分，并提供“下一题”按钮。

状态之间的转换由用户操作触发，例如点击“生成句子”按钮会从初始化状态进入练习状态。

![](img/8bf79ee38a3d52c6191c4671faccca1d_5.png)

## 详细功能规格

为了更清晰地指导开发，我们将上述流程转化为具体的功能规格说明。

![](img/8bf79ee38a3d52c6191c4671faccca1d_7.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_9.png)

### 初始化步骤
当应用首次加载时，需要执行以下操作：
*   从查询字符串中获取 `group_id` 参数。
*   向 `http://localhost:5000/api/groups/<group_id>/words/raw` 发送GET请求。
*   该端点将返回一个包含日语单词及其英文翻译的JSON结构。
*   将此单词集合存储在应用的内存中。

![](img/8bf79ee38a3d52c6191c4671faccca1d_11.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_13.png)

### 页面状态与用户行为
页面状态描述了单页面应用在不同阶段应有的行为。

**场景：用户首次启动应用**
*   **前提**：用户通过语言门户的链接启动应用，URL中包含 `session_id` 和 `group_id`。
*   **操作**：用户只能看到一个名为“生成句子”的按钮。
*   **结果**：当用户按下该按钮，应用将使用句子生成器LLM创建一个句子，并将状态切换到“练习状态”。

![](img/8bf79ee38a3d52c6191c4671faccca1d_15.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_17.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_19.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_21.png)

**场景：用户处于练习状态**
*   **前提**：应用处于练习状态。
*   **操作**：用户会看到一个英文句子、一个图片上传区域和一个“提交评审”按钮。
*   **结果**：当用户上传图片并点击“提交评审”按钮，图片将被传递给评分系统，应用状态将切换到“复习状态”。

![](img/8bf79ee38a3d52c6191c4671faccca1d_23.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_25.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_27.png)

**场景：用户处于复习状态**
*   **前提**：应用处于复习状态。
*   **操作**：用户仍然看到英文句子，但上传区域消失。取而代之的是评分系统的输出结果，以及一个“下一题”按钮。
*   **结果**：当用户点击“下一题”按钮，应用将生成一个新问题，并将用户带回“练习状态”。

![](img/8bf79ee38a3d52c6191c4671faccca1d_29.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_31.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_33.png)

### 句子生成器（LLM提示）
我们需要一个LLM来根据所学单词生成练习句子。提示词如下：
```
请使用以下单词：[插入目标单词]。
生成一个简单的句子，语法应限定在JLPT N5级别。
你可以使用以下词汇来构造句子：
- 简单名词：例如 书、车、拉面、寿司。
- 简单动词：例如 喝、吃、见。
- 简单时间词：例如 早上、明天、今天、昨天。
请确保句子简洁，适合初学者练习书写。
```

![](img/8bf79ee38a3d52c6191c4671faccca1d_35.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_37.png)

### 评分系统
评分系统负责处理用户上传的图片，并给出反馈。其工作流程如下：
1.  **转录**：使用 `Manga OCR` 将图片中的文字转录出来。
2.  **翻译**：使用 `LLM` 将转录出的文字翻译成英文。
3.  **评分**：使用另一个 `LLM`，对比用户的句子与目标句子，从准确性和书写质量方面进行评分。评分采用“S级”评分制（例如S， A， B， C），并附上改进建议。
4.  **返回数据**：将转录文本、翻译文本、评分等级和评语返回给前端应用。

## 代码实现与调试

![](img/8bf79ee38a3d52c6191c4671faccca1d_39.png)

基于以上设计，我们可以开始着手实现Streamlit应用。在开发过程中，有效的调试至关重要。

![](img/8bf79ee38a3d52c6191c4671faccca1d_41.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_43.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_45.png)

我们首先构建一个基本的Streamlit应用骨架，包含状态管理。关键步骤包括从查询参数获取`group_id`，并调用后端API。

![](img/8bf79ee38a3d52c6191c4671faccca1d_47.png)

```python
import streamlit as st
import requests
import logging

![](img/8bf79ee38a3d52c6191c4671faccca1d_49.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_51.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_53.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_55.png)

# 配置日志记录，便于调试
logging.basicConfig(level=logging.DEBUG, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
logger = logging.getLogger(__name__)

# 应用状态常量
class AppState:
    INITIAL = "initial"
    PRACTICE = "practice"
    REVIEW = "review"

![](img/8bf79ee38a3d52c6191c4671faccca1d_57.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_59.png)

# 初始化会话状态
if 'app_state' not in st.session_state:
    st.session_state.app_state = AppState.INITIAL
if 'vocabulary' not in st.session_state:
    st.session_state.vocabulary = []
if 'current_sentence' not in st.session_state:
    st.session_state.current_sentence = ""

![](img/8bf79ee38a3d52c6191c4671faccca1d_61.png)

def load_vocabulary():
    """从后端API加载单词数据"""
    # 从Streamlit查询参数中获取group_id
    query_params = st.query_params
    group_id = query_params.get("group_id", [None])[0]

    if not group_id:
        st.error("未提供group_id查询参数。")
        return

    logger.debug(f"获取到的 group_id: {group_id}")
    url = f"http://localhost:5000/api/groups/{group_id}/words/raw"
    logger.debug(f"请求URL: {url}")

    try:
        response = requests.get(url)
        logger.debug(f"API响应状态码: {response.status_code}")
        logger.debug(f"API响应内容: {response.text}")

        if response.status_code == 200:
            data = response.json()
            # 假设返回的数据结构是一个单词列表
            st.session_state.vocabulary = data
            logger.info(f"成功加载 {len(data)} 个单词。")
        else:
            st.error(f"从API加载数据失败，状态码：{response.status_code}")
    except Exception as e:
        st.error(f"请求发生错误：{e}")
        logger.exception("加载词汇时发生异常")

![](img/8bf79ee38a3d52c6191c4671faccca1d_63.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_65.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_67.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_69.png)

# 根据应用状态渲染不同界面
if st.session_state.app_state == AppState.INITIAL:
    st.title("书写练习")
    if st.button("加载词汇并生成句子"):
        load_vocabulary()
        if st.session_state.vocabulary:
            # 这里后续调用LLM生成句子
            st.session_state.current_sentence = "这是一个示例句子。（待实现LLM调用）"
            st.session_state.app_state = AppState.PRACTICE
            st.rerun()
elif st.session_state.app_state == AppState.PRACTICE:
    st.title("练习")
    st.write(f"请书写以下句子的翻译：**{st.session_state.current_sentence}**")
    uploaded_image = st.file_uploader("上传你书写的图片", type=['png', 'jpg', 'jpeg'])
    if st.button("提交评审") and uploaded_image is not None:
        # 这里后续调用评分系统
        st.session_state.app_state = AppState.REVIEW
        st.rerun()
elif st.session_state.app_state == AppState.REVIEW:
    st.title("评审结果")
    st.write("这是你的句子：（待实现OCR和评分）")
    st.write("评分：S （待实现）")
    st.write("评语：书写工整，接近目标。（待实现）")
    if st.button("下一题"):
        # 重新生成句子
        st.session_state.app_state = AppState.PRACTICE
        st.rerun()
```

![](img/8bf79ee38a3d52c6191c4671faccca1d_71.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_73.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_75.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_76.png)

在开发过程中，我们遇到了日志输出问题。默认的`print`语句在Streamlit的某些运行方式下可能不可见。为了解决这个问题，我们配置了Python的`logging`模块，将日志输出到文件和控制台，这是调试Streamlit应用的有效方法。

![](img/8bf79ee38a3d52c6191c4671faccca1d_78.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_80.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_82.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_84.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_86.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_88.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_90.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_91.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_93.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_95.png)

## 集成与测试

![](img/8bf79ee38a3d52c6191c4671faccca1d_97.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_99.png)

完成基础应用搭建后，我们需要将其集成到语言学习门户中，并进行测试。

![](img/8bf79ee38a3d52c6191c4671faccca1d_101.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_103.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_105.png)

首先，我们需要在门户的`study_activities`种子数据中添加这个新应用，并指定其运行端口（例如8081）。然后，确保Streamlit应用通过`streamlit run app.py --server.port 8081`命令在指定端口启动。

![](img/8bf79ee38a3d52c6191c4671faccca1d_107.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_109.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_111.png)

在集成测试中，我们发现API端点路径存在不一致的问题。最初的应用代码请求的路径是`/api/groups/{id}/raw`，而实际后端路径是`/api/groups/{id}/words/raw`。通过检查日志中的404错误和响应URL，我们定位并修正了这个问题，确保了前后端能成功通信。

![](img/8bf79ee38a3d52c6191c4671faccca1d_113.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_115.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_117.png)

## 总结

![](img/8bf79ee38a3d52c6191c4671faccca1d_119.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_121.png)

![](img/8bf79ee38a3d52c6191c4671faccca1d_123.png)

本节课中我们一起学习了如何将一个独立的Streamlit应用重构并整合到现有的语言学习平台中。我们从分析原有项目开始，设计了包含初始化、练习和复习三个状态的应用流程，并详细规划了每个步骤的技术要求。通过实现一个基础版本，我们实践了Streamlit的状态管理、API调用集成以及使用`logging`进行有效调试的方法。最后，我们完成了应用在门户中的基本集成，并解决了API路径不一致的问题，为后续实现句子生成、图像识别和评分等高级功能打下了坚实基础。