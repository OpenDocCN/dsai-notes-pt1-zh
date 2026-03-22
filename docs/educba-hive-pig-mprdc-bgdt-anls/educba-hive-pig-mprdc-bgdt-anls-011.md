# 011：利用Hive理解复杂数据集

在本节课中，我们将学习如何使用Hive来分析复杂的数据集。我们将重点介绍Hive中的复杂数据类型，并演示如何将Pig生成的输出作为Hive的输入，通过处理和分析来理解数据。

---

## 概述

之前，我们使用MapReduce和Pig处理并分析了基于位置和评论的数据集。现在，我们将转向Hive来完成同样的分析任务。Hive是一个数据分析工具，它不直接处理原始Excel文件，而是使用Pig生成的输出作为输入。我们将学习如何处理这种包含复杂结构（如数组）的数据，并将其转换为易于分析的格式。

## Hive与MapReduce、Pig的对比

在深入处理数据之前，我们先来理解Hive与MapReduce和Pig的主要区别，这有助于我们选择适合的工具。

以下是七点关键区别：

1.  **抽象级别**
    *   MapReduce是**低级抽象**。我们需要编写详细的Java、Scala或Python代码来处理数据，例如将文本转换为字符串并按分隔符分割。
    *   Pig是**高级抽象**。我们只需编写少量Pig Latin语句（关系操作），所有复杂的MapReduce逻辑都被隐藏。
    *   Hive是**高级抽象**。我们使用类似SQL的HiveQL语言编写查询，它内部会执行MapReduce作业。

2.  **数据处理语言**
    *   MapReduce是**数据处理语言**，可以处理结构化、半结构化和非结构化数据（如文本、图像、PDF）。
    *   Pig是**数据流语言**。我们定义数据的处理流程（加载、过滤、分组），但不控制内部执行细节。
    *   Hive是**数据声明式语言**。我们通过声明式的SQL语句（如SELECT、WHERE、GROUP BY）来执行操作。

3.  **语言类型**
    *   MapReduce是**编译型语言**（需要编译Java代码并生成JAR文件）。
    *   Pig是**脚本语言**（使用Pig Latin脚本）。
    *   Hive使用**类SQL的查询语言**。

4.  **代码量**
    *   MapReduce需要**大量代码**。
    *   Pig只需要**少量语句**。
    *   Hive只需要**少量查询**。

5.  **代码性能**
    *   MapReduce的**代码性能高**，因为开发者完全控制执行逻辑。
    *   Pig和Hive的性能取决于其内部实现，开发者控制较少。

6.  **适用场景**
    *   MapReduce适合处理**复杂的自定义逻辑**及非结构化数据。
    *   Pig便于执行**连接（Join）** 操作。
    *   Hive专为**数据分析**和**聚合查询**设计。

![](img/406aa61aae9e6906eeeb8747772811a8_1.png)

7.  **处理的数据类型**
    *   MapReduce和Pig能处理**结构化和非结构化数据**。
    *   Hive主要处理**结构化数据**，这也是我们使用Pig的输出而非原始Excel文件作为Hive输入的原因。

理解了这些区别后，我们现在可以开始学习如何在Hive中处理复杂数据。

![](img/406aa61aae9e6906eeeb8747772811a8_3.png)

## Hive中的复杂数据类型

![](img/406aa61aae9e6906eeeb8747772811a8_5.png)

![](img/406aa61aae9e6906eeeb8747772811a8_7.png)

为了处理我们的数据集（其中“位置”字段包含多个用“|”分隔的值），我们需要先了解Hive的复杂数据类型。Hive主要有三种复杂类型：**数组（Array）**、**映射（Map）** 和**结构体（Struct）**。

![](img/406aa61aae9e6906eeeb8747772811a8_9.png)

### 1. 数组类型

![](img/406aa61aae9e6906eeeb8747772811a8_11.png)

![](img/406aa61aae9e6906eeeb8747772811a8_13.png)

![](img/406aa61aae9e6906eeeb8747772811a8_15.png)

![](img/406aa61aae9e6906eeeb8747772811a8_16.png)

数组允许在单个列中存储多个值。例如，位置“America|Germany|Paris|India|England”可以视为一个字符串数组。

![](img/406aa61aae9e6906eeeb8747772811a8_18.png)

**创建表并加载数据：**
```sql
CREATE TABLE tbl_array (
    id INT,
    name STRING,
    sub ARRAY<STRING>,
    city STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
COLLECTION ITEMS TERMINATED BY '$';
```
这里，`sub`列被定义为`ARRAY<STRING>`。`COLLECTION ITEMS TERMINATED BY ‘$’` 指定了数组中每个元素的分隔符是美元符号`$`。

![](img/406aa61aae9e6906eeeb8747772811a8_20.png)

![](img/406aa61aae9e6906eeeb8747772811a8_22.png)

**查询数组元素：**
数组元素通过索引访问，索引从0开始。
```sql
-- 获取每行数组的第二个元素（索引1）
SELECT sub[1] FROM tbl_array;
-- 获取每行数组的第一个元素（索引0）
SELECT sub[0] FROM tbl_array;
```

![](img/406aa61aae9e6906eeeb8747772811a8_23.png)

![](img/406aa61aae9e6906eeeb8747772811a8_25.png)

### 2. 映射类型

![](img/406aa61aae9e6906eeeb8747772811a8_27.png)

![](img/406aa61aae9e6906eeeb8747772811a8_29.png)

映射以键值对的形式存储数据。例如，“Pf#500,EPF#200”表示键`Pf`对应值`500`，键`EPF`对应值`200`。

**创建表并加载数据：**
```sql
CREATE TABLE tbl_map (
    id INT,
    name STRING,
    sal MAP<STRING, INT>,
    city STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
COLLECTION ITEMS TERMINATED BY '$'
MAP KEYS TERMINATED BY '#';
```
`sal`列被定义为`MAP<STRING, INT>`。`MAP KEYS TERMINATED BY ‘#’` 指定了键值对中键和值的分隔符。

![](img/406aa61aae9e6906eeeb8747772811a8_31.png)

![](img/406aa61aae9e6906eeeb8747772811a8_33.png)

![](img/406aa61aae9e6906eeeb8747772811a8_35.png)

![](img/406aa61aae9e6906eeeb8747772811a8_37.png)

![](img/406aa61aae9e6906eeeb8747772811a8_39.png)

![](img/406aa61aae9e6906eeeb8747772811a8_41.png)

**通过键查询值：**
```sql
-- 获取键‘Pf’对应的值
SELECT sal[‘Pf’] FROM tbl_map;
-- 获取键‘EPF’对应的值
SELECT sal[‘EPF’] FROM tbl_map;
```

![](img/406aa61aae9e6906eeeb8747772811a8_43.png)

![](img/406aa61aae9e6906eeeb8747772811a8_45.png)

![](img/406aa61aae9e6906eeeb8747772811a8_47.png)

### 3. 结构体类型

![](img/406aa61aae9e6906eeeb8747772811a8_49.png)

![](img/406aa61aae9e6906eeeb8747772811a8_51.png)

结构体允许在单个列中定义一组具有特定类型的命名字段。例如，“Bangalore$Karnataka$560001”可以定义为一个包含城市、州和邮编的结构。

**创建表并加载数据：**
```sql
CREATE TABLE tbl_struct (
    id INT,
    name STRING,
    address STRUCT<city:STRING, state:STRING, pin:INT>
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
COLLECTION ITEMS TERMINATED BY '$';
```
`address`列被定义为`STRUCT<city:STRING, state:STRING, pin:INT>`。

**查询结构体字段：**
使用`列名.字段名`的格式访问。
```sql
-- 获取城市信息
SELECT address.city FROM tbl_struct;
-- 获取州信息
SELECT address.state FROM tbl_struct;
```

![](img/406aa61aae9e6906eeeb8747772811a8_53.png)

![](img/406aa61aae9e6906eeeb8747772811a8_55.png)

掌握了这些复杂数据类型的基本操作后，我们就可以开始处理实际的数据了。

![](img/406aa61aae9e6906eeeb8747772811a8_57.png)

![](img/406aa61aae9e6906eeeb8747772811a8_59.png)

## 实战：分析Pig输出数据

![](img/406aa61aae9e6906eeeb8747772811a8_61.png)

![](img/406aa61aae9e6906eeeb8747772811a8_63.png)

现在，我们将使用Hive处理Pig生成的输出文件。该文件包含三列：书籍ID、书籍名称和位置（多个位置用“|”分隔）。我们的目标是将位置数组“展开”，使每个位置独占一行，以便进行聚合分析。

**步骤1：准备数据与环境**
首先，将Pig的输出文件从本地系统复制到HDFS，并创建一个Hive数据库。
```bash
# 将文件复制到HDFS（假设文件名为 part-m-00000）
hadoop fs -put part-m-00000 /user/hive/
# 启动Hive shell
hive
# 创建数据库
CREATE DATABASE book_analysis;
USE book_analysis;
```

![](img/406aa61aae9e6906eeeb8747772811a8_65.png)

![](img/406aa61aae9e6906eeeb8747772811a8_67.png)

**步骤2：创建Hive表**
创建一个表，其中`loc`列定义为字符串数组，以匹配输入文件中用“|”分隔的位置数据。
```sql
CREATE TABLE tbl_book (
    id INT,
    name STRING,
    loc ARRAY<STRING>
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
COLLECTION ITEMS TERMINATED BY ‘|’;
```

**步骤3：加载数据**
将HDFS上的数据加载到刚创建的表中。
```sql
LOAD DATA INPATH ‘/user/hive/part-m-00000’ OVERWRITE INTO TABLE tbl_book;
```
查询`tbl_book`表，可以看到`loc`列现在以数组形式显示（例如 `[“America”, “Germany”, “Paris”, “India”, “England”]`）。

![](img/406aa61aae9e6906eeeb8747772811a8_69.png)

**步骤4：展开数组数据**
使用Hive的`EXPLODE`函数和`LATERAL VIEW`将数组中的每个元素转换为单独的行。
```sql
SELECT id, name, loc_single
FROM tbl_book
LATERAL VIEW EXPLODE(loc) loc_table AS loc_single;
```
执行后，原来的一行数据（包含5个位置）会变成5行数据，每行包含相同的书籍ID和名称，但只有一个位置。

![](img/406aa61aae9e6906eeeb8747772811a8_71.png)

**步骤5：存储展开后的数据并进行分析**
为了便于后续分析，我们将展开后的数据插入到一个新表中。
```sql
-- 创建用于存储中间结果的简单表
CREATE TABLE tbl_intermediate (
    id INT,
    name STRING,
    loc STRING
);

![](img/406aa61aae9e6906eeeb8747772811a8_73.png)

-- 将展开的数据插入新表
INSERT OVERWRITE TABLE tbl_intermediate
SELECT id, name, loc_single
FROM tbl_book
LATERAL VIEW EXPLODE(loc) loc_table AS loc_single;
```
现在，我们可以轻松地对`tbl_intermediate`表进行聚合查询。例如，统计每个位置出现的次数：
```sql
SELECT loc, COUNT(*) AS count
FROM tbl_intermediate
GROUP BY loc;
```
此查询将按位置分组并计数，例如 America: 2, Brazil: 1，从而让我们清晰地了解数据的分布情况。

![](img/406aa61aae9e6906eeeb8747772811a8_75.png)

![](img/406aa61aae9e6906eeeb8747772811a8_77.png)

![](img/406aa61aae9e6906eeeb8747772811a8_79.png)

对于“评论”字段，我们可以遵循完全相同的步骤：将其定义为数组，使用`EXPLODE`函数展开，然后进行各种分析。

## 总结

![](img/406aa61aae9e6906eeeb8747772811a8_81.png)

在本节课中，我们一起学习了如何利用Hive处理复杂数据集。我们首先对比了Hive、MapReduce和Pig的特点与适用场景。然后，深入探讨了Hive的三种复杂数据类型：**数组（Array）**、**映射（Map）** 和**结构体（Struct）**，并学习了如何创建表、加载数据以及查询这些类型。

![](img/406aa61aae9e6906eeeb8747772811a8_83.png)

![](img/406aa61aae9e6906eeeb8747772811a8_85.png)

最后，我们进行了一个完整的实战演练：将Pig的输出作为Hive输入，通过定义数组列、使用`EXPLODE`函数展开数据，最终成功对书籍位置信息进行了聚合分析。这个方法同样适用于数据集中的其他复杂字段（如评论）。掌握这些技能，你就能有效地使用Hive将复杂数据转换为简单格式，从而执行深入的数据分析。