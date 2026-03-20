# 013：Python中的直方图滤波器 🧭

在本节课中，我们将学习如何构建一个二维直方图滤波器。这是一种用于机器人或自动驾驶汽车定位的概率方法。我们将从一维概念扩展到二维空间，并动手实现代码。

## 概述

上一节我们介绍了一维直方图滤波器的基本原理。本节中，我们将挑战构建一个二维直方图滤波器。这个滤波器将用于为一个近似真实的自动驾驶汽车模型实现定位功能。

## 二维直方图滤波器概念

直方图滤波器通过将环境划分为离散的网格（或称为“格子”）来工作。每个格子代表机器人可能处于的一个小区域，并附有一个概率值，表示机器人位于该格子内的置信度。

在二维情况下，我们的状态空间由两个变量定义，例如世界中的 `x` 和 `y` 坐标。滤波器通过以下两个主要步骤循环更新所有格子的概率：
1.  **测量更新**：根据传感器测量数据调整概率。
2.  **运动更新**（或预测）：根据机器人的运动命令预测概率分布的变化。

其核心贝叶斯更新公式可以简化为：

**P(new) = η * P(measurement | state) * P(old)**

其中 `η` 是一个归一化常数，确保所有概率之和为1。

## 实现步骤

以下是构建二维直方图滤波器的主要步骤。

### 1. 初始化概率网格

首先，我们需要创建一个二维网格，并为每个单元格分配初始概率。在没有任何先验信息时，通常使用均匀分布。

```python
# 示例：初始化一个4x5的网格
grid_size = (4, 5)
initial_prob = 1.0 / (grid_size[0] * grid_size[1])
belief = [[initial_prob for _ in range(grid_size[1])] for _ in range(grid_size[0])]
```

### 2. 实现测量更新

当传感器获得新的测量数据（如激光测距、地标识别）时，我们需要根据测量模型来更新每个格子的概率。测量模型 `P(z|x)` 表示在状态 `x` 下观察到测量值 `z` 的可能性。

```python
def measurement_update(z, belief, sensor_model):
    new_belief = []
    total_prob = 0.0
    for row in belief:
        new_row = []
        for cell_prob in row:
            # 假设 get_likelihood 函数根据状态计算 P(z|x)
            likelihood = sensor_model.get_likelihood(z, cell_state)
            new_cell_prob = likelihood * cell_prob
            new_row.append(new_cell_prob)
            total_prob += new_cell_prob
        new_belief.append(new_row)
    # 归一化
    new_belief = [[prob / total_prob for prob in row] for row in new_belief]
    return new_belief
```

### 3. 实现运动更新

当机器人移动后，我们需要根据运动模型来平移或扩散概率分布。这通常通过卷积操作实现，将当前的概率分布与一个表示运动不确定性的核（kernel）进行卷积。

```python
def motion_update(belief, move, motion_model):
    # motion_model.kernel 表示运动不确定性，例如 [[0.1, 0.8, 0.1], ...]
    # 这里使用简单的二维卷积作为示意
    from scipy.signal import convolve2d
    shifted_belief = convolve2d(belief, motion_model.kernel, mode='same', boundary='wrap')
    return shifted_belief
```

### 4. 循环执行

将测量更新和运动更新步骤放入一个循环中，即可持续进行定位。

```python
def histogram_filter_loop(initial_belief, measurements, motions, sensor_model, motion_model):
    belief = initial_belief
    for z, u in zip(measurements, motions):
        belief = measurement_update(z, belief, sensor_model)
        belief = motion_update(belief, u, motion_model)
    return belief
```

## 总结

本节课中，我们一起学习了二维直方图滤波器的实现。我们从一维扩展到二维，定义了概率网格，并逐步实现了测量更新和运动更新两个核心步骤。通过循环执行这两个步骤，滤波器能够有效地利用传感器数据和运动信息，为自动驾驶汽车在二维环境中提供持续的定位估计。这是概率机器人学中一个基础而强大的工具。