# 44：使用交叉验证进行模型选择 📊

![](img/cab5e093444de73369d5851ac0399ae1_0.png)

在本节课中，我们将学习如何使用交叉验证进行模型选择。交叉验证是一种评估模型泛化能力的强大技术，尤其适用于样本量有限的情况。我们将重点介绍十折交叉验证，并通过R语言实现这一过程。

上一节我们介绍了使用验证集和Cp统计量进行模型选择的方法。本节中，我们来看看如何利用交叉验证，特别是十折交叉验证，来更稳健地选择最佳模型。

![](img/cab5e093444de73369d5851ac0399ae1_2.png)

![](img/cab5e093444de73369d5851ac0399ae1_3.png)

![](img/cab5e093444de73369d5851ac0399ae1_5.png)

## 实施十折交叉验证

![](img/cab5e093444de73369d5851ac0399ae1_7.png)

![](img/cab5e093444de73369d5851ac0399ae1_8.png)

![](img/cab5e093444de73369d5851ac0399ae1_10.png)

![](img/cab5e093444de73369d5851ac0399ae1_12.png)

以下是实施十折交叉验证的详细步骤。

![](img/cab5e093444de73369d5851ac0399ae1_14.png)

![](img/cab5e093444de73369d5851ac0399ae1_16.png)

首先，我们需要设置随机种子以确保结果的可重复性，并为数据集中的每个观测值分配一个折号。

![](img/cab5e093444de73369d5851ac0399ae1_18.png)

![](img/cab5e093444de73369d5851ac0399ae1_20.png)

![](img/cab5e093444de73369d5851ac0399ae1_21.png)

```r
set.seed(11)
folds <- sample(rep(1:10, length = nrow(Hitters)))
```

![](img/cab5e093444de73369d5851ac0399ae1_23.png)

![](img/cab5e093444de73369d5851ac0399ae1_25.png)

![](img/cab5e093444de73369d5851ac0399ae1_27.png)

这段代码创建了一个向量 `folds`，其长度与 `Hitters` 数据集的行数相同。它包含数字1到10的重复序列，然后通过 `sample` 函数进行随机打乱。这样，每个观测值都被随机分配到一个折中。

![](img/cab5e093444de73369d5851ac0399ae1_29.png)

![](img/cab5e093444de73369d5851ac0399ae1_31.png)

![](img/cab5e093444de73369d5851ac0399ae1_32.png)

接下来，我们创建一个矩阵来存储每次交叉验证的误差。

![](img/cab5e093444de73369d5851ac0399ae1_34.png)

![](img/cab5e093444de73369d5851ac0399ae1_36.png)

![](img/cab5e093444de73369d5851ac0399ae1_37.png)

```r
cv.errors <- matrix(NA, nrow=10, ncol=19)
```

![](img/cab5e093444de73369d5851ac0399ae1_39.png)

![](img/cab5e093444de73369d5851ac0399ae1_41.png)

![](img/cab5e093444de73369d5851ac0399ae1_43.png)

这个矩阵有10行（对应10折）和19列（对应从1到19个预测变量的所有可能子集模型）。

![](img/cab5e093444de73369d5851ac0399ae1_45.png)

![](img/cab5e093444de73369d5851ac0399ae1_47.png)

![](img/cab5e093444de73369d5851ac0399ae1_48.png)

现在，我们进入核心的双重循环过程。外层循环遍历每一折，内层循环遍历不同大小的子集模型。

![](img/cab5e093444de73369d5851ac0399ae1_50.png)

![](img/cab5e093444de73369d5851ac0399ae1_52.png)

![](img/cab5e093444de73369d5851ac0399ae1_53.png)

```r
for(k in 1:10){
    # 使用除第k折外的所有数据拟合最佳子集回归模型
    best.fit <- regsubsets(Salary ~ ., data=Hitters[folds != k, ], nvmax=19)
    for(i in 1:19){
        # 对第k折的数据进行预测
        pred <- predict(best.fit, Hitters[folds == k, ], id=i)
        # 计算并存储该折、该模型大小的均方误差
        cv.errors[k, i] <- mean((Hitters$Salary[folds == k] - pred)^2)
    }
}
```

在这个循环中：
1.  对于第 `k` 折，我们使用其他9折的数据（`folds != k`）来训练一个包含最多19个变量的最佳子集回归模型。
2.  然后，对于从1到19的每个模型大小 `i`，我们使用训练好的模型对第 `k` 折的数据（`folds == k`）进行预测。
3.  最后，我们计算预测值与真实值之间的均方误差（MSE），并将其存储在 `cv.errors` 矩阵的相应位置。

![](img/cab5e093444de73369d5851ac0399ae1_55.png)

![](img/cab5e093444de73369d5851ac0399ae1_56.png)

![](img/cab5e093444de73369d5851ac0399ae1_57.png)

![](img/cab5e093444de73369d5851ac0399ae1_58.png)

![](img/cab5e093444de73369d5851ac0399ae1_59.png)

![](img/cab5e093444de73369d5851ac0399ae1_60.png)

## 计算与可视化交叉验证误差

![](img/cab5e093444de73369d5851ac0399ae1_62.png)

![](img/cab5e093444de73369d5851ac0399ae1_63.png)

![](img/cab5e093444de73369d5851ac0399ae1_65.png)

循环结束后，我们得到了一个包含所有折和所有模型大小误差的矩阵。为了得到每个模型大小的总体性能评估，我们需要计算所有10折的平均误差。

![](img/cab5e093444de73369d5851ac0399ae1_67.png)

![](img/cab5e093444de73369d5851ac0399ae1_68.png)

```r
rmse.cv <- sqrt(apply(cv.errors, 2, mean))
```

![](img/cab5e093444de73369d5851ac0399ae1_70.png)

![](img/cab5e093444de73369d5851ac0399ae1_71.png)

![](img/cab5e093444de73369d5851ac0399ae1_73.png)

这行代码使用 `apply` 函数计算 `cv.errors` 矩阵每一列（`2` 表示按列）的均值，然后取平方根，得到每个模型大小对应的**均方根误差（RMSE）**。

![](img/cab5e093444de73369d5851ac0399ae1_74.png)

![](img/cab5e093444de73369d5851ac0399ae1_75.png)

![](img/cab5e093444de73369d5851ac0399ae1_77.png)

![](img/cab5e093444de73369d5851ac0399ae1_79.png)

最后，我们可以绘制交叉验证误差曲线，以直观地找出RMSE最小的最佳模型大小。

![](img/cab5e093444de73369d5851ac0399ae1_81.png)

![](img/cab5e093444de73369d5851ac0399ae1_82.png)

![](img/cab5e093444de73369d5851ac0399ae1_83.png)

```r
plot(rmse.cv, type='b', xlab="模型大小（变量个数）", ylab="交叉验证RMSE")
```

![](img/cab5e093444de73369d5851ac0399ae1_85.png)

与之前使用单一验证集得到的曲线相比，十折交叉验证曲线通常更加平滑，因为它综合了十次不同训练/测试划分的结果。根据这条曲线，我们可以选择RMSE达到最小或开始趋于平缓时所对应的模型大小。

![](img/cab5e093444de73369d5851ac0399ae1_87.png)

![](img/cab5e093444de73369d5851ac0399ae1_88.png)

![](img/cab5e093444de73369d5851ac0399ae1_90.png)

![](img/cab5e093444de73369d5851ac0399ae1_91.png)

本节课中我们一起学习了如何使用十折交叉验证进行模型选择。我们通过R代码实现了为数据分配折号、训练模型、计算预测误差以及最终确定最佳模型复杂度的完整流程。交叉验证通过多次划分数据来评估模型，提供了比单一验证集更可靠、更稳定的模型性能估计，是实践中的一个重要工具。