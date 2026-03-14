# 35：使用Darya Petrashka进行假名书写练习 🎌

![](img/ec42aceaa5cce0b1e0b76ba332614b50_1.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_3.png)

在本节课中，我们将学习如何构建一个用于练习日语假名（平假名和片假名）书写的交互式Web应用。我们将使用Streamlit构建前端界面，并利用一个专门的光学字符识别（OCR）模型来评估用户的书写。最后，我们会将整个应用部署到AWS云平台。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_5.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_6.png)

---

![](img/ec42aceaa5cce0b1e0b76ba332614b50_8.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_10.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_11.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_13.png)

## 应用概览与架构

首先，我们来了解一下这个应用的主要功能和整体架构。该应用旨在帮助用户学习和练习日语假名，包含学习、书写练习和读音练习三个主要页面。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_15.png)

以下是应用部署到AWS后的架构图：

![](img/ec42aceaa5cce0b1e0b76ba332614b50_17.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_1.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_19.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_21.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_23.png)

核心架构组件包括：
*   **Streamlit应用**：作为前端和后端逻辑的核心。
*   **AWS Fargate**：以无服务器方式运行Docker容器，托管Streamlit应用。
*   **应用负载均衡器（ALB）**：作为应用的入口点，分发流量并提供稳定的DNS地址。
*   **自动扩缩容**：根据CPU使用率自动调整应用容量以应对流量变化。
*   **VPC（虚拟私有云）**：将整个应用环境隔离在安全的私有网络中。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_25.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_27.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_29.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_31.png)

接下来，我们将从本地运行开始，逐步深入代码。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_33.png)

---

![](img/ec42aceaa5cce0b1e0b76ba332614b50_35.png)

## 本地环境设置与运行

![](img/ec42aceaa5cce0b1e0b76ba332614b50_37.png)

在部署到云端之前，我们首先需要在本地设置环境并运行应用。以下是具体步骤。

### 克隆代码库与安装依赖

首先，我们需要将项目代码克隆到本地，并创建一个独立的Python环境来安装所有必要的依赖包。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_39.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_41.png)

```bash
# 克隆代码仓库
git clone <repository-url>
cd <repository-directory>

# 创建并激活虚拟环境（以.venv为例）
python -m venv .venv
# 在Windows上激活
.venv\Scripts\activate
# 在Mac/Linux上激活
source .venv/bin/activate

# 安装依赖包
pip install -r requirements.txt
```

### 预加载OCR模型

我们的应用使用一个名为`manga-ocr`的专用模型来识别手写的假名。为了提升应用启动速度，我们提前在构建阶段下载该模型。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_43.png)

```bash
# 运行预加载脚本
python scripts/preload_model.py
```
这个脚本会从Hugging Face Hub下载模型，并将其保存在本地，这样在应用运行时就可以直接使用，无需每次加载。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_45.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_47.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_49.png)

### 启动Streamlit应用

依赖安装和模型预加载完成后，就可以启动应用了。

```bash
streamlit run app.py
```
执行上述命令后，Streamlit服务会在本地启动，并在浏览器中打开应用界面。

---

![](img/ec42aceaa5cce0b1e0b76ba332614b50_51.png)

## 应用代码结构解析

![](img/ec42aceaa5cce0b1e0b76ba332614b50_53.png)

现在，让我们深入查看应用的代码结构，理解各个页面是如何工作的。

### 多页面配置 (`app.py`)

Streamlit支持多页面应用。`app.py` 是应用的入口点，它定义了页面的结构和路径。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_55.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_57.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_59.png)

```python
import streamlit as st

# 配置页面
st.set_page_config(page_title="Kana学习助手", layout="wide")

# 定义页面
pages = {
    "学习假名": "pages/learn_kana.py",
    "假名书写练习": "pages/practice_writing.py",
    "假名读音练习": "pages/practice_reading.py",
}

# 显示侧边栏导航
st.sidebar.title("导航")
selection = st.sidebar.radio("前往", list(pages.keys()))

# 运行选中的页面
page = pages[selection]
with open(page) as f:
    exec(f.read())
```
这个文件主要负责导航，将不同的功能模块组织成独立的页面。

### 学习页面 (`pages/learn_kana.py`)

学习页面是应用中最简单的部分，主要用于展示平假名或片假名的图表。

```python
import streamlit as st

![](img/ec42aceaa5cce0b1e0b76ba332614b50_61.png)

st.title("🎌 学习假名")
st.subheader("选择你要学习的假名表")

# 用户选择学习模式：平假名或片假名
study_mode = st.radio("学习模式", ["平假名 (Hiragana)", "片假名 (Katakana)"])

# 根据选择显示对应的图片
if "平假名" in study_mode:
    st.image("images/hiragana_chart.png", caption="平假名图表")
else:
    st.image("images/katakana_chart.png", caption="片假名图表")

st.divider()
st.caption("祝你学习顺利！")
```
这个页面通过一个单选按钮让用户选择学习内容，并动态显示对应的假名表图片。

### 书写练习页面 (`pages/practice_writing.py`)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_63.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_64.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_66.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_68.png)

书写练习页面是应用的核心，它允许用户绘制假名，并使用OCR模型进行验证。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_69.png)

上一节我们介绍了静态的学习页面，本节中我们来看看交互性更强的书写练习页面。这个页面包含绘图、模型调用和结果验证等多个部分。

以下是该页面的关键功能模块：

1.  **模式选择与字符随机化**：用户可以选择练习平假名或片假名，并随机获取一个字符进行练习。
2.  **绘图画布**：使用`streamlit-drawable-canvas`库提供绘图功能，让用户能够绘制假名。
3.  **OCR识别与验证**：将用户绘制的图像传递给`manga-ocr`模型进行识别，并将识别结果与目标字符进行比对。

```python
import streamlit as st
from streamlit_drawable_canvas import st_canvas
from PIL import Image
import torch
from transformers import MangaOcr

# 初始化OCR模型（使用缓存避免重复加载）
@st.cache_resource
def load_ocr_model():
    return MangaOcr()

![](img/ec42aceaa5cce0b1e0b76ba332614b50_71.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_72.png)

model = load_ocr_model()

# 页面标题和模式选择
st.title("✍️ 假名书写练习")
mode = st.radio("选择假名表", ["平假名", "片假名"])

# 假名字典（字符->罗马音）
kana_dict = {"あ": "a", "い": "i", "ア": "a", "イ": "i"} # 示例字典

![](img/ec42aceaa5cce0b1e0b76ba332614b50_74.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_76.png)

# 随机选择一个目标字符
if 'target_kana' not in st.session_state or st.button("换一个字"):
    # 从对应模式的字典中随机选择
    st.session_state.target_kana = random.choice(list(kana_dict.keys()))

![](img/ec42aceaa5cce0b1e0b76ba332614b50_78.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_79.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_80.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_82.png)

st.subheader(f"请写出字符: {st.session_state.target_kana}")

![](img/ec42aceaa5cce0b1e0b76ba332614b50_84.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_86.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_87.png)

# 绘图画布
canvas_result = st_canvas(
    fill_color="rgba(255, 255, 255, 0)",
    stroke_width=3,
    stroke_color="#000",
    background_color="#FFF",
    height=200,
    width=200,
    drawing_mode="freedraw",
    key="canvas",
)

# 提交按钮和验证逻辑
if st.button("提交") and canvas_result.image_data is not None:
    # 将画布图像转换为PIL Image
    img = Image.fromarray(canvas_result.image_data.astype('uint8'), 'RGBA')
    # 转换为模型需要的RGB格式
    img = img.convert('RGB')
    
    # 使用OCR模型识别
    try:
        recognized_text = model(img)
        # 简单后处理：取第一个非空格字符
        recognized_char = recognized_text.strip()[0]
    except:
        recognized_char = ""
    
    # 验证结果
    if recognized_char == st.session_state.target_kana:
        st.balloons()
        st.success(f"正确！你写的是: {recognized_char}")
    else:
        st.warning(f"再试试看。你写的是 '{recognized_char}'， 目标字符是 '{st.session_state.target_kana}'。")
```
在这个页面中，我们使用了`st.session_state`来在用户交互过程中保持状态（如当前目标字符），并使用`st.cache_resource`来缓存OCR模型，避免每次调用都重新加载，从而提升性能。

### 读音练习页面 (`pages/practice_reading.py`)

读音练习页面与书写练习页面类似，但方向相反：它显示一个假名，要求用户输入其对应的罗马字读音。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_89.png)

本节我们继续来看第三个主要页面——读音练习。它的逻辑与书写练习对称，但考察的是用户从字符到发音的映射能力。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_91.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_92.png)

以下是该页面的核心逻辑：

1.  **显示目标假名**：随机显示一个平假名或片假名。
2.  **接收用户输入**：提供一个文本框让用户输入其认为的罗马音读音。
3.  **答案验证**：将用户输入与字典中存储的正确读音进行比对。

```python
import streamlit as st
import random

st.title("🔤 假名读音练习")
mode = st.radio("选择假名表", ["平假名", "片假名"])

# 假名字典（字符->罗马音）
kana_dict = {"あ": "a", "い": "i", "う": "u", "ア": "a", "イ": "i", "ウ": "u"}

# 根据模式过滤字典
current_dict = {k: v for k, v in kana_dict.items() if (mode=="平假名" and k in "あいうえお") or (mode=="片假名" and k in "アイウエオ")}

# 随机选择目标字符
if 'target_kana_reading' not in st.session_state or st.button("换一个字"):
    st.session_state.target_kana_reading = random.choice(list(current_dict.keys()))

st.subheader(f"这个假名怎么读？")
st.markdown(f"## {st.session_state.target_kana_reading}")

# 用户输入答案
user_answer = st.text_input("请输入对应的罗马音:").lower().strip()

if user_answer:
    correct_reading = current_dict.get(st.session_state.target_kana_reading, "")
    if user_answer == correct_reading:
        st.balloons()
        st.success(f"正确！读音是: {correct_reading}")
    else:
        st.warning(f"不对哦。正确读音是: {correct_reading}")
```
这个页面结构清晰，通过字典查询进行验证，为用户提供了即时的学习反馈。

---

## 容器化与Dockerfile

为了将应用部署到云平台，我们需要将其容器化。Dockerfile定义了构建应用镜像的步骤。

```dockerfile
# 使用官方Python镜像作为基础
FROM python:3.9-slim

# 设置工作目录
WORKDIR /app

# 复制依赖文件和代码
COPY requirements.txt .
COPY . .

# 安装系统依赖和Python包
RUN apt-get update && apt-get install -y \
    gcc \
    && rm -rf /var/lib/apt/lists/*
RUN pip install --no-cache-dir -r requirements.txt

# 预加载模型（在构建时完成，避免运行时下载）
RUN python scripts/preload_model.py

# 暴露Streamlit默认端口
EXPOSE 8501

# 启动命令
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```
这个Dockerfile确保了应用及其所有依赖（包括预下载的OCR模型）都被打包到一个可移植的容器中。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_94.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_95.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_96.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_97.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_99.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_100.png)

---

## 使用AWS CDK部署到云端

最后，我们将应用部署到AWS。我们使用AWS CDK（Cloud Development Kit）来定义基础设施即代码（IaC），这比手动编写CloudFormation模板更高效。

### CDK堆栈定义 (`app.py`)

CDK允许我们使用Python来定义云资源。以下堆栈创建了VPC、ECS集群、Fargate服务、负载均衡器等所有必要资源。

```python
from aws_cdk import (
    Stack,
    aws_ec2 as ec2,
    aws_ecs as ecs,
    aws_ecr_assets as ecr_assets,
    aws_elasticloadbalancingv2 as elbv2,
    aws_ecs_patterns as ecs_patterns,
    RemovalPolicy,
)
from constructs import Construct

class KanaAppStack(Stack):
    def __init__(self, scope: Construct, construct_id: str, **kwargs) -> None:
        super().__init__(scope, construct_id, **kwargs)

        # 1. 创建VPC
        vpc = ec2.Vpc(self, "KanaAppVpc", max_azs=2)

        # 2. 创建ECS集群
        cluster = ecs.Cluster(self, "KanaAppCluster", vpc=vpc)

        # 3. 从本地Dockerfile创建镜像并推送到ECR
        image_asset = ecr_assets.DockerImageAsset(self, "KanaAppImage",
            directory=".",
            file="Dockerfile"
        )

        # 4. 在Fargate上创建负载均衡服务
        fargate_service = ecs_patterns.ApplicationLoadBalancedFargateService(
            self, "KanaAppFargateService",
            cluster=cluster,
            cpu=256,          # 0.25 vCPU
            memory_limit_mib=512, # 512 MB
            desired_count=1,
            task_image_options=ecs_patterns.ApplicationLoadBalancedTaskImageOptions(
                image=ecs.ContainerImage.from_docker_image_asset(image_asset),
                container_port=8501,
            ),
            public_load_balancer=True
        )

        # 5. 配置自动扩缩容
        scaling = fargate_service.service.auto_scale_task_count(max_capacity=5)
        scaling.scale_on_cpu_utilization("CpuScaling",
            target_utilization_percent=70,
            scale_in_cooldown=Duration.seconds(60),
            scale_out_cooldown=Duration.seconds(60)
        )

        # 输出负载均衡器的URL
        self.url_output = CfnOutput(self, "LoadBalancerUrl",
            value=f"http://{fargate_service.load_balancer.load_balancer_dns_name}"
        )
```
这个CDK堆栈清晰地描述了整个应用的基础设施，从网络到计算再到负载均衡，实现了自动化部署。

### 部署与销毁命令

![](img/ec42aceaa5cce0b1e0b76ba332614b50_102.png)

使用CDK CLI，我们可以轻松地部署和销毁整个应用栈。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_104.png)

```bash
# 首次部署前，在目标AWS账户和区域引导CDK环境
cdk bootstrap

# 合成CloudFormation模板
cdk synth

![](img/ec42aceaa5cce0b1e0b76ba332614b50_106.png)

# 部署应用到AWS
cdk deploy

![](img/ec42aceaa5cce0b1e0b76ba332614b50_108.png)

# 部署完成后，销毁所有资源以避免产生费用
cdk destroy
```
`cdk deploy` 命令会触发一个构建和部署流水线，包括构建Docker镜像、推送至ECR以及创建所有定义的AWS资源。整个过程可能需要10-20分钟。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_110.png)

---

![](img/ec42aceaa5cce0b1e0b76ba332614b50_112.png)

![](img/ec42aceaa5cce0b1e0b76ba332614b50_113.png)

## 总结与扩展思路

![](img/ec42aceaa5cce0b1e0b76ba332614b50_115.png)

本节课中我们一起学习了如何构建一个完整的生成式AI应用——日语假名学习助手。我们从零开始，涵盖了以下关键步骤：

![](img/ec42aceaa5cce0b1e0b76ba332614b50_117.png)

1.  **应用开发**：使用Streamlit构建交互式Web界面，实现假名学习、书写练习和读音练习三大功能。
2.  **AI集成**：集成`manga-ocr`这个专用的视觉编码器-解码器模型，用于识别手写假名，这是应用的核心智能。
3.  **容器化**：通过Docker将应用及其环境打包，确保一致性。
4.  **云部署**：利用AWS CDK框架，以代码形式定义并自动化部署了整个云基础设施（VPC、Fargate、ALB等）。

这个项目展示了如何将机器学习模型、Web框架和云原生技术结合，快速打造一个实用、可分享的学习工具。你可以在此基础上进行扩展，例如：
*   增加汉字（Kanji）的学习和练习模块。
*   支持更多语言（如韩语谚文）的书写识别。
*   实现更复杂的练习模式，如单词拼写或句子听写。
*   探索使用更通用的多模态大模型（如GPT-4V）进行字符识别，并比较其与专用模型的性能、成本和速度差异。

![](img/ec42aceaa5cce0b1e0b76ba332614b50_119.png)

希望本教程能为你构建自己的生成式AI应用提供清晰的路径和灵感。