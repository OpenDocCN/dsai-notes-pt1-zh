# 035：HTTP API基础知识 🌐

![](img/39dbf84a95cbc58453635a4921613d46_0.png)

![](img/39dbf84a95cbc58453635a4921613d46_1.png)

![](img/39dbf84a95cbc58453635a4921613d46_3.png)

![](img/39dbf84a95cbc58453635a4921613d46_4.png)

![](img/39dbf84a95cbc58453635a4921613d46_5.png)

在本节课中，我们将学习HTTP API的基础知识，了解什么是cURL工具，并学习如何使用简单的cURL命令与Cloudant数据库进行交互。

![](img/39dbf84a95cbc58453635a4921613d46_7.png)

![](img/39dbf84a95cbc58453635a4921613d46_8.png)

## 概述

在前面的视频和实验中，你学习了如何使用Cloudant仪表板执行基本的CRUD操作，即如何在Cloudant数据库中创建、读取、更新和删除数据。

![](img/39dbf84a95cbc58453635a4921613d46_10.png)

![](img/39dbf84a95cbc58453635a4921613d46_11.png)

Cloudant仪表板本身是一个基于Web的应用程序，它通过超文本传输协议（HTTP）调用Cloudant的应用编程接口（API）。由于Cloudant数据库拥有HTTP API，它们具备一个显著优势：任何想要从中读取或写入数据的互联网设备都可以通过使用标准HTTP库的请求来访问它们。

## 什么是cURL？ 💻

cURL是一个免费的开源命令行工具，可用于向你的Cloudant API发出HTTP请求。它是客户端URL（cURL）计算机软件项目的一部分，该项目最初于1997年发布。

![](img/39dbf84a95cbc58453635a4921613d46_13.png)

![](img/39dbf84a95cbc58453635a4921613d46_14.png)

cURL可以从命令行直接使用标准URL语法获取和发送数据（包括文件）。对于更重复的任务，你也可以在脚本中使用它。它还通过使用安全套接字层（SSL）证书验证来支持安全的HTTP（HTTPS）请求。

你可以在 `github.com/curl/curl` 访问最新的cURL源代码。

![](img/39dbf84a95cbc58453635a4921613d46_16.png)

![](img/39dbf84a95cbc58453635a4921613d46_17.png)

![](img/39dbf84a95cbc58453635a4921613d46_18.png)

## 基本cURL命令

![](img/39dbf84a95cbc58453635a4921613d46_20.png)

如果你只想从cURL命令行提示符检索一个网页，只需输入单词 `curl`，然后输入网页的URL。

![](img/39dbf84a95cbc58453635a4921613d46_22.png)

![](img/39dbf84a95cbc58453635a4921613d46_23.png)

![](img/39dbf84a95cbc58453635a4921613d46_24.png)

以下示例将检索IBM Cloud主页：
```bash
curl https://cloud.ibm.com
```

![](img/39dbf84a95cbc58453635a4921613d46_25.png)

但为了避免每次连接时都输入你的Cloudant服务URL，你可以创建一个变量。

![](img/39dbf84a95cbc58453635a4921613d46_27.png)

![](img/39dbf84a95cbc58453635a4921613d46_28.png)

## 设置变量和别名

![](img/39dbf84a95cbc58453635a4921613d46_29.png)

![](img/39dbf84a95cbc58453635a4921613d46_30.png)

![](img/39dbf84a95cbc58453635a4921613d46_31.png)

![](img/39dbf84a95cbc58453635a4921613d46_32.png)

以下示例使用 `export` 命令将我们的Cloudant URL保存为一个名为 `URL` 的变量。你需要将用户名、密码和主机部分替换为你自己的Cloudant服务凭据信息。你将在本课结束时的动手实验中进行此操作。
```bash
export URL="https://username:password@hostname.cloudant.com"
```

![](img/39dbf84a95cbc58453635a4921613d46_34.png)

![](img/39dbf84a95cbc58453635a4921613d46_35.png)

你也可以创建一个别名，作为cURL的快捷方式。以下示例将创建一个名为 `acurl` 的别名，并指定了JSON内容类型头。它还使用了一些有用的命令行开关：
*   `-s` 使你的请求静默（即，你不会看到任何进度或错误消息返回）。
*   `-g` 禁用URL全局解析器。
*   `-H` 允许你指定内容类型头。
```bash
alias acurl='curl -sg -H "Content-Type: application/json"'
```

![](img/39dbf84a95cbc58453635a4921613d46_36.png)

![](img/39dbf84a95cbc58453635a4921613d46_37.png)

## 测试连接与操作数据库

![](img/39dbf84a95cbc58453635a4921613d46_39.png)

![](img/39dbf84a95cbc58453635a4921613d46_40.png)

要测试与Cloudant中数据库的连接，你可以使用 `acurl` 别名和 `URL` 变量（变量前必须加美元符号 `$`）。如果成功，你将收到一些包含欢迎消息、版本号、供应商名称和关于你的Cloudant实例功能信息的JSON代码。
```bash
acurl $URL
```

![](img/39dbf84a95cbc58453635a4921613d46_42.png)

![](img/39dbf84a95cbc58453635a4921613d46_43.png)

![](img/39dbf84a95cbc58453635a4921613d46_44.png)

![](img/39dbf84a95cbc58453635a4921613d46_45.png)

要检索所有数据库的列表，再次使用 `acurl` 别名和美元符号 `URL` 变量，但要加上 `_all_dbs` 端点。
```bash
acurl $URL/_all_dbs
```

![](img/39dbf84a95cbc58453635a4921613d46_47.png)

![](img/39dbf84a95cbc58453635a4921613d46_48.png)

作为一个附加选项，如果你的操作系统上安装了Python，你可以在这行命令的末尾添加管道命令 `| python -m json.tool`。这将把你的命令输出发送给Python，以便在终端中获得改进的JSON格式，将JSON数据转换为更易读的格式。
```bash
acurl $URL/_all_dbs | python -m json.tool
```

![](img/39dbf84a95cbc58453635a4921613d46_50.png)

![](img/39dbf84a95cbc58453635a4921613d46_52.png)

还有另一个名为 `jq` 的JSON格式化程序，你可以免费下载和安装，它也能将JSON数据格式化成更易读的形式。指定此格式化程序的管道命令是 `| jq`。
```bash
acurl $URL/_all_dbs | jq
```

![](img/39dbf84a95cbc58453635a4921613d46_54.png)

![](img/39dbf84a95cbc58453635a4921613d46_55.png)

![](img/39dbf84a95cbc58453635a4921613d46_56.png)

如果你想查看单个数据库的详细信息，只需在斜杠后指定数据库的名称。以下示例将检索名为 `training` 的数据库的详细信息。同样，你可以使用两个可选的管道命令之一，将命令输出发送到Python或 `jq`，以提高cURL终端的可读性。
```bash
acurl $URL/training
acurl $URL/training | python -m json.tool
acurl $URL/training | jq
```

要查看数据库中包含的文档，只需在斜杠后指定数据库名称，然后是 `_all_docs` 端点。这将仅检索所有文档的 `_id` 和 `_rev` 值。
```bash
acurl $URL/training/_all_docs
```

![](img/39dbf84a95cbc58453635a4921613d46_58.png)

![](img/39dbf84a95cbc58453635a4921613d46_59.png)

要同时检索文档的正文，你需要在命令末尾指定 `?include_docs=true`，如以下屏幕示例所示。
```bash
acurl $URL/training/_all_docs?include_docs=true
```

要从数据库中检索单个文档，你需要在包含它的数据库名称后指定文档的文档ID。
```bash
acurl $URL/training/document_id
```

## 总结

![](img/39dbf84a95cbc58453635a4921613d46_61.png)

![](img/39dbf84a95cbc58453635a4921613d46_62.png)

本节课中，我们一起学习了以下内容：
*   Cloudant数据库拥有HTTP API，因此可以通过使用标准HTTP库的请求来访问。
*   HTTP被Web浏览器、移动设备、常见编程语言以及像cURL这样的命令行脚本工具使用。
*   cURL是一个免费的开源命令行工具，可以向Cloudant API发出HTTP请求。
*   cURL可以从命令行或脚本中使用标准URL语法获取和发送数据（包括文件）。
*   你应该创建一个变量来访问你的Cloudant服务。
*   你可以使用别名作为cURL的快捷方式。
*   你可以将cURL命令的输出通过管道传递给Python或jq，以在终端中获得改进的JSON格式。