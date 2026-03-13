# 006：人机协同反馈循环 🎯

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_0.png)

## 概述

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_2.png)

在本节课中，我们将学习如何为上一节创建的自动表单填写代理添加人机协同（Human-in-the-Loop）反馈机制。我们将修改工作流，使其能够接收人类反馈，并根据反馈改进答案，从而生成更准确的结果。

---

## 工作流重构：分离解析与生成

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_4.png)

上一节我们介绍了如何自动识别表单字段并生成答案。本节中，我们来看看如何将表单解析与问题生成步骤分离，以便支持多次反馈循环。

为了实现人机协同，我们需要使用两个新的事件：`input_required` 和 `human_response`。它们专门设计用于允许工作流暂停并接收外部输入。

由于现在可能需要循环多次提问，我们不需要每次都解析表单。因此，我们将重构工作流，将原来的单一步骤拆分为多个步骤。这种重构在创建更复杂的工作流时非常常见。

新的 `generate_questions` 步骤将由两个事件触发：
1.  表单解析器触发的 `generate_questions` 事件。
2.  获取反馈后触发的 `feedback` 事件（即我们的循环）。

然后，我们将发出一个 `input_required` 事件。在同一流程中，工作流将暂停，等待 `human_response` 事件（即外部输入）。最后，使用 LLM 解析反馈，并决定是继续输出结果，还是需要循环回去重新生成答案。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_6.png)

让我们开始吧。首先，导入必要的库和设置 API 密钥。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_8.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_10.png)

```python
import asyncio
# ... 其他必要的导入
# 设置 API 密钥
```

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_12.png)

---

## 定义新的事件与工作流

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_14.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_15.png)

现在，像往常一样设置我们的事件。这里新增了一个 `feedback` 事件。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_16.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_17.png)

```python
# 定义事件类
class GenerateQuestionsEvent:
    pass
class InputRequiredEvent:
    pass
class HumanResponseEvent:
    pass
class FeedbackEvent:
    pass
# ... 其他事件
```

让我们逐步分析新的工作流。

```python
# 工作流定义示例
@workflow
async def my_workflow(context):
    # 1. 设置步骤（与之前相同）
    # 2. 解析表单步骤（已修改）
    #    解析表单后，将字段存入上下文，并发送 GenerateQuestionsEvent
    # 3. 生成问题步骤（新增逻辑）
    #    可由 GenerateQuestionsEvent 或 FeedbackEvent 触发
    #    检查是否有反馈，并据此修改问题
    #    为每个字段触发查询事件
    # 4. 收集答案步骤
    # 5. 发出 InputRequiredEvent，等待人类反馈
    # 6. 获取反馈步骤，接收 HumanResponseEvent
    #    使用 LLM 判断反馈内容
    #    若反馈为“OK”，则停止；若为其他，则触发 FeedbackEvent 循环
```

初始的设置和表单解析逻辑基本保持不变。关键变化在于，我们将表单解析与问题生成分离。解析表单得到字段列表后，将其存入上下文，然后发送 `GenerateQuestionsEvent`。

`generate_questions` 步骤可以由 `GenerateQuestionsEvent` 触发，也可以由后续的 `FeedbackEvent` 触发。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_19.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_20.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_21.png)

---

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_22.png)

## 实现生成问题与收集答案

在 `generate_questions` 步骤中，我们首先从上下文中获取需要填写的字段列表。

以下是该步骤的核心逻辑：

```python
# 伪代码逻辑
fields_to_fill = context.get(‘fields_to_fill’)
total_fields = len(fields_to_fill)
context.set(‘total_fields‘, total_fields)

for field in fields_to_fill:
    # 检查是否存在反馈，若有则附加到问题中
    question = f“What is the {field}?”
    if hasattr(event, ‘feedback’) and event.feedback:
        question += f“ Note: {event.feedback} (This may not be relevant to this field.)”
    # 触发查询事件
    emit(QueryEvent(question=question, field=field))
```

与之前一样，我们存储字段总数，以便 `collect_events` 步骤知道需要等待多少个答案。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_24.png)

`ask_questions` 事件保持不变。但在 `fill_application_form` 步骤中，我们现在会发出一个 `input_required` 事件。我们仍然等待收集所有答案事件，完成后将其转换为问题列表。新的操作是，我们将这个问题集存入上下文以备后用，然后发出 `input_required` 事件。

这个事件会向人类发送一条消息，展示当前表单填写情况，并请求人类给出反馈。

---

## 处理人类反馈

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_26.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_27.png)

在名为 `get_feedback` 的下一步中，我们接收一个 `human_response` 事件。这与之前的步骤不同，之前的步骤总是由前一步骤的事件触发。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_28.png)

现在工作流中出现了一个“间隙”：`input_required` 事件不是由工作流内部触发的，它需要被外部捕获；同样，`human_response` 事件也不是在工作流内部发出的，它必须来自工作流外部。

一旦我们获得反馈，就会要求 LLM 判断反馈是“好”（一切正常）还是“坏”（需要修改）。如果 LLM 认为一切正常，它将发出“OK”信号，工作流将停止。如果 LLM 发出“feedback”信号，那么我们将触发一个 `feedback` 事件，从而开始循环。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_30.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_31.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_32.png)

那么，我们如何实际获取反馈呢？实际上，方法你已经知道了。`input_required` 事件是事件流中的一个事件，就像之前发送的 `progress` 和 `text` 事件一样。你可以用同样的方式拦截它，并使用上下文上的 `send_event` 方法发回一个 `human_response` 事件。

所有这些都发生在执行工作流的代码中。

```python
# 执行工作流并处理反馈的示例
async def run_workflow():
    handler = await my_workflow.run()
    async for event in handler.stream_events():
        if isinstance(event, InputRequiredEvent):
            # 打印当前表单状态给用户看
            print(“Current form answers:“, context.get(‘questions’))
            # 获取键盘输入作为反馈
            response = input(“Please provide feedback (or type ‘OK’ if fine): “)
            # 将反馈作为 HumanResponseEvent 发送回工作流
            await handler.send_event(HumanResponseEvent(response=response))
        # ... 处理其他事件
    await handler
```

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_34.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_35.png)

当我们看到 `input_required` 事件时，会打印出表单内容供用户查看，然后使用 `input()` 方法获取键盘反馈。一旦在 `response` 变量中获得反馈，我们就使用 `send_event` 方法将其作为 `human_response` 事件发送回去，然后等待处理程序继续。

---

## 整合反馈以改进答案

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_37.png)

目前，我们的工作流可以接收反馈并循环，但还没有找到整合反馈的方法，所以它只会以完全相同的方式重复操作。现在，让我们进一步修改，以便实际利用生成的反馈做一些有用的事情。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_38.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_39.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_41.png)

这涉及到检查是否存在反馈，并将其附加到问题中。在这个简化的例子中，我们将把反馈附加到每个问题上（以防相关）。更复杂的代理可能只将反馈应用到相关的字段上。

以下是修改后的 `generate_questions` 步骤逻辑：

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_43.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_44.png)

```python
# 修改后的生成问题逻辑
if hasattr(event, ‘feedback’) and event.feedback:
    feedback_text = event.feedback
    for field in fields_to_fill:
        modified_question = f“What is the {field}? Note: {feedback_text} (This may not be relevant to this field.)”
        # 使用修改后的问题触发事件
else:
    # 使用原始问题触发事件
```

现在，当工作流再次运行时，LLM 将看到附加了反馈的问题，从而知道如何调整答案。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_46.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_47.png)

例如，第一次运行时，“项目组合”字段可能生成了一段描述。在收到“组合应该是一个URL”的反馈后，第二次运行时，LLM 会看到这个提示，从而为该字段生成一个链接。

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_48.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_49.png)

---

## 总结

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_51.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_52.png)

![](img/8f209c9e3ecea0f0b8855ebce1a4bb10_53.png)

本节课中，我们一起学习了如何为 LlamaIndex 智能代理工作流添加人机协同反馈循环。我们重构了工作流，引入了 `input_required` 和 `human_response` 事件来接收外部输入，并修改了问题生成逻辑以整合人类反馈。现在，我们的代理能够理解自然语言反馈，并根据反馈迭代改进其输出，从而生成更准确、更符合要求的表单填写结果。这体现了 LLM 作为人类助手、增强而非取代人类工作的强大能力。