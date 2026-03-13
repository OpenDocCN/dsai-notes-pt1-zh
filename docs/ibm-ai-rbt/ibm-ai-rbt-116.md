# 116：理解Watson Discovery 🧠

![](img/11ce409dfa8ea556d57a303efb43ba9f_1.png)

在本节课中，我们将要学习IBM Cloud上最酷的服务之一——Watson Discovery。我们将了解它如何作为一个高级的洞察引擎，利用人工智能能力从数据中提取有价值的信息。

![](img/11ce409dfa8ea556d57a303efb43ba9f_3.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_4.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_5.png)

## 概述：什么是Watson Discovery？ 🔍

![](img/11ce409dfa8ea556d57a303efb43ba9f_7.png)

Watson Discovery可以被视为一个高级的洞察引擎。它的核心目标是利用沃森的人工智能能力，从您的数据中提取洞察力。大多数公司都拥有丰富的数据。真正的挑战在于提取洞察力并发现可操作的信息。

![](img/11ce409dfa8ea556d57a303efb43ba9f_8.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_9.png)

这正是数据科学家角色近年来变得如此突出的原因。数据无处不在，而利用数据改善业务的能力才是真正的价值所在。

## Watson Discovery的实际应用示例 📰

![](img/11ce409dfa8ea556d57a303efb43ba9f_11.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_12.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_13.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_14.png)

为了让概念更具体，我们来看一个实际例子。Watson Discovery允许您加载自己的数据，但它也附带了一个便捷的Watson Discovery新闻数据集，其中包含过去60天的新闻文章。这个庞大的集合会自动为您维护和更新，让您可以轻松分析新闻。

![](img/11ce409dfa8ea556d57a303efb43ba9f_16.png)

现在想象一下，如果您是一名记者、作家或任何需要分析新闻的人，能够访问超过1000万篇新闻文章是非常惊人的。但这也相当令人不知所措。您如何查询如此庞大的数据集以从中提取洞察力呢？

## Watson Discovery的核心功能：数据增强与查询 ✨

![](img/11ce409dfa8ea556d57a303efb43ba9f_18.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_19.png)

这正是Watson Discovery的魔力所在。Discovery能够应用人工智能算法来增强您导入的原始数据。对于为您导入的新闻集合，这种增强包括从文档中提取关键词、标记概念、提取特定实体（如国家、公司甚至人物），以及进行情感分析，判断一篇关于特定公司的新闻文章是积极的、中性的还是消极的。

Discovery允许我们查询这些增强后的数据。我们可以通过自然语言、可视化构建器或使用Discovery特有的语法（称为Discovery查询语言）来构建查询。在Watson Discovery新闻中，我们甚至有一些示例查询可以立即获得结果，并向我们展示该语法的具体写法。

这些示例查询的另一个优点是，它们让您了解可以使用Watson Discovery提取何种洞察力。例如，使用这个新闻集合，我们可以找出新闻中受到最积极报道的前10家公司、最近被收购的AI公司，甚至是科技行业中被提及最多的人物。想象一下，如果没有像Discovery这样的服务，手动完成这些工作会是多么困难。

![](img/11ce409dfa8ea556d57a303efb43ba9f_21.png)

这些查询之所以可能，是因为Watson Discovery增强了文档。以一篇关于特斯拉的文章为例，沃森会阅读文章，判断其情感是积极、消极还是中性，检测相关人物（例如埃隆·马斯克），以及其中提到的公司（例如特斯拉和梅赛德斯-奔驰）等等。

![](img/11ce409dfa8ea556d57a303efb43ba9f_22.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_23.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_24.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_25.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_26.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_27.png)

## 深入Discovery查询语言 💻

在本模块的实验中，您会更熟悉Discovery查询语言，但我想让您了解一下它的样子。

![](img/11ce409dfa8ea556d57a303efb43ba9f_29.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_30.png)

让我们放大这个关于媒体中讨论最积极的公司的特定查询。

![](img/11ce409dfa8ea556d57a303efb43ba9f_31.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_33.png)

我们首先从`enriched_text`开始，这是一篇新闻文章的文本加上沃森为我们添加的所有增强信息。然后，我们选择文档中的`entities`。实体是由沃森检测到的特殊值。

![](img/11ce409dfa8ea556d57a303efb43ba9f_35.png)

可以将其理解为公司、人物、城市等。接着，我们筛选特定类型的实体，在我们的例子中是公司，但我们只想要积极情绪的子集。因此，我们使用情感分析增强功能来要求较高的积极情绪得分。

![](img/11ce409dfa8ea556d57a303efb43ba9f_37.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_38.png)

最后，我们指定希望通过实体名称（例如Netflix或Facebook）来聚合结果。

![](img/11ce409dfa8ea556d57a303efb43ba9f_40.png)

如果我们想找出受到负面媒体报道的人物，我们只需将实体类型过滤器从公司更改为人物，并调整情绪得分要求即可。有时您会发现同一个名字同时出现在最积极和最消极的列表中。这对于非常著名的政治家来说并不罕见，因为他们往往在新闻界收到两极分化的报道，所以他们可能会收到很多积极的提及，但同时也会受到很多批评。

不必过于担心立即掌握查询语言的细节。有更简单的方法可以使用自然语言或可视化模式来查询Discovery。

尽管如此，重要的是您要知道，如果需要，高级选项是存在的。您使用Discovery越多，操作越多，您就会越熟悉其强大的查询语言。

![](img/11ce409dfa8ea556d57a303efb43ba9f_42.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_43.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_44.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_45.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_46.png)

## 连接您自己的数据源 📂

![](img/11ce409dfa8ea556d57a303efb43ba9f_47.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_49.png)

能够访问新闻数据固然很好，但大多数企业可能更感兴趣的是能够查询他们自己的数据和文档。Discovery允许您轻松连接各种数据源以创建自己的集合。

![](img/11ce409dfa8ea556d57a303efb43ba9f_51.png)

例如，它与Salesforce、SharePoint、Box和IBM Cloud对象存储有集成。

![](img/11ce409dfa8ea556d57a303efb43ba9f_53.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_54.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_56.png)

您还可以让Discovery爬取网站信息，以及直接上传文档，包括PDF文件。

![](img/11ce409dfa8ea556d57a303efb43ba9f_58.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_59.png)

您的业务可能具有特定领域的实体，因此Discovery甚至为您提供了创建自定义机器学习模型的能力，用于识别和分类与您特定业务相关的实体。

![](img/11ce409dfa8ea556d57a303efb43ba9f_61.png)

这是一个更高级的主题，需要与另一个沃森工具——即Watson Knowledge Studio——集成，它允许您创建和训练模型。就本课程而言，我们不会利用此类功能，但重要的是您知道如果需要，它是可用的。事实上，您可以自由探索Discovery及其在本课程涵盖内容之外的功能，通过查阅官方文档。

## 总结 📝

![](img/11ce409dfa8ea556d57a303efb43ba9f_63.png)

![](img/11ce409dfa8ea556d57a303efb43ba9f_64.png)

本节课中，我们一起学习了Watson Discovery的核心概念。我们了解到它是一个强大的洞察引擎，能够通过人工智能增强数据（如提取实体、分析情感），并提供多种查询方式（自然语言、可视化、专用查询语言）来从海量数据（如新闻集合或企业自有文档）中提取有价值的业务洞察。这为解决企业“数据丰富，洞察匮乏”的挑战提供了强大工具。