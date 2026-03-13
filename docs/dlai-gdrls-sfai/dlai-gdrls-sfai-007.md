# 007：保持聊天机器人话题不偏离

![](img/233976ccb526499a3b61ece201c0f889_0.png)

## 概述
在本节课中，我们将学习如何构建一个护栏（Guardrail），以确保聊天机器人只讨论与您的用例或业务相关的主题。这有助于防止用户滥用聊天机器人来完成其他任务。

---

上一节我们介绍了如何处理RAG聊天机器人中的幻觉问题。本节中，我们来看看如何确保聊天机器人不偏离预设的话题。

首先，我们将设置一个简单的RAG应用，并演示它如何偏离主题。我们将使用一个披萨店聊天机器人，并让它回答关于福特汽车的问题。

```python
# 设置一个简单的RAG应用
unguarded_client = ...
vector_db = ...
system_message = ...
```

![](img/233976ccb526499a3b61ece201c0f889_2.png)

运行后，聊天机器人给出了关于福特F150和Ranger车型的详细对比列表。对于一个披萨店聊天机器人来说，这显然是不合适的。

---

## 理解零样本主题分类模型

为了创建话题护栏，我们首先需要理解其核心模型。我们将使用一个零样本主题分类模型，具体是Facebook的BART-large-mnli模型。

该模型的工作原理与我们之前课程中见过的自然语言推理模型类似。不同之处在于，我们使用特定的NLI公式：前提是我们的输入文本，假设是“该文本涉及以下主题”。通过判断是“蕴含”还是“矛盾”，我们可以得到文本属于特定主题的可能性。

以下是使用Hugging Face管道创建零样本分类器的示例：

```python
from transformers import pipeline
classifier = pipeline("zero-shot-classification", model="facebook/bart-large-mnli")
```

让我们用一个示例句子测试这个分类器的效果。句子是“Chick-fil-A周日不营业”，我们关心的主题是“食物”、“商业”和“政治”。

```python
result = classifier("Chick-fil-A is closed on Sundays.", candidate_labels=["food", "business", "politics"])
```

结果中，“食物”得分为0.672，“商业”为0.179，“政治”为0.027。这符合我们对句子主题的预期。

这个零样本主题分类器构成了我们话题护栏的核心。

---

## 比较零样本分类器与大语言模型

在深入之前，让我们比较一下零样本分类器与大语言模型在分类任务上的表现。

您可以使用LLM进行分类，尤其是在概念验证阶段，这可能是一个不错的选择。我们使用GPT-4o-mini进行了测试。

以下是使用LLM进行分类可能遇到的问题：
*   **随机性**：LLM是随机的，每次运行可能得到不同的答案。
*   **模型依赖性**：分类质量取决于所使用的模型，通常更大的模型效果更好。
*   **外部约束**：应用受限于第三方服务器的速率限制、正常运行时间等。在我们的测试中，运行10次请求大约需要30秒，且会因流量和服务条款变化而异。

相比之下，我们的零样本分类器：
*   **速度快**：在配备M1芯片的MacBook Pro上，10次迭代仅需几秒钟。
*   **结果一致**：每次运行结果完全相同，这对于可靠性很重要。
*   **数据隐私**：您可以在服务器或私有环境中运行这个小模型，无需将用户消息发送给第三方，确保数据安全。

---

## 创建话题检测函数

理解了分类器的工作原理后，让我们开始创建话题护栏。第一步是实现一个检测主题的函数。

我们将使用刚才用过的分类器。这个函数接收文本、主题列表和阈值分数，然后返回检测到的主题列表（每个主题的分数必须高于阈值）。

```python
def detect_topics(text, topics, threshold):
    # 使用零样本分类器对文本进行分类
    result = classifier(text, candidate_labels=topics)
    # 筛选出分数高于阈值的主题
    detected_topics = [label for label, score in zip(result['labels'], result['scores']) if score > threshold]
    return detected_topics
```

---

## 实现话题验证器

![](img/233976ccb526499a3b61ece201c0f889_4.png)

有了话题检测函数，我们现在用它来实现一个验证器，我们称之为“受限话题验证器”。

![](img/233976ccb526499a3b61ece201c0f889_6.png)

这个验证器接收一个禁止话题列表和一个阈值。在验证函数中，我们调用之前实现的`detect_topics`函数。如果检测到任何禁止话题，则引发失败结果；否则，引发通过结果。

```python
class ConstrainedTopicValidator:
    def __init__(self, banned_topics, threshold):
        self.banned_topics = banned_topics
        self.threshold = threshold

    def validate(self, text):
        detected = detect_topics(text, self.banned_topics, self.threshold)
        if detected:
            raise ValidationError(f"检测到禁止话题: {detected}")
        else:
            return PassResult()
```

这与我们之前课程中的做法非常相似。

---

## 构建并测试话题护栏

最后，使用我们刚刚创建的验证器来构建一个护栏，并在一些示例上进行测试。

以下是初始化护栏的代码：

```python
guard = Guard().use(ConstrainedTopicValidator(["politics", "automobiles"], threshold=0.5))
```

现在，让我们在一个明显涉及政治的语句上运行这个护栏。

```python
try:
    guard.validate("The government announced new economic policies today.")
except ValidationError as e:
    print(f"护栏触发: {e}")
```

如我们所料，护栏失败了，因为检测到了“政治”话题，而“汽车”话题未被检测到。

---

## 在护栏服务器中使用先进的话题分类器

现在，我们已经知道如何创建话题护栏。接下来，让我们尝试在护栏服务器中使用一个先进的话题分类器，看看是否能防止之前聊天机器人中出现的话题偏离问题。

为了使用先进的话题分类器，您需要从Guardrails Hub下载并安装特定的护栏到本地环境。在学习环境中，我们已经为您完成了这一步。

和之前一样，我们将使用服务器，并创建一个使用该话题护栏的受保护聊天机器人。

```python
# 创建受保护的客户端，它是在OpenAI客户端和运行话题分类器的护栏之间的一层
guarded_client = ...
guarded_chatbot = ...
```

现在，使用这个受保护的聊天机器人，看看我们能否让披萨店聊天机器人谈论福特汽车。

```python
response = guarded_chatbot.chat("Tell me about the differences between a Ford F150 and a Ranger.")
```

正如预期的那样，由于输出内容涉及福特F150卡车（这是我们的禁止话题），我们最终得到了一个验证错误，而不是将原始输出返回给用户。

同样，您不必使用完全相同的验证错误消息。您可以像在之前的课程中看到的那样捕获这个异常，并进行更优雅的错误处理，以告知用户不能与聊天机器人讨论这些话题。

---

![](img/233976ccb526499a3b61ece201c0f889_8.png)

## 总结
本节课中，我们一起学习了如何构建话题护栏来确保聊天机器人不偏离预设主题。我们了解了零样本主题分类模型的工作原理，并将其与大语言模型分类进行了比较。我们实现了话题检测函数和验证器，并最终构建了一个有效的话题护栏。在下一节课中，我们将学习如何处理个人身份信息泄露的问题。