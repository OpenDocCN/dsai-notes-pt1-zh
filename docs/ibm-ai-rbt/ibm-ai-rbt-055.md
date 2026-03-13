# 055：3_使用实体 🧠

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_0.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_1.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_3.png)

在本节课中，我们将学习聊天机器人构建的第三个核心概念：**实体**。实体是对话技能的关键组成部分，用于从用户的话语中提取具体的、结构化的信息。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_4.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_5.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_7.png)

上一节我们介绍了**意图**，它帮助我们理解用户的目标。本节中我们来看看**实体**，它使我们能够捕获用户话语中的具体细节，从而使机器人的回应更加精准和个性化。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_9.png)

## 什么是实体？

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_11.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_12.png)

实体允许我们从用户的话语中捕获特定的值。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_13.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_15.png)

您可能还记得上一模块中的这个例子。用户询问“你们的多伦多店什么时候开门？”和“你们的温哥华店什么时候开门？”。您会注意到，聊天机器人正确地检测到了意图（询问营业时间），但完全忽略了问题的具体细节。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_16.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_17.png)

换句话说，对于我们的聊天机器人来说，“你们的多伦多店什么时候开门”和“你们的温哥华店什么时候开门”是完全相同的。这是因为我们尚未定义一个实体来捕获那个特定的信息位，即商店的位置。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_19.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_20.png)

## 如何定义实体？

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_22.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_23.png)

我们可以创建一个位置实体来解决这个问题。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_25.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_27.png)

请注意，我们使用 `@` 符号而不是 `#` 符号来标识实体。然后，我们可以为实体定义多个值，例如 Toronto、Montreal、Vancouver 等，每个值对应我们拥有的一个商店位置。

您会注意到，我们还可以为给定的实体值定义**同义词**。这些可以是字典中的同义词，但更多时候是指用户可能以那种方式指代该实体值。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_29.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_30.png)

例如，假设我们的多伦多店位于 Warden Avenue。人们可能会询问我们多伦多店的营业时间，或者他们可能会表述为“你们 Warden Avenue 店的营业时间是几点？”。这两种表述都应该引导我们的聊天机器人检测到相同的实体值。同义词是可选的，但这是一个有用的功能，在适用时值得定义。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_31.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_32.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_33.png)

## 实体的语法细节

定义了位置实体后，我们现在可以同时检测意图和具体位置。有了这两条信息，我们就可以为用户提供恰当且具体的答案。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_35.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_36.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_37.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_38.png)

请注意，虽然实体值本身可以包含空格，但如果实体值中包含空格，我们需要一种特殊的语法来表示它。我们只需将其用括号 `()` 括起来，以便在后续定义对话规则时引用它。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_39.png)

## 实体的创建与管理

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_41.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_42.png)

与意图类似，我们也可以从 CSV 文件导入实体。事实上，在本模块的实验中，您将以这种方式导入它们，并学习如何将它们导出到 CSV。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_44.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_45.png)

除了在 Watson Assistant 中手动输入和从 CSV 导入之外，还有第三种方法可以为聊天机器人添加实体。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_47.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_49.png)

实际上，有一些预定义的**系统实体**，如果您的聊天机器人需要，可以启用它们。正如您所见，有相当多的选项。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_51.png)

以下是几个关键的系统实体：

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_53.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_54.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_55.png)

*   **`@sys-currency`**：允许我们检测用户输入中提及的货币。
*   **`@sys-date`**：检测日期提及。例如，用户可能用“下周一”来回答问题，`@sys-date` 将允许我们将该信息捕获为实际的特定日期。
*   **`@sys-person`**：允许我们检测常见姓名，这是收集人名以使聊天机器人更具个性化的一种简单方法，您将在课程后面看到。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_57.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_58.png)

系统实体非常方便，并且正在不断改进。在直接开始实现自己的实体之前，请务必检查您的对话技能中可用的系统实体。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_60.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_62.png)

## 实践与总结

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_64.png)

好了，现在轮到您了。到现在为止，您应该熟悉流程了。在本模块的其余部分，您将找到一系列指导练习，教您如何使用实体。您还会发现一些分级测验。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_66.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_67.png)

在下一个模块中，我们将看到如何将我们定义的意图和实体很好地利用起来，为用户提供准确和具体的回应。

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_69.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_70.png)

![](img/61c4cf3aca6cd44cb9b73c2ab869f8a1_71.png)

本节课中我们一起学习了**实体**的概念、创建方法（包括自定义实体和系统实体）以及其在构建精准对话机器人中的关键作用。实体使我们能够从用户输入中提取具体信息，是实现个性化交互的基础。