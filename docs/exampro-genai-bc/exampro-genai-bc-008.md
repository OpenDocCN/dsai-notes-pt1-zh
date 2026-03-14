# 8：句子构造器 - Meta AI 🧠

![](img/af24677c6bf72a82b6436026f1767f2a_1.png)

![](img/af24677c6bf72a82b6436026f1767f2a_3.png)

在本节课中，我们将学习如何为Meta AI（基于Llama 3 700亿参数模型）编写一个有效的提示词，以创建一个日语学习辅助工具。这个工具将帮助学生将英语句子翻译成日语，但不会直接给出答案，而是通过提供词汇表和句子结构线索来引导学生自己完成。

---

![](img/af24677c6bf72a82b6436026f1767f2a_5.png)

## 概述

![](img/af24677c6bf72a82b6436026f1767f2a_7.png)

![](img/af24677c6bf72a82b6436026f1767f2a_9.png)

我们将创建一个“句子构造器”提示词，其核心目标是扮演一位日语老师。当学生提供一个英语句子时，AI需要提供词汇表（不含助词）和句子结构线索，引导学生自己完成日语翻译，而不是直接给出答案。我们将通过迭代优化，逐步完善这个提示词。

![](img/af24677c6bf72a82b6436026f1767f2a_11.png)

![](img/af24677c6bf72a82b6436026f1767f2a_12.png)

## 环境准备与项目设置

![](img/af24677c6bf72a82b6436026f1767f2a_14.png)

![](img/af24677c6bf72a82b6436026f1767f2a_16.png)

首先，我们需要在GitHub上创建一个仓库来管理我们的工作。虽然讲师为了演示分叉了仓库，但你应该按照指示创建一个全新的仓库。

![](img/af24677c6bf72a82b6436026f1767f2a_18.png)

我们将使用GitHub Dev（一个免费的在线代码编辑器）作为我们的工作环境。在仓库页面按下 `.` 键即可打开。

![](img/af24677c6bf72a82b6436026f1767f2a_20.png)

![](img/af24677c6bf72a82b6436026f1767f2a_22.png)

在项目中，我们创建一个名为 `sentence-constructor` 的文件夹，并在其中为Meta AI创建一个子文件夹 `meta-ai`。在这里，我们将创建我们的提示词文件 `prompt.txt`。

![](img/af24677c6bf72a82b6436026f1767f2a_24.png)

![](img/af24677c6bf72a82b6436026f1767f2a_26.png)

![](img/af24677c6bf72a82b6436026f1767f2a_27.png)

![](img/af24677c6bf72a82b6436026f1767f2a_29.png)

![](img/af24677c6bf72a82b6436026f1767f2a_31.png)

为了跟踪我们的修改，我们可以在仓库中创建一个Issue（例如：#1），并将每次提交与这个Issue关联，使用类似 `p0.0.0` 的标签来标记提示词的版本。

![](img/af24677c6bf72a82b6436026f1767f2a_33.png)

![](img/af24677c6bf72a82b6436026f1767f2a_35.png)

## 了解模型：Meta AI

![](img/af24677c6bf72a82b6436026f1767f2a_37.png)

![](img/af24677c6bf72a82b6436026f1767f2a_39.png)

在开始编写提示词前，我们确认了当前使用的Meta AI助手是基于 **Llama 3 700亿参数模型**。这是一个性能相当强大的开源模型。了解底层模型有助于我们理解其能力和限制。

![](img/af24677c6bf72a82b6436026f1767f2a_41.png)

## 第一版提示词：基础设定

![](img/af24677c6bf72a82b6436026f1767f2a_43.png)

![](img/af24677c6bf72a82b6436026f1767f2a_45.png)

![](img/af24677c6bf72a82b6436026f1767f2a_47.png)

我们的起点非常简单：明确AI的角色和基本任务。

![](img/af24677c6bf72a82b6436026f1767f2a_49.png)

![](img/af24677c6bf72a82b6436026f1767f2a_51.png)

![](img/af24677c6bf72a82b6436026f1767f2a_53.png)

```markdown
Role: Japanese language teacher.
Teaching instructions: The student is going to provide you an English sentence. You need to help the student transcribe the sentence into Japanese.
Student input: Bears are at the door. Did you leave the garbage out?
```

**测试结果分析**：
AI直接给出了完整的日语翻译和分解，这违背了我们的初衷。我们不需要它直接给出答案，而是希望它提供学习线索。

![](img/af24677c6bf72a82b6436026f1767f2a_55.png)

## 第二版提示词：明确约束

![](img/af24677c6bf72a82b6436026f1767f2a_57.png)

![](img/af24677c6bf72a82b6436026f1767f2a_58.png)

![](img/af24677c6bf72a82b6436026f1767f2a_59.png)

![](img/af24677c6bf72a82b6436026f1767f2a_61.png)

![](img/af24677c6bf72a82b6436026f1767f2a_63.png)

根据第一次测试的问题，我们增加了更多约束条件，防止AI“剧透”。

![](img/af24677c6bf72a82b6436026f1767f2a_65.png)

![](img/af24677c6bf72a82b6436026f1767f2a_67.png)

```markdown
Role: Japanese language teacher.
Teaching instructions:
- The student will provide an English sentence.
- Help the student transcribe it into Japanese.
- **Do not provide the transcription.** Make the student work through it via clues.
- Provide a table of vocabulary. Do not provide particles in the vocabulary. The student needs to figure out the correct particles to use.
- Provide words in their dictionary form. The student needs to figure out conjugations and tenses.
Student input: Bears are at the door. Did you leave the garbage out?
```

![](img/af24677c6bf72a82b6436026f1767f2a_69.png)

**测试结果分析**：
输出有所改善，给出了词汇表。但出现了新问题：
1.  词汇表中包含了动词的礼貌形（如 `nokosu`），而我们要求的是字典形。
2.  提供的句子结构线索过于模糊，对初学者帮助不大。
3.  有时不显示日文字符。

![](img/af24677c6bf72a82b6436026f1767f2a_71.png)

## 第三版提示词：结构化与示例

![](img/af24677c6bf72a82b6436026f1767f2a_73.png)

为了获得更稳定、理想的输出，我们决定采用更结构化的提示方法，并引入好坏示例来指导AI。

![](img/af24677c6bf72a82b6436026f1767f2a_75.png)

我们首先将提示词文件改为 `prompt.md` 格式，以便更好地使用Markdown。然后，我们加入了详细的示例部分。

以下是核心改进点：

1.  **指定格式**：明确要求词汇表包含“日文”、“罗马字”、“英文”三列。
2.  **设定水平**：要求使用适合初学者的 `JLPT N5` 级别日语。
3.  **字符显示**：要求除词汇表外，不使用罗马字显示日文。
4.  **防止作弊**：强调即使学生直接索要答案，也只能提供线索，不能给出最终翻译。
5.  **提供示例**：这是最关键的一步。我们提供了一个“坏例子”（得分4/10）和一个“好例子”（得分10/10），并详细说明了得分理由。

**好例子**的特点包括：
*   开篇简洁，直接展示词汇表。
*   词汇表格式正确，包含日文字符。
*   提供的句子结构是概念性的（例如：`[主题] は [地点] に います/あります`），而非具体单词。
*   给出的线索是关于时态、助词选择的思考方向，而不是直接答案。

![](img/af24677c6bf72a82b6436026f1767f2a_77.png)

![](img/af24677c6bf72a82b6436026f1767f2a_79.png)

**坏例子**的问题包括：
*   开头有冗余的引导句。
*   词汇表缺少日文字符。
*   在线索中提供了动词的变形形式。
*   句子结构描述不佳。

![](img/af24677c6bf72a82b6436026f1767f2a_81.png)

![](img/af24677c6bf72a82b6436026f1767f2a_83.png)

通过提供这些对比鲜明的示例，我们让AI更清晰地理解了我们的期望。

![](img/af24677c6bf72a82b6436026f1767f2a_85.png)

![](img/af24677c6bf72a82b6436026f1767f2a_87.png)

## 最终测试与总结

在应用了包含好坏示例的提示词后，我们更换了学生输入的句子进行最终测试。

![](img/af24677c6bf72a82b6436026f1767f2a_89.png)

![](img/af24677c6bf72a82b6436026f1767f2a_91.png)

**输入**：`Did you see the raven this morning? They were looking at our garden.`

![](img/af24677c6bf72a82b6436026f1767f2a_93.png)

**输出结果**：
AI给出了一个清晰的词汇表（日文、罗马字、英文），并提供了一个概念性的句子结构框架。它随后给出了一系列引导性问题作为线索，例如询问关于过去时态的表达、描述位置的助词等，完美地避免了直接给出答案。

本节课中，我们一起学习了如何为Meta AI（Llama 3模型）迭代式地构建一个有效的提示词。我们从简单的角色设定开始，通过分析输出结果，逐步增加了格式、内容、难度级别等多重约束。最关键的一步是引入了**好坏示例对比**的方法，这极大地提升了AI输出结果的质量和稳定性。

![](img/af24677c6bf72a82b6436026f1767f2a_95.png)

![](img/af24677c6bf72a82b6436026f1767f2a_96.png)

![](img/af24677c6bf72a82b6436026f1767f2a_97.png)

最终，我们得到了一个能有效扮演日语老师角色的提示词，它能够引导学生主动思考，而不是简单地提供答案。这个练习展示了提示词工程的核心：通过清晰的指令和示例，引导大语言模型生成符合特定场景和目标的输出。在接下来的课程中，我们将把这种方法应用到其他AI模型上。