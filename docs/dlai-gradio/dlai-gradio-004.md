# 004：构建图像生成应用 🖼️

![](img/3debe5a7620fb7530158b03de96b66b4_0.png)

![](img/3debe5a7620fb7530158b03de96b66b4_2.png)

在本节课中，我们将学习如何使用开源的文本到图像模型构建一个图像生成应用。该应用接收一段文字描述，并据此生成相应的图像。我们将从基础应用开始，逐步引入更复杂的参数控制，并最终使用Gradio Blocks构建一个布局更清晰、功能更强大的用户界面。

![](img/3debe5a7620fb7530158b03de96b66b4_3.png)

## 概述：从文本到图像

上一节我们介绍了如何使用Gradio构建图像描述应用。本节中，我们将构建一个功能相反的应用：一个**文本到图像**的生成器。我们将使用一个名为**Stable Diffusion**的开源扩散模型，通过API连接到服务器上的模型来生成图像。

![](img/3debe5a7620fb7530158b03de96b66b4_5.png)

![](img/3debe5a7620fb7530158b03de96b66b4_7.png)

首先，我们需要设置API密钥以连接到模型服务。

![](img/3debe5a7620fb7530158b03de96b66b4_9.png)

![](img/3debe5a7620fb7530158b03de96b66b4_11.png)

```python
import os
os.environ[‘API_KEY‘] = ‘your_api_key_here‘
```

## 核心生成函数

与上节课的“图像到文本”端点不同，本节课我们使用“文本到图像”端点。模型训练的目标是相反的：它学习将一段文字描述与对应的图像关联起来，从而能够根据新的描述生成图像。

以下是调用API的核心函数 `generate_completion`：

![](img/3debe5a7620fb7530158b03de96b66b4_13.png)

```python
import requests

def generate_completion(prompt, api_url, headers):
    """
    向文本到图像API端点发送请求并获取生成的图像。
    """
    payload = {
        "inputs": prompt
    }
    response = requests.post(api_url, headers=headers, json=payload)
    # 假设API返回图像的字节数据或URL
    image_data = response.content
    return image_data
```

我们可以测试这个函数。输入一段描述，例如“一只在公园里的卡通狗”，函数会返回一张生成的图像。这表明我们的基础生成逻辑是有效的。

![](img/3debe5a7620fb7530158b03de96b66b4_15.png)

![](img/3debe5a7620fb7530158b03de96b66b4_16.png)

![](img/3debe5a7620fb7530158b03de96b66b4_17.png)

![](img/3debe5a7620fb7530158b03de96b66b4_18.png)

## 构建基础Gradio应用

![](img/3debe5a7620fb7530158b03de96b66b4_19.png)

![](img/3debe5a7620fb7530158b03de96b66b4_20.png)

![](img/3debe5a7620fb7530158b03de96b66b4_22.png)

现在，让我们将这个生成函数封装到一个Gradio应用中。这与之前构建的应用结构相似，但输入和输出组件互换了位置。

以下是构建基础应用的代码：

![](img/3debe5a7620fb7530158b03de96b66b4_24.png)

![](img/3debe5a7620fb7530158b03de96b66b4_25.png)

![](img/3debe5a7620fb7530158b03de96b66b4_26.png)

![](img/3debe5a7620fb7530158b03de96b66b4_27.png)

```python
import gradio as gr

def generate_image(prompt):
    # 此处应调用上述的 generate_completion 函数
    # 为简化示例，我们假设它返回一个图像文件路径或PIL图像对象
    image = generate_completion(prompt, API_URL, HEADERS)
    return image

# 创建Gradio界面
demo = gr.Interface(
    fn=generate_image,        # 处理函数
    inputs=gr.Textbox(label="输入图像描述"), # 输入组件：文本框
    outputs=gr.Image(label="生成的图像"),    # 输出组件：图像显示
    title="文本到图像生成器",
    description="输入一段描述，模型将为你生成对应的图像。"
)

demo.launch()
```

![](img/3debe5a7620fb7530158b03de96b66b4_29.png)

![](img/3debe5a7620fb7530158b03de96b66b4_31.png)

在这个界面中，用户在文本框中输入描述，点击提交后，右侧会显示生成的图像。例如，输入“一个电子宠物的灵魂漫步在维也纳城中”，模型会生成一幅符合该意境的图像。每次运行都会生成略有不同的新图像，增加了趣味性。

![](img/3debe5a7620fb7530158b03de96b66b4_33.png)

![](img/3debe5a7620fb7530158b03de96b66b4_35.png)

## 引入高级参数

Stable Diffusion模型功能强大，支持更多参数来精细控制图像生成效果。为了让用户能使用这些参数，我们需要升级应用界面。

![](img/3debe5a7620fb7530158b03de96b66b4_37.png)

![](img/3debe5a7620fb7530158b03de96b66b4_38.png)

以下是新增的几个关键参数：
*   **负向提示词**：引导模型避免生成包含某些元素的图像。
*   **推理步数**：控制生成过程的精细度，步数越多，质量可能越高，但耗时也越长。
*   **引导尺度**：控制生成结果与提示词的贴合程度。
*   **图像宽高**：设置生成图像的尺寸。

![](img/3debe5a7620fb7530158b03de96b66b4_40.png)

我们首先创建一个包含这些参数的新界面：

```python
def generate_image_advanced(prompt, negative_prompt, steps, guidance_scale, width, height):
    # 构建包含所有参数的请求
    payload = {
        "inputs": prompt,
        "parameters": {
            "negative_prompt": negative_prompt,
            "num_inference_steps": int(steps),
            "guidance_scale": guidance_scale,
            "width": int(width),
            "height": int(height)
        }
    }
    # ... 发送请求并返回图像
    return image

# 定义输入组件
prompt_input = gr.Textbox(label="正向提示词")
negative_input = gr.Textbox(label="负向提示词", placeholder="输入你不想在图像中出现的内容")
steps_slider = gr.Slider(minimum=1, maximum=100, value=20, label="推理步数")
guidance_slider = gr.Slider(minimum=1.0, maximum=20.0, value=7.5, label="引导尺度")
width_slider = gr.Slider(minimum=256, maximum=1024, value=512, step=64, label="宽度")
height_slider = gr.Slider(minimum=256, maximum=1024, value=512, step=64, label="高度")

# 创建界面
demo_advanced = gr.Interface(
    fn=generate_image_advanced,
    inputs=[prompt_input, negative_input, steps_slider, guidance_slider, width_slider, height_slider],
    outputs=gr.Image(),
    title="高级图像生成器"
)
```

测试这个应用，例如输入“动漫风格，公园里的一只狗”，并在负向提示词中输入“低质量”，同时增加推理步数，可以得到质量更优、更符合预期的图像。

## 使用Gradio Blocks优化布局

随着参数增多，基础界面变得拥挤。为了创建更灵活、美观的布局，我们将引入 **Gradio Blocks**。与 `gr.Interface` 这种快速但布局固定的方式不同，Blocks允许你像搭积木一样自定义UI结构。

以下是使用Blocks重构后的应用代码：

```python
import gradio as gr

def generate_image_advanced(prompt, negative_prompt, steps, guidance_scale, width, height):
    # 生成逻辑保持不变
    # ...
    return image

# 开始构建Blocks
with gr.Blocks() as demo:
    # 1. 标题
    gr.Markdown(“# 🎨 高级图像生成工作室”)

    # 2. 主要输入行：提示词和提交按钮并列
    with gr.Row():
        with gr.Column(scale=4, min_width=300):
            prompt_input = gr.Textbox(label=“描述你的画面”, placeholder=“例如：一只穿着宇航服的猫”)
        with gr.Column(scale=1, min_width=100):
            submit_btn = gr.Button(“生成”, variant=“primary”)

    # 3. 可折叠的“高级选项”区域
    with gr.Accordion(“🛠️ 高级选项”, open=False):
        negative_input = gr.Textbox(label=“负向提示词”, placeholder=“希望避免的内容，如：模糊、畸形”)
        with gr.Row():
            with gr.Column():
                steps_slider = gr.Slider(1, 100, value=20, label=“推理步数”)
                guidance_slider = gr.Slider(1.0, 20.0, value=7.5, label=“引导尺度”)
            with gr.Column():
                width_slider = gr.Slider(256, 1024, value=512, step=64, label=“图像宽度”)
                height_slider = gr.Slider(256, 1024, value=512, step=64, label=“图像高度”)

    # 4. 输出图像区域
    output_image = gr.Image(label=“生成结果”, height=400)

    # 5. 明确绑定按钮点击事件
    submit_btn.click(
        fn=generate_image_advanced,
        inputs=[prompt_input, negative_input, steps_slider, guidance_slider, width_slider, height_slider],
        outputs=output_image
    )

# 启动应用
demo.launch()
```

在这个布局中：
*   使用 `gr.Row()` 和 `gr.Column(scale=...)` 进行灵活排版。
*   使用 `gr.Accordion()` 将高级参数收纳起来，保持界面简洁。
*   使用 `gr.Button().click()` 显式地将按钮与生成函数绑定。
*   通过 `min_width` 等参数确保元素在不同屏幕尺寸下的可用性。

这种结构使得应用界面层次清晰：主要提示词输入和生成按钮突出显示，高级选项可按需展开，生成结果占据视觉中心。

## 总结

本节课中，我们一起学习了如何构建一个文本到图像的生成式AI应用。
1.  我们首先**连接了Stable Diffusion模型的API**，实现了核心的图像生成功能。
2.  接着，我们使用 **`gr.Interface`** 快速构建了一个基础应用原型。
3.  然后，我们为模型引入了**负向提示词、推理步数等高级参数**，增强了用户对生成结果的控制力。
4.  最后，为了管理复杂的输入控件，我们深入学习了 **Gradio Blocks**，通过自定义行、列、折叠面板等组件，构建了一个布局优雅、用户体验更佳的高级应用界面。

使用Gradio Blocks虽然需要编写更多布局代码，但它赋予了开发者极大的UI设计自由度，是构建复杂、专业级应用的强大工具。你可以尝试调整示例中的Blocks结构，创造出独一无二的界面。

![](img/3debe5a7620fb7530158b03de96b66b4_42.png)

在下一节课中，我们将把“图像描述”和“图像生成”两个模型结合起来，制作一个有趣的竞猜游戏应用。