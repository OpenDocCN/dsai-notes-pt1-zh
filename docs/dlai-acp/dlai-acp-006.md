# 006： 使用客户端调用ACP代理 🚀

在本节课中，我们将学习如何使用ACP客户端来调用我们已经搭建好的ACP服务器。我们将创建一个标准化的客户端，并通过它向服务器上的代理发送请求。

![](img/d017eb4cf5e74ae597b0c9efd980a86f_1.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_2.png)

---

![](img/d017eb4cf5e74ae597b0c9efd980a86f_3.png)

## 确认服务器状态 🔍

上一节我们介绍了如何搭建ACP服务器。在开始调用之前，首先需要确认服务器是否仍在运行。

以下是确认服务器状态的步骤：

![](img/d017eb4cf5e74ae597b0c9efd980a86f_5.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_6.png)

1.  打开终端。
2.  运行与启动服务器时相同的命令。
3.  检查终端输出，确认服务器进程是否活跃。

![](img/d017eb4cf5e74ae597b0c9efd980a86f_8.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_9.png)

如果服务器运行在Deep Learning Dojo AI环境中，它会持续运行120分钟。如果在本地运行，只要Python脚本未停止，服务器就会一直运行。

---

## 构建ACP客户端 🛠️

确认服务器正常运行后，接下来我们开始构建ACP客户端。客户端的作用是与ACP服务器的端点进行标准化通信。

首先，我们需要导入必要的库。为了能在Jupyter Notebook环境中运行异步调用，我们需要导入`asyncio`。同时，我们还会从ACP SDK中导入客户端模块，并使用`colorama`库来美化终端输出，使结果更清晰易读。

![](img/d017eb4cf5e74ae597b0c9efd980a86f_11.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_12.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_13.png)

以下是构建客户端所需的导入语句和初始化步骤：

```python
import asyncio
from acp import Client
from colorama import Fore, Style
```

---

## 创建异步调用函数 ⚡

我们将创建一个异步函数来执行对服务器的调用。这个函数会连接到我们的ACP服务器，并向指定的代理发送请求。

![](img/d017eb4cf5e74ae597b0c9efd980a86f_15.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_16.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_17.png)

以下是创建异步函数`example`的步骤：

1.  定义异步函数 `async def example():`。
2.  在函数内部，设置服务器的基本URL（例如 `http://localhost:8001`）。
3.  使用这个URL初始化一个`Client`实例。
4.  使用`await`关键字，通过客户端异步调用服务器上名为`policy_agent`的代理。
5.  调用时，需要传入一个提示词作为输入参数，例如：`"what is the waiting period for rehabilitation"`。
6.  最后，打印出代理返回的结果。我们使用`colorama`将输出文本设置为黄色，以便于识别。

完整的函数代码如下：

```python
async def example():
    base_url = "http://localhost:8001"
    client = Client(base_url=base_url)
    
    # 调用名为 ‘policy_agent‘ 的代理
    response = await client.run(agent="policy_agent", prompt="what is the waiting period for rehabilitation")
    
    # 打印返回结果
    print(Fore.YELLOW + response[0].content + Style.RESET_ALL)
```

---

![](img/d017eb4cf5e74ae597b0c9efd980a86f_19.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_20.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_21.png)

## 执行客户端调用 ▶️

![](img/d017eb4cf5e74ae597b0c9efd980a86f_22.png)

函数定义完成后，我们需要执行它来触发对服务器的调用。

使用 `asyncio.run()` 方法来运行我们定义的 `example` 函数：

```python
asyncio.run(example())
```

当执行这段代码时，你可以同时观察两个地方：
1.  **Jupyter Notebook 单元格输出**：这里将以黄色文本打印出代理的回复。
2.  **运行ACP服务器的终端窗口**：你会看到新的请求日志出现，显示提示词已被接收并正在处理。

如果一切正常，客户端将成功收到服务器的响应，例如：“The waiting period for rehabilitation in the insurance policy is two months.” 这证明我们已成功使用ACP客户端与服务器完成了通信。

![](img/d017eb4cf5e74ae597b0c9efd980a86f_24.png)

---

## 总结 📝

![](img/d017eb4cf5e74ae597b0c9efd980a86f_26.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_27.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_28.png)

![](img/d017eb4cf5e74ae597b0c9efd980a86f_29.png)

本节课中我们一起学习了如何使用ACP客户端。我们首先确认了服务器的运行状态，然后构建了一个标准化的客户端，并创建了一个异步函数来调用服务器上的特定代理。通过传递提示词并接收响应，我们完成了从客户端到服务器的完整调用流程。这为后续连接多个代理和服务器奠定了基础。你可以尝试修改提示词或上传不同的文档，来测试代理的不同功能。