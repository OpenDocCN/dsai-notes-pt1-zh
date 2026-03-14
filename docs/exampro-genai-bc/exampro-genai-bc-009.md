# 9：句子构造器 - ChatGPT 🧠

![](img/85a12d86fbe8a068e33e0eeae55ccb73_1.png)

在本节课中，我们将学习如何为ChatGPT设计一个用于日语教学的提示词（Prompt），目标是创建一个能引导学生逐步构造日语句子的AI助手。我们将通过迭代优化，探索不同模型（如GPT-4、GPT-4o-mini）的表现，并学习如何结构化提示词以获得更理想的输出。

![](img/85a12d86fbe8a068e33e0eeae55ccb73_3.png)

---

## 概述与准备工作

![](img/85a12d86fbe8a068e33e0eeae55ccb73_5.png)

上一节我们探讨了Meta AI的提示词设计。本节中，我们来看看如何为ChatGPT设计一个类似的句子构造器提示词。首先，我们需要了解ChatGPT的最佳提示实践。

![](img/85a12d86fbe8a068e33e0eeae55ccb73_7.png)

OpenAI提供了官方的提示工程指南。其中一些核心策略包括：
*   在查询中包含细节以获得更相关的答案。
*   要求模型采用特定角色。
*   使用分隔符来清晰划分输入的不同部分。
*   指定完成任务所需的步骤。
*   提供示例。
*   指定期望的输出长度。

与Meta AI类似，ChatGPT没有强制性的特定格式要求，关键在于提供清晰的指令和上下文。

## 构建初始提示词

我们将从一个基础提示词结构开始。这个结构借鉴了之前的经验，主要包含以下几个部分：

![](img/85a12d86fbe8a068e33e0eeae55ccb73_9.png)

1.  **角色定义**：`# Japanese Language Teacher`
2.  **学生水平**：`## Language Level: Beginner`
3.  **核心教学指令**：`## Teaching Instructions`
4.  **示例**：`## Example`

以下是初始提示词内容：

![](img/85a12d86fbe8a068e33e0eeae55ccb73_11.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_13.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_15.png)

```markdown
# Japanese Language Teacher
## Language Level: Beginner
## Teaching Instructions
Your task is to guide an English-speaking student through transcribing an English sentence into Japanese. Do not provide the full transcription directly. Instead, offer step-by-step instructions to help the student arrive at the answer independently.

When presented with an English sentence:
1.  Provide a vocabulary table containing only the necessary Japanese words from the sentence, in their dictionary form. Do not include particles (e.g., wa, ga, o, ni) in this table.
2.  Provide the sentence structure in English, using placeholders like [Subject], [Object], [Verb]. Do not fill in the Japanese words.
3.  Provide considerations or clues that focus on the grammar points needed (e.g., particle usage, verb tense). Do not give away the answer.

![](img/85a12d86fbe8a068e33e0eeae55ccb73_17.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_19.png)

## Example
**English Sentence:** "The cat sleeps on the sofa."

![](img/85a12d86fbe8a068e33e0eeae55ccb73_21.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_22.png)

**Your Response:**
### Vocabulary
| English | Japanese (Dictionary Form) |
| :--- | :--- |
| cat | neko |
| to sleep | neru |
| sofa | sofā |

### Sentence Structure
[Subject] wa [Location] de [Verb].

![](img/85a12d86fbe8a068e33e0eeae55ccb73_24.png)

### Cues and Considerations
*   The subject "cat" will be marked by the particle `wa`.
*   The location "on the sofa" requires the particle `de`.
*   The verb "sleeps" needs to be conjugated to the present tense.
```

## 第一次测试与问题发现

![](img/85a12d86fbe8a068e33e0eeae55ccb73_26.png)

我们将上述提示词输入ChatGPT（使用GPT-4模型），并给出测试句子：“Did you see the raven this morning? They were looking at our garden.”

![](img/85a12d86fbe8a068e33e0eeae55ccb73_28.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_30.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_32.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_34.png)

模型给出了包含词汇表、句子结构和提示的回复。但我们立刻发现了几个问题：

1.  **词汇重复**：动词“看 (miru)”在词汇表中出现了两次。
2.  **泄露答案**：在“句子结构”部分，模型直接给出了“verb past”这样的时态提示，这与我们“只提供结构框架”的指令相悖。
3.  **结构灵活性**：模型给出的日语句子结构（时间-主语-宾语-动词）与初学者常学的标准结构（时间-地点-主语-宾语-动词）略有不同，虽然这可能是正确的，但可能造成初学者的困惑。

## 迭代优化提示词

针对发现的问题，我们需要优化提示词，给出更明确的指令。

![](img/85a12d86fbe8a068e33e0eeae55ccb73_36.png)

### 优化1：明确输出格式与规则

我们修改了“教学指令”部分，使其更结构化：

![](img/85a12d86fbe8a068e33e0eeae55ccb73_38.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_40.png)

*   **格式化输出**：明确指出回复应包含三个部分：词汇表、句子结构、提示与考量。
*   **禁止时态提示**：在句子结构部分，明确指令“Do not provide tenses or conjugations in the sentence structure”。
*   **解释学生尝试**：增加指令“When the student makes an attempt, interpret their attempt so they can see what they actually said.”，这有助于提供针对性反馈。
*   **精简词汇表**：增加“Ensure there are no repeats in the vocabulary table”和“If there is more than one version of a word, show the simplest or most common example”。

### 优化2：提供更清晰的句子结构示例

我们发现模型有时会给出过于复杂或非常规的句子结构。因此，我们在提示词中加入了针对初学者的标准句子结构示例，例如：
*   `[Time] [Subject] wa [Adjective] desu.`
*   `[Subject] wa [Object] o [Verb].`

## 测试不同模型

![](img/85a12d86fbe8a068e33e0eeae55ccb73_42.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_44.png)

我们使用优化后的提示词测试了不同的ChatGPT模型：

1.  **GPT-4**：能力强大，但有时会“过度思考”，可能忽略部分指令（如直接纠正学生答案），导致输出冗长。
2.  **GPT-4o-mini**：速度更快，有时在遵循基础指令方面表现得更直接、更简洁，输出结果可能更接近我们的初始期望。
3.  **ChatGPT “项目”功能**：我们探索了ChatGPT的“项目”功能，它允许上传文件（如将示例单独存为XML文件）并与提示词结合使用。这有助于管理更复杂的提示和示例库，使主提示词更简洁。

![](img/85a12d86fbe8a068e33e0eeae55ccb73_46.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_48.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_50.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_51.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_52.png)

测试表明，更智能的模型（如GPT-4）并不总是能产出更符合特定指令的结果。根据任务需求选择合适的模型非常重要。

![](img/85a12d86fbe8a068e33e0eeae55ccb73_54.png)

---

![](img/85a12d86fbe8a068e33e0eeae55ccb73_56.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_58.png)

## 总结

![](img/85a12d86fbe8a068e33e0eeae55ccb73_60.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_62.png)

![](img/85a12d86fbe8a068e33e0eeae55ccb73_64.png)

本节课中我们一起学习了为ChatGPT设计日语教学提示词的完整过程。我们从OpenAI的官方指南出发，构建了初始提示词，并通过实际测试发现了词汇重复、答案泄露和结构不一致等问题。通过迭代优化，我们明确了输出格式、禁止了时态泄露、增加了对学生尝试的解释功能，并提供了更清晰的结构示例。最后，通过对比GPT-4和GPT-4o-mini等模型，我们认识到提示词的效果与模型特性密切相关，且ChatGPT的“项目”功能为管理复杂提示提供了有效工具。核心在于通过清晰的指令和持续的测试迭代，逐步引导AI助手的行为符合我们的教学设计目标。