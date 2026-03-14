# 课程P33：3-模板处理方法 📄➡️🔢

在本节课中，我们将学习如何从一张包含数字的图像中提取并处理数字模板。核心步骤包括轮廓检测、轮廓排序以及根据排序结果提取每个数字的图像区域，最终构建一个数字到其模板图像的映射字典。

---

![](img/0cd5d45eeeba1d40859642f0ad9218c1_1.png)

## 轮廓检测与提取 🔍

![](img/0cd5d45eeeba1d40859642f0ad9218c1_3.png)

![](img/0cd5d45eeeba1d40859642f0ad9218c1_5.png)

上一节我们介绍了图像预处理，本节中我们来看看如何检测并提取图像中的数字轮廓。

![](img/0cd5d45eeeba1d40859642f0ad9218c1_7.png)

首先，对输入图像进行轮廓检测。需要使用 `cv2.findContours` 函数，并指定参数为只检测最外层轮廓，因为内层轮廓对于计算数字的外接矩形没有用处。

```python
contours, _ = cv2.findContours(image_copy, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
```

![](img/0cd5d45eeeba1d40859642f0ad9218c1_9.png)

以下是关键参数说明：
*   `cv2.RETR_EXTERNAL`：表示只检测外轮廓。
*   `cv2.CHAIN_APPROX_SIMPLE`：压缩轮廓的坐标点，节省内存。

![](img/0cd5d45eeeba1d40859642f0ad9218c1_11.png)

执行后，我们获得了图像中所有的轮廓。可以将其绘制出来以验证结果。

![](img/0cd5d45eeeba1d40859642f0ad9218c1_13.png)

```python
cv2.drawContours(show_img, contours, -1, (0, 255, 0), 2)
```

![](img/0cd5d45eeeba1d40859642f0ad9218c1_15.png)

绘制结果显示共有10个轮廓，与图像中的数字数量（0-9）一致，说明检测正确。

![](img/0cd5d45eeeba1d40859642f0ad9218c1_17.png)

---

## 轮廓排序 📊

![](img/0cd5d45eeeba1d40859642f0ad9218c1_19.png)

虽然检测到了10个轮廓，但它们的顺序并不一定对应数字0到9的顺序。因此，我们需要根据每个轮廓在图像中的水平位置（从左到右）进行排序。

我们定义一个工具函数来实现排序。其核心思想是：
1.  计算每个轮廓的最小外接矩形。
2.  获取每个矩形左上角的X坐标。
3.  根据X坐标的大小对所有轮廓进行排序。

![](img/0cd5d45eeeba1d40859642f0ad9218c1_21.png)

![](img/0cd5d45eeeba1d40859642f0ad9218c1_23.png)

```python
def sort_contours(contours):
    bounding_boxes = [cv2.boundingRect(c) for c in contours]  # 计算外接矩形
    (contours, bounding_boxes) = zip(*sorted(zip(contours, bounding_boxes), key=lambda b: b[1][0]))  # 按X坐标排序
    return contours, bounding_boxes
```

![](img/0cd5d45eeeba1d40859642f0ad9218c1_25.png)

排序完成后，`contours` 列表中的第一个轮廓就对应数字“0”，第二个对应数字“1”，依此类推。

![](img/0cd5d45eeeba1d40859642f0ad9218c1_27.png)

---

![](img/0cd5d45eeeba1d40859642f0ad9218c1_29.png)

![](img/0cd5d45eeeba1d40859642f0ad9218c1_31.png)

## 构建模板字典 🗂️

![](img/0cd5d45eeeba1d40859642f0ad9218c1_33.png)

上一节我们完成了轮廓排序，本节中我们来看看如何根据排序后的轮廓提取每个数字的图像并构建模板字典。

![](img/0cd5d45eeeba1d40859642f0ad9218c1_35.png)

现在，我们已经有了按顺序排列的轮廓。接下来需要遍历这些轮廓，从原始图像中抠出每个数字所在的区域，并将其与对应的数字索引（0-9）关联起来。

![](img/0cd5d45eeeba1d40859642f0ad9218c1_37.png)

以下是实现步骤：

![](img/0cd5d45eeeba1d40859642f0ad9218c1_39.png)

1.  **初始化字典**：创建一个空字典，用于存储数字和其模板图像的对应关系。
2.  **遍历轮廓**：使用 `enumerate` 同时获取轮廓索引 `i`（即数字值）和轮廓本身 `c`。
3.  **提取区域**：计算当前轮廓的外接矩形 `(x, y, w, h)`，利用切片操作从原始图像中抠出该矩形区域。
4.  **调整大小**：将抠出的图像调整到一个统一、合适的大小。
5.  **存入字典**：将数字索引 `i` 作为键，调整大小后的图像作为值，存入字典。

```python
digits = {}  # 初始化模板字典

![](img/0cd5d45eeeba1d40859642f0ad9218c1_41.png)

for (i, c) in enumerate(sorted_contours):
    (x, y, w, h) = cv2.boundingRect(c)  # 获取外接矩形
    roi = ref[y:y + h, x:x + w]  # 抠出数字区域
    roi = cv2.resize(roi, (57, 88))  # 调整图像大小
    digits[i] = roi  # 存入字典
```

执行完循环后，`digits` 字典中就完整地包含了从0到9每个数字的模板图像。

---

![](img/0cd5d45eeeba1d40859642f0ad9218c1_43.png)

## 总结 📝

![](img/0cd5d45eeeba1d40859642f0ad9218c1_45.png)

本节课中我们一起学习了模板处理的全过程。
1.  首先，我们使用轮廓检测函数定位图像中的所有数字。
2.  接着，通过计算外接矩形并按其X坐标排序，确保了轮廓顺序与数字大小顺序一致。
3.  最后，遍历排序后的轮廓，提取每个数字的图像区域并存入字典，成功构建了数字模板库。

![](img/0cd5d45eeeba1d40859642f0ad9218c1_47.png)

![](img/0cd5d45eeeba1d40859642f0ad9218c1_49.png)

至此，模板处理阶段完成。我们获得了一个结构清晰的 `digits` 字典，为后续的模板匹配识别任务打下了基础。