# 076：从人类反馈中学习总结（论文解读）📚

## 概述

在本节课中，我们将一起学习OpenAI的一篇重要论文《Learning to Summarize from Human Feedback》。这篇论文探讨了如何利用人类反馈来训练模型，使其能够生成更高质量、更符合人类偏好的文本摘要。我们将从摘要任务的基本概念入手，分析传统方法的局限性，并详细解读论文提出的基于人类反馈的训练框架。

---

## 什么是文本摘要？✍️

文本摘要任务的目标是，将一篇较长的文章压缩成一段简短的文字，同时尽可能保留原文的核心信息。

![](img/3346172f219bbd96b427e9a17dd1f8d2_1.png)

例如，考虑以下一段来自Reddit论坛的求助帖：

![](img/3346172f219bbd96b427e9a17dd1f8d2_3.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_5.png)

> “High ratedit， my boyfriend and I have been dating for a year， and it has been great。 except for one thing， dota。The other day on a Saturday， I was over and he was playing a game。 I thought it would just be one， But instead， he proceeded to play for three hours as I just SAT there。 What can I do”

人类可以轻松地为其撰写摘要。以下是一个人类撰写的参考摘要：

> “My boyfriend games whenever he can。 how can I get him to stop gaming so much and focus more on school and our relationship？”

这个摘要准确地捕捉了原文的核心矛盾：男友因沉迷游戏而忽视了学业和关系，以及发帖人寻求建议的意图。

![](img/3346172f219bbd96b427e9a17dd1f8d2_7.png)

---

![](img/3346172f219bbd96b427e9a17dd1f8d2_9.png)

## 传统摘要方法及其局限

上一节我们介绍了摘要任务的目标。在机器学习中，完成此任务最简单的方法是**抽取式摘要**。

以下是抽取式摘要的基本思路：

![](img/3346172f219bbd96b427e9a17dd1f8d2_11.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_12.png)

*   **定义**：模型从原文中严格选取一个或多个连续的片段（子句或整句），将它们组合起来作为摘要。
*   **示例**：一个简单的抽取式模型可能直接选取原文的标题作为摘要，例如：“help。 my my boyfriend is neglecting his studies and our relationship because of a video game。”
*   **局限性**：这种方法无法生成原文中没有出现的新表述，也难以进行信息整合与意图推断。

更复杂的模型，如“Lead-2”模型，会抽取文章的前两句。对于我们的例子，摘要可能是：“Hi， Redit， my boyfriend and I have been dating for a year Indeed has been great。”

然而，生成一个优秀的摘要非常困难。模型不仅需要理解文本中的信息，还需要理解发帖人的**意图**。例如，原文中并未直接说“我该如何让他停止”，但人类能理解这是发帖人的潜在诉求。一个好的摘要模型需要具备这种深层的语义理解和信息压缩能力。

![](img/3346172f219bbd96b427e9a17dd1f8d2_14.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_15.png)

---

![](img/3346172f219bbd96b427e9a17dd1f8d2_16.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_17.png)

## 论文提出的解决方案：基于人类反馈的模型

面对传统方法的局限，OpenAI的研究人员提出了一种利用**人类反馈**来训练摘要模型的新方法。他们的最佳模型是一个拥有67亿参数的GPT风格模型。

该模型能够生成如下摘要：
> “My boyfriend is neglecting his studies and our relationship because of his excessive gaming of a video game What can I do to get him to stop”

这个摘要不仅融合了原文信息，还推断出了“我该如何让他停止”这一隐含意图，质量显著高于简单的抽取式摘要。

接下来，我们将深入探讨他们如何构建这个模型，以及人类反馈在其中扮演的关键角色。

---

![](img/3346172f219bbd96b427e9a17dd1f8d2_19.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_20.png)

## 为什么需要人类反馈？🤔

摘要不是一个有明确对错标签的简单任务。我们很难像图像分类那样，给模型一个绝对正确的“答案”进行学习。

![](img/3346172f219bbd96b427e9a17dd1f8d2_22.png)

传统上，研究人员会构建这样的数据集：将一篇文章交给多位人类标注员，让他们各自撰写摘要。这样，一篇文章就对应多个可能不同的“参考答案”。然后，使用自动评估指标（如**ROUGE**）来衡量机器生成的摘要与这些人类摘要的相似度。

![](img/3346172f219bbd96b427e9a17dd1f8d2_23.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_24.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_25.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_26.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_27.png)

**ROUGE指标**的核心思想是计算**N-gram重叠度**。例如，它会计机器摘要和人类摘要之间有多少个相同的单词（unigram）、双词短语（bigram）或最长公共子序列。

![](img/3346172f219bbd96b427e9a17dd1f8d2_28.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_29.png)

然而，语言的丰富性使得这种基于表面文字重叠的评估方式存在很大问题。ROUGE指标在区分“差摘要”和“好摘要”时可能有效，但在区分“好摘要”和“优秀摘要”时，其判别力会迅速下降。随着摘要质量的提升，ROUGE分数与人类真实评价之间的相关性会减弱。

![](img/3346172f219bbd96b427e9a17dd1f8d2_31.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_32.png)

因此，直接优化ROUGE分数无法让模型学会生成真正令人类满意的高质量摘要。这就是论文引入直接人类反馈的根本原因。

---

![](img/3346172f219bbd96b427e9a17dd1f8d2_34.png)

## 总结

本节课我们一起学习了OpenAI论文《Learning to Summarize from Human Feedback》的核心内容。

![](img/3346172f219bbd96b427e9a17dd1f8d2_36.png)

我们首先明确了文本摘要任务的目标与挑战。接着，分析了传统的抽取式摘要方法和基于ROUGE自动评估指标的局限性，指出它们难以衡量摘要的深层语义质量和人类偏好。

![](img/3346172f219bbd96b427e9a17dd1f8d2_38.png)

![](img/3346172f219bbd96b427e9a17dd1f8d2_40.png)

最后，我们介绍了论文提出的创新解决方案：利用**人类反馈**直接训练模型。这种方法绕过了不完美的自动评估指标，让模型从人类的直接评判中学习，从而生成更高质量、更符合人类理解和偏好的文本摘要。这篇论文为后续基于人类反馈的强化学习（RLHF）在大语言模型训练中的应用奠定了重要基础。