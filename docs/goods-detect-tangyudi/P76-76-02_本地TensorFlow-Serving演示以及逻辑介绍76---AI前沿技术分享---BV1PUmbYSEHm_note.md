# 课程 P76：TensorFlow Serving 本地演示与逻辑介绍 🚀

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_1.png)

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_3.png)

在本节课中，我们将学习如何使用 TensorFlow Serving 在生产环境中部署机器学习模型。我们将通过一个完整的本地演示，了解从模型导出、服务启动到客户端调用的全流程。

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_5.png)

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_7.png)

## 概述

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_9.png)

TensorFlow Serving 是一个专为生产环境设计的高性能服务系统。它的核心目标是实现模型生产者与使用者之间的解耦，使得模型部署和调用过程标准化、高效化。

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_11.png)

## 本地演示流程

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_13.png)

上一节我们概述了 TensorFlow Serving 的作用，本节中我们来看看一个完整的本地部署与调用演示。

以下是演示的核心步骤：

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_15.png)

1.  **导出模型**：首先，我们需要一个训练好的模型，并将其导出为 TensorFlow Serving 能够识别的格式。
2.  **启动服务**：使用 Docker 在本地启动 TensorFlow Serving 服务，并加载我们导出的模型。
3.  **客户端调用**：编写客户端代码，向运行中的服务发送请求，并接收模型的预测结果。

### 第一步：导出模型

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_17.png)

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_19.png)

我们使用 `tf.saved_model.builder.SavedModelBuilder` 模块来导出模型。导出的模型结构应尽可能简洁，其职责是接收符合要求的输入数据，并直接输出预测结果。

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_21.png)

**核心设计原则**：模型服务本身不处理复杂的业务逻辑（如数据预处理）。这些工作应在客户端或调用链的其他环节完成。

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_23.png)

### 第二步：启动服务

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_25.png)

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_27.png)

我们使用 Docker 命令来启动服务。命令的关键是指定模型在本地文件系统中的绝对路径。

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_29.png)

启动命令示例：
```bash
docker run -p 8501:8501 \
  --mount type=bind,source=/path/to/your/model,target=/models/commodity \
  -e MODEL_NAME=commodity -t tensorflow/serving
```
此命令将本地路径 `/path/to/your/model` 下的模型挂载到容器内的 `/models/commodity`，并以 `commodity` 为模型名启动服务。

### 第三步：客户端调用

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_31.png)

服务启动后，我们可以通过客户端代码向其发起预测请求。客户端代码会读取一张图片，将其转换为模型所需的输入格式，发送给服务端，并解析返回的预测结果。

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_33.png)

## TensorFlow Serving 架构介绍

了解了完整流程后，我们深入看一下 TensorFlow Serving 的架构逻辑。

它的工作流程清晰地体现在其架构图中：

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_35.png)

1.  **模型训练与导出**：开发者使用数据训练模型，并通过 SavedModel 格式导出。
2.  **服务部署**：将导出的模型部署到 TensorFlow Serving 服务器上。
3.  **客户端请求**：客户端应用程序向服务器发送预测请求（提交数据）。
4.  **结果返回**：服务器运行模型进行计算，并将预测结果返回给客户端。

这个过程实现了模型与应用的分离，便于模型的独立更新和管理。

## 模型导出代码结构

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_37.png)

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_39.png)

现在，我们具体看看如何用代码实现模型导出。

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_41.png)

以下是导出模型的关键代码结构：

1.  **定义模型输入输出**：明确模型的输入张量（如 `placeholder`）和输出张量。这决定了客户端需要提供什么格式的数据。
    *   **输入**：模型算法所要求的张量（例如，归一化后的图像张量 `[None, height, width, channels]`）。
    *   **输出**：模型的预测结果张量。

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_43.png)

2.  **构建 SavedModel**：使用 `SavedModelBuilder` 来构建并保存模型。
    *   定义模型导出的目标路径。
    *   创建一个 `signature_def`（签名定义），它是一份“协议”，明确指定了服务的输入、输出以及调用方法（例如 `predict`）。
    *   将包含计算图和变量信息的模型，连同定义好的签名，保存到指定目录。

通过这两步，我们就得到了一个可以被 TensorFlow Serving 加载并提供服务的模型文件。

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_45.png)

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_47.png)

## 总结

![](img/1e4ad782ae2c8f6b1c9ac40cf440df45_49.png)

本节课中我们一起学习了 TensorFlow Serving 的核心概念与本地部署流程。我们了解到 TensorFlow Serving 是一个用于生产环境模型服务的系统，并通过演示掌握了**导出模型**、**使用Docker启动服务**以及**编写客户端进行调用**的三个关键步骤。记住，良好的模型服务设计应保持模型本身的纯粹性，将业务处理逻辑置于服务之外。