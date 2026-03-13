# 007：评估与迭代 🧪

![](img/313ea25cd482ecc0b7f61a44d30f776b_0.png)

在本节课中，我们将学习如何评估微调后的大语言模型，并了解迭代改进的重要性。评估生成式模型是一项具有挑战性的任务，我们将探讨多种评估方法，包括人工评估、基准测试和错误分析。

## 评估的重要性与挑战

模型训练完成后，下一步是进行评估。这一步至关重要，因为人工智能的发展依赖于迭代，评估结果将帮助你持续改进模型。

![](img/313ea25cd482ecc0b7f61a44d30f776b_2.png)

评估生成式模型非常困难。没有明确的衡量标准，且随着模型性能的快速提升，度量标准本身也难以跟上发展。因此，**人工评估**往往是最可靠的方式，即让熟悉该领域的专家来评估模型的输出。

一个高质量的**测试数据集**对于有效利用专家时间极其重要。这意味着数据集需要：
*   **准确**：经过检查，确保内容正确。
*   **泛化**：涵盖大量不同的测试用例。
*   **独立**：确保数据在训练集中未曾出现。

## 主流评估方法

![](img/313ea25cd482ecc0b7f61a44d30f776b_4.png)

以下是几种主流的模型评估与比较方法。

### ELO 排名比较 🏆

![](img/313ea25cd482ecc0b7f61a44d30f776b_6.png)

这是一种流行的比较方法，类似于在多模型间进行A/B测试或锦标赛。ELO排名最初用于国际象棋，它能有效区分不同模型的性能优劣。

![](img/313ea25cd482ecc0b7f61a44d30f776b_8.png)

### 开放LLM基准测试

![](img/313ea25cd482ecc0b7f61a44d30f776b_10.png)

一个常见的开放LLM基准测试是**LMSys Chatbot Arena**。它采用一系列不同的评估方法，将结果平均后对模型进行排名。

这个基准由LMSys组织创建，它整合了多个学术基准：
*   **ARC**：一套小学水平的问题集。
*   **HellaSwag**：对常识推理能力的测试。
*   **MMLU**：涵盖多种小学科目。
*   **TruthfulQA**：衡量模型复现网络上常见错误信息（谎言）的能力。

![](img/313ea25cd482ecc0b7f61a44d30f776b_12.png)

这些基准被研究人员广泛使用。模型排名会持续更新，例如在录制本课程时，Llama 2表现优异，而使用Orca方法在Llama上微调的**Free Willy**模型也取得了良好成绩。

## 错误分析框架 🔍

![](img/313ea25cd482ecc0b7f61a44d30f776b_14.png)

分析和评估模型的另一个重要框架是**错误分析**，即对模型产生的错误进行分类，以理解常见的错误类型。一个关键优势是，在微调**之前**就可以对基础模型进行错误分析，这有助于理解其工作方式，并确定哪些数据能通过微调带来最大提升。

![](img/313ea25cd482ecc0b7f61a44d30f776b_16.png)

以下是一些常见的错误类别：

![](img/313ea25cd482ecc0b7f61a44d30f776b_18.png)

![](img/313ea25cd482ecc0b7f61a44d30f776b_20.png)

*   **拼写错误**：模型输出中存在简单的拼写错误。例如，将“杠杆”误写为“肝脏”。解决方法是在数据集中修正此类样本。
*   **冗长**：生成式模型通常倾向于给出冗长的回答。解决方法包括确保训练数据包含简洁的回答示例，或在提示模板中更明确地使用停止标记。
*   **重复**：模型输出可能包含大量重复内容。除了使用停止标记，还需确保数据集中包含多样性充足、重复较少的示例。

![](img/313ea25cd482ecc0b7f61a44d30f776b_22.png)

![](img/313ea25cd482ecc0b7f61a44d30f776b_24.png)

## 实践：运行评估

![](img/313ea25cd482ecc0b7f61a44d30f776b_26.png)

现在，我们将进入实践环节，在测试数据集上运行模型，并尝试几种评估指标。

首先，加载测试数据集并查看一个数据样本。

```python
# 打印一个问题-答案对
print(“问题：“, test_dataset[0][‘question’])
print(“答案：“, test_dataset[0][‘answer’])
```

![](img/313ea25cd482ecc0b7f61a44d30f776b_28.png)

![](img/313ea25cd482ecc0b7f61a44d30f776b_30.png)

接着，加载微调后的模型。我们将从Hugging Face获取之前微调好的模型。

![](img/313ea25cd482ecc0b7f61a44d30f776b_32.png)

![](img/313ea25cd482ecc0b7f61a44d30f776b_34.png)

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
model_name = “your-finetuned-model” # 替换为你的模型名称
model = AutoModelForCausalLM.from_pretrained(model_name)
tokenizer = AutoTokenizer.from_pretrained(model_name)
```

![](img/313ea25cd482ecc0b7f61a44d30f776b_36.png)

在评估模式下运行模型时，一个重要步骤是调用 `model.eval()`，以确保禁用Dropout等仅在训练时使用的功能。

![](img/313ea25cd482ecc0b7f61a44d30f776b_38.png)

```python
model.eval()
```

![](img/313ea25cd482ecc0b7f61a44d30f776b_40.png)

然后，可以运行推断函数来生成输出。我们将定义一个简单的评估指标：**精确匹配**，即比较生成的字符串与标准答案字符串是否完全一致（忽略空白字符）。

![](img/313ea25cd482ecc0b7f61a44d30f776b_42.png)

```python
def exact_match(prediction, target):
    return prediction.strip() == target.strip()
```

![](img/313ea25cd482ecc0b7f61a44d30f776b_44.png)

![](img/313ea25cd482ecc0b7f61a44d30f776b_46.png)

需要注意的是，对于创造性写作任务，存在许多可能的正确答案，因此“精确匹配”并不是一个高效的指标。它更适用于信息提取或分类性质的任务。

![](img/313ea25cd482ecc0b7f61a44d30f776b_48.png)

除了精确匹配，还有其他评估方法：
*   **使用另一个LLM进行评分**：将输出和答案交给另一个LLM，让其评估相似度。
*   **使用嵌入计算相似度**：分别对标准答案和生成答案进行嵌入，然后计算它们在高维空间中的距离（如余弦相似度）。

![](img/313ea25cd482ecc0b7f61a44d30f776b_50.png)

现在，让我们在整个测试集的一个子集上运行评估。

![](img/313ea25cd482ecc0b7f61a44d30f776b_52.png)

![](img/313ea25cd482ecc0b7f61a44d30f776b_54.png)

```python
import pandas as pd
results = []
for i in range(10): # 为了节省时间，只评估前10个样本
    question = test_dataset[i][‘question’]
    target_answer = test_dataset[i][‘answer’]
    predicted_answer = generate_answer(question, model, tokenizer) # 假设的生成函数
    is_exact = exact_match(predicted_answer, target_answer)
    results.append({‘question’: question, ‘target’: target_answer, ‘prediction’: predicted_answer, ‘exact_match’: is_exact})
df = pd.DataFrame(results)
print(f“精确匹配的数量：{df[‘exact_match’].sum()}”)
```

![](img/313ea25cd482ecc0b7f61a44d30f776b_56.png)

评估结果可能显示精确匹配数为零，这对于创造性任务并不意外。归根结底，更有效的方法是在精心策划的测试集上进行**人工检查**。你可以查看生成的数据框，逐一对比预测答案与目标答案的接近程度。

![](img/313ea25cd482ecc0b7f61a44d30f776b_58.png)

![](img/313ea25cd482ecc0b7f61a44d30f776b_59.png)

## 运行学术基准测试

最后，我们可以运行像**ARC**这样的学术基准测试。这个基准测试主要包含小学科学问题。

![](img/313ea25cd482ecc0b7f61a44d30f776b_61.png)

```python
# 假设使用evaluate库运行ARC基准
from evaluate import load
arc = load(‘arc’)
# 在模型上运行评估（此处为示意，实际调用需适配）
# scores = arc.compute(model=model, tokenizer=tokenizer)
```

![](img/313ea25cd482ecc0b7f61a44d30f776b_63.png)

![](img/313ea25cd482ecc0b7f61a44d30f776b_65.png)

运行后可能会得到一个分数。但需要强调的是，**不应过分关注模型在这些通用基准上的性能**。人们用这些基准给模型排名，主要是为了比较通用模型的能力。

![](img/313ea25cd482ecc0b7f61a44d30f776b_67.png)

然而，微调模型的目的是为特定用例量身定制。因此，评估的重点应始终放在与**你的业务目标**和**最终用例**相关的任务上。除非你微调模型的目的就是回答小学科学问题，否则ARC等基准的分数对你的实际应用参考价值有限。

![](img/313ea25cd482ecc0b7f61a44d30f776b_69.png)

## 课程总结

![](img/313ea25cd482ecc0b7f61a44d30f776b_71.png)

本节课中，我们一起学习了如何评估微调后的大语言模型。

我们首先了解了评估生成式模型的挑战，认识到人工评估和高质量测试集的重要性。接着，我们探讨了ELO比较和LMSys Chatbot Arena等开放基准测试。然后，我们介绍了错误分析框架，用于在微调前后系统性地识别和改进模型缺陷。

在实践部分，我们学习了加载模型、在评估模式下运行、以及实施简单的评估指标（如精确匹配）。我们还讨论了更高级的评估方法，如使用另一个LLM评分或嵌入相似度计算。最后，我们运行了ARC学术基准，并重点指出：**评估的核心必须围绕你的特定任务和业务需求展开**，通用基准的分数仅在选择基础模型时有参考意义。

![](img/313ea25cd482ecc0b7f61a44d30f776b_73.png)

通过持续的评估和基于反馈的迭代，你可以逐步提升模型在真实场景中的表现。