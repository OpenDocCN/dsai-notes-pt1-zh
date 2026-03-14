# 7：指令微调 🧠

![](img/0e95a061bb8146e003527632c1ac73e8_1.png)

![](img/0e95a061bb8146e003527632c1ac73e8_3.png)

![](img/0e95a061bb8146e003527632c1ac73e8_5.png)

![](img/0e95a061bb8146e003527632c1ac73e8_7.png)

![](img/0e95a061bb8146e003527632c1ac73e8_9.png)

![](img/0e95a061bb8146e003527632c1ac73e8_11.png)

![](img/0e95a061bb8146e003527632c1ac73e8_13.png)

在本节课中，我们将要学习如何通过指令微调（Instruction Finetuning）来教会大语言模型（LLM）遵循人类的指令。我们将构建一个能够理解多种查询并以自由文本形式回应的个人助手。虽然我们的模型规模较小，但核心概念与训练像ChatGPT这样的强大模型是相通的。

![](img/0e95a061bb8146e003527632c1ac73e8_15.png)

![](img/0e95a061bb8146e003527632c1ac73e8_16.png)

## 概述

![](img/0e95a061bb8146e003527632c1ac73e8_18.png)

![](img/0e95a061bb8146e003527632c1ac73e8_19.png)

指令微调是继预训练之后的关键步骤，它能让模型从仅仅预测下一个词，转变为理解并执行具体的任务指令。我们将从一个预训练的GPT-2模型开始，使用一个专门的数据集对其进行微调，使其能够处理诸如翻译、改写、问答等多种指令。

![](img/0e95a061bb8146e003527632c1ac73e8_21.png)

---

![](img/0e95a061bb8146e003527632c1ac73e8_23.png)

![](img/0e95a061bb8146e003527632c1ac73e8_25.png)

![](img/0e95a061bb8146e003527632c1ac73e8_26.png)

![](img/0e95a061bb8146e003527632c1ac73e8_28.png)

![](img/0e95a061bb8146e003527632c1ac73e8_30.png)

![](img/0e95a061bb8146e003527632c1ac73e8_32.png)

## 7.1：为监督式指令微调准备数据 📊

![](img/0e95a061bb8146e003527632c1ac73e8_34.png)

![](img/0e95a061bb8146e003527632c1ac73e8_35.png)

![](img/0e95a061bb8146e003527632c1ac73e8_37.png)

![](img/0e95a061bb8146e003527632c1ac73e8_38.png)

![](img/0e95a061bb8146e003527632c1ac73e8_39.png)

上一节我们介绍了本章的目标，本节中我们来看看如何准备用于训练的数据。数据质量是微调成功的关键。

我们使用一个包含约1100个训练示例的JSON格式数据集。每个示例包含三个字段：
*   `instruction`: 描述任务的指令（例如，“将以下句子改为被动语态”）。
*   `input`: 与指令对应的具体输入内容（例如，“The chef cooks the meal every day”）。此字段有时可能为空。
*   `output`: 我们希望模型生成的正确答案。

![](img/0e95a061bb8146e003527632c1ac73e8_41.png)

![](img/0e95a061bb8146e003527632c1ac73e8_43.png)

![](img/0e95a061bb8146e003527632c1ac73e8_44.png)

![](img/0e95a061bb8146e003527632c1ac73e8_45.png)

![](img/0e95a061bb8146e003527632c1ac73e8_47.png)

![](img/0e95a061bb8146e003527632c1ac73e8_48.png)

为了将这种结构化的数据输入给模型，我们需要将其转换为模型能理解的纯文本格式。这里我们采用经典的Alpaca提示模板风格。

![](img/0e95a061bb8146e003527632c1ac73e8_50.png)

![](img/0e95a061bb8146e003527632c1ac73e8_51.png)

![](img/0e95a061bb8146e003527632c1ac73e8_53.png)

![](img/0e95a061bb8146e003527632c1ac73e8_55.png)

![](img/0e95a061bb8146e003527632c1ac73e8_57.png)

![](img/0e95a061bb8146e003527632c1ac73e8_58.png)

![](img/0e95a061bb8146e003527632c1ac73e8_60.png)

![](img/0e95a061bb8146e003527632c1ac73e8_61.png)

以下是转换函数：
```python
def format_input(instruction, input_text):
    prompt = “””Below is an instruction that describes a task. Write a response that appropriately completes the request.
### Instruction:
{instruction}
### Input:
{input_text}
### Response:
“””.format(instruction=instruction, input_text=input_text if input_text else “”)
    return prompt
```
这个函数会将一个数据条目（如 `{“instruction”: “Identify the correct spelling”, “input”: “occassion”, “output”: “occasion”}`）转换为一段带有明确格式的文本。在训练时，我们会将 `output`（即期望的响应）附加在这个提示文本之后，共同作为模型的输入。

![](img/0e95a061bb8146e003527632c1ac73e8_63.png)

![](img/0e95a061bb8146e003527632c1ac73e8_65.png)

![](img/0e95a061bb8146e003527632c1ac73e8_66.png)

接下来，我们将数据集划分为训练集（85%）、验证集（5%）和测试集（10%），以便后续进行训练和评估。

![](img/0e95a061bb8146e003527632c1ac73e8_68.png)

![](img/0e95a061bb8146e003527632c1ac73e8_70.png)

---

![](img/0e95a061bb8146e003527632c1ac73e8_72.png)

![](img/0e95a061bb8146e003527632c1ac73e8_73.png)

## 7.2：构建数据集与数据加载器 🔧

上一节我们介绍了如何格式化数据，本节中我们来看看如何将这些文本数据打包成模型训练所需的批次（batches）。这是指令微调中最关键也最复杂的步骤之一。

![](img/0e95a061bb8146e003527632c1ac73e8_75.png)

![](img/0e95a061bb8146e003527632c1ac73e8_76.png)

我们遵循以下五个核心步骤来处理每个批次的数据：

1.  **格式化提示**：使用上一节的 `format_input` 函数，将JSON条目转换为带有指令、输入和期望响应的完整文本。
2.  **分词**：使用TikToken分词器将文本转换为模型能理解的Token ID序列。
3.  **批次内填充**：为了让一个批次内的所有样本长度一致以便并行计算，我们需要进行填充。与之前将整个数据集填充到最长样本不同，这里我们采用更高效的**批次内填充**，即只将当前批次内的样本填充到该批次中最长样本的长度。
4.  **创建目标标签**：训练LLM的核心是预测下一个Token。因此，我们需要创建目标Token ID，它等于输入Token ID序列向右移动一位。为了保持输入和目标长度一致，我们会在目标序列末尾添加一个额外的填充Token。
5.  **掩码处理**：为了避免模型学习无关的填充Token，我们将目标序列中**除最后一个之外的所有填充Token**替换为一个特殊的忽略索引（如 `-100`）。这样，在计算损失函数时，这些位置就会被忽略。保留最后一个填充Token是为了教会模型在生成响应后输出一个“结束符”（End-of-Sequence Token）。

以下是实现这些步骤的自定义数据整理函数（`collate_fn`）的核心逻辑：
```python
def custom_collate_fn(batch, pad_token_id=50256, ignore_index=-100, max_length=1024):
    # 步骤1 & 2: 数据已在InstructionDataset中完成格式化和分词
    input_ids_list = [item[‘input_ids’] for item in batch] # 假设每个item是包含’input_ids’的字典

    # 步骤3: 批次内填充
    max_len_in_batch = min(max(len(seq) for seq in input_ids_list), max_length)
    padded_inputs = []
    for seq in input_ids_list:
        truncated_seq = seq[:max_len_in_batch]
        padding = [pad_token_id] * (max_len_in_batch - len(truncated_seq))
        padded_inputs.append(truncated_seq + padding)

    # 步骤4: 创建目标标签（右移一位）
    targets_list = []
    for seq in padded_inputs:
        targets = seq[1:] + [pad_token_id] # 右移，并在末尾补一个pad_token
        targets_list.append(targets)

    # 步骤5: 掩码处理（忽略除最后一个外的所有填充Token）
    for targets in targets_list:
        pad_positions = [i for i, tok in enumerate(targets) if tok == pad_token_id]
        if len(pad_positions) > 1: # 保留最后一个填充Token
            for pos in pad_positions[:-1]:
                targets[pos] = ignore_index

    # 转换为PyTorch张量并返回
    inputs_tensor = torch.tensor(padded_inputs)
    targets_tensor = torch.tensor(targets_list)
    return inputs_tensor, targets_tensor
```
定义好数据集类和整理函数后，我们就可以创建PyTorch的 `DataLoader`，它会在训练时自动调用我们的 `custom_collate_fn` 来生成规整的批次数据。

![](img/0e95a061bb8146e003527632c1ac73e8_78.png)

![](img/0e95a061bb8146e003527632c1ac73e8_79.png)

---

## 7.3：加载预训练模型 🏗️

![](img/0e95a061bb8146e003527632c1ac73e8_81.png)

![](img/0e95a061bb8146e003527632c1ac73e8_82.png)

![](img/0e95a061bb8146e003527632c1ac73e8_84.png)

![](img/0e95a061bb8146e003527632c1ac73e8_85.png)

数据准备就绪后，下一步是加载我们想要微调的基座模型。这个过程与第6章类似。

![](img/0e95a061bb8146e003527632c1ac73e8_87.png)

![](img/0e95a061bb8146e003527632c1ac73e8_88.png)

![](img/0e95a061bb8146e003527632c1ac73e8_89.png)

![](img/0e95a061bb8146e003527632c1ac73e8_90.png)

我们选择加载OpenAI发布的GPT-2 Medium模型（约3.55亿参数），它比之前使用的1.24亿参数模型能力更强，同时仍能在消费级硬件上运行。我们使用与之前章节相同的 `load_weights_into_gpt` 函数来加载预训练的权重。

加载完成后，我们可以先测试一下模型在微调前的表现。例如，给它一个指令：“将主动句改为被动语态：The chef cooks the meal every day。” 此时，未经微调的模型很可能只是重复输入内容或生成不相关的文本，无法正确执行指令。这正体现了我们进行指令微调的必要性。

![](img/0e95a061bb8146e003527632c1ac73e8_92.png)

![](img/0e95a061bb8146e003527632c1ac73e8_93.png)

![](img/0e95a061bb8146e003527632c1ac73e8_95.png)

![](img/0e95a061bb8146e003527632c1ac73e8_96.png)

---

![](img/0e95a061bb8146e003527632c1ac73e8_98.png)

![](img/0e95a061bb8146e003527632c1ac73e8_100.png)

## 7.4：执行指令微调 🚀

现在，我们可以利用之前准备好的数据加载器和预训练模型，开始进行指令微调了。

令人高兴的是，指令微调所使用的损失函数与预训练完全相同，都是**下一个Token预测**（Next-Token Prediction）。这意味着我们可以重用第5章中编写的 `train_model_simple`、`calc_loss_batch` 和 `calc_loss_loader` 等训练工具函数。

![](img/0e95a061bb8146e003527632c1ac73e8_102.png)

![](img/0e95a061bb8146e003527632c1ac73e8_103.png)

![](img/0e95a061bb8146e003527632c1ac73e8_105.png)

训练过程包括：
*   **设置超参数**：例如，使用AdamW优化器，选择一个合适的学习率（如 `0.0001`），训练2个周期（Epoch）。
*   **监控训练**：在训练过程中，我们不仅观察训练损失和验证损失的下陷情况，还会定期让模型生成一些样本来直观地判断其进步。
*   **保存模型**：训练完成后，将微调好的模型权重保存下来，以便后续评估和使用。

![](img/0e95a061bb8146e003527632c1ac73e8_107.png)

![](img/0e95a061bb8146e003527632c1ac73e8_108.png)

在作者的M1 MacBook Air上，使用3.55亿参数的模型在这个小数据集上训练2个周期大约需要15-30分钟。训练后，模型应该能够对诸如“将主动句改为被动语态”这样的指令给出合理响应（例如，“The meal is cooked by the chef every day.”）。

![](img/0e95a061bb8146e003527632c1ac73e8_110.png)

![](img/0e95a061bb8146e003527632c1ac73e8_111.png)

---

## 7.5：评估微调后的模型 📈

![](img/0e95a061bb8146e003527632c1ac73e8_113.png)

![](img/0e95a061bb8146e003527632c1ac73e8_114.png)

![](img/0e95a061bb8146e003527632c1ac73e8_115.png)

模型训练完成后，我们需要系统地评估其性能。由于模型输出是自由文本，无法像分类任务那样简单计算准确率，因此评估更具挑战性。

![](img/0e95a061bb8146e003527632c1ac73e8_117.png)

![](img/0e95a061bb8146e003527632c1ac73e8_118.png)

![](img/0e95a061bb8146e003527632c1ac73e8_119.png)

![](img/0e95a061bb8146e003527632c1ac73e8_121.png)

![](img/0e95a061bb8146e003527632c1ac73e8_122.png)

![](img/0e95a061bb8146e003527632c1ac73e8_123.png)

![](img/0e95a061bb8146e003527632c1ac73e8_125.png)

一种流行且实用的方法是使用**另一个LLM作为评判员**。具体步骤如下：

![](img/0e95a061bb8146e003527632c1ac73e8_127.png)

![](img/0e95a061bb8146e003527632c1ac73e8_129.png)

![](img/0e95a061bb8146e003527632c1ac73e8_131.png)

![](img/0e95a061bb8146e003527632c1ac73e8_132.png)

1.  **生成测试集响应**：使用我们微调好的模型，在从未见过的测试集（110个样本）上运行，为每个指令生成模型响应。
2.  **准备评估提示**：为每个测试样本构建一个给“评判员LLM”的提示，其中包含原始指令、输入、标准答案以及我们模型的响应。我们要求评判员根据标准答案，为我们模型的响应在0-100分之间打分。
3.  **调用评判员LLM**：我们使用一个名为 **Ollama** 的工具，它可以在本地轻松运行各种开源LLM。这里我们选择使用较小的 `Llama 3.2`（30亿参数）模型作为评判员。通过Ollama提供的API，我们可以编程方式将评估提示发送给该模型并获取其打分。
4.  **计算平均得分**：收集所有测试样本的得分后，计算平均分，作为我们微调模型性能的一个量化指标。

![](img/0e95a061bb8146e003527632c1ac73e8_134.png)

以下是评估提示的示例模板：
```
Given the instruction: ‘{instruction}’ and the input: ‘{input}’,
the correct output is: ‘{reference_output}’.
Score the following model response on a scale from 0 to 100: ‘{model_response}’.
Respond with the integer number only.
```
**需要注意**：这种评估方法的可靠性很大程度上取决于评判员LLM本身的能力。较小的评判员模型可能打分不稳定或不准确。在实践中，人们通常会使用更强大的模型（如GPT-4）或专门针对评估任务微调的模型来获得更可靠的结果。此外，精心设计评估提示（Rubric）也有助于提高打分的一致性。

![](img/0e95a061bb8146e003527632c1ac73e8_136.png)

![](img/0e95a061bb8146e003527632c1ac73e8_137.png)

![](img/0e95a061bb8146e003527632c1ac73e8_139.png)

![](img/0e95a061bb8146e003527632c1ac73e8_140.png)

尽管存在局限，但这种自动化评估方法为我们提供了一种相对快速、可量化的方式来比较不同微调策略或超参数设置的效果。

---

![](img/0e95a061bb8146e003527632c1ac73e8_142.png)

![](img/0e95a061bb8146e003527632c1ac73e8_143.png)

## 总结

本节课中我们一起学习了指令微调（Instruction Finetuning）的完整流程。我们从准备结构化的指令数据集开始，学习了如何将其转换为模型可训练的格式，并实现了关键的数据批处理逻辑。接着，我们加载了一个预训练的GPT-2模型，并使用与预训练相同的下一个词预测目标对其进行了微调。最后，我们探讨了如何使用另一个LLM作为评判员来对生成式模型的性能进行自动化评估。

![](img/0e95a061bb8146e003527632c1ac73e8_145.png)

![](img/0e95a061bb8146e003527632c1ac73e8_146.png)

![](img/0e95a061bb8146e003527632c1ac73e8_147.png)

通过本章，你不仅掌握了构建一个能遵循指令的简单个人助手的技术细节，也理解了使当今强大AI助手变得“有用”的核心技术原理。虽然我们的模型规模有限，但其中涉及的提示工程、监督式微调、基于LLM的评估等概念，正是构建更高级大语言模型应用的基础。