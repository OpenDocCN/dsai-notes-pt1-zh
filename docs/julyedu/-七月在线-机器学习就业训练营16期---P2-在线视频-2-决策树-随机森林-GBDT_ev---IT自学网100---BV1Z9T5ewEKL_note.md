# 机器学习就业训练营 - 第2课：决策树、随机森林与GBDT 🌲

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_0.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_2.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_4.png)

在本节课中，我们将学习一类在工业界和数据科学竞赛中占据重要地位的模型——基于树的模型。我们将从最基础的决策树开始，了解其工作原理、构建方法以及如何用于分类和回归任务。接着，我们将探讨如何通过集成学习的思想，将多棵决策树组合成更强大的模型——随机森林。这些模型以其直观的逻辑、良好的可解释性和对数据预处理要求较低的特点而广受欢迎。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_6.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_8.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_10.png)

---

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_12.png)

## 从逻辑回归到决策树 🤔

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_14.png)

上一节我们介绍了逻辑回归模型，它通过数学计算 `WX + b` 并送入 Sigmoid 函数来得到分类概率。这是一种有效的分类方法。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_16.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_18.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_20.png)

然而，人类在做决策时，往往更倾向于使用一系列直观的规则，例如“如果年龄大于30岁，就不去相亲”。这种基于“如果-那么”规则的决策过程，就是**决策树**模型的核心思想。决策树模型将复杂的决策过程转化为一棵树形结构，逻辑清晰且易于理解。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_22.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_24.png)

---

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_26.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_28.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_30.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_32.png)

## 决策树模型详解 🌳

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_34.png)

决策树是一种基于树结构进行决策的模型。它的核心组成部分如下：
*   **内部节点**：代表对某个属性（特征）进行判断的条件。
*   **分支**：代表该属性判断后的一种可能结果（取值）。
*   **叶子节点**：代表最终的决策结果（预测值）。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_36.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_38.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_40.png)

决策树的预测过程非常简单：从根节点开始，根据数据属性的取值，沿着满足条件的分支向下移动，直到到达某个叶子节点，该节点的值即为预测结果。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_42.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_44.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_46.png)

### 决策树如何“生长”？🌱

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_48.png)

决策树的构建是一个“分而治之”的递归过程，核心在于解决两个问题：
1.  **如何选择当前最重要的划分属性？**
2.  **生长到何时停止？**

#### 停止生长的条件

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_50.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_52.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_54.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_56.png)

以下是决策树停止生长的三种常见条件：
1.  **当前节点包含的样本全属于同一个类别**：无需再划分。
2.  **当前属性集为空，或所有样本在所有属性上取值相同**：无法找到新的属性进行划分。
3.  **当前节点包含的样本集合为空**：没有样本可供划分。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_58.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_60.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_62.png)

#### 选择划分属性的准则

为了找到“最重要”的属性，我们需要一个衡量标准。核心思想是：选择一个属性进行划分后，希望数据集的“不纯度”下降得最多。“纯度”越高，意味着该节点样本的类别越一致。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_64.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_66.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_68.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_70.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_72.png)

以下是三种常用的不纯度度量及对应的经典算法：

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_74.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_76.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_78.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_79.png)

1.  **信息熵与信息增益 (ID3算法)**
    *   **信息熵**：度量样本集合纯度的指标。熵越大，不确定性越高，纯度越低。
        *   **公式**：`Entropy(D) = -Σ (p_k * log₂(p_k))`，其中 `p_k` 是第 `k` 类样本的比例。
    *   **信息增益**：表示使用某个属性 `a` 进行划分后，信息熵减少的程度。ID3算法选择**信息增益最大**的属性。
        *   **公式**：`Gain(D, a) = Entropy(D) - Σ (|D_v|/|D| * Entropy(D_v))`，其中 `D_v` 是属性 `a` 取值为 `v` 的样本子集。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_81.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_83.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_85.png)

2.  **信息增益率 (C4.5算法)**
    *   **问题**：信息增益准则对可取值数目较多的属性有偏好（例如“学号”）。
    *   **改进**：C4.5算法使用**信息增益率**，它在信息增益的基础上，除以该属性本身的信息熵（称为“固有值”），以消除属性取值数量带来的影响。
        *   **公式**：`Gain_ratio(D, a) = Gain(D, a) / IV(a)`，其中 `IV(a) = -Σ (|D_v|/|D| * log₂(|D_v|/|D|))`。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_87.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_89.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_91.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_93.png)

3.  **基尼指数 (CART算法)**
    *   **基尼指数**：另一种度量不纯度的指标。直观理解为：从数据集中随机抽取两个样本，其类别标记不一致的概率。基尼指数越小，纯度越高。
        *   **公式**：`Gini(D) = 1 - Σ (p_k²)`。
    *   CART（分类与回归树）算法使用基尼指数，并且它构建的是**二叉树**。对于每个属性，它会寻找一个最优切分点，将数据分为“是”和“否”两部分。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_95.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_97.png)

**核心概念关联**：通过数学中的泰勒展开可以证明，信息熵和基尼指数在误差允许范围内是等价的，它们衡量的趋势是一致的。

---

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_99.png)

## 处理连续值与回归问题 📈

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_101.png)

### 连续值处理

决策树本质处理离散属性。对于连续属性（如“年龄”），处理方法是将其**离散化**。
1.  将训练样本在该属性上的取值排序。
2.  取任意两个相邻取值的均值作为“候选划分点”。
3.  将每个候选划分点（如“年龄 ≤ 22.5?”）视为一个**二值离散属性**，然后像考察普通离散属性一样，用上述准则（信息增益、基尼指数等）评估其划分效果，选择最优的划分点。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_103.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_105.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_107.png)

### 回归任务

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_109.png)

决策树同样可以用于回归任务，此时称为**回归树**。
*   **预测方式**：每个叶子节点不再输出类别，而是输出一个具体的数值。通常取该叶子节点内所有样本目标值的**平均值**。
*   **划分准则**：回归树在划分时，旨在最小化划分后各区域的**平方误差和**。
    *   **公式**：对于划分产生的 `M` 个区域 `R1, R2, ..., Rm`，其损失函数常定义为 `RSS = Σ_m Σ_i (y_i - c_m)²`，其中 `c_m` 是区域 `Rm` 的预测值（均值）。
*   **构建方法**：采用启发式的、自顶向下的递归二分法。对于每个属性，遍历所有可能的切分点，选择能使 `RSS` 减少最多的属性和切分点进行划分。

**防止过拟合**：与分类树类似，回归树也需要控制过拟合。常见方法包括：
*   限制树的最大深度 (`max_depth`)。
*   限制叶子节点的最少样本数 (`min_samples_leaf`)。
*   直接对损失函数加入正则化项，例如：`Loss = RSS + α * (叶子节点数量)`，其中 `α` 是超参数。

---

## 集成学习与随机森林 🌲🌳🌲

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_111.png)

单棵决策树容易过拟合且不稳定。集成学习通过结合多个学习器来获得更好的性能。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_113.png)

### Bagging 思想

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_115.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_117.png)

**Bagging** 的核心是“自助采样法”和“投票/平均”。
1.  从原始训练集中**有放回地**随机抽取 `n` 个样本，作为一个子训练集。
2.  用该子训练集训练一个基学习器（如决策树）。
3.  重复上述步骤 `T` 次，得到 `T` 个基学习器。
4.  对于分类任务，`T` 个学习器进行**投票**；对于回归任务，对 `T` 个学习器的输出取**平均**。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_119.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_121.png)

Bagging通过降低对噪声样本和异常数据的敏感性来提高模型的泛化能力。

### 随机森林

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_123.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_125.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_127.png)

**随机森林** 是 Bagging 思想的一个扩展和特化，它以决策树为基学习器，并在 Bagging 的样本随机采样基础上，增加了**特征随机采样**。
*   在构建每棵决策树时，不仅从训练集中随机采样样本，还会从所有特征中随机选取一个特征子集，然后从这个子集中选择最优划分属性。
*   这种“双重随机性”进一步增强了模型的多样性和泛化能力，有效抑制过拟合。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_129.png)

随机森林通常能产生非常平滑的决策边界，对噪声不敏感，是工业界非常强大的工具。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_131.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_133.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_135.png)

---

## 实战案例 💻

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_137.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_139.png)

### 案例1：决策树分类

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_141.png)

使用决策树预测居民收入是否超过5万美元。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_143.png)

```python
import pandas as pd
from sklearn.preprocessing import OneHotEncoder
from sklearn.tree import DecisionTreeClassifier, plot_tree

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_145.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_147.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_149.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_151.png)

# 1. 加载数据
data = pd.read_csv('adult.csv')
features = data[['workclass', 'education', 'marital-status', ...]] # 特征列
labels = data['income'] # 标签列

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_153.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_155.png)

# 2. 特征处理（将类别特征转为数值，例如独热编码）
encoder = OneHotEncoder()
features_encoded = encoder.fit_transform(features)

# 3. 构建并训练决策树模型
model = DecisionTreeClassifier(criterion='entropy', max_depth=4)
model.fit(features_encoded, labels)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_157.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_159.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_161.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_163.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_165.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_167.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_169.png)

# 4. 可视化决策树
plot_tree(model, filled=True)
```

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_171.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_173.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_175.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_177.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_179.png)

### 案例2：随机森林回归

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_181.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_183.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_185.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_187.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_189.png)

使用随机森林预测波士顿房价。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_191.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_193.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_195.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_197.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_199.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_201.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_203.png)

```python
from sklearn.datasets import load_boston
from sklearn.ensemble import RandomForestRegressor

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_205.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_207.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_209.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_211.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_213.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_215.png)

# 1. 加载数据
boston = load_boston()
X, y = boston.data, boston.target

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_217.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_219.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_221.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_223.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_225.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_227.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_229.png)

# 2. 构建并训练随机森林回归模型
# n_estimators: 森林中树的数量
# max_features: 每棵树考虑的最大特征数（可以是整数、浮点数或‘auto’等）
model = RandomForestRegressor(n_estimators=15, max_features=0.6)
model.fit(X, y)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_231.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_233.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_235.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_237.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_239.png)

# 3. 进行预测
predictions = model.predict(X[:5])
print(predictions)
```

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_241.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_243.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_245.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_247.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_249.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_251.png)

---

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_253.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_255.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_257.png)

## 总结 📚

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_259.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_261.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_263.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_265.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_267.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_269.png)

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_271.png)

本节课我们一起学习了机器学习中非常重要的一类模型——树模型。
1.  **决策树**：我们了解了其基于规则决策的直观思想，掌握了其树形结构（节点、分支、叶子）。深入探讨了决策树生长的核心：如何用**信息增益、信息增益率、基尼指数**等准则选择划分属性，以及何时停止生长。
2.  **连续值与回归**：我们学习了如何将连续属性离散化以供决策树使用，并了解了回归树如何通过最小化平方误差来构建，用于解决回归问题。
3.  **集成学习**：我们认识了**Bagging**这种通过结合多个弱学习器来提升性能的集成思想。
4.  **随机森林**：作为Bagging与决策树的完美结合，随机森林通过**对样本和特征进行双重随机采样**，构建多棵差异化的决策树，并通过投票或平均得到最终结果，极大地提升了模型的稳定性和预测精度。

![](img/0d7cf4716f2a9fe62be8bbb35cdaa65d_273.png)

树模型以其强大的表现力和相对简单的预处理流程，成为解决众多分类与回归问题的利器。理解其原理，是灵活应用和调优的基础。