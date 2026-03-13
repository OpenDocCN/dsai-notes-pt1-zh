# 028：链（Chains）🚀

![](img/6979b54d544ad51cba7b8167af30399e_0.png)

![](img/6979b54d544ad51cba7b8167af30399e_2.png)

![](img/6979b54d544ad51cba7b8167af30399e_4.png)

在本节课中，我们将学习 LangChain 中最重要的核心构建块之一：**链（Chains）**。链通常结合了一个大型语言模型（LLM）和一个提示（Prompt），你也可以将多个这样的构建块串联起来，对文本或数据执行一系列操作。

![](img/6979b54d544ad51cba7b8167af30399e_6.png)

![](img/6979b54d544ad51cba7b8167af30399e_8.png)

![](img/6979b54d544ad51cba7b8167af30399e_10.png)

---

![](img/6979b54d544ad51cba7b8167af30399e_12.png)

![](img/6979b54d544ad51cba7b8167af30399e_14.png)

## 环境与数据准备

![](img/6979b54d544ad51cba7b8167af30399e_16.png)

首先，我们需要加载环境变量和一些将要使用的数据。

![](img/6979b54d544ad51cba7b8167af30399e_18.png)

![](img/6979b54d544ad51cba7b8167af30399e_20.png)

我们将加载一个 Pandas DataFrame，这是一个包含多行不同数据元素的数据结构。如果你不熟悉 Pandas 也没关系，这里我们只需要知道它存储了我们可以用于后续链处理的数据。

![](img/6979b54d544ad51cba7b8167af30399e_22.png)

![](img/6979b54d544ad51cba7b8167af30399e_24.png)

以下是数据预览：
- 包含 `产品` 列和 `评论` 列。
- 每一行都是一个独立的数据点，可以传递给链进行处理。

![](img/6979b54d544ad51cba7b8167af30399e_26.png)

![](img/6979b54d544ad51cba7b8167af30399e_28.png)

---

![](img/6979b54d544ad51cba7b8167af30399e_30.png)

![](img/6979b54d544ad51cba7b8167af30399e_32.png)

## LLM 链：基础构建块

![](img/6979b54d544ad51cba7b8167af30399e_34.png)

![](img/6979b54d544ad51cba7b8167af30399e_36.png)

我们将要介绍的第一个链是 **LLM 链**。这是一个简单但功能强大的链，它是许多后续复杂链的基础。

![](img/6979b54d544ad51cba7b8167af30399e_38.png)

![](img/6979b54d544ad51cba7b8167af30399e_40.png)

我们需要导入三个关键组件：
1.  OpenAI 模型
2.  聊天提示模板
3.  LLM 链

![](img/6979b54d544ad51cba7b8167af30399e_42.png)

![](img/6979b54d544ad51cba7b8167af30399e_44.png)

以下是初始化步骤：
1.  **初始化语言模型**：使用 `ChatOpenAI` 并设置较高的 `temperature` 参数，以获得更有创造性的输出。
2.  **初始化提示模板**：创建一个提示，它接受一个名为 `product` 的变量，并要求 LLM 为生产该产品的公司生成最佳名称。
3.  **组合成链**：将语言模型和提示模板组合成一个 LLM 链。

![](img/6979b54d544ad51cba7b8167af30399e_46.png)

![](img/6979b54d544ad51cba7b8167af30399e_48.png)

**核心代码示例**：
```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain.chains import LLMChain

![](img/6979b54d544ad51cba7b8167af30399e_50.png)

![](img/6979b54d544ad51cba7b8167af30399e_52.png)

# 1. 初始化模型
llm = ChatOpenAI(temperature=0.9)

![](img/6979b54d544ad51cba7b8167af30399e_54.png)

![](img/6979b54d544ad51cba7b8167af30399e_56.png)

# 2. 初始化提示模板
prompt = ChatPromptTemplate.from_template(
    “为生产 {product} 的公司起一个最佳名称是什么？”
)

![](img/6979b54d544ad51cba7b8167af30399e_58.png)

![](img/6979b54d544ad51cba7b8167af30399e_60.png)

![](img/6979b54d544ad51cba7b8167af30399e_62.png)

# 3. 组合成链
chain = LLMChain(llm=llm, prompt=prompt)
```

![](img/6979b54d544ad51cba7b8167af30399e_64.png)

![](img/6979b54d544ad51cba7b8167af30399e_66.png)

现在，我们可以通过 `chain.run()` 方法将产品描述（例如“Queen Size Sheet Set”）传递给链。链会在底层格式化提示，并将其传递给 LLM，最终返回结果（例如“Royal Beddings”）。

![](img/6979b54d544ad51cba7b8167af30399e_68.png)

![](img/6979b54d544ad51cba7b8167af30399e_70.png)

你可以尝试输入不同的产品描述，观察链的输出结果。

![](img/6979b54d544ad51cba7b8167af30399e_72.png)

![](img/6979b54d544ad51cba7b8167af30399e_74.png)

---

![](img/6979b54d544ad51cba7b8167af30399e_76.png)

![](img/6979b54d544ad51cba7b8167af30399e_78.png)

## 顺序链（Sequential Chains）

![](img/6979b54d544ad51cba7b8167af30399e_80.png)

![](img/6979b54d544ad51cba7b8167af30399e_82.png)

上一节我们介绍了基础的 LLM 链，本节中我们来看看如何将多个链按顺序连接起来，这就是**顺序链**。

![](img/6979b54d544ad51cba7b8167af30399e_84.png)

![](img/6979b54d544ad51cba7b8167af30399e_86.png)

顺序链会一个接一个地运行一系列链。我们首先导入 `SimpleSequentialChain`，它适用于子链都只有一个输入和一个输出的情况。

![](img/6979b54d544ad51cba7b8167af30399e_88.png)

![](img/6979b54d544ad51cba7b8167af30399e_90.png)

![](img/6979b54d544ad51cba7b8167af30399e_92.png)

我们将创建两个链：
1.  **第一个链**：输入产品，输出公司名称。
2.  **第二个链**：输入公司名称，输出一段20字的公司描述。

![](img/6979b54d544ad51cba7b8167af30399e_94.png)

![](img/6979b54d544ad51cba7b8167af30399e_96.png)

第一个链的输出（公司名称）会自动作为输入传递给第二个链。

![](img/6979b54d544ad51cba7b8167af30399e_98.png)

**核心代码示例**：
```python
from langchain.chains import SimpleSequentialChain

![](img/6979b54d544ad51cba7b8167af30399e_100.png)

![](img/6979b54d544ad51cba7b8167af30399e_102.png)

# 创建第一个链 (生成公司名)
chain_one = LLMChain(llm=llm, prompt=company_prompt)
# 创建第二个链 (生成公司描述)
chain_two = LLMChain(llm=llm, prompt=description_prompt)

![](img/6979b54d544ad51cba7b8167af30399e_104.png)

![](img/6979b54d544ad51cba7b8167af30399e_106.png)

# 组合成顺序链
overall_chain = SimpleSequentialChain(chains=[chain_one, chain_two], verbose=True)
```

![](img/6979b54d544ad51cba7b8167af30399e_108.png)

![](img/6979b54d544ad51cba7b8167af30399e_110.png)

当我们用“Queen Size Sheet Set”运行这个顺序链时，它会先输出“Royal Beddings”，然后将其传递给第二个链，生成关于该公司的描述。

![](img/6979b54d544ad51cba7b8167af30399e_112.png)

![](img/6979b54d544ad51cba7b8167af30399e_114.png)

---

![](img/6979b54d544ad51cba7b8167af30399e_116.png)

![](img/6979b54d544ad51cba7b8167af30399e_118.png)

## 通用顺序链（处理多输入/输出）

![](img/6979b54d544ad51cba7b8167af30399e_120.png)

![](img/6979b54d544ad51cba7b8167af30399e_122.png)

`SimpleSequentialChain` 在只有一个输入和输出时工作良好。但当链有多个输入或多个输出时，我们需要使用更通用的 `SequentialChain`。

![](img/6979b54d544ad51cba7b8167af30399e_124.png)

![](img/6979b54d544ad51cba7b8167af30399e_126.png)

![](img/6979b54d544ad51cba7b8167af30399e_128.png)

我们将创建一个处理产品评论的复杂流程，包含四个子链：
1.  **翻译链**：将评论翻译成英文。
2.  **总结链**：将英文评论总结成一句话。
3.  **语言检测链**：检测原始评论使用的语言。
4.  **回复链**：接受总结和语言信息，用指定语言生成对总结的回复。

![](img/6979b54d544ad51cba7b8167af30399e_130.png)

**关键点**：每个子链的 `input_key` 和 `output_key` 必须精确匹配，以确保数据能在链间正确传递。

![](img/6979b54d544ad51cba7b8167af30399e_132.png)

**核心代码示例（部分）**：
```python
from langchain.chains import SequentialChain

![](img/6979b54d544ad51cba7b8167af30399e_134.png)

![](img/6979b54d544ad51cba7b8167af30399e_136.png)

# 链1：翻译
translation_chain = LLMChain(llm=llm, prompt=translation_prompt, output_key=“english_review”)
# 链2：总结
summary_chain = LLMChain(llm=llm, prompt=summary_prompt, output_key=“summary”)
# 链3：语言检测
language_chain = LLMChain(llm=llm, prompt=language_prompt, output_key=“language”)
# 链4：生成回复
response_chain = LLMChain(llm=llm, prompt=response_prompt, output_key=“followup_message”)

![](img/6979b54d544ad51cba7b8167af30399e_138.png)

![](img/6979b54d544ad51cba7b8167af30399e_140.png)

# 组合成通用顺序链
overall_chain = SequentialChain(
    chains=[translation_chain, summary_chain, language_chain, response_chain],
    input_variables=[“review”],
    output_variables=[“english_review”, “summary”, “followup_message”],
    verbose=True
)
```

![](img/6979b54d544ad51cba7b8167af30399e_142.png)

![](img/6979b54d544ad51cba7b8167af30399e_144.png)

运行一条法语评论，我们可以看到它被翻译成英文、总结、检测出原始语言，最后用法语生成了回复。

![](img/6979b54d544ad51cba7b8167af30399e_146.png)

![](img/6979b54d544ad51cba7b8167af30399e_148.png)

---

![](img/6979b54d544ad51cba7b8167af30399e_150.png)

![](img/6979b54d544ad51cba7b8167af30399e_152.png)

![](img/6979b54d544ad51cba7b8167af30399e_154.png)

## 路由链（Router Chains）

![](img/6979b54d544ad51cba7b8167af30399e_156.png)

![](img/6979b54d544ad51cba7b8167af30399e_158.png)

我们已经学习了如何按顺序执行链。但有时，我们需要根据输入内容的不同，将其路由到不同的专用子链进行处理，这时就需要**路由链**。

![](img/6979b54d544ad51cba7b8167af30399e_160.png)

![](img/6979b54d544ad51cba7b8167af30399e_162.png)

想象一下，你有一个路由器链和多个子链（例如分别处理物理、数学、历史、计算机科学问题）。路由器链首先决定输入应该传递给哪个子链，然后进行路由。

![](img/6979b54d544ad51cba7b8167af30399e_164.png)

![](img/6979b54d544ad51cba7b8167af30399e_166.png)

**实现步骤**：
1.  为不同主题（物理、数学等）定义专用的提示模板。
2.  为每个提示模板提供名称和描述，供路由器链决策使用。
3.  创建对应的目的地链（LLM 链）。
4.  创建一个默认链，当路由器无法决定时使用。
5.  构建路由器链，它使用一个 LLM 来解析输入并决定路由目的地。
6.  将所有部分组合成最终的多提示链。

![](img/6979b54d544ad51cba7b8167af30399e_168.png)

![](img/6979b54d544ad51cba7b8167af30399e_170.png)

**核心代码示例（关键部分）**：
```python
from langchain.chains.router import MultiPromptChain
from langchain.chains.router.llm_router import LLMRouterChain, RouterOutputParser
from langchain.prompts import PromptTemplate

![](img/6979b54d544ad51cba7b8167af30399e_172.png)

![](img/6979b54d544ad51cba7b8167af30399e_174.png)

# 1. 定义不同主题的提示信息
destinations = [
    {“name”: “physics”, “description”: “适用于回答物理问题”, “prompt”: physics_prompt},
    # ... 其他主题
]

![](img/6979b54d544ad51cba7b8167af30399e_176.png)

# 2. 创建目的地链
destination_chains = {}
for d in destinations:
    prompt = d[“prompt”]
    chain = LLMChain(llm=llm, prompt=prompt)
    destination_chains[d[“name”]] = chain

![](img/6979b54d544ad51cba7b8167af30399e_178.png)

![](img/6979b54d544ad51cba7b8167af30399e_180.png)

# 3. 创建默认链
default_chain = LLMChain(llm=llm, prompt=default_prompt)

![](img/6979b54d544ad51cba7b8167af30399e_182.png)

![](img/6979b54d544ad51cba7b8167af30399e_184.png)

# 4. 构建路由器模板和链
router_template = “““根据用户问题，将其路由到最合适的提示模板...““”
router_prompt = PromptTemplate.from_template(router_template)
router_chain = LLMRouterChain.from_llm(llm, router_prompt, output_parser=RouterOutputParser())

![](img/6979b54d544ad51cba7b8167af30399e_186.png)

# 5. 组合成最终的多提示链
chain = MultiPromptChain(
    router_chain=router_chain,
    destination_chains=destination_chains,
    default_chain=default_chain,
    verbose=True
)
```

![](img/6979b54d544ad51cba7b8167af30399e_188.png)

![](img/6979b54d544ad51cba7b8167af30399e_190.png)

现在，当我们问一个物理问题（如“什么是黑体辐射？”），它会被路由到物理链并得到详细解答。如果问一个生物学问题（未定义专门链），则会被路由到默认链处理。

![](img/6979b54d544ad51cba7b8167af30399e_192.png)

![](img/6979b54d544ad51cba7b8167af30399e_194.png)

---

![](img/6979b54d544ad51cba7b8167af30399e_196.png)

## 总结 🎯

![](img/6979b54d544ad51cba7b8167af30399e_198.png)

![](img/6979b54d544ad51cba7b8167af30399e_200.png)

![](img/6979b54d544ad51cba7b8167af30399e_202.png)

本节课我们一起学习了 LangChain 中“链”这一核心概念：
1.  **LLM 链**：将 LLM 与提示模板结合的基本单元。
2.  **顺序链**：将多个链按顺序连接，`SimpleSequentialChain` 用于单输入输出，`SequentialChain` 用于处理多输入输出。
3.  **路由链**：根据输入内容，智能地将其路由到不同的专用子链进行处理。

这些基础构建块是创建复杂 LangChain 应用的基石。在接下来的课程中，我们将学习如何将这些链组合起来，构建更加强大和有趣的应用程序。