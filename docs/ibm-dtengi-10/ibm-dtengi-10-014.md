# 014：CRUD操作 🛠️

![](img/44b68190fe0f1ba52229a427e9fb3133_0.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_1.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_3.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_4.png)

在本节课中，我们将学习如何使用Mongo Shell连接MongoDB数据库，并执行基本的CRUD操作。CRUD是创建、读取、更新和删除操作的缩写，是数据库管理的核心。

![](img/44b68190fe0f1ba52229a427e9fb3133_6.png)

## 概述 📋

![](img/44b68190fe0f1ba52229a427e9fb3133_7.png)

Mongo Shell是MongoDB提供的一个命令行工具，用于与数据库进行交互。它是一个交互式的JavaScript接口，可以用来执行数据操作和管理任务。

## 连接数据库与查看信息 🔗

![](img/44b68190fe0f1ba52229a427e9fb3133_9.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_10.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_11.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_12.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_13.png)

上一节我们介绍了Mongo Shell的基本概念。本节中我们来看看如何连接数据库并查看其结构。

![](img/44b68190fe0f1ba52229a427e9fb3133_15.png)

首先，通过提供连接字符串连接到集群。连接成功后，可以查看所有数据库的列表。为简化说明，我们假设Shell已经连接到MongoDB实例。

![](img/44b68190fe0f1ba52229a427e9fb3133_16.png)

要开始操作`campus_management`数据库，请运行以下语句：
```javascript
use campus_management
```

然后，使用以下命令查看`campus_management`数据库中存在哪些集合：
```javascript
show collections
```
该命令显示两个集合：`staff`和`students`。

![](img/44b68190fe0f1ba52229a427e9fb3133_18.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_19.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_20.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_21.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_22.png)

## 创建操作 ➕

![](img/44b68190fe0f1ba52229a427e9fb3133_24.png)

现在，让我们开始进行创建操作。

![](img/44b68190fe0f1ba52229a427e9fb3133_26.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_27.png)

我们将向`students`集合中插入一个新文档。操作格式如下：
```javascript
db.students.insertOne({ "first_name": "John", "last_name": "Doe", "email": "john.doe@example.com" })
```
操作结果是确认操作成功的回执。由于这是插入操作，它会显示`insertedId`。

![](img/44b68190fe0f1ba52229a427e9fb3133_29.png)

`_id`是MongoDB中每个文档的必填字段。由于我们在John Doe的文档中没有自己提供，Mongo Shell驱动程序将自动添加`_id`字段并为我们生成对象ID。

![](img/44b68190fe0f1ba52229a427e9fb3133_30.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_31.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_32.png)

Mongo Shell是一个JavaScript解释器，这意味着您也可以在其中定义变量和执行其他函数。以下是创建包含两个文档的变量，并将其传递给`insertMany`函数的示例：
```javascript
let students_list = [
    { "first_name": "Jane", "last_name": "Doe", "email": "jane.doe@example.com" },
    { "first_name": "Bob", "last_name": "Smith", "email": "bob.smith@example.com" }
];
db.students.insertMany(students_list);
```

![](img/44b68190fe0f1ba52229a427e9fb3133_34.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_35.png)

## 读取操作 👁️

![](img/44b68190fe0f1ba52229a427e9fb3133_36.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_38.png)

上一节我们学习了如何创建数据。本节中我们来看看如何读取数据。

![](img/44b68190fe0f1ba52229a427e9fb3133_40.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_41.png)

以下是几种读取操作的示例。

![](img/44b68190fe0f1ba52229a427e9fb3133_43.png)

在第一个操作中，我们调用不带任何过滤条件的`findOne`函数。这将返回自然顺序中的第一个文档，即数据库在磁盘上引用文档的顺序。
```javascript
db.students.findOne()
```

![](img/44b68190fe0f1ba52229a427e9fb3133_44.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_45.png)

在第二个读取操作中，我们希望查找具有特定电子邮件地址的第一个学生。
```javascript
db.students.findOne({ "email": "john.doe@example.com" })
```
如果有多个文档符合条件，由于我们使用的是`findOne`函数，只会读取第一个。

![](img/44b68190fe0f1ba52229a427e9fb3133_47.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_49.png)

接下来，我们想检索所有姓氏为“Doe”的学生。
```javascript
db.students.find({ "last_name": "Doe" })
```

![](img/44b68190fe0f1ba52229a427e9fb3133_51.png)

最后，如果我们想计算有多少学生的姓氏是“Doe”，可以调用`count`函数。这个计数将在MongoDB上执行，只返回数值。
```javascript
db.students.count({ "last_name": "Doe" })
```

![](img/44b68190fe0f1ba52229a427e9fb3133_52.png)

## 更新操作 ✏️

![](img/44b68190fe0f1ba52229a427e9fb3133_54.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_55.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_56.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_57.png)

现在我们将执行替换操作。让我们检索一个学生记录并对文档进行一些更改。

我们创建一个新字段来标识哪些学生是在校生或仅在线学习。我们还将用校园邮箱更新电子邮件。完成所有更改后，我们将调用`replaceOne`函数。
```javascript
// 首先，检索并修改文档
let student = db.students.findOne({ "last_name": "Doe" });
student.student_type = "on_campus";
student.email = "john.doe@campus.edu";

![](img/44b68190fe0f1ba52229a427e9fb3133_59.png)

// 然后替换文档
db.students.replaceOne({ "last_name": "Doe" }, student)
```
此语句的结果是一个确认回执，包含有关多少文档匹配我们的过滤条件以及实际修改了多少文档的计数信息。

![](img/44b68190fe0f1ba52229a427e9fb3133_60.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_61.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_62.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_63.png)

在替换示例中，我们将整个文档与新更改一起发送回去。对于较大的文档，这将在客户端和数据库之间占用大量传输时间。相反，对于小的更改，我们可以使用MongoDB的原位更新。

![](img/44b68190fe0f1ba52229a427e9fb3133_65.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_66.png)

我们通过构建一个更改文档来实现这一点，在我们的例子中，该文档只有两个赋值，它们属于`$set`操作符。然后，我们将调用`updateOne`函数。
```javascript
db.students.updateOne(
    { "last_name": "Doe" },
    { $set: { "student_type": "on_campus", "email": "john.doe@campus.edu" } }
)
```

由于封锁，要将所有学生更新为仅在线学习，我们可以运行不带任何过滤条件和带有更改文档的`updateMany`函数。
```javascript
db.students.updateMany(
    {},
    { $set: { "student_type": "online_only" } }
)
```

![](img/44b68190fe0f1ba52229a427e9fb3133_68.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_69.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_70.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_71.png)

## 删除操作 🗑️

![](img/44b68190fe0f1ba52229a427e9fb3133_73.png)

与更新和查找类似，`deleteOne`函数将过滤条件作为第一个参数。运行该删除语句会提供一个确认回执和一个表示已删除文档数量的数字。
```javascript
db.students.deleteOne({ "last_name": "Smith" })
```

![](img/44b68190fe0f1ba52229a427e9fb3133_75.png)

![](img/44b68190fe0f1ba52229a427e9fb3133_76.png)

同样，您可以使用`deleteMany`函数删除多个符合条件的文档。
```javascript
db.students.deleteMany({ "student_type": "online_only" })
```

## 总结 🎯

![](img/44b68190fe0f1ba52229a427e9fb3133_78.png)

本节课中我们一起学习了以下内容：
*   Mongo Shell是MongoDB提供的一个交互式命令行工具，用于与数据库交互。
*   要使用Mongo Shell，首先需要通过连接字符串连接到集群。
*   使用`show dbs`列出数据库，使用`use <database_name>`选择数据库，使用`show collections`显示数据库中的集合。
*   CRUD操作包括创建、读取、更新和删除。
*   有用的函数包括：`insertOne`、`insertMany`、`findOne`、`find`、`count`、`replaceOne`、`updateOne`、`updateMany`、`deleteOne`和`deleteMany`。
*   最后，根据运行的操作，会返回不同种类的确认回执。