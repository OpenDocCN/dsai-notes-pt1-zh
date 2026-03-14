#  009：如何从音频中提取均方根能量与过零率 🎵

在本节课中，我们将学习如何从音频信号中提取两种重要的时域特征：**均方根能量** 和 **过零率**。我们将使用 `librosa` 库来实现这些功能，并会从零开始编写代码来理解其背后的原理。

![](img/b3729772e16d0e187e40453400f2ad8b_1.png)

![](img/b3729772e16d0e187e40453400f2ad8b_2.png)

## 概述与准备工作

首先，我们需要导入必要的库并加载一些音频文件用于演示。我们将使用三首不同风格的音乐片段：一首德彪西的管弦乐、一首艾灵顿公爵的爵士乐和一首红辣椒乐队的摇滚乐。

![](img/b3729772e16d0e187e40453400f2ad8b_4.png)

![](img/b3729772e16d0e187e40453400f2ad8b_5.png)

![](img/b3729772e16d0e187e40453400f2ad8b_6.png)

![](img/b3729772e16d0e187e40453400f2ad8b_7.png)

```python
import matplotlib.pyplot as plt
import numpy as np
import librosa
import librosa.display
import IPython.display as ipd
```

接下来，我们加载音频文件。为了节省时间，我们直接复用之前课程中的代码。

```python
# 定义音频文件路径
debussy_path = ‘audio/debussy.wav’
redhot_path = ‘audio/redhot.wav’
duke_path = ‘audio/duke.wav’

# 加载音频信号
debussy, _ = librosa.load(debussy_path)
redhot, _ = librosa.load(redhot_path)
duke, _ = librosa.load(duke_path)
```

现在我们已经准备好了音频数据，可以开始提取特征了。

## 使用Librosa提取均方根能量

均方根能量是衡量音频信号在一段时间内平均能量的指标。它比振幅包络更平滑，因为它考虑了帧内所有样本的贡献。

以下是使用 `librosa` 提取均方根能量的步骤。首先，我们定义帧长和跳数。

```python
FRAME_LENGTH = 1024
HOP_LENGTH = 512
```

![](img/b3729772e16d0e187e40453400f2ad8b_9.png)

![](img/b3729772e16d0e187e40453400f2ad8b_11.png)

![](img/b3729772e16d0e187e40453400f2ad8b_13.png)

然后，我们使用 `librosa.feature.rms` 函数来计算每首音乐的均方根能量。

```python
rms_debussy = librosa.feature.rms(y=debussy, frame_length=FRAME_LENGTH, hop_length=HOP_LENGTH)
rms_redhot = librosa.feature.rms(y=redhot, frame_length=FRAME_LENGTH, hop_length=HOP_LENGTH)
rms_duke = librosa.feature.rms(y=duke, frame_length=FRAME_LENGTH, hop_length=HOP_LENGTH)
```

![](img/b3729772e16d0e187e40453400f2ad8b_15.png)

![](img/b3729772e16d0e187e40453400f2ad8b_16.png)

![](img/b3729772e16d0e187e40453400f2ad8b_18.png)

该函数返回一个NumPy数组。为了绘图方便，我们通常需要提取其第一行数据。

```python
rms_debussy = rms_debussy[0]
rms_redhot = rms_redhot[0]
rms_duke = rms_duke[0]
```

现在，我们可以将均方根能量与原始波形一起可视化。

![](img/b3729772e16d0e187e40453400f2ad8b_20.png)

```python
frames = range(len(rms_debussy))
t = librosa.frames_to_time(frames, hop_length=HOP_LENGTH)

plt.figure(figsize=(15, 17))

![](img/b3729772e16d0e187e40453400f2ad8b_22.png)

![](img/b3729772e16d0e187e40453400f2ad8b_23.png)

![](img/b3729772e16d0e187e40453400f2ad8b_24.png)

![](img/b3729772e16d0e187e40453400f2ad8b_25.png)

# 绘制德彪西的波形和RMS
ax = plt.subplot(3, 1, 1)
librosa.display.waveshow(debussy, alpha=0.5)
plt.plot(t, rms_debussy, color=‘r’)
plt.title(‘Debussy’)

# 绘制红辣椒乐队的波形和RMS
plt.subplot(3, 1, 2)
librosa.display.waveshow(redhot, alpha=0.5)
plt.plot(t, rms_redhot, color=‘r’)
plt.title(‘Red Hot Chili Peppers’)

![](img/b3729772e16d0e187e40453400f2ad8b_27.png)

# 绘制艾灵顿公爵的波形和RMS
plt.subplot(3, 1, 3)
librosa.display.waveshow(duke, alpha=0.5)
plt.plot(t, rms_duke, color=‘r’)
plt.title(‘Duke Ellington’)

![](img/b3729772e16d0e187e40453400f2ad8b_29.png)

![](img/b3729772e16d0e187e40453400f2ad8b_31.png)

plt.show()
```

![](img/b3729772e16d0e187e40453400f2ad8b_33.png)

从图中可以看出，均方根能量曲线比之前学习的振幅包络曲线平滑得多，因为它平均了帧内所有样本的能量，而不是只取最大值。

## 从零实现均方根能量计算

![](img/b3729772e16d0e187e40453400f2ad8b_35.png)

为了深入理解均方根能量的计算原理，我们现在尝试自己编写一个函数来实现它。

均方根能量的计算公式如下：

**RMS(t) = sqrt( (1/N) * Σ_{n=0}^{N-1} x(t, n)^2 )**

其中，`N` 是帧长，`x(t, n)` 是第 `t` 帧中的第 `n` 个样本。

![](img/b3729772e16d0e187e40453400f2ad8b_37.png)

![](img/b3729772e16d0e187e40453400f2ad8b_39.png)

以下是该函数的实现代码：

```python
def rms_manual(signal, frame_length, hop_length):
    rms = []
    for i in range(0, len(signal), hop_length):
        current_frame = signal[i:i+frame_length]
        frame_energy = np.sum(current_frame ** 2)
        frame_rms = np.sqrt(frame_energy / frame_length)
        rms.append(frame_rms)
    return np.array(rms)
```

![](img/b3729772e16d0e187e40453400f2ad8b_41.png)

现在，我们用这个自定义函数来计算均方根能量，并与 `librosa` 的结果进行比较。

```python
rms_debussy_manual = rms_manual(debussy, FRAME_LENGTH, HOP_LENGTH)
rms_redhot_manual = rms_manual(redhot, FRAME_LENGTH, HOP_LENGTH)
rms_duke_manual = rms_manual(duke, FRAME_LENGTH, HOP_LENGTH)
```

![](img/b3729772e16d0e187e40453400f2ad8b_43.png)

![](img/b3729772e16d0e187e40453400f2ad8b_44.png)

![](img/b3729772e16d0e187e40453400f2ad8b_45.png)

我们将自定义函数的结果（黄色）与 `librosa` 的结果（红色）绘制在同一张图上进行对比。

```python
plt.figure(figsize=(15, 17))

![](img/b3729772e16d0e187e40453400f2ad8b_47.png)

ax = plt.subplot(3, 1, 1)
librosa.display.waveshow(debussy, alpha=0.5)
plt.plot(t, rms_debussy, color=‘r’)
plt.plot(t, rms_debussy_manual, color=‘y’)
plt.title(‘Debussy’)

![](img/b3729772e16d0e187e40453400f2ad8b_49.png)

![](img/b3729772e16d0e187e40453400f2ad8b_50.png)

plt.subplot(3, 1, 2)
librosa.display.waveshow(redhot, alpha=0.5)
plt.plot(t, rms_redhot, color=‘r’)
plt.plot(t, rms_redhot_manual, color=‘y’)
plt.title(‘Red Hot Chili Peppers’)

![](img/b3729772e16d0e187e40453400f2ad8b_52.png)

plt.subplot(3, 1, 3)
librosa.display.waveshow(duke, alpha=0.5)
plt.plot(t, rms_duke, color=‘r’)
plt.plot(t, rms_duke_manual, color=‘y’)
plt.title(‘Duke Ellington’)

![](img/b3729772e16d0e187e40453400f2ad8b_54.png)

![](img/b3729772e16d0e187e40453400f2ad8b_56.png)

plt.show()
```

![](img/b3729772e16d0e187e40453400f2ad8b_57.png)

可以看到，两条曲线几乎完全重合，这验证了我们自定义函数的正确性。

## 提取过零率

过零率是指音频信号在单位时间内穿过零点的次数。它是一个简单的时域特征，常用于语音/音乐分类和打击乐检测。

使用 `librosa` 提取过零率非常简单。

```python
zcr_debussy = librosa.feature.zero_crossing_rate(debussy, frame_length=FRAME_LENGTH, hop_length=HOP_LENGTH)[0]
zcr_redhot = librosa.feature.zero_crossing_rate(redhot, frame_length=FRAME_LENGTH, hop_length=HOP_LENGTH)[0]
zcr_duke = librosa.feature.zero_crossing_rate(duke, frame_length=FRAME_LENGTH, hop_length=HOP_LENGTH)[0]
```

`librosa` 返回的过零率是归一化的值（除以帧长）。为了在同一图中比较三首音乐，我们进行可视化。

```python
frames = range(len(zcr_debussy))
t = librosa.frames_to_time(frames, hop_length=HOP_LENGTH)

![](img/b3729772e16d0e187e40453400f2ad8b_59.png)

plt.figure(figsize=(10, 5))
plt.plot(t, zcr_debussy, color=‘y’, label=‘Debussy’)
plt.plot(t, zcr_redhot, color=‘r’, label=‘Red Hot Chili Peppers’)
plt.plot(t, zcr_duke, color=‘b’, label=‘Duke Ellington’)
plt.ylim(0, 1) # 归一化值范围在0到1之间
plt.legend()
plt.show()
```

![](img/b3729772e16d0e187e40453400f2ad8b_60.png)

![](img/b3729772e16d0e187e40453400f2ad8b_62.png)

![](img/b3729772e16d0e187e40453400f2ad8b_64.png)

从图中可以看出，红辣椒乐队的摇滚乐过零率最高，这是因为摇滚乐中打击乐成分较多。古典乐（德彪西）的过零率较低，而爵士乐（艾灵顿公爵）则介于两者之间。

![](img/b3729772e16d0e187e40453400f2ad8b_65.png)

![](img/b3729772e16d0e187e40453400f2ad8b_67.png)

如果我们想要得到实际的过零次数（而非归一化值），只需将结果乘以帧长即可。

```python
zcr_debussy_abs = zcr_debussy * FRAME_LENGTH
zcr_redhot_abs = zcr_redhot * FRAME_LENGTH
zcr_duke_abs = zcr_duke * FRAME_LENGTH
```

![](img/b3729772e16d0e187e40453400f2ad8b_69.png)

![](img/b3729772e16d0e187e40453400f2ad8b_70.png)

![](img/b3729772e16d0e187e40453400f2ad8b_72.png)

## 语音与噪声的过零率对比

![](img/b3729772e16d0e187e40453400f2ad8b_74.png)

最后，我们通过对比语音和纯噪声的过零率，来进一步理解这个特征。我们加载一段15秒的语音和一段白噪声。

![](img/b3729772e16d0e187e40453400f2ad8b_76.png)

![](img/b3729772e16d0e187e40453400f2ad8b_78.png)

```python
voice_path = ‘audio/voice.wav’
noise_path = ‘audio/noise.wav’

![](img/b3729772e16d0e187e40453400f2ad8b_80.png)

voice, _ = librosa.load(voice_path, duration=15)
noise, _ = librosa.load(noise_path, duration=15)
```

计算它们的过零率。

![](img/b3729772e16d0e187e40453400f2ad8b_82.png)

![](img/b3729772e16d0e187e40453400f2ad8b_84.png)

```python
zcr_voice = librosa.feature.zero_crossing_rate(voice, frame_length=FRAME_LENGTH, hop_length=HOP_LENGTH)[0]
zcr_noise = librosa.feature.zero_crossing_rate(noise, frame_length=FRAME_LENGTH, hop_length=HOP_LENGTH)[0]
```

然后进行可视化比较。

![](img/b3729772e16d0e187e40453400f2ad8b_86.png)

```python
frames_voice = range(len(zcr_voice))
t_voice = librosa.frames_to_time(frames_voice, hop_length=HOP_LENGTH)

plt.figure(figsize=(10, 5))
plt.plot(t_voice, zcr_voice, color=‘y’, label=‘Voice’)
plt.plot(t_voice, zcr_noise, color=‘r’, label=‘White Noise’)
plt.ylim(0, 1)
plt.legend()
plt.show()
```

正如预期，白噪声的过零率明显高于语音信号，因为白噪声的随机性导致其更频繁地穿过零点。

## 总结

本节课中，我们一起学习了两种重要的音频时域特征：

1.  **均方根能量**：它提供了信号能量的平滑估计，计算公式为 `RMS = sqrt( (1/N) * Σ x(n)^2 )`。我们使用 `librosa.feature.rms` 进行提取，并成功从零实现了该计算。
2.  **过零率**：它衡量信号符号变化的速率，是区分语音、音乐和噪声的简单有效特征。我们使用 `librosa.feature.zero_crossing_rate` 进行提取，并观察了它在不同音乐风格以及语音与噪声之间的差异。

![](img/b3729772e16d0e187e40453400f2ad8b_88.png)

至此，关于时域音频特征的学习就告一段落了。下一节课，我们将进入频域，开始学习信号处理中一个极其重要的工具——**傅里叶变换**。