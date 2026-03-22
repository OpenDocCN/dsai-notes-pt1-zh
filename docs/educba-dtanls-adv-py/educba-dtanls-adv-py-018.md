# 018：为客户创建消息 📨

在本节课中，我们将学习如何为聊天室中的每个客户端创建和广播消息。我们将重点讲解服务器端如何接收来自一个客户端的消息，并将其转发给其他所有连接的客户端，同时避免将消息发回给发送者本身。

![](img/19c5c57765adcd8d4dd74ebecaa99038_1.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_2.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_3.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_4.png)

---

![](img/19c5c57765adcd8d4dd74ebecaa99038_5.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_6.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_7.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_8.png)

上一节我们介绍了如何建立客户端连接并存储其信息。本节中我们来看看如何实现消息的接收与广播。

![](img/19c5c57765adcd8d4dd74ebecaa99038_10.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_12.png)

首先，服务器需要遍历所有已连接的客户端，以便将消息发送给它们。每个客户端的信息都存储在名为 `clients` 的字典中，其连接对象可以通过键（客户端名）来访问。

![](img/19c5c57765adcd8d4dd74ebecaa99038_14.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_15.png)

以下是实现消息广播的核心循环逻辑：

![](img/19c5c57765adcd8d4dd74ebecaa99038_17.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_18.png)

```python
for client in clients:
    # 获取该客户端对应的连接列表
    connection_list = clients[client]
    # 向列表中的每个连接发送消息
    for connection in connection_list:
        connection.send(message.encode())
```

![](img/19c5c57765adcd8d4dd74ebecaa99038_20.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_21.png)

然而，我们发送的不能仅仅是原始数据。为了让其他客户端知道消息来自谁，我们需要构建一个完整的消息字符串，其中包含发送者的名称。

![](img/19c5c57765adcd8d4dd74ebecaa99038_22.png)

构建消息的步骤如下：

![](img/19c5c57765adcd8d4dd74ebecaa99038_24.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_25.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_26.png)

1.  获取当前发送消息的客户端名称。
2.  将客户端名称与接收到的消息内容组合起来，中间用冒号和空格分隔。
3.  注意，从网络接收到的数据是字节格式，需要先解码为字符串才能进行拼接。

![](img/19c5c57765adcd8d4dd74ebecaa99038_27.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_29.png)

以下是构建消息的代码示例：

![](img/19c5c57765adcd8d4dd74ebecaa99038_31.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_32.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_33.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_35.png)

```python
# 假设 `client_name` 是发送者名称，`raw_data` 是接收到的字节数据
decoded_message = raw_data.decode('utf-8')
full_message = f"{client_name}: {decoded_message}"
# 现在将完整消息编码回字节并发送
encoded_message = full_message.encode('utf-8')
```

![](img/19c5c57765adcd8d4dd74ebecaa99038_37.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_39.png)

---

![](img/19c5c57765adcd8d4dd74ebecaa99038_41.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_43.png)

现在，让我们将注意力转向客户端。客户端需要同时处理两件事：接收来自服务器的消息，以及发送用户输入的消息。这需要通过多线程来实现。

![](img/19c5c57765adcd8d4dd74ebecaa99038_45.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_47.png)

以下是客户端需要实现的两个核心函数：

![](img/19c5c57765adcd8d4dd74ebecaa99038_49.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_51.png)

*   **`receive_data` 函数**：在一个循环中持续从服务器套接字接收消息，解码后打印出来。
*   **`send_data` 函数**：在另一个循环中获取用户输入，编码后通过套接字发送给服务器。

![](img/19c5c57765adcd8d4dd74ebecaa99038_53.png)

为了让这两个函数能同时运行，我们使用线程。主线程用于发送消息，而新创建的线程则专门用于接收消息。

客户端启动线程的代码如下：

![](img/19c5c57765adcd8d4dd74ebecaa99038_55.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_56.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_57.png)

```python
from threading import Thread

![](img/19c5c57765adcd8d4dd74ebecaa99038_59.png)

# 创建并启动接收消息的线程
receive_thread = Thread(target=receive_data)
receive_thread.start()

![](img/19c5c57765adcd8d4dd74ebecaa99038_61.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_62.png)

# 主线程继续执行发送消息的函数
send_data()
```

![](img/19c5c57765adcd8d4dd74ebecaa99038_64.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_66.png)

---

![](img/19c5c57765adcd8d4dd74ebecaa99038_68.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_70.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_71.png)

我们完成了初步的代码，现在来运行测试一下。启动服务器和多个客户端，并尝试发送消息。

测试发现了一个问题：每个客户端都收到了自己发送的消息。这是因为我们的广播逻辑是发送给 `clients` 字典中的每一个客户端，没有排除消息的发送者。

![](img/19c5c57765adcd8d4dd74ebecaa99038_73.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_74.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_75.png)

为了解决这个问题，我们需要在服务器端的广播循环中添加一个条件判断：只有当遍历到的客户端名称与当前发送消息的客户端名称**不同**时，才向其发送消息。

![](img/19c5c57765adcd8d4dd74ebecaa99038_77.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_78.png)

修改后的广播逻辑如下：

```python
for client in clients:
    if client != sending_client_name:  # 排除发送者自己
        connection_list = clients[client]
        for connection in connection_list:
            connection.send(message.encode())
```

![](img/19c5c57765adcd8d4dd74ebecaa99038_80.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_81.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_82.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_83.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_85.png)

---

![](img/19c5c57765adcd8d4dd74ebecaa99038_87.png)

![](img/19c5c57765adcd8d4dd74ebecaa99038_88.png)

本节课中我们一起学习了如何构建一个简单的多人聊天室的核心消息系统。我们掌握了在服务器端接收、格式化并广播消息的方法，在客户端使用多线程来同时处理消息的接收与发送，并通过条件判断解决了消息回传的问题，确保了聊天信息只广播给其他参与者。