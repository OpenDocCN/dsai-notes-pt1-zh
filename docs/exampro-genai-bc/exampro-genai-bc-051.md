# 51：OPEA TTS 微服务 🎙️

![](img/fd6775a7d532057fed50cc63301cb671_1.png)

在本节课中，我们将学习如何集成并使用 OPEA 框架中的文本转语音（TTS）微服务组件。我们将尝试部署 Speech T5 服务，并探索一个功能更丰富的 GOT 服务，了解如何通过 Docker Compose 配置和运行这些服务。

## 概述

![](img/fd6775a7d532057fed50cc63301cb671_3.png)

我们将从 OPEA 组件库中选择一个 TTS 服务进行部署。虽然最初尝试的视觉语言模型（VLM）未能成功运行，但我们可以灵活选择其他组件。本节课将重点介绍 Speech T5 文本转语音服务，并尝试配置一个带有 Web UI 的 GOT 服务。我们将通过 Docker Compose 管理这些服务，并学习如何解决常见的端口冲突和环境变量配置问题。

## 选择 TTS 组件

上一节我们尝试了其他模型，本节我们来看看 OPEA 组件库中可用的选项。我们的目标是找到一个可以运行的文本转语音服务。

![](img/fd6775a7d532057fed50cc63301cb671_5.png)

以下是 OPEA 组件库中一些相关的 TTS 服务：
*   **Speech T5 Service**: 一个基础的文本转语音微服务。
*   **GOT Service**: 一个功能强大的少样本语音转换和文本转语音 Web UI，支持即时语音合成。
*   **TTS Service**: 一个通用的 TTS 服务接口，可能作为其他服务的基础。

![](img/fd6775a7d532057fed50cc63301cb671_7.png)

## 部署基础 Speech T5 服务

![](img/fd6775a7d532057fed50cc63301cb671_9.png)

首先，我们尝试部署最基础的 Speech T5 服务。根据文档，我们可以将其配置添加到 Docker Compose 文件中。

```yaml
speecht5-service:
  image: ghcr.io/opea-project/speecht5-service:latest
  ports:
    - "7775:7775"
```

启动服务后，我们尝试使用 `curl` 命令调用其 API 端点来生成语音。我们使用了示例中的命令格式。

![](img/fd6775a7d532057fed50cc63301cb671_11.png)

![](img/fd6775a7d532057fed50cc63301cb671_13.png)

```bash
curl -X POST "http://localhost:7775/v1/audio/speech" -H "Content-Type: application/json" -d '{"input": "Who are you?", "voice": "default"}'
```

服务器返回了“Not Found”错误。这表明我们猜测的 API 端点不正确，或者该服务需要通过特定的客户端（可能是 gRPC）进行通信，而不是简单的 HTTP REST API。

![](img/fd6775a7d532057fed50cc63301cb671_15.png)

## 集成 TTS 服务接口

![](img/fd6775a7d532057fed50cc63301cb671_17.png)

为了解决 API 调用问题，我们需要部署 `tts-service`。这个服务作为一个 FastAPI 包装器，提供了标准的 HTTP 端点，并可能集成了日志和追踪功能，这正是 OPEA 企业级能力的体现。

![](img/fd6775a7d532057fed50cc63301cb671_19.png)

我们将 `tts-service` 的配置加入 Docker Compose。它的配置可能依赖于或扩展自 `speecht5-service`。

![](img/fd6775a7d532057fed50cc63301cb671_21.png)

```yaml
tts-service:
  extends:
    file: speecht5-service.yml
    service: speecht5-service
  image: ghcr.io/opea-project/tts-service:latest
  ports:
    - "9090:9090"
  environment:
    - TTS_ENDPOINT=http://speecht5-service:7775
```

![](img/fd6775a7d532057fed50cc63301cb671_23.png)

![](img/fd6775a7d532057fed50cc63301cb671_25.png)

![](img/fd6775a7d532057fed50cc63301cb671_27.png)

![](img/fd6775a7d532057fed50cc63301cb671_29.png)

![](img/fd6775a7d532057fed50cc63301cb671_31.png)

![](img/fd6775a7d532057fed50cc63301cb671_32.png)

![](img/fd6775a7d532057fed50cc63301cb671_34.png)

然而，直接使用 `extends` 并同时启动多个服务时遇到了端口冲突问题。为了简化，我们选择将配置合并，直接在一个服务定义中指定所有参数。

![](img/fd6775a7d532057fed50cc63301cb671_36.png)

![](img/fd6775a7d532057fed50cc63301cb671_38.png)

![](img/fd6775a7d532057fed50cc63301cb671_40.png)

## 配置环境变量与测试

![](img/fd6775a7d532057fed50cc63301cb671_42.png)

![](img/fd6775a7d532057fed50cc63301cb671_44.png)

合并配置后，我们还需要正确设置环境变量，特别是 `TTS_ENDPOINT`，以告诉 `tts-service` 后端语音合成引擎的位置。

![](img/fd6775a7d532057fed50cc63301cb671_46.png)

![](img/fd6775a7d532057fed50cc63301cb671_48.png)

![](img/fd6775a7d532057fed50cc63301cb671_50.png)

我们在 Docker Compose 文件中硬编码了这些环境变量：

![](img/fd6775a7d532057fed50cc63301cb671_52.png)

```yaml
environment:
  - TTS_ENDPOINT=http://speecht5-service:7775
  - NO_PROXY=localhost
```

重新启动服务后，再次使用 `curl` 命令调用 `tts-service` 的端点（端口 9090）。这次请求成功，并返回了一个音频文件。

![](img/fd6775a7d532057fed50cc63301cb671_54.png)

```bash
curl -X POST "http://localhost:9090/v1/audio/speech" -H "Content-Type: application/json" -d '{"input": "Who are you?", "voice": "default"}' --output speech.wav
```

播放音频文件，可以听到合成语音“Who are you?”，虽然音质有改进空间，但证明服务已成功运行。

![](img/fd6775a7d532057fed50cc63301cb671_56.png)

![](img/fd6775a7d532057fed50cc63301cb671_58.png)

![](img/fd6775a7d532057fed50cc63301cb671_60.png)

![](img/fd6775a7d532057fed50cc63301cb671_62.png)

## 尝试部署 GOT Web UI 服务

![](img/fd6775a7d532057fed50cc63301cb671_64.png)

![](img/fd6775a7d532057fed50cc63301cb671_66.png)

![](img/fd6775a7d532057fed50cc63301cb671_68.png)

在基础 TTS 功能运行成功后，我们尝试部署功能更丰富的 GOT 服务，它提供了一个 Web 界面，并支持少样本语音克隆。

我们将其配置添加到 Docker Compose 文件中，并注意处理与已有服务的端口冲突（将 `tts-service` 注释掉）。GOT 服务运行在 9880 端口。

![](img/fd6775a7d532057fed50cc63301cb671_70.png)

```yaml
got-service:
  image: ghcr.io/opea-project/got-service:latest
  ports:
    - "9880:9880"
```

![](img/fd6775a7d532057fed50cc63301cb671_72.png)

![](img/fd6775a7d532057fed50cc63301cb671_74.png)

服务启动后，我们尝试访问 `http://localhost:9880`，但遇到了“Internal Server Error”。查看日志发现存在参数类型错误。由于时间关系，我们未能完全解决此问题，但明确了该服务需要额外的调试和配置才能正常工作。

![](img/fd6775a7d532057fed50cc63301cb671_76.png)

![](img/fd6775a7d532057fed50cc63301cb671_78.png)

## 总结

![](img/fd6775a7d532057fed50cc63301cb671_80.png)

![](img/fd6775a7d532057fed50cc63301cb671_82.png)

![](img/fd6775a7d532057fed50cc63301cb671_84.png)

本节课中我们一起学习了如何在 OPEA 框架中集成文本转语音微服务。我们成功部署了 Speech T5 引擎及其 FastAPI 包装器 `tts-service`，并通过 HTTP API 成功合成了语音。这个过程涉及了 Docker Compose 配置、环境变量设置和端口管理。我们还尝试部署了带有高级功能的 GOT 服务，虽然未能完全运行，但了解了其潜力。这些组件可以作为构建块，未来集成到更复杂的 AI 应用管道中，实现语音交互功能。