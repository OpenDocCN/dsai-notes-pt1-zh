# 66：主成分分析（PCA）深入与实例 📊

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_0.png)

在本节课中，我们将深入学习主成分分析（PCA）的后续内容，包括如何计算第二及更多主成分，并通过一个美国犯罪数据的实例来展示PCA的实际应用与解读方法。我们还将探讨PCA的另一种几何解释，并讨论一些重要的实践问题，如变量标准化和主成分数量的选择。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_2.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_3.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_4.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_5.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_6.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_7.png)

## 从第一主成分到更多主成分 🔄

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_9.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_10.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_11.png)

上一节我们介绍了如何计算第一主成分，其核心是找到一个能最大化数据方差的线性组合。当我们有 `P` 个变量时，我们可以进一步寻找第二主成分。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_13.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_14.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_16.png)

第二主成分 `Z₂` 同样是一个具有大方差的线性组合。但为了避免得到与第一主成分相同的结果，我们需要施加一个约束条件：第二主成分必须与第一主成分**不相关**。这意味着它将提供关于数据的不同信息。在满足不相关的条件下，我们希望其方差最大化。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_18.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_19.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_20.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_22.png)

这个问题可以表述为：寻找第二个线性组合 `Z₂ = φ₂₁X₁ + φ₂₂X₂ + ... + φ₂ₚXₚ`，在满足与 `Z₁` 不相关的约束下，最大化 `Z₂` 的方差。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_24.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_25.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_26.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_27.png)

## 数学性质与求解 🧮

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_29.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_31.png)

这里涉及一些有趣的数学性质。不相关性意味着定义主成分的**载荷向量**（即系数向量 `φ`）是正交的。具体来说，第二主成分的载荷向量 `φ₂` 与第一主成分的载荷向量 `φ₁` 正交。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_32.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_33.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_34.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_35.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_36.png)

这个性质是解的自然结果。我们可以将不相关性描述为载荷向量的正交性。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_38.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_39.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_40.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_41.png)

求解过程同样源于数据矩阵 `X` 的**奇异值分解**。第二主成分由第二个右奇异向量定义。我们可以继续寻找第三、第四主成分，依此类推。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_43.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_44.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_45.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_47.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_48.png)

这些主成分是**顺序地**被定义的：每一个新主成分都在与之前所有主成分不相关的条件下，最大化其方差。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_50.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_51.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_52.png)

主成分的数量最多为 `min(n-1, p)`，其中 `n` 是数据点数量，`p` 是变量数量。如果 `p > n`，则数量受限于 `n`，否则受限于 `p`。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_54.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_55.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_56.png)

最终，我们得到了数据矩阵 `X` 基于其主成分的完整分解。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_58.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_59.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_60.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_61.png)

## 实例：美国犯罪数据 🏙️

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_63.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_64.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_66.png)

为了说明PCA的应用，我们使用一个关于美国逮捕情况的数据集。该数据集包含美国50个州每10万居民中三种犯罪（谋杀、袭击、强奸）的逮捕率，以及各州城市人口占总人口的百分比。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_68.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_69.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_71.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_72.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_73.png)

数据有50个观测值（州）和4个变量。在计算主成分之前，我们通常会将变量**标准化**，使其均值为0，标准差为1。这是一个标准做法，我们稍后会讨论其原因。现在，让我们先看看结果。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_75.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_76.png)

### 双标图解读 📈

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_78.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_79.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_81.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_82.png)

结果以一张**双标图**展示。这是一种将主成分得分和载荷向量结合显示在一张图上的方法。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_84.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_85.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_87.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_88.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_89.png)

以下是双标图的解读要点：
*   图中的每个蓝色标记代表一个州，其名称已标出。
*   坐标原点位于图中心，所有数据都已中心化。
*   该图以**第一主成分**为横轴，**第二主成分**为纵轴，绘制了前两个主成分，因为它们通常是最重要的两个。
*   为了理解主成分，我们既看各观测值（州）在主成分上的**得分**（图中点的位置），也看各变量在主成分上的**载荷**（图中带标签的箭头向量）。
*   载荷向量表示每个变量在第一和第二主成分上的载荷，因此可以将其绘制为该空间中的一个向量。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_91.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_92.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_93.png)

### 主成分含义分析 🔍

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_94.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_95.png)

从图中我们可以解读：
*   **第一主成分**：在谋杀、袭击和强奸三个犯罪变量上都有较大的正载荷。这意味着在第一主成分的正方向（右侧）代表了高犯罪率区域，负方向（左侧）代表了低犯罪率区域。例如，佛罗里达州位于第一主成分的正方向，是一个高犯罪率地区。
*   **第二主成分**：主要载荷在城市人口变量上。这意味着在第二主成分的正方向（上方）代表了高城市人口比例，负方向（下方）代表了低城市人口比例。此外，强奸变量在第二主成分上也有轻微的正载荷，暗示在高城市人口地区强奸犯罪率可能略高。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_97.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_98.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_99.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_100.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_101.png)

通过观察各州在双标图上的投影位置，我们可以了解它们与主成分的关系。例如，缅因州、新罕布什尔州、爱荷华州等位于图的左侧，属于低犯罪率地区。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_103.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_104.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_106.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_107.png)

### 载荷表 📋

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_109.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_110.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_111.png)

除了图形，通常也会提供载荷表来总结结果。下表印证了我们在双标图中看到的信息：

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_113.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_114.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_115.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_117.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_119.png)

| 变量 | 第一主成分 | 第二主成分 |
| :--- | :--- | :--- |
| 谋杀 | 0.535 | -0.418 |
| 袭击 | 0.583 | -0.188 |
| 强奸 | 0.278 | 0.873 |
| 城市人口 | 0.543 | -0.167 |

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_121.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_123.png)

从表中可见，第一主成分在三个犯罪变量上载荷相近且为正，城市人口载荷稍低。第二主成分在城市人口上有高载荷，谋杀也有较高的负载荷，而强奸有正载荷。尽管查看数字有用，但图形化展示通常更直观、信息更丰富。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_125.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_126.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_127.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_128.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_130.png)

## PCA的另一种视角：数据近似 🔳

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_132.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_133.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_135.png)

PCA还有另一种重要的视角：用低维超平面来近似数据点云。这两种视角是等价的。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_137.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_138.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_140.png)

假设我们有一个三维空间的人工数据集（左图）。我们可以寻找一个二维超平面（右图中的平面）来尽可能接近这些数据点。

“接近”的定义是：计算每个数据点到该超平面的**垂直距离**，并最小化这些距离的平方和。可以证明，由前两个最大主成分的方向向量所定义的超平面，正是满足这个条件的解。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_142.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_143.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_144.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_145.png)

这直观上是合理的：为了找到一个最接近所有点的平面，我们希望数据点在该平面上的投影尽可能分散开，而“分散”正是方差最大的体现。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_146.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_147.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_148.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_149.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_150.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_151.png)

### 与线性回归的区别 ❓

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_153.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_154.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_155.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_157.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_159.png)

这里有一个常见的疑问：这听起来很像之前学过的**最小二乘线性回归**，它们有什么区别？

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_161.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_162.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_164.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_165.png)

主要区别在于距离的定义：
*   **线性回归**：有一个特定的响应变量 `Y`。距离定义为观测值 `Y` 与回归超平面预测值之间的**垂直距离**（仅在 `Y` 轴方向上）。
*   **主成分分析**：没有特定的响应变量。距离定义为数据点 `X` 到超平面的**垂直距离**（最短距离，垂直于超平面本身）。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_167.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_169.png)

简而言之，回归关注的是预测 `Y` 的误差，而PCA关注的是用低维结构表示所有 `X` 变量时的整体近似误差。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_171.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_172.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_173.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_175.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_177.png)

## 重要实践问题 ⚙️

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_179.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_181.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_183.png)

### 变量标准化的重要性

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_184.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_185.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_187.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_189.png)

之前我们提到，在分析前对变量进行了标准化（均值为0，标准差为1）。这很重要，因为**变量的缩放会影响PCA的结果**。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_190.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_192.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_194.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_196.png)

如果我们不对原始犯罪数据进行标准化，会得到一张非常不同的双标图。第一主成分将几乎完全由“袭击”变量主导。这是因为在原始数据中，“袭击”变量的方差（由于其测量单位）远大于其他变量。如果一个变量的方差特别大，它就会主导主成分的方向。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_198.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_199.png)

因此，如果我们希望避免测量单位或单个变量方差过大的影响，公平地看待所有变量的贡献，就应该在分析前将变量标准化。标准化后，所有变量都处于同一尺度，我们才能真正看到哪些变量的组合能产生大的方差。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_201.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_202.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_203.png)

### 方差解释与主成分选择

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_205.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_206.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_207.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_208.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_210.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_211.png)

与PCA相关的一个重要概念是**方差分解**。数据的总方差是所有原始变量方差之和。每个主成分 `Z_m` 也有其方差。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_213.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_215.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_216.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_217.png)

可以证明，所有原始变量的总方差等于所有主成分的方差之和。因此，我们可以计算每个主成分所解释的方差占总方差的比例，这有助于衡量每个成分的重要性。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_219.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_221.png)

对于标准化后的犯罪数据：
*   第一主成分解释了约 **60%** 的总方差。
*   第二主成分解释了约 **20%** 的总方差。
*   第三和第四主成分解释剩余的方差。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_223.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_224.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_225.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_227.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_228.png)

这些比例必然递减，因为我们是按顺序寻找最大方差的成分。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_230.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_231.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_232.png)

我们通常会绘制**方差解释比例图**（又称碎石图）。有时也绘制**累积方差解释比例图**，它显示前 `M` 个主成分总共解释了多大比例的方差。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_234.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_235.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_236.png)

#### 如何选择主成分数量？

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_238.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_239.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_240.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_241.png)

一个自然的问题是：我们应该保留多少个主成分？没有绝对严格的规则。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_243.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_244.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_245.png)

一种方法是观察碎石图，寻找“拐点”（肘部），即解释方差比例开始急剧下降并趋于平缓的点。例如，如果前两个成分解释了大部分方差，而后续成分解释的方差很少，那么保留两个成分可能是合理的。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_247.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_248.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_249.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_250.png)

另一种情况是，如果我们将PCA用于**回归降维**（用前几个主成分作为预测变量），那么由于此时存在响应变量 `Y`，我们可以使用**交叉验证**来选择在回归模型中应包含多少个主成分。然而，在纯粹的、无监督的PCA分析中，因为没有需要预测的响应变量，所以无法直接使用交叉验证。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_251.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_252.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_253.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_254.png)

## 总结 📝

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_256.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_257.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_258.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_259.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_261.png)

本节课中，我们一起深入学习了主成分分析（PCA）。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_263.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_264.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_265.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_266.png)

我们首先了解了如何顺序地计算第二及更多主成分，其核心是在与之前所有主成分不相关的约束下寻找最大方差的线性组合，并且这些主成分的载荷向量是正交的。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_268.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_269.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_270.png)

随后，我们通过一个美国犯罪数据的实例，学习了如何用**双标图**同时展示主成分得分和载荷，并从中解读主成分的实际含义。我们还探讨了PCA的另一种几何视角——用低维超平面最佳近似数据点云，并厘清了其与线性回归的区别。

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_271.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_272.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_274.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_276.png)

![](img/dbb9351a38f4abf81d2e3bc746d3bbe4_277.png)

最后，我们讨论了PCA中两个重要的实践问题：**变量标准化**的必要性，以及通过**方差解释比例**和**碎石图**来辅助决定保留多少主成分的方法。PCA是应用统计学中最常用、最强大的工具之一，在众多领域都有广泛应用。

在接下来的课程中，我们将介绍另一个非常重要的无监督学习工具：**聚类分析**，并学习两种不同的聚类方法。