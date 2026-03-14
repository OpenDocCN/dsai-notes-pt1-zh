# 50：尝试集成vLLM服务失败

## 概述
在本节课中，我们将尝试在OPA项目中集成vLLM服务，以替代之前使用的Ollama。我们将深入分析服务注册机制、Docker容器配置，并尝试解决在本地运行vLLM时遇到的各种问题，特别是GPU支持和模型加载方面的挑战。

## 服务注册机制的困惑与澄清
上一节我们介绍了微服务的注册机制，本节中我们来看看在实际项目中它是如何工作的。

最初，我对`@service`装饰器感到困惑，因为查看LLM服务的实现代码时，发现它已经在组件端注册了微服务。

```python
# 在组件代码中注册服务
@service
def text_generation_service():
    ...
```

![](img/2a288569a56be0d22df6bd271891473a_1.png)

但在我们的聚合服务中，我们也进行了注册。经过分析，我意识到这些组件实际上是作为预构建的Docker容器提供的。

![](img/2a288569a56be0d22df6bd271891473a_3.png)

![](img/2a288569a56be0d22df6bd271891473a_4.png)

![](img/2a288569a56be0d22df6bd271891473a_5.png)

![](img/2a288569a56be0d22df6bd271891473a_7.png)

## 探索预构建的容器镜像
以下是OPA项目提供的预构建容器：

![](img/2a288569a56be0d22df6bd271891473a_9.png)

*   **vLLM容器**：用于高性能推理
*   **Ollama容器**：用于本地模型运行
*   **TGI容器**：已弃用（自v1.2版本起）

![](img/2a288569a56be0d22df6bd271891473a_11.png)

![](img/2a288569a56be0d22df6bd271891473a_13.png)

通过查看Docker Compose文件，我确认了这些镜像直接从Docker Hub拉取，无需本地构建。

![](img/2a288569a56be0d22df6bd271891473a_15.png)

```yaml
# Docker Compose配置示例
services:
  vllm-service:
    image: opa/vllm:latest
    ports:
      - "9009:80"
```

## 尝试集成vLLM服务
鉴于vLLM在生产环境中的分布式优势，我决定尝试集成它，而不是继续使用Ollama。

![](img/2a288569a56be0d22df6bd271891473a_17.png)

![](img/2a288569a56be0d22df6bd271891473a_19.png)

我从现有的Chat QA服务中复制了vLLM的配置，并准备将其集成到我们的项目中。

![](img/2a288569a56be0d22df6bd271891473a_20.png)

![](img/2a288569a56be0d22df6bd271891473a_21.png)

![](img/2a288569a56be0d22df6bd271891473a_22.png)

![](img/2a288569a56be0d22df6bd271891473a_24.png)

```yaml
# 复制的vLLM服务配置
vllm-service:
  image: opa/vllm:latest
  ports:
    - "9009:80"
  volumes:
    - ./data:/data
  shm_size: 12gb
  environment:
    - NO_PROXY=vllm
    - HTTP_PROXY
    - HTTPS_PROXY
```

## 配置环境变量
为了让vLLM服务正常运行，需要正确配置环境变量。

![](img/2a288569a56be0d22df6bd271891473a_26.png)

我创建了`.env`文件来管理敏感信息，但发现Docker Compose可能没有正确加载它。作为临时解决方案，我选择在配置文件中硬编码这些值进行测试。

![](img/2a288569a56be0d22df6bd271891473a_28.png)

```yaml
# 硬编码环境变量进行测试
environment:
  - HUGGING_FACE_HUB_TOKEN=your_token_here
  - LLM_MODEL_ID=meta-llama/Llama-3.2-1B
```

## 遇到的运行时问题
启动vLLM服务后，我遇到了一系列错误。

![](img/2a288569a56be0d22df6bd271891473a_30.png)

服务日志显示与GPU相关的错误，表明vLLM尝试使用GPU加速，但我的环境配置可能有问题。

```
ERROR: CUDA is not supported on CPU
ERROR: Triton is not compatible with available functions
```

![](img/2a288569a56be0d22df6bd271891473a_32.png)

![](img/2a288569a56be0d22df6bd271891473a_34.png)

## 尝试配置GPU支持
为了解决GPU问题，我尝试修改Docker Compose配置以启用GPU支持。

![](img/2a288569a56be0d22df6bd271891473a_36.png)

![](img/2a288569a56be0d22df6bd271891473a_38.png)

我参考了vLLM的文档和社区示例，添加了GPU资源配置。

![](img/2a288569a56be0d22df6bd271891473a_40.png)

![](img/2a288569a56be0d22df6bd271891473a_41.png)

![](img/2a288569a56be0d22df6bd271891473a_43.png)

```yaml
# 尝试添加GPU支持
deploy:
  resources:
    reservations:
      devices:
        - driver: nvidia
          count: 1
          capabilities: [gpu]
```

![](img/2a288569a56be0d22df6bd271891473a_45.png)

## 持续的配置挑战
尽管多次尝试调整配置，vLLM服务仍然无法正常运行。

![](img/2a288569a56be0d22df6bd271891473a_47.png)

主要问题包括：
1.  Docker无法找到合适的NVIDIA驱动
2.  模型架构检查失败
3.  环境变量加载问题

我尝试了各种解决方案，包括安装NVIDIA工具包、重启Docker服务，但问题依然存在。

## 模型兼容性问题
在排查过程中，我发现模型选择可能也是一个问题。

vLLM可能不支持我选择的Llama 3.2-1B模型。我尝试切换到更小的模型进行测试。

![](img/2a288569a56be0d22df6bd271891473a_49.png)

![](img/2a288569a56be0d22df6bd271891473a_51.png)

![](img/2a288569a56be0d22df6bd271891473a_53.png)

```yaml
# 尝试使用不同的模型
environment:
  - LLM_MODEL_ID=google/gemma-2b
```

![](img/2a288569a56be0d22df6bd271891473a_55.png)

![](img/2a288569a56be0d22df6bd271891473a_57.png)

![](img/2a288569a56be0d22df6bd271891473a_58.png)

![](img/2a288569a56be0d22df6bd271891473a_60.png)

![](img/2a288569a56be0d22df6bd271891473a_62.png)

## 分析vLLM的构建过程
为了深入理解问题，我查看了vLLM容器的Dockerfile。

我发现OPA的vLLM镜像是基于OpenVINO优化的，专门为CPU设计，这与我遇到的GPU错误相矛盾。

![](img/2a288569a56be0d22df6bd271891473a_64.png)

![](img/2a288569a56be0d22df6bd271891473a_66.png)

```dockerfile
# Dockerfile片段显示OpenVINO优化
RUN git clone -b v0.7.3 https://github.com/vllm-project/vllm.git
RUN cd vllm && pip install -e .[openvino]
```

## 放弃vLLM集成
经过多次尝试和排查，我决定暂时放弃vLLM集成。

![](img/2a288569a56be0d22df6bd271891473a_68.png)

![](img/2a288569a56be0d22df6bd271891473a_69.png)

失败的原因可能包括：
1.  本地环境与vLLM要求的配置不匹配
2.  模型兼容性问题
3.  Docker GPU配置复杂

![](img/2a288569a56be0d22df6bd271891473a_71.png)

对于本地开发环境，继续使用Ollama可能是更简单可靠的选择。

![](img/2a288569a56be0d22df6bd271891473a_73.png)

## 总结
本节课中我们一起学习了尝试在OPA项目中集成vLLM服务的完整过程。我们从分析服务注册机制开始，探索了预构建的容器镜像，尝试配置和运行vLLM服务，并遇到了GPU支持、环境变量加载和模型兼容性等多重挑战。虽然最终未能成功集成vLLM，但这个过程中我们深入了解了微服务架构的容器化部署、Docker配置的复杂性，以及在实际项目中解决问题的方法论。对于生产环境，vLLM仍然是值得考虑的选择，但在本地开发中，Ollama提供了更简单直接的解决方案。