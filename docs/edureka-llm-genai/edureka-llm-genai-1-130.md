1.3：词频计算演示 📊

![](img/f17d503baff012beeff2de367b652c3e_0.png)

在本节课中，我们将学习如何计算词频。词频是自然语言处理中的一项基础技术，用于衡量一个词语在特定文档中出现的频繁程度。我们将通过一个具体的例子，一步步演示如何从一组文档中构建词汇表，并计算每个文档中每个词的词频。

上一节我们介绍了词频的基本概念，本节中我们来看看如何通过一个具体的例子进行手动计算和代码实现。

![](img/f17d503baff012beeff2de367b652c3e_2.png)

我们有一个包含四个文档的集合：
1.  `This is the first document`
2.  `This document is the second document`
3.  `And this is the third one`
4.  `Is this the first document`

![](img/f17d503baff012beeff2de367b652c3e_3.png)

![](img/f17d503baff012beeff2de367b652c3e_4.png)

![](img/f17d503baff012beeff2de367b652c3e_5.png)

![](img/f17d503baff012beeff2de367b652c3e_6.png)

![](img/f17d503baff012beeff2de367b652c3e_7.png)

首先，我们需要从所有文档中提取出唯一的词汇，构建一个词汇表。

![](img/f17d503baff012beeff2de367b652c3e_8.png)

![](img/f17d503baff012beeff2de367b652c3e_9.png)

![](img/f17d503baff012beeff2de367b652c3e_10.png)

![](img/f17d503baff012beeff2de367b652c3e_11.png)

![](img/f17d503baff012beeff2de367b652c3e_12.png)

以下是构建出的词汇表：
*   and
*   document
*   first
*   is
*   one
*   second
*   the
*   third
*   this

![](img/f17d503baff012beeff2de367b652c3e_13.png)

现在，我们将为每个文档计算词频。词频的计算方法是：针对词汇表中的每个词，统计它在当前文档中出现的次数。

让我们从第一个文档 `This is the first document` 开始计算。

以下是第一个文档的词频向量，顺序对应词汇表 `[and, document, first, is, one, second, the, third, this]`：
*   `and`: 0
*   `document`: 1
*   `first`: 1
*   `is`: 1
*   `one`: 0
*   `second`: 0
*   `the`: 1
*   `third`: 0
*   `this`: 1

![](img/f17d503baff012beeff2de367b652c3e_15.png)

![](img/f17d503baff012beeff2de367b652c3e_16.png)

![](img/f17d503baff012beeff2de367b652c3e_17.png)

因此，第一个文档的词频向量是 `[0, 1, 1, 1, 0, 0, 1, 0, 1]`。

![](img/f17d503baff012beeff2de367b652c3e_18.png)

![](img/f17d503baff012beeff2de367b652c3e_19.png)

![](img/f17d503baff012beeff2de367b652c3e_20.png)

![](img/f17d503baff012beeff2de367b652c3e_21.png)

![](img/f17d503baff012beeff2de367b652c3e_22.png)

接下来，我们计算第二个文档 `This document is the second document` 的词频。

![](img/f17d503baff012beeff2de367b652c3e_23.png)

![](img/f17d503baff012beeff2de367b652c3e_24.png)

![](img/f17d503baff012beeff2de367b652c3e_25.png)

![](img/f17d503baff012beeff2de367b652c3e_26.png)

以下是第二个文档的词频向量：
*   `and`: 0
*   `document`: 2
*   `first`: 0
*   `is`: 1
*   `one`: 0
*   `second`: 1
*   `the`: 1
*   `third`: 0
*   `this`: 1

因此，第二个文档的词频向量是 `[0, 2, 0, 1, 0, 1, 1, 0, 1]`。

按照同样的方法，我们可以计算出第三个和第四个文档的词频向量。

![](img/f17d503baff012beeff2de367b652c3e_28.png)

通过以上步骤，我们手动演示了词频的计算过程。其核心是遍历词汇表中的每个词，并统计它在指定文档中出现的次数。用公式可以表示为：

![](img/f17d503baff012beeff2de367b652c3e_29.png)

![](img/f17d503baff012beeff2de367b652c3e_30.png)

![](img/f17d503baff012beeff2de367b652c3e_31.png)

![](img/f17d503baff012beeff2de367b652c3e_32.png)

![](img/f17d503baff012beeff2de367b652c3e_33.png)

**TF(词, 文档) = (词在文档中出现的次数)**

![](img/f17d503baff012beeff2de367b652c3e_34.png)

![](img/f17d503baff012beeff2de367b652c3e_35.png)

![](img/f17d503baff012beeff2de367b652c3e_36.png)

![](img/f17d503baff012beeff2de367b652c3e_37.png)

![](img/f17d503baff012beeff2de367b652c3e_38.png)

在接下来的课程中，我们将深入探讨如何用代码自动化实现这一过程。

![](img/f17d503baff012beeff2de367b652c3e_39.png)

![](img/f17d503baff012beeff2de367b652c3e_41.png)

本节课中我们一起学习了词频的手动计算过程。我们首先从文档集构建了词汇表，然后针对每个文档，统计了词汇表中每个词的出现次数，从而得到了可以表示文档特征的词频向量。这是将文本转换为数值形式的第一步，为后续的文本分析任务奠定了基础。