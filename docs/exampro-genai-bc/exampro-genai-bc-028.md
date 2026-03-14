# 28：构建OPEA Mega Service 🚀

![](img/6f88b8c43126bbbb459b224b1d0a19c6_1.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_2.png)

在本节课中，我们将学习如何构建一个OPEA（Open Platform for Enterprise AI）的“Mega Service”。Mega Service是指将多个独立的微服务组合在一起，使其协同工作的复合服务。我们将跟随视频中的探索过程，从理解文档、解决依赖问题，到尝试运行一个基础的Mega Service。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_4.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_6.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_8.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_10.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_12.png)

## 回顾与目标设定

![](img/6f88b8c43126bbbb459b224b1d0a19c6_14.png)

上一节我们介绍了OPEA的基本概念。本节中，我们来看看如何动手构建一个Mega Service。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_16.png)

视频开始时，作者回顾了之前失败的尝试，并决定重新查阅OPEA的技术文档来寻找构建Mega Service的正确方法。核心目标是理解如何将多个服务组合并运行起来。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_18.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_20.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_22.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_24.png)

## 查阅文档与寻找示例

![](img/6f88b8c43126bbbb459b224b1d0a19c6_26.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_27.png)

作者首先回到了OPEA的开发文档页面，特别关注了关于构建Mega Service的“技术文档”部分。文档中提到可以使用装饰器创建微服务，并以嵌入服务为例。然而，文档中的代码示例并不完整，没有展示如何调用或运行它。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_29.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_31.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_33.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_35.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_37.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_39.png)

为了解决这个问题，作者转向了OPEA的示例项目。这是一个关键的转折点，因为示例项目中包含了已经组装好的Mega Service，可以作为参考。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_41.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_43.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_45.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_47.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_49.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_51.png)

以下是分析示例项目时发现的要点：
*   在示例项目的Dockerfile中，可以看到服务的入口点是 `chat_qa_api`。
*   打开对应的Python文件，发现其代码结构与文档中展示的代码非常相似。
*   代码中包含一个 `main` 函数和 `start` 函数，用于启动服务。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_53.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_55.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_57.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_59.png)

## 创建项目与解决依赖

![](img/6f88b8c43126bbbb459b224b1d0a19c6_61.png)

基于对示例的分析，作者决定在本地创建一个新的Mega Service项目。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_63.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_65.png)

首先，在 `OPEA-comps` 目录下创建了一个名为 `megaservice` 的新文件夹，并创建了 `app.py` 文件。接着，从文档中复制了基础的Mega Service代码框架到该文件中。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_67.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_69.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_71.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_73.png)

代码复制后，遇到了第一个挑战：导入 `comps` 模块。尽管之前通过 `pip install -r requirements.txt` 安装了 `opea-comps` 包，但Python解释器仍然无法识别 `comps`。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_74.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_76.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_78.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_80.png)

为了解决这个导入问题，作者进行了一系列诊断：
1.  使用 `print(comps.__file__)` 来检查包的位置，发现路径为 `None`，表明安装可能有问题。
2.  使用 `pip uninstall opea-comps` 然后重新安装，解决了安装问题。
3.  确认导入成功后，代码中的 `from comps import ...` 语句不再报错。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_82.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_84.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_86.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_88.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_90.png)

## 构建并尝试运行服务

![](img/6f88b8c43126bbbb459b224b1d0a19c6_92.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_94.png)

导入问题解决后，下一步是让Mega Service运行起来。作者参考了 `chat_qa` 示例的代码结构。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_96.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_98.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_100.png)

首先，需要创建服务的实例并启动它。在示例中，这是通过初始化一个类并调用其 `start()` 方法完成的。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_102.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_104.png)

```python
# 创建服务实例
example_service = ExampleService()
# 启动服务
example_service.start()
```

![](img/6f88b8c43126bbbb459b224b1d0a19c6_106.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_108.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_110.png)

然而，直接运行这段代码会遇到更多问题。服务类需要定义 `endpoint` 属性和处理请求的 `handle_request` 方法。作者从示例代码中借鉴了这些部分的实现，特别是 `handle_request` 方法，它负责接收请求、通过服务编排器转发给LLM（大语言模型）并返回响应。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_112.png)

## 连接实际服务与调试

服务框架搭建好后，需要连接实际的后端服务（如LLM）。在代码中，通过 `add_remote_service` 方法添加了一个远程LLM服务，并指定其运行在 `localhost:9000`。

为了测试，作者启动了另一个终端，在9000端口运行了一个Ollama的LLM服务（使用Llama 3模型）。然后尝试用curl命令向Mega Service（运行在8000端口）发送请求。

以下是测试时使用的curl命令示例：
```bash
curl -X POST http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama3.2:1b",
    "messages": [{"role": "user", "content": "Hello!"}],
    "stream": false
  }'
```

![](img/6f88b8c43126bbbb459b224b1d0a19c6_114.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_116.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_118.png)

测试过程中遇到了几个错误：
1.  **连接被拒绝**：最初因为OpenTelemetry（用于遥测数据收集）试图连接不存在的端口而失败。通过设置环境变量 `OTEL_SDK_DISABLED=true` 临时禁用它来解决。
2.  **响应格式错误**：服务收到了LLM的回复，但在将响应流格式化为最终结果时出错。作者根据AI助手的建议多次修改了 `handle_request` 方法中的响应处理逻辑。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_120.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_122.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_124.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_126.png)

尽管经过多次调试，最终返回的响应内容仍然不正确（例如返回“no response content available”）。作者推测这可能与使用的Ollama后端与Mega Service期望的TGI（Text Generation Inference）格式不完全兼容有关。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_128.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_130.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_132.png)

## 总结与展望

![](img/6f88b8c43126bbbb459b224b1d0a19c6_134.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_136.png)

本节课中我们一起学习了构建OPEA Mega Service的完整流程。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_138.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_140.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_142.png)

我们从分析文档和示例代码开始，逐步创建了自己的服务框架，解决了Python包依赖和导入的核心问题。我们实现了服务的基本启动逻辑和请求处理流程，并尝试将其与一个实际的Ollama LLM服务连接。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_144.png)

![](img/6f88b8c43126bbbb459b224b1d0a19c6_146.png)

虽然最终未能让服务返回完美的结果，但这个过程揭示了企业级AI应用集成中的常见挑战，例如服务间通信、数据格式协商和错误处理。关键的收获在于掌握了通过研究现有项目、逐步调试和利用AI辅助编程来解决复杂问题的方法。

![](img/6f88b8c43126bbbb459b224b1d0a19c6_148.png)

正如作者在视频末尾提到的，OPEA的示例项目中已有配置好能与Ollama协同工作的Mega Service（如`chat-qa`）。在未来的学习中，我们可以直接深入研究这些成功案例的代码，从而更快地构建出可工作的复合AI服务。