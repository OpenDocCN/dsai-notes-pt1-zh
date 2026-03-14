#  023：使用Python和Librosa提取频谱质心与带宽 🎵

在本节课中，我们将学习如何使用Librosa库来计算音频信号的频谱质心与频谱带宽。这是本系列的最后一课，我们将通过分析三首不同类型的音乐片段来实践这两个重要的频谱特征。

## 概述与准备工作

![](img/29e74a766bda69812908c49e36510eae_1.png)

![](img/29e74a766bda69812908c49e36510eae_2.png)

首先，我们需要导入必要的库并加载音频文件。我们将使用三首风格迥异的音乐：一首德彪西的管弦乐作品、一首红辣椒乐队的歌曲以及一首艾灵顿公爵的爵士乐。

```python
import librosa
import librosa.display
import matplotlib.pyplot as plt
import numpy as np

![](img/29e74a766bda69812908c49e36510eae_4.png)

![](img/29e74a766bda69812908c49e36510eae_5.png)

# 加载音频文件
debussy_path = ‘audio/debussy.wav’
redhot_path = ‘audio/redhot.wav’
duke_path = ‘audio/duke.wav’

debussy, sr_debussy = librosa.load(debussy_path)
redhot, sr_redhot = librosa.load(redhot_path)
duke, sr_duke = librosa.load(duke_path)
```

接下来，我们需要设置用于特征提取的帧长和跳跃长度参数。

![](img/29e74a766bda69812908c49e36510eae_7.png)

![](img/29e74a766bda69812908c49e36510eae_8.png)

```python
FRAME_SIZE = 2048
HOP_LENGTH = 512
```

## 提取频谱质心

上一节我们完成了准备工作，本节中我们来看看如何提取频谱质心。频谱质心可以被视为频谱的“重心”或平均频率，它反映了声音的明亮度。

以下是使用Librosa提取频谱质心的步骤：

![](img/29e74a766bda69812908c49e36510eae_10.png)

```python
# 为德彪西作品计算频谱质心
spectral_centroid_debussy = librosa.feature.spectral_centroid(y=debussy,
                                                              sr=sr_debussy,
                                                              n_fft=FRAME_SIZE,
                                                              hop_length=HOP_LENGTH)

![](img/29e74a766bda69812908c49e36510eae_12.png)

# 为红辣椒乐队歌曲计算频谱质心
spectral_centroid_redhot = librosa.feature.spectral_centroid(y=redhot,
                                                             sr=sr_redhot,
                                                             n_fft=FRAME_SIZE,
                                                             hop_length=HOP_LENGTH)

# 为艾灵顿公爵爵士乐计算频谱质心
spectral_centroid_duke = librosa.feature.spectral_centroid(y=duke,
                                                           sr=sr_duke,
                                                           n_fft=FRAME_SIZE,
                                                           hop_length=HOP_LENGTH)
```

Librosa返回的是一个二维数组，其中第一维是1，第二维是帧的数量。我们通常只需要时间序列上的值。

```python
# 获取一维的时间序列数据
sc_debussy = spectral_centroid_debussy[0]
sc_redhot = spectral_centroid_redhot[0]
sc_duke = spectral_centroid_duke[0]
```

![](img/29e74a766bda69812908c49e36510eae_14.png)

![](img/29e74a766bda69812908c49e36510eae_15.png)

## 可视化频谱质心

![](img/29e74a766bda69812908c49e36510eae_17.png)

![](img/29e74a766bda69812908c49e36510eae_18.png)

计算完成后，我们可以将三首音乐的频谱质心随时间变化的情况绘制在同一张图中进行比较。

以下是创建可视化图表的代码：

```python
# 计算时间轴
frames = range(len(sc_debussy))
t = librosa.frames_to_time(frames, hop_length=HOP_LENGTH)

# 绘制图形
plt.figure(figsize=(25, 10))

plt.plot(t, sc_debussy, color=‘b’, label=‘Debussy (Orchestral)’)
plt.plot(t, sc_redhot, color=‘r’, label=‘Red Hot Chili Peppers (Rock)’)
plt.plot(t, sc_duke, color=‘y’, label=‘Duke Ellington (Jazz)’)

![](img/29e74a766bda69812908c49e36510eae_20.png)

![](img/29e74a766bda69812908c49e36510eae_21.png)

![](img/29e74a766bda69812908c49e36510eae_23.png)

![](img/29e74a766bda69812908c49e36510eae_25.png)

plt.xlabel(‘Time (seconds)’)
plt.ylabel(‘Spectral Centroid (Hz)’)
plt.legend()
plt.show()
```

![](img/29e74a766bda69812908c49e36510eae_27.png)

观察图表可以发现，红辣椒乐队摇滚乐的频谱质心整体上高于德彪西的管弦乐和艾灵顿公爵的爵士乐。这通常是因为摇滚乐或电子音乐中高频成分更丰富。

## 提取频谱带宽

了解了频谱质心后，我们进一步看看频谱带宽。频谱带宽衡量的是频谱能量围绕质心的分散程度。

![](img/29e74a766bda69812908c49e36510eae_29.png)

提取频谱带宽的代码与提取质心非常相似：

![](img/29e74a766bda69812908c49e36510eae_31.png)

```python
# 为三首音乐计算频谱带宽
spectral_bandwidth_debussy = librosa.feature.spectral_bandwidth(y=debussy,
                                                                sr=sr_debussy,
                                                                n_fft=FRAME_SIZE,
                                                                hop_length=HOP_LENGTH)
spectral_bandwidth_redhot = librosa.feature.spectral_bandwidth(y=redhot,
                                                               sr=sr_redhot,
                                                               n_fft=FRAME_SIZE,
                                                               hop_length=HOP_LENGTH)
spectral_bandwidth_duke = librosa.feature.spectral_bandwidth(y=duke,
                                                             sr=sr_duke,
                                                             n_fft=FRAME_SIZE,
                                                             hop_length=HOP_LENGTH)

![](img/29e74a766bda69812908c49e36510eae_33.png)

![](img/29e74a766bda69812908c49e36510eae_34.png)

![](img/29e74a766bda69812908c49e36510eae_36.png)

![](img/29e74a766bda69812908c49e36510eae_38.png)

# 获取一维时间序列数据
bw_debussy = spectral_bandwidth_debussy[0]
bw_redhot = spectral_bandwidth_redhot[0]
bw_duke = spectral_bandwidth_duke[0]
```

## 可视化频谱带宽

同样地，我们可以将频谱带宽的可视化结果绘制出来。

![](img/29e74a766bda69812908c49e36510eae_40.png)

![](img/29e74a766bda69812908c49e36510eae_41.png)

![](img/29e74a766bda69812908c49e36510eae_42.png)

![](img/29e74a766bda69812908c49e36510eae_44.png)

以下是绘制频谱带宽图表的代码：

```python
# 使用相同的时间轴（因为帧数相同）
plt.figure(figsize=(25, 10))

![](img/29e74a766bda69812908c49e36510eae_46.png)

![](img/29e74a766bda69812908c49e36510eae_47.png)

plt.plot(t, bw_debussy, color=‘b’, label=‘Debussy (Orchestral)’)
plt.plot(t, bw_redhot, color=‘r’, label=‘Red Hot Chili Peppers (Rock)’)
plt.plot(t, bw_duke, color=‘y’, label=‘Duke Ellington (Jazz)’)

![](img/29e74a766bda69812908c49e36510eae_49.png)

plt.xlabel(‘Time (seconds)’)
plt.ylabel(‘Spectral Bandwidth (Hz)’)
plt.legend()
plt.show()
```

从图中可以观察到，与频谱质心的趋势类似，古典乐和爵士乐的频谱带宽通常小于摇滚乐或使用大量电子乐器、打击乐的音乐。这是因为前者的频谱能量通常更集中。

## 总结

![](img/29e74a766bda69812908c49e36510eae_51.png)

本节课中我们一起学习了如何使用Librosa库提取和可视化两个关键的频谱特征：频谱质心和频谱带宽。

我们通过分析三首不同风格的音乐，实践了完整的计算与绘图流程。频谱质心反映了声音的明亮度，而频谱带宽则描述了频谱能量的分散程度。掌握这些特征的提取方法，是进行音频机器学习任务（如音乐分类、情感分析或声音事件检测）的重要基础。

![](img/29e74a766bda69812908c49e36510eae_53.png)

本系列课程至此结束，希望大家已经建立了坚实的音频信号处理知识基础，能够在未来的AI音频或音乐工程项目中灵活运用。