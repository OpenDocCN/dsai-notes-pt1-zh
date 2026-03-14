# 57：决策树与剪枝 🌳

![](img/876cbf8f5647761e44969d7e251d00e5_0.png)

![](img/876cbf8f5647761e44969d7e251d00e5_2.png)

在本节课中，我们将学习如何使用R语言构建和解释决策树模型。我们将从加载数据开始，创建一个二元响应变量，拟合一个初始的决策树，然后通过训练集/测试集划分和交叉验证来优化模型，最终通过剪枝得到一个更简洁、易于解释的树模型。

![](img/876cbf8f5647761e44969d7e251d00e5_4.png)

---

![](img/876cbf8f5647761e44969d7e251d00e5_6.png)

## 加载数据与包

![](img/876cbf8f5647761e44969d7e251d00e5_8.png)

首先，我们需要加载必要的R包，并导入我们将要使用的数据集。

![](img/876cbf8f5647761e44969d7e251d00e5_10.png)

![](img/876cbf8f5647761e44969d7e251d00e5_11.png)

```r
# 加载所需的包，包括tree包
library(tree)
# 加载汽车座椅数据集
data(Carseats)
```

我们使用的数据集是`Carseats`。让我们先查看一下数据，特别是其中的`Sales`（销售额）变量。

![](img/876cbf8f5647761e44969d7e251d00e5_13.png)

![](img/876cbf8f5647761e44969d7e251d00e5_14.png)

![](img/876cbf8f5647761e44969d7e251d00e5_15.png)

```r
# 查看Sales变量的分布
hist(Carseats$Sales)
```

![](img/876cbf8f5647761e44969d7e251d00e5_16.png)

![](img/876cbf8f5647761e44969d7e251d00e5_17.png)

![](img/876cbf8f5647761e44969d7e251d00e5_18.png)

`Sales`是一个定量变量。为了演示如何对二元响应变量使用决策树，我们需要将`Sales`转换为一个二元变量。

---

![](img/876cbf8f5647761e44969d7e251d00e5_20.png)

![](img/876cbf8f5647761e44969d7e251d00e5_21.png)

![](img/876cbf8f5647761e44969d7e251d00e5_22.png)

## 创建二元响应变量

![](img/876cbf8f5647761e44969d7e251d00e5_24.png)

我们将创建一个名为`High`的新变量。如果`Sales`小于8，则`High`为“No”，否则为“Yes”。

```r
# 使用ifelse函数创建二元变量High
Carseats$High <- ifelse(Carseats$Sales <= 8, "No", "Yes")
```

![](img/876cbf8f5647761e44969d7e251d00e5_26.png)

![](img/876cbf8f5647761e44969d7e251d00e5_27.png)

![](img/876cbf8f5647761e44969d7e251d00e5_28.png)

![](img/876cbf8f5647761e44969d7e251d00e5_29.png)

现在，`High`变量已经添加到了`Carseats`数据框中，它将作为我们模型中的响应变量。

---

![](img/876cbf8f5647761e44969d7e251d00e5_31.png)

![](img/876cbf8f5647761e44969d7e251d00e5_32.png)

![](img/876cbf8f5647761e44969d7e251d00e5_34.png)

## 拟合初始决策树

![](img/876cbf8f5647761e44969d7e251d00e5_36.png)

接下来，我们使用所有其他变量（除了`Sales`，因为`High`由其衍生）来预测`High`。在R公式中，可以使用减号来排除特定变量。

![](img/876cbf8f5647761e44969d7e251d00e5_37.png)

![](img/876cbf8f5647761e44969d7e251d00e5_38.png)

```r
# 拟合决策树模型，使用除Sales外的所有变量
tree.carseats <- tree(High ~ . - Sales, data = Carseats)
```

![](img/876cbf8f5647761e44969d7e251d00e5_40.png)

我们可以使用`summary()`函数查看模型的摘要信息。

![](img/876cbf8f5647761e44969d7e251d00e5_42.png)

![](img/876cbf8f5647761e44969d7e251d00e5_43.png)

```r
# 查看模型摘要
summary(tree.carseats)
```

摘要信息会显示模型中使用的变量、终端节点的数量以及残差平均偏差（对于二元响应，这是二项偏差）。

![](img/876cbf8f5647761e44969d7e251d00e5_45.png)

为了直观理解模型，我们可以绘制这棵树。

![](img/876cbf8f5647761e44969d7e251d00e5_46.png)

![](img/876cbf8f5647761e44969d7e251d00e5_47.png)

```r
# 绘制决策树
plot(tree.carseats)
# 在图上添加文本标签
text(tree.carseats, pretty = 0)
```

![](img/876cbf8f5647761e44969d7e251d00e5_49.png)

绘制的树可能非常庞大和复杂，难以看清所有细节。每个分裂点都标有变量名和分裂值，每个终端节点根据其内部样本的多数类别（Yes或No）进行标记。

![](img/876cbf8f5647761e44969d7e251d00e5_51.png)

---

![](img/876cbf8f5647761e44969d7e251d00e5_53.png)

## 查看树的详细信息

![](img/876cbf8f5647761e44969d7e251d00e5_55.png)

为了获取更详细的节点信息，我们可以直接打印树对象。

```r
# 打印树的详细结构
tree.carseats
```

这将列出每个节点的编号、包含的观测数、偏差以及节点内“Yes”的比例等信息。

---

## 划分训练集与测试集

为了评估模型的泛化能力，我们需要将数据划分为训练集和测试集。我们使用250个观测作为训练集，150个作为测试集。

```r
# 设置随机种子以保证结果可重现
set.seed(1011)
# 从400个观测中随机抽取250个作为训练集索引
train <- sample(1:nrow(Carseats), 250)
# 使用训练集数据重新拟合决策树
tree.carseats <- tree(High ~ . - Sales, data = Carseats, subset = train)
```

现在，我们有了一个基于训练集拟合的新树。

---

## 在测试集上评估模型

使用拟合好的树模型对测试集进行预测，并计算错误率。

```r
# 对测试集进行预测，type="class"表示预测类别
tree.pred <- predict(tree.carseats, Carseats[-train, ], type = "class")
# 使用with函数在测试集数据环境中创建混淆矩阵
with(Carseats[-train, ], table(tree.pred, High))
# 计算测试集正确分类的比例（1 - 错误率）
correct_rate <- with(Carseats[-train, ], mean(tree.pred == High))
```

这个庞大而茂盛的树可能在测试集上存在较高的方差。接下来，我们将使用交叉验证来对其进行剪枝优化。

---

## 使用交叉验证进行剪枝

我们使用`cv.tree()`函数进行交叉验证，以寻找最优的树复杂度。

```r
# 执行10折交叉验证，使用误分类误差作为标准
cv.carseats <- cv.tree(tree.carseats, FUN = prune.misclass)
# 查看交叉验证结果
cv.carseats
```

结果会显示不同树大小对应的交叉验证误差。我们可以将其可视化。

```r
# 绘制交叉验证误差随树大小变化的图形
plot(cv.carseats)
```

图形可能有些跳跃，但我们可以观察到误差在达到某个最小值后开始上升。我们选择一个接近最小值的树大小（例如13）进行剪枝。

---

![](img/876cbf8f5647761e44969d7e251d00e5_57.png)

![](img/876cbf8f5647761e44969d7e251d00e5_58.png)

![](img/876cbf8f5647761e44969d7e251d00e5_59.png)

## 剪枝并评估最终模型

![](img/876cbf8f5647761e44969d7e251d00e5_61.png)

根据交叉验证的结果，我们将原始训练集上拟合的树剪枝到指定大小。

![](img/876cbf8f5647761e44969d7e251d00e5_63.png)

```r
# 将树剪枝至包含13个终端节点
prune.carseats <- prune.misclass(tree.carseats, best = 13)
# 绘制剪枝后的树
plot(prune.carseats)
text(prune.carseats, pretty = 0)
```

![](img/876cbf8f5647761e44969d7e251d00e5_64.png)

![](img/876cbf8f5647761e44969d7e251d00e5_65.png)

![](img/876cbf8f5647761e44969d7e251d00e5_66.png)

剪枝后的树更浅，更易于解释。最后，我们在测试集上评估这个剪枝后的模型。

```r
# 使用剪枝后的树对测试集进行预测
tree.pred <- predict(prune.carseats, Carseats[-train, ], type = "class")
# 计算剪枝树在测试集上的表现
with(Carseats[-train, ], table(tree.pred, High))
pruned_correct_rate <- with(Carseats[-train, ], mean(tree.pred == High))
```

剪枝可能不会显著提升预测准确率，但它能产生一个更简洁、更容易理解和向他人解释的模型。

![](img/876cbf8f5647761e44969d7e251d00e5_68.png)

---

![](img/876cbf8f5647761e44969d7e251d00e5_69.png)

![](img/876cbf8f5647761e44969d7e251d00e5_70.png)

![](img/876cbf8f5647761e44969d7e251d00e5_71.png)

## 总结

本节课中，我们一起学习了决策树在R中的基本应用流程：
1.  **数据准备**：加载数据并创建合适的响应变量。
2.  **模型拟合**：使用`tree()`函数构建初始决策树。
3.  **模型评估**：通过划分训练集/测试集来评估模型的泛化错误。
4.  **模型优化**：利用`cv.tree()`进行交叉验证，找到最优的树复杂度，并使用`prune.misclass()`进行剪枝，在保持预测能力的同时获得更易解释的模型。

决策树，特别是浅层树，非常易于解释。然而，它们单独的预测性能有时可能有限。在接下来的课程中，我们将学习随机森林和提升法，这些方法通常能获得比单棵决策树更好的预测性能。