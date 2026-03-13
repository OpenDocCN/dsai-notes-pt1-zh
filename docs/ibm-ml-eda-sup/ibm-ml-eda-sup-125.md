# 125：36_其他决策树分割准则 🌳

在本节课中，我们将学习决策树中除基尼不纯度外的其他分割准则，包括分类错误率与信息熵。我们将通过图表对比它们的特点，并理解为何某些准则能更好地促进树的持续分割。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_1.png)

---

![](img/8922ff76e149a7a1db1a381a38bf7a8e_2.png)

上一节我们介绍了决策树的基本概念，本节中我们来看看分类错误率与信息熵在图形上的表现。

X轴代表节点的纯度，即节点中某一类别样本占总样本的比例。Y轴代表分类错误率，这里指若将节点中所有样本预测为多数类所产生的错误率。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_1.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_2.png)

由于我们使用多数类进行预测，最不确定、因而错误率最高的情况发生在50/50的均匀分割点。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_4.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_7.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_5.png)

我们的目标是找到远离这个中心点的分割，即希望Y轴上的错误率尽可能小。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_9.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_10.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_7.png)

从给定的方程可以看出，我们得到了一种直线关系。如果一个子集中75%的样本属于类别1，那么错误率将是25%。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_12.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_9.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_10.png)

如果你的纯度提高5%，例如达到80%，那么错误率将线性下降5%。因此，子集纯度每增加5%，错误率就线性下降5%，形成了直线关系。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_14.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_15.png)

---

![](img/8922ff76e149a7a1db1a381a38bf7a8e_12.png)

现在，让我们观察更复杂的信息熵函数所产生的曲线。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_17.png)

与预期一致，它在50/50分割点也具有最大的不纯度。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_14.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_19.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_15.png)

但在50/50分割点之外，它现在是弯曲的。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_17.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_21.png)

正是这种曲率使得分割能够继续进行。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_19.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_23.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_21.png)

为什么我们这里更复杂的方程会产生这种情况？

![](img/8922ff76e149a7a1db1a381a38bf7a8e_23.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_25.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_25.png)

让我们通过一个例子来理解原因。首先从分类错误率开始。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_27.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_27.png)

假设这是我们的父节点。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_29.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_29.png)

这些是子节点。左边的节点是之前看到的那个具有50/50分割的较小子集。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_31.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_31.png)

右边的节点是一个较大的子集，分割稍好一些，因此分类错误率较低。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_33.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_33.png)

现在，蓝色和红色节点的加权平均值将始终是连接这两点的直线上的另一个点。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_35.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_36.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_35.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_36.png)

因此，正如这里所示，它可能恰好落在父节点上，这将阻止我们的树进一步分割。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_38.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_38.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_39.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_39.png)

加权平均值将始终位于连接左右节点的直线上。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_41.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_41.png)

现在你可能会想，有了曲率，也许我们就不会有这个问题，因为它不是一条直线。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_43.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_43.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_44.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_44.png)

---

现在转到我们的信息熵曲线。曲率现在在直线上方产生了这个凸起。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_46.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_46.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_48.png)

我们可以回到父节点的例子，它位于我们的信息熵曲线上。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_50.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_48.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_52.png)

左边是那个较小子集的50/50分割节点。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_50.png)

右边是那个分割稍好一点的节点。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_54.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_55.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_52.png)

现在，加权平均值位于连接红色和蓝色节点的直线上，这保证会低于我们的父节点。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_57.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_54.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_55.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_59.png)

在这里，父节点的X值与加权平均值的X值将对齐。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_61.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_57.png)

这里的加权平均值确保我们获得的信息熵值小于父节点的值。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_63.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_59.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_61.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_64.png)

因此，我们现在确保了分割时能获得信息增益，从而可以继续分割，直到得到同质的节点。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_63.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_64.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_66.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_67.png)

---

考虑到这一点，另一个需要了解的流行度量是基尼指数，事实上，它是Scikit-learn中默认使用的度量。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_69.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_66.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_67.png)

与信息熵类似，它也具有那种凸起，因此能确保后续存在进行完美分割的可能性。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_71.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_69.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_73.png)

基尼指数相对于信息熵的一个优势是它不包含对数运算，因此计算更简便。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_75.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_71.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_73.png)

其方程非常简单：**`Gini = 1 - Σ(p_i²)`**，其中 `p_i` 是第 i 类在节点中的比例。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_75.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_77.png)

---

![](img/8922ff76e149a7a1db1a381a38bf7a8e_78.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_79.png)

我们在这里讨论了很多关于确保完美分割能力的内容，但归根结底，在所有叶节点都获得完美分割最终会导致过拟合，正如我们很早之前讨论过的。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_77.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_78.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_79.png)

因此，我们确实需要这种可能性，但我们知道，如果允许它完全下降到那些同质节点，就会发生过拟合。所以在下一个视频中，我们将探讨这一点，并像处理所有算法一样，讨论如何避免对训练集的过拟合并确保偏差与方差的正确平衡。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_81.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_81.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_82.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_84.png)
![](img/8922ff76e149a7a1db1a381a38bf7a8e_85.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_82.png)

好的，我们下个视频见。

![](img/8922ff76e149a7a1db1a381a38bf7a8e_87.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_84.png)

![](img/8922ff76e149a7a1db1a381a38bf7a8e_85.png)

---

![](img/8922ff76e149a7a1db1a381a38bf7a8e_87.png)

本节课中我们一起学习了决策树的不同分割准则。我们对比了分类错误率的线性特性与信息熵、基尼指数的非线性凸起特性，理解了后者如何通过确保信息增益来促进树的持续生长。同时，我们也认识到追求完美分割可能导致过拟合，为下一节讨论决策树的正则化做了铺垫。