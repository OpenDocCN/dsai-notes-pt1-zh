# 013：Python多线程工具包 🧵

![](img/2e359917ca32b442a304f3b180fb0a47_0.png)

![](img/2e359917ca32b442a304f3b180fb0a47_2.png)

在本节课中，我们将要学习如何使用Python的`threading`模块来创建和管理多线程。多线程允许程序同时执行多个任务，这对于构建需要并发处理的应用（如聊天服务器）至关重要。

![](img/2e359917ca32b442a304f3b180fb0a47_4.png)

![](img/2e359917ca32b442a304f3b180fb0a47_6.png)

![](img/2e359917ca32b442a304f3b180fb0a47_8.png)

## 创建多线程包与文件

首先，我们需要创建一个新的Python包和文件来组织我们的多线程代码。

创建一个名为`multithing`的新Python包。

![](img/2e359917ca32b442a304f3b180fb0a47_10.png)

![](img/2e359917ca32b442a304f3b180fb0a47_11.png)

在该包内，创建一个新的Python文件，命名为`multi_thread_one.py`。

## 导入线程模块

在`multi_thread_one.py`文件中，我们需要从Python的`threading`模块导入`Thread`类。这个类用于创建线程对象。

```python
from threading import Thread
```

![](img/2e359917ca32b442a304f3b180fb0a47_13.png)

![](img/2e359917ca32b442a304f3b180fb0a47_14.png)

![](img/2e359917ca32b442a304f3b180fb0a47_16.png)

从`threading`模块导入`Thread`类，用于创建多线程对象。

![](img/2e359917ca32b442a304f3b180fb0a47_18.png)

## 定义线程目标函数

接下来，我们需要定义一个函数，作为线程启动后要执行的任务。

![](img/2e359917ca32b442a304f3b180fb0a47_20.png)

```python
import time

![](img/2e359917ca32b442a304f3b180fb0a47_22.png)

def f1():
    print("Hi from F1")
    time.sleep(5)
    print("Hi F1 woke up")
```

这个`f1`函数会先打印一条消息，然后休眠5秒钟，最后打印另一条消息。

![](img/2e359917ca32b442a304f3b180fb0a47_24.png)

## 创建并启动线程

![](img/2e359917ca32b442a304f3b180fb0a47_26.png)

现在，我们可以创建一个`Thread`对象，并将`f1`函数设置为其目标。然后启动这个线程。

```python
t1 = Thread(target=f1)
t1.start()
```

`t1.start()`会启动线程，并开始执行`f1`函数中的代码。

![](img/2e359917ca32b442a304f3b180fb0a47_28.png)

## 主程序的并发执行

![](img/2e359917ca32b442a304f3b180fb0a47_30.png)

当线程启动后，主程序并不会等待线程结束，而是继续执行自己的代码。这演示了并发执行。

```python
print("Hi from Main")
time.sleep(5)
print("Hi Main got up")
t1.join()
```

主程序打印消息、休眠，然后使用`t1.join()`等待线程`t1`执行完毕，最后程序结束。

运行这段代码，你会看到来自线程和主程序的输出交错出现，这证明了它们是在同时执行的。

## 创建多个线程并传递参数

上一节我们介绍了如何创建单个线程，本节中我们来看看如何创建多个线程并向目标函数传递参数。

以下是创建多个线程的步骤：

1.  使用循环创建多个`Thread`对象。
2.  通过`args`参数向目标函数传递数据。

```python
def f1(arg):
    print("Hi from F1 with argument " + str(arg))
    time.sleep(5)
    print("Hi F1 woke up with argument " + str(arg))

![](img/2e359917ca32b442a304f3b180fb0a47_32.png)

![](img/2e359917ca32b442a304f3b180fb0a47_33.png)

for i in range(10):
    t = Thread(target=f1, args=(i,))
    t.start()
```

![](img/2e359917ca32b442a304f3b180fb0a47_34.png)

![](img/2e359917ca32b442a304f3b180fb0a47_35.png)

在这段代码中，我们创建了10个线程，每个线程都执行`f1`函数，并传入一个不同的`i`值作为参数。由于线程的执行顺序由操作系统调度决定，因此每次运行的输出顺序可能不同。

## 多线程在聊天应用中的应用

我们学习了多线程的基本概念。这个技术将被用于构建一个聊天应用，其中客户端与服务器需要同时进行发送和接收消息。多线程使得这种双向同时通信成为可能。

![](img/2e359917ca32b442a304f3b180fb0a47_37.png)

![](img/2e359917ca32b442a304f3b180fb0a47_38.png)

![](img/2e359917ca32b442a304f3b180fb0a47_39.png)

本节课中我们一起学习了Python多线程的基础知识，包括如何导入模块、创建线程、定义目标函数以及实现并发执行。我们还了解了如何创建多个线程并传递参数，为后续构建需要并发处理的网络应用（如聊天服务器）打下了基础。