# 018：从Python访问MongoDB 🐍

![](img/7603e49e2ca8e75f35e37ac602741823_0.png)

![](img/7603e49e2ca8e75f35e37ac602741823_1.png)

在本节课中，我们将学习如何使用Python与MongoDB数据库进行交互。具体来说，我们将了解什么是MongoClient，并学习如何使用Python对校园管理数据库执行基本的CRUD（创建、读取、更新、删除）操作。

![](img/7603e49e2ca8e75f35e37ac602741823_3.png)

## 🧩 理解MongoClient

MongoClient是一个帮助您与MongoDB交互的类。要使用它，我们首先需要从`pymongo`导入它，`pymongo`是Python的官方MongoDB驱动程序。

![](img/7603e49e2ca8e75f35e37ac602741823_5.png)

以下是建立连接并获取集合引用的基本步骤：

1.  **导入MongoClient**：从`pymongo`库中导入`MongoClient`类。
2.  **创建客户端实例**：使用MongoDB服务器的地址（URI）创建一个`MongoClient`实例。这个操作在代码中通常只执行一次。
3.  **获取数据库对象**：通过客户端实例获取指向特定数据库（例如“campus_management”）的对象。
4.  **获取集合对象**：从数据库对象中获取指向特定集合（例如“students”）的对象。

这个过程可以用以下代码概括：

```python
from pymongo import MongoClient

# 创建MongoClient实例，连接到本地MongoDB服务器
client = MongoClient('mongodb://localhost:27017/')

![](img/7603e49e2ca8e75f35e37ac602741823_7.png)

# 获取‘campus_management’数据库对象
db = client['campus_management']

![](img/7603e49e2ca8e75f35e37ac602741823_8.png)

![](img/7603e49e2ca8e75f35e37ac602741823_9.png)

# 获取‘students’集合对象
students_collection = db['students']
```

![](img/7603e49e2ca8e75f35e37ac602741823_10.png)

![](img/7603e49e2ca8e75f35e37ac602741823_11.png)

现在，我们已经获得了对`students`集合的引用，可以开始执行数据操作了。

![](img/7603e49e2ca8e75f35e37ac602741823_12.png)

![](img/7603e49e2ca8e75f35e37ac602741823_13.png)

## ➕ 创建（Create）操作

我们可以向集合中插入新的文档。以下是两种插入数据的方法。

### 插入单个文档

使用`insert_one`函数插入一个文档。该函数以要插入的文档字典作为输入。

![](img/7603e49e2ca8e75f35e37ac602741823_15.png)

![](img/7603e49e2ca8e75f35e37ac602741823_16.png)

```python
new_student = {
    "first_name": "John",
    "last_name": "Doe",
    "email": "john.doe@example.com"
}
result = students_collection.insert_one(new_student)
print(f"插入的文档ID: {result.inserted_id}")
```

![](img/7603e49e2ca8e75f35e37ac602741823_17.png)

![](img/7603e49e2ca8e75f35e37ac602741823_18.png)

### 批量插入文档

![](img/7603e49e2ca8e75f35e37ac602741823_20.png)

使用`insert_many`函数一次性插入多个文档。该函数接受一个文档字典的列表作为输入。

```python
new_students = [
    {"first_name": "Alice", "last_name": "Smith", "email": "alice@example.com"},
    {"first_name": "Bob", "last_name": "Johnson", "email": "bob@example.com"}
]
result = students_collection.insert_many(new_students)
print(f"插入的文档ID列表: {result.inserted_ids}")
```

## 🔍 读取（Read）操作

![](img/7603e49e2ca8e75f35e37ac602741823_22.png)

![](img/7603e49e2ca8e75f35e37ac602741823_23.png)

从集合中查询数据是常见的操作。MongoDB提供了多种查询方法。

### 查询单个文档

使用`find_one`函数。如果不提供过滤条件，它将返回集合中自然顺序下的第一个文档（即数据库在磁盘上引用文档的顺序）。

![](img/7603e49e2ca8e75f35e37ac602741823_25.png)

![](img/7603e49e2ca8e75f35e37ac602741823_26.png)

```python
# 获取第一个文档
first_student = students_collection.find_one()
print(first_student)

# 根据条件获取第一个匹配的文档（例如，按邮箱查找）
student_by_email = students_collection.find_one({"email": "john.doe@example.com"})
print(student_by_email)
```

![](img/7603e49e2ca8e75f35e37ac602741823_28.png)

![](img/7603e49e2ca8e75f35e37ac602741823_29.png)

> **注意**：即使有多个文档匹配条件，`find_one`也只会返回第一个。

### 查询多个文档

使用`find`函数可以获取匹配条件的所有文档。它返回一个**游标（Cursor）**对象。

![](img/7603e49e2ca8e75f35e37ac602741823_31.png)

![](img/7603e49e2ca8e75f35e37ac602741823_32.png)

```python
# 查找所有姓氏为‘Doe’的学生
cursor = students_collection.find({"last_name": "Doe"})
```

![](img/7603e49e2ca8e75f35e37ac602741823_33.png)

### 理解游标（Cursor）

当对MongoDB集合执行`find`操作时，返回的是一个游标。这个游标指向MongoDB中的文档。要获取所有文档，需要遍历或转换这个游标。

```python
# 将游标转换为列表以获取所有文档
all_does = list(cursor)
for student in all_does:
    print(student)
```

![](img/7603e49e2ca8e75f35e37ac602741823_35.png)

![](img/7603e49e2ca8e75f35e37ac602741823_36.png)

### 统计文档数量

![](img/7603e49e2ca8e75f35e37ac602741823_37.png)

如果只想统计匹配条件的文档数量，可以使用`count_documents`函数。计数操作在MongoDB服务器端完成，只将数字结果返回给客户端。

![](img/7603e49e2ca8e75f35e37ac602741823_39.png)

```python
count = students_collection.count_documents({"last_name": "Doe"})
print(f"姓氏为Doe的学生数量: {count}")
```

![](img/7603e49e2ca8e75f35e37ac602741823_40.png)

## ✏️ 更新（Update）操作

![](img/7603e49e2ca8e75f35e37ac602741823_42.png)

更新文档有两种主要方式：替换整个文档或进行局部更新。

### 替换整个文档

![](img/7603e49e2ca8e75f35e37ac602741823_44.png)

首先检索一个文档，在本地修改后，使用`replace_one`函数将其替换回数据库。第一个参数是过滤条件，第二个参数是更新后的Python字典对象。

![](img/7603e49e2ca8e75f35e37ac602741823_45.png)

```python
# 1. 检索一个学生文档
student_to_update = students_collection.find_one({"last_name": "Doe"})

# 2. 修改文档（添加新字段，更新邮箱）
student_to_update["on_campus"] = True
student_to_update["email"] = "john.doe@campus.edu"

![](img/7603e49e2ca8e75f35e37ac602741823_47.png)

# 3. 替换文档
result = students_collection.replace_one({"last_name": "Doe"}, student_to_update)
print(f"匹配到的文档数: {result.matched_count}, 修改的文档数: {result.modified_count}")
```

![](img/7603e49e2ca8e75f35e37ac602741823_48.png)

![](img/7603e49e2ca8e75f35e37ac602741823_49.png)

对于大型文档，这种方式会在客户端和数据库之间传输大量数据，可能效率较低。

### 局部更新（推荐）

对于小的更改，可以使用MongoDB的原地更新操作符（如`$set`）。这不需要先检索整个文档，只需构造一个描述更改的文档。

![](img/7603e49e2ca8e75f35e37ac602741823_51.png)

![](img/7603e49e2ca8e75f35e37ac602741823_52.png)

```python
# 定义要更新的字段
update_data = {
    "$set": {
        "on_campus": False,
        "email": "john.doe@online.edu"
    }
}

# 执行更新
result = students_collection.update_one({"last_name": "Doe"}, update_data)
print(f"匹配到的文档数: {result.matched_count}, 修改的文档数: {result.modified_count}")
```

### 更新多个文档

![](img/7603e49e2ca8e75f35e37ac602741823_54.png)

使用`update_many`函数可以一次性更新所有匹配过滤条件的文档。

![](img/7603e49e2ca8e75f35e37ac602741823_55.png)

![](img/7603e49e2ca8e75f35e37ac602741823_56.png)

```python
# 例如，由于封锁，将所有学生更新为仅在线学习
result = students_collection.update_many({}, {"$set": {"on_campus": False}})
print(f"所有学生已更新为仅在线模式。修改的文档数: {result.modified_count}")
```

![](img/7603e49e2ca8e75f35e37ac602741823_57.png)

## 🗑️ 删除（Delete）操作

与更新和查找类似，删除操作也提供了删除单个或多个文档的函数。

![](img/7603e49e2ca8e75f35e37ac602741823_59.png)

![](img/7603e49e2ca8e75f35e37ac602741823_60.png)

### 删除单个文档

![](img/7603e49e2ca8e75f35e37ac602741823_61.png)

使用`delete_one`函数，其第一个参数是过滤条件，用于找到要删除的文档。

```python
result = students_collection.delete_one({"email": "john.doe@online.edu"})
print(f"删除的文档数量: {result.deleted_count}")
```

### 删除多个文档

![](img/7603e49e2ca8e75f35e37ac602741823_63.png)

![](img/7603e49e2ca8e75f35e37ac602741823_64.png)

使用`delete_many`函数，删除所有匹配给定条件的文档。

![](img/7603e49e2ca8e75f35e37ac602741823_65.png)

```python
result = students_collection.delete_many({"on_campus": False})
print(f"删除的文档数量: {result.deleted_count}")
```

## 📝 总结

在本节课中，我们一起学习了如何使用Python连接和操作MongoDB数据库。

*   **MongoClient**：是从`pymongo`导入的一个类，是与MongoDB交互的主要入口点。
*   **连接与引用**：我们学会了如何建立连接，并获取数据库和集合的引用。
*   **CRUD操作**：
    *   **创建**：可以使用`insert_one`进行单条插入，或使用`insert_many`进行批量插入。
    *   **读取**：使用`find_one`查询单个文档，使用`find`查询多个文档并返回游标，使用`count_documents`进行计数。
    *   **更新**：可以使用`replace_one`替换整个文档，但更推荐使用`update_one`或`update_many`配合`$set`等操作符进行高效的局部更新。
    *   **删除**：使用`delete_one`或`delete_many`根据条件删除文档。

![](img/7603e49e2ca8e75f35e37ac602741823_67.png)

通过掌握这些基本操作，你已经能够使用Python脚本有效地管理MongoDB中的数据了。