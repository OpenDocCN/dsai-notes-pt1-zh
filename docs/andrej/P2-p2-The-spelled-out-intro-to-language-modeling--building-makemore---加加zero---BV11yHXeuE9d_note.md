# P2：语言模型详解：构建MakeMore 🧠

![](img/9b0f3ae8b78024dba93e80534d70c09b_1.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_3.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_4.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_6.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_8.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_10.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_12.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_14.png)

在本节课中，我们将学习如何构建一个名为MakeMore的字符级语言模型。我们将从零开始，逐步实现一个能够根据给定数据集（如名字列表）生成新内容的模型。课程将涵盖从简单的双字母模型到基于神经网络的模型，并介绍如何训练、评估和从模型中采样。

![](img/9b0f3ae8b78024dba93e80534d70c09b_16.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_18.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_20.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_22.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_24.png)

## 概述：什么是MakeMore？ 🤔

![](img/9b0f3ae8b78024dba93e80534d70c09b_26.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_28.png)

MakeMore是一个字符级语言模型。如其名所示，它的核心功能是“生成更多”。我们将使用一个包含约32,000个名字的数据集（`names.txt`）来训练它。训练完成后，模型将能够生成听起来像名字但实际上是独特的新名字。

![](img/9b0f3ae8b78024dba93e80534d70c09b_30.png)

在底层，MakeMore将每个名字视为一个字符序列（例如，“R-E-E-S-E”）。作为字符级语言模型，它的目标是学习序列中字符出现的模式，从而能够预测给定前序字符后，下一个最可能出现的字符。

我们将实现多种类型的字符级语言模型，从简单的双字母模型和词袋模型，到多层感知机、循环神经网络，最终构建一个类似于GPT-2的现代Transformer模型。通过本系列课程，你将深入理解这些模型在字符层面的工作原理。

![](img/9b0f3ae8b78024dba93e80534d70c09b_32.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_34.png)

## 第一步：加载与探索数据 📂

![](img/9b0f3ae8b78024dba93e80534d70c09b_36.png)

首先，我们需要加载数据集并对其有一个基本的了解。

![](img/9b0f3ae8b78024dba93e80534d70c09b_38.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_40.png)

```python
# 打开并读取数据集
with open('names.txt', 'r') as f:
    words = f.read().splitlines()

![](img/9b0f3ae8b78024dba93e80534d70c09b_42.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_44.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_46.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_48.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_50.png)

# 查看前10个单词
print(words[:10])
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_52.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_54.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_56.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_58.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_60.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_61.png)

运行上述代码，我们会得到一个名字列表，例如 `['Emma', 'Olivia', 'Ava', ...]`。这个列表可能按频率排序。让我们进一步查看数据集的一些统计信息：

![](img/9b0f3ae8b78024dba93e80534d70c09b_63.png)

```python
# 统计总词数、最短和最长的单词长度
total_words = len(words)
min_len = min(len(w) for w in words)
max_len = max(len(w) for w in words)
print(f"总词数: {total_words}")
print(f"最短单词长度: {min_len}")
print(f"最长单词长度: {max_len}")
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_65.png)

我们预计总词数约为32,000。最短的单词可能只有2个字母，而最长的可能达到15个字符。这些信息对我们构建模型很重要。

![](img/9b0f3ae8b78024dba93e80534d70c09b_67.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_69.png)

## 第二步：构建双字母模型 🔤

![](img/9b0f3ae8b78024dba93e80534d70c09b_71.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_73.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_75.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_77.png)

上一节我们介绍了数据集，本节中我们来看看如何构建第一个语言模型——双字母模型。在这个模型中，我们只关注连续的两个字符（即“双字母”），并试图用前一个字符来预测后一个字符。这是一个非常简单且基础的模型，但它是很好的起点。

![](img/9b0f3ae8b78024dba93e80534d70c09b_79.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_80.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_82.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_84.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_86.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_88.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_90.png)

以下是构建双字母模型的关键步骤：

![](img/9b0f3ae8b78024dba93e80534d70c09b_92.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_94.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_96.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_98.png)

1.  **创建字符到索引的映射**：我们需要将字符（如‘a’, ‘b’, …）以及特殊的开始/结束标记转换为整数，以便后续处理。
2.  **统计双字母出现频率**：遍历数据集中的所有单词，统计每一个双字母组合出现的次数。
3.  **将频率转换为概率**：对统计结果进行归一化，使得给定前一个字符后，所有可能的下一个字符的概率之和为1。

![](img/9b0f3ae8b78024dba93e80534d70c09b_100.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_102.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_104.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_106.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_108.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_110.png)

首先，我们创建字符表。除了26个字母，我们引入一个特殊的开始/结束标记‘.’。

![](img/9b0f3ae8b78024dba93e80534d70c09b_112.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_114.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_116.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_118.png)

```python
# 创建字符列表，包括特殊标记‘.’
chars = sorted(list(set(''.join(words))))
stoi = {s:i+1 for i,s in enumerate(chars)} # 字符到索引，a从1开始
stoi['.'] = 0 # 特殊标记‘.’的索引为0
itos = {i:s for s,i in stoi.items()} # 索引到字符的反向映射
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_120.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_122.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_124.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_126.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_128.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_130.png)

接下来，我们初始化一个计数矩阵 `N`，其大小为 `(27, 27)`，用于统计双字母出现次数。行索引代表第一个字符，列索引代表第二个字符。

![](img/9b0f3ae8b78024dba93e80534d70c09b_132.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_134.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_136.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_138.png)

```python
import torch

![](img/9b0f3ae8b78024dba93e80534d70c09b_140.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_142.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_144.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_146.png)

N = torch.zeros((27, 27), dtype=torch.int32)
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_148.png)

现在，我们遍历所有单词和其中的双字母进行计数。对于每个单词，我们在其开头和结尾分别加上开始标记‘.’和结束标记‘.’。

![](img/9b0f3ae8b78024dba93e80534d70c09b_150.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_152.png)

```python
for w in words:
    chs = ['.'] + list(w) + ['.'] # 例如： ['.', 'E', 'm', 'm', 'a', '.']
    for ch1, ch2 in zip(chs, chs[1:]):
        ix1 = stoi[ch1]
        ix2 = stoi[ch2]
        N[ix1, ix2] += 1
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_154.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_156.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_158.png)

计数完成后，我们可以将计数矩阵 `N` 可视化，以直观感受不同双字母组合的出现频率。

为了从计数得到概率，我们需要对矩阵 `N` 的每一行进行归一化（即让每一行的元素之和为1）。这里我们使用PyTorch的广播功能高效实现。

![](img/9b0f3ae8b78024dba93e80534d70c09b_160.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_162.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_164.png)

```python
# 将计数转换为浮点数，并计算概率矩阵P
P = (N.float() + 1) # 加1平滑，防止零概率
P /= P.sum(dim=1, keepdim=True) # 按行归一化
```

**注意**：`keepdim=True` 参数在这里至关重要，它确保了广播除法的方向正确（按行归一化）。如果设置错误，会导致对列进行归一化，得到完全错误的结果。

![](img/9b0f3ae8b78024dba93e80534d70c09b_166.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_168.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_170.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_172.png)

## 第三步：从模型中采样 ✨

现在我们已经有了一个训练好的双字母模型（概率矩阵 `P`）。我们可以利用这个模型来生成新的名字。采样的过程是迭代式的：

![](img/9b0f3ae8b78024dba93e80534d70c09b_174.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_176.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_178.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_180.png)

1.  从开始标记‘.’（索引0）开始。
2.  查看 `P` 矩阵中对应当前字符索引的那一行，这是一个概率分布。
3.  根据这个概率分布，随机抽取下一个字符的索引。
4.  将新抽到的字符作为当前字符，重复步骤2-3。
5.  当抽到结束标记‘.’（索引0）时，停止生成。

![](img/9b0f3ae8b78024dba93e80534d70c09b_182.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_184.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_186.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_188.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_190.png)

以下是采样的代码实现：

![](img/9b0f3ae8b78024dba93e80534d70c09b_192.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_194.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_195.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_197.png)

```python
g = torch.Generator().manual_seed(2147483647) # 设置随机种子以保证结果可复现

![](img/9b0f3ae8b78024dba93e80534d70c09b_199.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_201.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_203.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_205.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_207.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_209.png)

for _ in range(5): # 生成5个名字
    out = []
    ix = 0 # 从开始标记开始
    while True:
        p = P[ix] # 当前字符对应的下一个字符概率分布
        ix = torch.multinomial(p, num_samples=1, replacement=True, generator=g).item()
        if ix == 0: # 如果抽到结束标记
            break
        out.append(itos[ix])
    print(''.join(out))
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_211.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_213.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_215.png)

运行上述代码，可能会生成如 “mor.”, “axx.” 等名字。虽然这些名字看起来不太像真实名字，但这正是双字母模型能力有限的表现——它只考虑了前一个字符的信息。

![](img/9b0f3ae8b78024dba93e80534d70c09b_217.png)

## 第四步：评估模型质量 📊

![](img/9b0f3ae8b78024dba93e80534d70c09b_219.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_221.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_223.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_225.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_227.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_229.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_231.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_233.png)

为了量化模型的好坏，我们需要一个评估指标。在统计建模中，常用的是**似然**（Likelihood）和**对数似然**（Log-Likelihood）。

![](img/9b0f3ae8b78024dba93e80534d70c09b_235.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_237.png)

*   **似然**：模型为整个训练集中所有真实出现的双字母分配的概率的乘积。值越高，说明模型对训练数据的拟合越好。
*   **对数似然**：由于概率乘积可能是一个非常小的数字，通常取其对数进行处理，称为对数似然。对数是一个单调函数，因此最大化似然等价于最大化对数似然。

![](img/9b0f3ae8b78024dba93e80534d70c09b_239.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_241.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_243.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_245.png)

然而，在机器学习中，我们习惯最小化一个损失函数。因此，我们定义**负对数似然**（Negative Log-Likelihood, NLL）作为损失函数。对于我们的双字母模型，损失计算如下：

![](img/9b0f3ae8b78024dba93e80534d70c09b_247.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_249.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_251.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_253.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_255.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_257.png)

```python
log_likelihood = 0.0
n = 0

![](img/9b0f3ae8b78024dba93e80534d70c09b_259.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_261.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_263.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_265.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_267.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_269.png)

for w in words[:3]: # 这里先用前3个单词示例
    chs = ['.'] + list(w) + ['.']
    for ch1, ch2 in zip(chs, chs[1:]):
        ix1 = stoi[ch1]
        ix2 = stoi[ch2]
        prob = P[ix1, ix2]
        logprob = torch.log(prob)
        log_likelihood += logprob
        n += 1

![](img/9b0f3ae8b78024dba93e80534d70c09b_271.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_273.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_275.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_277.png)

nll = -log_likelihood # 负对数似然
print(f"负对数似然: {nll}")
print(f"平均负对数似然（损失）: {nll/n}")
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_279.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_281.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_283.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_285.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_286.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_288.png)

平均负对数似然（即损失）越低，模型质量越好。一个完美的模型（总是预测概率为1）的损失为0。我们还可以在整个训练集上计算这个损失，作为模型的最终评估指标。

![](img/9b0f3ae8b78024dba93e80534d70c09b_290.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_292.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_294.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_296.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_298.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_300.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_302.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_304.png)

**模型平滑**：在计数时，我们给矩阵 `N` 的所有元素加了1（`N.float() + 1`）。这个技巧称为“加1平滑”或“拉普拉斯平滑”。它确保了概率矩阵 `P` 中没有绝对为零的元素，从而避免了当模型遇到训练集中未出现过的双字母时，对数似然变成负无穷大的情况。

![](img/9b0f3ae8b78024dba93e80534d70c09b_306.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_308.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_310.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_312.png)

## 第五步：用神经网络框架重建模型 🧠

![](img/9b0f3ae8b78024dba93e80534d70c09b_314.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_316.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_318.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_320.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_322.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_324.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_326.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_328.png)

上一节我们通过直接计数和归一化的“统计”方法得到了双字母模型。本节我们将使用神经网络和梯度下降的“学习”方法来达到同样的目的。这虽然对于简单的双字母模型显得大材小用，但这种方法具有极强的扩展性，是构建更复杂模型的基础。

![](img/9b0f3ae8b78024dba93e80534d70c09b_330.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_332.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_334.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_336.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_338.png)

我们的目标不变：输入一个字符（索引），输出下一个字符的概率分布。我们将构建一个极简的神经网络：

![](img/9b0f3ae8b78024dba93e80534d70c09b_340.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_342.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_344.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_346.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_348.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_350.png)

1.  **输入层**：将字符索引进行**独热编码**（One-hot Encoding）。例如，索引5变为一个长度为27的向量，只有第5位是1，其余为0。
2.  **线性层**：一个没有偏置项（bias）的全连接层。权重矩阵 `W` 的形状是 `(27, 27)`。这相当于用输入向量的索引去“查找” `W` 矩阵的某一行。
3.  **Softmax层**：将线性层的输出（称为logits）通过指数运算和归一化，转换为概率分布。

![](img/9b0f3ae8b78024dba93e80534d70c09b_352.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_354.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_356.png)

首先，我们需要准备训练数据（输入 `Xs` 和标签 `Ys`）。

![](img/9b0f3ae8b78024dba93e80534d70c09b_358.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_360.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_362.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_364.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_366.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_368.png)

```python
xs, ys = [], []
for w in words:
    chs = ['.'] + list(w) + ['.']
    for ch1, ch2 in zip(chs, chs[1:]):
        ix1 = stoi[ch1]
        ix2 = stoi[ch2]
        xs.append(ix1)
        ys.append(ix2)

![](img/9b0f3ae8b78024dba93e80534d70c09b_370.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_371.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_373.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_375.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_377.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_379.png)

xs = torch.tensor(xs)
ys = torch.tensor(ys)
num = xs.nelement()
print(f"训练样本数量: {num}")
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_381.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_383.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_385.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_387.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_388.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_390.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_392.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_394.png)

接下来，我们初始化网络参数 `W`，并实现前向传播过程。

![](img/9b0f3ae8b78024dba93e80534d70c09b_396.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_398.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_400.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_402.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_403.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_405.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_407.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_409.png)

```python
import torch.nn.functional as F

![](img/9b0f3ae8b78024dba93e80534d70c09b_411.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_413.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_415.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_417.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_419.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_421.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_423.png)

g = torch.Generator().manual_seed(2147483647)
W = torch.randn((27, 27), generator=g, requires_grad=True) # 初始化权重，并告知PyTorch需要计算梯度

![](img/9b0f3ae8b78024dba93e80534d70c09b_425.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_427.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_429.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_431.png)

# 前向传播
xenc = F.one_hot(xs, num_classes=27).float() # 独热编码
logits = xenc @ W # 线性层，等价于 W[xs]
counts = logits.exp() # 指数运算，得到“计数”
probs = counts / counts.sum(1, keepdim=True) # 归一化得到概率，即Softmax
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_433.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_435.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_437.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_439.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_441.png)

现在，我们需要计算损失。损失函数仍然是平均负对数似然。我们需要提取出模型为每个训练样本中**真实下一个字符**所分配的概率。

![](img/9b0f3ae8b78024dba93e80534d70c09b_443.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_445.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_447.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_449.png)

```python
# 提取正确标签对应的概率
loss = -probs[torch.arange(num), ys].log().mean()
print(f"初始损失: {loss.item()}")
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_451.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_453.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_455.png)

由于 `W` 是随机初始化的，初始损失会很高。接下来，我们使用梯度下降来优化 `W`。

![](img/9b0f3ae8b78024dba93e80534d70c09b_457.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_459.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_461.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_463.png)

```python
# 梯度下降优化
for k in range(100):
    # 前向传播
    xenc = F.one_hot(xs, num_classes=27).float()
    logits = xenc @ W
    counts = logits.exp()
    probs = counts / counts.sum(1, keepdim=True)
    loss = -probs[torch.arange(num), ys].log().mean()

    # 反向传播
    W.grad = None # 将梯度置零
    loss.backward()

    # 更新参数
    W.data += -50 * W.grad # 学习率为50

![](img/9b0f3ae8b78024dba93e80534d70c09b_465.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_467.png)

print(f"优化后损失: {loss.item()}")
```

![](img/9b0f3ae8b78024dba93e80534d70c09b_469.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_471.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_473.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_475.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_477.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_479.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_481.png)

经过优化，损失会下降到接近我们之前用统计方法得到的值（约2.45）。此时，神经网络的权重矩阵 `W` 经过 `exp()` 运算后，就近似等于我们之前通过计数得到的概率矩阵 `P`（的转置）。两种方法殊途同归。

![](img/9b0f3ae8b78024dba93e80534d70c09b_483.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_485.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_487.png)

**正则化的联系**：在统计方法中，我们通过“加1平滑”来防止过拟合。在神经网络方法中，这等价于一种叫做**权重衰减**或**L2正则化**的技术。我们可以在损失函数中添加一项 `0.01 * (W**2).mean()`，这会鼓励 `W` 的数值变小，从而使输出概率分布更平滑、更均匀，其效果类似于增加平滑计数。

![](img/9b0f3ae8b78024dba93e80534d70c09b_488.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_490.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_492.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_494.png)

## 总结与展望 🚀

![](img/9b0f3ae8b78024dba93e80534d70c09b_496.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_498.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_500.png)

本节课中我们一起学习了：

![](img/9b0f3ae8b78024dba93e80534d70c09b_502.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_503.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_505.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_507.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_509.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_511.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_513.png)

1.  **字符级语言模型**的基本概念：将文本视为字符序列，并预测序列中的下一个字符。
2.  **双字母模型**的构建：通过统计字符共现频率并归一化，得到一个简单的概率查找表模型。
3.  **模型评估**：使用负对数似然作为损失函数来衡量模型质量。
4.  **神经网络方法**：用独热编码、线性层和Softmax构建了等效的模型，并通过梯度下降进行训练。
5.  **两种方法的统一**：统计方法和神经网络方法在双字母模型上是等价的，但后者为构建更复杂的模型提供了框架。

![](img/9b0f3ae8b78024dba93e80534d70c09b_514.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_516.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_518.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_520.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_522.png)

双字母模型的能力非常有限，因为它只考虑了一个字符的上下文。在接下来的课程中，我们将扩展这一框架：

![](img/9b0f3ae8b78024dba93e80534d70c09b_524.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_526.png)

*   考虑更多的前序字符（如3个、5个或整个单词）。
*   用更复杂的神经网络（如MLP、RNN、Transformer）来代替简单的线性层。
*   处理更长的序列和更大的数据集。

![](img/9b0f3ae8b78024dba93e80534d70c09b_528.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_530.png)

![](img/9b0f3ae8b78024dba93e80534d70c09b_532.png)

神经网络方法的强大之处在于其可扩展性。当上下文变长时，可能的组合呈指数级增长，无法再用一个简单的表格来存储所有概率。而神经网络可以通过学习到的参数，泛化到未见过的字符组合上，从而构建出强大的语言模型。