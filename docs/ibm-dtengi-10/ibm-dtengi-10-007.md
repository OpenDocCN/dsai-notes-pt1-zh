# 007：ACID与BASE对比 🆚

![](img/8fe7ce88c823c13234b33c1db9b658eb_0.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_1.png)

在本节课中，我们将要学习关系型数据库与NoSQL数据库在数据一致性模型上的核心区别，即ACID与BASE模型。我们将定义这两个术语，识别它们各自的适用场景，并描述它们之间的关键差异。

关系型数据库管理系统（RDBMS）与NoSQL数据库之间最显著的区别之一，在于它们的数据一致性模型。当今最广为人知且广泛使用的两种一致性模型是ACID和BASE。关系型数据库采用ACID模型，而NoSQL数据库则采用BASE模型。

![](img/8fe7ce88c823c13234b33c1db9b658eb_3.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_4.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_5.png)

尽管ACID和BASE模型常被视为对立面，但事实上，两者都各有其优缺点。接下来，让我们深入了解这两个术语的含义。

![](img/8fe7ce88c823c13234b33c1db9b658eb_6.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_7.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_8.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_9.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_10.png)

## ACID模型详解 💎

![](img/8fe7ce88c823c13234b33c1db9b658eb_11.png)

ACID是一个缩写，代表原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持久性（Durability）。以下是其核心概念的详细解释：

![](img/8fe7ce88c823c13234b33c1db9b658eb_13.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_14.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_15.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_16.png)

*   **原子性 (Atomicity)**：一个事务中的所有操作要么全部成功，要么全部失败回滚。这确保了事务的完整性。
    *   **公式/代码描述**：`事务 = {操作1, 操作2, ...}`。若任一操作失败，则整个事务状态回滚到开始之前。
*   **一致性 (Consistency)**：事务完成后，数据库的结构完整性不会被破坏，数据必须符合所有预定义的规则。
*   **隔离性 (Isolation)**：并发执行的事务彼此隔离，一个事务在完成前，其中间状态不会影响其他事务。
*   **持久性 (Durability)**：一旦事务提交，其所做的数据更改将是永久性的，即使发生系统故障（如断电）也不会丢失。

![](img/8fe7ce88c823c13234b33c1db9b658eb_18.png)

许多开发者通过使用关系型数据库而熟悉ACID事务，因此ACID一致性模型曾长期是数据一致性的标准。

![](img/8fe7ce88c823c13234b33c1db9b658eb_19.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_20.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_21.png)

### ACID模型的适用场景

![](img/8fe7ce88c823c13234b33c1db9b658eb_23.png)

ACID一致性模型确保已执行的事务始终保持一致，这使其非常适合处理在线事务处理（OLTP）业务，例如金融机构或数据仓库类型的应用程序。

![](img/8fe7ce88c823c13234b33c1db9b658eb_25.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_26.png)

这些组织需要能够处理大量小型、并发事务的数据库系统，就像关系型数据库那样。ACID系统提供了一个可靠的一致性模型，您可以信赖其数据的结构完整性。

在众多ACID数据库的用例中，有一个尤为突出：金融机构几乎完全使用ACID数据库进行资金转账。因为这些操作依赖于ACID事务的原子性。一个被中断但未立即从数据库中清除的事务可能导致严重问题，例如资金从一个账户扣除后，由于错误而从未或未能存入另一个账户。

![](img/8fe7ce88c823c13234b33c1db9b658eb_28.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_29.png)

## BASE模型详解 🏗️

![](img/8fe7ce88c823c13234b33c1db9b658eb_31.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_32.png)

上一节我们介绍了强调强一致性的ACID模型，本节中我们来看看在可用性上做出权衡的BASE模型。BASE同样是缩写，代表基本可用（Basically Available）、软状态（Soft State）和最终一致性（Eventually Consistent）。

![](img/8fe7ce88c823c13234b33c1db9b658eb_33.png)

以下是其核心概念的详细解释：

![](img/8fe7ce88c823c13234b33c1db9b658eb_35.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_36.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_37.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_38.png)

*   **基本可用 (Basically Available)**：BASE模型的NoSQL数据库不强制要求即时一致性，而是通过跨数据库集群节点分布和复制数据来确保数据的可用性。
*   **软状态 (Soft State)**：由于缺乏即时一致性，数据值可能会随时间而变化。在BASE模型中，数据存储不必始终保持强一致，不同的数据副本也不必时刻保持相互一致。
*   **最终一致性 (Eventually Consistent)**：BASE模型不强制即时一致性，但这并不意味着它永远无法达成一致性。系统保证，在没有新的更新操作后，经过一段时间，所有副本最终会达到一致状态。然而，在达到最终一致之前，读取的数据可能是不一致的。

![](img/8fe7ce88c823c13234b33c1db9b658eb_40.png)

在NoSQL数据库领域，ACID事务不那么流行，因为一些数据库放宽了对即时一致性、数据新鲜度和准确性的要求，以换取其他优势，如高可用性、可扩展性和弹性。

![](img/8fe7ce88c823c13234b33c1db9b658eb_42.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_43.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_44.png)

### BASE模型的适用场景

![](img/8fe7ce88c823c13234b33c1db9b658eb_46.png)

BASE一致性模型被处理情感分析和社交网络研究的市场营销与客户服务公司、包含海量数据并需要始终保持可用的社交媒体应用，以及像Netflix、Spotify和Uber这样需要全球可用的在线服务所采用。

![](img/8fe7ce88c823c13234b33c1db9b658eb_47.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_48.png)

BASE数据存储将可用性置于一致性之上，但它不保证在写入时复制数据的一致性。NoSQL数据库使用BASE一致性模型。本质上，BASE一致性模型提供了高可用性。

BASE数据存储可以并且确实被用于许多特定用例。BASE数据库的知名度之所以提升，是因为Netflix、Apple、Spotify和Uber等全球知名在线服务在其应用（如用户配置文件数据存储）中使用了它们。

这些服务的一个共同特点是，无论您身处世界何处，它们都需要随时可访问。因此，如果其数据库集群的某一部分变得不可用，系统也需要能够无缝地响应用户请求。

![](img/8fe7ce88c823c13234b33c1db9b658eb_50.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_51.png)

## 总结 📝

本节课中我们一起学习了ACID和BASE分别是关系型数据库和NoSQL数据库中使用的一致性模型。

![](img/8fe7ce88c823c13234b33c1db9b658eb_53.png)

![](img/8fe7ce88c823c13234b33c1db9b658eb_54.png)

*   **ACID** 代表原子性、一致性、隔离性、持久性。
*   **BASE** 代表基本可用、软状态、最终一致性。

![](img/8fe7ce88c823c13234b33c1db9b658eb_56.png)

ACID模型侧重于**数据一致性**，而BASE模型侧重于**数据可用性**。两种一致性模型各有其适用性，应根据具体案例分析来选择使用哪一种。