# 深度学习实践课程：2：模型部署与数据清洗 🚀

在本节课中，我们将学习如何将训练好的深度学习模型部署到生产环境，并探索一个强大的数据清洗技巧：**先训练模型，再清洗数据**。我们将使用Hugging Face Spaces和Gradio来快速创建交互式Web应用。

---

## 概述 📋

上一节我们学习了如何快速构建一个图像分类器。本节中，我们将看看如何将这个模型变成一个可供他人使用的实际应用。核心步骤包括：数据清洗、模型导出、创建Web界面以及部署。我们将从一个反直觉但极其有效的技巧开始：**在清洗数据之前先训练一个模型**。

---

## 数据清洗前的模型训练 🧠

传统流程通常是先清洗数据，再训练模型。但我们将采用一个更高效的方法：先快速训练一个初始模型，然后利用这个模型来帮助我们**发现数据中的问题**。

### 训练初始模型

我们使用与上节课类似的代码，用`DataBlock`创建数据加载器并训练一个简单的分类器（例如，区分泰迪熊、灰熊和黑熊）。

```python
from fastai.vision.all import *
path = Path('bears')
bears = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224))
dls = bears.dataloaders(path)
learn = vision_learner(dls, resnet34, metrics=error_rate)
learn.fine_tune(4)
```

### 利用模型识别问题数据

训练完成后，我们可以使用FastAI的`ClassificationInterpretation`工具来查看模型预测最不准的样本。

```python
interp = ClassificationInterpretation.from_learner(learn)
interp.plot_top_losses(9, figsize=(15,10))
```

`plot_top_losses`会展示损失值最高的图片。高损失可能源于两种情况：
1.  **预测错误且置信度高**：模型很自信地给出了错误答案。
2.  **预测正确但置信度低**：模型猜对了，但非常不确定。

这些样本正是我们需要重点检查的数据点。

---

## 使用ImageClassifierCleaner进行交互式清洗 🧹

FastAI提供了一个名为`ImageClassifierCleaner`的图形化工具，它利用我们刚刚训练的模型来辅助数据清洗。

```python
cleaner = ImageClassifierCleaner(learn)
cleaner
```

运行上述代码会弹出一个交互界面。你可以：
*   选择类别（如“teddy”）。
*   界面会按损失值从高到低显示该类别下的图片。
*   对于错误标记的图片，你可以将其**移动到正确的类别**。
*   对于不属于任何类别的无关图片，你可以点击**删除**。

这个工具的核心优势在于，它让我们能够**有针对性地检查最可能出问题的数据**，而不是盲目地浏览整个数据集。

清洗完成后，运行以下代码应用更改：

```python
for idx, cat in cleaner.change(): shutil.move(str(cleaner.files[idx]), path/cat)
for idx in cleaner.delete(): cleaner.files[idx].unlink()
```

**总结一下这个强大的工作流**：我们通过一个快速训练的模型，智能地定位了数据中的噪声和错误，从而实现了高效、精准的数据清洗。

---

## 模型部署到生产环境 🌐

数据清洗完毕后，下一步就是将模型部署出去，让其他人也能使用。我们将使用**Hugging Face Spaces**和**Gradio**，它们提供了免费、简单的模型托管和界面创建服务。

![](img/ee7b9a2faf685413bc8869fb66d9d516_1.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_3.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_5.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_6.png)

### 第一步：创建Hugging Face Space

![](img/ee7b9a2faf685413bc8869fb66d9d516_8.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_10.png)

1.  访问 [Hugging Face Spaces](https://huggingface.co/spaces)。
2.  点击“Create new Space”。
3.  填写空间名称，选择“Gradio”作为SDK，设置许可证为“Apache 2.0”，并创建空间。

![](img/ee7b9a2faf685413bc8869fb66d9d516_12.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_14.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_16.png)

### 第二步：准备模型文件

![](img/ee7b9a2faf685413bc8869fb66d9d516_18.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_20.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_22.png)

在Kaggle、Google Colab等免费GPU平台上训练并导出你的模型。

![](img/ee7b9a2faf685413bc8869fb66d9d516_24.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_26.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_28.png)

```python
# 在训练完成后，导出模型
learn.export('model.pkl')
```

下载这个`model.pkl`文件。

### 第三步：编写Gradio应用脚本

![](img/ee7b9a2faf685413bc8869fb66d9d516_30.png)

我们需要创建一个`app.py`文件，它包含加载模型和处理预测的逻辑。

![](img/ee7b9a2faf685413bc8869fb66d9d516_32.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_34.png)

```python
from fastai.vision.all import *
import gradio as gr

![](img/ee7b9a2faf685413bc8869fb66d9d516_36.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_38.png)

# 1. 加载导出的模型
learn = load_learner('model.pkl')

# 2. 定义预测函数
def classify_image(img):
    # 模型预测
    pred, pred_idx, probs = learn.predict(img)
    # 将结果格式化为Gradio需要的字典形式
    return {learn.dls.vocab[i]: float(probs[i]) for i in range(len(learn.dls.vocab))}

# 3. 创建Gradio界面
image = gr.Image(type="pil", label="上传图片")
label = gr.Label(num_top_classes=3, label="预测结果")
examples = ['dog.jpg', 'cat.jpg', 'dunno.jpg']

# 4. 启动界面
iface = gr.Interface(fn=classify_image, inputs=image, outputs=label, examples=examples)
iface.launch()
```

![](img/ee7b9a2faf685413bc8869fb66d9d516_40.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_41.png)

**关键点**：如果模型在训练时使用了自定义的标签函数（如`is_cat`），你必须在`app.py`中重新定义这个函数，因为导出的模型不包含源代码。

### 第四步：推送文件到Space

将`model.pkl`和`app.py`文件上传到你的Hugging Face Space仓库中。你可以使用Git命令、GitHub Desktop或VS Code的Git功能来完成。

![](img/ee7b9a2faf685413bc8869fb66d9d516_43.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_45.png)

```bash
git add .
git commit -m "Add model and app"
git push
```

![](img/ee7b9a2faf685413bc8869fb66d9d516_47.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_49.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_51.png)

推送完成后，Hugging Face会自动构建并发布你的应用。几分钟后，任何人都可以通过生成的URL访问你的图像分类器了！

---

## 进阶：使用JavaScript API构建自定义前端 🛠️

Gradio界面适合快速原型开发。如果你需要更定制化的用户体验，可以利用Hugging Face Spaces提供的**API端点**，用JavaScript构建自己的网页。

![](img/ee7b9a2faf685413bc8869fb66d9d516_53.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_54.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_56.png)

### 调用API

每个Gradio Space都自带一个API。你可以在Space页面点击“View API”查看调用方式。通常，你可以通过向一个URL发送POST请求（包含图片数据）来获得模型的JSON格式预测结果。

![](img/ee7b9a2faf685413bc8869fb66d9d516_58.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_60.png)

### 简单HTML/JavaScript示例

![](img/ee7b9a2faf685413bc8869fb66d9d516_62.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_64.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_66.png)

以下是一个极简的HTML文件，它调用部署好的模型API：

![](img/ee7b9a2faf685413bc8869fb66d9d516_68.png)

```html
<!DOCTYPE html>
<html>
<body>
  <input type="file" id="file-input" accept="image/*">
  <div id="result"></div>

  <script>
    const API_URL = "https://your-username.hf.space/run/predict";

    document.getElementById('file-input').addEventListener('change', function(e) {
      const file = e.target.files[0];
      const reader = new FileReader();
      reader.onload = function(e) {
        const data = e.target.result;
        // 调用API
        fetch(API_URL, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ "data": [data] })
        })
        .then(response => response.json())
        .then(data => {
          // 显示结果
          document.getElementById('result').innerHTML = `预测结果: ${JSON.stringify(data.data)}`;
        });
      };
      reader.readAsDataURL(file);
    });
  </script>
</body>
</html>
```

### 部署静态网站

你可以将HTML/JS/CSS文件放在**GitHub Pages**上免费托管。使用**FastPages**可以更轻松地创建和部署基于Jekyll的网站。

1.  访问 [FastPages模板](https://github.com/fastai/fastpages/generate) 生成你自己的仓库。
2.  克隆仓库到本地，将你的网页文件（如`index.html`）放入。
3.  推送更改到GitHub，你的网站就会自动发布在 `https://你的用户名.github.io/仓库名`。

![](img/ee7b9a2faf685413bc8869fb66d9d516_70.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_72.png)

---

## 本地开发环境设置 💻

![](img/ee7b9a2faf685413bc8869fb66d9d516_74.png)

为了在本地运行Jupyter Notebook和进行开发，我们推荐使用Mamba（一个更快的Conda替代品）来管理Python环境。

![](img/ee7b9a2faf685413bc8869fb66d9d516_76.png)

### 简易安装步骤

![](img/ee7b9a2faf685413bc8869fb66d9d516_78.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_80.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_81.png)

1.  **安装Mambaforge**：使用[fastsetup](https://github.com/fastai/fastsetup)脚本可以一键安装。
2.  **创建环境并安装库**：
    ```bash
    mamba create -n fastai python=3.9
    mamba activate fastai
    mamba install -c fastchan fastai
    ```
3.  **安装Jupyter Notebook扩展**（可选但推荐）：
    ```bash
    pip install jupyter_contrib_nbextensions
    ```
    这可以提供目录导航、可折叠标题等实用功能。

![](img/ee7b9a2faf685413bc8869fb66d9d516_83.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_85.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_87.png)

![](img/ee7b9a2faf685413bc8869fb66d9d516_89.png)

---

## 总结 🎯

本节课中我们一起学习了：

1.  **创新的数据清洗流程**：先训练一个简单模型，再利用`ClassificationInterpretation`和`ImageClassifierCleaner`智能定位并清洗问题数据，这比传统手动浏览高效得多。
2.  **完整的模型部署流水线**：从导出模型（`learn.export`）到使用Hugging Face Spaces和Gradio创建免费的、可交互的Web应用。
3.  **自定义前端开发**：了解了如何通过Gradio Space的API，用简单的HTML和JavaScript构建更个性化的用户界面，并利用GitHub Pages进行免费部署。
4.  **本地环境搭建**：介绍了使用Mamba配置稳定Python开发环境的最佳实践。

![](img/ee7b9a2faf685413bc8869fb66d9d516_91.png)

现在，你不仅能够构建深度学习模型，还能将它变成任何人都可以使用的实际产品。在下一课中，我们将转向自然语言处理领域，并深入探索模型是如何通过“随机梯度下降”等机制进行学习的。