# 014：与聊天机器人协作 🤖

![](img/663685fc9be55e006de7cce1f6541c52_0.png)

![](img/663685fc9be55e006de7cce1f6541c52_2.png)

![](img/663685fc9be55e006de7cce1f6541c52_3.png)

在本节课中，我们将学习如何使用Python的`socket`和`threading`模块构建一个简单的聊天机器人客户端与服务器。我们将通过创建两个独立的脚本来实现网络通信，并利用多线程技术实现同时发送和接收消息。

![](img/663685fc9be55e006de7cce1f6541c52_5.png)

![](img/663685fc9be55e006de7cce1f6541c52_7.png)

---

## 创建项目结构

首先，我们需要为聊天机器人应用创建一个新的项目包。我们将创建两个主要的Python文件：一个用于客户端，另一个用于服务器。

![](img/663685fc9be55e006de7cce1f6541c52_9.png)

![](img/663685fc9be55e006de7cce1f6541c52_10.png)

![](img/663685fc9be55e006de7cce1f6541c52_11.png)

![](img/663685fc9be55e006de7cce1f6541c52_12.png)

以下是创建项目结构的步骤：

1.  创建一个名为`chatbot`的新包。
2.  在该包内，创建两个Python文件：
    *   `client.py`：用于实现聊天客户端。
    *   `server.py`：用于实现聊天服务器。

![](img/663685fc9be55e006de7cce1f6541c52_14.png)

---

![](img/663685fc9be55e006de7cce1f6541c52_15.png)

## 构建客户端

![](img/663685fc9be55e006de7cce1f6541c52_17.png)

![](img/663685fc9be55e006de7cce1f6541c52_18.png)

上一节我们创建了项目结构，本节中我们来看看如何构建客户端。

客户端的主要任务是连接到服务器，并能够同时发送和接收消息。这需要用到`socket`模块来建立网络连接，以及`threading`模块来并行处理收发操作。

![](img/663685fc9be55e006de7cce1f6541c52_20.png)

以下是客户端`client.py`的核心实现步骤：

1.  **导入模块**：导入`socket`和`threading`模块。
2.  **创建套接字**：使用`socket.socket()`创建一个客户端套接字对象。
3.  **连接服务器**：使用`client_socket.connect()`方法连接到运行在本地主机（`localhost`）端口`2000`上的服务器。
4.  **定义收发函数**：
    *   `receive_data()`：一个循环，持续从服务器接收数据（`client_socket.recv(1024)`），解码后打印到控制台。
    *   `send_data()`：一个循环，持续从用户输入获取消息，编码后发送给服务器（`client_socket.sendall()`）。
5.  **启动线程**：创建两个线程，分别以`receive_data`和`send_data`函数为目标，并启动它们，实现同时收发。

![](img/663685fc9be55e006de7cce1f6541c52_22.png)

![](img/663685fc9be55e006de7cce1f6541c52_23.png)

**核心代码示例**：
```python
import socket
import threading

![](img/663685fc9be55e006de7cce1f6541c52_24.png)

def receive_data(client_socket):
    while True:
        data = client_socket.recv(1024)
        print(f"来自服务器的消息: {data.decode()}")

def send_data(client_socket):
    while True:
        user_input = input("请输入要发送的消息: ")
        client_socket.sendall(user_input.encode())

# 创建并连接套接字
client_socket = socket.socket()
client_socket.connect(('localhost', 2000))

# 创建并启动线程
receive_thread = threading.Thread(target=receive_data, args=(client_socket,))
send_thread = threading.Thread(target=send_data, args=(client_socket,))
receive_thread.start()
send_thread.start()
```

---

![](img/663685fc9be55e006de7cce1f6541c52_26.png)

## 构建服务器

了解了客户端的构建后，本节我们来构建服务器端。

服务器的任务是绑定到一个端口，监听客户端的连接请求，并为每个连接处理消息的收发。与客户端类似，服务器也需要使用多线程来处理并发的通信。

![](img/663685fc9be55e006de7cce1f6541c52_28.png)

以下是服务器`server.py`的核心实现步骤：

![](img/663685fc9be55e006de7cce1f6541c52_29.png)

1.  **导入模块**：导入`socket`和`threading`模块。
2.  **创建并绑定套接字**：使用`socket.socket()`创建服务器套接字，并使用`server_socket.bind(('', port))`将其绑定到指定的端口（例如2000）。
3.  **监听连接**：调用`server_socket.listen()`开始监听传入的连接。
4.  **接受连接**：在循环中调用`server_socket.accept()`接受客户端连接。该方法返回一个新的连接对象（`conn`）和客户端地址。
5.  **为连接创建处理线程**：对于每个接受的连接，创建两个线程：
    *   一个线程用于从该连接接收数据（`conn.recv()`），解码后打印或处理。
    *   另一个线程用于向该连接发送数据（`conn.sendall()`），数据可以来自服务器控制台输入或其他逻辑。
6.  **启动线程**：启动这些线程，使服务器能够与该客户端进行全双工通信。

**核心代码示例**：
```python
import socket
import threading

def handle_client(conn, addr):
    print(f"新连接来自: {addr}")
    # 为每个客户端连接创建收发线程（逻辑与客户端类似）
    # receive_thread = threading.Thread(target=receive_from_client, args=(conn,))
    # send_thread = threading.Thread(target=send_to_client, args=(conn,))
    # receive_thread.start()
    # send_thread.start()

# 主服务器循环
server_socket = socket.socket()
port = 2000
server_socket.bind(('', port))
server_socket.listen()
print(f"服务器已在端口 {port} 启动，等待连接...")

while True:
    conn, addr = server_socket.accept()
    client_thread = threading.Thread(target=handle_client, args=(conn, addr))
    client_thread.start()
```

---

## 总结

![](img/663685fc9be55e006de7cce1f6541c52_31.png)

![](img/663685fc9be55e006de7cce1f6541c52_32.png)

本节课中我们一起学习了如何使用Python构建一个基础的聊天机器人通信框架。我们掌握了以下核心技能：

![](img/663685fc9be55e006de7cce1f6541c52_34.png)

1.  使用**`socket`模块**创建网络套接字，实现客户端与服务器之间的TCP连接。
2.  理解**客户端-服务器模型**，包括客户端的`connect`操作和服务器端的`bind`、`listen`、`accept`操作。
3.  运用**`threading`模块**创建多线程，使得程序能够同时执行发送和接收消息的任务，实现了实时双向通信。
4.  掌握了网络数据传输中**编码（`encode`）与解码（`decode`）** 的必要性，以确保文本信息能正确通过字节流传输。

![](img/663685fc9be55e006de7cce1f6541c52_36.png)

通过将上述概念组合在`client.py`和`server.py`两个脚本中，我们搭建了一个可以同时处理多个消息流的简单聊天系统原型。这是开发更复杂网络应用（如在线聊天室或分布式数据处理节点）的重要基础。