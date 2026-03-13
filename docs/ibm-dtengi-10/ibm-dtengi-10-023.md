# 023：Cassandra查询语言Shell（CQLSH）简介

![](img/60714bb083a5b64b8c85c987575bc159_0.png)

![](img/60714bb083a5b64b8c85c987575bc159_1.png)

在本节课中，我们将学习Cassandra查询语言（CQL）以及其交互式Shell工具CQLSH。你将了解CQL的基本概念、如何运行CQL查询、CQLSH的常用命令行选项以及一些特殊的Shell命令。

---

![](img/60714bb083a5b64b8c85c987575bc159_3.png)

![](img/60714bb083a5b64b8c85c987575bc159_4.png)

## 🗣️ 什么是Cassandra查询语言（CQL）？

Cassandra查询语言，简称CQL，是与Apache Cassandra集群通信的主要语言。CQL具有简单直观、类似SQL的语法，允许创建键空间（keyspace）、表（table），以及执行插入、更新、删除和选择查询。

虽然CQL语法与SQL有相似之处，但两者之间存在许多差异。最显著的区别之一是CQL不支持JOIN语句。在Cassandra中，如果需要连接数据，你需要在存储时就将数据预先连接好。此外，尽管某些操作在语法上看似与SQL相似，但它们在Cassandra中的行为是不同的。例如，插入、更新和删除操作是直接在内存中进行的，无需事先读取数据来定位。

在后续的实践练习中，你会看到CQL关键字是大小写不敏感的。标识符（如表名）也是大小写不敏感的，除非它们被双引号括起来。例如，`select` 和 `SELECT` 被视为相同。如果你使用大写字母输入标识符名称，Cassandra默认会将其存储为小写。

![](img/60714bb083a5b64b8c85c987575bc159_6.png)

注释文本由双斜杠 `//` 表示，CQL会忽略这些注释。

---

## 💻 如何运行CQL查询？

运行CQL查询主要有两种方式：

1.  **通过编程方式运行**：使用获得许可的Cassandra客户端驱动程序。有多种选择，包括Java、Python和Scala。默认的驱动程序是开源的DataStax Java驱动。
2.  **通过CQL Shell客户端运行**：使用随Cassandra软件包提供的CQLSH工具。

![](img/60714bb083a5b64b8c85c987575bc159_8.png)

![](img/60714bb083a5b64b8c85c987575bc159_9.png)

![](img/60714bb083a5b64b8c85c987575bc159_10.png)

![](img/60714bb083a5b64b8c85c987575bc159_11.png)

CQLSH是一个基于Python的命令行Shell，它使用CQL与Cassandra集群通信。该软件随每个Cassandra软件包一起提供。CQLSH默认连接到运行命令的单个节点，也可以通过命令行选项指定要连接的节点。

![](img/60714bb083a5b64b8c85c987575bc159_12.png)

![](img/60714bb083a5b64b8c85c987575bc159_13.png)

市场上还有其他使用CQL与Cassandra通信的CQL编辑器。

---

## 🛠️ CQLSH命令行选项简介

在使用CQLSH之前，了解一些常用的命令行选项很有帮助。以下是几个关键选项：

![](img/60714bb083a5b64b8c85c987575bc159_15.png)

![](img/60714bb083a5b64b8c85c987575bc159_16.png)

*   `--help`：显示关于CQLSH命令选项的帮助信息。
*   `--version`：用于检查正在使用的CQLSH版本。
*   `--user` 和 `--password`：用于身份验证。
*   `--keyspace`：指定要连接到的键空间。
*   `--file`：启用从给定文件执行命令。
*   `--request-timeout`：可以为查询设置超时时间，默认为10秒。

![](img/60714bb083a5b64b8c85c987575bc159_17.png)

---

## 📝 CQLSH使用示例

让我们看一个简单的CQLSH输出示例。假设我们使用 `intro_cassandra` 键空间。

![](img/60714bb083a5b64b8c85c987575bc159_19.png)

首先，我们选择组ID为12的用户：

![](img/60714bb083a5b64b8c85c987575bc159_20.png)

![](img/60714bb083a5b64b8c85c987575bc159_21.png)

![](img/60714bb083a5b64b8c85c987575bc159_22.png)

![](img/60714bb083a5b64b8c85c987575bc159_23.png)

```sql
USE intro_cassandra;
SELECT * FROM users WHERE group_id = 12;
```

![](img/60714bb083a5b64b8c85c987575bc159_24.png)

![](img/60714bb083a5b64b8c85c987575bc159_26.png)

接着，我们向 `groups` 表插入一条组ID为12的新记录：

![](img/60714bb083a5b64b8c85c987575bc159_28.png)

```sql
INSERT INTO groups (group_id, group_name) VALUES (12, 'New Group');
```

最后，再次选择组ID为12的用户，并查看插入的数据：

```sql
SELECT * FROM groups WHERE group_id = 12;
```

![](img/60714bb083a5b64b8c85c987575bc159_30.png)

![](img/60714bb083a5b64b8c85c987575bc159_31.png)

![](img/60714bb083a5b64b8c85c987575bc159_32.png)

执行这些命令后，你将在CQLSH中看到相应的查询结果。

![](img/60714bb083a5b64b8c85c987575bc159_33.png)

![](img/60714bb083a5b64b8c85c987575bc159_34.png)

![](img/60714bb083a5b64b8c85c987575bc159_35.png)

![](img/60714bb083a5b64b8c85c987575bc159_36.png)

---

## ⚙️ CQLSH的特殊命令

CQLSH还提供了一些特殊命令来辅助操作和管理。以下是其中几个：

*   `CAPTURE`：捕获命令的输出并将其添加到文件中。
*   `CONSISTENCY`：显示当前的一致性级别并设置新的级别。
*   `COPY`：将数据复制到Cassandra或从Cassandra复制出来。
*   `DESCRIBE`：描述当前的Cassandra集群及其对象。
*   `EXIT`：终止CQL会话。
*   `PAGING`：启用或禁用查询结果的分页显示。
*   `TRACING`：启用或禁用请求跟踪。

在实践环节中，你将有机会体验其中一些命令。接下来，我们将更详细地了解 `CONSISTENCY` 和 `COPY` 命令。

![](img/60714bb083a5b64b8c85c987575bc159_38.png)

![](img/60714bb083a5b64b8c85c987575bc159_39.png)

---

![](img/60714bb083a5b64b8c85c987575bc159_40.png)

![](img/60714bb083a5b64b8c85c987575bc159_41.png)

![](img/60714bb083a5b64b8c85c987575bc159_42.png)

![](img/60714bb083a5b64b8c85c987575bc159_43.png)

### 深入理解 `CONSISTENCY` 命令

![](img/60714bb083a5b64b8c85c987575bc159_44.png)

![](img/60714bb083a5b64b8c85c987575bc159_45.png)

![](img/60714bb083a5b64b8c85c987575bc159_46.png)

Cassandra具有可调一致性（tunable consistency）。这意味着你可以在操作级别设置Cassandra的一致性级别。

在Cassandra中，一致性（consistency）指的是，为了使一个写或读操作被视为成功，需要从所有副本节点中获得响应的节点数量。

`CONSISTENCY` 命令可用的选项包括：
*   `ONE`， `TWO` 或 `THREE`：指定需要响应的具体节点数量。
*   `QUORUM`：需要集群中所有副本节点的大多数节点响应。
*   `ALL`：需要所有副本节点都响应。
*   `LOCAL_QUORUM`：需要本地数据中心（根据键空间设置的复制策略）中大多数节点响应。

**示例**：假设我们有一个包含8个节点和2个数据中心的Cassandra集群。一个数据中心的复制因子（Replication Factor）为2，另一个为3，那么总复制因子就是5。

![](img/60714bb083a5b64b8c85c987575bc159_48.png)

如果我们在一个写操作上设置一致性级别为 `QUORUM`，那么：
*   `QUORUM` 意味着需要副本节点中的大多数节点响应。在这个例子中，大多数是3个节点。
*   写操作会发送给所有5个副本节点，但需要至少3个来自两个数据中心的节点响应，操作才能被视为成功。

![](img/60714bb083a5b64b8c85c987575bc159_49.png)

![](img/60714bb083a5b64b8c85c987575bc159_50.png)

![](img/60714bb083a5b64b8c85c987575bc159_51.png)

---

### 深入理解 `COPY` 命令

虽然Cassandra中的批量复制通常应通过特殊程序完成，但如果你只是想测试模型，或者正在处理较小的文本分隔数据集，那么可以使用 `COPY FROM` 或 `COPY TO` 操作将数据导入Cassandra或从Cassandra导出。

*   `COPY TO`：将表中的数据导出到CSV文件中。每一行都被写入目标文件的一行，字段之间由分隔符分隔。
*   `COPY FROM`：将CSV文件中的数据导入到现有表中。源文件中的每一行都被导入为一行数据。数据中的所有行必须包含相同数量的字段，并且主键字段必须有值。该过程会验证主键并相应地导入数据。

![](img/60714bb083a5b64b8c85c987575bc159_53.png)

---

![](img/60714bb083a5b64b8c85c987575bc159_54.png)

![](img/60714bb083a5b64b8c85c987575bc159_55.png)

## 🎯 课程总结

在本节课中，我们一起学习了以下内容：

![](img/60714bb083a5b64b8c85c987575bc159_57.png)

1.  CQL是与Apache Cassandra集群通信的主要语言。CQL关键字和标识符是大小写不敏感的。
2.  CQL查询可以通过编程方式（使用Cassandra客户端驱动程序）运行，也可以在Cassandra提供的基于Python的CQL Shell客户端上运行。
3.  使用CQL Shell，你可以创建、修改和删除键空间与表，插入、更新和删除数据，以及使用 `SELECT` 执行查询。
4.  CQL Shell提供了一些特殊命令，包括 `CONSISTENCY` 和 `COPY` 命令。`CONSISTENCY` 命令可用于调整数据的一致性级别，而 `COPY` 命令可用于向Cassandra导入数据或从Cassandra导出数据。