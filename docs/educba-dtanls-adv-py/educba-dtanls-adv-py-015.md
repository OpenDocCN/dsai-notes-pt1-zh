# 015：发送与接收数据 🚀

在本节课中，我们将学习如何构建一个能够同时发送和接收数据的网络客户端与服务器。我们将使用Python的`socket`和`threading`模块来实现双向通信。

上一节我们介绍了网络连接的基础，本节中我们来看看如何让程序同时处理发送和接收任务。

## 核心概念与流程

为了实现同时收发数据，我们需要使用多线程。一个线程负责持续接收数据，另一个线程（通常是主线程）负责处理用户输入并发送数据。以下是实现这一功能的核心步骤：

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_1.png)

1.  **客户端**：启动一个后台线程来持续接收服务器发来的消息，同时主线程循环等待用户输入并发送给服务器。
2.  **服务器**：同样启动一个线程来持续接收客户端发来的消息，同时主线程循环等待管理员输入并发送给客户端。

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_2.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_3.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_4.png)

关键点在于，接收数据的操作需要在一个**无限循环**（`while True`）中进行，以确保能持续监听消息。

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_5.png)

## 客户端实现详解

以下是客户端代码的核心结构。

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_7.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_8.png)

首先，我们需要导入必要的模块并建立连接。

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_9.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_11.png)

```python
import socket
from threading import Thread

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_13.png)

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('localhost', 9999))
```

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_15.png)

接下来，我们定义接收数据的函数。这个函数将在单独的线程中运行。

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_17.png)

```python
def receive_data():
    while True:
        data = client.recv(1024).decode()
        print(f"来自服务器的消息：{data}")
```

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_19.png)

然后，我们定义发送数据的函数。这个函数将在主线程中运行。

```python
def send_data():
    while True:
        user_input = input("请输入要发送的消息：")
        client.send(user_input.encode())
```

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_21.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_22.png)

最后，我们启动线程并运行主程序。

```python
# 创建并启动接收线程
t = Thread(target=receive_data)
t.start()

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_24.png)

# 在主线程中运行发送函数
send_data()
```

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_25.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_26.png)

## 服务器端实现详解

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_28.png)

现在，让我们构建服务器端以配合客户端工作。

首先，服务器需要绑定端口并监听连接。

```python
import socket
from threading import Thread

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_30.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_31.png)

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('localhost', 9999))
server.listen()
print("服务器正在监听...")

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_33.png)

conn, addr = server.accept()
print(f"接收到来自 {addr} 的客户端连接")
```

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_35.png)

与客户端类似，服务器也需要定义接收和发送函数。

以下是接收数据的函数：

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_37.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_38.png)

```python
def receive_data(connection):
    while True:
        data = connection.recv(1024).decode()
        print(f"来自客户端的消息：{data}")
```

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_40.png)

以下是发送数据的函数：

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_42.png)

```python
def send_data(connection):
    while True:
        msg = input("服务器请输入消息：")
        connection.send(msg.encode())
```

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_43.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_45.png)

最后，服务器启动线程来处理双向通信。

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_47.png)

```python
# 创建并启动接收线程，传入连接对象 `conn`
recv_thread = Thread(target=receive_data, args=(conn,))
recv_thread.start()

# 在主线程中运行发送函数，传入连接对象 `conn`
send_data(conn)
```

## 运行与调试

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_49.png)

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_50.png)

按照以下步骤运行程序：

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_52.png)

1.  首先运行服务器端代码。控制台将显示“服务器正在监听...”。
2.  接着运行客户端代码。服务器端控制台会显示“接收到来自...的客户端连接”。
3.  此时，在客户端输入消息，服务器端会收到并显示。
4.  在服务器端输入消息，客户端也会收到并显示。

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_54.png)

如果遇到错误，例如“receive takes at least one argument”，请检查函数定义是否正确接收了连接对象参数。添加`print`调试语句可以帮助定位程序执行到了哪一步。

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_56.png)

## 总结

![](img/950b01b82fc0fefb02fba9ebc2e77c0a_58.png)

本节课中我们一起学习了如何构建一个支持全双工通信的Python网络应用。我们利用**多线程**技术，让`receive_data()`函数在后台持续运行，而`send_data()`函数在主线程中处理用户输入，从而实现了客户端与服务器之间的实时双向数据收发。关键在于理解每个功能模块（连接、接收、发送）都需要被正确地放入循环中，并通过线程并发执行。