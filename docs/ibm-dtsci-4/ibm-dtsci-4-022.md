# 022：网页抓取HTML基础（选修）🔍

![](img/094def98038d41563c92daa7ae11b99b_0.png)

![](img/094def98038d41563c92daa7ae11b99b_1.png)

在本节课中，我们将回顾超文本标记语言（HTML），这是进行网页抓取的基础。网页上存在大量有用数据，例如房地产价格和编程问题的解决方案。万维网和维基百科是世界信息的宝库。如果你理解了HTML，就可以使用Python来提取这些信息。本视频将带你回顾一个基础网页的HTML结构、HTML标签的构成、HTML树状结构，并理解HTML表格。

## 🌐 网页与HTML概述

![](img/094def98038d41563c92daa7ae11b99b_3.png)

![](img/094def98038d41563c92daa7ae11b99b_4.png)

![](img/094def98038d41563c92daa7ae11b99b_5.png)

![](img/094def98038d41563c92daa7ae11b99b_6.png)

![](img/094def98038d41563c92daa7ae11b99b_7.png)

假设你有一个需求：从一个网页中找出国家篮球联盟球员的姓名和薪水。该网页由HTML构成，它包含一系列被尖括号包围的蓝色元素，这些元素称为标签。标签告诉浏览器如何显示内容，而我们需要的数据就在这些文本中。

![](img/094def98038d41563c92daa7ae11b99b_8.png)

![](img/094def98038d41563c92daa7ae11b99b_9.png)

![](img/094def98038d41563c92daa7ae11b99b_10.png)

![](img/094def98038d41563c92daa7ae11b99b_11.png)

## 🏷️ HTML标签的构成

让我们仔细看看一个HTML标签的构成。以下是一个HTML锚标签的例子：

```html
<a href="https://www.ibm.com">IBM</a>
```

![](img/094def98038d41563c92daa7ae11b99b_13.png)

![](img/094def98038d41563c92daa7ae11b99b_14.png)

![](img/094def98038d41563c92daa7ae11b99b_15.png)

![](img/094def98038d41563c92daa7ae11b99b_16.png)

![](img/094def98038d41563c92daa7ae11b99b_17.png)

它将显示“IBM”，当你点击它时，会跳转到IBM.com的网站。

![](img/094def98038d41563c92daa7ae11b99b_18.png)

![](img/094def98038d41563c92daa7ae11b99b_19.png)

![](img/094def98038d41563c92daa7ae11b99b_20.png)

![](img/094def98038d41563c92daa7ae11b99b_21.png)

![](img/094def98038d41563c92daa7ae11b99b_22.png)

*   **标签名称**：在这个例子中是 `a`。这个标签定义了一个超链接，用于从一个页面链接到另一个页面。将每个标签名称想象成Python中的一个类，而每个单独的标签则是该类的一个实例，会很有帮助。
*   **开始标签与结束标签**：我们有开始标签 `<a>` 和结束标签 `</a>`。结束标签在标签名称前有一个斜杠 `/`。这些标签包含了内容，即网页上显示的部分。
*   **属性**：属性由属性名和属性值组成。在这个例子中，`href` 是属性名，其值是目标网页的URL `https://www.ibm.com`。

真实的网页更为复杂。根据你使用的浏览器，你可以选择HTML元素，然后点击“检查”。结果将允许你查看该页面的HTML。网页中还有其他类型的内容，如CSS和JavaScript，本课程将不涉及。

## 🌳 HTML树状结构

![](img/094def98038d41563c92daa7ae11b99b_24.png)

![](img/094def98038d41563c92daa7ae11b99b_25.png)

![](img/094def98038d41563c92daa7ae11b99b_26.png)

![](img/094def98038d41563c92daa7ae11b99b_27.png)

每个HTML文档实际上都可以被称为一个文档树。让我们看一个简单的例子。标签可以包含字符串和其他标签，这些元素就是该标签的子元素。我们可以将其表示为一个家族树，每个嵌套的标签都是树中的一个层级。

![](img/094def98038d41563c92daa7ae11b99b_28.png)

![](img/094def98038d41563c92daa7ae11b99b_29.png)

![](img/094def98038d41563c92daa7ae11b99b_30.png)

![](img/094def98038d41563c92daa7ae11b99b_31.png)

*   `<html>` 标签包含了 `<head>` 和 `<body>` 标签。
*   `<head>` 和 `<body>` 标签是 `<html>` 标签的后代。具体来说，它们是 `<html>` 标签的子元素。`<html>` 标签是它们的父元素。
*   `<head>` 和 `<body>` 标签是兄弟元素，因为它们处于同一层级。
*   `<title>` 标签是 `<head>` 标签的子元素，其父元素是 `<head>` 标签。`<title>` 标签是 `<html>` 标签的后代，但不是其子元素。
*   标题标签（如 `<h1>`）和段落标签（`<p>`）是 `<body>` 标签的子元素，并且由于它们都是 `<body>` 标签的子元素，因此彼此是兄弟元素。
*   加粗标签（`<b>`）是标题标签的子元素。

标签的内容也是树的一部分，但这会使图示变得复杂。

## 📊 HTML表格

![](img/094def98038d41563c92daa7ae11b99b_33.png)

![](img/094def98038d41563c92daa7ae11b99b_34.png)

![](img/094def98038d41563c92daa7ae11b99b_35.png)

接下来，让我们回顾HTML表格。要定义一个HTML表格，我们使用 `<table>` 标签。

![](img/094def98038d41563c92daa7ae11b99b_36.png)

![](img/094def98038d41563c92daa7ae11b99b_37.png)

![](img/094def98038d41563c92daa7ae11b99b_38.png)

![](img/094def98038d41563c92daa7ae11b99b_39.png)

![](img/094def98038d41563c92daa7ae11b99b_40.png)

![](img/094def98038d41563c92daa7ae11b99b_41.png)

以下是定义一个简单表格的代码结构：

![](img/094def98038d41563c92daa7ae11b99b_42.png)

![](img/094def98038d41563c92daa7ae11b99b_43.png)

![](img/094def98038d41563c92daa7ae11b99b_45.png)

```html
<table>
  <tr>
    <th>表头1</th>
    <th>表头2</th>
  </tr>
  <tr>
    <td>行1， 单元格1</td>
    <td>行1， 单元格2</td>
  </tr>
  <tr>
    <td>行2， 单元格1</td>
    <td>行2， 单元格2</td>
  </tr>
</table>
```

![](img/094def98038d41563c92daa7ae11b99b_47.png)

![](img/094def98038d41563c92daa7ae11b99b_48.png)

*   每个表格行由 `<tr>` 标签定义。
*   第一行可以使用 `<th>` 标签作为表头。
*   表格行中的单元格包含一组 `<td>` 标签，每个 `<td>` 定义一个表格单元格。

![](img/094def98038d41563c92daa7ae11b99b_49.png)

![](img/094def98038d41563c92daa7ae11b99b_50.png)

![](img/094def98038d41563c92daa7ae11b99b_51.png)

![](img/094def98038d41563c92daa7ae11b99b_53.png)

## 🎯 总结

![](img/094def98038d41563c92daa7ae11b99b_54.png)

![](img/094def98038d41563c92daa7ae11b99b_56.png)

![](img/094def98038d41563c92daa7ae11b99b_57.png)

本节课我们一起学习了HTML的基础知识，这是进行网页抓取的关键第一步。我们了解了网页如何由HTML构成，深入探讨了HTML标签的组成部分（包括标签名、属性和内容），认识了HTML文档的树状结构（父元素、子元素、兄弟元素），并学习了如何用HTML创建表格。掌握这些概念后，你将能够更好地理解网页的结构，为后续使用Python工具（如BeautifulSoup）从网页中提取所需数据打下坚实的基础。