# 024：多种文件格式处理（CSV/XML/JSON/XLSX）

![](img/3694168a4f31e31a6990ca815878b938_0.png)

![](img/3694168a4f31e31a6990ca815878b938_1.png)

在本节课中，我们将学习如何使用Python处理多种常见的数据文件格式，包括CSV、JSON、XML和XLSX。你将了解如何识别这些文件，以及使用哪些Python库来读取和提取其中的数据。

---

![](img/3694168a4f31e31a6990ca815878b938_3.png)

![](img/3694168a4f31e31a6990ca815878b938_4.png)

## 认识不同的文件格式

在收集数据时，你会发现需要处理多种不同的文件格式，才能完成数据驱动的分析或故事叙述。Python通过其预定义的库，可以使这个过程变得更简单。但在探索Python之前，让我们先了解一些常见的文件格式。

![](img/3694168a4f31e31a6990ca815878b938_6.png)

观察文件名时，你会注意到标题末尾有一个扩展名。这些扩展名让你知道文件的类型以及打开它需要什么。例如，如果你看到一个标题如 `file_example.csv`，你会知道这是一个CSV文件。但这只是不同文件类型的一个例子，还有更多类型，例如JSON或XML。

![](img/3694168a4f31e31a6990ca815878b938_7.png)

![](img/3694168a4f31e31a6990ca815878b938_8.png)

![](img/3694168a4f31e31a6990ca815878b938_9.png)

---

## 使用Python库读取数据

当遇到这些不同的文件格式并试图访问其中的数据时，我们需要利用Python库来简化这个过程。首先要熟悉的Python库是 **pandas**。通过在代码开头导入这个库，我们就能轻松读取不同类型的文件。

![](img/3694168a4f31e31a6990ca815878b938_11.png)

![](img/3694168a4f31e31a6990ca815878b938_12.png)

![](img/3694168a4f31e31a6990ca815878b938_13.png)

```python
import pandas as pd
```

![](img/3694168a4f31e31a6990ca815878b938_14.png)

![](img/3694168a4f31e31a6990ca815878b938_15.png)

既然我们已经导入了pandas库，让我们用它来读取第一个CSV文件。在这个例子中，我们遇到了 `file_example.csv` 文件。第一步是将文件分配给一个变量，然后创建另一个变量，借助pandas库来读取文件。我们可以调用 `read_csv` 函数将数据输出到屏幕。

```python
file_path = 'file_example.csv'
df = pd.read_csv(file_path)
print(df)
```

![](img/3694168a4f31e31a6990ca815878b938_17.png)

在这个例子中，数据没有表头，所以它将第一行数据作为了表头。由于我们不希望第一行数据作为表头，让我们看看如何纠正这个问题。

![](img/3694168a4f31e31a6990ca815878b938_18.png)

![](img/3694168a4f31e31a6990ca815878b938_19.png)

![](img/3694168a4f31e31a6990ca815878b938_20.png)

---

## 整理CSV数据输出

上一节我们学习了如何读取和输出CSV文件的数据，现在让我们让它看起来更有条理一些。

![](img/3694168a4f31e31a6990ca815878b938_22.png)

![](img/3694168a4f31e31a6990ca815878b938_23.png)

![](img/3694168a4f31e31a6990ca815878b938_24.png)

在上一个例子中，我们能够打印出数据，但因为文件没有表头，它将第一行数据打印成了表头。我们可以通过添加一个数据框属性来轻松解决这个问题。我们使用变量 `df` 来调用文件，然后添加 `columns` 属性。通过在我们的程序中添加这一行，我们可以将数据输出整齐地组织到每列指定的表头下。

![](img/3694168a4f31e31a6990ca815878b938_25.png)

![](img/3694168a4f31e31a6990ca815878b938_26.png)

```python
df.columns = ['Column1', 'Column2', 'Column3']  # 根据实际列数命名
print(df)
```

---

![](img/3694168a4f31e31a6990ca815878b938_28.png)

## 处理JSON文件格式

![](img/3694168a4f31e31a6990ca815878b938_30.png)

![](img/3694168a4f31e31a6990ca815878b938_31.png)

接下来我们将探索的文件格式是JSON文件格式。在这种类型的文件中，文本是以一种独立于语言的数据格式编写的，类似于Python字典。读取此类文件的第一步是导入 `json` 库。

```python
import json
```

导入json后，我们可以添加一行代码来打开文件，调用json的 `load` 属性开始读取文件，最后我们可以打印文件内容。

![](img/3694168a4f31e31a6990ca815878b938_33.png)

![](img/3694168a4f31e31a6990ca815878b938_34.png)

![](img/3694168a4f31e31a6990ca815878b938_35.png)

```python
with open('file_example.json', 'r') as f:
    data = json.load(f)
print(data)
```

![](img/3694168a4f31e31a6990ca815878b938_36.png)

![](img/3694168a4f31e31a6990ca815878b938_37.png)

![](img/3694168a4f31e31a6990ca815878b938_38.png)

---

## 处理XML文件格式

![](img/3694168a4f31e31a6990ca815878b938_40.png)

![](img/3694168a4f31e31a6990ca815878b938_41.png)

下一个文件格式类型是XML，即可扩展标记语言。虽然pandas库没有直接读取此类文件的属性，但让我们探索如何解析这种类型的文件。

读取此类文件的第一步是导入 `xml.etree.ElementTree` 库。通过导入这个库，我们可以使用 `ElementTree` 属性来解析XML文件。然后我们添加列标题并将它们分配给数据框。

```python
import xml.etree.ElementTree as ET
import pandas as pd

![](img/3694168a4f31e31a6990ca815878b938_43.png)

![](img/3694168a4f31e31a6990ca815878b938_44.png)

![](img/3694168a4f31e31a6990ca815878b938_45.png)

tree = ET.parse('file_example.xml')
root = tree.getroot()

![](img/3694168a4f31e31a6990ca815878b938_46.png)

![](img/3694168a4f31e31a6990ca815878b938_47.png)

# 定义列标题
columns = ['Column1', 'Column2', 'Column3']
data = []

# 创建一个循环来遍历文档以收集必要的数据，并将数据附加到数据框中
for elem in root.findall('.//record'):  # 根据实际XML结构调整路径
    row = []
    for col in columns:
        # 假设每个记录下有对应标签
        cell = elem.find(col).text if elem.find(col) is not None else ''
        row.append(cell)
    data.append(row)

![](img/3694168a4f31e31a6990ca815878b938_49.png)

![](img/3694168a4f31e31a6990ca815878b938_50.png)

df = pd.DataFrame(data, columns=columns)
print(df)
```

![](img/3694168a4f31e31a6990ca815878b938_52.png)

---

![](img/3694168a4f31e31a6990ca815878b938_54.png)

## 课程总结

![](img/3694168a4f31e31a6990ca815878b938_55.png)

![](img/3694168a4f31e31a6990ca815878b938_56.png)

![](img/3694168a4f31e31a6990ca815878b938_57.png)

本节课中，我们一起学习了如何识别不同的文件类型，如何使用Python库来提取数据，以及在收集数据时如何使用数据框。我们掌握了处理CSV、JSON和XML文件的基本方法，这是进行数据科学分析的重要基础技能。