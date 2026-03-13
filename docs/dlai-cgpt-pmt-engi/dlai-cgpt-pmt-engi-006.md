# 006：文本转换

![](img/ed21d6cb87e54be3aa36b62268b33e0d_0.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_2.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_4.png)

在本节课中，我们将学习如何利用大型语言模型进行文本转换。这包括语言翻译、语气转换、格式转换以及拼写和语法检查。通过学习这些技巧，你可以让模型帮助你处理多种文本处理任务。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_6.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_8.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_10.png)

---

大型语言模型非常擅长将其输入转换为不同的格式。例如，将一种语言的文本转换成另一种语言，或者帮助进行拼写和语法纠正。输入一段可能不完全符合语法的文字，模型可以帮你整理。它还能转换格式，例如将HTML转换为JSON。这些应用在过去实现起来可能比较复杂，但现在使用大型语言模型和一些提示词，过程会简单得多。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_12.png)

## 🌍 语言翻译

![](img/ed21d6cb87e54be3aa36b62268b33e0d_14.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_16.png)

上一节我们介绍了文本转换的基本概念，本节中我们来看看具体的翻译任务。大型语言模型在来自互联网等多种来源的大量文本上训练，其中包含许多不同的语言。这赋予了模型翻译的能力，使其能以不同的熟练程度处理数百种语言。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_18.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_20.png)

以下是几个翻译示例：

![](img/ed21d6cb87e54be3aa36b62268b33e0d_22.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_24.png)

**示例1：英译西**
*   **提示**：将以下英文文本翻译成西班牙文：`Hi, I'd like to order a blender.`
*   **回应**：`Hola, me gustaría ordenar una licuadora.`

![](img/ed21d6cb87e54be3aa36b62268b33e0d_26.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_28.png)

**示例2：语言识别**
*   **提示**：告诉我这是什么语言：`Combien coûte le lampadaire?`
*   **回应**：`这是法语。`

该模型还可以一次性处理多个翻译任务。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_30.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_32.png)

**示例3：多语言翻译**
*   **提示**：翻译下面的文本为英语和西班牙语：`I want to order a basketball.`
*   **回应**：
    *   英语：`I want to order a basketball.`
    *   西班牙语：`Quiero ordenar un balón de baloncesto.`

![](img/ed21d6cb87e54be3aa36b62268b33e0d_34.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_36.png)

在某些语言中，翻译会根据说话者与听者的关系而变化。你也可以向语言模型解释这一点，使其能够进行相应的翻译。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_38.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_39.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_41.png)

**示例4：正式与非正式翻译**
*   **提示**：将下列文本分别翻译成西班牙语的正式和非正式形式：`¿Quieres pedir una almohada?`
*   **回应**：
    *   正式：`¿Desea usted pedir una almohada?`
    *   非正式：`¿Quieres pedir una almohada?`

![](img/ed21d6cb87e54be3aa36b62268b33e0d_43.png)

### 构建通用翻译器

![](img/ed21d6cb87e54be3aa36b62268b33e0d_45.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_47.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_49.png)

假设我们负责一家跨国电子商务公司，用户会用各种语言提交信息。我们需要一个通用翻译器来处理这些问题。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_51.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_53.png)

以下是实现步骤：

![](img/ed21d6cb87e54be3aa36b62268b33e0d_55.png)

1.  首先，我们准备一个包含不同语言用户消息的列表。
    ```python
    user_messages = [
        "La performance du système est plus lente que d'habitude.",
        "Mi monitor tiene píxeles que no se iluminan.",
        "Il mio mouse non funziona",
        "Mój klawisz Ctrl jest zepsuty",
        "我的屏幕在闪烁"
    ]
    ```
2.  然后，我们循环处理每条消息，让模型识别语言并将其翻译成目标语言（如英语和韩语）。
    ```python
    for issue in user_messages:
        prompt = f"""请告诉我以下文本是什么语言，并将其翻译成英语和韩语：
        ```{issue}```
        """
        response = get_completion(prompt)
        print(response)
    ```
通过这种方式，你可以快速构建一个通用翻译器。你可以随时暂停，添加其他你想尝试的语言，看看模型表现如何。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_57.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_59.png)

## ✍️ 语气与格式转换

写作可以根据预期受众的不同而调整。例如，给同事的邮件和给弟弟的短信语气会截然不同。ChatGPT可以帮助生成不同的语气。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_61.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_63.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_65.png)

**示例5：俚语转商务信函**
*   **提示**：将以下内容从俚语翻译成商务信函：`Dude, This is Joe, check out this spec on the standing lamp.`
*   **回应**：`Dear Sir/Madam, This is Joe. Please review the specifications for the standing lamp.`

![](img/ed21d6cb87e54be3aa36b62268b33e0d_66.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_68.png)

### 格式转换

![](img/ed21d6cb87e54be3aa36b62268b33e0d_70.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_72.png)

ChatGPT非常擅长在不同格式之间进行转换，例如JSON到HTML、XML、Markdown等。在提示中，我们需要清晰描述输入和输出格式。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_74.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_76.png)

**示例6：JSON转HTML表格**
*   **输入数据**：
    ```json
    {
        "restaurant employees": [
            {"name": "Shyam", "email": "shyamjaiswal@gmail.com"},
            {"name": "Bob", "email": "bob32@gmail.com"},
            {"name": "Jai", "email": "jai87@gmail.com"}
        ]
    }
    ```
*   **提示**：将下面的Python字典从JSON转换为带有列标题和表头的HTML表格。
*   **模型响应**：模型会生成对应的HTML代码。我们可以使用Python的`display`函数来查看渲染后的表格效果。
    ```python
    from IPython.display import display, HTML
    display(HTML(html_string))
    ```

![](img/ed21d6cb87e54be3aa36b62268b33e0d_78.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_79.png)

## ✅ 拼写与语法检查

![](img/ed21d6cb87e54be3aa36b62268b33e0d_81.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_83.png)

拼写和语法检查是ChatGPT非常流行的用途。对于非母语写作者尤其有帮助。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_85.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_87.png)

以下是实现步骤：

![](img/ed21d6cb87e54be3aa36b62268b33e0d_89.png)

1.  我们准备一个包含语法或拼写错误的句子列表。
    ```python
    texts = [
        "The girl with the black and white puppies have a ball.",
        "Yolanda has her notebook.",
        "Its going to be a long day. Does the car need it’s oil changed?",
        "Their goes my freedom. There going to bring they’re suitcases."
    ]
    ```
2.  循环每个句子，让模型进行校对和纠正。
    ```python
    for text in texts:
        prompt = f"""请校对并更正以下文本。如果未发现错误，请说“未发现错误”：
        ```{text}```
        """
        response = get_completion(prompt)
        print(f"原始文本: {text}")
        print(f"更正后: {response}\n")
    ```
通过迭代提示词，你可以开发出更可靠、每次都能工作的校对提示。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_91.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_93.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_95.png)

### 进阶应用：检查与润色评论

![](img/ed21d6cb87e54be3aa36b62268b33e0d_97.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_99.png)

在将内容发布到公共论坛前进行检查总是有用的。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_101.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_103.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_105.png)

**示例7：校对产品评论**
*   **原始评论**：`Got this for my daughter for her birthday cuz she keeps taking mine from my room. Yes, adults also like pandas too. She takes it everywhere with her, and it's super soft and cute. One of the ears is a bit lower than the other, and I don't think that was designed to be asymmetrical. It's a bit small for what I paid for it though. I think there might be other options that are bigger for the same price. It arrived a day earlier than expected, so I got to play with it myself before I gave it to her.`
*   **提示**：`请校对并纠正这篇评论。`
*   **模型响应**：模型会输出语法更正确、表达更清晰的版本。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_107.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_109.png)

我们甚至可以要求模型进行更大幅度的修改，例如改变语气或遵循特定风格。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_111.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_113.png)

**示例8：润色并转换风格**
*   **提示**：`请校对并纠正以下评论。同时，使其更引人注目，遵循APA风格，以高级读者为目标，并以Markdown格式输出。`
*   **模型响应**：模型会生成一篇扩展过的、符合APA风格的、面向高级读者的产品评论。

![](img/ed21d6cb87e54be3aa36b62268b33e0d_115.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_117.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_119.png)

---

![](img/ed21d6cb87e54be3aa36b62268b33e0d_120.png)

![](img/ed21d6cb87e54be3aa36b62268b33e0d_122.png)

本节课中我们一起学习了如何利用大型语言模型进行多种文本转换操作。我们涵盖了**语言翻译**、**语气转换**、**格式转换**以及**拼写和语法检查**。通过清晰的提示词，你可以指导模型完成这些任务，从而更高效地处理文本内容。在接下来的课程中，我们将探索如何让模型进行文本扩展，即根据较短的提示生成更长的内容。