# 课程 P41：格式转换：图片数据以及XML读取 🖼️📄

![](img/6aaa25960e8f12f4d7ede28718c8df15_1.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_3.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_5.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_7.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_9.png)

在本节课中，我们将学习如何将图片数据和对应的XML标注文件读取并处理，为后续封装成TFRecord格式做准备。我们将重点关注如何从XML文件中提取关键信息，并对边界框坐标进行归一化处理。

![](img/6aaa25960e8f12f4d7ede28718c8df15_11.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_13.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_15.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_17.png)

## 概述

![](img/6aaa25960e8f12f4d7ede28718c8df15_19.png)

上一节我们介绍了将数据集分割并写入多个TFRecord文件的整体逻辑。本节中，我们来看看如何具体处理每一张图片及其对应的XML文件，提取必要信息，以便封装成一个`tf.train.Example`。

![](img/6aaa25960e8f12f4d7ede28718c8df15_21.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_23.png)

## 核心逻辑与函数定义

![](img/6aaa25960e8f12f4d7ede28718c8df15_25.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_27.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_29.png)

我们的核心任务是为每一张图片执行以下三步操作：
1.  读取图片的二进制数据及其XML文件内容。
2.  将提取的信息封装成一个`tf.train.Example`。
3.  使用`TFRecordWriter`将序列化后的`Example`写入文件。

![](img/6aaa25960e8f12f4d7ede28718c8df15_31.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_33.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_35.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_37.png)

我们首先定义一个函数 `add_to_tfrecord` 来实现这个流程。

![](img/6aaa25960e8f12f4d7ede28718c8df15_39.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_41.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_43.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_45.png)

```python
def add_to_tfrecord(output_filename, image_name, tfrecord_writer):
    """
    添加一个图片文件内容以及XML内容写入到TFRecord文件当中。
    参数:
        output_filename: 输出文件路径。
        image_name: 图片编号/名称。
        tfrecord_writer: TFRecord文件写入实例。
    """
    # 1. 读取图片数据及XML内容
    # 2. 将图片封装成Example
    # 3. 写入Example的序列化结果
    return None
```

![](img/6aaa25960e8f12f4d7ede28718c8df15_47.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_49.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_51.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_53.png)

## 第一步：读取图片与XML内容

![](img/6aaa25960e8f12f4d7ede28718c8df15_55.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_57.png)

以下是实现第一步“读取图片数据及XML内容”的详细步骤。我们将这些步骤封装在一个独立的函数 `process_image` 中。

![](img/6aaa25960e8f12f4d7ede28718c8df15_59.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_61.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_63.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_65.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_67.png)

### 1. 读取图片二进制数据
使用TensorFlow的 `gfile` 模块以二进制模式读取图片文件。

![](img/6aaa25960e8f12f4d7ede28718c8df15_69.png)

```python
import tensorflow as tf
image_filename = f'{dataset_dir}/JPEGImages/{image_name}.jpg'
with tf.gfile.FastGFile(image_filename, 'rb') as f:
    image_data = f.read()
```

### 2. 解析XML文件
我们将使用Python的 `xml.etree.ElementTree` 库来解析XML标注文件，提取图片尺寸、物体类别、边界框等信息。

![](img/6aaa25960e8f12f4d7ede28718c8df15_71.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_73.png)

```python
import xml.etree.ElementTree as ET

![](img/6aaa25960e8f12f4d7ede28718c8df15_75.png)

xml_filename = f'{dataset_dir}/Annotations/{image_name}.xml'
tree = ET.parse(xml_filename)
root = tree.getroot()
```

![](img/6aaa25960e8f12f4d7ede28718c8df15_77.png)

### 3. 提取关键信息
我们需要从XML树结构中提取以下信息：
*   **图片尺寸**：宽度、高度、通道数。
*   **物体信息**：每个物体的类别名称、对应的数字编号、难度标志（difficult）、截断标志（truncated）以及边界框坐标。

![](img/6aaa25960e8f12f4d7ede28718c8df15_79.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_81.png)

以下是提取这些信息的代码逻辑：

![](img/6aaa25960e8f12f4d7ede28718c8df15_83.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_85.png)

```python
# 获取图片尺寸
size = root.find('size')
shape = [
    int(size.find('height').text),
    int(size.find('width').text),
    int(size.find('depth').text)
]

# 初始化列表，用于存储所有物体的信息
labels = []          # 物体类别对应的数字编号
labels_text = []     # 物体类别名称（字符串）
difficult = []       # 难度标志列表
truncated = []       # 截断标志列表
bboxes = []          # 边界框坐标列表

# 遍历XML中的所有‘object’节点
for obj in root.findall('object'):
    # 获取类别名称
    label_name = obj.find('name').text
    labels_text.append(label_name)

    # 将类别名称映射为数字编号（此处需要一个预定义的映射字典，例如 VOC_LABELS）
    labels.append(VOC_LABELS[label_name][0])

    # 提取difficult标志，若不存在则默认为0
    isdifficult = obj.find('difficult')
    difficult.append(0 if isdifficult is None else int(isdifficult.text))

    # 提取truncated标志，若不存在则默认为0
    istruncated = obj.find('truncated')
    truncated.append(0 if istruncated is None else int(istruncated.text))

    # 提取边界框坐标 (xmin, ymin, xmax, ymax)
    bbox = obj.find('bndbox')
    # 注意：为了与后续模型计算兼容，我们存储顺序为 (ymin, xmin, ymax, xmax)
    # 并对坐标进行归一化（除以图片高度或宽度），使其值在0~1之间。
    ymin = float(bbox.find('ymin').text) / shape[0]
    xmin = float(bbox.find('xmin').text) / shape[1]
    ymax = float(bbox.find('ymax').text) / shape[0]
    xmax = float(bbox.find('xmax').text) / shape[1]

    bboxes.append([ymin, xmin, ymax, xmax])
```

![](img/6aaa25960e8f12f4d7ede28718c8df15_87.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_89.png)

**核心概念解释：边界框归一化**
*   **公式**：`坐标值 / 图片对应维度尺寸`
*   **目的**：将边界框的绝对坐标转换为相对于图片尺寸的比例值（0到1之间）。这样做的好处是使坐标值与图片的具体分辨率解耦，便于模型进行统一的回归计算，是目标检测任务中的常见预处理步骤。

## 第二步：封装为TFRecord Example

在获取了所有必要信息（`image_data`, `shape`, `labels`, `labels_text`, `difficult`, `truncated`, `bboxes`）后，下一步就是将它们按照`tf.train.Example`要求的`BytesList`、`Int64List`、`FloatList`格式进行封装。这部分代码将在下一节详细展开。

![](img/6aaa25960e8f12f4d7ede28718c8df15_91.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_93.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_95.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_97.png)

## 总结

![](img/6aaa25960e8f12f4d7ede28718c8df15_99.png)

本节课中我们一起学习了处理VOC数据集中单张图片及其XML标注文件的核心流程。我们实现了：
1.  使用 `tf.gfile` 读取图片二进制数据。
2.  使用 `xml.etree.ElementTree` 解析XML文件，并提取了图片尺寸、物体类别、边界框等关键信息。
3.  理解了**边界框坐标归一化**的重要性及其计算方法。

![](img/6aaa25960e8f12f4d7ede28718c8df15_101.png)

![](img/6aaa25960e8f12f4d7ede28718c8df15_103.png)

这些处理后的数据已经准备好，可以在下一环节被封装成标准的 `tf.train.Example` 格式，并写入TFRecord文件，从而构建高效、易于读取的数据管道。