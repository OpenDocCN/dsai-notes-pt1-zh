# 课程 P22：轮廓特征与近似 📐

在本节课中，我们将学习如何计算轮廓的特征，例如面积和周长，并探索轮廓近似的方法。这些技术对于从图像中提取结构化信息和简化形状表示至关重要。

![](img/8fe87be69dd7f94e92a5044796fecb8f_1.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_3.png)

## 轮廓特征计算

![](img/8fe87be69dd7f94e92a5044796fecb8f_5.png)

上一节我们介绍了如何检测和绘制轮廓。本节中，我们来看看如何计算轮廓的几何特征。OpenCV 提供了多个函数来计算这些特征，但在使用前，需要将具体的轮廓提取出来。

![](img/8fe87be69dd7f94e92a5044796fecb8f_7.png)

以下是计算轮廓面积和周长的核心方法：

*   **计算面积**：使用 `cv2.contourArea(contour)` 函数，传入一个轮廓即可得到其面积。
*   **计算周长**：使用 `cv2.arcLength(contour, closed)` 函数。参数 `closed` 通常设为 `True`，表示轮廓是闭合的。

```python
# 假设 contours 是之前检测到的轮廓列表
cnt = contours[0]  # 提取第一个轮廓
area = cv2.contourArea(cnt)  # 计算面积
perimeter = cv2.arcLength(cnt, True)  # 计算周长
```

## 轮廓近似原理

有时，检测到的轮廓可能包含许多不规则的“毛刺”。轮廓近似（Contour Approximation）的目标是用更少、更规则的线段来近似原始轮廓，从而简化其形状。

其基本原理是 **Douglas-Peucker 算法**，过程可以概括为：
1.  连接曲线（轮廓）的起点 A 和终点 B，得到一条直线段 AB。
2.  在原始曲线上找到离直线段 AB 距离最远的点 C。
3.  如果点 C 到直线 AB 的距离 **小于** 设定的阈值（T），则认为直线 AB 足以近似整条曲线。
4.  如果该距离 **大于** 阈值 T，则算法会递归处理。即，分别用直线 AC 近似曲线 AC 部分，用直线 CB 近似曲线 CB 部分，并重复步骤 2 和 3 的判断。
5.  递归过程持续进行，直到所有曲线段都能用直线段在阈值范围内近似为止。

最终，复杂的曲线被一系列首尾相连的直线段所替代。

![](img/8fe87be69dd7f94e92a5044796fecb8f_9.png)

## 轮廓近似实践

![](img/8fe87be69dd7f94e92a5044796fecb8f_11.png)

了解了原理后，我们来看看如何在 OpenCV 中实现轮廓近似。以下是一个完整的示例流程：

![](img/8fe87be69dd7f94e92a5044796fecb8f_13.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_15.png)

```python
import cv2

# 1. 读取图像并转为灰度图
img = cv2.imread('shape.png')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

![](img/8fe87be69dd7f94e92a5044796fecb8f_17.png)

# 2. 二值化处理
ret, thresh = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)

![](img/8fe87be69dd7f94e92a5044796fecb8f_19.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_21.png)

# 3. 查找轮廓
contours, hierarchy = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)

![](img/8fe87be69dd7f94e92a5044796fecb8f_23.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_25.png)

# 4. 提取一个轮廓并进行近似
cnt = contours[0]  # 以第一个轮廓为例
epsilon = 0.01 * cv2.arcLength(cnt, True)  # 设置阈值为周长的 1%
approx = cv2.approxPolyDP(cnt, epsilon, True)  # 进行轮廓近似

![](img/8fe87be69dd7f94e92a5044796fecb8f_27.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_29.png)

# 5. 绘制近似后的轮廓
# 注意：approx 本身也是一个轮廓，需要重新绘制
img_approx = img.copy()
cv2.drawContours(img_approx, [approx], -1, (0, 255, 0), 3)
cv2.imshow('Approximated Contour', img_approx)
cv2.waitKey(0)
```

![](img/8fe87be69dd7f94e92a5044796fecb8f_31.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_33.png)

**参数 `epsilon` 的影响**：
*   `epsilon` 值越小，近似结果越接近原始轮廓，细节保留越多。
*   `epsilon` 值越大，近似结果越简化，可能变成一个三角形甚至一条直线。
*   通常，`epsilon` 被设置为轮廓周长的一个百分比（如 0.01 * 周长），需要根据实际需求调整。

![](img/8fe87be69dd7f94e92a5044796fecb8f_35.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_37.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_39.png)

## 轮廓的外接形状

除了近似，我们还可以为轮廓计算外接的几何形状，这有助于获取轮廓的包围框或拟合形状，进而计算更多特征（如面积比）。

以下是计算外接矩形和外接圆的方法：

![](img/8fe87be69dd7f94e92a5044796fecb8f_41.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_43.png)

*   **外接矩形（Bounding Rectangle）**：`cv2.boundingRect(cnt)` 返回一个矩形的 (x, y, w, h)，即左上角坐标和宽高。
*   **最小外接矩形（Rotated Rectangle）**：`cv2.minAreaRect(cnt)` 返回一个可以旋转的矩形。
*   **外接圆**：`cv2.minEnclosingCircle(cnt)` 返回圆心坐标和半径。

![](img/8fe87be69dd7f94e92a5044796fecb8f_45.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_47.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_49.png)

```python
# 计算并绘制外接矩形
cnt = contours[0]
x, y, w, h = cv2.boundingRect(cnt)
cv2.rectangle(img, (x, y), (x+w, y+h), (0, 255, 0), 2)  # 用绿色绘制

![](img/8fe87be69dd7f94e92a5044796fecb8f_51.png)

# 计算并绘制外接圆
(x, y), radius = cv2.minEnclosingCircle(cnt)
center = (int(x), int(y))
radius = int(radius)
cv2.circle(img, center, radius, (255, 0, 0), 2)  # 用蓝色绘制
```

**特征衍生示例**：通过外接矩形，可以计算轮廓的“紧密度”或“饱满度”特征，例如 **轮廓面积 / 外接矩形面积**。这个比值越接近1，说明轮廓形状越饱满，越接近矩形。

![](img/8fe87be69dd7f94e92a5044796fecb8f_53.png)

## 总结

![](img/8fe87be69dd7f94e92a5044796fecb8f_55.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_57.png)

本节课中我们一起学习了轮廓分析的两个核心操作：特征计算与形状近似。
*   我们掌握了如何计算轮廓的**面积**和**周长**。
*   我们理解了**轮廓近似（Douglas-Peucker算法）** 的原理，并学会了使用 `cv2.approxPolyDP` 函数来简化轮廓形状。
*   我们探索了如何为轮廓计算**外接矩形**和**外接圆**，并了解到这些外接形状可用于衍生出更多有价值的图像特征。

![](img/8fe87be69dd7f94e92a5044796fecb8f_59.png)

![](img/8fe87be69dd7f94e92a5044796fecb8f_61.png)

这些技术是图像处理和计算机视觉中形状分析的基础，在物体识别、测量和分类等任务中有着广泛的应用。