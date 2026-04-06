# 021：在Python中使用文件 📂

在本节课中，我们将要学习如何使用Python读取存储在计算机上的文件。你将了解如何打开文本文件、读取其中的内容，并将其用于AI处理，例如让大型语言模型总结一封电子邮件。

---

数据通常以文件形式存储在计算机上，例如文档、待办事项列表或笔记。这些笔记可能是在记事本或文本编辑器中记录的。实际上，许多应用程序（如Obsidian）也使用文本文件来存储数据。你可以使用Python读取这些文件中的文本数据，从而编写代码，利用AI处理你自己文件中的内容。

上一节我们介绍了如何在Jupyter笔记本中直接创建和操作数据。本节中我们来看看如何从外部文件加载数据。

![](img/406f5ee82c03292318d21551056fb0a0_1.png)

## 从文本文件读取数据

![](img/406f5ee82c03292318d21551056fb0a0_3.png)

之前，我们直接在代码中创建了字符串、列表等数据。例如，以下代码创建了一个食材列表，并让AI根据它生成食谱：

```python
from IPython.display import display, Markdown

ingredients = ["chicken", "broccoli", "rice"]
prompt = f"Create a recipe using these ingredients: {', '.join(ingredients)}"
response = get_completion(prompt)
print(response)
```

然而，我们也可以从存储在计算机上的文本文件中加载数据。让我们从读取一个名为 `email.txt` 的文本文件开始。

![](img/406f5ee82c03292318d21551056fb0a0_5.png)

![](img/406f5ee82c03292318d21551056fb0a0_6.png)

以下是读取文件内容的基本步骤：

![](img/406f5ee82c03292318d21551056fb0a0_8.png)

![](img/406f5ee82c03292318d21551056fb0a0_10.png)

```python
f = open("email.txt", "r")
email = f.read()
f.close()
print(email)
```

这段代码执行了以下操作：
1.  `open("email.txt", "r")`：以**读取模式**打开名为 `email.txt` 的文件，并将这个“打开的文件对象”赋值给变量 `f`。
2.  `email = f.read()`：读取文件 `f` 中的**全部文本内容**，并将其作为一个字符串存储在变量 `email` 中。
3.  `f.close()`：操作完成后**关闭文件**。这是一个好习惯，可以释放计算机内存。

运行这段代码后，文件 `email.txt` 中的所有文本就被加载到了变量 `email` 中。打印 `email` 即可查看其内容，例如一封来自Daniel的电子邮件。

![](img/406f5ee82c03292318d21551056fb0a0_12.png)

## 使用AI处理文件内容

![](img/406f5ee82c03292318d21551056fb0a0_14.png)

现在，我们已经将文本加载到Python变量中，可以方便地使用AI模型进行处理。

例如，我们可以让AI总结这封电子邮件。以下是构建提示词并获取响应的过程：

![](img/406f5ee82c03292318d21551056fb0a0_16.png)

![](img/406f5ee82c03292318d21551056fb0a0_17.png)

![](img/406f5ee82c03292318d21551056fb0a0_19.png)

```python
prompt = f"""
Extract bullet points from the following email and include the sender information:
{email}
"""
bullet_points = get_completion(prompt)
print(bullet_points)
```

![](img/406f5ee82c03292318d21551056fb0a0_21.png)

AI模型会生成一个包含关键信息的要点列表。你可能会注意到输出中包含了 `**` 这样的符号，这是一种名为 **Markdown** 的轻量级标记语言，用于格式化文本。

如果你想在Jupyter笔记本中更美观地显示Markdown格式的文本，可以使用 `display` 和 `Markdown` 函数。这就是为什么在本笔记本开头需要先导入它们：

```python
from IPython.display import display, Markdown
display(Markdown(bullet_points))
```

![](img/406f5ee82c03292318d21551056fb0a0_23.png)

这会使输出以清晰的格式化列表呈现，更易于阅读。关于 `import` 命令的更多细节，我们将在后续课程中学习。

---

![](img/406f5ee82c03292318d21551056fb0a0_25.png)

![](img/406f5ee82c03292318d21551056fb0a0_26.png)

本节课中我们一起学习了如何使用Python打开、读取和关闭文本文件，并将文件内容传递给AI模型进行处理。在下一课中，我们将看到如何对你自己的文本文件进行这些操作。