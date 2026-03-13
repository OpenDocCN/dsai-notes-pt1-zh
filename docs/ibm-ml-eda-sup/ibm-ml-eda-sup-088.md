# 088：正则化详解（选修）第三部分 🧪

![](img/176e1a59db003165d346401f94420beb_0.png)

![](img/176e1a59db003165d346401f94420beb_2.png)

![](img/176e1a59db003165d346401f94420beb_4.png)

在本节课中，我们将学习如何应用Lasso回归与线性回归，并在一个保留测试集上比较它们的性能。我们将重点关注模型的R²分数、系数大小以及非零系数的数量，以理解正则化如何帮助模型更好地泛化。

![](img/176e1a59db003165d346401f94420beb_6.png)

---

![](img/176e1a59db003165d346401f94420beb_8.png)

![](img/176e1a59db003165d346401f94420beb_9.png)

![](img/176e1a59db003165d346401f94420beb_10.png)

## 实验设置与目标

上一节我们介绍了正则化的基本概念。本节中，我们将通过一个具体的代码实验来观察Lasso回归的效果。

![](img/176e1a59db003165d346401f94420beb_12.png)

![](img/176e1a59db003165d346401f94420beb_13.png)

![](img/176e1a59db003165d346401f94420beb_15.png)

实验的目标如下：
*   使用一个较小的alpha值（0.001）训练Lasso回归模型。
*   在保留测试集上评估其性能（R²分数）。
*   将其与普通线性回归模型进行比较。
*   比较两种模型系数的总绝对值和以及非零系数的数量。

![](img/176e1a59db003165d346401f94420beb_17.png)

我们预期Lasso回归能够降低系数的幅度，并减少模型中非零系数的数量。

---

![](img/176e1a59db003165d346401f94420beb_19.png)

![](img/176e1a59db003165d346401f94420beb_20.png)

![](img/176e1a59db003165d346401f94420beb_21.png)

![](img/176e1a59db003165d346401f94420beb_22.png)

## 第一部分：训练与评估Lasso回归

首先，我们需要初始化Lasso回归对象。当alpha值较小时，模型可能需要更多迭代次数才能收敛到最优解，因此我们需要增加`max_iter`参数。

![](img/176e1a59db003165d346401f94420beb_24.png)

![](img/176e1a59db003165d346401f94420beb_25.png)

![](img/176e1a59db003165d346401f94420beb_26.png)

以下是初始化并训练Lasso模型的代码步骤：

![](img/176e1a59db003165d346401f94420beb_28.png)

![](img/176e1a59db003165d346401f94420beb_29.png)

```python
# 初始化Lasso回归模型，alpha=0.001
lasso_001 = Lasso(alpha=0.001, max_iter=100000)

![](img/176e1a59db003165d346401f94420beb_31.png)

![](img/176e1a59db003165d346401f94420beb_33.png)

# 使用训练集拟合标准化器，并转换训练数据
X_train_s = s.fit_transform(X_train)

![](img/176e1a59db003165d346401f94420beb_35.png)

![](img/176e1a59db003165d346401f94420beb_37.png)

![](img/176e1a59db003165d346401f94420beb_38.png)

# 使用标准化后的训练数据拟合Lasso模型
lasso_001.fit(X_train_s, y_train)

![](img/176e1a59db003165d346401f94420beb_40.png)

# 使用拟合好的标准化器转换测试数据（注意：不是fit_transform）
X_test_s = s.transform(X_test)

![](img/176e1a59db003165d346401f94420beb_42.png)

![](img/176e1a59db003165d346401f94420beb_43.png)

# 使用训练好的模型进行预测
y_pred_lasso = lasso_001.predict(X_test_s)

![](img/176e1a59db003165d346401f94420beb_45.png)

# 计算并输出模型在测试集上的R²分数
r2_lasso = r2_score(y_test, y_pred_lasso)
print(f"Lasso R² Score: {r2_lasso}")
```

![](img/176e1a59db003165d346401f94420beb_46.png)

![](img/176e1a59db003165d346401f94420beb_48.png)

![](img/176e1a59db003165d346401f94420beb_49.png)

---

![](img/176e1a59db003165d346401f94420beb_51.png)

## 第二部分：训练与评估线性回归

![](img/176e1a59db003165d346401f94420beb_53.png)

接下来，我们对普通线性回归模型重复相同的过程，以便进行公平比较。

![](img/176e1a59db003165d346401f94420beb_55.png)

![](img/176e1a59db003165d346401f94420beb_56.png)

以下是相应的代码步骤：

![](img/176e1a59db003165d346401f94420beb_58.png)

![](img/176e1a59db003165d346401f94420beb_59.png)

```python
# 初始化线性回归模型
lr = LinearRegression()

![](img/176e1a59db003165d346401f94420beb_61.png)

![](img/176e1a59db003165d346401f94420beb_62.png)

![](img/176e1a59db003165d346401f94420beb_64.png)

# 使用相同的标准化训练数据拟合线性回归模型
lr.fit(X_train_s, y_train)

![](img/176e1a59db003165d346401f94420beb_66.png)

# 使用相同的标准化测试数据进行预测
y_pred_lr = lr.predict(X_test_s)

![](img/176e1a59db003165d346401f94420beb_68.png)

![](img/176e1a59db003165d346401f94420beb_70.png)

# 计算并输出线性回归的R²分数
r2_lr = r2_score(y_test, y_pred_lr)
print(f"Linear Regression R² Score: {r2_lr}")
```

![](img/176e1a59db003165d346401f94420beb_71.png)

![](img/176e1a59db003165d346401f94420beb_73.png)

为了比较模型的复杂度，我们还需要计算系数的总绝对值和以及非零系数的数量。

![](img/176e1a59db003165d346401f94420beb_75.png)

![](img/176e1a59db003165d346401f94420beb_76.png)

以下是计算这些指标的代码：

![](img/176e1a59db003165d346401f94420beb_78.png)

![](img/176e1a59db003165d346401f94420beb_80.png)

```python
# 计算Lasso模型系数的总绝对值和
lasso_coeff_sum = np.abs(lasso_001.coef_).sum()
print(f"Sum of absolute Lasso coefficients: {lasso_coeff_sum}")

![](img/176e1a59db003165d346401f94420beb_82.png)

# 计算Lasso模型中非零系数的数量
lasso_non_zero = np.sum(lasso_001.coef_ != 0)
print(f"Number of non-zero Lasso coefficients: {lasso_non_zero}")

![](img/176e1a59db003165d346401f94420beb_84.png)

![](img/176e1a59db003165d346401f94420beb_85.png)

# 计算线性回归模型系数的总绝对值和
lr_coeff_sum = np.abs(lr.coef_).sum()
print(f"Sum of absolute Linear Regression coefficients: {lr_coeff_sum}")

![](img/176e1a59db003165d346401f94420beb_86.png)

# 计算线性回归模型中非零系数的数量（通常全部非零）
lr_non_zero = np.sum(lr.coef_ != 0)
print(f"Number of non-zero Linear Regression coefficients: {lr_non_zero}")
```

---

![](img/176e1a59db003165d346401f94420beb_88.png)

![](img/176e1a59db003165d346401f94420beb_89.png)

![](img/176e1a59db003165d346401f94420beb_90.png)

![](img/176e1a59db003165d346401f94420beb_91.png)

## 第三部分：结果分析与比较

![](img/176e1a59db003165d346401f94420beb_93.png)

运行上述代码后，我们可以观察并分析结果。

![](img/176e1a59db003165d346401f94420beb_95.png)

![](img/176e1a59db003165d346401f94420beb_96.png)

![](img/176e1a59db003165d346401f94420beb_98.png)

假设我们得到以下输出：
*   Lasso R² Score: 0.868
*   Linear Regression R² Score: 0.855
*   Sum of absolute Lasso coefficients: 436
*   Sum of absolute Linear Regression coefficients: 1185
*   Number of non-zero Lasso coefficients: 89
*   Number of non-zero Linear Regression coefficients: 104

![](img/176e1a59db003165d346401f94420beb_99.png)

从结果可以看出：
1.  **性能**：在保留测试集上，Lasso回归（R²=0.868）比线性回归（R²=0.855）更好地解释了数据的变异，表明其泛化能力更强。
2.  **系数幅度**：Lasso模型系数的总绝对值（436）远小于线性回归（1185），说明Lasso有效缩减了系数，降低了模型复杂度。
3.  **特征选择**：Lasso模型将部分系数缩减为零（89个非零系数），实现了特征选择，而线性回归使用了所有特征（104个系数）。

![](img/176e1a59db003165d346401f94420beb_101.png)

这体现了正则化的核心思想：通过降低模型的方差，使其在未知数据上表现更稳定、更优。

![](img/176e1a59db003165d346401f94420beb_102.png)

![](img/176e1a59db003165d346401f94420beb_103.png)

---

![](img/176e1a59db003165d346401f94420beb_105.png)

![](img/176e1a59db003165d346401f94420beb_106.png)

## 第四部分：引入岭回归（Ridge Regression）

![](img/176e1a59db003165d346401f94420beb_108.png)

Lasso和岭回归的正则化形式相似。为了更全面地理解，我们快速引入岭回归进行比较。两者的代价函数主要区别在于惩罚项：Lasso使用系数的绝对值（L1范数），而岭回归使用系数的平方（L2范数）。

![](img/176e1a59db003165d346401f94420beb_110.png)

![](img/176e1a59db003165d346401f94420beb_111.png)

![](img/176e1a59db003165d346401f94420beb_112.png)

以下是使用相同alpha值（0.001）训练岭回归模型的代码：

![](img/176e1a59db003165d346401f94420beb_114.png)

![](img/176e1a59db003165d346401f94420beb_115.png)

```python
# 导入岭回归
from sklearn.linear_model import Ridge

![](img/176e1a59db003165d346401f94420beb_116.png)

# 初始化岭回归模型
ridge_001 = Ridge(alpha=0.001)

![](img/176e1a59db003165d346401f94420beb_118.png)

![](img/176e1a59db003165d346401f94420beb_119.png)

![](img/176e1a59db003165d346401f94420beb_120.png)

# 使用相同的标准化数据拟合模型
ridge_001.fit(X_train_s, y_train)

# 进行预测并计算R²分数
y_pred_ridge = ridge_001.predict(X_test_s)
r2_ridge = r2_score(y_test, y_pred_ridge)
print(f"Ridge R² Score: {r2_ridge}")

![](img/176e1a59db003165d346401f94420beb_122.png)

![](img/176e1a59db003165d346401f94420beb_123.png)

![](img/176e1a59db003165d346401f94420beb_124.png)

# 比较系数
ridge_coeff_sum = np.abs(ridge_001.coef_).sum()
ridge_non_zero = np.sum(ridge_001.coef_ != 0)

![](img/176e1a59db003165d346401f94420beb_125.png)

print(f"Sum of absolute Ridge coefficients: {ridge_coeff_sum}")
print(f"Number of non-zero Ridge coefficients: {ridge_non_zero}")
```

![](img/176e1a59db003165d346401f94420beb_127.png)

![](img/176e1a59db003165d346401f94420beb_128.png)

![](img/176e1a59db003165d346401f94420beb_129.png)

通常，对于相同的alpha值：
*   **岭回归**倾向于缩小所有系数，但很少会将系数精确设置为零。
*   **Lasso回归**则更可能产生精确的零系数，从而实现特征选择。

![](img/176e1a59db003165d346401f94420beb_131.png)

在本例中，我们可能会发现岭回归的系数总绝对值更高，且没有零系数，而Lasso则提供了一个更稀疏（更简单）的模型。对于这个特定的alpha，Lasso展现了更强的整体正则化效果。当然，最佳的正则化强度和类型（L1或L2）取决于具体数据和特征，需要通过交叉验证等技术来调优。

![](img/176e1a59db003165d346401f94420beb_133.png)

![](img/176e1a59db003165d346401f94420beb_135.png)

---

![](img/176e1a59db003165d346401f94420beb_137.png)

![](img/176e1a59db003165d346401f94420beb_138.png)

## 第五部分：关于数据标准化的关键说明

![](img/176e1a59db003165d346401f94420beb_140.png)

![](img/176e1a59db003165d346401f94420beb_141.png)

在应用正则化模型时，数据标准化的步骤至关重要。正确的做法是：

![](img/176e1a59db003165d346401f94420beb_143.png)

1.  **仅在训练集上** 计算标准化所需的参数（如均值、标准差），即调用 `fit_transform`。
2.  然后使用这些从训练集学到的参数去**转换测试集**，即调用 `transform`。

![](img/176e1a59db003165d346401f94420beb_145.png)

![](img/176e1a59db003165d346401f94420beb_146.png)

以下代码演示了错误与正确的做法：

![](img/176e1a59db003165d346401f94420beb_148.png)

![](img/176e1a59db003165d346401f94420beb_149.png)

```python
# ❌ 错误做法：在整个数据集上拟合标准化器（数据泄露）
s.fit(X_full) # X_full 包含训练和测试数据
X_train_s_wrong = s.transform(X_train)
X_test_s_wrong = s.transform(X_test)

# ✅ 正确做法：仅在训练集上拟合标准化器
s.fit(X_train) # 仅使用训练数据
X_train_s_correct = s.transform(X_train)
X_test_s_correct = s.transform(X_test) # 使用训练集学到的参数
```

![](img/176e1a59db003165d346401f94420beb_151.png)

![](img/176e1a59db003165d346401f94420beb_152.png)

![](img/176e1a59db003165d346401f94420beb_153.png)

对于普通的线性回归，错误做法可能不会导致R²分数变化，因为模型本身不受特征尺度影响。然而，对于Lasso、岭回归等将系数值直接纳入损失函数的模型，使用错误的标准化方法（即让测试集信息“泄露”到训练过程中）会导致得到有偏差的均值和标准差，从而影响正则化效果，最终得到不同的、不可靠的模型预测结果和评估分数。

![](img/176e1a59db003165d346401f94420beb_155.png)

![](img/176e1a59db003165d346401f94420beb_157.png)

因此，为了保证评估的公正性和模型的有效性，必须始终坚持：**只在训练集上进行 `fit` 或 `fit_transform` 操作，然后对测试集仅进行 `transform` 操作。**

![](img/176e1a59db003165d346401f94420beb_159.png)

---

![](img/176e1a59db003165d346401f94420beb_161.png)

![](img/176e1a59db003165d346401f94420beb_162.png)

![](img/176e1a59db003165d346401f94420beb_163.png)

## 总结

![](img/176e1a59db003165d346401f94420beb_165.png)

![](img/176e1a59db003165d346401f94420beb_167.png)

本节课中，我们一起完成了一个完整的正则化对比实验。

我们学习了：
1.  如何用代码实现Lasso回归和线性回归，并比较它们在测试集上的R²分数。
2.  如何量化并比较模型的复杂度，包括**系数总绝对值和**与**非零系数数量**。我们观察到Lasso回归能有效降低这两者。
3.  引入了岭回归，并理解了L1正则化（Lasso）与L2正则化（Ridge）在系数缩减模式上的关键区别：Lasso倾向于产生稀疏解。
4.  强调了在机器学习工作流中**正确进行数据标准化**的重要性，特别是对于涉及正则化的模型，必须防止数据泄露。

![](img/176e1a59db003165d346401f94420beb_169.png)

![](img/176e1a59db003165d346401f94420beb_170.png)

![](img/176e1a59db003165d346401f94420beb_171.png)

通过本实验，我们直观地看到了正则化如何通过控制模型复杂度来减少过拟合、提高泛化能力。这些知识是构建稳健机器学习模型的基础。