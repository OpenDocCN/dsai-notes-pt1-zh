# 课程P46：第二阶段总结 🧠

![](img/d58ebb1ce4eabce428b04decafeccf4b_1.png)

![](img/d58ebb1ce4eabce428b04decafeccf4b_3.png)

在本节课中，我们将对第二阶段——数据集处理的核心内容进行总结。我们将回顾从数据获取、格式转换到最终读取的完整流程，并梳理其中的关键概念与代码实现。

## 数据集处理概述

上一节我们介绍了数据集处理的完整代码逻辑。本节中，我们来看看整个阶段的要点总结。

数据集处理阶段主要涉及如何获取数据以及需要获取哪些数据。其核心目标是准备用于模型训练的规范化数据。

## 数据集种类与格式

我们介绍了常见的数据集种类。

![](img/d58ebb1ce4eabce428b04decafeccf4b_5.png)

以下是两种典型的数据集：
*   **VOC数据集**：提供XML格式的标注文件以及图片。
*   **ImageNet V4数据集**：通常提供JSON等多种格式的标注文件，结构更为复杂。

![](img/d58ebb1ce4eabce428b04decafeccf4b_7.png)

## 数据标记

![](img/d58ebb1ce4eabce428b04decafeccf4b_9.png)

标记数据集的目的是确定需要标注的数据类别和内容。这通常由产品、业务或数据标注团队决定，而非开发者。

数据标记后，对于目标检测任务，通常会生成**XML标注文件**和对应的**图片文件**。

![](img/d58ebb1ce4eabce428b04decafeccf4b_11.png)

## 格式转换的目的与原因

![](img/d58ebb1ce4eabce428b04decafeccf4b_13.png)

![](img/d58ebb1ce4eabce428b04decafeccf4b_15.png)

我们进行数据集格式转换，主要是为了提供结构更简单、更高效的数据给训练过程或其他用户使用。

![](img/d58ebb1ce4eabce428b04decafeccf4b_17.png)

我们采用的转换格式是基于 **Protobuf协议** 的 **TFRecords文件**。这种格式的特点是**速度快、体积小**，是跨平台和GRPC传输中常用的协议。

![](img/d58ebb1ce4eabce428b04decafeccf4b_19.png)

## 存储为TFRecords文件

![](img/d58ebb1ce4eabce428b04decafeccf4b_21.png)

![](img/d58ebb1ce4eabce428b04decafeccf4b_23.png)

在转换格式时，我们选择将数据存储到TFRecords文件中。通常，每个文件会指定一个固定大小，以避免文件过大。

![](img/d58ebb1ce4eabce428b04decafeccf4b_25.png)

以下是存储过程的关键步骤：

![](img/d58ebb1ce4eabce428b04decafeccf4b_27.png)

![](img/d58ebb1ce4eabce428b04decafeccf4b_29.png)

**1. 读取数据**
*   读取图片使用：`tf.gfile.GFile`
*   读取XML使用：`ET` 工具

![](img/d58ebb1ce4eabce428b04decafeccf4b_31.png)

**2. 封装与写入**
使用 `tf.train.Example` 协议将数据封装起来。封装的原则是**尽可能多地存储与图片相关的信息**。封装后，使用 `write` 方法将序列化的结果写入文件。

![](img/d58ebb1ce4eabce428b04decafeccf4b_33.png)

![](img/d58ebb1ce4eabce428b04decafeccf4b_35.png)

**3. 关键处理：边界框归一化**
在存储过程中，有一个必须注意的操作：将标注的边界框（Bounding Box）坐标进行**归一化处理**（即除以图片的长和宽）。这样处理后的数据可以直接提供给后续模型使用。

![](img/d58ebb1ce4eabce428b04decafeccf4b_37.png)

## 读取TFRecords文件

![](img/d58ebb1ce4eabce428b04decafeccf4b_39.png)

![](img/d58ebb1ce4eabce428b04decafeccf4b_41.png)

数据存储后，我们需要从中读取以供训练。读取TFRecords文件的流程分为两步：

![](img/d58ebb1ce4eabce428b04decafeccf4b_43.png)

![](img/d58ebb1ce4eabce428b04decafeccf4b_45.png)

**1. 准备数据集规范**
使用 `tf.data.TFRecordDataset` API来准备数据集的规范信息。相关的参数和函数都在此API中定义。

![](img/d58ebb1ce4eabce428b04decafeccf4b_47.png)

**2. 获取数据**
通过 `provider` 工具中的 `get` 方法来获取数据。这个过程包括准备数据集对象和解码（decode）数据。

![](img/d58ebb1ce4eabce428b04decafeccf4b_49.png)

![](img/d58ebb1ce4eabce428b04decafeccf4b_51.png)

## 总结

![](img/d58ebb1ce4eabce428b04decafeccf4b_53.png)

本节课中，我们一起学习了数据集处理第二阶段的完整总结。我们回顾了：
1.  常见的数据集种类（如VOC、ImageNet）及其格式。
2.  数据标记的职责归属。
3.  将原始数据转换为 **TFRecords文件** 的目的、原因及具体步骤，包括数据读取、`tf.train.Example`封装、边界框归一化等关键操作。
4.  从TFRecords文件中读取数据的两步流程：准备数据集规范和通过provider获取数据。

![](img/d58ebb1ce4eabce428b04decafeccf4b_55.png)

![](img/d58ebb1ce4eabce428b04decafeccf4b_57.png)

至此，数据集处理阶段的核心内容已全部梳理完毕。