# 003：数据库对象 🗂️

![](img/5edce115bcd5f6c3dfe13fd22df98edf_0.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_1.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_3.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_4.png)

在本节课中，我们将学习关系数据库管理系统（RDBMS）中数据库对象的层次结构。我们将了解什么是数据库实例、模式（Schema）以及常见的数据库对象，例如表、约束、索引等。掌握这些概念对于有效组织和管理数据库至关重要。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_6.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_7.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_8.png)

## 🏗️ 数据库对象的层次结构

![](img/5edce115bcd5f6c3dfe13fd22df98edf_10.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_11.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_12.png)

关系数据库管理系统包含许多对象，数据库工程师和数据库管理员必须对它们进行组织。将表、约束、索引和其他数据库对象存储在层次结构中，有助于DBA管理安全性、维护和可访问性。

以下是一个典型的RDBMS层次结构示例，它概述了RDBMS是如何组织的。尽管不同产品之间可能存在细微差异，但大多数RDBMS都从一个**实例**开始。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_14.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_15.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_16.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_17.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_18.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_19.png)

## 🖥️ 数据库实例

一个实例是数据库或一组数据库的逻辑边界，您在其中组织数据库对象并设置配置参数。实例内的每个数据库都被分配一个唯一的名称，拥有自己的一组系统目录表（用于跟踪数据库内的对象），并拥有自己的配置文件。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_21.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_22.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_23.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_24.png)

您可以在同一物理服务器上创建多个实例，为每个实例提供唯一的数据库服务器环境。一个实例内的数据库及其对象与其他任何实例中的对象是隔离的。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_26.png)

以下是使用多个实例的常见场景：
*   将一个实例用于开发环境，另一个实例用于生产环境。
*   限制对敏感信息的访问。
*   控制高级管理访问权限。

并非所有RDBMS都使用实例的概念，它们通常将数据库配置信息管理在一个特殊的数据库中。在基于云的RDBMS中，术语“实例”指的是服务的特定运行副本。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_28.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_29.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_30.png)

## 📦 模式

![](img/5edce115bcd5f6c3dfe13fd22df98edf_32.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_33.png)

模式是一种专门的数据库对象，它提供了一种逻辑上分组其他数据库对象的方式。一个模式可以包含表、索引、约束和其他对象。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_35.png)

在大多数RDBMS中，当您创建数据库对象时，可以将其分配给一个模式。默认模式通常是当前登录用户的用户模式。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_37.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_38.png)

许多RDBMS使用专门的模式来保存特定数据库的配置信息和元数据。例如，系统模式中的表可以存储数据库用户列表及其访问权限、表上的索引信息、存在的任何数据库分区的详细信息以及用户定义的数据库类型。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_40.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_41.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_42.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_43.png)

## 🧱 数据库对象

![](img/5edce115bcd5f6c3dfe13fd22df98edf_45.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_46.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_47.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_48.png)

数据库对象是存在于数据库中的项目。数据库设计过程包括定义数据库对象及其相互关系。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_50.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_51.png)

您可以通过图形化数据库管理工具、脚本或通过API访问数据库来创建和管理数据库对象。如果使用SQL来创建或管理对象，您将使用数据定义语言（DDL）语句，如 `CREATE` 或 `ALTER`。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_52.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_53.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_55.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_56.png)

上一节我们介绍了数据库对象的基本概念，本节中我们来看看常见的数据库对象类型。

以下是常见的数据库对象列表：
*   **表**：由行和列组成的逻辑结构，用于存储数据。
*   **约束**：业务数据通常需要遵守某些限制或规则，例如，员工编号必须唯一。约束提供了一种强制执行此类规则的方法。
*   **索引**：索引是一组指针，用于提高性能并确保数据的唯一性。
*   **键**：键唯一标识表中的一行。键使DBA能够定义表之间的关系。
*   **视图**：视图提供了一种表示一个或多个表中数据的不同方式。视图不是实际的表，不需要永久存储。
*   **别名**：别名是对象（如表）的替代名称。DBA使用别名来提供更短、更简单的名称来引用对象。
*   **事件**：事件是数据库对象上的数据操作语言（DML）或数据定义语言（DDL）操作，可以启动触发器。
*   **触发器**：触发器定义了一组操作，以响应在指定表上的插入、更新或删除操作而执行。
*   **日志文件**：日志文件存储数据库中有关事务的信息。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_58.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_59.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_60.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_61.png)

## 📝 总结

![](img/5edce115bcd5f6c3dfe13fd22df98edf_62.png)

本节课中我们一起学习了关系数据库管理系统中数据库对象的层次结构。我们了解到：
*   **实例**是数据库或一组数据库的逻辑边界，用于组织数据库对象和设置配置参数。
*   **模式**是一种专门的数据库对象，它提供了一种逻辑上分组其他数据库对象的方式，并为数据库对象提供了命名上下文。
*   **数据库对象**是存在于数据库中的项目，例如表、约束、索引、键、视图、别名、触发器、事件和日志文件。

![](img/5edce115bcd5f6c3dfe13fd22df98edf_64.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_65.png)

![](img/5edce115bcd5f6c3dfe13fd22df98edf_66.png)

理解这些核心概念是进行有效数据库管理和设计的基础。