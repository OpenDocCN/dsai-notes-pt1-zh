# 003：提示与参数控制 🎛️

![](img/10ace67a3b7118c1bf47cd308b4e972f_0.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_2.png)

在本节课中，我们将学习如何使用文本和图像来提示多模态模型Gemini。我们将探索各种模型参数，了解如何选择它们，并理解它们对模型创造性和一致性的影响。

## 环境设置与认证 🔧

为了运行本教程的代码，我们需要先进行一些设置。首先，我们需要导入必要的工具并进行身份认证，以便从我们的环境调用Gemini API。

以下是设置步骤：
1.  导入工具并进行身份认证。
2.  设置项目ID和区域（例如 `us-central1`）。
3.  导入Vertex AI SDK，这是一个用于与Gemini交互的Python工具包。
4.  初始化Vertex AI SDK，指定项目、区域和凭据。

```python
# 示例：初始化Vertex AI
from google.cloud import aiplatform
aiplatform.init(project=PROJECT_ID, location=LOCATION)
```

## 选择并调用模型 🤖

设置好环境后，我们需要选择要使用的Gemini模型并调用它。

以下是具体步骤：
1.  从SDK中导入生成模型。
2.  指定模型版本，例如 `gemini-1.0-pro-vision`。
3.  使用辅助函数或直接调用 `model.generate_content()` 来获取模型响应。

![](img/10ace67a3b7118c1bf47cd308b4e972f_4.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_6.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_8.png)

```python
# 示例：导入并指定模型
from vertexai.preview.generative_models import GenerativeModel
model = GenerativeModel(“gemini-1.0-pro-vision”)
```

![](img/10ace67a3b7118c1bf47cd308b4e972f_10.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_12.png)

## 基础文本提示 📝

![](img/10ace67a3b7118c1bf47cd308b4e972f_14.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_16.png)

让我们从一个简单的文本提示开始，了解Gemini如何响应。

![](img/10ace67a3b7118c1bf47cd308b4e972f_18.png)

我们可以问：“什么是多模态模型？”。调用API后，模型会返回一个解释多模态模型的文本响应。你可以尝试更改提示，观察模型的不同回答。

![](img/10ace67a3b7118c1bf47cd308b4e972f_20.png)

## 流式传输响应 ⚡

为了提升交互体验，特别是构建实时应用时，我们可以使用流式传输功能。

![](img/10ace67a3b7118c1bf47cd308b4e972f_22.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_24.png)

流式传输允许我们在模型生成完整响应之前就开始处理文本片段。这通过将 `stream` 参数设置为 `True` 来实现。

![](img/10ace67a3b7118c1bf47cd308b4e972f_26.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_27.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_29.png)

```python
# 示例：使用流式传输
response = model.generate_content(prompt, stream=True)
for chunk in response:
    print(chunk.text)
```

![](img/10ace67a3b7118c1bf47cd308b4e972f_31.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_33.png)

## 结合图像与文本提示 🖼️➕📝

![](img/10ace67a3b7118c1bf47cd308b4e972f_35.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_37.png)

上一节我们介绍了纯文本提示，本节中我们来看看如何结合图像和文本来提示Gemini模型。

![](img/10ace67a3b7118c1bf47cd308b4e972f_39.png)

首先，需要从SDK导入处理图像所需的类。然后，我们可以加载一张本地图像，并组合图像和一个文本提示（例如“描述这张图片”）发送给模型。

以下是关键步骤：
1.  导入 `Image` 和 `Part` 类。
2.  加载本地图像文件。
3.  将图像和文本提示组合成 `content`。
4.  调用模型并获取响应。

模型不仅能描述图像内容，还能根据提示进行推理。例如，询问“这个人的可能职业是什么？”，模型会根据图像中的工具（如锤子、电钻）推断出可能的职业（如木匠、建筑工人）。

## 处理视频输入 🎥

除了静态图像，Gemini还能处理视频输入。在这个例子中，我们将从云存储中加载一个视频，并向模型提出几个需要观看完整视频才能回答的问题。

以下是处理视频的步骤：
1.  从云存储桶获取视频的URI（统一资源标识符）。
2.  准备一个包含多个问题的复杂提示。
3.  将视频URI和提示组合后发送给模型。
4.  模型会分析视频内容并逐一回答问题。

![](img/10ace67a3b7118c1bf47cd308b4e972f_41.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_43.png)

## 理解模型参数 🎚️

![](img/10ace67a3b7118c1bf47cd308b4e972f_45.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_47.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_49.png)

在尝试调整模型参数之前，我们先快速了解几个关键参数，它们就像控制模型输出“风味”的旋钮。

![](img/10ace67a3b7118c1bf47cd308b4e972f_51.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_53.png)

![](img/10ace67a3b7118c1bf47cd308b4e972f_55.png)

以下是核心参数及其作用：
*   **温度 (Temperature)**：控制输出的随机性。公式可理解为 `P(word) ∝ exp(logit / T)`，其中T是温度。
    *   **低温度（如0.2）**：输出更确定、更集中。
    *   **高温度（如0.8）**：输出更具创造性、更多样。
*   **Top-k**：限制模型在每个步骤中只考虑概率最高的前k个候选词。
*   **Top-p**（核采样）：从累积概率超过阈值p的最小候选词集合中采样。
*   **最大输出标记数 (Max Output Tokens)**：限制响应长度的最大值。
*   **停止序列 (Stop Sequences)**：指定一个字符串列表，遇到即停止生成。

## 实践：调整参数以控制输出 🔄

现在，让我们通过代码实践，看看调整这些参数如何影响Gemini对同一张图片的描述。

首先，我们使用默认参数让模型描述一张图片。然后，我们创建 `GenerationConfig` 对象来定制参数。

以下是调整参数的示例：
1.  **降低随机性**：将温度设为 `0.0`，top-k设为 `1`。多次运行会得到非常一致的输出。
2.  **提高创造性**：将温度设为 `1.0`，top-k设为 `40`。输出会更冗长且富有创意。
3.  **使用Top-p**：在高温、高top-k基础上，设置较低的top-p（如`0.01`），可以抵消部分随机性，使输出回归简洁。
4.  **限制输出长度**：设置 `max_output_tokens=10`，会得到一个非常简短的响应。
5.  **使用停止序列**：设置 `stop_sequences=[“熊猫”]`，则响应会在“熊猫”一词出现前被截断。

```python
# 示例：设置生成配置
from vertexai.preview.generative_models import GenerationConfig
generation_config = GenerationConfig(
    temperature=0.2,
    top_p=0.8,
    top_k=40,
    max_output_tokens=256,
    stop_sequences=[“熊猫”]
)
response = model.generate_content(content, generation_config=generation_config)
```

## 总结 📚

本节课中，我们一起学习了如何对Gemini多模态模型进行提示和参数控制。

我们首先完成了环境设置和基础模型调用。接着，探索了如何结合图像、文本乃至视频进行多模态提示。最后，我们深入了解了温度、Top-k、Top-p、最大输出标记数和停止序列等关键参数，并通过实践看到了它们如何显著影响模型的输出风格、创造性和一致性。

![](img/10ace67a3b7118c1bf47cd308b4e972f_57.png)

记住，为特定用例找到最佳参数组合需要反复试验。对于需要确定性的任务（如分类），建议使用较低的随机性参数；对于需要创意的任务（如故事生成），则可以适当调高这些参数。