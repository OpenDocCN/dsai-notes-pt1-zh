# 024：从日记条目中提取餐厅信息 🍽️

在本节课中，我们将学习如何利用大语言模型从文本中提取特定信息，例如餐厅名称和菜品。我们将看到如何高亮显示这些信息，并将其保存为易于阅读和使用的格式，如HTML和CSV文件。

上一节我们介绍了文本分类，即判断一段文本是否与食物相关。本节中，我们将深入一步，看看如何让大语言模型从文档中提取特定的信息片段。

## 高亮显示餐厅与菜品信息

![](img/fbb3308f34a1be765ffecc93c9c97c62_1.png)

首先，我们来看如何从日记条目中提取餐厅和菜品信息，并用不同颜色高亮显示它们，以便于阅读。

以下代码用于读取里约热内卢的日记文件：

```python
journal_rio_de_janeiro = read_journal("Rio de Janeiro.txt")
```

![](img/fbb3308f34a1be765ffecc93c9c97c62_3.png)

接下来，我们构建一个提示词，要求大语言模型识别并高亮餐厅和菜品。我们指定输出格式为HTML，以便在网页中显示丰富的格式。

```python
prompt = f"""
Given the following journal entry from a food critic, identify all restaurants and the best dishes mentioned.
Highlight and bold each restaurant and best dish within the original text.
Provide the output as HTML suitable for display in a Jupyter notebook.
Journal entry: {journal_rio_de_janeiro}
"""
```

![](img/fbb3308f34a1be765ffecc93c9c97c62_5.png)

调用大语言模型获取响应，并显示生成的HTML：

```python
response_html = get_completion(prompt)
display(HTML(response_html))
```

![](img/fbb3308f34a1be765ffecc93c9c97c62_7.png)

运行后，输出结果会将餐厅名称标记为橙色，菜品名称标记为蓝色，并以HTML段落形式呈现。

![](img/fbb3308f34a1be765ffecc93c9c97c62_9.png)

我们对东京的日记条目执行相同的操作：

```python
journal_tokyo = read_journal("Tokyo.txt")
response_html_tokyo = get_completion(prompt)
display(HTML(response_html_tokyo))
```

尽管东京日记的格式与里约热内卢的截然不同，大语言模型依然能够成功处理并高亮相关信息。

![](img/fbb3308f34a1be765ffecc93c9c97c62_11.png)

![](img/fbb3308f34a1be765ffecc93c9c97c62_12.png)

你可以尝试修改提示词，例如将甜点高亮为绿色，或在每种食材旁添加表情符号。

## 提取信息并保存为CSV格式

![](img/fbb3308f34a1be765ffecc93c9c97c62_14.png)

除了高亮显示，我们还可以修改提示词，直接从文本中提取结构化的信息列表。

以下提示词要求模型提取所有餐厅及其对应菜品，并以CSV格式输出：

![](img/fbb3308f34a1be765ffecc93c9c97c62_16.png)

```python
prompt_extract = f"""
Extract the comprehensive list of restaurants and their respective dishes mentioned in the following journal entry.
Ensure each restaurant name is accurately identified and listed.
Provide your answer in CSV format, ready to save.
Journal entry: {journal_rio_de_janeiro}
"""
```

![](img/fbb3308f34a1be765ffecc93c9c97c62_17.png)

![](img/fbb3308f34a1be765ffecc93c9c97c62_18.png)

运行此提示词，会得到类似下面的CSV格式输出：

```
Restaurant, Dish
A Confeitaria Colombo, Coxinha
Porcão Rio's, Picanha
...
```

![](img/fbb3308f34a1be765ffecc93c9c97c62_20.png)

CSV（逗号分隔值）是一种常见的表格数据存储格式。第一行是列标题，后续每一行代表一条数据记录。

![](img/fbb3308f34a1be765ffecc93c9c97c62_22.png)

## 批量处理多个日记文件

为了高效处理多个文件，我们可以使用循环。以下是处理一系列城市日记文件的代码示例：

![](img/fbb3308f34a1be765ffecc93c9c97c62_24.png)

```python
files = ["Cape Town.txt", "Istanbul.txt", "New York.txt", "Paris.txt", "Rio de Janeiro.txt", "Sydney.txt", "Tokyo.txt"]

![](img/fbb3308f34a1be765ffecc93c9c97c62_25.png)

![](img/fbb3308f34a1be765ffecc93c9c97c62_26.png)

![](img/fbb3308f34a1be765ffecc93c9c97c62_27.png)

for file in files:
    journal = read_journal(file)
    response = get_completion(prompt_extract)
    print(f"--- {file} ---")
    print(response)
    print()  # 打印空行分隔
```

![](img/fbb3308f34a1be765ffecc93c9c97c62_29.png)

这段代码会依次读取列表中的每个文件，提取餐厅和菜品信息，并打印出CSV格式的结果。这展示了当需要分析的文件数量增加时，大语言模型如何显著提升效率。

![](img/fbb3308f34a1be765ffecc93c9c97c62_31.png)

## 将数据保存到文件

处理完数据后，我们可能需要将其保存到本地文件中以供后续使用。

![](img/fbb3308f34a1be765ffecc93c9c97c62_33.png)

以下是如何将之前生成的HTML响应保存到一个新文件中：

![](img/fbb3308f34a1be765ffecc93c9c97c62_35.png)

```python
# 以写入模式打开文件
f = open("highlighted_rio.html", "w")
# 将HTML内容写入文件
f.write(response_html)
# 关闭文件
f.close()
```

![](img/fbb3308f34a1be765ffecc93c9c97c62_37.png)

![](img/fbb3308f34a1be765ffecc93c9c97c62_39.png)

与读取文件的主要区别在于，打开文件时使用模式 `"w"`（写入）而非 `"r"`（读取），并使用 `f.write()` 方法写入内容。

![](img/fbb3308f34a1be765ffecc93c9c97c62_41.png)

保存后，你可以在本地使用网页浏览器打开这个 `.html` 文件，查看高亮格式化的结果。

## 总结

本节课中我们一起学习了如何利用大语言模型进行信息提取。我们掌握了两个核心技能：
1.  **高亮关键信息**：通过构造特定的提示词，让模型在原文中识别并高亮显示餐厅和菜品，输出为美观的HTML格式。
2.  **提取结构化数据**：修改提示词，让模型提取出餐厅和菜品的列表，并以CSV格式输出，便于后续分析和使用。
此外，我们还学习了如何使用Python代码将模型生成的结果保存到本地文件中。

![](img/fbb3308f34a1be765ffecc93c9c97c62_43.png)

在下一节课中，我们将学习如何加载本节课生成的这种CSV格式数据，并利用它来帮助规划你自己的假期行程。请务必完成本Jupyter笔记本末尾的练习，我们下节课再见。