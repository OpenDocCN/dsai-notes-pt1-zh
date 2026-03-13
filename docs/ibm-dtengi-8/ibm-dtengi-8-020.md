# 020：使用Kafka构建事件流处理管道 🚀

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_0.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_1.png)

在本节课中，我们将学习如何使用Apache Kafka构建事件流处理管道。我们将了解Kafka的核心组件，学习如何发布和订阅事件流，并探讨一个端到端的事件流处理管道示例。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_3.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_4.png)

## 概述 📋

Kafka是一个分布式流处理平台，用于构建实时数据管道和流应用程序。它能够以高吞吐量、低延迟的方式处理数据流。本节课程将介绍Kafka的基本概念和操作。

---

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_6.png)

## Kafka核心组件 🏗️

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_7.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_8.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_9.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_10.png)

Kafka集群包含一个或多个**Broker**。你可以将Kafka Broker视为一个专用服务器，用于接收、存储、处理和分发事件。Broker由另一个名为**Zookeeper**的专用服务器进行同步和管理。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_12.png)

例如，在Broker 0中可能有一个日志主题和一个交易主题；在Broker 1中有一个支付主题和一个GPS主题；在Broker 2中有一个用户点击主题和一个用户搜索主题。每个Broker包含一个或多个**主题**。你可以将主题视为一个数据库，用于存储特定类型的事件，例如日志、交易和指标。

Broker负责将发布的事件保存到主题中，并将事件分发给订阅的消费者。

---

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_14.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_15.png)

## 分区与复制 🔄

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_16.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_17.png)

与许多其他分布式系统一样，Kafka实现了**分区**和**复制**的概念。它使用主题分区和副本来提高容错能力和吞吐量，从而使事件发布和消费可以并行进行，并利用多个Broker。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_19.png)

此外，即使某些Broker宕机，Kafka客户端仍然能够与其他正常工作的Broker中复制的目标主题进行交互。

例如，一个日志主题被分成两个分区（0和1），一个用户主题也被分成两个分区（0和1）。每个主题分区被复制成两个副本，并存储在不同的Broker中。

---

## Kafka命令行界面（CLI） 💻

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_21.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_22.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_23.png)

Kafka CLI或命令行界面客户端提供了一系列强大的脚本文件，供用户构建事件流处理管道。`kafka-topics`脚本可能是你经常用来管理Kafka集群中主题的工具，它的使用非常直接。

以下是`kafka-topics`脚本的一些常见用法：

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_25.png)

*   **创建主题**：使用`--create`选项。例如，创建一个名为`log_topic`、具有两个分区和两个副本的主题。请注意，许多Kafka命令（如`kafka-topics`）要求用户通过主机和端口（例如`localhost:9092`）来指定一个正在运行的Kafka集群。
*   **列出主题**：创建一些主题后，可以使用`--list`选项检查集群中所有已创建的主题。
*   **查看主题详情**：如果你想查看主题的更多详细信息，例如分区和副本信息，可以使用`--describe`选项。
*   **删除主题**：可以使用`--delete`选项删除一个主题。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_26.png)

接下来，我们将了解如何使用Kafka生产者发布事件。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_28.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_29.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_30.png)

---

## Kafka生产者 📤

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_32.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_33.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_34.png)

Kafka生产者是客户端应用程序，它们将事件发布到主题分区中。事件按照发布的顺序存储。

在Kafka生产者中发布事件时，可以选择性地为事件关联一个**键**。具有相同键的事件将被发布到同一个主题分区。未关联任何键的事件将以轮询方式发布到各个主题分区。

让我们通过一个例子看看如何将事件发布到主题分区。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_36.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_37.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_38.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_39.png)

假设你有一个事件源1，它生成各种日志条目；还有一个事件源2，它生成用户活动跟踪记录。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_40.png)

你可以创建一个Kafka生产者，将日志记录发布到日志主题分区；再创建一个用户生产者，将用户活动事件发布到用户主题分区。在生产者中发布事件时，可以选择为事件关联一个键，例如应用程序名称或用户ID。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_42.png)

与`kafka-topics` CLI类似，Kafka提供了`kafka-console-producer` CLI供用户管理生产者。最重要的方面是启动一个生产者，将事件写入或发布到一个主题。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_43.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_44.png)

例如，你可以启动一个生产者并将其指向`log_topic`，然后在控制台中输入一些消息来开始发布事件，例如`log1`、`log2`和`log3`。你可以为事件提供键，以确保具有相同键的事件进入同一个分区。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_46.png)

例如，你可以启动一个生产者到`user_topic`，并将`--parse-key`选项设置为`true`，同时指定键分隔符为逗号。然后，你可以按以下格式写入消息：`key,user1 value,login website`、`key,user1 value,click the top item`和`key,user1 value,logout website`。

这样，所有关于`user1`的事件都将保存在同一个分区中，以方便消费者读取。

---

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_48.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_49.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_50.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_51.png)

## Kafka消费者 📥

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_52.png)

一旦事件被发布并正确存储在主题分区中，你就可以创建消费者来读取它们。消费者是客户端应用程序，可以订阅主题并读取存储的事件。然后，事件目的地可以进一步从Kafka消费者读取事件。

消费者按照事件发布的相同顺序从主题分区读取数据。消费者还会存储每个主题分区的**偏移量**，作为最后读取的位置。通过偏移量，可以保证消费者读取到实时发生的事件。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_54.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_55.png)

通过将偏移量重置为零，也可以实现**回放**。这样，消费者可以从头开始读取主题分区中的所有事件。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_56.png)

在Kafka中，生产者和消费者是完全解耦的。因此，生产者不需要与消费者同步。事件存储在主题中后，消费者可以按照独立的计划来消费它们。

为了从主题分区读取已发布的日志和用户事件，你需要创建日志和用户消费者，并让它们订阅相应的主题。然后，Kafka会将事件推送给那些订阅的消费者，消费者再进一步发送给事件目的地。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_58.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_59.png)

使用`kafka-console-consumer`脚本启动消费者也很简单。例如，从`log_topic`读取事件，你只需要运行`kafka-console-consumer`脚本，并指定一个Kafka集群和要订阅的主题。启动的消费者将只读取从上次分区偏移量之后的新事件。这些事件被消费后，消费者的分区偏移量也会被更新并提交回Kafka。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_60.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_61.png)

通常，用户希望从头开始读取所有事件，作为所有历史事件的回放。为此，你只需要添加`--from-beginning`选项。现在，你可以从偏移量零开始读取所有事件。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_63.png)

---

## 端到端事件流处理管道示例 🌐

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_65.png)

让我们看一个更具体的例子，帮助你理解如何构建一个端到端的事件流处理管道。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_66.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_67.png)

假设你想要收集和分析天气和Twitter事件流，以便将人们在Twitter上谈论极端天气的情况关联起来。你可以使用两个事件源：
1.  IBM Weather API：获取实时和预测的天气数据（JSON格式）。
2.  Twitter API：获取实时推文和提及（同样是JSON格式）。

为了在Kafka中接收天气和Twitter的JSON数据，你需要在Kafka集群中创建一个天气主题和一个Twitter主题，并设置一些分区和副本。

为了将天气和Twitter JSON数据发布到这两个主题，你需要创建一个天气生产者和一个Twitter生产者。事件的JSON数据将被序列化为字节并保存在Kafka主题中。

为了从这两个主题读取事件，你需要创建一个天气消费者和一个Twitter消费者。存储在Kafka主题中的字节将被反序列化为事件的JSON数据。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_69.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_70.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_71.png)

如果你现在想将天气和Twitter事件的JSON数据从消费者传输到关系型数据库，你可以使用一个数据库写入器来解析这些JSON文件并创建数据库记录，然后使用SQL `INSERT`语句将这些记录写入数据库。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_72.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_73.png)

最后，你可以从关系型数据库中查询这些记录，并在仪表板中可视化和分析它们，从而完成端到端的管道。

---

## 总结 🎯

在本节课中，我们一起学习了：
*   Kafka的核心组件：
    *   **Broker**：接收、存储、处理和分发事件的专用服务器。
    *   **Topic**：事件的容器或数据库。
    *   **Partition**：将主题划分到不同Broker中。
    *   **Replication**：将分区复制到不同Broker中。
    *   **Producer**：将事件发布到主题中的Kafka客户端应用程序。
    *   **Consumer**：订阅主题并从其中读取事件的Kafka客户端应用程序。
*   我们还学习了：
    *   `kafka-topics` CLI用于管理主题。
    *   `kafka-console-producer` CLI用于管理生产者。
    *   `kafka-console-consumer` CLI用于管理消费者。

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_75.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_76.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_77.png)

![](img/2c3ac98b8f3916b5304dc58cd1f61e9e_78.png)

通过掌握这些核心概念和工具，你已经具备了使用Kafka构建基本事件流处理管道的基础知识。