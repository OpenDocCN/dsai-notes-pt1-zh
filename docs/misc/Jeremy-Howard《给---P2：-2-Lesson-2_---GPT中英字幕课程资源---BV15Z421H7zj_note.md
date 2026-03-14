# 课程2：数据清洗与模型部署 🚀

在本节课中，我们将学习如何清洗数据，并使用训练好的模型创建一个可投入生产的应用程序。我们将从数据收集开始，训练一个初始模型来辅助数据清洗，然后使用Hugging Face Spaces和Gradio将模型部署为Web应用。

---

## 环境与工具准备 🛠️

上一节我们介绍了如何使用FastAI快速构建图像分类器。本节中，我们来看看如何将模型投入实际使用。首先，确保你拥有必要的工具和环境。

以下是推荐的设置步骤：
*   **终端**：在Windows上，推荐使用Windows Terminal和WSL（Windows Subsystem for Linux）来获得完整的Linux环境。在Mac或Linux上，可以直接使用系统终端。
*   **Python环境**：不要使用系统自带的Python。推荐使用Mambaforge（一个更快的Conda替代品）来管理独立的Python环境，以避免依赖冲突。
*   **代码编辑器**：Visual Studio Code是一个功能强大的集成开发环境，对Jupyter Notebook和Git支持良好。
*   **Git**：用于版本控制和代码推送。可以使用GitHub Desktop简化操作。

你可以通过运行FastAI的 `fastsetup` 仓库中的脚本来快速配置环境。

---

## 数据收集与初步训练 🐻

在开始数据清洗之前，我们首先需要收集数据并训练一个初步模型。这个模型将帮助我们识别数据中的问题。

我们使用DuckDuckGo搜索API（书中使用Bing API，但前者无需密钥）来收集“灰熊”、“黑熊”和“泰迪熊”的图片。代码与第一课类似：

```python
from fastai.vision.all import *
search_images_ddg  # 假设这是自定义的搜索函数
bear_types = ‘grizzly’, ‘black’, ‘teddy’
path = Path(‘bears’)
for o in bear_types:
    dest = (path/o)
    dest.mkdir(exist_ok=True, parents=True)
    download_images(dest, urls=search_images_ddg(f‘{o} bear’))
    resize_images(path/o, max_size=400, dest=path/o)
```

接下来，我们使用数据块（DataBlock）创建DataLoaders，并训练一个模型。这里我们使用 `RandomResizedCrop` 和 `aug_transforms` 进行数据增强，以帮助模型更好地泛化。

```python
bears = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=RandomResizedCrop(224, min_scale=0.5),
    batch_tfms=aug_transforms()
)
dls = bears.dataloaders(path)
learn = vision_learner(dls, resnet18, metrics=error_rate)
learn.fine_tune(4)
```

训练完成后，即使数据尚未清洗，模型也可能已经达到不错的准确率（例如3%的错误率）。这个模型将成为我们数据清洗的得力助手。

---

## 利用模型进行数据清洗 🧹

传统流程是先清洗数据再训练模型。但我们采用一个更高效的方法：先训练一个初步模型，然后利用它来找出数据中的“疑难杂症”。

模型训练完成后，我们可以使用 `ClassificationInterpretation` 类。

![](img/db648937386866e30f85c95de73d0b35_1.png)

![](img/db648937386866e30f85c95de73d0b35_2.png)

![](img/db648937386866e30f85c95de73d0b35_4.png)

![](img/db648937386866e30f85c95de73d0b35_6.png)

![](img/db648937386866e30f85c95de73d0b35_7.png)

![](img/db648937386866e30f85c95de73d0b35_9.png)

以下是分析模型错误的主要步骤：
1.  **查看混淆矩阵**：了解模型在哪些类别间容易混淆。
    ```python
    interp = ClassificationInterpretation.from_learner(learn)
    interp.plot_confusion_matrix()
    ```
2.  **查看最高损失样本**：损失高的样本通常是模型预测错误且非常自信，或者预测正确但非常不自信的样本。这些正是我们需要检查的。
    ```python
    interp.plot_top_losses(9, figsize=(15,11))
    ```
3.  **使用ImageClassifierCleaner进行交互式清洗**：这是FastAI提供的强大工具。它根据损失排序显示图片，你可以轻松地删除错误图片或将它们移动到正确的类别。
    ```python
    cleaner = ImageClassifierCleaner(learn)
    cleaner  # 运行后会显示一个交互式界面
    ```
    在界面中，你可以选择类别，浏览模型认为“困难”的图片，并进行删除或重新分类操作。操作完成后，运行以下代码应用更改：
    ```python
    for idx,cat in cleaner.change(): shutil.move(cleaner.fns[idx], path/cat)
    for idx in cleaner.delete(): (cleaner.fns[idx]).unlink()
    ```

![](img/db648937386866e30f85c95de73d0b35_11.png)

![](img/db648937386866e30f85c95de73d0b35_13.png)

这种方法能让你有针对性地清洗数据，效率远高于手动浏览全部数据。

![](img/db648937386866e30f85c95de73d0b35_15.png)

![](img/db648937386866e30f85c95de73d0b35_17.png)

![](img/db648937386866e30f85c95de73d0b35_19.png)

![](img/db648937386866e30f85c95de73d0b35_21.png)

![](img/db648937386866e30f85c95de73d0b35_23.png)

---

![](img/db648937386866e30f85c95de73d0b35_25.png)

![](img/db648937386866e30f85c95de73d0b35_27.png)

![](img/db648937386866e30f85c95de73d0b35_29.png)

## 模型导出与本地预测 📦

数据清洗完成后，我们可以重新训练模型或直接使用现有模型。为了部署，首先需要将模型导出。

![](img/db648937386866e30f85c95de73d0b35_31.png)

![](img/db648937386866e30f85c95de73d0b35_33.png)

在Kaggle或Google Colab等免费GPU平台上训练模型后，使用 `export` 方法保存模型。

![](img/db648937386866e30f85c95de73d0b35_35.png)

![](img/db648937386866e30f85c95de73d0b35_37.png)

![](img/db648937386866e30f85c95de73d0b35_39.png)

```python
learn.export(‘model.pkl’)
```

下载导出的 `model.pkl` 文件到本地。现在，我们可以在不需要GPU的本地环境中加载模型并进行预测。

```python
# 在本地脚本或笔记本中
from fastai.vision.all import *
import PIL

![](img/db648937386866e30f85c95de73d0b35_41.png)

![](img/db648937386866e30f85c95de73d0b35_42.png)

learn = load_learner(‘model.pkl’)
img = PILImage.create(‘dog.jpg’) # 加载一张图片
pred, pred_idx, probs = learn.predict(img)
print(f‘Prediction: {pred}; Probability: {probs[pred_idx]:.04f}’)
```

预测过程非常快速，通常在毫秒级别完成。

![](img/db648937386866e30f85c95de73d0b35_44.png)

![](img/db648937386866e30f85c95de73d0b35_46.png)

![](img/db648937386866e30f85c95de73d0b35_48.png)

---

![](img/db648937386866e30f85c95de73d0b35_50.png)

![](img/db648937386866e30f85c95de73d0b35_52.png)

## 使用Gradio和Hugging Face Spaces创建Web应用 🌐

现在，我们将模型变成一个任何人都可以通过网页访问的应用程序。我们将使用Hugging Face Spaces（免费托管平台）和Gradio（快速创建UI的库）。

![](img/db648937386866e30f85c95de73d0b35_54.png)

![](img/db648937386866e30f85c95de73d0b35_55.png)

![](img/db648937386866e30f85c95de73d0b35_57.png)

![](img/db648937386866e30f85c95de73d0b35_58.png)

以下是创建应用的基本流程：
1.  **在Hugging Face上创建Space**：选择Gradio作为SDK，创建一个新的Space仓库。
2.  **准备应用脚本**：我们需要创建一个 `app.py` 文件，其中包含加载模型、定义预测函数和创建Gradio界面的代码。
    ```python
    import gradio as gr
    from fastai.vision.all import *

    learn = load_learner(‘model.pkl’)
    categories = (‘dog’, ‘cat’)

    def classify_image(img):
        pred, idx, probs = learn.predict(img)
        return dict(zip(categories, map(float, probs)))

    image = gr.Image(type=“pil”)
    label = gr.Label()
    examples = [‘dog.jpg’, ‘cat.jpg’, ‘dunno.jpg’]

    iface = gr.Interface(fn=classify_image, inputs=image, outputs=label, examples=examples)
    iface.launch()
    ```
3.  **推送代码到Space**：将 `model.pkl` 和 `app.py` 文件推送到Git仓库。Hugging Face会自动构建并部署你的应用。几分钟后，你就获得了一个可公开访问的URL。

![](img/db648937386866e30f85c95de73d0b35_60.png)

![](img/db648937386866e30f85c95de73d0b35_62.png)

![](img/db648937386866e30f85c95de73d0b35_64.png)

![](img/db648937386866e30f85c95de73d0b35_65.png)

---

![](img/db648937386866e30f85c95de73d0b35_67.png)

![](img/db648937386866e30f85c95de73d0b35_69.png)

![](img/db648937386866e30f85c95de73d0b35_71.png)

![](img/db648937386866e30f85c95de73d0b35_73.png)

## 进阶：构建自定义JavaScript前端 🖥️

![](img/db648937386866e30f85c95de73d0b35_75.png)

![](img/db648937386866e30f85c95de73d0b35_77.png)

![](img/db648937386866e30f85c95de73d0b35_79.png)

![](img/db648937386866e30f85c95de73d0b35_81.png)

Gradio界面适合快速原型。如果你想拥有完全自定义外观和功能的Web应用，可以利用Hugging Face Spaces提供的API。

每个Gradio Space都自动提供了一个API端点。你可以使用JavaScript调用这个API，并用自己的前端页面展示结果。

以下是调用API的核心JavaScript代码示例：

![](img/db648937386866e30f85c95de73d0b35_83.png)

![](img/db648937386866e30f85c95de73d0b35_85.png)

```javascript
async function query(data) {
    const response = await fetch(
        “https://your-username.hf.space/api/predict”,
        {
            method: “POST”,
            body: data,
        }
    );
    const result = await response.json();
    return result;
}

// 使用FormData上传图片并获取预测结果
let formData = new FormData();
formData.append(“data”, imageFile);
query(formData).then((response) => {
    console.log(JSON.stringify(response));
});
```

![](img/db648937386866e30f85c95de73d0b35_87.png)

![](img/db648937386866e30f85c95de73d0b35_89.png)

![](img/db648937386866e30f85c95de73d0b35_91.png)

你可以用简单的HTML和JavaScript编写前端，并使用GitHub Pages（配合FastPages模板可以简化流程）免费托管这个静态网站。这样，你就拥有了一个完全独立、界面自定义的深度学习应用。

![](img/db648937386866e30f85c95de73d0b35_93.png)

![](img/db648937386866e30f85c95de73d0b35_95.png)

![](img/db648937386866e30f85c95de73d0b35_97.png)

![](img/db648937386866e30f85c95de73d0b35_99.png)

![](img/db648937386866e30f85c95de73d0b35_101.png)

![](img/db648937386866e30f85c95de73d0b35_103.png)

---

## 总结 📝

本节课中我们一起学习了深度学习项目的后半段关键流程：
1.  **数据清洗新思路**：先训练一个初步模型，再利用模型（通过 `ImageClassifierCleaner` 和最高损失分析）高效定位和清洗问题数据。
2.  **模型部署**：将训练好的模型（`model.pkl`）导出，并在本地加载进行快速预测。
3.  **快速创建Web应用**：使用Hugging Face Spaces和Gradio，在几分钟内将模型部署为可公开访问的Web应用，无需服务器管理。
4.  **自定义前端开发**：通过调用Hugging Face Spaces提供的API，结合JavaScript和GitHub Pages，可以构建功能与界面完全自定义的应用程序。

![](img/db648937386866e30f85c95de73d0b35_105.png)

通过这些步骤，你可以将任何一个想法从数据收集、模型训练，最终落地为一个真正的、可用的产品。下一课，我们将深入自然语言处理领域，并开始探索模型内部的工作原理。