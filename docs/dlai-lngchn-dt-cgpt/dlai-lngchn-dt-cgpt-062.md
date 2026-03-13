# 062：使用大语言模型应对技术债务 📚

![](img/915f4e03e9426f118b05e783568bb8c3_0.png)

![](img/915f4e03e9426f118b05e783568bb8c3_2.png)

在本节课中，我们将学习如何利用大语言模型来理解和处理软件开发中的“技术债务”。我们将通过一个具体的Swift代码示例，演示如何使用LLM来解释复杂代码并自动生成技术文档。

![](img/915f4e03e9426f118b05e783568bb8c3_4.png)

![](img/915f4e03e9426f118b05e783568bb8c3_6.png)

## 概述

![](img/915f4e03e9426f118b05e783568bb8c3_8.png)

![](img/915f4e03e9426f118b05e783568bb8c3_10.png)

技术债务是软件开发中普遍存在的挑战，它随着时间推移由开发者传递下来。这些代码通常难以维护，因为它们编写已久、过于复杂或依赖关系太多，修改时存在系统崩溃的风险。大语言模型（LLMs）为解决这一问题提供了强大的辅助工具。

![](img/915f4e03e9426f118b05e783568bb8c3_12.png)

上一节我们介绍了技术债务的概念，本节中我们来看看如何利用LLMs来理解和解释一段复杂的遗留代码。

## 环境设置与代码准备

首先，我们需要设置API环境并导入必要的库。以下是配置步骤：

```python
import google.generativeai as palm
from google.api_core import retry

# 配置Palm库的API密钥
palm.configure(api_key='YOUR_API_KEY')

# 查看可用的模型
models = palm.list_models()
for model in models:
    print(model.name)
```

![](img/915f4e03e9426f118b05e783568bb8c3_14.png)

![](img/915f4e03e9426f118b05e783568bb8c3_16.png)

我们将使用文本生成模型，例如 `models/text-bison-001`。为了获得确定性的输出，我们创建一个辅助函数并将温度参数设置为0。

```python
@retry.Retry()
def generate_text(prompt, model=None, temperature=0.0):
    # 调用模型生成文本，覆盖默认温度以减少随机性
    completion = palm.generate_text(
        model=model,
        prompt=prompt,
        temperature=temperature
    )
    return completion.result
```

![](img/915f4e03e9426f118b05e783568bb8c3_18.png)

![](img/915f4e03e9426f118b05e783568bb8c3_20.png)

## 分析复杂代码示例

![](img/915f4e03e9426f118b05e783568bb8c3_22.png)

现在，我们准备开始探索代码。这里有一段非常复杂的Swift代码，它摘自一本关于在移动设备上创建机器学习模型的书籍。这段代码的作用是从iOS设备获取图像，将其传递给TensorFlow Lite模型进行分类。

![](img/915f4e03e9426f118b05e783568bb8c3_24.png)

其复杂性在于需要进行图像转换，这涉及到读取图像的底层内存。在移动开发中，直接访问内存可能触发操作系统的安全机制，因此代码使用了复杂且安全的API来安全地访问iPhone的设备内存。

![](img/915f4e03e9426f118b05e783568bb8c3_26.png)

![](img/915f4e03e9426f118b05e783568bb8c3_28.png)

这段代码量很大，是一个典型的技术债务例子。如果有人构建了包含此代码的应用，后来者继承时很难快速理解其工作原理和如何使用它。

![](img/915f4e03e9426f118b05e783568bb8c3_30.png)

## 使用LLM解释代码

![](img/915f4e03e9426f118b05e783568bb8c3_32.png)

我们可以使用大语言模型来帮助理解这段代码。以下是操作步骤：

![](img/915f4e03e9426f118b05e783568bb8c3_34.png)

![](img/915f4e03e9426f118b05e783568bb8c3_36.png)

首先，我们构建一个提示词模板，要求模型解释代码的工作原理。

![](img/915f4e03e9426f118b05e783568bb8c3_38.png)

```python
prompt_template = """
请解释以下Swift代码是如何工作的。请提供大量细节，并尽可能清晰地说明。

{code_block}
"""

![](img/915f4e03e9426f118b05e783568bb8c3_40.png)

![](img/915f4e03e9426f118b05e783568bb8c3_42.png)

# 将我们的复杂Swift代码填入模板
code_explanation_prompt = prompt_template.format(code_block=complex_swift_code)
```

![](img/915f4e03e9426f118b05e783568bb8c3_44.png)

然后，我们调用生成函数来获取解释。

![](img/915f4e03e9426f118b05e783568bb8c3_46.png)

```python
explanation = generate_text(prompt=code_explanation_prompt, model=selected_model, temperature=0.0)
print(explanation)
```

![](img/915f4e03e9426f118b05e783568bb8c3_48.png)

模型会输出非常详细的解释。例如，它会说明`ModelDataProcessor`是一个TensorFlow模型处理器，列出其属性和初始化器，并解释各个方法的功能。它甚至能识别出Swift特有的扩展方法（`extension`），并解释这些扩展如何安全地复制内存缓冲区，以及为何这种方式能通过应用商店的审核。

![](img/915f4e03e9426f118b05e783568bb8c3_50.png)

## 使用LLM生成技术文档

![](img/915f4e03e9426f118b05e783568bb8c3_52.png)

![](img/915f4e03e9426f118b05e783568bb8c3_54.png)

除了解释代码，大语言模型还可以帮助我们自动生成技术文档。这对于减少技术债务至关重要。

![](img/915f4e03e9426f118b05e783568bb8c3_56.png)

以下是生成技术文档的步骤：

我们从一个新的提示模板开始，要求模型为代码编写技术文档。

```python
documentation_prompt_template = """
请为以下代码编写技术文档。要求文档易于非Swift开发者理解，并以Markdown格式输出。

![](img/915f4e03e9426f118b05e783568bb8c3_58.png)

{code_block}
"""

![](img/915f4e03e9426f118b05e783568bb8c3_60.png)

doc_prompt = documentation_prompt_template.format(code_block=complex_swift_code)
```

![](img/915f4e03e9426f118b05e783568bb8c3_62.png)

![](img/915f4e03e9426f118b05e783568bb8c3_64.png)

接着，我们生成文档内容。

```python
technical_documentation = generate_text(prompt=doc_prompt, model=selected_model, temperature=0.0)
print(technical_documentation)
```

模型生成的文档会采用Markdown格式，包含清晰的标题（如`# ModelDataProcessor`）、项目符号列表来解释公共属性和方法。这为我们提供了一个可以直接使用的技术文档初稿，极大地节省了手动编写文档的时间。

## 总结

本节课中，我们一起学习了如何利用大语言模型来应对软件开发中的技术债务。我们通过一个具体的Swift代码案例，演示了如何使用LLM来完成两项关键任务：

1.  **解释复杂代码**：LLM能够详细解析代码的功能、结构和关键部分，帮助开发者快速理解遗留代码。
2.  **自动生成文档**：LLM可以生成结构清晰、易于理解的技术文档（如Markdown格式），为代码维护和团队协作奠定基础。

![](img/915f4e03e9426f118b05e783568bb8c3_66.png)

通过这种方法，我们可以显著降低理解、维护和迭代复杂代码库的成本与风险。你可以尝试将这项技术应用到你自己的项目中，探索LLM在减少技术债务方面的更多可能性。