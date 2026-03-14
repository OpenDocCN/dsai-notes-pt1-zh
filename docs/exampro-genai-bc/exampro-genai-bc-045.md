# 45：OPEA故障排除处理程序 🛠️

![](img/dce2f3656509ffb41b42e3850720452e_1.png)

在本节课中，我们将学习如何诊断和解决一个OPEA（Open Platform for Enterprise AI）Mega Service（大型服务）在运行中遇到的问题。我们将通过实际调试过程，理解服务架构、请求流程，并最终使服务正常工作。

## 概述

我们将从一个无法正常工作的Mega Service开始。目标是使其能够成功接收请求，与底层的LLaMA模型服务通信，并返回正确的响应。过程中会涉及Docker容器管理、端口配置、请求格式验证、日志调试以及利用OpenTelemetry进行追踪。

![](img/dce2f3656509ffb41b42e3850720452e_3.png)

![](img/dce2f3656509ffb41b42e3850720452e_4.png)

---

![](img/dce2f3656509ffb41b42e3850720452e_6.png)

![](img/dce2f3656509ffb41b42e3850720452e_8.png)

## 启动基础服务

![](img/dce2f3656509ffb41b42e3850720452e_10.png)

首先，我们需要启动构成我们服务的基础组件，即LLaMA模型服务。

### 运行LLaMA服务

![](img/dce2f3656509ffb41b42e3850720452e_12.png)

![](img/dce2f3656509ffb41b42e3850720452e_14.png)

LLaMA模型通过Docker Compose交付。我们可以通过设置环境变量来指定服务监听的端口。

```bash
# 在项目目录下运行
cd opa-comps/megaservice
LLM_ENDPOINT_PORT=9000 docker-compose up
```

![](img/dce2f3656509ffb41b42e3850720452e_16.png)

**关键点**：如果未设置 `LLM_ENDPOINT_PORT` 环境变量，服务将默认在端口 `8008` 上运行。为了与后续Mega Service配置保持一致，我们将其设置为 `9000`。

![](img/dce2f3656509ffb41b42e3850720452e_18.png)

![](img/dce2f3656509ffb41b42e3850720452e_20.png)

### 验证LLaMA服务

![](img/dce2f3656509ffb41b42e3850720452e_22.png)

![](img/dce2f3656509ffb41b42e3850720452e_24.png)

![](img/dce2f3656509ffb41b42e3850720452e_26.png)

服务启动后，我们需要验证模型是否已下载并可以正常调用。

![](img/dce2f3656509ffb41b42e3850720452e_28.png)

![](img/dce2f3656509ffb41b42e3850720452e_30.png)

![](img/dce2f3656509ffb41b42e3850720452e_32.png)

```bash
# 拉取模型（如果尚未下载）
curl -X POST http://localhost:9000/api/tags -H "Content-Type: application/json" -d '{"name": "llama3:21b"}'

![](img/dce2f3656509ffb41b42e3850720452e_34.png)

# 测试模型生成能力（使用非流式调用）
curl -X POST http://localhost:9000/api/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama3:21b",
    "messages": [{"role": "user", "content": "Hello"}],
    "stream": false
  }'
```

![](img/dce2f3656509ffb41b42e3850720452e_36.png)

![](img/dce2f3656509ffb41b42e3850720452e_37.png)

![](img/dce2f3656509ffb41b42e3850720452e_39.png)

![](img/dce2f3656509ffb41b42e3850720452e_41.png)

![](img/dce2f3656509ffb41b42e3850720452e_43.png)

![](img/dce2f3656509ffb41b42e3850720452e_45.png)

如果上述命令能返回一个完整的响应（而非流式输出块），则证明LLaMA服务运行正常。

![](img/dce2f3656509ffb41b42e3850720452e_46.png)

![](img/dce2f3656509ffb41b42e3850720452e_48.png)

![](img/dce2f3656509ffb41b42e3850720452e_50.png)

![](img/dce2f3656509ffb41b42e3850720452e_52.png)

![](img/dce2f3656509ffb41b42e3850720452e_54.png)

![](img/dce2f3656509ffb41b42e3850720452e_56.png)

---

## 配置并运行Mega Service

![](img/dce2f3656509ffb41b42e3850720452e_58.png)

上一节我们启动了LLaMA服务，本节中我们来看看如何配置和运行我们自己的Mega Service。

![](img/dce2f3656509ffb41b42e3850720452e_60.png)

Mega Service是我们的主应用，它负责接收外部请求，进行编排（Orchestration），并将任务调度给LLaMA等服务。

![](img/dce2f3656509ffb41b42e3850720452e_62.png)

![](img/dce2f3656509ffb41b42e3850720452e_64.png)

### 启动Mega Service

![](img/dce2f3656509ffb41b42e3850720452e_66.png)

Mega Service是一个Python应用，通常通过 `app.py` 启动。

![](img/dce2f3656509ffb41b42e3850720452e_68.png)

![](img/dce2f3656509ffb41b42e3850720452e_70.png)

![](img/dce2f3656509ffb41b42e3850720452e_72.png)

![](img/dce2f3656509ffb41b42e3850720452e_74.png)

![](img/dce2f3656509ffb41b42e3850720452e_75.png)

```bash
# 在Mega Service项目目录下
python app.py
```
应用默认会在端口 `8000` 启动。

![](img/dce2f3656509ffb41b42e3850720452e_77.png)

![](img/dce2f3656509ffb41b42e3850720452e_78.png)

![](img/dce2f3656509ffb41b42e3850720452e_80.png)

### 测试Mega Service端点

服务启动后，我们可以向其发送一个测试请求。

![](img/dce2f3656509ffb41b42e3850720452e_82.png)

![](img/dce2f3656509ffb41b42e3850720452e_84.png)

```bash
curl -X POST http://localhost:8000/ \
  -H "Content-Type: application/json" \
  -d '{
    "model": "test-model",
    "messages": [{"role": "user", "content": "Hello, this is a test message"}],
    "stream": false
  }' | jq .
```
我们使用 `jq` 工具来美化输出的JSON，便于阅读。

![](img/dce2f3656509ffb41b42e3850720452e_86.png)

![](img/dce2f3656509ffb41b42e3850720452e_88.png)

**遇到的问题**：初始测试返回 `"no response content available"`，并且响应中的模型名称为 `"test-model"`，而非我们配置的 `"llama3:21b"`。这表明请求未能正确传递到LLaMA服务。

---

![](img/dce2f3656509ffb41b42e3850720452e_90.png)

![](img/dce2f3656509ffb41b42e3850720452e_91.png)

![](img/dce2f3656509ffb41b42e3850720452e_93.png)

![](img/dce2f3656509ffb41b42e3850720452e_95.png)

![](img/dce2f3656509ffb41b42e3850720452e_97.png)

![](img/dce2f3656509ffb41b42e3850720452e_98.png)

![](img/dce2f3656509ffb41b42e3850720452e_100.png)

![](img/dce2f3656509ffb41b42e3850720452e_101.png)

## 诊断请求流程

![](img/dce2f3656509ffb41b42e3850720452e_103.png)

![](img/dce2f3656509ffb41b42e3850720452e_105.png)

![](img/dce2f3656509ffb41b42e3850720452e_107.png)

![](img/dce2f3656509ffb41b42e3850720452e_109.png)

![](img/dce2f3656509ffb41b42e3850720452e_111.png)

为了找出问题所在，我们需要在代码中添加详细的日志，以跟踪请求在系统中的流转路径。

![](img/dce2f3656509ffb41b42e3850720452e_113.png)

![](img/dce2f3656509ffb41b42e3850720452e_114.png)

![](img/dce2f3656509ffb41b42e3850720452e_116.png)

![](img/dce2f3656509ffb41b42e3850720452e_118.png)

![](img/dce2f3656509ffb41b42e3850720452e_120.png)

![](img/dce2f3656509ffb41b42e3850720452e_122.png)

![](img/dce2f3656509ffb41b42e3850720452e_124.png)

![](img/dce2f3656509ffb41b42e3850720452e_126.png)

![](img/dce2f3656509ffb41b42e3850720452e_128.png)

![](img/dce2f3656509ffb41b42e3850720452e_130.png)

![](img/dce2f3656509ffb41b42e3850720452e_132.png)

### 修改处理器代码

![](img/dce2f3656509ffb41b42e3850720452e_134.png)

![](img/dce2f3656509ffb41b42e3850720452e_136.png)

![](img/dce2f3656509ffb41b42e3850720452e_138.png)

![](img/dce2f3656509ffb41b42e3850720452e_140.png)

![](img/dce2f3656509ffb41b42e3850720452e_142.png)

![](img/dce2f3656509ffb41b42e3850720452e_143.png)

![](img/dce2f3656509ffb41b42e3850720452e_144.png)

![](img/dce2f3656509ffb41b42e3850720452e_146.png)

Mega Service的核心是请求处理器（Handler）。我们在 `handle_request` 方法中添加打印语句。

![](img/dce2f3656509ffb41b42e3850720452e_148.png)

```python
# 在 app.py 的 handle_request 方法中
async def handle_request(self, request: Request):
    # ... 其他代码 ...
    print(f"1. 收到原始请求数据: {data}")
    print(f"2. 流选项: {stream_opt}")
    print(f"3. 解析后的聊天请求: {chat_request}")
    # ... 调度请求 ...
    print(f"4. 调度结果: {result}")
    # ... 处理响应 ...
```
通过检查这些日志，我们可以确认：
1.  客户端发送的原始数据格式。
2.  是否正确解析出了流式调用选项。
3.  聊天请求对象是否包含正确的模型和消息。

![](img/dce2f3656509ffb41b42e3850720452e_150.png)

![](img/dce2f3656509ffb41b42e3850720452e_152.png)

![](img/dce2f3656509ffb41b42e3850720452e_154.png)

![](img/dce2f3656509ffb41b42e3850720452e_156.png)

![](img/dce2f3656509ffb41b42e3850720452e_158.png)

![](img/dce2f3656509ffb41b42e3850720452e_160.png)

![](img/dce2f3656509ffb41b42e3850720452e_162.png)

### 检查调度器输入

![](img/dce2f3656509ffb41b42e3850720452e_164.png)

![](img/dce2f3656509ffb41b42e3850720452e_166.png)

问题可能出在Mega Service内部调度器（Scheduler）的输入格式上。我们深入调度器代码，查看它实际接收到什么。

![](img/dce2f3656509ffb41b42e3850720452e_168.png)

![](img/dce2f3656509ffb41b42e3850720452e_170.png)

![](img/dce2f3656509ffb41b42e3850720452e_172.png)

![](img/dce2f3656509ffb41b42e3850720452e_174.png)

![](img/dce2f3656509ffb41b42e3850720452e_176.png)

![](img/dce2f3656509ffb41b42e3850720452e_178.png)

```python
# 在调度器相关代码中添加
import json
print("调试 - 调度器输入:", json.dumps(inputs, indent=2))
```
我们发现，经过 `handle_message` 函数处理后，多条消息历史被合并，并且格式可能不符合LLaMA API的预期。

![](img/dce2f3656509ffb41b42e3850720452e_180.png)

![](img/dce2f3656509ffb41b42e3850720452e_182.png)

![](img/dce2f3656509ffb41b42e3850720452e_184.png)

---

![](img/dce2f3656509ffb41b42e3850720452e_186.png)

## 修正请求格式与参数

经过逐步排查，我们发现几个关键问题：

![](img/dce2f3656509ffb41b42e3850720452e_188.png)

1.  **模型参数未传递**：调度器调用时，未将 `model` 参数放入 `llm_parameters` 中。
2.  **消息格式错误**：直接使用了经过处理的 `prompt` 字符串，而非LLaMA所需的 `messages` 数组格式。
3.  **流式参数**：虽然在请求中指定了 `"stream": false`，但内部处理可能未正确传递此参数。

### 最终修正

![](img/dce2f3656509ffb41b42e3850720452e_190.png)

![](img/dce2f3656509ffb41b42e3850720452e_192.png)

以下是修正后的 `handle_request` 方法核心部分：

![](img/dce2f3656509ffb41b42e3850720452e_194.png)

![](img/dce2f3656509ffb41b42e3850720452e_196.png)

![](img/dce2f3656509ffb41b42e3850720452e_198.png)

![](img/dce2f3656509ffb41b42e3850720452e_200.png)

```python
async def handle_request(self, request: Request):
    data = await request.json()
    stream_opt = data.get("stream", False)
    chat_request = ChatCompletionRequest.model_validate(data)

    # 准备调度参数
    llm_parameters = {
        "model": chat_request.model,  # 确保传递模型名称
        "stream": stream_opt          # 确保传递流式选项
    }

    # 使用正确的消息格式作为输入
    initial_inputs = {"messages": chat_request.messages}

    # 调用调度器
    result = await self.megaservice.schedule(
        initial_inputs=initial_inputs,
        llm_parameters=llm_parameters
    )
    # ... 后续处理响应 ...
```
**关键修正**：
*   从验证后的 `chat_request` 中提取 `model` 和 `messages`。
*   将 `model` 和 `stream` 明确放入 `llm_parameters`。
*   将原始的 `messages` 列表直接作为 `initial_inputs` 传递，而不是使用处理后的文本。

![](img/dce2f3656509ffb41b42e3850720452e_202.png)

![](img/dce2f3656509ffb41b42e3850720452e_204.png)

![](img/dce2f3656509ffb41b42e3850720452e_206.png)

![](img/dce2f3656509ffb41b42e3850720452e_208.png)

![](img/dce2f3656509ffb41b42e3850720452e_210.png)

![](img/dce2f3656509ffb41b42e3850720452e_212.png)

![](img/dce2f3656509ffb41b42e3850720452e_214.png)

### 处理响应格式

![](img/dce2f3656509ffb41b42e3850720452e_216.png)

![](img/dce2f3656509ffb41b42e3850720452e_217.png)

LLaMA返回的响应是OpenAI兼容格式，我们需要从结果中正确提取信息。

![](img/dce2f3656509ffb41b42e3850720452e_219.png)

![](img/dce2f3656509ffb41b42e3850720452e_221.png)

![](img/dce2f3656509ffb41b42e3850720452e_223.png)

```python
# 处理非流式响应
if not isinstance(response, StreamingResponse):
    # 假设响应结构为 Open AI 格式
    if "choices" in response and len(response["choices"]) > 0:
        content = response["choices"][0].get("message", {}).get("content")
        if content:
            return JSONResponse(content={"response": content})
    # 错误处理
    return JSONResponse(
        content={"error": "Unexpected response format"},
        status_code=500
    )
```

---

![](img/dce2f3656509ffb41b42e3850720452e_225.png)

![](img/dce2f3656509ffb41b42e3850720452e_227.png)

## 利用可观测性工具

![](img/dce2f3656509ffb41b42e3850720452e_229.png)

![](img/dce2f3656509ffb41b42e3850720452e_231.png)

![](img/dce2f3656509ffb41b42e3850720452e_233.png)

![](img/dce2f3656509ffb41b42e3850720452e_234.png)

![](img/dce2f3656509ffb41b42e3850720452e_236.png)

![](img/dce2f3656509ffb41b42e3850720452e_238.png)

![](img/dce2f3656509ffb41b42e3850720452e_240.png)

在调试过程中，配置可观测性工具（如Jaeger）可以极大地帮助理解请求在分布式系统中的流向。

![](img/dce2f3656509ffb41b42e3850720452e_242.png)

![](img/dce2f3656509ffb41b42e3850720452e_244.png)

![](img/dce2f3656509ffb41b42e3850720452e_246.png)

### 配置OpenTelemetry与Jaeger

![](img/dce2f3656509ffb41b42e3850720452e_248.png)

![](img/dce2f3656509ffb41b42e3850720452e_250.png)

![](img/dce2f3656509ffb41b42e3850720452e_252.png)

在 `docker-compose.yml` 中添加Jaeger服务。

![](img/dce2f3656509ffb41b42e3850720452e_254.png)

```yaml
version: '3.8'
services:
  jaeger:
    image: jaegertracing/all-in-one:latest
    ports:
      - "16686:16686"  # Jaeger UI 端口
      - "4317:4317"    # OTLP gRPC 接收端口
      - "4318:4318"    # OTLP HTTP 接收端口
```
在Mega Service中配置OpenTelemetry端点指向Jaeger。

![](img/dce2f3656509ffb41b42e3850720452e_256.png)

![](img/dce2f3656509ffb41b42e3850720452e_258.png)

```python
# 在 app.py 的配置中
self.telemetry_endpoint = "http://jaeger:4318"
```
启动后，可以通过 `http://localhost:16686` 访问Jaeger UI，查看请求的追踪链，包括在Mega Service内部及对LLaMA服务的调用。

---

## 总结

![](img/dce2f3656509ffb41b42e3850720452e_260.png)

![](img/dce2f3656509ffb41b42e3850720452e_262.png)

![](img/dce2f3656509ffb41b42e3850720452e_263.png)

本节课中我们一起学习了如何系统性地对一个复杂的生成式AI服务（OPEA Mega Service）进行故障排除。我们经历了以下关键步骤：

![](img/dce2f3656509ffb41b42e3850720452e_265.png)

![](img/dce2f3656509ffb41b42e3850720452e_267.png)

1.  **环境检查**：确保所有依赖服务（如LLaMA）正确启动并配置了正确的端口。
2.  **请求验证**：使用 `curl` 直接测试底层服务，隔离问题。
3.  **日志注入**：在代码关键路径添加打印语句，跟踪数据流和格式变化。
4.  **格式对齐**：仔细对比Mega Service内部数据结构与目标API（LLaMA）的期望格式，修正了模型参数传递和消息格式问题。
5.  **利用工具**：配置OpenTelemetry和Jaeger，实现请求链路的可视化追踪，辅助调试。

![](img/dce2f3656509ffb41b42e3850720452e_269.png)

核心收获是：**在复杂AI工程中，清晰的日志、对数据格式的严格把控以及可观测性基础设施是成功调试的基石**。即使有AI辅助编程工具，深入理解系统架构和代码执行流程的能力仍然不可或缺。