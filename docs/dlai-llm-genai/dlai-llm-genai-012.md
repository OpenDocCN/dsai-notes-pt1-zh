# 012：11_实验室1演练 🧪

在本节课中，我们将学习如何利用大型语言模型（LLM）来总结对话。我们将从加载数据集和模型开始，逐步探索零样本、单样本和少样本提示工程，并了解如何通过调整生成参数来影响模型的输出。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_1.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_2.png)

---

![](img/e7932f5ef0eac2c4ad991634ab7c4295_3.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_5.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_7.png)

## 环境与库安装 💻

首先，我们需要设置实验环境。实验运行在拥有8个CPU核心和32GB内存的机器上，使用Python 3。

以下是需要安装的核心Python库：

*   **PyTorch**：一个主流的深度学习框架。
*   **Transformers**：由Hugging Face公司开发的开源库，提供了大量预训练模型和工具。
*   **Datasets**：同样来自Hugging Face，用于轻松加载和处理公共数据集。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_9.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_10.png)

运行以下代码块来安装这些库。安装过程可能需要几分钟，请耐心等待。如果出现一些警告信息，可以忽略，这不会影响后续实验。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_11.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_12.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_13.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_14.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_15.png)

```python
# 安装必要的库
!pip install torch torchdata transformers datasets
```

---

## 导入库与加载数据 📥

![](img/e7932f5ef0eac2c4ad991634ab7c4295_17.png)

上一节我们完成了环境配置，本节中我们来看看如何导入必要的函数并加载我们的实验数据。

安装完成后，我们导入关键函数并加载数据集。我们将使用名为 `dialogsum` 的公开数据集，它包含了大量对话及其人工编写的摘要。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_19.png)

```python
from datasets import load_dataset
from transformers import AutoModelForSeq2SeqLM, AutoTokenizer

![](img/e7932f5ef0eac2c4ad991634ab7c4295_20.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_21.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_22.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_23.png)

# 加载 dialogsum 数据集
dataset = load_dataset("dialogsum")
```

---

![](img/e7932f5ef0eac2c4ad991634ab7c4295_25.png)

## 探索数据集 🔍

在开始使用模型之前，让我们先观察一下数据的样子。数据集中的每个样本都包含一段对话和一个由人工编写的“基线摘要”。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_27.png)

以下是一个数据示例：

![](img/e7932f5ef0eac2c4ad991634ab7c4295_28.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_29.png)

*   **对话**:
    *   Person 1: What time is it, Tom?
    *   Person 2: Just a minute. It‘s 10 to 9 by my watch.
    *   ...
*   **基线摘要 (人类标注)**: Person 1 is in a hurry and Tom tells Person 2 there is plenty of time.

我们的目标是让模型生成的摘要尽可能接近这个人类编写的“黄金标准”。目前我们还没有加载任何模型，这只是原始数据。

---

## 加载模型与分词器 🤖

![](img/e7932f5ef0eac2c4ad991634ab7c4295_31.png)

现在，我们将加载用于本次实验的模型——**FLAN-T5**。这是一个通用的指令微调模型，能够执行多种任务，包括文本摘要。

加载模型后，我们还需要加载对应的**分词器**。分词器的作用是将原始文本（如对话）转换成模型能够理解的数字序列（即词元ID）。

```python
# 加载 FLAN-T5 模型和分词器
model_name = “google/flan-t5-base”
model = AutoModelForSeq2SeqLM.from_pretrained(model_name)
tokenizer = AutoTokenizer.from_pretrained(model_name)
```

为了理解分词器的工作，我们可以看一个简单的例子：

![](img/e7932f5ef0eac2c4ad991634ab7c4295_33.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_34.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_35.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_36.png)

```python
# 分词示例
sample_text = “What time is it, Tom?”
encoded = tokenizer(sample_text)
print(“编码后的词元ID:”, encoded[“input_ids”])
decoded = tokenizer.decode(encoded[“input_ids”])
print(“解码后的文本:”, decoded)
```
输出将展示文本如何被转换为数字，并能够被还原。这些数字对应着模型词汇表中的**嵌入向量**，是后续所有数学运算（如线性代数、反向传播）的基础。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_37.png)

---

![](img/e7932f5ef0eac2c4ad991634ab7c4295_39.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_40.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_41.png)

## 零样本推理尝试 🎯

我们已经加载了模型和分词器，本节中我们来看看直接使用模型进行摘要的效果如何，这被称为**零样本推理**。

我们直接将对话输入模型，不提供任何任务说明或示例，观察其生成的摘要。

对于之前的“时间”对话，模型可能生成类似 `“It‘s 10 to 9.”` 的摘要。与人类编写的基线摘要 `“Person 1 is in a hurry...”` 相比，模型遗漏了大量关键信息（如“Person 1很着急”这个上下文）。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_43.png)

对于另一个关于“升级软件”的对话，模型可能错误地总结为 `“Person 1 is thinking about upgrading their computer.”`，而基线摘要准确地描述了 `“Person 1 teaches Person 2 how to upgrade...”`。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_44.png)

可见，在没有明确指导的情况下，模型的摘要能力有限。

---

## 提示工程：零样本指令学习 📝

在课程中我们学到，可以通过**指令**来引导模型。这是一种**上下文学习**，具体到这里是**零样本指令推理**。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_46.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_47.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_48.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_49.png)

我们修改输入，在对话前加上明确的指令。以下是两种不同的提示模板：

**模板 1:**
```
Summarize the following conversation.

{对话内容}

Summary:
```

**模板 2:**
```
Dialogue:
{对话内容}

![](img/e7932f5ef0eac2c4ad991634ab7c4295_51.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_52.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_53.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_54.png)

What was going on?
```

![](img/e7932f5ef0eac2c4ad991634ab7c4295_55.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_56.png)

让我们看看效果。对于“时间”对话，使用模板1后，模型可能生成 `“The train is about to leave.”`，这比之前好一点，捕捉到了“赶火车”的细节，但仍不完整。对于“升级软件”对话，结果可能变为 `“Person 2 wants to add a painting program.”`，更接近但仍有偏差。

提示工程就是尝试不同指令，以找到能让模型表现最佳的表达方式。

---

## 提示工程：单样本与少样本学习 ✨

![](img/e7932f5ef0eac2c4ad991634ab7c4295_58.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_59.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_60.png)

如果仅靠指令效果不佳，我们可以为模型提供示例，这就是**单样本**和**少样本**学习。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_61.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_62.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_63.png)

*   **单样本学习**：在提示中提供一个完整的“输入-输出”示例（即一条对话及其正确摘要），然后再给出需要总结的新对话。
*   **少样本学习**：提供多个（例如三个）“输入-输出”示例，然后再给出新任务。

以下是单样本提示的结构：
```
对话: [示例对话]
摘要: [示例摘要]

对话: [需要总结的新对话]
摘要:
```

使用单样本提示后，对于“升级软件”对话，模型生成的摘要可能变为 `“Person 1 wants to upgrade software, Person 2 wants a painting program, Person 1 suggests a CD-ROM.”`，这显著更接近人类摘要，捕捉到了更多细节。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_65.png)

尝试少样本提示（提供三个示例）后，我们可能会发现，对于当前任务，单样本带来的提升已经足够，增加更多示例（少样本）可能不会带来显著改善。在实践中，通常尝试到5-6个示例就足够了，模型能力是核心。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_66.png)

---

![](img/e7932f5ef0eac2c4ad991634ab7c4295_68.png)

## 调整生成参数 🎛️

最后，我们可以通过调整生成参数来探索模型输出的多样性。这是**提示工程**的另一面。

关键的参数是**采样温度**：
*   **公式**: 在多项式分布中，温度参数 **T** 用于调整概率分布的平滑程度。`P_modified(i) = exp(log(P(i)) / T) / sum(exp(log(P(j)) / T))`。
*   **低温度 (如 0.1)**：使输出分布更“尖锐”，模型倾向于选择最高概率的词元，结果更确定、保守，可能重复。
*   **高温度 (如 1.0 或 2.0)**：使输出分布更“平滑”，模型选择低概率词元的机会增加，结果更具创造性、随机性，甚至可能产生不合理内容。

在Hugging Face的 `generation_config` 中，我们可以这样设置：

![](img/e7932f5ef0eac2c4ad991634ab7c4295_70.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_71.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_72.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_73.png)

```python
from transformers import GenerationConfig

![](img/e7932f5ef0eac2c4ad991634ab7c4295_74.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_75.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_76.png)

# 高温度，更具创造性
generation_config = GenerationConfig(max_new_tokens=50, temperature=1.5)
# 低温度，更确定和保守
generation_config = GenerationConfig(max_new_tokens=50, temperature=0.1)

outputs = model.generate(..., generation_config=generation_config)
```

![](img/e7932f5ef0eac2c4ad991634ab7c4295_78.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_79.png)

你可以尝试不同的温度值，直观感受它对生成文本风格的影响。

---

## 总结 📚

本节课中我们一起学习了使用FLAN-T5模型进行对话摘要的完整流程：
1.  **环境准备**：安装必要的库（PyTorch, Transformers, Datasets）。
2.  **数据加载**：使用 `datasets` 库加载 `dialogsum` 数据集。
3.  **模型加载**：加载预训练的FLAN-T5模型及其分词器。
4.  **基础推理**：观察到直接进行零样本推理的效果不佳。
5.  **提示工程**：通过**添加指令**（零样本）、**提供示例**（单样本、少样本）来显著提升模型摘要质量。
6.  **参数调优**：通过调整**采样温度**等生成参数，可以控制模型输出的创造性与确定性。

![](img/e7932f5ef0eac2c4ad991634ab7c4295_81.png)

![](img/e7932f5ef0eac2c4ad991634ab7c4295_82.png)

这是探索和评估一个大型语言模型能力的标准起点，也是决定是否需要以及如何对模型进行微调的重要步骤。