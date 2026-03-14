# OpenCV入门课程 P8：图像的读取与处理 📸

![](img/644696af046a3dda36c64e7fd579b712_0.png)

![](img/644696af046a3dda36c64e7fd579b712_2.png)

在本节课中，我们将学习如何使用OpenCV进行图像和视频的基本操作，包括读取、显示、属性获取、格式转换以及保存。这些是计算机视觉任务中最基础也是最重要的步骤。

![](img/644696af046a3dda36c64e7fd579b712_4.png)

## 图像读取与显示

![](img/644696af046a3dda36c64e7fd579b712_6.png)

![](img/644696af046a3dda36c64e7fd579b712_8.png)

上一节我们介绍了OpenCV的环境配置，本节中我们来看看如何读取和显示一张图像。

首先，我们使用`cv2.imread()`函数读取一张图像。读取后，为了展示图像，我们需要执行三个步骤：创建窗口、显示图像、等待用户操作。

```python
import cv2

![](img/644696af046a3dda36c64e7fd579b712_10.png)

![](img/644696af046a3dda36c64e7fd579b712_12.png)

# 读取图像
img = cv2.imread('cat.jpg')
# 创建窗口并显示图像
cv2.namedWindow('image', cv2.WINDOW_NORMAL)
cv2.imshow('image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

![](img/644696af046a3dda36c64e7fd579b712_14.png)

如果每次显示图像都要写这三行代码，会显得比较繁琐。我们可以将这些绘图操作封装到一个函数中。

![](img/644696af046a3dda36c64e7fd579b712_16.png)

以下是定义一个名为`cv_show`的显示函数的方法：

![](img/644696af046a3dda36c64e7fd579b712_18.png)

```python
def cv_show(name, img):
    cv2.namedWindow(name, cv2.WINDOW_NORMAL)
    cv2.imshow(name, img)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
```

![](img/644696af046a3dda36c64e7fd579b712_20.png)

定义好函数后，只需传入窗口名称和图像对象，即可完成图像的显示。

## 图像属性

![](img/644696af046a3dda36c64e7fd579b712_22.png)

在读取图像后，我们常常需要了解其基本属性，例如尺寸和通道数。

![](img/644696af046a3dda36c64e7fd579b712_24.png)

![](img/644696af046a3dda36c64e7fd579b712_26.png)

最常用的属性是`shape`属性，通过它可以获取图像的高度（H）、宽度（W）和通道数（C）。

![](img/644696af046a3dda36c64e7fd579b712_28.png)

```python
# 获取图像尺寸和通道信息
h, w, c = img.shape
print(f'高度: {h}, 宽度: {w}, 通道数: {c}')
```

![](img/644696af046a3dda36c64e7fd579b712_30.png)

对于使用`cv2.imread()`默认方式读取的彩色图像，其通道顺序是BGR，而不是常见的RGB。

![](img/644696af046a3dda36c64e7fd579b712_32.png)

## 读取灰度图像

在许多检测或识别任务中，我们通常需要先将彩色图像转换为灰度图进行预处理。

![](img/644696af046a3dda36c64e7fd579b712_34.png)

![](img/644696af046a3dda36c64e7fd579b712_36.png)

在OpenCV中，有两种主要的图像读取模式：
1.  彩色图像 (`cv2.IMREAD_COLOR`)
2.  灰度图像 (`cv2.IMREAD_GRAYSCALE`)

![](img/644696af046a3dda36c64e7fd579b712_38.png)

我们可以在读取图像时通过指定第二个参数来直接读取为灰度图。

![](img/644696af046a3dda36c64e7fd579b712_40.png)

```python
# 以灰度模式读取图像
img_gray = cv2.imread('cat.jpg', cv2.IMREAD_GRAYSCALE)
cv_show('Gray Image', img_gray)
```

读取后，查看其`shape`属性，会发现它只有高度和宽度两个维度，表示这是一个单通道的灰度图像。

除了在读取时转换，我们也可以在任意阶段将彩色图和灰度图进行相互转换，这在后续的实际应用中会经常遇到。

```python
# 将彩色图像转换为灰度图像
img_gray_converted = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
cv_show('Converted Gray Image', img_gray_converted)
```

转换后，图像中的彩色信息消失，只保留了亮度（灰度）信息。

## 图像保存

图像保存操作相对简单，使用`cv2.imwrite()`函数即可。

```python
# 保存图像
cv2.imwrite('saved_cat.jpg', img)
```

执行后，就会在指定路径生成图像文件。

此外，OpenCV读取的图像在底层是NumPy数组格式。我们可以查看其`size`（像素点总数）和`dtype`（数据类型）等属性。

```python
print(f'像素总数: {img.size}')
print(f'数据类型: {img.dtype}')
```

![](img/644696af046a3dda36c64e7fd579b712_42.png)

## 视频处理基础

![](img/644696af046a3dda36c64e7fd579b712_44.png)

![](img/644696af046a3dda36c64e7fd579b712_46.png)

了解了静态图像的处理后，我们来看看如何处理动态的视频。

![](img/644696af046a3dda36c64e7fd579b712_48.png)

视频本质上是由一系列连续的静态图像（称为“帧”）组成的。当这些帧以一定速度（如每秒30帧，即30fps）连续播放时，人眼就会感觉到动态画面。帧率越低，视频看起来就越卡顿。

因此，处理视频的核心就是将其拆分成一帧一帧的图像，然后对每一帧图像进行处理（例如人脸检测），最后再将处理结果组合或展示出来。

以下是读取和处理视频的基本步骤：

首先，使用`cv2.VideoCapture()`创建视频捕获对象。

```python
# 创建视频捕获对象
vc = cv2.VideoCapture('test.mp4')
```

创建后，需要检查视频是否能被成功打开。

```python
# 检查视频是否成功打开
if vc.isOpened():
    print("视频打开成功")
else:
    print("无法打开视频")
```

如果能成功打开，我们就可以在一个循环中逐帧读取和处理视频。

以下是遍历视频每一帧并进行处理的代码框架：

![](img/644696af046a3dda36c64e7fd579b712_50.png)

![](img/644696af046a3dda36c64e7fd579b712_52.png)

```python
while True:
    # 读取一帧，ret为是否读取成功的标志，frame为当前帧图像
    ret, frame = vc.read()
    
    # 如果读取失败（如视频已到结尾），则退出循环
    if not ret:
        break
        
    # 在此处对当前帧 `frame` 进行处理
    # 例如，将彩色帧转换为灰度帧
    gray_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    
    # 显示处理后的帧
    cv2.imshow('Video Processing', gray_frame)
    
    # 等待一定时间，并检测按键
    # 参数1：等待的毫秒数，控制播放速度。值越小播放越快。
    # 参数27：代表ESC键，按下ESC可退出循环
    if cv2.waitKey(10) & 0xFF == 27:
        break

# 释放视频捕获对象并关闭所有窗口
vc.release()
cv2.destroyAllWindows()
```

在上面的循环中：
*   `vc.read()` 每次调用都会返回下一帧。
*   `cv2.waitKey(10)` 中的参数 `10` 表示每帧显示后等待10毫秒。这个值可以控制视频播放的速度。值越小，帧与帧之间的间隔越短，视频播放越快；值越大，则播放越慢，看起来可能卡顿。
*   按 `ESC` 键（键码27）可以随时中断视频播放并退出程序。

通过这种方式，我们就实现了对视频的逐帧读取、处理（本例中转换为灰度图）和实时显示。

![](img/644696af046a3dda36c64e7fd579b712_54.png)

---

![](img/644696af046a3dda36c64e7fd579b712_56.png)

本节课中我们一起学习了OpenCV处理图像和视频的核心基础操作。我们掌握了如何读取、显示和保存图像，了解了图像的基本属性，并学会了在彩色与灰度图之间进行转换。同时，我们也理解了视频是由帧构成的原理，并实践了如何读取视频流、逐帧处理并将其动态展示出来。这些技能是进行更复杂计算机视觉任务的重要基石。