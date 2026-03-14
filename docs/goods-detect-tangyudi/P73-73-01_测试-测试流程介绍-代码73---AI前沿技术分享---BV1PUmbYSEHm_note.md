# 课程 P73：73.01_测试流程介绍与代码实现 🧪

![](img/c29eafed08b5e9ee1345407ca2fb2599_1.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_3.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_5.png)

在本节课中，我们将学习如何对训练好的目标检测模型进行测试。我们将详细介绍测试流程的每一步，包括数据准备、模型加载、预测以及结果的后处理与可视化，并最终在 Jupyter Notebook 中实现完整的测试代码。

---

## 测试流程概述 📋

![](img/c29eafed08b5e9ee1345407ca2fb2599_7.png)

上一节我们完成了模型的训练，本节中我们来看看如何对训练好的模型进行测试。测试流程与训练流程有相似之处，但也有其独特步骤。

![](img/c29eafed08b5e9ee1345407ca2fb2599_9.png)

测试流程的目标是：完成一个简单的测试，了解如何加载已学习的模型、如何产生预测结果，以及如何处理这些预测结果。

![](img/c29eafed08b5e9ee1345407ca2fb2599_11.png)

以下是测试流程的核心步骤：

![](img/c29eafed08b5e9ee1345407ca2fb2599_13.png)

1.  **准备数据**：输入一张图片。
2.  **图片预处理**：将图片调整至模型所需的输入尺寸。
3.  **加载模型**：加载训练好的权重。
4.  **模型预测**：将处理后的图片输入模型，得到预测输出。
5.  **后处理**：对模型的原始输出进行筛选和调整，得到最终的检测框。
6.  **结果可视化**：使用工具库将带有检测框的图片显示出来。

与训练流程的主要区别在于：
*   **预处理**：测试时只需进行尺寸缩放 (`resize`)，无需数据增强。
*   **后处理**：测试时必须进行，包括通过置信度 (`score`) 筛选、非极大值抑制 (`NMS`) 以及将边界框 (`bbox`) 坐标缩放回原图尺寸。训练时不需要此步骤。

![](img/c29eafed08b5e9ee1345407ca2fb2599_15.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_17.png)

---

## 文件结构准备 📁

![](img/c29eafed08b5e9ee1345407ca2fb2599_19.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_21.png)

为了组织代码，我们需要创建特定的文件结构。我们将在项目根目录下创建一个名为 `test` 的目录。

![](img/c29eafed08b5e9ee1345407ca2fb2599_23.png)

以下是测试所需的文件结构：

```
test/
├── ssd_notebook.ipynb          # 测试流程的主代码文件
└── visualization.py            # 用于结果可视化的辅助代码
```

![](img/c29eafed08b5e9ee1345407ca2fb2599_25.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_27.png)

我们选择在 **Jupyter Notebook** 中编写测试流程，因为它便于我们逐步执行、输入图片、查看中间结果，并直接利用 `matplotlib` 等库可视化最终图片。

![](img/c29eafed08b5e9ee1345407ca2fb2599_29.png)

因此，我们需要：
1.  创建 `test` 文件夹。
2.  将提供的 `visualization.py` 文件复制到该文件夹下。这个文件包含一个针对商品数据集的显示函数。
3.  在 `test` 目录下启动 Jupyter Notebook 并创建新的 notebook 文件，例如命名为 `predict_image.ipynb`。

![](img/c29eafed08b5e9ee1345407ca2fb2599_31.png)

---

![](img/c29eafed08b5e9ee1345407ca2fb2599_33.png)

## 代码实现：逐步构建测试流程 💻

![](img/c29eafed08b5e9ee1345407ca2fb2599_35.png)

现在，我们进入 Jupyter Notebook 环境，开始一步步编写测试代码。

![](img/c29eafed08b5e9ee1345407ca2fb2599_37.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_39.png)

### 第一步：导入必要的模块

![](img/c29eafed08b5e9ee1345407ca2fb2599_41.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_43.png)

首先，我们需要导入所有必需的 Python 模块。为了能调用项目其他目录下的处理模块，我们需要将上级目录添加到系统路径中。

```python
import sys
sys.path.append('../')  # 添加上级目录到路径，以便调用项目模块

![](img/c29eafed08b5e9ee1345407ca2fb2599_45.png)

import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt
from preprocessing import preprocessing_factory
from nets import nets_factory
from notebooks.test import visualization  # 导入可视化工具
```

![](img/c29eafed08b5e9ee1345407ca2fb2599_47.png)

**代码解释**：
*   `sys.path.append('../')`：确保 Python 能在项目根目录下查找和导入我们自定义的模块（如 `preprocessing`, `nets`）。
*   导入 `tensorflow`, `numpy`, `matplotlib` 用于张量操作、数值计算和绘图。
*   从 `preprocessing` 和 `nets` 模块导入工厂类，用于获取预处理函数和网络模型。
*   导入我们准备好的可视化模块。

![](img/c29eafed08b5e9ee1345407ca2fb2599_49.png)

### 第二步：定义数据输入占位符

![](img/c29eafed08b5e9ee1345407ca2fb2599_51.png)

在 TensorFlow 中，我们常使用 `placeholder` 和 `feed_dict` 的方式在运行会话时输入数据。

```python
# 定义输入图片数据的占位符
# 形状为 [None, None, None, 3]，表示批次数、高、宽、通道数均可变
image_input = tf.placeholder(tf.uint8, shape=(None, None, None, 3))
```

![](img/c29eafed08b5e9ee1345407ca2fb2599_53.png)

**代码解释**：
*   `tf.placeholder` 定义了一个数据占位符 `image_input`。
*   类型为 `tf.uint8`，对应图片的像素值范围。
*   形状 `(None, None, None, 3)` 表示：
    *   第一个 `None`：批处理大小（可输入单张或多张图片）。
    *   第二、三个 `None`：图片的高度和宽度（可接受任意尺寸的图片）。
    *   `3`：RGB 三个颜色通道。

![](img/c29eafed08b5e9ee1345407ca2fb2599_55.png)

### 第三步：图片预处理

接下来，我们需要对输入的图片进行预处理，使其符合模型输入要求（例如，缩放到 300x300 像素）。

```python
# 获取预处理函数工厂
preprocessing_fn = preprocessing_factory.get_preprocessing(
    'ssd_vgg_300',  # 模型名称
    is_training=False  # 测试阶段，非训练
)

![](img/c29eafed08b5e9ee1345407ca2fb2599_57.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_59.png)

# 定义网络输入形状
net_shape = (300, 300)  # SSD300 模型的输入尺寸

# 应用预处理函数
# 对于测试，没有标签和边界框，所以传入 None
image_pre, _, _, bbox_img = preprocessing_fn(
    image_input,
    None,  # labels
    None,  # bboxes
    out_shape=net_shape,
    data_format='NHWC'  # 数据格式：批次数、高、宽、通道
)

# 卷积神经网络通常需要4维输入 [batch, height, width, channels]
# 因此需要扩展维度
image_4d = tf.expand_dims(image_pre, axis=0)
```

**代码解释**：
1.  通过 `preprocessing_factory.get_preprocessing` 获取针对 `ssd_vgg_300` 模型在测试模式下的预处理函数 `preprocessing_fn`。
2.  调用该函数处理 `image_input`。由于是测试，没有真实标签和边界框，所以 `labels` 和 `bboxes` 参数传入 `None`。
3.  参数 `out_shape=net_shape` 指定输出图片尺寸为 300x300。
4.  函数返回处理后的图片 `image_pre` 和一个用于后续后处理的参考值 `bbox_img`。
5.  使用 `tf.expand_dims(image_pre, axis=0)` 将图片从 `[height, width, channels]` 扩展为 `[1, height, width, channels]`，添加了批次维度。

![](img/c29eafed08b5e9ee1345407ca2fb2599_61.png)

### 第四步：加载模型并进行预测

![](img/c29eafed08b5e9ee1345407ca2fb2599_63.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_65.png)

现在，我们需要加载 SSD 网络模型，并将预处理后的图片输入模型以得到预测结果。

![](img/c29eafed08b5e9ee1345407ca2fb2599_67.png)

```python
# 获取网络工厂
network_fn = nets_factory.get_network_fn(
    'ssd_vgg_300',  # 模型名称
    num_classes=8 + 1,  # 商品数据集类别数 (8类商品 + 1个背景类)
    is_training=False  # 测试模式
)

# 在 Jupyter Notebook 中运行时，需要处理变量重用问题
# 第一次运行为创建新变量，后续运行需重用已创建的变量
reuse = True if 'ssd_net' in locals() else False

# 使用 slim.arg_scope 设置网络层的公共参数（如数据格式）
with tf.contrib.slim.arg_scope(network_fn.arg_scope(data_format='NHWC')):
    # 将图片输入网络，得到预测结果
    predictions, localizations, _, _ = network_fn(
        image_4d,
        is_training=False,
        reuse=reuse
    )
```

**代码解释**：
1.  通过 `nets_factory.get_network_fn` 获取 SSD 网络构造函数 `network_fn`。关键参数 `num_classes` 必须设置为商品数据集的类别数（8类商品 + 1个背景类 = 9）。
2.  `reuse` 逻辑：在 Jupyter Notebook 中，如果重复运行定义网络的单元格，TensorFlow 会报错，因为变量已存在。此逻辑判断如果 `'ssd_net'` 已存在，则设置 `reuse=True` 以重用变量。
3.  `tf.contrib.slim.arg_scope` 用于为网络内部的许多层函数统一设置默认参数，这里统一指定数据格式为 `'NHWC'`。
4.  调用 `network_fn`，输入预处理后的图片 `image_4d`，并指定 `is_training=False` 和 `reuse` 参数。函数返回的 `predictions` 是类别预测置信度，`localizations` 是预测的边界框偏移量。

### 第五步：后处理与结果可视化

![](img/c29eafed08b5e9ee1345407ca2fb2599_69.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_71.png)

模型输出的 `predictions` 和 `localizations` 是原始数据，包含了大量默认框（anchor）的预测信息。我们需要通过后处理来筛选出最终的、有意义的检测结果。

后处理通常包括：
*   **置信度筛选**：过滤掉置信度低于阈值的预测。
*   **非极大值抑制 (NMS)**：去除对同一物体的重复检测框。
*   **坐标转换与缩放**：将预测的边界框坐标从网络输出格式转换并缩放到原始输入图片的尺寸。

![](img/c29eafed08b5e9ee1345407ca2fb2599_73.png)

这部分代码逻辑相对复杂，会用到 SSD 算法中定义的解码和筛选函数。为了课程清晰，我们在此概述步骤，具体函数调用请参考项目提供的 `visualization.py` 或后续教程。

![](img/c29eafed08b5e9ee1345407ca2fb2599_75.png)

```python
# 注意：此处为逻辑示意，具体实现需调用项目中的后处理函数
# 1. 解码：将 localizations 和 anchor 信息结合，得到图像上的绝对坐标。
# 2. 筛选：根据 predictions 的置信度进行阈值筛选。
# 3. NMS：对筛选后的框进行非极大值抑制。
# 4. 缩放：将框的坐标从网络输入尺寸(300x300)缩放到原始图片尺寸。

![](img/c29eafed08b5e9ee1345407ca2fb2599_77.png)

# 假设我们有一个后处理函数 `postprocess`
final_boxes, final_scores, final_labels = postprocess(
    predictions, localizations, bbox_img, original_image_shape
)

![](img/c29eafed08b5e9ee1345407ca2fb2599_79.png)

# 使用 visualization.py 中的函数显示结果
visualization.plt_bboxes(original_image, final_boxes, final_labels, final_scores)
```

![](img/c29eafed08b5e9ee1345407ca2fb2599_81.png)

**流程总结**：
1.  开启一个 TensorFlow 会话 (`tf.Session`)。
2.  在会话中，运行之前定义好的计算图。需要向 `image_input` 这个占位符 `feed` 一张真实的图片数据（例如，通过 `cv2.imread` 读取）。
3.  运行的计算节点应包括：预处理后的图片 `image_4d`、网络预测 `predictions` 和 `localizations`。
4.  将运行得到的原始预测结果，送入后处理流程，得到最终的边界框、置信度和类别标签。
5.  最后，调用可视化函数，将原始图片和绘制好的检测框一起显示出来。

---

## 总结 🎯

![](img/c29eafed08b5e9ee1345407ca2fb2599_83.png)

![](img/c29eafed08b5e9ee1345407ca2fb2599_85.png)

本节课中，我们一起学习了目标检测模型的完整测试流程。

我们首先**概述了测试的步骤**，明确了其与训练流程的异同。接着，我们**准备了测试环境的文件结构**。最后，我们**在 Jupyter Notebook 中逐步实现了测试代码**，涵盖了数据输入、预处理、模型加载与预测等核心环节。

![](img/c29eafed08b5e9ee1345407ca2fb2599_87.png)

通过本课的学习，你应该能够理解如何将训练好的模型应用于新图片，并得到可视化的检测结果。虽然后处理的具体代码未完全展开，但你已经掌握了测试流程的主干和关键概念。在后续实践中，你可以进一步完善后处理部分，并尝试用自己训练的模型检测不同的图片。