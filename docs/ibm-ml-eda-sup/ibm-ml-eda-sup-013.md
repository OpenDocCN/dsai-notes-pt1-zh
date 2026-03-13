# 013：从SQL数据库读取数据 📊

![](img/3fab3db4190950b2686d26a66e0b8acd_1.png)

在本节课中，我们将学习如何使用Python连接到一个SQL数据库，并从中读取数据到Pandas DataFrame中。我们将重点介绍建立数据库连接、执行SQL查询以及使用`pandas.read_sql`函数的一些常用参数。

![](img/3fab3db4190950b2686d26a66e0b8acd_3.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_4.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_6.png)

---

## 概述

![](img/3fab3db4190950b2686d26a66e0b8acd_8.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_9.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_10.png)

我们将通过一个Jupyter Notebook示例，演示如何连接到SQLite数据库，执行查询，并将结果加载到Pandas中。课程目标包括创建数据库连接、执行基本SQL查询以及探索`read_sql`函数的关键参数。

![](img/3fab3db4190950b2686d26a66e0b8acd_11.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_13.png)

---

![](img/3fab3db4190950b2686d26a66e0b8acd_15.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_16.png)

## 导入必要的库

首先，我们需要导入处理数据和数据库连接所需的库。

![](img/3fab3db4190950b2686d26a66e0b8acd_18.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_19.png)

```python
import sqlite3
import pandas as pd
```

![](img/3fab3db4190950b2686d26a66e0b8acd_20.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_21.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_22.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_24.png)

**核心概念**：我们使用`sqlite3`来建立与SQLite数据库的连接，并使用`pandas`进行数据处理。

---

## 连接到数据库

![](img/3fab3db4190950b2686d26a66e0b8acd_26.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_27.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_28.png)

上一节我们导入了必要的库，本节中我们来看看如何建立与数据库的连接。

![](img/3fab3db4190950b2686d26a66e0b8acd_29.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_30.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_31.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_33.png)

数据库文件位于`data/classic_rock.db`路径下。我们使用`sqlite3.connect()`函数来创建连接对象。

![](img/3fab3db4190950b2686d26a66e0b8acd_35.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_36.png)

```python
# 定义数据库路径
database_path = 'data/classic_rock.db'

# 创建数据库连接
connection = sqlite3.connect(database_path)
```

![](img/3fab3db4190950b2686d26a66e0b8acd_38.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_39.png)

运行此代码后，`connection`变量将成为一个`sqlite3.Connection`对象，代表我们与数据库的活动连接。

![](img/3fab3db4190950b2686d26a66e0b8acd_41.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_42.png)

**注意**：在实际工作中，您可能使用其他数据库（如Microsoft SQL Server、PostgreSQL、MySQL）。以下是相应库的示例：
*   **SQLAlchemy**：适用于多种数据库。
*   **psycopg2**：适用于PostgreSQL。
*   **mysqlclient** 或 **PyMySQL**：适用于MySQL。

![](img/3fab3db4190950b2686d26a66e0b8acd_43.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_45.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_46.png)

---

![](img/3fab3db4190950b2686d26a66e0b8acd_48.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_50.png)

## 执行SQL查询并读取数据

现在我们已经建立了数据库连接，接下来可以执行SQL查询并将结果读入Pandas DataFrame。

![](img/3fab3db4190950b2686d26a66e0b8acd_52.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_53.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_54.png)

我们首先编写一个简单的SQL查询，从`rock_songs`表中选择所有数据。

![](img/3fab3db4190950b2686d26a66e0b8acd_56.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_57.png)

```python
# 定义SQL查询
query = "SELECT * FROM rock_songs"

![](img/3fab3db4190950b2686d26a66e0b8acd_59.png)

# 使用pandas的read_sql函数执行查询
observations = pd.read_sql(query, connection)

![](img/3fab3db4190950b2686d26a66e0b8acd_61.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_62.png)

# 显示数据的前5行
observations.head()
```

![](img/3fab3db4190950b2686d26a66e0b8acd_64.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_65.png)

**代码解释**：
1.  `query`变量存储了SQL语句`SELECT * FROM rock_songs`，意思是选择`rock_songs`表中的所有列和所有行。
2.  `pd.read_sql()`函数接收两个主要参数：SQL查询字符串和数据库连接对象。
3.  函数执行查询，并将返回的结果直接转换为Pandas DataFrame，存储在`observations`变量中。
4.  `.head()`方法用于查看DataFrame的前5行数据。

![](img/3fab3db4190950b2686d26a66e0b8acd_67.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_68.png)

---

![](img/3fab3db4190950b2686d26a66e0b8acd_70.png)

## 执行更复杂的SQL查询

![](img/3fab3db4190950b2686d26a66e0b8acd_71.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_73.png)

我们可以执行任何受支持的SQL查询。以下是一个更复杂的示例，它按艺术家和发行年份对歌曲进行分组，并计算歌曲数量和平均播放次数。

![](img/3fab3db4190950b2686d26a66e0b8acd_74.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_75.png)

```python
# 定义更复杂的SQL查询
complex_query = """
SELECT
    artist,
    release_year,
    COUNT(*) AS number_of_songs,
    AVG(play_count) AS average_plays
FROM rock_songs
GROUP BY artist, release_year
ORDER BY number_of_songs DESC
"""

![](img/3fab3db4190950b2686d26a66e0b8acd_77.png)

# 执行查询
summary_df = pd.read_sql(complex_query, connection)
summary_df.head()
```

![](img/3fab3db4190950b2686d26a66e0b8acd_79.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_80.png)

**查询解释**：
*   `SELECT`：选择艺术家、发行年份，并计算歌曲数量（`COUNT(*)`）和平均播放次数（`AVG(play_count)`）。
*   `FROM rock_songs`：指定数据来源的表。
*   `GROUP BY artist, release_year`：按艺术家和发行年份的组合进行分组。
*   `ORDER BY number_of_songs DESC`：按歌曲数量从多到少降序排列结果。

![](img/3fab3db4190950b2686d26a66e0b8acd_81.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_83.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_84.png)

运行后，我们将看到一个汇总的DataFrame，例如显示The Beatles在1967年发行了23首歌，平均播放次数约为6.5次。

![](img/3fab3db4190950b2686d26a66e0b8acd_86.png)

---

![](img/3fab3db4190950b2686d26a66e0b8acd_87.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_88.png)

## 探索`read_sql`的常用参数

![](img/3fab3db4190950b2686d26a66e0b8acd_90.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_92.png)

`pd.read_sql()`函数提供了一些有用的参数，可以在读取数据时进行格式化和优化。

![](img/3fab3db4190950b2686d26a66e0b8acd_94.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_95.png)

以下是三个常用参数：
1.  `coerce_float`：尝试将数字列强制转换为浮点数类型。
2.  `parse_dates`：将指定的列解析为日期时间类型。
3.  `chunksize`：指定每次读取的行数，用于处理大型数据集，返回一个可迭代的生成器对象，而不是一次性加载所有数据。

让我们看看如何使用这些参数。

![](img/3fab3db4190950b2686d26a66e0b8acd_97.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_98.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_99.png)

```python
# 使用参数化读取
query = "SELECT * FROM rock_songs"

![](img/3fab3db4190950b2686d26a66e0b8acd_100.png)

# 创建生成器对象，每次读取5行
observation_generator = pd.read_sql(
    query,
    connection,
    coerce_float=True,    # 尝试将数字转为浮点数
    parse_dates=['release_year'], # 将'release_year'列解析为日期
    chunksize=5           # 每次返回5行数据
)

![](img/3fab3db4190950b2686d26a66e0b8acd_102.png)

# 遍历生成器，查看前几个块（chunk）
for index, chunk in enumerate(observation_generator):
    if index < 5:  # 只查看前5个数据块
        print(f"Chunk {index}:")
        print(chunk)
        print("\n---\n")
    else:
        break
```

![](img/3fab3db4190950b2686d26a66e0b8acd_104.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_105.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_106.png)

**关键概念解释**：
*   **生成器 (Generator)**：类似于迭代器（如列表），但不会一次性将所有数据加载到内存中。它在你每次请求时“生成”下一部分数据（这里是一个包含5行的DataFrame），这对于处理远超内存容量的大型数据集非常高效。
*   `enumerate()`：在循环遍历生成器时，它不仅返回数据块（`chunk`），还返回一个索引号（`index`），从0开始递增。

![](img/3fab3db4190950b2686d26a66e0b8acd_108.png)

运行此代码，您将看到数据被分成了多个小块输出，并且`release_year`列的数据类型已从整数变为日期时间类型。

![](img/3fab3db4190950b2686d26a66e0b8acd_109.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_111.png)

---

![](img/3fab3db4190950b2686d26a66e0b8acd_112.png)

## 总结

![](img/3fab3db4190950b2686d26a66e0b8acd_114.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_115.png)

本节课中我们一起学习了如何从SQL数据库读取数据到Python环境中。

![](img/3fab3db4190950b2686d26a66e0b8acd_117.png)

我们首先导入了`sqlite3`和`pandas`库，然后建立了与SQLite数据库文件的连接。接着，我们使用`pd.read_sql()`函数执行了简单的`SELECT *`查询和更复杂的分组聚合查询，成功将SQL结果集转换为了Pandas DataFrame。

![](img/3fab3db4190950b2686d26a66e0b8acd_118.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_119.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_121.png)

最后，我们探索了`read_sql`函数的几个重要参数：`coerce_float`用于数据类型转换，`parse_dates`用于解析日期列，以及`chunksize`用于以流式方式分批读取大型数据集，这对管理内存使用至关重要。

![](img/3fab3db4190950b2686d26a66e0b8acd_123.png)

![](img/3fab3db4190950b2686d26a66e0b8acd_124.png)

掌握这些技能是进行数据探索和机器学习数据准备的基础步骤。