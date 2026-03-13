# 087：网络抓取的可选HTML基础 📄

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_0.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_1.png)

在本节课中，我们将学习超文本标记语言（HTML）的基础知识，这是进行网络抓取的关键。理解HTML的结构能帮助我们使用Python等工具从网页中提取有用信息，例如房地产价格或编程问题的解决方案。

## 概述

本节课将回顾HTML的基本构成。我们将分析一个基础网页的HTML结构，了解HTML标签的组成、HTML文档树的概念，并学习如何识别HTML表格。掌握这些知识是后续使用自动化工具提取网页数据的前提。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_3.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_4.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_5.png)

## 网页的HTML构成

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_6.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_7.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_8.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_9.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_10.png)

假设我们需要从一个国家篮球联盟的网页中找出球员的姓名和薪水。该网页由HTML代码构成。HTML由一系列被尖括号包围的蓝色文本元素（称为标签）以及标签内的文本组成。标签告诉浏览器如何显示内容，而我们所需的数据就包含在这些文本中。



![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_11.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_0.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_1.png)

代码的第一部分包含文档类型声明 `<!DOCTYPE html>`，它声明此文档是一个HTML文档。`<html>` 元素是HTML页面的根元素。`<head>` 元素包含关于HTML页面的元信息。接下来是 `<body>`，这是在网页上显示的内容，通常也是我们感兴趣的数据所在。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_13.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_14.png)

我们看到带有 `<h3>` 标签的元素，这表示三级标题，会使文本变大加粗。这些标签内包含了球员的姓名。请注意，数据被包裹在元素中，它以 `<h3>` 开始，以 `</h3>` 结束。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_15.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_16.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_17.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_18.png)

还有一个不同的标签 `<p>`，表示段落。`<p>` 标签包含球员的薪水。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_19.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_20.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_21.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_22.png)

## HTML标签的组成

上一节我们看到了HTML标签如何包裹数据，本节我们来仔细看看一个HTML标签的具体构成。以下是一个HTML锚点标签的例子：

```html
<a href="https://www.ibm.com">IBM</a>
```
它将在页面上显示“IBM”，当你点击它时，会跳转到 `ibm.com`。

*   **标签名称**：在这个例子中是 `a`。这个标签定义了一个超链接，用于从一个页面链接到另一个页面。将每个标签名称想象成Python中的一个类，而每个单独的标签则是该类的一个实例，会很有帮助。
*   **开始标签与结束标签**：我们有开始标签 `<a>` 和结束标签 `</a>`。结束标签在标签名称前有一个斜杠 `/`。这些标签包含了要显示在网页上的内容，本例中是“IBM”。
*   **属性**：属性由属性名和属性值组成。本例中是 `href="https://www.ibm.com"`，它是目标网页的URL。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_24.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_25.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_26.png)

真实的网页更为复杂。根据你使用的浏览器，你可以选择HTML元素并点击“检查”，结果将使你能够查看该元素的HTML代码。网页中还有其他类型的内容，如CSS和JavaScript，本课程将不涉及。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_27.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_28.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_29.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_30.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_31.png)

## HTML文档树

实际的HTML文档可以被称为**文档树**。让我们看一个简单的例子。标签可以包含字符串和其他标签，这些元素就是该标签的**子元素**。我们可以将其表示为一棵家族树，每个嵌套的标签都是树中的一个层级。

考虑以下简单HTML结构：
```html
<html>
  <head>
    <title>页面标题</title>
  </head>
  <body>
    <h1>这是一个<b>标题</b></h1>
    <p>这是一个段落。</p>
  </body>
</html>
```

其树状结构可理解如下：
*   `<html>` 标签包含了 `<head>` 和 `<body>` 标签。
*   `<head>` 和 `<body>` 标签是 `<html>` 标签的**后代**。具体来说，它们是 `<html>` 标签的**子元素**。`<html>` 标签是它们的**父元素**。
*   `<head>` 和 `<body>` 标签是**兄弟元素**，因为它们处于同一层级。
*   `<title>` 标签是 `<head>` 标签的**子元素**，其**父元素**是 `<head>` 标签。`<title>` 标签是 `<html>` 标签的**后代**，但不是其**子元素**。
*   标题 `<h1>` 和段落 `<p>` 标签是 `<body>` 标签的**子元素**，并且因为它们都是 `<body>` 标签的子元素，所以彼此是**兄弟元素**。
*   加粗 `<b>` 标签是标题 `<h1>` 标签的**子元素**。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_33.png)

标签的内容也是树的一部分，但画出来会显得繁琐。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_34.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_35.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_36.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_37.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_38.png)

## HTML表格

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_39.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_40.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_41.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_42.png)

理解了文档的树状结构后，我们来看看一种常见的数据呈现形式：表格。接下来，让我们学习HTML表格。要定义一个HTML表格，我们使用 `<table>` 标签。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_43.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_45.png)

以下是定义一个简单表格的HTML结构示例：
```html
<table>
  <tr>
    <th>姓名</th>
    <th>薪水</th>
  </tr>
  <tr>
    <td>张三</td>
    <td>$100,000</td>
  </tr>
  <tr>
    <td>李四</td>
    <td>$120,000</td>
  </tr>
</table>
```

以下是表格的关键组成部分：
*   **`<table>` 标签**：定义整个表格。
*   **`<tr>` 标签**：定义表格中的一行。
*   **`<th>` 标签**：通常用于第一行，定义表头单元格。
*   **`<td>` 标签**：定义表格中的标准数据单元格。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_47.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_48.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_49.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_50.png)

对于第一行，第一个单元格我们使用 `<th>` 标签定义“姓名”，第二个单元格定义“薪水”。对于第二行，第一个单元格 `<td>` 是“张三”，第二个单元格是“$100,000”，依此类推。

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_52.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_54.png)

## 总结

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_56.png)

![](img/55f28ab46ce5bf1b6f21395f7ba37fcd_57.png)

本节课我们一起学习了HTML的基础知识，这是进行网络抓取的重要第一步。我们回顾了网页的HTML构成，分析了标签的组成部分（标签名、开始/结束标签、属性），理解了HTML文档的树状结构（父元素、子元素、兄弟元素），并认识了用于组织数据的HTML表格（`<table>`, `<tr>`, `<th>`, `<td>`）。现在，我们已经具备了从网页中提取数据所需的基本HTML知识。