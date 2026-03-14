# 第一部分 71：展平层 📏

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_0.png)

在本节课中，我们将要学习神经网络中的一个关键预处理步骤——展平层。我们将了解它如何将多维数据转换为一维向量，以便后续的全连接层能够进行处理。

---

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_2.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_3.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_4.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_5.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_6.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_7.png)

上一节我们讨论了卷积和池化操作，它们输出的通常是多维的特征图。本节中我们来看看如何将这些特征图“展平”，为最终的分类任务做准备。

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_8.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_9.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_10.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_11.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_12.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_13.png)

展平层是神经网络中的一种特定层，用于将多维数据（例如池化后的特征图）转换为一维线性向量。这种转换至关重要，因为它将数据准备成适合输入到网络后续层（特别是全连接层）的格式。全连接层要求输入数据是向量格式。

在构建分类模型时，经过卷积和池化处理后的数据，需要以一维线性向量的格式输入到全连接层。这些全连接层负责基于提取的特征进行预测或分类。

以下是展平过程的核心作用：
*   **转换格式**：将具有多个维度（如高度、宽度、通道数）的池化特征图转换为单一维度的数组。
*   **保持关系**：这种转换在准备数据以供进一步处理的同时，保留了特征之间的空间关系。
*   **衔接网络**：使卷积神经网络（CNN）的特征提取部分能够与传统的全连接分类器部分顺利连接。

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_15.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_16.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_17.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_18.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_19.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_20.png)

具体来说，展平过程涉及将池化特征图中的每个元素，按照特定的顺序（通常是逐行、逐通道）排列成一个长向量。假设我们有一个池化后的特征图，其维度为 `(height, width, channels)`。经过展平层后，它将变成一个长度为 `height * width * channels` 的一维向量。

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_21.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_22.png)

用公式可以表示为：
**`Flattened_Vector = reshape(Pooled_Feature_Maps, [-1, height * width * channels])`**
或者用简单的伪代码描述这个过程：
```python
# 第一部分 假设 pooled_features 是一个三维数组，形状为 (4, 4, 32)
flattened_output = pooled_features.reshape(-1, 4 * 4 * 32)
# 第一部分 现在 flattened_output 是一个形状为 (1, 512) 的一维向量
```

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_24.png)

---

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_25.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_26.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_27.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_28.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_29.png)

![](img/f3d93435cdd2a069c33b0ab8d6e50feb_30.png)

本节课中我们一起学习了神经网络内部的一个关键预处理步骤，特别是如何将卷积层输出的多维数据转换为单一的线性向量，从而实现与后续网络层的无缝集成。这个过程被称为数据展平，它确保了特征图的高效处理，并有助于构建有效的分类模型。