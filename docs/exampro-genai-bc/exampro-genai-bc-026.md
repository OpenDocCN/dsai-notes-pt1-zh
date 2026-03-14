# 26：OPEA组件与Ollama 🚀

在本节课中，我们将学习如何使用OPEA（企业开放平台）框架的基础组件，特别是如何运行一个基于Ollama的模型服务容器。我们将从设置环境开始，逐步配置并运行服务，最后通过API与模型进行交互。

---

## 概述

OPEA是一个开源项目集合，旨在为企业提供构建生成式AI工作负载的模块化组件。本节课的核心目标是理解并运行其核心组件之一：Ollama模型服务。我们将通过Docker容器来部署它，并学习如何与之交互。

![](img/59451dcf14943cb7b0e883332bf11af5_1.png)

![](img/59451dcf14943cb7b0e883332bf11af5_3.png)

上一节我们介绍了OPEA项目的背景和组件概览，本节中我们来看看如何具体运行一个Ollama模型服务。

---

## 环境准备与Docker安装

首先，我们需要确保本地环境已安装Docker。以下是在Linux（例如WSL）环境下安装Docker的步骤。

以下是安装Docker所需的命令序列：

![](img/59451dcf14943cb7b0e883332bf11af5_5.png)

```bash
# 更新包索引并安装依赖包
sudo apt-get update
sudo apt-get install ca-certificates curl

![](img/59451dcf14943cb7b0e883332bf11af5_7.png)

# 添加Docker官方GPG密钥
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# 设置Docker稳定版仓库
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 更新包索引并安装Docker引擎
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

安装完成后，为了避免每次使用`docker`命令都需要`sudo`权限，可以将当前用户添加到`docker`用户组。

![](img/59451dcf14943cb7b0e883332bf11af5_9.png)

![](img/59451dcf14943cb7b0e883332bf11af5_11.png)

![](img/59451dcf14943cb7b0e883332bf11af5_12.png)

以下是配置用户组和验证安装的命令：

![](img/59451dcf14943cb7b0e883332bf11af5_14.png)

![](img/59451dcf14943cb7b0e883332bf11af5_16.png)

```bash
# 将当前用户添加到docker组
sudo usermod -aG docker $USER
newgrp docker

![](img/59451dcf14943cb7b0e883332bf11af5_18.png)

# 验证Docker安装是否成功
docker run hello-world
```

![](img/59451dcf14943cb7b0e883332bf11af5_20.png)

如果看到“Hello from Docker!”的输出，说明Docker已成功安装并可以正常运行。

![](img/59451dcf14943cb7b0e883332bf11af5_22.png)

---

![](img/59451dcf14943cb7b0e883332bf11af5_24.png)

![](img/59451dcf14943cb7b0e883332bf11af5_26.png)

![](img/59451dcf14943cb7b0e883332bf11af5_27.png)

## 获取并配置Ollama服务

![](img/59451dcf14943cb7b0e883332bf11af5_29.png)

![](img/59451dcf14943cb7b0e883332bf11af5_31.png)

![](img/59451dcf14943cb7b0e883332bf11af5_33.png)

OPEA的组件以微服务形式提供。我们将使用其`ollama`组件，该组件将Ollama封装在Docker容器中运行。

![](img/59451dcf14943cb7b0e883332bf11af5_35.png)

![](img/59451dcf14943cb7b0e883332bf11af5_37.png)

![](img/59451dcf14943cb7b0e883332bf11af5_39.png)

首先，从OPEA的GitHub仓库获取`docker-compose.yaml`配置文件。该文件定义了Ollama服务的容器配置。

![](img/59451dcf14943cb7b0e883332bf11af5_41.png)

![](img/59451dcf14943cb7b0e883332bf11af5_43.png)

![](img/59451dcf14943cb7b0e883332bf11af5_45.png)

以下是`docker-compose.yaml`文件的基本内容：

![](img/59451dcf14943cb7b0e883332bf11af5_47.png)

![](img/59451dcf14943cb7b0e883332bf11af5_48.png)

```yaml
version: '3.8'
services:
  ollama-server:
    image: intel/gen-ai-hub-ollama:latest
    container_name: ollama-server
    ports:
      - "8008:11434"
    environment:
      - NO_PROXY=${NO_PROXY:-}
      - HOST_IP=${HOST_IP:-}
      - LLM_MODEL_ID=${LLM_MODEL_ID:-}
    networks:
      - bridge-net
    restart: unless-stopped

![](img/59451dcf14943cb7b0e883332bf11af5_50.png)

![](img/59451dcf14943cb7b0e883332bf11af5_52.png)

![](img/59451dcf14943cb7b0e883332bf11af5_53.png)

networks:
  bridge-net:
    driver: bridge
```

![](img/59451dcf14943cb7b0e883332bf11af5_55.png)

配置文件中有几个关键的环境变量需要设置：
*   `NO_PROXY`: 代理设置，通常在本机运行时留空或设为`localhost`。
*   `HOST_IP`: 宿主机的IP地址，容器需要知道如何被外部访问。
*   `LLM_MODEL_ID`: 要运行的模型标识符，例如`llama3.2:1b`。

![](img/59451dcf14943cb7b0e883332bf11af5_57.png)

我们需要在运行前设置这些环境变量。可以通过在命令行中导出，或者创建一个`.env`文件来管理。

![](img/59451dcf14943cb7b0e883332bf11af5_59.png)

![](img/59451dcf14943cb7b0e883332bf11af5_61.png)

![](img/59451dcf14943cb7b0e883332bf11af5_63.png)

以下是设置环境变量并启动服务的命令：

```bash
# 获取宿主机的IP地址（在WSL/Linux环境下）
export HOST_IP=$(hostname -I | awk '{print $1}')

![](img/59451dcf14943cb7b0e883332bf11af5_65.png)

![](img/59451dcf14943cb7b0e883332bf11af5_67.png)

![](img/59451dcf14943cb7b0e883332bf11af5_69.png)

# 设置其他环境变量
export NO_PROXY=localhost
export LLM_MODEL_ID=llama3.2:1b

![](img/59451dcf14943cb7b0e883332bf11af5_71.png)

![](img/59451dcf14943cb7b0e883332bf11af5_73.png)

![](img/59451dcf14943cb7b0e883332bf11af5_75.png)

# 进入包含docker-compose.yaml文件的目录
cd /path/to/your/opa-comps-folder

# 启动Ollama服务容器
docker compose up -d
```

使用`docker ps`命令可以查看容器是否正在运行。如果看到`ollama-server`容器状态为`Up`，则说明服务已成功启动。

![](img/59451dcf14943cb7b0e883332bf11af5_77.png)

![](img/59451dcf14943cb7b0e883332bf11af5_79.png)

---

![](img/59451dcf14943cb7b0e883332bf11af5_81.png)

![](img/59451dcf14943cb7b0e883332bf11af5_83.png)

## 与Ollama API交互

![](img/59451dcf14943cb7b0e883332bf11af5_85.png)

![](img/59451dcf14943cb7b0e883332bf11af5_87.png)

Ollama服务启动后，会提供一个REST API。默认情况下，在桥接网络模式下，我们可以从宿主机通过映射的端口（本例中为8008）访问容器内的服务。

![](img/59451dcf14943cb7b0e883332bf11af5_89.png)

![](img/59451dcf14943cb7b0e883332bf11af5_91.png)

![](img/59451dcf14943cb7b0e883332bf11af5_92.png)

首先，我们需要通过API将指定的模型拉取（下载）到容器中。

![](img/59451dcf14943cb7b0e883332bf11af5_94.png)

![](img/59451dcf14943cb7b0e883332bf11af5_96.png)

以下是使用`curl`命令拉取模型的示例：

![](img/59451dcf14943cb7b0e883332bf11af5_98.png)

![](img/59451dcf14943cb7b0e883332bf11af5_100.png)

```bash
curl http://localhost:8008/api/pull -d '{
  "model": "llama3.2:1b"
}'
```

![](img/59451dcf14943cb7b0e883332bf11af5_102.png)

命令执行后，终端会显示下载进度。下载完成后，模型就准备就绪了。

接下来，我们可以向模型发送文本生成请求。

![](img/59451dcf14943cb7b0e883332bf11af5_104.png)

![](img/59451dcf14943cb7b0e883332bf11af5_105.png)

以下是使用`curl`命令请求模型生成文本的示例：

```bash
curl http://localhost:8008/api/generate -d '{
  "model": "llama3.2:1b",
  "prompt": "Why is the sky blue?",
  "stream": false
}'
```

![](img/59451dcf14943cb7b0e883332bf11af5_107.png)

API会返回一个JSON响应，其中包含模型生成的答案。虽然原始JSON输出可能不够美观，但这证明了我们的模型服务正在正常工作。

![](img/59451dcf14943cb7b0e883332bf11af5_109.png)

![](img/59451dcf14943cb7b0e883332bf11af5_111.png)

![](img/59451dcf14943cb7b0e883332bf11af5_113.png)

![](img/59451dcf14943cb7b0e883332bf11af5_114.png)

---

![](img/59451dcf14943cb7b0e883332bf11af5_116.png)

![](img/59451dcf14943cb7b0e883332bf11af5_117.png)

## 技术要点与不确定性探讨

![](img/59451dcf14943cb7b0e883332bf11af5_119.png)

在实践过程中，我们遇到并澄清了几个技术问题：

![](img/59451dcf14943cb7b0e883332bf11af5_121.png)

1.  **网络访问**：在Docker的`bridge`网络模式下，通过正确映射端口（如`8008:11434`），宿主机可以直接访问容器内的服务API。
2.  **模型生命周期**：通过`docker-compose.yaml`文件启动服务时，即使设置了`LLM_MODEL_ID`环境变量，容器也不会自动下载模型。必须显式调用`/api/pull`端点。此外，模型默认下载在容器内部，容器停止后数据会丢失。在生产环境中，通常需要挂载持久化存储卷。
3.  **组件角色**：像Ollama这样的“模型服务”组件通常不会直接暴露给最终用户或应用。它们应该被另一个“编排”或“接口”组件（例如OPEA中的`lm`或`llm`组件）所封装，后者负责处理安全、格式化、路由等逻辑，从而实现解耦和灵活性。

---

![](img/59451dcf14943cb7b0e883332bf11af5_123.png)

## 总结

本节课中我们一起学习了OPEA框架的基础实践。我们从安装Docker开始，配置并运行了Ollama模型服务容器，最后通过其原生API完成了模型的拉取和文本生成。这个过程揭示了企业级AI工作负载容器化部署的基本模式：将核心能力（如模型推理）封装为独立的、可网络访问的服务。

然而，直接使用底层模型API并不友好，这引出了我们下节课的内容：如何组合多个OPEA组件，例如在前端添加一个LangChain服务来提供更结构化、更强大的交互能力。