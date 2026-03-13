# 012：管理对数据库及其对象的访问 🔐

![](img/af6c5e57d95c1f3feef9b884bffe59ae_0.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_1.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_3.png)

在本节课中，我们将要学习如何管理用户对数据库及其内部对象（如表、存储过程等）的访问权限。具体来说，我们将了解如何授予、撤销和拒绝访问权限。

用户通过身份验证登录数据库后，需要获得相应的权限或特权才能访问数据库中的对象和数据。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_5.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_6.png)

权限可以授予给单个用户，也可以授予给组或角色。一个用户的最终权限，是其个人权限与其所属组或角色的权限的组合。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_8.png)

在本视频中，你将看到通用的SQL语句示例。请注意，你所使用的RDBMS（关系数据库管理系统）的语法可能与示例略有不同，并且很可能也提供了图形界面选项来执行这些任务。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_9.png)

## 授予数据库连接权限

![](img/af6c5e57d95c1f3feef9b884bffe59ae_11.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_12.png)

上一节我们介绍了权限的基本概念，本节中我们来看看如何授予用户连接数据库的权限。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_13.png)

在一些系统中，用户是在数据库外部（例如操作系统或通过外部插件）进行身份验证的。对于这些用户，你可能还需要授予他们访问特定数据库的权限。你可以使用SQL的 `GRANT CONNECT` 命令来实现。

以下是授予组或角色连接权限的示例：

![](img/af6c5e57d95c1f3feef9b884bffe59ae_15.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_16.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_17.png)

```sql
GRANT CONNECT ON DATABASE MyDB TO GROUP sales_team;
```

要授予单个用户（而非组或角色）连接权限，只需将组名替换为用户名即可。

```sql
GRANT CONNECT ON DATABASE MyDB TO USER john_doe;
```

![](img/af6c5e57d95c1f3feef9b884bffe59ae_19.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_20.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_21.png)

如果你的RDBMS提供了访客（guest）或公共（public）账户，你可能会发现默认情况下它们拥有连接所有数据库的权限。在这种情况下，最佳实践是撤销该权限，以确保只有被明确授予权限的用户才能访问你的数据。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_22.png)

## 授予对数据库对象的权限

![](img/af6c5e57d95c1f3feef9b884bffe59ae_24.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_25.png)

现在我们已经了解了如何连接数据库，接下来看看如何控制对数据库内部对象的操作。

你可以使用SQL的 `GRANT` 命令来授予对数据库中表的权限。以下示例授予 `sales_team` 组在 `MyDB` 数据库的 `MyTable` 表上的 `SELECT` 权限。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_27.png)

```sql
GRANT SELECT ON MyDB.MyTable TO GROUP sales_team;
```

![](img/af6c5e57d95c1f3feef9b884bffe59ae_28.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_29.png)

你可以使用类似的语句，通过授予用户在表上的相关权限，来允许他们插入、更新和删除数据。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_31.png)

```sql
GRANT INSERT, UPDATE, DELETE ON MyDB.MyTable TO GROUP sales_team;
```

![](img/af6c5e57d95c1f3feef9b884bffe59ae_32.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_33.png)

要将这些权限授予单个用户，只需将组名 `sales_team` 替换为该用户的用户名。

你还可以使用 `GRANT` 语句授予在数据库中创建对象的权限。以下示例展示了如何启用一个组或角色来创建表。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_35.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_36.png)

```sql
GRANT CREATE TABLE ON DATABASE MyDB TO ROLE developer_role;
```

同样，你可以使用类似的语句来允许用户创建表或存储过程等对象。

对于存储过程或函数，你可以允许用户或组查看其代码。这意味着他们可以查看该函数或过程的定义，但无法运行或更改它。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_38.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_39.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_40.png)

```sql
GRANT VIEW DEFINITION ON PROCEDURE MyDB.MyProc TO USER analyst_jane;
```

![](img/af6c5e57d95c1f3feef9b884bffe59ae_41.png)

要运行代码，用户需要 `EXECUTE` 权限；要更改过程的定义，用户需要 `ALTER` 权限。

```sql
GRANT EXECUTE ON PROCEDURE MyDB.MyProc TO USER analyst_jane;
GRANT ALTER ON PROCEDURE MyDB.MyProc TO ROLE admin_role;
```

![](img/af6c5e57d95c1f3feef9b884bffe59ae_43.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_44.png)

## 撤销与拒绝权限

![](img/af6c5e57d95c1f3feef9b884bffe59ae_45.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_46.png)

除了授予权限，有时你可能需要移除它们。你可以使用 `REVOKE` 语句来移除已授予的权限，大多数RDBMS也会在用户界面中提供撤销功能。

以下是撤销权限的示例：

```sql
REVOKE SELECT ON MyDB.MyTable FROM GROUP sales_team;
```

![](img/af6c5e57d95c1f3feef9b884bffe59ae_48.png)

然而，由于个人的权限是其所属所有组或角色权限的组合，`sales_team` 角色的成员可能仍能通过其所属的另一个角色访问此表。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_49.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_50.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_51.png)

如果你想确保用户对某个对象或操作没有权限，可以使用 `DENY` 语句来覆盖之前授予的任何该权限。`DENY` 的优先级通常高于 `GRANT`。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_52.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_53.png)

以下是拒绝权限的示例：

![](img/af6c5e57d95c1f3feef9b884bffe59ae_55.png)

```sql
DENY SELECT ON MyDB.MyTable TO USER restricted_user;
```

![](img/af6c5e57d95c1f3feef9b884bffe59ae_56.png)

## 总结

本节课中我们一起学习了数据库访问权限管理的核心操作。

*   你学会了将权限授予用户、组或角色。
*   权限控制着对数据库及其内部对象的访问。
*   从 `SELECT`、`INSERT`、`UPDATE`、`DELETE` 到 `CREATE`、`ALTER`、`DROP` 等一系列针对不同类型对象的权限，允许对用户、组和角色的访问进行精细调整。
*   你可以撤销先前授予的权限。
*   你可以通过拒绝权限来覆盖现有的已授予权限。

![](img/af6c5e57d95c1f3feef9b884bffe59ae_58.png)

![](img/af6c5e57d95c1f3feef9b884bffe59ae_59.png)

通过合理运用授予、撤销和拒绝这三种操作，你可以构建一个安全、可控的数据库访问环境。