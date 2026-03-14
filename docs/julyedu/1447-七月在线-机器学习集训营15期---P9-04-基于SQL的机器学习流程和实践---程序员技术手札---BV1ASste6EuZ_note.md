# 🚀 机器学习集训营15期 - P9：基于SQL的机器学习流程和实践

![](img/a865a7c748e69aa4281094bc45320c08_0.png)

![](img/a865a7c748e69aa4281094bc45320c08_1.png)

![](img/a865a7c748e69aa4281094bc45320c08_3.png)

在本节课中，我们将学习如何在大数据环境下，使用SQL和Spark进行机器学习。我们将从SQL基础开始，逐步深入到Spark的核心概念和实际操作，并通过具体的机器学习案例来巩固所学知识。

---

## 📚 第一部分：SQL基础与大数据开发

SQL是用于访问和操作数据库的标准计算机语言。它能够完成数据的插入、查询、更新和删除等所有与数据库相关的操作。常见的数据库软件如MySQL、SQL Server和Oracle都使用SQL进行交互。

SQL是计算机专业的基础技能，也是大数据开发的必备技能。它独立于编程语言，与数据库紧密相关。学习SQL成本低，简单易懂，建议没有基础的同学花一天时间系统学习。

![](img/a865a7c748e69aa4281094bc45320c08_5.png)

![](img/a865a7c748e69aa4281094bc45320c08_7.png)

![](img/a865a7c748e69aa4281094bc45320c08_9.png)

![](img/a865a7c748e69aa4281094bc45320c08_11.png)

### SQL与Pandas的对比

SQL和Pandas在数据处理上有许多相似之处，只是实现方式不同。以下是两者的对比：

*   **数据选择**：
    *   SQL: `SELECT * FROM tips LIMIT 5;`
    *   Pandas: `tips[['total_bill', 'tip', 'smoker', 'time']].head(5)`
*   **条件筛选**：
    *   SQL: `SELECT * FROM tips WHERE time = 'Dinner' LIMIT 5;`
    *   Pandas: `tips[tips['time'] == 'Dinner'].head(5)`
*   **分组聚合**：
    *   SQL: `SELECT sex, COUNT(*) FROM tips GROUP BY sex;`
    *   Pandas: `tips.groupby('sex').size()`
*   **表连接**：
    *   SQL: `SELECT * FROM df1 INNER JOIN df2 ON df1.key = df2.key;`
    *   Pandas: `pd.merge(df1, df2, on='key')`

### 大数据开发岗位

![](img/a865a7c748e69aa4281094bc45320c08_13.png)

![](img/a865a7c748e69aa4281094bc45320c08_15.png)

大数据开发主要分为三类岗位：
1.  **大数据开发平台与管理**：如数据仓库工程师、数据运维工程师。
2.  **大数据平台的数据分析、挖掘与机器学习**：如数据开发工程师、算法工程师。
3.  **大数据分析结果展示**：如数据分析师。

我们重点关注第二个方向，即大数据平台下的机器学习和数据应用技能。掌握SQL和Spark对于应聘大公司的算法工程师或数据开发工程师岗位至关重要。

---

## ⚡ 第二部分：Spark介绍与使用

上一节我们介绍了SQL的基础知识，本节中我们来看看大数据处理框架Spark。Spark是加州大学伯克利分校开发的一个基于内存的并行计算框架，是目前流行的大数据处理工具之一。

![](img/a865a7c748e69aa4281094bc45320c08_17.png)

### Spark的优势

![](img/a865a7c748e69aa4281094bc45320c08_19.png)

*   **运行速度快**：基于内存计算，速度比Hadoop快10倍以上。
*   **易用性好**：支持Scala、Java、Python和R等多种语言。
*   **生态完善**：支持Spark SQL（交互式查询）、Spark Streaming（流处理）、MLlib（机器学习库）和GraphX（图计算库）。
*   **数据源丰富**：可以从CSV、JSON、MySQL、Hive等多种数据源读取数据。

![](img/a865a7c748e69aa4281094bc45320c08_21.png)

![](img/a865a7c748e69aa4281094bc45320c08_23.png)

![](img/a865a7c748e69aa4281094bc45320c08_25.png)

### Spark的核心概念：RDD

![](img/a865a7c748e69aa4281094bc45320c08_27.png)

![](img/a865a7c748e69aa4281094bc45320c08_29.png)

![](img/a865a7c748e69aa4281094bc45320c08_31.png)

RDD是Spark最基础的数据抽象，代表一个不可变、可分区的数据集合。Spark通过构建有向无环图来优化RDD的操作流程，实现高效的数据处理。

![](img/a865a7c748e69aa4281094bc45320c08_33.png)

![](img/a865a7c748e69aa4281094bc45320c08_35.png)

**创建RDD示例**：
```python
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("example").getOrCreate()
sc = spark.sparkContext
data = sc.parallelize(range(14), 4) # 创建RDD，分为4个分区
squared_rdd = data.map(lambda x: x*x) # 对每个元素进行平方操作（惰性计算）
print(squared_rdd.collect()) # 触发实际计算并收集结果
```

![](img/a865a7c748e69aa4281094bc45320c08_37.png)

![](img/a865a7c748e69aa4281094bc45320c08_39.png)

### Spark的核心概念：DataFrame

DataFrame是一种以命名列方式组织的分布式数据集，类似于关系型数据库中的表或Pandas中的DataFrame。它提供了丰富的操作接口，并且底层由RDD实现。

**创建与操作DataFrame示例**：
```python
# 从JSON文件创建DataFrame
df = spark.read.json("people.json")
df.show()
# 查看结构
df.printSchema()
# 选择列
df.select("name").show()
df.select(df['name'], df['age'] + 1).show()
# 条件筛选
df.filter(df['age'] > 21).show()
# 分组聚合
df.groupBy("age").count().show()
```

---

## 🔧 第三部分：Spark SQL使用案例

了解了RDD和DataFrame的基本操作后，本节我们来看看如何使用更接近SQL语法的Spark SQL进行数据查询和处理。

### 使用Spark SQL查询

首先需要将DataFrame注册为一个临时视图，然后即可使用SQL语句进行查询。

![](img/a865a7c748e69aa4281094bc45320c08_41.png)

![](img/a865a7c748e69aa4281094bc45320c08_43.png)

```python
# 将DataFrame注册为临时视图
df.createOrReplaceTempView("people")
# 使用Spark SQL查询
spark.sql("SELECT * FROM people").show()
spark.sql("SELECT name FROM people WHERE age > 20").show()
```

![](img/a865a7c748e69aa4281094bc45320c08_45.png)

![](img/a865a7c748e69aa4281094bc45320c08_47.png)

![](img/a865a7c748e69aa4281094bc45320c08_49.png)

![](img/a865a7c748e69aa4281094bc45320c08_51.png)

![](img/a865a7c748e69aa4281094bc45320c08_53.png)

![](img/a865a7c748e69aa4281094bc45320c08_55.png)

### 数据转换：RDD与DataFrame的互操作

![](img/a865a7c748e69aa4281094bc45320c08_57.png)

![](img/a865a7c748e69aa4281094bc45320c08_59.png)

在实际工作中，经常需要在RDD和DataFrame之间进行转换。

**1. 通过反射推断Schema（推荐）**：
这种方法通过样本数据自动推断数据类型。
```python
from pyspark.sql import Row
# 从文本文件创建RDD
lines = sc.textFile("people.txt")
parts = lines.map(lambda l: l.split(","))
# 将RDD映射为Row对象
people_rdd = parts.map(lambda p: Row(name=p[0], age=int(p[1])))
# 通过反射创建DataFrame
people_df = spark.createDataFrame(people_rdd)
people_df.show()
```

**2. 通过编程方式指定Schema**：
这种方法需要显式定义数据结构，适用于已知Schema的情况。
```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType
# 定义Schema
schema = StructType([
    StructField("name", StringType(), True),
    StructField("age", IntegerType(), True)
])
# 将RDD转换为Row对象，并与Schema结合创建DataFrame
row_rdd = parts.map(lambda p: Row(p[0], int(p[1])))
people_df2 = spark.createDataFrame(row_rdd, schema)
people_df2.show()
```

---

## 🤖 第四部分：Spark机器学习案例

最后，我们将运用前面所学的知识，在Spark平台上实现完整的机器学习流程。Spark MLlib库提供了常见的机器学习算法。

### 案例一：垃圾邮件分类（文本分类）

本案例演示如何使用Spark MLlib的Pipeline进行文本处理和朴素贝叶斯分类。

**核心流程**：
1.  **数据读取与预览**：读取CSV格式的邮件数据。
2.  **特征工程**：使用Tokenizer分词、StopWordsRemover去除停用词、CountVectorizer计算词频、IDF计算逆文档频率。
3.  **标签编码**：使用StringIndexer将字符串标签（如“spam”, “ham”）转换为数值。
4.  **构建Pipeline**：将上述步骤和朴素贝叶斯分类器串联。
5.  **训练与评估**：划分训练集和测试集，训练模型并评估准确率。

```python
from pyspark.ml import Pipeline
from pyspark.ml.feature import Tokenizer, StopWordsRemover, CountVectorizer, IDF, StringIndexer
from pyspark.ml.classification import NaiveBayes
from pyspark.ml.evaluation import MulticlassClassificationEvaluator

# 1. 读取数据
df = spark.read.csv("spam.csv", inferSchema=True, sep='\t')
df = df.withColumnRenamed("_c0", "class").withColumnRenamed("_c1", "text")

# 2. 定义Pipeline阶段
tokenizer = Tokenizer(inputCol="text", outputCol="words")
remover = StopWordsRemover(inputCol="words", outputCol="filtered")
cv = CountVectorizer(inputCol="filtered", outputCol="rawFeatures")
idf = IDF(inputCol="rawFeatures", outputCol="features")
label_indexer = StringIndexer(inputCol="class", outputCol="label")
nb = NaiveBayes()

# 3. 构建Pipeline
pipeline = Pipeline(stages=[label_indexer, tokenizer, remover, cv, idf, nb])

# 4. 划分数据集并训练
train_df, test_df = df.randomSplit([0.7, 0.3], seed=42)
model = pipeline.fit(train_df)
predictions = model.transform(test_df)

# 5. 评估
evaluator = MulticlassClassificationEvaluator(labelCol="label", predictionCol="prediction", metricName="accuracy")
accuracy = evaluator.evaluate(predictions)
print(f"测试集准确率: {accuracy}")
```

![](img/a865a7c748e69aa4281094bc45320c08_61.png)

![](img/a865a7c748e69aa4281094bc45320c08_63.png)

### 案例二：泰坦尼克号生存预测

![](img/a865a7c748e69aa4281094bc45320c08_65.png)

本案例演示如何不使用Pipeline，而是手动进行数据预处理，并训练一个随机森林模型。

**核心步骤**：
1.  **数据清洗**：处理缺失值，进行类型转换。
2.  **特征编码**：对分类变量（如“性别”）进行标签编码或独热编码。
3.  **特征组装**：将多个特征列组合成一个特征向量。
4.  **模型训练与预测**：使用随机森林分类器。

![](img/a865a7c748e69aa4281094bc45320c08_67.png)

![](img/a865a7c748e69aa4281094bc45320c08_69.png)

![](img/a865a7c748e69aa4281094bc45320c08_71.png)

```python
from pyspark.ml.feature import VectorAssembler, StringIndexer
from pyspark.ml.classification import RandomForestClassifier

![](img/a865a7c748e69aa4281094bc45320c08_73.png)

![](img/a865a7c748e69aa4281094bc45320c08_75.png)

![](img/a865a7c748e69aa4281094bc45320c08_77.png)

![](img/a865a7c748e69aa4281094bc45320c08_79.png)

![](img/a865a7c748e69aa4281094bc45320c08_81.png)

# 1. 读取并选择特征列
df = spark.read.csv("titanic.csv", header=True, inferSchema=True)
df = df.select("Survived", "Pclass", "Sex", "Age", "Fare")

![](img/a865a7c748e69aa4281094bc45320c08_83.png)

![](img/a865a7c748e69aa4281094bc45320c08_85.png)

![](img/a865a7c748e69aa4281094bc45320c08_87.png)

# 2. 处理缺失值并转换类型
df = df.dropna()
df = df.withColumn("Age", df["Age"].cast("float"))

![](img/a865a7c748e69aa4281094bc45320c08_89.png)

![](img/a865a7c748e69aa4281094bc45320c08_91.png)

![](img/a865a7c748e69aa4281094bc45320c08_93.png)

# 3. 对“Sex”列进行标签编码
indexer = StringIndexer(inputCol="Sex", outputCol="SexIndex")
df_indexed = indexer.fit(df).transform(df)

# 4. 组装特征向量
assembler = VectorAssembler(inputCols=["Pclass", "SexIndex", "Age", "Fare"], outputCol="features")
df_assembled = assembler.transform(df_indexed)

# 5. 划分数据集，训练随机森林模型
train_df, test_df = df_assembled.randomSplit([0.7, 0.3])
rf = RandomForestClassifier(labelCol="Survived", featuresCol="features")
model = rf.fit(train_df)
predictions = model.transform(test_df)
predictions.select("Survived", "prediction", "probability").show(10)
```

---

## 📝 总结

![](img/a865a7c748e69aa4281094bc45320c08_95.png)

![](img/a865a7c748e69aa4281094bc45320c08_97.png)

本节课我们一起学习了基于SQL和Spark的机器学习全流程。

1.  **SQL是基石**：作为数据查询和管理的标准语言，是从事数据相关工作的基础技能。
2.  **Spark是强大工具**：作为成熟的大数据处理和机器学习框架，Spark（特别是其PySpark API）能够高效处理海量数据，并内置了丰富的机器学习算法。
3.  **核心概念**：我们理解了RDD（弹性分布式数据集）和DataFrame这两种核心数据抽象，以及如何通过Spark SQL用熟悉的语法操作数据。
4.  **完整流程**：通过垃圾邮件分类和泰坦尼克号预测两个案例，我们实践了从数据读取、清洗、特征工程到模型训练、评估的完整机器学习流程。

![](img/a865a7c748e69aa4281094bc45320c08_99.png)

![](img/a865a7c748e69aa4281094bc45320c08_100.png)

对于希望在大数据平台下进行算法开发的工程师来说，掌握SQL和Spark是至关重要的。建议大家在课后积极动手实践，巩固本节课的知识点。