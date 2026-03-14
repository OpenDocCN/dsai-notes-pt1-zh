# 用 Pandas 进行数据处理与分析！P11：更多数据源 - Excel、JSON、SQL 等格式数据读写 📊

在本节课中，我们将学习如何使用 Pandas 读取和写入不同来源的数据。数据科学中，数据可能以多种格式存储，例如 CSV、Excel、JSON 或 SQL 数据库。通过本节课的学习，你将能够灵活地使用 Pandas 处理这些常见的数据格式。

## 📁 CSV 文件的读写

上一节我们介绍了本课程的目标。本节中，我们来看看最基础的数据格式——CSV 文件。我们之前已经使用过 CSV 文件，但让我们系统地回顾一下读取和写入的方法。

以下是读取 CSV 文件的基本步骤：

```python
import pandas as pd
df = pd.read_csv('data/survey_results.csv', index_col='Respondent')
print(df.head())
```

代码中，`read_csv` 函数用于读取文件。`index_col` 参数指定将 ‘Respondent’ 列设置为数据框的索引。

现在，假设我们对数据进行了筛选，例如只保留来自印度的调查结果，并希望将结果保存为新文件。

以下是写入 CSV 文件的步骤：

```python
# 筛选数据
filter_india = df['Country'] == 'India'
india_df = df.loc[filter_india]

# 将筛选后的数据框写入新的 CSV 文件
india_df.to_csv('data/modified_survey.csv')
```

运行后，你可以在文件系统中找到新生成的 `modified_survey.csv` 文件。

## 📄 处理制表符分隔文件 (TSV)

有时数据不是用逗号分隔，而是用制表符分隔。处理这种文件的方法与 CSV 类似，只需指定分隔符参数。

以下是写入 TSV 文件的示例：

```python
india_df.to_csv('data/modified_survey.tsv', sep='\t')
```

读取 TSV 文件时，同样需要在 `read_csv` 函数中设置 `sep='\t'`。

## 📊 Excel 文件的读写

Excel 是另一种非常流行的数据存储格式。要使用 Pandas 处理 Excel 文件，需要先安装必要的依赖包。

以下是安装所需包的命令：

```bash
pip install xlwt openpyxl xlrd
```

安装完成后，我们可以将数据框写入 Excel 文件。

以下是写入 Excel 文件的示例：

```python
india_df.to_excel('data/modified_survey.xlsx')
```

写入可能需要一些时间。完成后，你可以在 Excel 或类似软件中打开该文件查看数据。

要从 Excel 文件读取数据，使用 `read_excel` 函数。

以下是读取 Excel 文件的示例：

```python
test_df = pd.read_excel('data/modified_survey.xlsx', index_col='Respondent')
print(test_df.head())
```

## 📝 JSON 文件的读写

JSON 格式在 Web 开发和数据交换中非常常见。Pandas 也支持 JSON 格式的读写。

首先，我们将数据框写入 JSON 文件。默认情况下，数据会以“字典”形式（`orient='dict'`）存储。

以下是写入 JSON 文件的示例：

```python
india_df.to_json('data/modified_survey.json')
```

生成的 JSON 文件结构类似于一个字典，键是列名，值是该列所有数据的列表。

我们也可以改变存储格式，例如以“记录”列表形式（`orient='records'`）存储，每条记录占一行。

以下是按记录形式写入 JSON 文件的示例：

```python
india_df.to_json('data/modified_survey_records.json', orient='records', lines=True)
```

读取 JSON 文件时，需要根据文件的实际结构传入相应的参数。

以下是读取 JSON 文件的示例：

```python
test_json_df = pd.read_json('data/modified_survey_records.json', orient='records', lines=True)
print(test_json_df.head())
```

## 🗃️ SQL 数据库的读写

从 SQL 数据库读取和写入数据是数据处理中的常见任务。这需要先建立数据库连接。

首先，需要安装 SQLAlchemy 和对应数据库的驱动（例如，PostgreSQL 需要 `psycopg2`）。

以下是安装所需包的命令：

```bash
pip install sqlalchemy psycopg2-binary
```

安装完成后，在 Python 中创建数据库连接引擎。

![](img/77e06e113266309258f73542889b4603_1.png)

以下是创建 PostgreSQL 数据库连接的示例：

```python
from sqlalchemy import create_engine
import psycopg2

# 创建连接引擎（请勿在生产环境中明文存储密码）
engine = create_engine('postgresql://db_user:db_pass@localhost:5432/sample_db')
```

**注意**：在实际项目中，应使用环境变量等方式管理数据库凭证，而非直接写在代码中。

连接建立后，可以将数据框写入数据库的表中。如果表不存在，Pandas 会创建它。

以下是写入 SQL 数据库的示例：

```python
india_df.to_sql('sample_table', engine, index=False, if_exists='replace')
```

`if_exists='replace'` 参数表示如果表已存在，则替换它。你也可以使用 `'append'` 来追加数据。

要从数据库读取数据，可以使用 `read_sql` 函数读取整张表，或使用 `read_sql_query` 执行特定的 SQL 查询。

以下是读取 SQL 数据的两种方法：

```python
# 方法一：读取整张表
sql_df = pd.read_sql('sample_table', engine, index_col='respondent')

# 方法二：通过 SQL 查询读取
query_df = pd.read_sql_query('SELECT * FROM sample_table', engine, index_col='respondent')
print(sql_df.head())
print(query_df.head())
```

## 🌐 从 URL 读取数据

![](img/77e06e113266309258f73542889b4603_3.png)

![](img/77e06e113266309258f73542889b4603_4.png)

Pandas 支持直接从网络 URL 读取数据，前提是 URL 指向的是有效的数据文件（如 CSV、JSON）。

![](img/77e06e113266309258f73542889b4603_5.png)

![](img/77e06e113266309258f73542889b4603_6.png)

以下是从 GitHub URL 读取 JSON 数据的示例：

```python
url = 'https://raw.githubusercontent.com/username/repo/main/data/posts.json'
post_df = pd.read_json(url)
print(post_df.head())
```

![](img/77e06e113266309258f73542889b4603_8.png)

这种方法无需先将文件下载到本地，非常方便。

![](img/77e06e113266309258f73542889b4603_9.png)

## 📚 总结

本节课中，我们一起学习了如何使用 Pandas 读写多种格式的数据源。我们涵盖了以下内容：
*   读写 **CSV** 和制表符分隔的 **TSV** 文件。
*   安装依赖包后，读写 **Excel** 文件。
*   以不同格式（字典、记录列表）读写 **JSON** 文件。
*   连接 **SQL** 数据库，并进行数据的写入和读取操作。
*   直接从 **URL** 读取远程数据文件。

![](img/77e06e113266309258f73542889b4603_11.png)

掌握这些技能，你将能够轻松应对数据科学项目中遇到的各种数据源，高效地将数据导入 Pandas 进行分析，或将处理结果导出到所需格式。