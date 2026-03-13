# 086：REST API HTTP请求 第2部分 🚀

![](img/321dfcc24456f2c479328def63705ddb_0.png)

![](img/321dfcc24456f2c479328def63705ddb_1.png)

在本节课中，我们将学习如何使用Python的Requests库来处理HTTP协议。我们将重点介绍GET请求和POST请求，并通过实际例子展示如何发送请求、处理响应以及理解请求与响应的关键组成部分。

---

## 概述：Python Requests库

![](img/321dfcc24456f2c479328def63705ddb_3.png)

![](img/321dfcc24456f2c479328def63705ddb_4.png)

上一节我们介绍了HTTP协议的基础知识。本节中，我们来看看如何在Python中实际使用HTTP协议，特别是通过一个名为`requests`的流行库。

![](img/321dfcc24456f2c479328def63705ddb_5.png)

![](img/321dfcc24456f2c479328def63705ddb_6.png)

`requests`是Python中一个用于发送HTTP/1.1请求的库，它比标准库中的`httplib`或`urllib`等更易于使用。

以下是导入该库并发送一个简单GET请求的方法：

```python
import requests

r = requests.get('https://www.ibm.com')
```

变量`r`是一个响应对象，它包含了关于此次请求的所有信息，例如请求的状态。

![](img/321dfcc24456f2c479328def63705ddb_8.png)

![](img/321dfcc24456f2c479328def63705ddb_9.png)

---

![](img/321dfcc24456f2c479328def63705ddb_10.png)

![](img/321dfcc24456f2c479328def63705ddb_11.png)

## 解析HTTP响应

![](img/321dfcc24456f2c479328def63705ddb_12.png)

![](img/321dfcc24456f2c479328def63705ddb_13.png)

发送请求后，我们需要理解服务器返回的响应。响应对象提供了多种属性来访问这些信息。

![](img/321dfcc24456f2c479328def63705ddb_14.png)

我们可以查看状态码，状态码`200`通常表示请求成功：

```python
print(r.status_code)  # 输出：200
```

![](img/321dfcc24456f2c479328def63705ddb_16.png)

我们可以查看响应头，它包含了服务器返回的元数据：

```python
print(r.headers)
```

![](img/321dfcc24456f2c479328def63705ddb_18.png)

响应头是一个Python字典。例如，我们可以获取服务器发送响应的日期：

![](img/321dfcc24456f2c479328def63705ddb_19.png)

![](img/321dfcc24456f2c479328def63705ddb_20.png)

```python
print(r.headers['date'])
```

对于GET请求，通常没有请求体，因此`r.request.body`的值为`None`：

![](img/321dfcc24456f2c479328def63705ddb_22.png)

```python
print(r.request.body)  # 输出：None
```

![](img/321dfcc24456f2c479328def63705ddb_23.png)

![](img/321dfcc24456f2c479328def63705ddb_24.png)

如果响应内容是文本或HTML，我们可以使用`.text`属性来查看：

```python
print(r.text[:100])  # 打印前100个字符
```

![](img/321dfcc24456f2c479328def63705ddb_26.png)

![](img/321dfcc24456f2c479328def63705ddb_27.png)

---

![](img/321dfcc24456f2c479328def63705ddb_28.png)

## 使用GET请求发送查询参数

GET请求常用于从服务器检索数据，我们可以在URL中添加查询参数来过滤或指定所需的数据。

![](img/321dfcc24456f2c479328def63705ddb_30.png)

![](img/321dfcc24456f2c479328def63705ddb_31.png)

查询字符串是URL的一部分，以问号`?`开始，后面跟着一系列`参数=值`对，每对之间用`&`符号分隔。

![](img/321dfcc24456f2c479328def63705ddb_32.png)

例如，以下URL包含两个参数：
`https://httpbin.org/get?name=Joseph&id=123`

在Python中，我们可以使用字典来构建查询参数，并将其传递给`get`方法的`params`参数：

```python
import requests

![](img/321dfcc24456f2c479328def63705ddb_34.png)

![](img/321dfcc24456f2c479328def63705ddb_35.png)

![](img/321dfcc24456f2c479328def63705ddb_36.png)

url_get = 'https://httpbin.org/get'
payload = {'name': 'Joseph', 'id': '123'}

![](img/321dfcc24456f2c479328def63705ddb_38.png)

r = requests.get(url_get, params=payload)
```

我们可以打印出最终构造的URL，以确认参数已正确添加：

```python
print(r.url)  # 输出：https://httpbin.org/get?name=Joseph&id=123
```

![](img/321dfcc24456f2c479328def63705ddb_40.png)

![](img/321dfcc24456f2c479328def63705ddb_41.png)

![](img/321dfcc24456f2c479328def63705ddb_42.png)

由于信息是通过URL发送的，GET请求的请求体仍然是`None`。我们可以检查状态码并查看JSON格式的响应内容：

![](img/321dfcc24456f2c479328def63705ddb_43.png)

```python
print(r.status_code)  # 输出：200
print(r.json())  # 将响应内容解析为Python字典
```

在返回的JSON字典中，`args`键对应的值就是我们发送的查询参数字典。

---

![](img/321dfcc24456f2c479328def63705ddb_45.png)

![](img/321dfcc24456f2c479328def63705ddb_46.png)

## 使用POST请求发送数据

与GET请求不同，POST请求通常用于向服务器提交数据，数据被放置在请求体中，而不是URL里。

![](img/321dfcc24456f2c479328def63705ddb_48.png)

![](img/321dfcc24456f2c479328def63705ddb_49.png)

为了发送POST请求，我们需要将端点路径改为`/post`，并将数据字典传递给`post`方法的`data`参数。

![](img/321dfcc24456f2c479328def63705ddb_50.png)

以下是发送POST请求的示例：

```python
import requests

url_post = 'https://httpbin.org/post'
payload = {'name': 'Joseph', 'id': '123'}

r_post = requests.post(url_post, data=payload)
```

---

## 比较GET与POST请求

![](img/321dfcc24456f2c479328def63705ddb_52.png)

理解GET和POST请求的区别至关重要。让我们比较一下它们的URL和请求体。

![](img/321dfcc24456f2c479328def63705ddb_53.png)

![](img/321dfcc24456f2c479328def63705ddb_54.png)

比较URL：POST请求的URL中不包含查询参数。

![](img/321dfcc24456f2c479328def63705ddb_55.png)

![](img/321dfcc24456f2c479328def63705ddb_56.png)

![](img/321dfcc24456f2c479328def63705ddb_57.png)

```python
print("GET请求URL:", r.url)      # 包含参数
print("POST请求URL:", r_post.url) # 不包含参数
```

![](img/321dfcc24456f2c479328def63705ddb_58.png)

![](img/321dfcc24456f2c479328def63705ddb_59.png)

比较请求体：只有POST请求拥有非空的请求体。

![](img/321dfcc24456f2c479328def63705ddb_60.png)

![](img/321dfcc24456f2c479328def63705ddb_61.png)

```python
print("GET请求体:", r.request.body)      # 输出：None
print("POST请求体:", r_post.request.body) # 输出：name=Joseph&id=123
```

在POST请求的JSON响应中，我们可以通过`form`键来获取我们发送的数据：

```python
print(r_post.json()['form'])  # 输出：{'name': 'Joseph', 'id': '123'}
```

![](img/321dfcc24456f2c479328def63705ddb_63.png)

![](img/321dfcc24456f2c479328def63705ddb_64.png)

---

![](img/321dfcc24456f2c479328def63705ddb_65.png)

![](img/321dfcc24456f2c479328def63705ddb_66.png)

## 总结

![](img/321dfcc24456f2c479328def63705ddb_67.png)

![](img/321dfcc24456f2c479328def63705ddb_68.png)

本节课中我们一起学习了如何使用Python的`requests`库进行HTTP通信。我们掌握了如何发送基本的GET请求来获取网页内容，以及如何通过查询字符串向GET请求添加参数。我们还学习了如何使用POST请求向服务器提交数据，并理解了GET与POST在数据传递位置（URL vs 请求体）上的核心区别。这些是构建与Web API交互的应用程序的基础技能。