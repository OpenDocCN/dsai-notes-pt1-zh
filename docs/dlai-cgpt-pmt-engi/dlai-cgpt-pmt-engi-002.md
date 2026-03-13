# 002：第2集 📝 提示指南

![](img/12f51ab2f1bd161e43ce0f3df4918c45_0.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_2.png)

在本节课中，我们将学习如何编写有效的提示词，以引导大型语言模型（如ChatGPT）生成更准确、更符合预期的输出。课程将围绕两个核心原则展开，并通过具体策略和代码示例进行讲解。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_4.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_6.png)

---

![](img/12f51ab2f1bd161e43ce0f3df4918c45_8.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_10.png)

## 原则一：编写明确且具体的指令 ✍️

![](img/12f51ab2f1bd161e43ce0f3df4918c45_12.png)

第一个核心原则是向模型提供**明确且具体**的指令。清晰的指令能引导模型产生所需的输出，并减少无关或不正确答案的出现。请注意，清晰的指令不一定是简短的指令，有时更长的提示能提供更多上下文，从而获得更详细和相关的输出。

以下是实现这一原则的四个具体策略。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_14.png)

### 策略一：使用分隔符

![](img/12f51ab2f1bd161e43ce0f3df4918c45_16.png)

使用分隔符可以清晰地标示出输入文本的不同部分，帮助模型准确理解需要处理的内容。分隔符可以是三重反引号、引号、XML标签等任何清晰的标点符号。

以下是使用分隔符总结文本的示例：

![](img/12f51ab2f1bd161e43ce0f3df4918c45_18.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_20.png)

```python
text = """在这里插入需要总结的文本内容。"""
prompt = f"""
请总结由三重反引号分隔的文本。
用一句话完成总结。
```{text}```
"""
response = get_completion(prompt)
print(response)
```

![](img/12f51ab2f1bd161e43ce0f3df4918c45_22.png)

使用分隔符还能有效防止“提示注入”（Prompt Injection），即用户输入可能包含与你的指令相矛盾的指令。通过分隔符，模型能明确区分指令和待处理的文本。

### 策略二：要求结构化输出

![](img/12f51ab2f1bd161e43ce0f3df4918c45_24.png)

为了便于后续处理，可以要求模型以JSON或HTML等结构化格式输出结果。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_26.png)

以下是一个要求JSON格式输出的示例：

![](img/12f51ab2f1bd161e43ce0f3df4918c45_28.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_30.png)

```python
prompt = """
请生成包含三本虚构书籍的列表。
每本书需包含书名、作者和体裁。
以JSON格式输出，并包含以下键：book_id, title, author, genre。
"""
response = get_completion(prompt)
print(response)
```

![](img/12f51ab2f1bd161e43ce0f3df4918c45_32.png)

这样，输出结果可以直接被Python读入字典或列表，便于程序化处理。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_34.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_35.png)

### 策略三：要求模型检查条件

![](img/12f51ab2f1bd161e43ce0f3df4918c45_37.png)

如果任务基于某些可能不成立的假设，可以指示模型先检查这些条件。如果条件不满足，则停止执行任务或进行相应处理。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_39.png)

以下是指示模型检查文本是否包含指令的示例：

![](img/12f51ab2f1bd161e43ce0f3df4918c45_41.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_43.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_45.png)

```python
text_1 = """泡一杯茶需要以下步骤：1. 烧水。 2. 放入茶叶。 3. 等待片刻。 4. 享用。"""
prompt = f"""
您将获得由三引号分隔的文本。
如果文本包含一系列指令，请按以下格式重写这些指令：
步骤 1 - ...
步骤 2 - ...
...
如果文本不包含指令，则直接输出“未提供步骤”。
\"\"\"{text_1}\"\"\"
"""
response = get_completion(prompt)
print("文本1的检查结果：", response)
```

![](img/12f51ab2f1bd161e43ce0f3df4918c45_47.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_49.png)

### 策略四：使用少量示例提示（Few-shot Prompting）

![](img/12f51ab2f1bd161e43ce0f3df4918c45_51.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_53.png)

通过提供少量成功执行任务的示例，可以引导模型以一致的风格完成新任务。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_55.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_57.png)

以下是一个使用示例引导模型回答风格的示例：

![](img/12f51ab2f1bd161e43ce0f3df4918c45_59.png)

```python
prompt = """
请以一致的风格回答。
<孩子>：请教我耐心。
<祖父母>：耐心就像一条河流，它不急于到达大海，而是在流淌中塑造两岸。
<孩子>：请教我韧性。
"""
response = get_completion(prompt)
print(response)
```

![](img/12f51ab2f1bd161e43ce0f3df4918c45_61.png)

模型会根据提供的示例，用类似的隐喻风格来回答关于“韧性”的问题。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_63.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_65.png)

---

## 原则二：给予模型充足的“思考”时间 ⏳

第二个核心原则是**给予模型充足的“思考”时间**。如果模型因急于得出结论而犯错，你应该尝试重构查询，要求模型在给出最终答案前，先进行一系列相关的推理。这相当于让模型在任务上投入更多的计算精力。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_67.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_69.png)

以下是实现这一原则的两个策略。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_71.png)

### 策略一：指定任务步骤

![](img/12f51ab2f1bd161e43ce0f3df4918c45_73.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_75.png)

将复杂任务分解为明确的步骤，可以引导模型更有条理地工作，并产出更可控的输出。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_77.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_79.png)

以下是指定多步骤任务的示例：

![](img/12f51ab2f1bd161e43ce0f3df4918c45_81.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_83.png)

```python
text = """在这里插入关于杰克和吉尔的故事文本。"""
prompt = f"""
请执行以下操作：
1. 用一句话总结由三重反引号分隔的文本。
2. 将摘要翻译成法语。
3. 列出法语摘要中出现的每个人名。
4. 输出一个JSON对象，包含 `french_summary` 和 `num_names` 两个键。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_85.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_87.png)

请用换行符分隔每个回答。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_89.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_91.png)

文本：```{text}```
"""
response = get_completion(prompt)
print(response)
```

![](img/12f51ab2f1bd161e43ce0f3df4918c45_93.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_95.png)

为了获得更标准化的输出格式，你甚至可以指定模型输出的具体结构：

![](img/12f51ab2f1bd161e43ce0f3df4918c45_97.png)

```python
prompt = f"""
请执行以下操作：
1. 总结由<>分隔的文本。
2. 将摘要翻译成法语。
3. 列出法语摘要中出现的每个人名。
4. 输出一个包含 `french_summary` 和 `num_names` 的JSON对象。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_99.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_101.png)

请使用以下格式：
文本：<待总结文本>
摘要：<摘要>
翻译：<法语翻译>
人名：<人名列表>
输出JSON：<json>

![](img/12f51ab2f1bd161e43ce0f3df4918c45_103.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_105.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_106.png)

文本：<{text}>
"""
```

![](img/12f51ab2f1bd161e43ce0f3df4918c45_108.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_110.png)

### 策略二：指导模型自主推导解决方案

![](img/12f51ab2f1bd161e43ce0f3df4918c45_112.png)

在模型判断对错之前，先指示它自己推导出解决方案，然后将自己的方案与给定方案进行比较，这能显著提高判断的准确性。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_114.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_116.png)

以下是一个判断学生数学答案正误的改进示例：

```python
question = """
一个工厂生产零件。总成本由固定成本100,000美元和每平方英尺10美元的绝缘成本组成。
设x为绝缘面积（平方英尺），请写出总成本C(x)的公式。
"""
student_solution = “C(x) = 100,000 + 450x” # 学生的错误答案

![](img/12f51ab2f1bd161e43ce0f3df4918c45_118.png)

prompt = f"""
请判断学生的答案是否正确。
请按以下步骤操作：
- 首先，自己推导出问题的正确解法。
- 然后，将你的解法与学生的解法进行比较。
- 最后，判断学生的答案是否正确。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_120.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_122.png)

请使用以下格式输出：
问题：<问题>
学生解答：<学生解答>
实际解答：<你的推导步骤和最终公式>
结果：<[一致/不一致]>
学生成绩：<[正确/不正确]>

![](img/12f51ab2f1bd161e43ce0f3df4918c45_124.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_126.png)

问题：{question}
学生解答：{student_solution}
"""
response = get_completion(prompt)
print(response)
```

![](img/12f51ab2f1bd161e43ce0f3df4918c45_128.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_130.png)

通过这种方式，模型会先计算出正确公式（C(x) = 100,000 + 10x），然后发现与学生答案不一致，从而得出学生答案不正确的结论。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_132.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_134.png)

---

![](img/12f51ab2f1bd161e43ce0f3df4918c45_136.png)

## 模型的局限性与“幻觉”问题 🧠

![](img/12f51ab2f1bd161e43ce0f3df4918c45_138.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_140.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_142.png)

了解大型语言模型的局限性对于开发应用至关重要。模型在训练中接触了大量知识，但并未完全记住，也不清楚自己知识的边界。因此，当遇到生僻话题时，它可能会编造听起来合理但实际错误的信息，这种现象被称为“幻觉”（Hallucination）。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_144.png)

以下是一个模型产生“幻觉”的示例：

![](img/12f51ab2f1bd161e43ce0f3df4918c45_146.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_148.png)

```python
prompt = “请告诉我关于‘AeroGlide UltraSlim Smart Toothbrush’这款牙刷的详细信息。”
response = get_completion(prompt)
print(response)
```

![](img/12f51ab2f1bd161e43ce0f3df4918c45_150.png)

模型可能会生成一段关于这个虚构产品的、听起来非常真实的描述。为了减少幻觉，一个有效的策略是要求模型在基于给定文本回答时，先从中找出相关引用，并用这些引用来支撑它的答案。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_152.png)

---

![](img/12f51ab2f1bd161e43ce0f3df4918c45_154.png)

## 总结 📚

![](img/12f51ab2f1bd161e43ce0f3df4918c45_156.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_158.png)

在本节课中，我们一起学习了编写有效提示词的两个核心原则及其具体策略：

![](img/12f51ab2f1bd161e43ce0f3df4918c45_160.png)

![](img/12f51ab2f1bd161e43ce0f3df4918c45_162.png)

1.  **原则一：编写明确且具体的指令**。我们学习了使用分隔符、要求结构化输出、检查任务条件以及使用少量示例提示（Few-shot Prompting）这四种策略。
2.  **原则二：给予模型充足的“思考”时间**。我们学习了通过指定任务步骤和指导模型自主推导解决方案，来提升复杂任务处理的准确性。
3.  **模型的局限性**。我们了解了模型可能产生“幻觉”的问题，并知道了通过要求引用原文来减少幻觉的基本思路。

![](img/12f51ab2f1bd161e43ce0f3df4918c45_164.png)

掌握这些原则和策略，将帮助你更有效地与大型语言模型交互，构建出更可靠的应用。