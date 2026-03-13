#  123：Great Expectations 工作流示例 🧪

![](img/72435659aa1093ae39f80c7f55dc8205_0.png)

在本节课中，我们将通过一个具体示例，学习如何使用 Great Expectations 来验证数据质量。我们将以 DVD 租赁数据库中的 `payment` 表为例，检查其 `payment_id` 列的唯一性、`customer_id` 列的非空性以及 `amount` 列的非负性。

---

## 项目初始化与环境准备

上一节我们介绍了 Great Expectations 的核心组件和典型工作流。本节中，我们来看看如何在一个具体项目中应用这些步骤。

首先，需要在本地机器上设置一个 PostgreSQL 数据库并加载数据。接着，在终端中安装 Great Expectations 包。

![](img/72435659aa1093ae39f80c7f55dc8205_2.png)

```bash
pip install great_expectations
```

然后，通过以下命令初始化一个新的 Great Expectations 项目。此命令会初始化数据上下文对象，设置项目文件夹结构，并创建检查点、期望、数据文档和验证存储等后端存储（默认为本地目录）。

```bash
great_expectations init
```

在提示时输入 `y` 以继续。请注意，后端存储的位置可以配置，例如在实验环境中，你可能会将其配置为 S3 存储桶。本视频中，我们将保持其为本地目录。

为了与 Great Expectations 的各个组件交互，我们将在同一根目录下启动一个 Jupyter Notebook 并创建一个新的笔记本文件。

---

![](img/72435659aa1093ae39f80c7f55dc8205_4.png)

![](img/72435659aa1093ae39f80c7f55dc8205_5.png)

## 连接数据源与创建数据资产

在笔记本文件中，我们首先导入 Great Expectations 包，并获取项目的上下文对象。通过此对象，我们可以连接数据源、定义期望、创建验证器并运行检查点。

```python
import great_expectations as gx
context = gx.get_context()
```

接下来，使用上下文对象创建数据源对象。Great Expectations 提供了多种方法来连接不同的数据源。为了连接到本地的 SQL 数据库，我们调用 `add_sql` 方法。

```python
# 创建连接字符串（请替换为你的实际数据库信息）
connection_string = "postgresql://username:password@localhost:5432/dvdrental"
datasource = context.sources.add_sql(
    name="my_datasource",
    connection_string=connection_string
)
```

连接建立后，我们从数据源创建数据资产。由于我们只关注 `payment` 表，因此添加一个表资产。

```python
asset = datasource.add_table_asset(
    name="payment_tbl",
    table_name="payment"
)
```

如果你想基于日期或列值对数据资产进行分批，Great Expectations 也提供了相应的方法。例如，我们可以按 `payment_date` 列的月份进行分批。

```python
asset.add_splitter_column_value(column_name="payment_date", method="monthly")
```

最后，在资产对象上调用 `build_batch_request` 方法来创建批次请求对象。

```python
batch_request = asset.build_batch_request()
```

在继续工作流之前，我们可以快速查看一下批次划分情况。

```python
batches = asset.get_batch_list_from_batch_request(batch_request)
for batch in batches:
    print(batch.batch_spec)
```

你将看到数据被分成了四个批次，每个批次对应一个月的数据。

---

## 定义数据期望

现在我们已经创建了批次请求对象，接下来定义数据期望。我们将以交互方式直接使用验证器来完成。

首先，创建一个期望套件，它将包含我们定义的所有期望。

```python
context.add_or_update_expectation_suite("my_suite")
```

然后，使用上下文对象获取验证器，并传入批次请求对象和期望套件名称。

```python
validator = context.get_validator(
    batch_request=batch_request,
    expectation_suite_name="my_suite"
)
```

现在，使用验证器，你可以调用期望库中的任何期望方法。以下是我们要定义的三个期望：

1.  检查 `payment_id` 列值的唯一性。
2.  检查 `customer_id` 列不包含空值。
3.  检查 `amount` 列的所有值都是非负的。

```python
# 期望1: payment_id 列值唯一
validator.expect_column_values_to_be_unique(column="payment_id")

# 期望2: customer_id 列值非空
validator.expect_column_values_to_not_be_null(column="customer_id")

# 期望3: amount 列最小值在0及以上（即非负）
validator.expect_column_min_to_be_between(column="amount", min_value=0)
```

结果显示所有测试都成功通过。请注意，虽然结果只显示了最后一个批次，但测试实际上在所有批次上都已运行。

为了能在其他会话中使用这些期望，我们需要保存它们。

```python
validator.save_expectation_suite(discard_failed_expectations=False)
```

此方法将包含三个期望的期望套件 `my_suite` 保存到期望存储中。

---

## 使用检查点自动化验证过程

为了自动化验证过程，我们将创建一个检查点对象。

```python
checkpoint = context.add_or_update_checkpoint(
    name="my_checkpoint",
    validations=[
        {
            "batch_request": batch_request,
            "expectation_suite_name": "my_suite"
        }
    ]
)
```

`validations` 参数是一个列表，包含数据批次与其对应期望套件的配对。在本例中，我们只使用了一个批次请求和套件。

现在，运行检查点。

```python
checkpoint_result = checkpoint.run()
```

要获取每个结果的更详细信息，可以查看数据文档。

```python
context.build_data_docs()
```

此方法会返回一个链接，打开后可以查看数据文档。在数据文档中，你可以找到每个批次上执行的验证结果。所有测试都应显示为成功。点击任何一行，可以找到关于已评估期望的统计信息、成功与失败的期望、对每列执行的期望及其对应结果。

你还可以在“期望套件”选项卡下找到关于 `my_suite` 的详细信息。

---

## 总结

![](img/72435659aa1093ae39f80c7f55dc8205_7.png)

本节课中，我们一起学习了如何将 Great Expectations 应用于实际数据验证任务。我们完成了从项目初始化、连接数据源、定义数据期望，到使用检查点自动化验证并查看结果的完整工作流。现在，轮到你通过实验来练习使用 Great Expectations 验证数据了。