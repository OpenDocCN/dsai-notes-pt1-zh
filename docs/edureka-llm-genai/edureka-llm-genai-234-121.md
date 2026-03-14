# 第二三四部分 121：如何使用OpenCV加载图像 🖼️

![](img/cf88a07aa9e7aba66d15cf1e1ccba58e_1.png)

在本节课中，我们将学习如何使用OpenCV库将图像加载到Python环境中。课程结束时，你将清晰地理解如何将图像加载为NumPy数组，并访问其像素值以便进行后续处理。

![](img/cf88a07aa9e7aba66d15cf1e1ccba58e_2.png)

## 概述

OpenCV是一个功能强大的计算机视觉库。加载图像是进行任何图像处理任务的第一步。本节将指导你完成使用OpenCV加载图像的基本步骤。

## 加载图像的步骤

以下是使用OpenCV加载图像的核心步骤。

### 步骤1：导入库

在开始加载图像之前，我们需要导入必要的库。OpenCV将用于图像处理，而NumPy将用于高效地处理数组。

```python
import cv2
import numpy as np
```

### 步骤2：加载图像

现在，让我们使用`cv2.imread`函数加载一张图像。我们需要将图像文件的路径作为参数指定给该函数。此函数返回一个代表图像的NumPy数组。

```python
image = cv2.imread('path/to/your/image.jpg')
```

### 步骤3：检查图像尺寸

验证已加载图像的尺寸对于理解其结构至关重要。我们可以使用NumPy数组的`shape`属性来检查尺寸。

以下代码片段旨在检查名为`image`的变量是否已成功加载图像。它通过检查`image`是否为`None`来实现。

```python
if image is not None:
    print("图像加载成功")
    print("图像尺寸:", image.shape)
else:
    print("加载图像时出错")
```

如果`image`包含数据（即加载成功），则打印确认消息“图像加载成功”，并通过打印`image.shape`的值来显示图像的尺寸。`image.shape`通常包括高度、宽度和颜色通道数（对于灰度图像，则只有高度和宽度）。

如果`image`为`None`（例如，由于文件路径错误或文件格式不受支持导致图像加载失败），则打印错误消息“加载图像时出错”。

## 显示图像（可选）

在某些环境中，如Google Colab，传统的OpenCV图像显示函数（如`cv2.imshow`）不受支持。以下代码提供了一种在Colab笔记本中内联显示图像的兼容方法。

```python
# 第二三四部分 适用于 Google Colab
from google.colab.patches import cv2_imshow

if image is not None:
    cv2_imshow(image)
else:
    print("图像未加载")
```

如果你使用的是Jupyter Notebook，可以直接导入`cv2.imshow`。

条件语句`if image is not None:`用于验证图像是否已成功加载到变量`image`中。如果加载成功，则调用`cv2_imshow`在笔记本中显示图像。这对于在Colab笔记本中将图像可视化作为数据分析或机器学习工作流的一部分特别有用。

如果图像未正确加载（即`image`为`None`，表明图像加载过程出现问题），则打印“图像未加载”以通知用户。这种方法通过处理图像加载的成功和失败场景，确保了代码的稳健性。

![](img/cf88a07aa9e7aba66d15cf1e1ccba58e_4.png)

## 总结

本节课中，我们一起学习了如何使用OpenCV加载图像并在Python中将其作为NumPy数组进行操作。这些基础知识将作为进行更高级图像处理和计算机视觉任务的基石。

![](img/cf88a07aa9e7aba66d15cf1e1ccba58e_6.png)

感谢你加入本节关于使用OpenCV加载图像的课程。我们希望你觉得内容充实，并期待你开始在Python项目中处理图像。请继续关注后续关于OpenCV的课程。