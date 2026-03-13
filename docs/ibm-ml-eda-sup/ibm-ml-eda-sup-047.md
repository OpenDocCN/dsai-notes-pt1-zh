# 047：假设检验演示（第二部分）🔬

![](img/9971ad9aa29e3c02756fef2ccc84002f_1.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_3.png)

在本节课中，我们将继续学习假设检验，特别是当存在两个相互竞争的假设时，如何设定决策边界、理解两类错误，以及如何通过增加样本量来优化检验效果。

![](img/9971ad9aa29e3c02756fef2ccc84002f_4.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_6.png)

上一节我们介绍了如何基于单一零假设（例如，预测准确率为50%）进行检验。本节中我们来看看当存在两个竞争假设时，情况会变得更为复杂。

![](img/9971ad9aa29e3c02756fef2ccc84002f_8.png)

## 回顾：单一假设检验的决策边界

![](img/9971ad9aa29e3c02756fef2ccc84002f_10.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_11.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_12.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_14.png)

我们之前的目标是找出一个人需要连续猜对多少次硬币正反面，我们才能相信他的预测能力优于50%的随机猜测。

![](img/9971ad9aa29e3c02756fef2ccc84002f_16.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_17.png)

我们使用了累积分布函数（CDF）。CDF告诉我们，在给定抛硬币次数的情况下，曲线下面积给出了猜对次数**小于或等于**某个值的概率。

我们关注的是分布右侧的极端情况。我们希望找到一个值，使得猜对次数**大于或等于**该值的概率只有5%（即显著性水平α=0.05）。这意味着猜对该值**或更少**的次数有95%的概率发生。

![](img/9971ad9aa29e3c02756fef2ccc84002f_19.png)

我们使用以下代码计算这个临界值：
```python
# 假设零假设为公平硬币（p=0.5），进行100次试验
stats.binom.ppf(0.95, 100, 0.5) + 1
```
计算结果是59。这意味着，在零假设（硬币公平）下，猜对59次或更少的概率是95%，而猜对59次或更多的概率只有5%。因此，如果一个人猜对了59次以上，我们就有足够的证据拒绝“他没有预测能力”的零假设。

![](img/9971ad9aa29e3c02756fef2ccc84002f_21.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_22.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_23.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_24.png)

## 引入竞争假设 🥊

![](img/9971ad9aa29e3c02756fef2ccc84002f_26.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_27.png)

现在，假设我声称自己并非完美，但能**60%** 的时间预测正确。这仍然比50%的随机猜测要好。

![](img/9971ad9aa29e3c02756fef2ccc84002f_29.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_31.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_33.png)

如果我实际只猜对了57次，按照之前的95%置信度标准（临界值59），我们会拒绝“我比50%猜得更好”的假设。但我可以辩称：57更接近60而不是50，也许我这次有点失误，但考虑到我声称有60%的能力，这个结果还算合理。

![](img/9971ad9aa29e3c02756fef2ccc84002f_35.png)

这里的问题在于，我们不再只有一个需要证明或证伪的零假设，而是有了两个竞争的假设：
*   **假设A（零假设 H₀）**：没有预测能力，准确率 p = 0.5。
*   **假设B（备择假设 H₁）**：有预测能力，准确率 p = 0.6。

![](img/9971ad9aa29e3c02756fef2ccc84002f_37.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_38.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_40.png)

## 可视化两个假设的分布 📊

![](img/9971ad9aa29e3c02756fef2ccc84002f_42.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_43.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_44.png)

当我们使用二项分布（大量试验时可近似为正态分布）时，每个假设都会对应一条正态曲线。
*   对于 p=0.5 的假设，曲线中心在 μ=50（100次试验的期望值）。
*   对于 p=0.6 的假设，曲线中心在 μ=60。

![](img/9971ad9aa29e3c02756fef2ccc84002f_46.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_47.png)

我们使用以下代码绘制这两条曲线：
```python
import numpy as np
import scipy.stats as stats
import matplotlib.pyplot as plt

![](img/9971ad9aa29e3c02756fef2ccc84002f_49.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_50.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_51.png)

# 定义参数
n = 100 # 试验次数
p1 = 0.5 # 假设A的准确率
p2 = 0.6 # 假设B的准确率
mu1 = n * p1 # 假设A的均值
mu2 = n * p2 # 假设B的均值
sigma = np.sqrt(n * p1 * (1-p1)) # 标准差（使用零假设的p计算）

![](img/9971ad9aa29e3c02756fef2ccc84002f_53.png)

# 生成X轴数据点
x = np.linspace(1, 100, 200)

![](img/9971ad9aa29e3c02756fef2ccc84002f_55.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_56.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_57.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_58.png)

# 计算两个正态分布的概率密度函数（PDF）
y1 = stats.norm.pdf(x, mu1, sigma)
y2 = stats.norm.pdf(x, mu2, sigma)

![](img/9971ad9aa29e3c02756fef2ccc84002f_60.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_62.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_64.png)

# 绘图
plt.plot(x, y1, label='H₀: p=0.5 (No Power)')
plt.plot(x, y2, label='H₁: p=0.6 (Has Power)')
plt.axvline(x=57, color='red', linestyle='--', label='Observed: 57 correct')
plt.xlim(30, 80)
plt.legend()
plt.xlabel('Number of Correct Guesses')
plt.ylabel('Probability Density')
plt.show()
```
从图中可以看到，57次正确猜测大约位于两条曲线的交汇处。这意味着，无论是基于p=0.5还是p=0.6的假设，得到57这个结果都不算极端不可能。

![](img/9971ad9aa29e3c02756fef2ccc84002f_66.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_67.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_69.png)

计算其p值可以证实这一点：
*   对于 H₀ (p=0.5)：得到57次或更多正确的概率 `1 - stats.binom.cdf(56, 100, 0.5)` 约为6%，略高于5%的阈值。
*   对于 H₁ (p=0.6)：得到57次或更少正确的概率 `stats.binom.cdf(57, 100, 0.6)` 约为30%，远高于5%。

![](img/9971ad9aa29e3c02756fef2ccc84002f_71.png)

因此，在95%的置信水平下，我们既不能拒绝H₀，也不能拒绝H₁。57次这个观测结果无法明确支持任何一个假设。

![](img/9971ad9aa29e3c02756fef2ccc84002f_73.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_75.png)

## 设定决策边界与两类错误 ⚖️

![](img/9971ad9aa29e3c02756fef2ccc84002f_77.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_78.png)

当存在两个竞争假设时，我们需要设定一个决策边界（cutoff）。如果猜测正确次数高于这个边界，我们接受H₁（有预测能力）；如果低于，则接受H₀（无预测能力）。

![](img/9971ad9aa29e3c02756fef2ccc84002f_79.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_81.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_83.png)

以下是设定决策边界的几种思路：

![](img/9971ad9aa29e3c02756fef2ccc84002f_85.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_86.png)

**1. 选取中点并降低置信度**
一个直观的方法是取两个假设均值的中间点，即55。我们可以说，如果猜对次数超过55，就认为有预测能力。
```python
cutoff = 55
# 在H₀下，猜对次数 >= 56 的概率
alpha = 1 - stats.binom.cdf(cutoff, 100, 0.5)
# 在H₁下，猜对次数 <= 55 的概率
beta = stats.binom.cdf(cutoff, 100, 0.6)
```
计算可得，α 和 β 都约为13%。这意味着：
*   有13%的概率某人其实没有预测能力（H₀为真），但我们错误地说他有（接受H₁）。这称为**第一类错误（Type I Error）**。
*   有13%的概率某人其实有60%的预测能力（H₁为真），但我们错误地说他没有（接受H₀）。这称为**第二类错误（Type II Error）**。

![](img/9971ad9aa29e3c02756fef2ccc84002f_88.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_89.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_91.png)

此时我们的置信水平是87%。这个方法简单，但选择55作为边界点具有一定主观性。

![](img/9971ad9aa29e3c02756fef2ccc84002f_93.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_94.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_96.png)

**2. 保持对某一类错误的严格控制**
我们可以选择偏向控制其中一类错误。例如，坚持对“错误地认为无能力者有能力”（第一类错误）保持5%的严格标准。

![](img/9971ad9aa29e3c02756fef2ccc84002f_98.png)

根据之前的计算，要达到这个标准，决策边界应为58（因为猜对≥59次的概率在H₀下为5%）。
```python
cutoff_strict = stats.binom.ppf(0.95, 100, 0.5) + 1 # 结果为58
# 计算此时的第二类错误概率（H₁为真时，猜对次数 <= 58 的概率）
beta_strict = stats.binom.cdf(cutoff_strict, 100, 0.6)
```
此时，第一类错误概率被严格控制为5%，但第二类错误概率上升到了约38%。这意味着，如果某人真有60%的能力，我们有近四成的概率会误判他为没有能力。

![](img/9971ad9aa29e3c02756fef2ccc84002f_100.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_102.png)

选择哪种策略取决于实际问题。例如，在药物试验中，我们可能严格控制第一类错误（假阳性），以免将无效药物误判为有效。而在一些探索性研究中，可能更容忍第一类错误，以避免错过潜在发现（控制第二类错误）。

![](img/9971ad9aa29e3c02756fef2ccc84002f_104.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_105.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_106.png)

## 最佳解决方案：增加样本量 📈

![](img/9971ad9aa29e3c02756fef2ccc84002f_108.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_110.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_111.png)

在上述100次试验的例子中，两条分布曲线重叠区域很大，导致我们必须在两类错误之间进行艰难的权衡。

![](img/9971ad9aa29e3c02756fef2ccc84002f_113.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_115.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_116.png)

最好的解决方法是**增加样本量**。如果我们进行1000次抛硬币试验，两个分布（均值分别为500和600）的标准差相对均值会变小，曲线会变得更“瘦高”，重叠区域将大大减少。

![](img/9971ad9aa29e3c02756fef2ccc84002f_118.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_120.png)

以下是增加样本量后的代码示意：
```python
n_large = 1000
mu1_large = n_large * 0.5
mu2_large = n_large * 0.6
sigma_large = np.sqrt(n_large * 0.5 * 0.5)

![](img/9971ad9aa29e3c02756fef2ccc84002f_122.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_124.png)

# 重新计算决策边界（保持第一类错误为5%）
cutoff_large = stats.binom.ppf(0.95, n_large, 0.5) + 1
# 计算对应的第二类错误
beta_large = stats.binom.cdf(cutoff_large, n_large, 0.6)

![](img/9971ad9aa29e3c02756fef2ccc84002f_126.png)

print(f“大样本下，决策边界为：{cutoff_large}”)
print(f“大样本下，第二类错误概率为：{beta_large:.4f}”)
```
在大样本下，决策边界会变得更精确（例如可能是526），并且第二类错误概率会急剧下降。这样，我们可以在严格控制第一类错误的同时，也获得很低的第二类错误，从而更有效地区分两个竞争假设。

![](img/9971ad9aa29e3c02756fef2ccc84002f_128.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_130.png)

## 总结

![](img/9971ad9aa29e3c02756fef2ccc84002f_132.png)

本节课中我们一起学习了假设检验中更复杂的情况：
1.  **竞争假设**：当存在两个（如 p=0.5 和 p=0.6）都需要考虑的假设时，单一的显著性检验不再适用。
2.  **决策边界**：需要设定一个临界值来做出决策，这个边界的选择会影响两类错误。
3.  **两类错误**：
    *   **第一类错误（α）**：H₀为真时拒绝H₀（假阳性）。
    *   **第二类错误（β）**：H₁为真时未拒绝H₀（假阴性）。
4.  **权衡与选择**：在固定样本量下，α和β存在此消彼长的关系。可以根据研究重点选择控制其中一类错误。
5.  **增加样本量**：这是最根本的解决方案，能同时减少两类错误，使分布更分离，从而提高检验的效力（Power）。

![](img/9971ad9aa29e3c02756fef2ccc84002f_134.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_136.png)

![](img/9971ad9aa29e3c02756fef2ccc84002f_137.png)

理解这些概念对于设计科学的实验和正确地解读统计结果至关重要。