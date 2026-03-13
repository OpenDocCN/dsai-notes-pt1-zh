# 004：数据泄露与毒性检测 🛡️

![](img/bcc9159ebf7d99d4996e39f7204ddf80_1.png)

在本节课中，我们将学习如何检测数据泄露。数据泄露是指私人数据出现在提示词或大语言模型的响应中。我们将从简单的指标开始，逐步介绍最先进的方法。

## 概述

数据泄露与上一课讨论的幻觉不同，它主要是一个安全问题。我们将探讨三种与大语言模型相关的数据泄露场景，并学习使用正则表达式、实体识别和毒性模型等工具来检测它们。

## 数据泄露场景

上一节我们介绍了数据泄露的概念，本节中我们来看看三种具体的泄露场景。

以下是三种主要的数据泄露场景：

1.  **用户提示词中包含个人身份信息**：用户在向模型提问时，无意或有意地输入了个人身份信息或机密信息。
2.  **模型响应中包含个人身份信息**：模型在回答时，输出了其训练数据中记忆的个人身份信息或机密信息。例如，当询问一种罕见疾病时，模型可能泄露了训练数据中某位患者的姓名。
3.  **测试数据泄露到训练数据中**：由于许多大语言模型的训练数据不透明，我们用于测试模型的数据可能已经包含在其训练集中，这会使得评估模型泛化能力和准确性的测试失效。

本节课我们将重点探讨前两种场景，并通过实例数据中的提示词和响应来观察它们。

## 初始设置与数据导入

在开始检测之前，我们需要进行一些准备工作。

首先，导入必要的库并设置pandas以便更好地查看数据。

```python
import pandas as pd
import whylogs
from helper_functions import *
```

接下来，导入我们的示例数据集。

```python
data = load_dataset()
```

![](img/bcc9159ebf7d99d4996e39f7204ddf80_3.png)

现在，我们可以查看一个数据泄露的示例。

## 使用正则表达式进行模式匹配

检测数据泄露的一个强大工具是正则表达式。我们可以通过定义特定模式来查找文本中的电子邮件地址、社会安全号码等信息。

我们将首先使用`whylogs`库中的`Regexes`模块来实现。

```python
from whylogs.core.validators import Regexes

# 定义并应用正则表达式模式
patterns = {
    "email": r"\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b",
    "ssn": r"\b\d{3}-\d{2}-\d{4}\b",
    # ... 其他模式
}
```

我们可以使用这些模式来扫描提示词和响应。例如，在提示词中发现了2个电子邮件地址和1个社会安全号码。

为了系统化地评估，我们使用`UDF Schema`将定义好的指标应用到数据集上，逐行标注数据。

```python
from whylogs.core.udf_schema import udf_schema

# 创建包含模式检测指标的schema
schema = udf_schema()
annotated_chats = data.with_columns(schema.apply(data))
```

然后，我们可以过滤出提示词和响应中均检测到模式的行，这些行可能存在数据泄露问题。

```python
leakage_candidates = annotated_chats.filter(
    (pl.col("prompt_has_patterns").is_not_null()) &
    (pl.col("response_has_patterns").is_not_null())
)
```

使用评估辅助函数进行评估后，我们发现简单的正则表达式规则能捕捉到一些基础的数据泄露案例，但也会产生误报，并且难以处理更复杂的情况。

## 使用实体识别检测机密信息

虽然正则表达式对检测标准格式的个人身份信息很有帮助，但对于产品名、员工名、项目名等非标准格式的机密信息，我们需要更强大的工具。

实体识别任务可以将文本中的单词或短语标记为特定类型的实体（如人物、地点、组织）。

我们将使用`spaCy`库的预训练模型来进行实体识别。

```python
import spacy

# 加载预训练的实体识别模型（例如‘en_core_web_trf’）
entity_model = spacy.load("en_core_web_trf")
```

定义我们认为可能泄露的实体类型，例如`PERSON`（人物）、`PRODUCT`（产品）、`ORG`（组织）。

```python
CONFIDENTIAL_ENTITIES = {"PERSON", "PRODUCT", "ORG"}
```

接下来，创建一个基于实体模型的评估指标函数。

```python
from whylogs.core.udf_schema import register_metric_udf

@register_metric_udf(["prompt"])
def prompt_entity_leakage(text: str) -> int:
    doc = entity_model(text)
    for ent in doc.ents:
        if ent.label_ in CONFIDENTIAL_ENTITIES:
            return 1
    return 0

![](img/bcc9159ebf7d99d4996e39f7204ddf80_5.png)

# 为响应创建类似的指标
@register_metric_udf(["response"])
def response_entity_leakage(text: str) -> int:
    # ... 实现逻辑与上面类似
    pass
```

将这些新指标应用到数据上并过滤结果，我们可以发现更多潜在的泄露点，例如模型响应中出现了被标记为`PRODUCT`的专有技术名词。

结合正则表达式和实体识别的结果，我们能构建更健壮的检测机制。

```python
advanced_leakage_filter = (
    (pl.col("prompt_has_patterns").is_not_null()) |
    (pl.col("response_has_patterns").is_not_null()) |
    (pl.col("prompt_entity_leakage") == 1) |
    (pl.col("response_entity_leakage") == 1)
)
```

## 关于毒性检测的补充

数据泄露和毒性有相似之处，都涉及我们不希望出现在模型输出中的训练数据内容。毒性检测主要关注有害、冒犯性或带有偏见的内容。

毒性分为两种：
*   **显性毒性**：文本中包含明显的脏话、侮辱性词汇或针对特定群体的直接攻击。
*   **隐性毒性**：文本不直接使用冒犯性词汇，但隐含了有害的刻板印象或偏见，例如“那个群体的人都不擅长这个”。

我们可以使用`Hugging Face`的`transformers`库中的预训练模型来检测毒性。例如，基于`toxigen`数据集训练的模型擅长检测隐性毒性。

```python
from transformers import pipeline

# 加载毒性检测模型
toxicity_pipeline = pipeline("text-classification", model="tomh/toxigen_hatebert")
```

创建一个毒性检测指标：

```python
@register_metric_udf(["prompt"])
def prompt_implicit_toxicity(text: str) -> int:
    result = toxicity_pipeline(text)[0]
    # 假设标签‘1’代表有毒，‘0’代表无毒
    return int(result['label'] == 'LABEL_1')
```

应用此指标可以帮助我们发现那些看似无害但可能隐含偏见的提示词或响应。需要注意的是，这类细微的检测可能会产生较多误报，需要仔细调整阈值和规则。

## 总结

本节课中我们一起学习了如何检测大语言模型应用中的数据泄露问题。

我们首先了解了数据泄露的三种主要场景。接着，从使用**正则表达式**进行基础模式匹配开始，检测如邮箱、电话等格式固定的信息。然后，引入了更强大的**实体识别**技术，用于发现产品名、人名等非标准格式的机密信息。最后，补充介绍了**毒性检测**的概念和方法，包括显性毒性和隐性毒性的区别，以及如何使用预训练模型来识别有害内容。

![](img/bcc9159ebf7d99d4996e39f7204ddf80_7.png)

通过结合多种方法，我们可以构建更全面、更有效的监控体系，以保障LLM应用程序的质量与安全性。在下一节课中，我们将探讨“拒绝回答”和“提示词注入”这两个重要的安全话题。