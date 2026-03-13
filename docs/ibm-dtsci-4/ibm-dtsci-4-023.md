# 023：网络数据抓取 🕸️

![](img/18d0d7bb642f40443621e412bc7eb97c_0.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_1.png)

在本节课中，我们将要学习网络数据抓取。网络数据抓取是一个可以自动从网站提取信息的过程，它能在几分钟内完成原本需要数小时的手动工作。

## 什么是网络数据抓取？ 🤔

如果你需要分析数百个数据点来找出体育队伍中的最佳球员，你会怎么做？你会开始从不同网站手动复制粘贴信息到电子表格吗？还是会花费数小时寻找正确数据，最终因为任务过于繁重而放弃？网络数据抓取正是在这些场景下能提供帮助的工具。

网络数据抓取是一个用于自动从网站提取信息的过程，可以轻松地在几分钟内完成，而不是数小时。

![](img/18d0d7bb642f40443621e412bc7eb97c_3.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_4.png)

## 开始使用：Requests与Beautiful Soup 🚀

![](img/18d0d7bb642f40443621e412bc7eb97c_5.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_6.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_7.png)

要开始网络数据抓取，我们只需要一点Python代码以及两个名为`requests`和`beautifulsoup4`的模块的帮助。

![](img/18d0d7bb642f40443621e412bc7eb97c_8.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_9.png)

假设你被要求从以下网页中找出国家篮球联赛球员的姓名和薪水。

首先，我们导入Beautiful Soup。我们可以将网页HTML作为字符串存储在变量`html`中。要解析文档，将其传递给Beautiful Soup构造函数。我们会得到一个Beautiful Soup对象`soup`，它将文档表示为嵌套的数据结构。

Beautiful Soup将HTML表示为一组树状对象，并提供了用于解析HTML的方法。

## 理解Beautiful Soup对象 🌳

使用我们创建的Beautiful Soup对象`soup`，标签对象对应于原始文档中的HTML标签。例如，`title`标签。

![](img/18d0d7bb642f40443621e412bc7eb97c_11.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_12.png)

考虑`<h3>`标签。如果存在多个具有相同名称的标签，则选择具有该标签的第一个元素。在本例中，是“勒布朗·詹姆斯”。

![](img/18d0d7bb642f40443621e412bc7eb97c_13.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_14.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_15.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_16.png)

我们看到姓名被包裹在粗体属性`<b>`中。要提取它，需要使用树状表示法。

![](img/18d0d7bb642f40443621e412bc7eb97c_17.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_18.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_19.png)

变量`tag_object`位于此处。我们可以访问标签的子元素，或者沿着分支向下导航，如下所示：

```python
tag_object.child
```

你可以通过使用`parent`属性在树中向上导航。变量`tag_child`位于此处，我们可以访问其父元素。这就是原始的`tag_object`。

![](img/18d0d7bb642f40443621e412bc7eb97c_21.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_22.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_23.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_24.png)

我们还可以找到`tag_object`的兄弟元素。我们只需使用`next_sibling`属性。

![](img/18d0d7bb642f40443621e412bc7eb97c_25.png)

我们可以找到`sibling1`的兄弟元素。我们只需使用`next_sibling`属性。

考虑`tag_child`对象。你可以像字典中的键值对一样访问属性名称和值，如下所示：

```python
tag_child.attrs
```

![](img/18d0d7bb642f40443621e412bc7eb97c_27.png)

你可以将内容作为可导航字符串返回。这类似于支持Beautiful Soup功能的Python字符串。

![](img/18d0d7bb642f40443621e412bc7eb97c_28.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_29.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_30.png)

## 探索`find_all`方法 🔍

现在，让我们回顾一下`find_all`方法。这是一个过滤器。

你可以使用过滤器基于标签名称、其属性、字符串文本或这些条件的某种组合进行过滤。

考虑披萨店的列表。像之前一样，创建一个Beautiful Soup对象，但这次将其命名为`table`。

![](img/18d0d7bb642f40443621e412bc7eb97c_32.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_33.png)

`find_all`方法会遍历标签的后代，并检索所有与你的过滤器匹配的后代。将其应用于标签为`<tr>`的`table`。结果是一个Python可迭代对象，就像一个列表。

![](img/18d0d7bb642f40443621e412bc7eb97c_34.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_35.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_36.png)

每个元素都是`<tr>`的标签对象。这对应于列表中的每一行，包括表头。

![](img/18d0d7bb642f40443621e412bc7eb97c_37.png)

每个元素都是一个标签对象。因此，考虑第一行。例如，我们可以提取第一个表格单元格。

我们也可以遍历每个表格单元格。首先，我们通过变量`row`遍历列表`tr_rows`。每个元素对应于表格中的一行。

我们可以应用`find_all`方法来查找所有表格单元格。然后，我们可以为每一行遍历变量`cells`。

在每次迭代中，变量`cell`对应于该特定行表格中的一个元素，我们继续遍历每个元素，并为每一行重复此过程。

## 将Beautiful Soup应用于网页 🌐

![](img/18d0d7bb642f40443621e412bc7eb97c_39.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_40.png)

让我们看看如何将Beautiful Soup应用于网页以进行抓取。要抓取网页，我们还需要`requests`库。

![](img/18d0d7bb642f40443621e412bc7eb97c_41.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_42.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_43.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_44.png)

第一步是导入所需的模块。使用`requests`库中的`get`方法来下载网页。输入是URL。

![](img/18d0d7bb642f40443621e412bc7eb97c_45.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_46.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_47.png)

使用`text`属性获取文本，并将其分配给变量`page`，然后从变量`page`创建一个Beautiful Soup对象`soup`。

![](img/18d0d7bb642f40443621e412bc7eb97c_48.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_49.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_50.png)

这将允许你解析HTML页面，现在你可以开始抓取页面了。请查看实验部分以获取更多信息。

## 总结 📝

![](img/18d0d7bb642f40443621e412bc7eb97c_52.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_53.png)

![](img/18d0d7bb642f40443621e412bc7eb97c_54.png)

本节课中，我们一起学习了网络数据抓取的基础知识。我们定义了网络数据抓取，了解了Beautiful Soup对象的作用，学习了如何应用`find_all`方法，并掌握了抓取网站的基本步骤。通过结合使用`requests`和`beautifulsoup4`库，你可以高效地从网页中提取所需的结构化数据。