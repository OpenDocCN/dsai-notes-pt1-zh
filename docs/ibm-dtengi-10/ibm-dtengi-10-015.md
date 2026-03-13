# 015：索引

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_0.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_1.png)

在本节课中，我们将要学习数据库索引的概念、作用及其在MongoDB中的实现方式。索引是提升数据库查询效率的关键工具，理解其工作原理对于设计高效的数据应用至关重要。

## 🎯 为什么需要索引？

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_3.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_4.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_5.png)

索引帮助我们在不进行全局搜索的情况下快速定位数据。想象一下，大英图书馆收藏了2500万册图书。如果你想找到马丁·克莱普曼所著的《设计数据密集型应用》这本书，在没有索引的情况下，你需要逐一检查所有藏书。而有了索引，你可以直接前往计算机类图书区域，再进入数据库分类的书架，最后在标有“M”的书架上找到目标书籍。

我们每天都在使用索引来快速查找信息。例如，过去的电话簿非常流行，其中所有姓名都按姓氏和名字进行了索引。要找到“John Doe”，你可以直接跳转到姓氏为“Doe”的部分，然后在其中查找名字以“J”开头的条目。其他日常例子包括字典和书籍末尾的索引。

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_7.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_8.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_9.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_10.png)

## 🔍 数据库中的索引应用

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_11.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_12.png)

在数据库系统中，应为最频繁执行的查询创建索引。以校园管理数据库中的课程注册集合为例，我们经常需要根据课程ID查找学生信息。为了避免扫描整个集合，我们在课程注册集合的`courseID`字段上创建一个索引。

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_14.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_15.png)

创建索引的命令如下：
```javascript
db.courseEnrollment.createIndex({ courseID: 1 })
```
其中，`courseID: 1`表示按升序存储索引。

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_16.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_17.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_19.png)

我们在`courseID`上创建的索引能帮助更高效地找到相关文档。但是，如果我们需要按学生ID升序来组织学生信息，MongoDB将需要在内存中进行排序，这在某些情况下可能效率不高。

## 📊 复合索引

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_21.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_22.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_23.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_24.png)

为了解决上述问题，我们可以将索引改为复合索引，即对多个字段进行索引。命令如下：
```javascript
db.courseEnrollment.createIndex({ courseID: 1, studentID: 1 })
```

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_26.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_27.png)

由于索引中的条目是按升序组织的，一旦我们找到所有`courseID`为1547的文档，它们将因为此索引而自动按`studentID`排序。

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_28.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_30.png)

## 🌳 MongoDB如何存储索引？

MongoDB中的索引是特殊的数据结构。它们存储被索引的字段以及文档在磁盘上的存储位置。

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_32.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_33.png)

MongoDB以树的形式存储索引，具体来说是一种平衡树。以我们最后的例子为例，在我们的复合索引中，`courseID`作为第一个字段按升序排列。在每个树节点下，学生ID也按升序排列。这使得查找文档更加高效，无论是进行等值查询还是范围查询。如果你在一个已经建立索引的字段上进行排序，MongoDB无需再次对其进行排序。

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_34.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_35.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_36.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_37.png)

## 📝 总结

本节课中我们一起学习了：
*   索引的作用是帮助快速定位数据，避免全局扫描。
*   应为最频繁的查询创建索引。
*   复合索引可以对多个字段进行索引。
*   MongoDB在索引条目中存储被索引的数据以及文档在磁盘上的位置。
*   MongoDB将索引存储为树形结构，以提高查找文档的效率。

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_39.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_40.png)

![](img/3a62f0ac3b5f8501e7f35c7903f2eabc_41.png)

理解并合理使用索引，是优化NoSQL数据库性能的核心技能之一。