#  007：线性分类器（第二部分）🚀

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_1.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_2.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_4.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_6.png)

在本节课中，我们将继续学习线性分类器。我们将完成猫狗分类问题的剩余步骤，包括寻找算法、运行算法和验证结果。你将编写并运行你的第一个机器学习算法。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_8.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_10.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_12.png)

上一节我们介绍了猫狗分类问题的数据、假设空间和损失函数。本节中，我们来看看如何找到最优的分类器参数。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_14.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_16.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_18.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_19.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_20.png)

我们的目标是找到一条最优的直线（线性分类器），能够最好地区分猫和狗的数据点。这条直线由参数 **θ₀**、**θ₁** 和 **θ₂** 决定，其方程为：
**θ₁x₁ + θ₂x₂ + θ₀ = 0**

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_22.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_23.png)

## 步骤四：寻找算法 🧠

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_25.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_27.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_29.png)

现在，我们进入机器学习项目的第四个步骤：寻找算法。我们将介绍第一个算法——随机线性分类器。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_30.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_32.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_34.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_36.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_38.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_40.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_42.png)

随机线性分类器算法的工作原理如下：

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_44.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_46.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_48.png)

**输入**：
*   数据集 **D**
*   一个超参数 **K**（例如，K=100）

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_50.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_52.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_54.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_56.png)

**输出**：
*   最优的参数 **θ₀**、**θ₁**、**θ₂**，即区分猫狗的最佳直线。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_58.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_60.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_62.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_63.png)

以下是该算法的三步流程：

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_65.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_66.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_67.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_69.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_71.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_72.png)

1.  **随机选择参数**：随机生成 **K** 组 **θ₀**、**θ₁**、**θ₂** 的值。这相当于随机画出 K 条不同的直线。
2.  **计算训练误差**：对于这 K 组参数（即 K 条直线），分别计算它们在训练数据上的分类错误数量。训练误差就是分类错误的样本数。
3.  **选择最优假设**：从所有尝试的直线中，选择训练误差最低的那一条直线所对应的参数。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_74.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_76.png)

直观地理解，这个算法就是尝试许多条随机生成的直线，然后挑出犯错最少的那一条。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_78.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_80.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_81.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_83.png)

## 步骤五：运行算法 ▶️

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_85.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_87.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_89.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_91.png)

上一节我们定义了算法，本节中我们来看看如何实际运行它。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_93.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_94.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_96.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_97.png)

运行随机线性分类器算法意味着用代码实现上述三步流程。你需要：
1.  编写代码来随机生成参数。
2.  编写代码来计算给定参数下的分类错误数。
3.  编写代码来比较所有尝试的结果，并保留最佳参数。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_99.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_100.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_101.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_102.png)

## 步骤六：验证结果 ✅

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_104.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_106.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_108.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_110.png)

算法运行完毕后，我们会得到一组“最优”参数。最后一步是验证这些参数和对应的分类器是否真的有效。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_112.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_113.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_114.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_116.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_118.png)

验证通常包括：
*   **检查训练集性能**：确认算法在训练数据上确实达到了低错误率。
*   **使用测试集**：在一个全新的、算法从未见过的数据集（测试集）上评估分类器的性能。这是检验模型泛化能力的关键。
*   **分析结果**：如果测试集上性能也很好，说明模型成功。如果性能下降很多，可能意味着模型过拟合了训练数据。

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_120.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_122.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_123.png)

![](img/a59ed9cb76c2c3d682a77f24ebbd8526_125.png)

本节课中我们一起学习了机器学习项目的完整流程，并以猫狗分类为例，完成了从定义问题到运行验证的所有步骤。我们重点介绍了随机线性分类器算法，这是一个简单但直观的入门算法，帮助你理解机器学习寻找最优解的基本思想。