# 机器学习集训营第15期 - P5：决策树、随机森林与GBDT 🌲

![](img/b9eae516af5cdb175d1e0993413e8b35_0.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_2.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_4.png)

在本节课中，我们将学习一类在工业界和数据科学竞赛中极为重要的模型——树模型。我们将从基础的决策树开始，了解其工作原理和构建方法，然后探讨如何通过集成学习构建更强大的模型，如随机森林。这些模型以其直观的逻辑、强大的预测能力和对数据预处理要求较低的特点而广受欢迎。

![](img/b9eae516af5cdb175d1e0993413e8b35_6.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_8.png)

## 从逻辑回归到决策树

![](img/b9eae516af5cdb175d1e0993413e8b35_10.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_12.png)

上一节我们介绍了逻辑回归模型，它通过线性组合特征并经过Sigmoid函数转换来得到分类概率。其核心公式为：

![](img/b9eae516af5cdb175d1e0993413e8b35_14.png)

**P = σ(WX + b)**

![](img/b9eae516af5cdb175d1e0993413e8b35_16.png)

其中，σ代表Sigmoid函数。

![](img/b9eae516af5cdb175d1e0993413e8b35_18.png)

然而，人类的决策过程往往更直观，更像是一系列“如果...那么...”的规则判断。例如，决定是否去相亲，可能会基于一系列条件：**如果年龄 > 30岁，则不去；否则，如果长相不佳，则不去；否则，如果收入高，则去...** 这种基于规则链的决策方式，就是决策树模型的直观体现。

![](img/b9eae516af5cdb175d1e0993413e8b35_20.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_22.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_24.png)

决策树模型是“透明”的，其决策逻辑清晰可见，具有很强的可解释性。

![](img/b9eae516af5cdb175d1e0993413e8b35_26.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_28.png)

## 决策树模型详解

![](img/b9eae516af5cdb175d1e0993413e8b35_30.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_32.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_34.png)

决策树是一种基于树结构进行决策的模型。其基本构成如下：
*   **内部节点**：代表对某个属性（特征）进行判断。
*   **分支**：代表该属性判断的一种可能结果（取值）。
*   **叶子节点**：代表最终的决策结果（分类标签或回归值）。

![](img/b9eae516af5cdb175d1e0993413e8b35_36.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_38.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_40.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_42.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_44.png)

预测时，只需从根节点开始，根据样本的属性值选择分支向下遍历，直到到达某个叶子节点，即可得到预测结果。

![](img/b9eae516af5cdb175d1e0993413e8b35_46.png)

### 决策树的核心问题：生长与停止

构建决策树需要解决两个核心问题：**如何生长**（选择划分属性）和**何时停止**。

以下是决策树停止生长的三种条件：
1.  **当前节点包含的样本全属于同一个类别**：无需再划分。
2.  **当前属性集为空，或所有样本在所有属性上取值相同**：无法找到有效的属性进行进一步划分。
3.  **当前节点包含的样本集合为空**：无法划分。

![](img/b9eae516af5cdb175d1e0993413e8b35_48.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_50.png)

### 如何选择最优划分属性

![](img/b9eae516af5cdb175d1e0993413e8b35_52.png)

决策树生长的关键在于，在每个节点上选择哪个属性进行划分能最大程度地提升模型的“纯度”。我们引入**信息熵**来衡量样本集合的不纯度。

![](img/b9eae516af5cdb175d1e0993413e8b35_54.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_56.png)

**信息熵公式**：
**Ent(D) = - Σ (pk * log₂(pk))**
其中，pk是当前样本集合D中第k类样本所占的比例。Ent(D)的值越小，则D的纯度越高。

![](img/b9eae516af5cdb175d1e0993413e8b35_58.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_60.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_62.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_64.png)

我们希望划分后数据的不纯度降低。因此，可以用**信息增益**来衡量某个属性a进行划分所带来的“纯度提升”：

![](img/b9eae516af5cdb175d1e0993413e8b35_66.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_68.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_69.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_71.png)

**信息增益公式**：
**Gain(D, a) = Ent(D) - Σ (|Dᵛ| / |D|) * Ent(Dᵛ)**
其中，v是属性a的取值，Dᵛ是D中在属性a上取值为v的样本子集。我们选择信息增益最大的属性作为当前节点的划分属性。

![](img/b9eae516af5cdb175d1e0993413e8b35_73.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_75.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_77.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_79.png)

基于信息增益的决策树算法称为**ID3**。

![](img/b9eae516af5cdb175d1e0993413e8b35_81.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_83.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_85.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_87.png)

### 决策树的演变：ID3、C4.5与CART

ID3算法倾向于选择取值数目较多的属性（如“学号”），这可能导致过拟合。**C4.5**算法使用**信息增益率**来改进这一点，它通过除以属性a本身的“固有值”（属性熵）来抵消属性取值数目带来的影响。

![](img/b9eae516af5cdb175d1e0993413e8b35_89.png)

**信息增益率公式**：
**Gain_ratio(D, a) = Gain(D, a) / IV(a)**
**IV(a) = - Σ (|Dᵛ| / |D|) * log₂(|Dᵛ| / |D|)**

另一种广泛使用的算法是**CART**，它使用**基尼系数**来衡量不纯度，并且每次都生成二叉树。

![](img/b9eae516af5cdb175d1e0993413e8b35_91.png)

**基尼系数公式**：
**Gini(D) = 1 - Σ (pk²)**
基尼系数反映了从数据集D中随机抽取两个样本，其类别标记不一致的概率。Gini(D)越小，数据集D的纯度越高。

![](img/b9eae516af5cdb175d1e0993413e8b35_93.png)

有趣的是，通过对信息熵的泰勒展开进行一阶近似，可以发现信息熵和基尼系数在趋势上是一致的。

![](img/b9eae516af5cdb175d1e0993413e8b35_95.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_97.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_99.png)

### 处理连续值特征

决策树同样可以处理连续值特征（如年龄）。处理方法是将连续值**离散化**。具体步骤为：
1.  将样本在该特征上的取值排序。
2.  考察每两个相邻取值的中点，作为候选划分点。
3.  将每个候选划分点（如“年龄是否<22.5”）视为一个**二值离散属性**。
4.  使用与离散属性相同的方法（计算信息增益/增益率/基尼系数）来评估这些候选划分点，选择最优的一个。

这样，连续的年龄特征就被转化为了一系列二值判断，决策树通过多次判断自然可以划分出“24岁到28岁”这样的区间。

## 回归树

决策树不仅可以用于分类，也可用于回归问题，此时称为**回归树**。与分类树预测类别不同，回归树预测的是连续值。

![](img/b9eae516af5cdb175d1e0993413e8b35_101.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_103.png)

回归树的核心思想是：将特征空间划分为若干个互不重叠的区域（R1, R2, ..., Rm），并在每个区域上有一个固定的输出值c_m。对于回归问题，通常取该区域内所有样本目标值的均值作为c_m。

![](img/b9eae516af5cdb175d1e0993413e8b35_105.png)

**划分准则**：通过递归二叉分裂，寻找最优的划分属性和划分点，使得划分后各区域的**残差平方和**最小。

![](img/b9eae516af5cdb175d1e0993413e8b35_107.png)

**RSS公式**：
**RSS = Σ ( Σ (y_i - c_m)² )**
其中，外层求和遍历所有区域，内层求和遍历该区域内的所有样本。

![](img/b9eae516af5cdb175d1e0993413e8b35_109.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_111.png)

为了防止过拟合（如将每个样本点都划分到单独区域），可以在损失函数中加入对模型复杂度的惩罚项，例如叶子节点数量的正则项。

## 集成学习与随机森林 🌳

![](img/b9eae516af5cdb175d1e0993413e8b35_113.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_115.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_117.png)

单棵决策树可能不稳定，容易过拟合。集成学习通过构建并结合多个学习器来完成学习任务，可以获得比单一学习器更优越的性能。

![](img/b9eae516af5cdb175d1e0993413e8b35_119.png)

### Bagging思想

![](img/b9eae516af5cdb175d1e0993413e8b35_121.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_123.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_125.png)

**Bagging**是并行式集成学习的代表方法，其核心是**自助采样法**。
1.  从原始样本集中随机抽取（有放回）一个子集，用于训练一个基学习器。
2.  重复上述过程T次，得到T个基学习器。
3.  对于分类任务，采用投票法结合T个学习器的结果；对于回归任务，采用平均法。

Bagging通过样本扰动增加了基学习器之间的多样性，从而降低了整体模型的方差。

![](img/b9eae516af5cdb175d1e0993413e8b35_127.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_129.png)

### 随机森林

**随机森林**是Bagging的一个扩展变体。它在以决策树为基学习器构建Bagging集成的基础上，进一步在决策树的训练过程中引入了**属性扰动**。
*   传统决策树在选择划分属性时，是从当前节点的所有属性集合中选择一个最优属性。
*   随机森林则先从该属性集合中随机选择一个包含k个属性的子集，然后再从这个子集中选择最优属性。参数k控制了随机性的程度。

![](img/b9eae516af5cdb175d1e0993413e8b35_131.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_133.png)

因此，随机森林的基学习器的多样性不仅来自样本扰动，还来自属性扰动，这使得最终集成的泛化性能进一步提升。随机森林通常能产生非常平滑的决策边界，对噪声数据有很好的鲁棒性。

![](img/b9eae516af5cdb175d1e0993413e8b35_135.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_137.png)

## 案例实践

![](img/b9eae516af5cdb175d1e0993413e8b35_139.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_141.png)

### 案例一：决策树分类

![](img/b9eae516af5cdb175d1e0993413e8b35_143.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_145.png)

我们使用一个经典数据集，根据人口普查信息（工作类型、教育程度、婚姻状况等）预测个人年收入是否超过5万美元。

![](img/b9eae516af5cdb175d1e0993413e8b35_147.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_149.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_151.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_153.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_155.png)

以下是核心代码步骤：
```python
# 1. 导入库与数据
import pandas as pd
from sklearn import preprocessing
from sklearn.tree import DecisionTreeClassifier, export_graphviz
data = pd.read_csv(‘decision_tree.csv‘)

![](img/b9eae516af5cdb175d1e0993413e8b35_157.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_159.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_161.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_163.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_165.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_167.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_169.png)

# 2. 准备特征X和标签y
feature_names = [‘workclass‘, ‘education‘, ‘marital-status‘, ...]
X = data[feature_names]
y = data[‘income‘]

![](img/b9eae516af5cdb175d1e0993413e8b35_171.png)

# 3. 特征工程：将类别特征转换为数值（如独热编码）
# ... (使用preprocessing进行处理)

![](img/b9eae516af5cdb175d1e0993413e8b35_173.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_175.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_177.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_179.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_181.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_183.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_185.png)

# 4. 构建并训练决策树模型
clf = DecisionTreeClassifier(criterion=‘entropy‘, max_depth=4)
clf.fit(X_train, y_train)

![](img/b9eae516af5cdb175d1e0993413e8b35_187.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_189.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_191.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_193.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_195.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_197.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_199.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_201.png)

# 5. 可视化决策树
export_graphviz(clf, out_file=‘tree.dot‘, feature_names=feature_names, class_names=[‘<=50K‘, ‘>50K‘], filled=True)
```

![](img/b9eae516af5cdb175d1e0993413e8b35_203.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_205.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_207.png)

### 案例二：随机森林回归

![](img/b9eae516af5cdb175d1e0993413e8b35_209.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_211.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_213.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_215.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_217.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_219.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_221.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_223.png)

我们使用波士顿房价数据集，根据房屋的各种特征（犯罪率、房间数等）预测其价格。

![](img/b9eae516af5cdb175d1e0993413e8b35_225.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_227.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_229.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_231.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_233.png)

以下是核心代码步骤：
```python
# 1. 导入库与数据
from sklearn.datasets import load_boston
from sklearn.ensemble import RandomForestRegressor
boston = load_boston()
X, y = boston.data, boston.target

![](img/b9eae516af5cdb175d1e0993413e8b35_235.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_237.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_239.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_241.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_243.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_245.png)

# 2. 构建并训练随机森林回归模型
# n_estimators: 森林中树的数量
# max_features: 寻找最佳分割时考虑的特征数（可设为特征总数的百分比，如0.6）
regr = RandomForestRegressor(n_estimators=15, max_features=0.6, random_state=0)
regr.fit(X, y)

![](img/b9eae516af5cdb175d1e0993413e8b35_247.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_249.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_251.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_253.png)

# 3. 进行预测
print(regr.predict(X[:5]))
```

![](img/b9eae516af5cdb175d1e0993413e8b35_255.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_257.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_259.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_261.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_263.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_265.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_267.png)

![](img/b9eae516af5cdb175d1e0993413e8b35_269.png)

## 总结

![](img/b9eae516af5cdb175d1e0993413e8b35_271.png)

本节课我们一起学习了树模型的核心知识。我们从决策树的基本概念出发，探讨了其构建过程（如何选择划分属性、何时停止生长），并介绍了三种主要算法：ID3、C4.5和CART。接着，我们将决策树扩展到回归问题，了解了回归树的工作原理。最后，我们学习了集成学习中的Bagging思想以及以其为基础的强大模型——随机森林，它通过结合多棵决策树并引入样本和特征的双重随机性，极大地提升了模型的泛化能力和鲁棒性。树模型以其直观、强大且易于使用的特点，成为机器学习工具箱中不可或缺的利器。