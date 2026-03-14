# Transformers原理细节及NLP任务应用！P37：L6.5 - 使用Hugging Face Hub 🚀

在本节课中，我们将学习如何利用Hugging Face Hub平台。Hub是一个中央平台，用于发现、使用和共享最先进的机器学习模型与数据集。我们将了解如何浏览平台、使用现有模型、将模型上传至Hub，以及如何创建详细的模型卡。

---

## 概述：Hugging Face Hub平台

![](img/c09cf1ac6e21d36580a79d639f204d72_1.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_2.png)

Hugging Face Hub是一个中央平台，使任何人都能发现、使用和贡献新的最先进模型和数据集。它托管了超过11,000个公开可用的模型，涵盖自然语言处理、计算机视觉、语音等多个领域。Hub上的模型作为Git仓库存在，便于版本控制和社区协作。

---

## 浏览与使用模型 🔍

上一节我们介绍了Hub平台的基本概念，本节中我们来看看如何浏览和使用Hub上的模型。

![](img/c09cf1ac6e21d36580a79d639f204d72_4.png)

### 访问模型中心

![](img/c09cf1ac6e21d36580a79d639f204d72_6.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_7.png)

要访问模型中心，请访问Hugging Face官方网站并点击右上角的“模型”选项卡。网页界面分为几个部分，左侧是用于筛选模型的类别。

以下是主要的筛选类别：

*   **任务**：模型可用于的任务类型，如文本分类、问答、图像分类、自动语音识别等。
*   **库**：模型基于的框架，如PyTorch、TensorFlow、JAX，或高层框架如Transformers、Sentence Transformers。
*   **数据集**：选择在特定数据集上训练的模型。
*   **语言**：选择能够处理特定语言的模型。
*   **许可证**：根据许可证类型筛选模型。

### 理解模型卡

点击一个模型后，你会看到它的模型卡。模型卡包含关于模型的关键信息，对使用模型至关重要。

模型卡通常包含以下部分：

![](img/c09cf1ac6e21d36580a79d639f204d72_9.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_10.png)

*   **模型描述**：解释模型的用途和架构。
*   **预期用途与限制**：说明模型适合和不适合的场景。
*   **如何使用**：提供加载和使用模型的代码片段。
*   **训练数据与过程**：描述用于训练的数据集和方法。
*   **评估结果**：展示模型在基准测试上的性能。
*   **限制与偏见**：指出模型可能存在的偏见和局限性。

### 使用推理API

在模型卡的右侧是推理API小部件，允许你直接在网页上与模型交互。你可以修改输入文本并点击“Compute”查看模型的预测结果。

### 以编程方式使用模型

有三种主要方式以编程方式使用Hub上的模型。

以下是使用模型的三种方法：

![](img/c09cf1ac6e21d36580a79d639f204d72_12.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_13.png)

1.  **使用`pipeline`（高级API）**：这是最简单的方法，适用于常见任务。
    ```python
    from transformers import pipeline
    unmasker = pipeline(‘fill-mask’, model=‘camembert-base’)
    unmasker(“Le but de la <mask> est de vivre.”)
    ```
2.  **使用`AutoModel`和`AutoTokenizer`（中级API）**：这种方法更灵活，是架构无关的。
    ```python
    from transformers import AutoModelForMaskedLM, AutoTokenizer
    model = AutoModelForMaskedLM.from_pretrained(‘camembert-base’)
    tokenizer = AutoTokenizer.from_pretrained(‘camembert-base’)
    ```
3.  **使用特定模型类（低级API）**：直接指定模型架构，灵活性最高，但需要了解具体架构。
    ```python
    from transformers import CamembertForMaskedLM, CamembertTokenizer
    ```

**注意**：使用模型时，请确保所选模型是针对你打算执行的任务进行过训练或微调的。

![](img/c09cf1ac6e21d36580a79d639f204d72_15.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_16.png)

---

## 共享模型至Hub 📤

上一节我们学习了如何使用Hub上的现有模型，本节中我们来看看如何将自己的模型共享到Hub，供社区使用。

### 创建模型仓库

首先，你需要一个Hugging Face账户。登录后，点击“New Model”创建一个新的模型仓库。你需要填写以下信息：

*   **所有者**：你的用户名或所属组织。
*   **模型名称**：模型的唯一标识符。
*   **可见性**：选择“Public”（公开，推荐）或“Private”（私有）。

### 上传文件的三种方法

创建仓库后，你可以通过多种方式上传模型文件。

以下是上传模型文件的三种主要方法：

![](img/c09cf1ac6e21d36580a79d639f204d72_18.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_20.png)

1.  **网页界面**：直接在仓库页面上点击“Add file”按钮上传文件。
2.  **Git命令行与Git LFS**：这是最常用和强大的方法，适合管理大型模型文件。
    *   首先在本地克隆你的仓库：`git clone https://huggingface.co/你的用户名/模型名`
    *   将模型文件（如`pytorch_model.bin`, `config.json`, `tokenizer.json`）复制到本地仓库文件夹。
    *   使用`git add .`跟踪文件，`git commit -m “添加模型文件”`提交更改，最后`git push`推送到Hub。
    *   对于大文件（>10MB），使用Git LFS：`git lfs track “*.bin” “*.h5”`，然后正常进行`git add`和`git push`。
3.  **使用`push_to_hub` API（编程方式）**：这是最方便的方法，尤其在使用`Trainer` API训练后。
    *   在训练参数中设置`push_to_hub=True`，并指定`hub_model_id`。
    *   训练结束后，调用`trainer.push_to_hub()`即可。
    *   你也可以直接推送模型和分词器：
        ```python
        model.push_to_hub(“你的用户名/模型名”)
        tokenizer.push_to_hub(“你的用户名/模型名”)
        ```

![](img/c09cf1ac6e21d36580a79d639f204d72_22.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_24.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_26.png)

### 身份验证

![](img/c09cf1ac6e21d36580a79d639f204d72_28.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_29.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_31.png)

上传模型需要进行身份验证。你可以通过以下方式之一完成：

![](img/c09cf1ac6e21d36580a79d639f204d72_33.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_34.png)

*   在终端运行 `huggingface-cli login` 并输入你的访问令牌。
*   在代码中直接指定令牌：`model.push_to_hub(“模型名”, use_auth_token=“你的令牌”)`。

![](img/c09cf1ac6e21d36580a79d639f204d72_36.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_38.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_40.png)

---

![](img/c09cf1ac6e21d36580a79d639f204d72_42.png)

## 创建有效的模型卡 📝

上一节我们介绍了如何上传模型，本节中我们来看看如何创建一份信息丰富、有用的模型卡。模型卡是模型仓库的核心，它决定了社区成员能否理解并有效使用你的模型。

### 模型卡的重要性

![](img/c09cf1ac6e21d36580a79d639f204d72_44.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_46.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_48.png)

模型卡是一个`README.md`文件，它不仅是模型的说明书，更是确保模型可重复性、公平性和透明度的关键。一份优秀的模型卡应包含足够的信息，让他人能够复现结果、理解模型的局限性，并在其基础上进行构建。

![](img/c09cf1ac6e21d36580a79d639f204d72_50.png)

### 模型卡的核心内容

![](img/c09cf1ac6e21d36580a79d639f204d72_52.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_54.png)

一份完整的模型卡应包含以下几个部分。

![](img/c09cf1ac6e21d36580a79d639f204d72_56.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_58.png)

以下是模型卡推荐包含的核心部分：

![](img/c09cf1ac6e21d36580a79d639f204d72_60.png)

*   **模型描述**：简要说明模型是什么、基于什么架构、训练目标是什么。
*   **预期用途**：说明模型设计用于解决什么问题，在什么场景下使用。
*   **限制与偏见**：**这是至关重要的部分**。必须明确指出模型在哪些数据或群体上可能表现不佳，以及训练数据中存在的任何已知偏见。例如，一个语言模型可能将某些职业与特定性别关联。
*   **训练数据**：描述用于训练模型的数据集及其来源。
*   **训练过程**：概述关键的训练超参数、硬件配置和训练时长。
*   **评估结果**：以表格形式展示模型在标准评估数据集上的性能指标（如准确率、F1分数）。
*   **如何使用**：提供清晰的代码示例，展示如何加载模型并进行推理。
*   **引用**：提供相关的论文或技术报告引用。

![](img/c09cf1ac6e21d36580a79d639f204d72_62.png)

### 添加元数据

![](img/c09cf1ac6e21d36580a79d639f204d72_64.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_66.png)

你可以在模型卡的顶部添加YAML格式的元数据，这有助于Hub更好地索引和展示你的模型。

例如：
```yaml
---
language: zh
license: apache-2.0
datasets:
  - your_dataset_name
tags:
  - text-classification
  - sentiment-analysis
---
```

### 查看优秀示例

你可以参考Hub上热门模型的模型卡（如`bert-base-uncased`）来了解最佳实践。模型卡的概念源于论文《Model Cards for Model Reporting》，其中提供了详细的指导原则。

---

## 额外工具与功能 🛠️

除了核心功能，Hugging Face Hub还提供了一些强大的客户端工具。

![](img/c09cf1ac6e21d36580a79d639f204d72_68.png)

### `huggingface_hub` 库

![](img/c09cf1ac6e21d36580a79d639f204d72_70.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_72.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_74.png)

`huggingface_hub`是一个Python客户端库，提供了与Hub交互的编程接口。

![](img/c09cf1ac6e21d36580a79d639f204d72_76.png)

你可以使用它进行以下操作：

![](img/c09cf1ac6e21d36580a79d639f204d72_78.png)

*   **列出模型**：`list_models(filter=“fill-mask”, library=“transformers”)`
*   **下载/上传文件**：`upload_file`, `download_file`
*   **以编程方式管理仓库**：使用`Repository`类在代码中轻松管理文件提交。
    ```python
    from huggingface_hub import Repository
    with Repository(“本地目录”, clone_from=“用户名/仓库名”).commit(commit_message=“我的提交”):
        with open(“file.txt”, “w”) as f:
            f.write(“内容”)
    # 文件会自动添加、提交并推送到Hub
    ```

![](img/c09cf1ac6e21d36580a79d639f204d72_80.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_82.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_84.png)

### 托管TensorBoard日志

![](img/c09cf1ac6e21d36580a79d639f204d72_86.png)

如果你在训练过程中生成了TensorBoard日志文件（`runs/`目录），并将它们推送到Hub，Hub会自动为你提供一个托管的TensorBoard实例，方便你在线查看训练指标图表。

![](img/c09cf1ac6e21d36580a79d639f204d72_88.png)

---

![](img/c09cf1ac6e21d36580a79d639f204d72_90.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_91.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_93.png)

## 总结与下一步 🎯

![](img/c09cf1ac6e21d36580a79d639f204d72_95.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_97.png)

本节课中我们一起学习了Hugging Face Hub的完整使用流程。

我们首先了解了Hub作为模型共享中心的价值。接着，我们学习了如何浏览和筛选Hub上的海量模型，并通过模型卡和推理API来理解和使用它们。然后，我们深入探讨了如何将自己的模型通过网页、Git或`push_to_hub` API共享到社区。最后，我们强调了创建详尽模型卡的重要性，并介绍了一些额外的工具，如`huggingface_hub`库。

![](img/c09cf1ac6e21d36580a79d639f204d72_99.png)

![](img/c09cf1ac6e21d36580a79d639f204d72_100.png)

现在，你已经掌握了将你的工作开放给全球社区所需的所有技能。我们鼓励你搜索一个感兴趣的数据集，微调一个模型，并将你的项目分享到Hub和课程论坛上。