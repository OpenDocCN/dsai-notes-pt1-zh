# 004：可选网络抓取 🕸️

![](img/64afc6d80ff3abd2216cf6da2d84a609_0.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_1.png)

在本节课中，我们将学习网络抓取的基本概念和操作方法。通过本课，你将能够定义网络抓取，理解Beautiful Soup对象的作用，应用`find_all`方法，并实际完成一个网站的抓取任务。

## 概述 📋

网络抓取是一种自动从网站提取信息的过程。想象一下，如果你需要分析数百个数据点来找出运动队的最佳球员，手动从不同网站复制粘贴信息到电子表格不仅耗时，而且可能因为任务过于繁重而放弃。网络抓取可以在几分钟内自动完成这项工作。

## 开始之前：工具准备 🛠️

![](img/64afc6d80ff3abd2216cf6da2d84a609_3.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_4.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_5.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_6.png)

要开始网络抓取，我们只需要一些Python代码以及两个模块的帮助：`requests`和`beautifulsoup4`。

![](img/64afc6d80ff3abd2216cf6da2d84a609_7.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_8.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_9.png)

假设你需要从一个网页中找出国家篮球联赛球员的姓名和薪水。

首先，我们导入Beautiful Soup。我们可以将网页HTML作为字符串存储在变量中。为了解析文档，我们将其传递给Beautiful Soup构造函数，从而得到一个Beautiful Soup对象`soup`。这个对象将文档表示为嵌套的数据结构。

Beautiful Soup将HTML表示为一组树状对象，并提供了用于解析HTML的方法。

## 理解Beautiful Soup对象 🌳

我们将使用创建的Beautiful Soup对象`soup`来回顾其结构。

![](img/64afc6d80ff3abd2216cf6da2d84a609_11.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_12.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_13.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_14.png)

**标签对象**对应于原始文档中的HTML标签。例如，`title`标签。

![](img/64afc6d80ff3abd2216cf6da2d84a609_15.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_16.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_17.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_18.png)

考虑`h3`标签。如果存在多个相同名称的标签，默认会选择第一个具有该标签的元素。以“勒布朗·詹姆斯”为例，我们看到姓名被包裹在粗体属性`<b>`中。为了提取它，我们使用树状表示法。

![](img/64afc6d80ff3abd2216cf6da2d84a609_19.png)

变量`tag_object`位于此处。我们可以访问标签的子元素，或者沿着分支向下导航，如下所示：

```python
tag_object.child
```

![](img/64afc6d80ff3abd2216cf6da2d84a609_21.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_22.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_23.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_24.png)

你可以通过使用`parent`属性向上导航树。变量`tag_child`位于此处，我们可以访问其父元素。这就是原始的标签对象。

我们还可以找到标签对象的兄弟节点。只需使用`next_sibling`属性。

我们可以找到`sibling1`的兄弟节点。只需使用`next_sibling`属性。

考虑`tag_child`对象。你可以像字典中的键值对一样访问属性名称和值，如下所示：

![](img/64afc6d80ff3abd2216cf6da2d84a609_26.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_27.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_28.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_29.png)

```python
tag_child.attrs
```

![](img/64afc6d80ff3abd2216cf6da2d84a609_30.png)

你可以将内容作为可导航字符串返回。这类似于支持Beautiful Soup功能的Python字符串。

## 探索`find_all`方法 🔍

现在，让我们回顾`find_all`方法。这是一个过滤器。

![](img/64afc6d80ff3abd2216cf6da2d84a609_32.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_33.png)

你可以使用过滤器基于标签名称、属性、字符串文本或这些条件的组合进行过滤。

![](img/64afc6d80ff3abd2216cf6da2d84a609_34.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_35.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_36.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_37.png)

考虑一个披萨店列表。像之前一样，创建一个Beautiful Soup对象，但这次将其命名为`table`。

![](img/64afc6d80ff3abd2216cf6da2d84a609_38.png)

`find_all`方法会遍历标签的后代，并检索所有匹配过滤器的后代。将其应用于带有`<tr>`标签的表格。结果是一个Python可迭代对象，就像一个列表。

每个元素都是`<tr>`的标签对象。这对应于列表中的每一行，包括表头。

每个元素都是一个标签对象。以第一行为例，我们可以提取第一个表格单元格。我们还可以遍历每个表格单元格。

首先，我们通过变量`row`遍历列表`table_rows`。每个元素对应于表格中的一行。我们可以应用`find_all`方法来查找所有表格单元格。然后，我们可以为每一行遍历变量`cells`。在每次迭代中，变量`cell`对应于该特定行中表格的一个元素，我们继续遍历每个元素，并为每一行重复此过程。

## 应用Beautiful Soup抓取网页 🌐

![](img/64afc6d80ff3abd2216cf6da2d84a609_40.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_41.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_42.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_43.png)

为了抓取网页，我们还需要`requests`库。

![](img/64afc6d80ff3abd2216cf6da2d84a609_44.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_45.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_46.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_47.png)

第一步是导入所需的模块。使用`requests`库的`get`方法下载网页，输入是URL。使用`text`属性获取文本，并将其分配给变量`page`。然后，从变量`page`创建一个Beautiful Soup对象`soup`。这将允许你解析HTML页面，现在你可以开始抓取页面了。

![](img/64afc6d80ff3abd2216cf6da2d84a609_48.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_49.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_50.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_51.png)

查看实验部分以获取更多信息。

## 总结 📝

![](img/64afc6d80ff3abd2216cf6da2d84a609_53.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_54.png)

![](img/64afc6d80ff3abd2216cf6da2d84a609_55.png)

在本节课中，我们一起学习了网络抓取的基本概念和操作方法。我们了解了如何使用`requests`和`beautifulsoup4`库来下载和解析网页，掌握了Beautiful Soup对象的结构和`find_all`方法的应用，并初步实践了从网页中提取所需信息的过程。网络抓取是数据工程中一项强大的技能，能够高效地从网络资源中收集和整理数据。