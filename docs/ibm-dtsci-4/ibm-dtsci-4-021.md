# 021：REST API与HTTP请求（下）🔗

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_0.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_1.png)

在本节课中，我们将学习如何使用Python的Requests库处理HTTP协议。我们将重点介绍GET和POST请求，并通过实际示例展示如何发送请求、处理响应以及理解请求与响应的关键组成部分。



## 概述：Python Requests库 🌐

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_3.png)

上一节我们介绍了REST API的基本概念。本节中，我们来看看如何使用Python的Requests库来实际执行HTTP请求。

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_4.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_5.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_6.png)

Requests是Python中一个用于发送HTTP/1.1请求的流行库。它比Python内置的`http.client`或`urllib`等库更易于使用。



## 导入库与发送GET请求

首先，我们需要导入Requests库。以下代码展示了如何导入库并向IBM官网发送一个简单的GET请求。

```python
import requests

r = requests.get('https://www.ibm.com')
```

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_8.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_9.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_10.png)

变量`r`是一个响应对象，它包含了关于此次请求的所有信息，例如状态码。



![](img/12e1d1f0a25977ea3013e2b2d8ba3645_11.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_12.png)

## 检查响应状态与头部

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_13.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_14.png)

我们可以通过响应对象的属性来检查请求的状态和详细信息。以下是检查状态码和响应头的方法。

```python
# 打印状态码，200表示成功
print(r.status_code)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_16.png)

# 打印响应头部（返回一个字典）
print(r.headers)

# 获取特定的头部信息，例如日期
print(r.headers['date'])

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_18.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_19.png)

# 获取内容类型
print(r.headers['Content-Type'])

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_20.png)

# 检查响应编码
print(r.encoding)
```



![](img/12e1d1f0a25977ea3013e2b2d8ba3645_22.png)

## 查看响应内容

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_23.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_24.png)

根据`Content-Type`，我们可以使用不同的属性来查看响应的主体内容。对于HTML内容，我们使用`.text`属性。

```python
# 显示响应体中的HTML内容（前100个字符）
print(r.text[0:100])
```



![](img/12e1d1f0a25977ea3013e2b2d8ba3645_26.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_27.png)

## 使用GET请求查询参数

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_28.png)

GET请求常用于从API检索数据，我们可以通过查询字符串（Query String）向服务器发送参数。在实验中，我们将使用`httpbin.org`这个服务进行演示。

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_30.png)

查询字符串是URL的一部分，以问号`?`开始，后面跟着一系列`参数=值`对，各对之间用`&`符号连接。

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_31.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_32.png)

以下是构建带参数的GET请求的步骤：

1.  定义基础URL和端点（例如 `/get`）。
2.  创建一个字典作为载荷（payload），其中键是参数名，值是参数值。
3.  将这个字典传递给`requests.get()`函数的`params`参数。

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_34.png)

```python
# 定义基础URL
url_get = 'http://httpbin.org/get'

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_35.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_36.png)

# 创建查询参数字典
payload = {'name': 'Joseph', 'ID': '123'}

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_38.png)

# 发送带参数的GET请求
r = requests.get(url_get, params=payload)
```

发送请求后，我们可以检查URL、状态码和响应内容。

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_40.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_41.png)

```python
# 打印最终生成的URL，可以看到参数已被附加
print(r.url)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_42.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_43.png)

# GET请求没有请求体，所以此项为None
print(r.request.body)

# 打印状态码
print(r.status_code)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_45.png)

# 查看响应文本
print(r.text)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_46.png)

# 由于响应是JSON格式，我们可以将其解析为Python字典
print(r.json())
# 在返回的字典中，'args'键下包含了我们发送的查询参数
```



![](img/12e1d1f0a25977ea3013e2b2d8ba3645_48.png)

## 发送POST请求

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_49.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_50.png)

与GET请求不同，POST请求将数据放在请求体中发送，而不是URL里。这更适合发送敏感或大量数据。

要发送POST请求，我们需要：
1.  将端点改为 `/post`。
2.  使用`requests.post()`函数。
3.  将包含数据的字典传递给`data`参数。

```python
# 定义POST请求的URL
url_post = 'http://httpbin.org/post'

# 创建要发送的数据字典
payload_post = {'name': 'Joseph', 'ID': '123'}

# 发送POST请求
r_post = requests.post(url_post, data=payload_post)
```



## 比较GET与POST请求

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_52.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_53.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_54.png)

我们可以通过对比来清晰地理解GET和POST请求的区别。

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_55.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_56.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_57.png)

以下是GET与POST请求的关键区别：

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_58.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_59.png)

*   **URL**：GET请求的参数显示在URL中；POST请求的URL是干净的，不包含参数。
*   **请求体**：GET请求通常没有请求体（`None`）；POST请求的数据包含在请求体中。
*   **响应内容**：在`httpbin.org`的响应中，GET请求的参数在`args`键下，而POST请求发送的数据在`form`键下。

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_60.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_61.png)

```python
# 比较URL
print("GET URL:", r.url)      # 包含参数
print("POST URL:", r_post.url) # 不包含参数

# 比较请求体
print("GET 请求体:", r.request.body)   # 为 None
print("POST 请求体:", r_post.request.body) # 包含数据（字节形式）

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_63.png)

# 从POST响应中获取我们发送的数据
print(r_post.json()['form'])
```



![](img/12e1d1f0a25977ea3013e2b2d8ba3645_64.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_65.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_66.png)

## 总结 🎯

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_67.png)

![](img/12e1d1f0a25977ea3013e2b2d8ba3645_68.png)

本节课我们一起学习了如何使用Python的Requests库进行HTTP通信。我们掌握了如何发送GET和POST请求，如何添加查询参数，以及如何查看和分析响应对象的状态码、头部和内容。理解GET与POST在数据传输方式（URL vs. 请求体）上的根本区别，是有效使用Web API的关键。