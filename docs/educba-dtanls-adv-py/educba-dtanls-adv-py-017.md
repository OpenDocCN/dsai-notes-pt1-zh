# 017：创建与导入客户端套接字 🚀

在本节课中，我们将要学习如何创建一个多客户端聊天应用。我们将从构建服务器和客户端的基础套接字开始，并引入多线程来处理多个客户端的并发连接。核心概念是让服务器能够接受多个客户端连接，并将一个客户端发送的消息广播给所有其他客户端。

## 创建聊天应用项目

首先，我们创建一个新的Python项目来构建聊天应用。我们将创建两个主要的Python文件：一个用于服务器，一个用于客户端。

以下是创建项目结构的步骤：

1.  创建一个新的Python包或项目文件夹，命名为 `chat_app`。
2.  在该文件夹内，创建两个Python文件：`server.py` 和 `client.py`。

## 导入套接字模块

上一节我们介绍了项目结构，本节中我们来看看如何导入必要的模块。服务器和客户端都需要使用Python的 `socket` 模块来建立网络连接。

在 `server.py` 和 `client.py` 文件的开头，我们都需要导入 `socket` 模块。

**代码示例：导入socket**
```python
import socket
```

## 创建服务器套接字

导入模块后，我们首先在服务器端创建套接字。服务器套接字负责监听特定端口，等待客户端的连接请求。

![](img/8ce0d7193891fcb0b884d59f45de3f6d_1.png)

以下是创建和配置服务器套接字的步骤：

1.  使用 `socket.socket()` 创建一个新的套接字对象。
2.  使用 `bind()` 方法将套接字绑定到本地主机的特定端口（例如 2000）。
3.  使用 `listen()` 方法开始监听传入的连接。

**代码示例：创建服务器套接字**
```python
# 在 server.py 中
server_socket = socket.socket()
server_socket.bind(('localhost', 2000))
server_socket.listen()
print("服务器已启动，正在监听端口 2000...")
```

![](img/8ce0d7193891fcb0b884d59f45de3f6d_3.png)

## 创建客户端套接字

现在，让我们转向客户端。客户端套接字用于主动连接到服务器。

以下是创建和连接客户端套接字的步骤：

1.  同样使用 `socket.socket()` 创建一个客户端套接字对象。
2.  使用 `connect()` 方法连接到服务器的地址和端口。

**代码示例：创建并连接客户端套接字**
```python
# 在 client.py 中
client_socket = socket.socket()
client_socket.connect(('localhost', 2000))
print("已连接到服务器。")
```

## 处理多客户端连接

上一节我们介绍了基本的客户端连接，但我们的目标是支持多个客户端。服务器不能只连接一个客户端，它需要持续接受新的连接。为此，我们需要一个循环来不断接受客户端。

以下是服务器端接受多客户端连接的逻辑：

1.  在一个无限循环中，使用 `accept()` 方法等待并接受新的客户端连接。该方法会返回一个新的连接对象（`conn`）和客户端的地址（`addr`）。
2.  每当有客户端连接时，打印连接信息。

**代码示例：接受多客户端连接**
```python
# 在 server.py 的 listen() 之后
clients = {}  # 创建一个字典来存储所有客户端连接

while True:
    conn, addr = server_socket.accept()
    print(f"收到来自 {addr} 的连接")
    # 接下来将处理这个连接（例如，接收客户端名称并启动线程）
```

## 客户端发送名称

当客户端成功连接到服务器后，它需要做的第一件事是向服务器发送自己的名称，以便服务器识别。

以下是客户端发送名称的步骤：

1.  提示用户输入其名称。
2.  将名称字符串编码为字节。
3.  通过套接字将编码后的名称发送给服务器。

**代码示例：客户端发送名称**
```python
# 在 client.py 的 connect() 之后
name = input("请输入您的名称：")
client_socket.send(name.encode())
```

## 服务器接收并存储客户端信息

服务器在接受连接后，需要立即接收客户端发送过来的名称，并将该客户端的信息（名称和连接对象）存储起来，以便后续广播消息。

以下是服务器处理新客户端连接的步骤：

1.  从新建立的连接（`conn`）中接收数据（客户端发送的名称）。
2.  将接收到的字节数据解码为字符串。
3.  将这个客户端的信息（名称和连接对象）存储在一个字典中。

**代码示例：服务器接收并存储客户端**
```python
# 在 server.py 的 accept() 循环内
    data = conn.recv(1024)  # 接收最多1024字节的数据
    client_name = data.decode()
    clients[client_name] = conn  # 将客户端名称和连接对象存入字典
    print(f"客户端 {client_name} 已加入。")
```

## 为每个客户端创建线程

为了能让所有客户端同时进行通信，服务器需要为每个新连接的客户端创建一个独立的线程。这样，每个客户端的消息收发操作都不会阻塞其他客户端。

以下是创建线程的步骤：

1.  从 `threading` 模块导入 `Thread` 类。
2.  定义一个函数（例如 `client_task`），用于处理单个客户端的消息接收和广播。
3.  每当有新客户端连接时，就创建一个新的 `Thread` 对象，将 `client_task` 函数设为目标，并传入客户端名称、地址和连接对象作为参数。
4.  启动线程。

**代码示例：导入线程并创建客户端线程**
```python
# 在 server.py 文件开头导入
from threading import Thread

# 定义客户端处理函数
def client_task(name, addr, conn):
    # 这里将实现消息接收和广播逻辑
    pass

# 在 accept() 循环内，接收名称后
    # ... 接收 client_name ...
    thread = Thread(target=client_task, args=(client_name, addr, conn))
    thread.start()
```

## 实现消息广播逻辑

最后，我们需要在 `client_task` 函数中实现核心的聊天逻辑：持续接收来自该客户端的消息，并将每条消息转发（广播）给字典中存储的所有其他客户端。

以下是 `client_task` 函数的基本逻辑：

1.  使用一个 `while` 循环持续从该客户端的连接（`conn`）中接收消息。
2.  将接收到的消息解码。
3.  遍历 `clients` 字典，将消息发送给除发送者本人以外的每一个客户端连接。

**代码示例：客户端任务函数（广播逻辑）**
```python
def client_task(name, addr, conn):
    while True:
        try:
            data = conn.recv(1024)
            if not data:
                break  # 连接已关闭
            message = data.decode()
            # 广播给其他所有客户端
            for client_name, client_conn in clients.items():
                if client_name != name:  # 不发送给自己
                    try:
                        broadcast_msg = f"{name} 说：{message}"
                        client_conn.send(broadcast_msg.encode())
                    except:
                        pass  # 如果某个客户端连接失败，跳过
        except:
            break
    # 客户端断开连接后的清理工作
    conn.close()
    if name in clients:
        del clients[name]
    print(f"客户端 {name} 已断开连接。")
```

![](img/8ce0d7193891fcb0b884d59f45de3f6d_5.png)

本节课中我们一起学习了如何构建一个多客户端聊天应用的基础框架。我们从创建服务器和客户端套接字开始，实现了服务器对多客户端连接的接受与处理。通过为每个客户端创建独立的线程，我们确保了聊天的并发性。核心的广播逻辑使得一个客户端发送的消息能够被所有其他客户端接收。这为构建更复杂的实时通信应用奠定了坚实的基础。