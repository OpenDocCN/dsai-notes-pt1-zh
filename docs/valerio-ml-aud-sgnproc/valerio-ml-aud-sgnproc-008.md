#  008：使用Python从零开始提取振幅包络特征 🎵

在本节课中，我们将学习如何使用Python和Librosa库从零开始提取音频信号的振幅包络特征。我们将加载音频文件、绘制波形图，并实现计算振幅包络的算法。

![](img/391cbaece27f64c284238d800f2bc1c4_1.png)

![](img/391cbaece27f64c284238d800f2bc1c4_2.png)

## 概述

![](img/391cbaece27f64c284238d800f2bc1c4_4.png)

![](img/391cbaece27f64c284238d800f2bc1c4_5.png)

我们将使用Librosa库加载音频文件，并实现一个函数来计算振幅包络。振幅包络是音频信号在每个时间帧内振幅的最大值，它能有效反映信号的整体能量变化。我们还将学习如何可视化波形和振幅包络。

![](img/391cbaece27f64c284238d800f2bc1c4_7.png)

## 导入必要的库

![](img/391cbaece27f64c284238d800f2bc1c4_8.png)

首先，我们需要导入处理音频和绘图所需的库。

```python
import librosa
import librosa.display
import IPython.display as ipd
import matplotlib.pyplot as plt
import numpy as np
```

## 加载音频文件

我们将加载三个不同风格的音乐片段：古典乐、摇滚乐和爵士乐。

```python
# 定义音频文件路径
bach_file = ‘audio/bach.wav’
redhot_file = ‘audio/red.wav’
duke_file = ‘audio/duke.wav’

# 使用IPython播放音频
ipd.Audio(bach_file)
ipd.Audio(redhot_file)
ipd.Audio(duke_file)
```

## 使用Librosa加载音频

Librosa的`load`函数可以将音频文件加载为单声道信号，并返回信号数据及其采样率。

```python
# 加载音频文件，默认采样率为22050 Hz，转换为单声道
bach, sr = librosa.load(bach_file)
redhot, _ = librosa.load(redhot_file)
duke, _ = librosa.load(duke_file)
```

## 计算信号时长

我们可以通过采样率和样本总数来计算音频信号的时长。

```python
# 计算单个样本的时长
sample_duration = 1 / sr
print(f“Duration of one sample: {sample_duration:.6f} seconds”)

# 计算整个信号的时长
duration = sample_duration * len(bach)
print(f“Duration of signal: {duration:.2f} seconds”)
```

![](img/391cbaece27f64c284238d800f2bc1c4_10.png)

## 可视化波形

![](img/391cbaece27f64c284238d800f2bc1c4_11.png)

![](img/391cbaece27f64c284238d800f2bc1c4_13.png)

为了直观比较不同音乐的波形，我们将使用Matplotlib绘制它们的波形图。

```python
# 创建图形，设置大小
plt.figure(figsize=(15, 17))

![](img/391cbaece27f64c284238d800f2bc1c4_15.png)

![](img/391cbaece27f64c284238d800f2bc1c4_16.png)

![](img/391cbaece27f64c284238d800f2bc1c4_17.png)

![](img/391cbaece27f64c284238d800f2bc1c4_18.png)

# 绘制巴赫音频的波形
plt.subplot(3, 1, 1)
librosa.display.waveplot(bach, alpha=0.5)
plt.title(‘Bach’)
plt.ylim(-1, 1)

![](img/391cbaece27f64c284238d800f2bc1c4_19.png)

# 绘制红辣椒乐队音频的波形
plt.subplot(3, 1, 2)
librosa.display.waveplot(redhot, alpha=0.5)
plt.title(‘Red Hot Chili Peppers’)
plt.ylim(-1, 1)

![](img/391cbaece27f64c284238d800f2bc1c4_21.png)

# 绘制艾灵顿公爵音频的波形
plt.subplot(3, 1, 3)
librosa.display.waveplot(duke, alpha=0.5)
plt.title(‘Duke Ellington’)
plt.ylim(-1, 1)

![](img/391cbaece27f64c284238d800f2bc1c4_22.png)

![](img/391cbaece27f64c284238d800f2bc1c4_23.png)

![](img/391cbaece27f64c284238d800f2bc1c4_24.png)

![](img/391cbaece27f64c284238d800f2bc1c4_26.png)

plt.show()
```

通过观察波形图，我们可以发现古典音乐的振幅变化范围较大，而流行音乐的波形则相对稳定。

## 计算振幅包络

振幅包络反映了信号在每个时间帧内的最大振幅。以下是两种实现方式。

### 基础实现方法

这种方法通过循环遍历每个帧来计算振幅包络，易于理解。

```python
def amplitude_envelope(signal, frame_size, hop_length):
    “”“计算信号的振幅包络。”“”
    ae = []
    for i in range(0, len(signal), hop_length):
        current_frame = signal[i:i + frame_size]
        ae.append(max(current_frame))
    return np.array(ae)

# 设置帧大小和跳跃长度
FRAME_SIZE = 1024
HOP_LENGTH = 512

![](img/391cbaece27f64c284238d800f2bc1c4_28.png)

# 计算巴赫音频的振幅包络
ae_bach = amplitude_envelope(bach, FRAME_SIZE, HOP_LENGTH)
print(f“Amplitude envelope size: {ae_bach.size}”)
```

![](img/391cbaece27f64c284238d800f2bc1c4_30.png)

### 进阶实现方法

这种方法使用列表推导式，代码更简洁，但算法逻辑相同。

```python
def fancy_amplitude_envelope(signal, frame_size, hop_length):
    “”“使用列表推导式计算振幅包络。”“”
    return np.array([max(signal[i:i + frame_size]) for i in range(0, len(signal), hop_length)])

# 计算并验证两种方法的结果是否一致
fancy_ae_bach = fancy_amplitude_envelope(bach, FRAME_SIZE, HOP_LENGTH)
print(np.array_equal(ae_bach, fancy_ae_bach))  # 应输出 True
```

## 可视化振幅包络

现在，我们可以在波形图上叠加绘制振幅包络，以更直观地观察信号的能量变化。

```python
# 计算其他音频的振幅包络
ae_redhot = amplitude_envelope(redhot, FRAME_SIZE, HOP_LENGTH)
ae_duke = amplitude_envelope(duke, FRAME_SIZE, HOP_LENGTH)

# 将帧索引转换为时间
frames = range(ae_bach.size)
time = librosa.frames_to_time(frames, hop_length=HOP_LENGTH)

# 绘制波形和振幅包络
plt.figure(figsize=(15, 17))

# 巴赫音频
plt.subplot(3, 1, 1)
librosa.display.waveplot(bach, alpha=0.5)
plt.plot(time, ae_bach, color=‘r’)
plt.title(‘Bach with Amplitude Envelope’)
plt.ylim(-1, 1)

# 红辣椒乐队音频
plt.subplot(3, 1, 2)
librosa.display.waveplot(redhot, alpha=0.5)
plt.plot(time, ae_redhot, color=‘r’)
plt.title(‘Red Hot Chili Peppers with Amplitude Envelope’)
plt.ylim(-1, 1)

# 艾灵顿公爵音频
plt.subplot(3, 1, 3)
librosa.display.waveplot(duke, alpha=0.5)
plt.plot(time, ae_duke, color=‘r’)
plt.title(‘Duke Ellington with Amplitude Envelope’)
plt.ylim(-1, 1)

![](img/391cbaece27f64c284238d800f2bc1c4_32.png)

plt.show()
```

![](img/391cbaece27f64c284238d800f2bc1c4_34.png)

从图中可以看出，振幅包络（红色曲线）紧密跟随波形的轮廓。在摇滚乐中，振幅包络的峰值通常对应鼓点，而古典音乐的振幅包络则表现出更大幅度的起伏。

![](img/391cbaece27f64c284238d800f2bc1c4_36.png)

![](img/391cbaece27f64c284238d800f2bc1c4_37.png)

![](img/391cbaece27f64c284238d800f2bc1c4_38.png)

## 总结

![](img/391cbaece27f64c284238d800f2bc1c4_40.png)

本节课中，我们一起学习了如何使用Python和Librosa库从零开始提取振幅包络特征。我们掌握了加载音频文件、计算信号时长、绘制波形图以及实现振幅包络算法的方法。通过比较不同音乐风格的振幅包络，我们了解到这一特征如何反映音频信号的动态变化。