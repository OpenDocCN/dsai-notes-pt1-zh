# 课程1447-P18：XGBoost精讲教程 🚀

![](img/4461d05e218e8fd47973912ad5a92706_0.png)

![](img/4461d05e218e8fd47973912ad5a92706_2.png)

在本节课中，我们将要学习极限梯度提升（XGBoost）模型。XGBoost是一种在梯度提升决策树（GBDT）基础上，通过引入正则化和二阶梯度信息进行重大改进的集成学习模型。它以计算准确、速度快而著称，在各类数据科学竞赛和实际工程中应用广泛。

![](img/4461d05e218e8fd47973912ad5a92706_4.png)

---

## 1. 模型发展脉络回顾 📈

上一节我们介绍了提升方法（Boosting）和提升决策树（BDT）。本节中，我们来看看这些基础如何演进到梯度提升决策树（GBDT），并最终形成XGBoost。

XGBoost并非深度神经网络模型，它基于决策树和集成学习思想，在模型结构和优化策略上与传统深度学习不同。

---

![](img/4461d05e218e8fd47973912ad5a92706_6.png)

![](img/4461d05e218e8fd47973912ad5a92706_8.png)

![](img/4461d05e218e8fd47973912ad5a92706_10.png)

## 2. 提升方法与BDT回顾

提升方法的核心是**加法模型**和**前向分布算法**。

*   **加法模型**：将一系列基模型 $b(x; \gamma_m)$ 与其权重系数 $\beta_m$ 累加，得到最终模型 $f(x)$。
    $$ f(x) = \sum_{m=1}^{M} \beta_m b(x; \gamma_m) $$
    优化目标是在整个加法模型上最小化损失函数。

*   **前向分布算法**：一种优化加法模型的策略。它从前向后，每一步只学习一个基函数及其系数，逐步逼近优化目标。
    $$ f_m(x) = f_{m-1}(x) + \beta_m b(x; \gamma_m) $$
    每一步的优化目标是：
    $$ (\beta_m, \gamma_m) = \arg\min_{\beta, \gamma} \sum_{i=1}^{N} L(y_i, f_{m-1}(x_i) + \beta b(x_i; \gamma)) $$

**提升决策树（BDT）** 是以决策树为基模型的提升方法。其模型形式为：
$$ f_M(x) = \sum_{m=1}^{M} T(x; \Theta_m) $$
其中，$T(x; \Theta_m)$ 表示第 $m$ 棵决策树。

当损失函数为平方损失时，BDT的学习过程有一个直观解释：每一步构建一棵新的决策树，去拟合当前模型预测值与真实值之间的**残差**。

---

## 3. 梯度提升决策树（GBDT）🔍

GBDT在BDT的基础上进行了关键改进：它使用损失函数的**负梯度**作为残差的近似值，来拟合每一棵新的回归树。这使得GBDT可以适用于各种损失函数，而不仅限于平方损失。

GBDT的算法步骤如下：

1.  初始化模型，例如使 $f_0(x)$ 为一个常数。
2.  对于 $m = 1$ 到 $M$（$M$ 为树的数量）：
    a. 对每个样本 $i$，计算负梯度（即伪残差）：
       $$ r_{mi} = -\left[\frac{\partial L(y_i, f(x_i))}{\partial f(x_i)}\right]_{f(x)=f_{m-1}(x)} $$
    b. 使用所有样本 ${(x_i, r_{mi})}$ 拟合一棵新的回归树 $T_m(x)$，其叶子节点区域为 $R_{mj}, j=1,2,...,J_m$。
    c. 对每个叶子区域 $R_{mj}$，计算最佳拟合值 $c_{mj}$，通常是通过最小化该区域内样本的损失来确定。
    d. 更新模型：$f_m(x) = f_{m-1}(x) + \sum_{j=1}^{J_m} c_{mj} I(x \in R_{mj})$。
3.  得到最终的提升树模型：$f_M(x)$。

![](img/4461d05e218e8fd47973912ad5a92706_12.png)

![](img/4461d05e218e8fd47973912ad5a92706_14.png)

![](img/4461d05e218e8fd47973912ad5a92706_16.png)

![](img/4461d05e218e8fd47973912ad5a92706_18.png)

![](img/4461d05e218e8fd47973912ad5a92706_20.png)

![](img/4461d05e218e8fd47973912ad5a92706_22.png)

**核心理解**：GBDT可以看作是在**函数空间**中进行梯度下降。每一步增加一棵树，相当于沿着损失函数对当前模型的负梯度方向前进，从而逐步降低总损失。

![](img/4461d05e218e8fd47973912ad5a92706_24.png)

![](img/4461d05e218e8fd47973912ad5a92706_26.png)

![](img/4461d05e218e8fd47973912ad5a92706_28.png)

![](img/4461d05e218e8fd47973912ad5a92706_30.png)

---

## 4. XGBoost的核心原理 ⚙️

XGBoost在GBDT的基础上主要做了两点核心改进：**引入正则化**和**使用二阶梯度信息**。

### 4.1 模型定义与正则化目标函数

在XGBoost中，单棵决策树被定义为：
$$ f_t(x) = w_{q(x)}, \quad w \in R^T, \quad q: R^d \to \{1,2,...,T\} $$
其中：
*   $q(x)$ 函数将输入样本 $x$ 映射到树的某个叶子节点索引（共 $T$ 个叶子）。
*   $w$ 是叶子节点的权重向量，$w_j$ 表示第 $j$ 个叶子节点的得分。
*   因此，$f_t(x)$ 的输出就是样本 $x$ 所在叶子节点的权重 $w_{q(x)}$。

对于包含 $K$ 棵树的加法模型，预测输出为：
$$ \hat{y}_i = \phi(x_i) = \sum_{k=1}^{K} f_k(x_i) $$

XGBoost的目标函数由损失函数和正则化项组成：
$$ Obj(\Theta) = \sum_{i=1}^{n} l(y_i, \hat{y}_i) + \sum_{k=1}^{K} \Omega(f_k) $$
其中，正则化项 $\Omega(f)$ 定义为：
$$ \Omega(f) = \gamma T + \frac{1}{2} \lambda \sum_{j=1}^{T} w_j^2 $$
这里：
*   $T$ 是树的叶子节点个数，控制模型复杂度。
*   $\sum w_j^2$ 是叶子节点权重的L2范数平方，防止权重过大。
*   $\gamma$ 和 $\lambda$ 是控制正则化强度的超参数。

### 4.2 二阶泰勒展开与目标函数化简

在第 $t$ 步迭代时，我们的目标是学习第 $t$ 棵树 $f_t$。目标函数可以写为：
$$ Obj^{(t)} = \sum_{i=1}^{n} l(y_i, \hat{y}_i^{(t-1)} + f_t(x_i)) + \Omega(f_t) + constant $$

为了优化这个目标，XGBoost在 $\hat{y}_i^{(t-1)}$ 处对损失函数 $l$ 进行**二阶泰勒展开**。定义：
$$ g_i = \frac{\partial l(y_i, \hat{y}_i^{(t-1)})}{\partial \hat{y}_i^{(t-1)}}, \quad h_i = \frac{\partial^2 l(y_i, \hat{y}_i^{(t-1)})}{\partial (\hat{y}_i^{(t-1)})^2} $$
即 $g_i$ 和 $h_i$ 分别是一阶和二阶梯度。

![](img/4461d05e218e8fd47973912ad5a92706_32.png)

![](img/4461d05e218e8fd47973912ad5a92706_34.png)

![](img/4461d05e218e8fd47973912ad5a92706_35.png)

经过泰勒展开并忽略常数项后，第 $t$ 步的目标函数近似为：
$$ Obj^{(t)} \approx \sum_{i=1}^{n} [g_i f_t(x_i) + \frac{1}{2} h_i f_t^2(x_i)] + \Omega(f_t) $$

### 4.3 统一求和与最优解

![](img/4461d05e218e8fd47973912ad5a92706_37.png)

由于 $f_t(x_i) = w_{q(x_i)}$，我们可以将按样本 $i$ 的求和，转换为按叶子节点 $j$ 的求和。定义叶子节点 $j$ 上的样本集合为 $I_j = \{ i | q(x_i) = j \}$。

![](img/4461d05e218e8fd47973912ad5a92706_39.png)

![](img/4461d05e218e8fd47973912ad5a92706_41.png)

![](img/4461d05e218e8fd47973912ad5a92706_43.png)

将目标函数按叶子节点重新组织：
$$ \begin{aligned}
Obj^{(t)} &\approx \sum_{j=1}^{T} [(\sum_{i \in I_j} g_i) w_j + \frac{1}{2} (\sum_{i \in I_j} h_i + \lambda) w_j^2] + \gamma T \\
&= \sum_{j=1}^{T} [G_j w_j + \frac{1}{2} (H_j + \lambda) w_j^2] + \gamma T
\end{aligned} $$
其中，$G_j = \sum_{i \in I_j} g_i$，$H_j = \sum_{i \in I_j} h_i$。

![](img/4461d05e218e8fd47973912ad5a92706_45.png)

这是一个关于 $w_j$ 的二次函数。对于每个叶子节点 $j$，可以求出其最优权重 $w_j^*$：
$$ w_j^* = -\frac{G_j}{H_j + \lambda} $$

![](img/4461d05e218e8fd47973912ad5a92706_47.png)

将 $w_j^*$ 代回目标函数，得到**当前树结构 $q$ 下的最小目标函数值**（即结构分数）：
$$ Obj^* = -\frac{1}{2} \sum_{j=1}^{T} \frac{G_j^2}{H_j + \lambda} + \gamma T $$

### 4.4 树结构的生成与分裂依据

$Obj^*$ 的值越小，说明当前树结构越好。XGBoost使用贪心算法来生成树：从根节点开始，迭代地选择最佳分裂点。

对于一次候选分裂，将节点上的样本分裂到左子树 $I_L$ 和右子树 $I_R$。分裂后的目标函数值减少量（即**增益**）为：
$$ Gain = \frac{1}{2} \left[ \frac{G_L^2}{H_L + \lambda} + \frac{G_R^2}{H_R + \lambda} - \frac{(G_L+G_R)^2}{H_L+H_R + \lambda} \right] - \gamma $$
其中，$G_L, H_L$ 和 $G_R, H_R$ 分别是左右子节点的一阶、二阶梯度之和。

**分裂准则**：选择使 $Gain$ 最大的特征和特征值进行分裂。如果最大 $Gain$ 小于0（或小于某个阈值），则停止分裂。这里的 $\gamma$ 起到了**预剪枝**的作用。

![](img/4461d05e218e8fd47973912ad5a92706_49.png)

![](img/4461d05e218e8fd47973912ad5a92706_51.png)

---

## 5. XGBoost应用与参数调优 🛠️

![](img/4461d05e218e8fd47973912ad5a92706_53.png)

![](img/4461d05e218e8fd47973912ad5a92706_55.png)

![](img/4461d05e218e8fd47973912ad5a92706_57.png)

![](img/4461d05e218e8fd47973912ad5a92706_59.png)

### 5.1 主要参数类别

![](img/4461d05e218e8fd47973912ad5a92706_61.png)

以下是XGBoost中需要调节的主要参数：

![](img/4461d05e218e8fd47973912ad5a92706_63.png)

![](img/4461d05e218e8fd47973912ad5a92706_65.png)

*   **通用参数**
    *   `booster`: 选择基础模型，`gbtree`（树模型）或 `gblinear`（线性模型）。
    *   `nthread`: 并行线程数。
    *   `verbosity`: 打印信息的详细程度。

*   **提升器参数（对于树模型）**
    *   `eta` (`learning_rate`): 学习率，缩小每棵树的贡献，防止过拟合。
    *   `gamma`: 节点分裂所需的最小损失减少量（即正则项 $\gamma$），用于控制树的分裂。
    *   `max_depth`: 树的最大深度。
    *   `min_child_weight`: 叶子节点中样本权重和（即 $H_j$）的最小值，用于控制过拟合。
    *   `subsample`: 训练每棵树时使用的样本子集比例。
    *   `colsample_bytree`: 训练每棵树时使用的特征子集比例。
    *   `lambda` (`reg_lambda`): L2正则化权重 $\lambda$。
    *   `alpha` (`reg_alpha`): L1正则化权重。

![](img/4461d05e218e8fd47973912ad5a92706_67.png)

![](img/4461d05e218e8fd47973912ad5a92706_69.png)

*   **学习任务参数**
    *   `objective`: 定义学习任务和损失函数，如 `reg:squarederror`（回归），`binary:logistic`（二分类），`multi:softmax`（多分类）。
    *   `eval_metric`: 评估指标，如 `rmse`, `logloss`, `error`。

### 5.2 基本使用示例

![](img/4461d05e218e8fd47973912ad5a92706_71.png)

![](img/4461d05e218e8fd47973912ad5a92706_73.png)

以下是一个简单的二分类示例代码框架：

```python
import xgboost as xgb
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# 假设 X, y 是你的数据和标签
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 转换为DMatrix格式，XGBoost高效的数据结构
dtrain = xgb.DMatrix(X_train, label=y_train)
dtest = xgb.DMatrix(X_test, label=y_test)

# 设置参数
params = {
    'booster': 'gbtree',
    'objective': 'binary:logistic', # 输出概率的二分类
    'eval_metric': 'logloss',
    'eta': 0.1,
    'max_depth': 6,
    'min_child_weight': 1,
    'gamma': 0,
    'subsample': 0.8,
    'colsample_bytree': 0.8,
    'lambda': 1,
    'alpha': 0,
    'seed': 42
}

![](img/4461d05e218e8fd47973912ad5a92706_75.png)

![](img/4461d05e218e8fd47973912ad5a92706_77.png)

# 训练模型
num_rounds = 100
bst = xgb.train(params, dtrain, num_rounds, evals=[(dtest, 'test')], early_stopping_rounds=10)

![](img/4461d05e218e8fd47973912ad5a92706_79.png)

# 预测（输出概率）
y_pred_proba = bst.predict(dtest)
# 将概率转换为类别
y_pred = (y_pred_proba > 0.5).astype(int)

![](img/4461d05e218e8fd47973912ad5a92706_81.png)

![](img/4461d05e218e8fd47973912ad5a92706_83.png)

# 评估准确率
accuracy = accuracy_score(y_test, y_pred)
print(f"Accuracy: {accuracy:.4f}")
```

![](img/4461d05e218e8fd47973912ad5a92706_85.png)

![](img/4461d05e218e8fd47973912ad5a92706_87.png)

### 5.3 与LightGBM的简单对比

LightGBM是微软开发的另一个高效的梯度提升框架。它与XGBoost的主要区别在于**树的生长策略**：
*   **XGBoost**：使用**按层生长（Level-wise）** 的策略，同一层的所有节点都进行分裂。
*   **LightGBM**：使用**按叶子生长（Leaf-wise）** 的策略，每次从当前所有叶子中，选择增益最大的一个进行分裂。

![](img/4461d05e218e8fd47973912ad5a92706_89.png)

![](img/4461d05e218e8fd47973912ad5a92706_91.png)

![](img/4461d05e218e8fd47973912ad5a92706_93.png)

![](img/4461d05e218e8fd47973912ad5a92706_95.png)

这种差异使得LightGBM通常在**训练速度**和**内存消耗**上更有优势，尤其是在数据量大或特征维度高时。但在小数据集上，XGBoost可能因其更细致的分裂控制而表现更稳定。在实际项目中，两者都值得尝试。

---

## 总结 📝

![](img/4461d05e218e8fd47973912ad5a92706_97.png)

![](img/4461d05e218e8fd47973912ad5a92706_99.png)

![](img/4461d05e218e8fd47973912ad5a92706_101.png)

![](img/4461d05e218e8fd47973912ad5a92706_103.png)

本节课中我们一起学习了XGBoost模型。我们从提升方法和BDT出发，回顾了GBDT利用负梯度拟合残差的思想。然后，我们深入探讨了XGBoost的两大核心改进：

![](img/4461d05e218e8fd47973912ad5a92706_105.png)

![](img/4461d05e218e8fd47973912ad5a92706_107.png)

![](img/4461d05e218e8fd47973912ad5a92706_109.png)

![](img/4461d05e218e8fd47973912ad5a92706_111.png)

![](img/4461d05e218e8fd47973912ad5a92706_113.png)

![](img/4461d05e218e8fd47973912ad5a92706_115.png)

![](img/4461d05e218e8fd47973912ad5a92706_117.png)

![](img/4461d05e218e8fd47973912ad5a92706_119.png)

1.  **引入正则化**：在目标函数中加入控制叶子节点数（$\gamma T$）和叶子权重（$\frac{1}{2}\lambda \sum w_j^2$）的正则项，有效防止过拟合。
2.  **利用二阶梯度**：通过二阶泰勒展开近似损失函数，在优化时同时利用一阶梯度（$g_i$）和二阶梯度（$h_i$）信息，使得优化更精确、收敛更快。

![](img/4461d05e218e8fd47973912ad5a92706_121.png)

![](img/4461d05e218e8fd47973912ad5a92706_123.png)

![](img/4461d05e218e8fd47973912ad5a92706_125.png)

![](img/4461d05e218e8fd47973912ad5a92706_127.png)

我们推导了在给定树结构下，叶子节点最优权重的计算公式 $w_j^* = -\frac{G_j}{H_j + \lambda}$，以及衡量树结构好坏的结构分数 $Obj^*$。树的分裂依据正是基于分裂前后结构分数的变化（增益 $Gain$）来决定的。

![](img/4461d05e218e8fd47973912ad5a92706_129.png)

![](img/4461d05e218e8fd47973912ad5a92706_131.png)

最后，我们简要介绍了XGBoost的主要参数、基本使用方法，并将其与LightGBM进行了对比。XGBoost因其出色的性能、灵活性和可解释性，至今仍是结构化数据建模中最强大的工具之一。