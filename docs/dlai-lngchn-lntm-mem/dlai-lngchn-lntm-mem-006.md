# 006：为智能体添加程序性记忆 📧

![](img/edb9094f067d848ec283d9d6ebbff2da_0.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_2.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_4.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_6.png)

在本节课中，我们将学习如何为LangGraph智能体添加第三种，也是最后一种记忆类型：**程序性记忆**。程序性记忆通常以系统提示指令的形式存在，指导智能体如何执行任务。我们将修改现有的邮件助手智能体，使其能够从长期记忆存储中动态读取和更新这些指令，从而实现智能体行为的自动优化。

![](img/edb9094f067d848ec283d9d6ebbff2da_8.png)

---

![](img/edb9094f067d848ec283d9d6ebbff2da_10.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_12.png)

## 概述

上一节我们为智能体添加了以少量示例形式存在的**情景记忆**。本节中，我们将引入**程序性记忆**，它体现在主智能体和分流智能体的系统提示中。我们将修改代码，使这些原本硬编码的提示指令能够从长期记忆存储中读取，并支持根据用户反馈进行自动更新。

![](img/edb9094f067d848ec283d9d6ebbff2da_14.png)

---

![](img/edb9094f067d848ec283d9d6ebbff2da_16.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_17.png)

## 环境与配置初始化

![](img/edb9094f067d848ec283d9d6ebbff2da_18.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_19.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_20.png)

首先，我们需要加载环境变量、定义用户配置以及初始的提示指令。这些初始指令定义了智能体在忽略、通知、回复邮件以及整体行为时应遵循的规则。

![](img/edb9094f067d848ec283d9d6ebbff2da_22.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_23.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_24.png)

```python
# 加载环境变量
import os
from dotenv import load_dotenv
load_dotenv()

# 定义用户配置
profile = "John Doe"

![](img/edb9094f067d848ec283d9d6ebbff2da_26.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_27.png)

# 定义初始的硬编码提示指令
prompt_instructions = {
    "ignore": "Ignore promotional emails and spam.",
    "notify": "Notify me for urgent issues regarding service downtime.",
    "respond": "Respond to all customer support inquiries.",
    "agent": "You are a helpful email assistant. Write concise and professional emails."
}
```

![](img/edb9094f067d848ec283d9d6ebbff2da_29.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_31.png)

这些指令目前是硬编码的，我们稍后会修改代码，使其从长期记忆存储中动态获取。

![](img/edb9094f067d848ec283d9d6ebbff2da_33.png)

---

![](img/edb9094f067d848ec283d9d6ebbff2da_35.png)

## 修改分流路由器以使用记忆存储

接下来，我们修改分流步骤，使其从长期记忆存储中读取指令，而不是使用硬编码值。

![](img/edb9094f067d848ec283d9d6ebbff2da_37.png)

以下是关键修改步骤：

![](img/edb9094f067d848ec283d9d6ebbff2da_39.png)

1.  **获取用户命名空间**：我们使用配置中的用户ID来创建一个唯一的命名空间，用于在记忆存储中查找指令。
2.  **尝试获取记忆**：对于分流步骤的三种指令（忽略、通知、回复），我们尝试从记忆存储中获取。
3.  **设置默认值**：如果记忆存储中没有找到对应的指令，则将我们定义的默认值存入存储，并用于本次运行。
4.  **使用获取的指令**：将获取到的指令变量传递给分流路由器的提示模板。

![](img/edb9094f067d848ec283d9d6ebbff2da_41.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_43.png)

```python
# 从配置中获取用户ID，用于创建记忆存储的命名空间
from langgraph.config import Config
config = Config()
langgraph_user_id = config.get("langgraph_user_id", "default_user")
namespace = (langgraph_user_id,)

![](img/edb9094f067d848ec283d9d6ebbff2da_45.png)

# 尝试获取“忽略”指令
ignore_key = "triage_ignore"
result = long_term_memory_store.get(namespace, ignore_key)
if result is None:
    # 如果不存在，存入默认值
    default_ignore_prompt = prompt_instructions["ignore"]
    long_term_memory_store.put(namespace, ignore_key, {"prompt": default_ignore_prompt})
    ignore_prompt = default_ignore_prompt
else:
    # 如果存在，使用存储的值
    ignore_prompt = result["prompt"]

![](img/edb9094f067d848ec283d9d6ebbff2da_47.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_48.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_49.png)

# 对“通知”和“回复”指令重复上述过程
# ... (代码类似，使用 triage_notify 和 triage_respond 作为键)

![](img/edb9094f067d848ec283d9d6ebbff2da_51.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_52.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_53.png)

# 在创建分流路由器提示时，使用从存储中获取的变量
triage_router_prompt = f"""
Based on the email content, decide to: IGNORE, NOTIFY, or RESPOND.
IGNORE instructions: {ignore_prompt}
NOTIFY instructions: {notify_prompt}
RESPOND instructions: {respond_prompt}
Email: {{email}}
Decision:
"""
```

通过以上修改，分流路由器现在会使用存储在长期记忆中的指令。这是第一步。

---

![](img/edb9094f067d848ec283d9d6ebbff2da_55.png)

## 修改主智能体提示创建函数

![](img/edb9094f067d848ec283d9d6ebbff2da_57.png)

同样，我们需要修改主智能体的提示创建函数，使其指令也来源于长期记忆存储。

![](img/edb9094f067d848ec283d9d6ebbff2da_59.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_61.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_63.png)

我们创建一个 `create_prompt` 函数，它接受配置和存储作为参数，并执行类似的逻辑：首先尝试从存储中获取“agent_instructions”，如果没有则存入默认值，最后使用获取到的指令来构建系统消息。

![](img/edb9094f067d848ec283d9d6ebbff2da_65.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_66.png)

```python
def create_prompt(config, store, prompt_instructions):
    """创建主智能体的提示，指令来自长期记忆存储。"""
    langgraph_user_id = config.get("langgraph_user_id", "default_user")
    namespace = (langgraph_user_id,)
    key = "agent_instructions"

    # 尝试从存储获取指令
    result = store.get(namespace, key)
    if result is None:
        # 存入默认指令
        default_agent_prompt = prompt_instructions["agent"]
        store.put(namespace, key, {"prompt": default_agent_prompt})
        agent_prompt_value = default_agent_prompt
    else:
        # 使用存储的指令
        agent_prompt_value = result["prompt"]

    # 使用获取到的指令构建系统消息
    system_message = f"""
    {agent_prompt_value}
    Your profile: {profile}
    """
    return system_message
```

![](img/edb9094f067d848ec283d9d6ebbff2da_68.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_69.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_71.png)

现在，主智能体的行为指令也来自于长期记忆存储。这意味着我们可以从外部更新这些指令，并让智能体在未来的运行中自动采用新指令。

![](img/edb9094f067d848ec283d9d6ebbff2da_73.png)

---

![](img/edb9094f067d848ec283d9d6ebbff2da_75.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_77.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_79.png)

## 运行智能体并查看初始记忆

让我们用一个示例邮件来运行更新后的智能体。

```python
example_email = "Hi John, urgent issue. Your service is down."
config = {"langgraph_user_id": "lance"}

![](img/edb9094f067d848ec283d9d6ebbff2da_81.png)

# 运行智能体
response = email_agent.invoke({"email": example_email}, config=config)
print(response)
```

运行后，我们可以检查长期记忆存储，确认初始的默认指令已被存入。

![](img/edb9094f067d848ec283d9d6ebbff2da_83.png)

```python
# 检查存储的内容
print(long_term_memory_store.get(("lance",), "agent_instructions"))
print(long_term_memory_store.get(("lance",), "triage_respond"))
```

---

![](img/edb9094f067d848ec283d9d6ebbff2da_85.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_87.png)

## 基于反馈自动更新程序性记忆

![](img/edb9094f067d848ec283d9d6ebbff2da_89.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_91.png)

程序性记忆的核心优势在于它可以被优化。我们将使用 `langchain.memory` 中的 `create_multi_prompt_optimizer` 函数，根据用户对智能体整体表现的反馈，自动判断需要更新哪些具体的提示指令，并进行更新。

![](img/edb9094f067d848ec283d9d6ebbff2da_92.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_94.png)

以下是实现步骤：

![](img/edb9094f067d848ec283d9d6ebbff2da_95.png)

1.  **准备对话轨迹和反馈**：收集智能体运行产生的消息链（轨迹）以及用户的文本反馈。
2.  **定义待优化的提示列表**：列出所有可能被更新的提示（主智能体指令、分流忽略、分流通知、分流回复）。每个提示需要提供名称、当前值、更新说明（如何更新）以及更新条件（何时更新）。
3.  **创建并运行优化器**：使用一个LLM（如Anthropic Claude）来分析反馈，根据“更新条件”判断哪些提示需要修改，并依据“更新说明”生成新的提示内容。
4.  **将更新写回记忆存储**：比较优化前后的提示内容，如果发生变化，则将新指令保存回长期记忆存储。

![](img/edb9094f067d848ec283d9d6ebbff2da_97.png)

```python
from langchain.memory import create_multi_prompt_optimizer
from langchain_anthropic import ChatAnthropic

![](img/edb9094f067d848ec283d9d6ebbff2da_99.png)

# 1. 准备轨迹和反馈
trajectory = response_messages # 假设这是上次运行智能体产生的消息列表
feedback = "Always sign your emails, John Doe."

![](img/edb9094f067d848ec283d9d6ebbff2da_101.png)

# 2. 定义待优化提示列表（从存储中获取当前值）
prompts_to_optimize = [
    {
        "name": "main_agent",
        "prompt": long_term_memory_store.get(("lance",), "agent_instructions")["prompt"],
        "update_instructions": "Keep the instructions short and to the point.",
        "when_to_update": "Update this prompt whenever there is feedback on how the agent should write emails or schedule events."
    },
    {
        "name": "triage_ignore",
        "prompt": long_term_memory_store.get(("lance",), "triage_ignore")["prompt"],
        "update_instructions": "Keep the instructions short and to the point.",
        "when_to_update": "Update this prompt whenever there is feedback on which emails should be ignored."
    },
    # ... 类似定义 triage_notify 和 triage_respond
]

![](img/edb9094f067d848ec283d9d6ebbff2da_103.png)

# 3. 创建优化器并运行
llm = ChatAnthropic(model="claude-3-sonnet-20240229")
optimizer = create_multi_prompt_optimizer(llm, kind="prompt_memory")
updated_prompts = optimizer.invoke({
    "conversations": [trajectory],
    "prompts": prompts_to_optimize,
    "feedback": feedback
})

# 4. 将更新写回存储
for updated_prompt in updated_prompts:
    old_prompt = next(p for p in prompts_to_optimize if p["name"] == updated_prompt["name"])
    if updated_prompt["prompt"] != old_prompt["prompt"]:
        # 内容已更新，写回存储
        key_map = {
            "main_agent": "agent_instructions",
            "triage_ignore": "triage_ignore",
            "triage_notify": "triage_notify",
            "triage_respond": "triage_respond"
        }
        store_key = key_map[updated_prompt["name"]]
        long_term_memory_store.put(("lance",), store_key, {"prompt": updated_prompt["prompt"]})
        print(f"Updated {updated_prompt['name']} prompt.")
```

![](img/edb9094f067d848ec283d9d6ebbff2da_105.png)

运行上述代码后，检查记忆存储，会发现 `agent_instructions` 中增加了“When sending emails, always sign them as John Doe.”这条规则。重新运行智能体处理邮件，它会自动在邮件末尾添加签名。

![](img/edb9094f067d848ec283d9d6ebbff2da_107.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_108.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_110.png)

---

![](img/edb9094f067d848ec283d9d6ebbff2da_112.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_114.png)

## 另一个更新示例：修改忽略规则

![](img/edb9094f067d848ec283d9d6ebbff2da_115.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_117.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_119.png)

假设我们收到一封来自“Alice Jones”的邮件，智能体回复了它，但我们希望未来忽略来自此人的所有邮件。

![](img/edb9094f067d848ec283d9d6ebbff2da_121.png)

我们可以提供反馈：“Ignore any emails from Alice Jones.”。优化器会分析这次交互，很可能判定需要更新 `triage_ignore` 指令。更新后，`triage_ignore` 的指令会变为“Ignore all emails from Alice Jones.”。之后，智能体再收到来自Alice的邮件时，就会执行忽略操作。

![](img/edb9094f067d848ec283d9d6ebbff2da_123.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_125.png)

---

![](img/edb9094f067d848ec283d9d6ebbff2da_127.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_128.png)

## 总结

![](img/edb9094f067d848ec283d9d6ebbff2da_130.png)

![](img/edb9094f067d848ec283d9d6ebbff2da_131.png)

本节课中，我们一起完成了为LangGraph智能体集成**程序性记忆**的工作。我们主要学习了：

![](img/edb9094f067d848ec283d9d6ebbff2da_133.png)

1.  **概念**：程序性记忆表现为指导智能体行为的系统提示指令。
2.  **实现**：修改智能体代码，使其从长期记忆存储中动态读取提示指令，而非使用硬编码值。
3.  **优化**：利用 `create_multi_prompt_optimizer`，根据用户对智能体整体表现的反馈，自动判断并更新相关的具体提示指令，实现智能体行为的持续学习和改进。

![](img/edb9094f067d848ec283d9d6ebbff2da_135.png)

现在，你的邮件助手智能体已经具备了**语义记忆**（向量存储）、**情景记忆**（少量示例）和**程序性记忆**（可更新的系统指令）这三种记忆能力。建议你尝试传入不同的对话和反馈，观察智能体的哪些记忆部分会被更新以及如何更新，从而更深入地理解其工作原理。