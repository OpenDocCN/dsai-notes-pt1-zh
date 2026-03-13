# 028：使用Python处理Spark数据框 🐍

![](img/f82626dd33e61fdf0bbcfd3a9210d4e5_0.png)

在本节课中，我们将学习如何使用Python对Spark数据框进行基本操作。我们将涵盖数据框的创建、数据操作、数据清洗、数据聚合以及如何定义和使用用户自定义函数。

---

## 概述

我们将从安装必要的库和创建Spark会话开始。接着，我们会学习如何手动创建数据框以及从外部文件（如CSV）读取数据。然后，我们将探索如何选择、操作和清洗数据框中的列。之后，我们会介绍如何进行数据聚合操作。最后，我们将了解如何定义和使用用户自定义函数，并讨论其性能考量。

---

## 创建Spark会话

在创建Spark数据框之前，你需要创建一个Spark会话，它作为处理Spark数据框的入口点。

首先，从`pyspark.sql`导入`SparkSession`类。

```python
from pyspark.sql import SparkSession
```

![](img/f82626dd33e61fdf0bbcfd3a9210d4e5_2.png)

然后，通过调用`getOrCreate`方法创建一个名为“example”的Spark会话。如果名为“example”的会话已存在，此方法将获取它；否则，它将创建一个新的会话。

```python
spark = SparkSession.builder.appName("example").getOrCreate()
```

你可以查看会话的详细信息，包括Spark版本号和会话名称。

---

## 创建数据框

你可以手动创建数据框，也可以从外部数据源导入数据。

### 手动创建数据框

假设你想手动创建一个包含订单详情数据的数据框。你可以创建一个名为`orders_df`的数据框，指定之前创建的Spark会话对象，并对其调用`createDataFrame`方法。

此方法需要两个参数：第一个参数是数据，你可以提供一个元组列表，每个元组代表一行数据；第二个参数是模式，用于指定每列的名称和类型。

```python
orders_df = spark.createDataFrame([
    (1, "Product A", 10.0),
    (2, "Product B", 15.0)
], ["order_id", "product", "price"])
```

你可以使用`show`方法查看数据框的行。

```python
orders_df.show()
```

### 从CSV文件创建数据框

假设你想通过读取包含在线零售商店交易数据的CSV文件来创建数据框。你可以创建另一个名为`transactions_df`的数据框，使用相同的Spark会话对象，但这次调用`read.csv`方法，指定CSV文件的名称，并指明数据包含表头。

```python
transactions_df = spark.read.csv("transactions.csv", header=True)
```

你可以通过调用`show`方法并设置`n=5`来查看数据的前五行。

```python
transactions_df.show(5)
```

---

## 选择数据列

如果你只关心数据框中的某些列，可以先使用`columns`命令查看所有列名，然后使用`select`方法选择所需的列。

以下是查看所有列名并选择“price”、“quantity”和“country”列的示例。

```python
print(transactions_df.columns)
selected_df = transactions_df.select("price", "quantity", "country")
selected_df.show(5)
```

如果你想快速探索这些列的内容，可以使用`describe`方法计算每列的基本摘要统计信息，包括值计数、均值、标准差和最大值。

```python
selected_df.describe().show()
```

---

## 操作数据框

现在，我们来看看如何操作数据框，例如添加、更新、重命名或删除现有数据框中的列。

### 添加新列

假设你想创建一个新列，表示每个产品的支付金额。你可以调用`withColumn`方法，指定新列的名称（例如“amount”），并通过将现有数据框中的“price”和“quantity”列相乘来获取新列的值。

Spark会检查“amount”列是否已存在于数据框中。如果存在，它将用新值替换该列；如果不存在，它将使用这些值创建一个新列。

```python
transactions_df = transactions_df.withColumn("amount", transactions_df["price"] * transactions_df["quantity"])
transactions_df.show()
```

### 重命名列

接下来，让我们将“invoice”列重命名为“ID”。为此，我将调用`withColumnRenamed`方法，并指定现有列名为“invoice”，新列名应为“ID”。

```python
transactions_df = transactions_df.withColumnRenamed("invoice", "ID")
transactions_df.show()
```

### 删除列

最后，让我们删除“description”列。我将调用`drop`方法，并指定要删除的列的名称。

```python
transactions_df = transactions_df.drop("description")
transactions_df.show()
```

---

## 清洗数据

在数据框中清洗数据时，你可以通过调用`dropna`方法轻松删除包含空值的行。

```python
transactions_df = transactions_df.dropna()
```

你也可以调用`filter`方法，根据布尔条件删除行。例如，假设“quantity”列包含一些负值，而你只想考虑包含正值的行。你可以调用`filter`方法，并提供一个条件，即“quantity”列必须大于零。

```python
transactions_df = transactions_df.filter(transactions_df["quantity"] > 0)
transactions_df.show()
```

这样，你将只得到不包含空值且包含正数量值的行。

---

## 数据聚合操作

接下来，我们来看看数据框上的一些聚合操作。

### 计算每个订单的总金额

假设你想找出每个订单的总花费。首先，我将使用`groupBy`方法按订单ID对行进行分组。然后，我将对“amount”列求和，以获取每个订单ID的总金额。

```python
total_amount_df = transactions_df.groupBy("order_id").sum("amount")
total_amount_df.show()
```

### 统计每个国家的总行数并按降序排序

作为另一个例子，让我们统计每个国家的总行数，并按降序对计数进行排序。再次，我将调用`groupBy`方法按国家分组行，然后调用`count`方法进行计数。接着，我将调用`orderBy`方法按计数排序，并指定`ascending=False`，以便结果按降序排序。

```python
country_count_df = transactions_df.groupBy("country").count().orderBy("count", ascending=False)
country_count_df.show()
```

---

## 用户自定义函数

虽然Spark提供了许多内置功能，你可以在文档中找到，但它也支持用户自定义函数。

### 定义和使用UDF

例如，假设你想将所有国家名称转换为大写。你可以定义一个名为`to_upper`的函数，该函数接收一个字符串并返回大写的字符串。然后，你需要将此函数包装在UDF对象中。

首先，从`pyspark.sql.functions`导入`udf`类。

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType
```

然后，实例化一个名为`udf_to_upper`的UDF对象，指定刚刚定义的函数及其返回类型。

```python
def to_upper(s):
    return s.upper()

udf_to_upper = udf(to_upper, StringType())
```

现在，选择“ID”列和“country”列，并将`udf_to_upper`对象应用于“country”列。

```python
uppercased_df = transactions_df.select("ID", udf_to_upper(transactions_df["country"]).alias("country"))
uppercased_df.show()
```

你可以看到，“country”列中的所有条目现在都已转换为大写字符串。

### 使用装饰器定义UDF

或者，你可以添加带有返回类型的UDF装饰器，而不是将定义的函数包装在UDF对象中。然后，你可以直接将`to_upper`函数应用于“country”列。

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

@udf(returnType=StringType())
def to_upper(s):
    return s.upper()

uppercased_df = transactions_df.select("ID", to_upper(transactions_df["country"]).alias("country"))
uppercased_df.show()
```

请记住，Spark数据框是不可变的，因此使用UDF时，你实际上创建了一个新的数据框，其中包含大写的“country”列。

让我们用包含大写值的新列替换交易数据框中的“country”列。我将调用`withColumn`方法，并指定我想用大写值替换“country”列中的值。

```python
transactions_df = transactions_df.withColumn("country", to_upper(transactions_df["country"]))
transactions_df.show()
```

这是新的交易数据框。

---

## UDF性能考量

既然你已经了解了Python UDF的工作原理，在使用这些函数时应格外小心。这是因为Spark原生使用Scala，工作节点中的每个执行器都是一个Java虚拟机进程，托管一部分数据。当你使用Python UDF时，Spark会在工作节点内启动一个单独的Python进程，并需要从JVM传输数据以便由Python处理。Spark将数据序列化为Python可以理解的格式。然后，Python进程逐行对该数据执行函数，最后将行操作的结果返回给JVM。这个过程效率不高，因为JVM和Python进程会竞争内存资源，并且序列化和反序列化数据的成本很高。

为了获得更好的性能，你应该考虑用Scala或Java重写Python UDF，因为这些函数将直接在JVM内运行。好消息是，在Spark中注册这些UDF后，你仍然可以在Python中使用它们。

![](img/f82626dd33e61fdf0bbcfd3a9210d4e5_4.png)

---

## 总结

![](img/f82626dd33e61fdf0bbcfd3a9210d4e5_6.png)

在本节课中，我们一起学习了如何使用Python对Spark数据框进行基本操作。我们从创建Spark会话和数据框开始，然后探索了如何选择、操作和清洗数据。接着，我们介绍了数据聚合操作和用户自定义函数的定义与使用。最后，我们讨论了Python UDF的性能考量。在下一个视频中，我们将学习如何使用SparkSQL发出SQL查询。