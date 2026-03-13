# 002：NoSQL数据库的特点 🗄️

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_0.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_1.png)

在本节课中，我们将学习NoSQL数据库的核心概念、主要类型以及它们的关键特性。我们将探讨NoSQL数据库与关系型数据库的主要区别，并了解采用NoSQL数据库带来的主要优势。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_3.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_4.png)

## NoSQL数据库的概述

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_6.png)

NoSQL数据库最普遍的共同特征是它们在架构上是**非关系型**的。这意味着它们不依赖于传统的表结构，也不使用SQL作为主要查询语言。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_7.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_8.png)

## NoSQL数据库的主要类型

业界普遍认为，NoSQL数据库主要分为四种类型。以下是这四种类型的简要介绍：

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_10.png)

*   **键值数据库**：以键值对的形式存储数据。
*   **文档数据库**：将数据存储为类似JSON的文档格式。
*   **列式数据库**：按列族存储数据，优化读取和分析。
*   **图数据库**：专注于存储实体（节点）和它们之间的关系（边）。

这些类型之间存在一些重叠，定义并非总是绝对清晰。在本课程后续部分，我们将详细探讨不同类型及其适用场景。

## NoSQL数据库的共同点

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_12.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_13.png)

尽管技术实现各异，但NoSQL数据库有一些重要的共同特征。

上一节我们介绍了NoSQL数据库的主要类型，本节中我们来看看将它们联系在一起的共同点。一个关键共性是，大多数NoSQL数据库都植根于**开源社区**，并以开源方式被使用和发展。这对它们在行业内的快速增长起到了根本性的推动作用。许多公司会同时提供商业版本（包含服务和技术支持）并赞助其对应的开源项目，例如IBM Cloudant之于CouchDB，DataStax之于Apache Cassandra，MongoDB公司也维护着开源的MongoDB版本。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_15.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_16.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_17.png)

从技术角度看，它们虽然差异很大，但仍有一些共同点浮现。以下是NoSQL数据库的一些常见技术特征：

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_18.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_19.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_20.png)

*   **水平扩展性**：大多数NoSQL数据库为**水平扩展**而设计，能够比关系型数据库更轻松地在服务器集群间分布数据。这通常需要在**整个数据库中使用全局唯一键**来简化分区或分片。
    *   **公式/概念**：`数据分布 → 分区(Partitioning) / 分片(Sharding)`
*   **专用性**：相比曾是数据存储“瑞士军刀”的关系型数据库管理系统，NoSQL数据库通常更**专注于特定用例**。
*   **易用性**：开发者因NoSQL数据库**易于进行数据建模和使用**而被吸引。
*   **灵活的模式**：与关系型数据库的固定模式相比，许多NoSQL数据库通过其**灵活的模式**支持更敏捷的开发。

## 采用NoSQL数据库的优势

我们已经了解了NoSQL的含义、其在数据库技术世界中的历史，以及与关系型数据库相比的一些基本特点和差异。那么，为什么要使用NoSQL数据库？为什么这些数据库的普及速度如此之快？

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_22.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_23.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_24.png)

原因有很多，以下列出了一些更常见的优势。请注意，并非所有NoSQL数据库都具备所有这些优点。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_25.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_26.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_27.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_28.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_29.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_30.png)

### 可扩展性与性能

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_31.png)

首先，采用NoSQL数据库最常见的原因是为了**可扩展性**，特别是跨服务器集群、机架甚至数据中心进行**水平扩展**的能力。根据应用需求弹性地**向上和向下扩展**是关键。NoSQL数据库非常适合满足大数据应用所呈现的海量数据规模和巨大并发用户数。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_33.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_34.png)

第二点是**性能**，它与可扩展性密不可分。现代应用必须能够**提供快速的响应时间**，即使面对大数据量和高并发。NoSQL数据库能够利用大型服务器集群的资源，使其在这些情况下成为实现高性能的理想选择。

### 高可用性与成本效益

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_36.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_37.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_38.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_39.png)

**高可用性**是数据库的明确要求。在集群上运行数据库并存储数据的多个副本，比单服务器解决方案更具弹性。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_40.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_41.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_42.png)

历史上，大型数据库运行在昂贵的机器或大型机上。现代企业采用云架构来支持其应用，而NoSQL数据库的分布式数据特性意味着它们可以部署和运行在云架构的服务器集群上，从而**大幅降低成本**。成本对任何技术项目都很重要，经常可以听到NoSQL采用者相比其现有数据库显著降低成本，同时仍能获得相同或更好的性能和功能。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_44.png)

### 灵活的开发模式

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_45.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_46.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_47.png)

**灵活的模式和直观的数据结构**是开发者在希望高效构建应用时喜爱的关键特性。大多数NoSQL数据库允许**灵活的模式**，这意味着可以快速为应用构建新功能，而无需任何数据库锁定或停机。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_49.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_50.png)

NoSQL数据库还具有**多样化的数据结构**，这些结构通常比关系型数据存储的行和列更能贴切地满足开发需求。例如，包括用于快速查找的键值存储、用于存储非规范化直观信息的文档存储，以及用于关联数据集的图数据库。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_51.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_52.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_53.png)

### 特殊功能

某些NoSQL提供商还提供吸引最终用户的各种**专业化功能**。例如包括特定的索引和查询功能（如地理空间搜索）、数据复制的健壮性以及现代的HTTP API。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_55.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_56.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_57.png)

拥有所有这些优势，你可能会问为什么还有人会使用NoSQL以外的数据库。可以说，在当今大多数情况下确实如此，但肯定仍有一些需求最好由RDBMS来满足。我们将在课程后面讨论这一点。

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_58.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_59.png)

## 总结

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_61.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_62.png)

![](img/ed1db8cda1d9115e3d78e7fab4dd3259_63.png)

本节课中我们一起学习了以下核心要点：
1.  NoSQL数据库是**非关系型**的。
2.  NoSQL数据库主要分为**四种类别**。
3.  NoSQL数据库**植根于开源社区**。
4.  不同的NoSQL数据库在**技术实现上各有不同**。
5.  采用NoSQL数据库能带来**多方面的显著好处**。