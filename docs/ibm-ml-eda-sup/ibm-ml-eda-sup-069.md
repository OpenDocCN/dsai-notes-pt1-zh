# 069：交叉验证演示第二部分 🧪

![](img/195ec22cbacf9fa0cc96b582f6eb3531_1.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_2.png)

在本节课中，我们将学习如何在交叉验证流程中集成数据预处理步骤，并介绍Scikit-learn中的`Pipeline`（管道）和`cross_val_predict`（交叉验证预测）功能。这些工具能极大地简化代码，使机器学习工作流程更加高效和整洁。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_4.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_6.png)

上一节我们介绍了基本的K折交叉验证流程。本节中我们来看看如何将数据标准化等预处理步骤整合进去，并使用更高级的工具来优化代码。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_8.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_9.png)

---

![](img/195ec22cbacf9fa0cc96b582f6eb3531_11.png)

## 引入数据预处理步骤

![](img/195ec22cbacf9fa0cc96b582f6eb3531_13.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_14.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_15.png)

首先，我们像之前一样初始化一个空列表`scores`来存储分数，并创建线性回归对象`lr`。不同的是，我们现在要增加一个数据标准化的预处理步骤。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_17.png)

以下是初始化步骤的代码：
```python
scores = []
lr = LinearRegression()
s = StandardScaler()
```

![](img/195ec22cbacf9fa0cc96b582f6eb3531_19.png)

接着，我们运行相同的`for`循环，使用`KFold.split()`方法生成训练集和测试集的索引。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_21.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_22.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_23.png)

在循环内部，我们首先对训练集`X_train`进行拟合和转换，使用`StandardScaler`计算均值和标准差，并生成缩放后的新数据`X_train_s`。
```python
X_train_s = s.fit_transform(X_train)
```

数据缩放后，我们将缩放后的训练数据`X_train_s`和原始的训练目标变量`y_train`传入线性回归模型进行拟合。
```python
lr.fit(X_train_s, y_train)
```

![](img/195ec22cbacf9fa0cc96b582f6eb3531_25.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_26.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_27.png)

然后，我们必须使用从训练集`StandardScaler`学到的参数来转换测试集`X_test`，使其处于相同的尺度上。我们只需对测试集运行`.transform`方法。
```python
X_test_s = s.transform(X_test)
```

![](img/195ec22cbacf9fa0cc96b582f6eb3531_29.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_30.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_31.png)

接着，我们将转换后的测试数据`X_test_s`传入已拟合好的线性回归模型，得到测试集的预测值。
```python
predictions = lr.predict(X_test_s)
```

![](img/195ec22cbacf9fa0cc96b582f6eb3531_33.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_34.png)

最后，我们使用这些预测值与实际值`y_test`计算R²分数，并将每次交叉验证的分数添加到`scores`列表中。
```python
score = r2_score(y_test, predictions)
scores.append(score)
```

![](img/195ec22cbacf9fa0cc96b582f6eb3531_36.png)

运行后我们发现，对于普通的线性回归（没有正则化），是否进行数据缩放对性能分数没有影响。缩放对于后续要介绍的Lasso和Ridge等正则化方法才是必要的。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_38.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_39.png)

---

![](img/195ec22cbacf9fa0cc96b582f6eb3531_41.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_42.png)

## 使用Pipeline简化流程

![](img/195ec22cbacf9fa0cc96b582f6eb3531_43.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_45.png)

随着预处理步骤的增加，手动进行`fit_transform`和`transform`操作会变得非常繁琐。幸运的是，Scikit-learn提供了`Pipeline`功能来解决这个问题。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_46.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_48.png)

`Pipeline`允许你将多个对数据进行操作的步骤串联起来，只要每个步骤都有`fit`方法。并且，除了最后一步，前面的每一步都必须有`fit`和`transform`方法，这样前一步的输出才能作为下一步的输入。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_50.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_51.png)

以下是创建管道的步骤：
首先，我们重新初始化标准化器和线性回归器，然后从`sklearn.pipeline`导入`Pipeline`。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_53.png)

以下是创建并拟合管道的代码：
```python
from sklearn.pipeline import Pipeline

![](img/195ec22cbacf9fa0cc96b582f6eb3531_55.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_57.png)

s = StandardScaler()
lr = LinearRegression()

![](img/195ec22cbacf9fa0cc96b582f6eb3531_59.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_60.png)

estimator = Pipeline([('scale', s), ('lr', lr)])
estimator.fit(X_train, y_train)
```

![](img/195ec22cbacf9fa0cc96b582f6eb3531_62.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_63.png)

这个管道有两个步骤：首先缩放数据，然后将结果传递给线性回归。这使我们能够绕过之前手动进行`fit_transform`和`transform`的步骤。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_65.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_66.png)

管道拟合后，我们可以像使用普通模型一样调用`.predict`方法。
```python
predictions = estimator.predict(X_test)
```

![](img/195ec22cbacf9fa0cc96b582f6eb3531_68.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_70.png)

---

![](img/195ec22cbacf9fa0cc96b582f6eb3531_72.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_73.png)

## 使用cross_val_predict进行交叉验证预测

![](img/195ec22cbacf9fa0cc96b582f6eb3531_75.png)

现在，我们不再手动编写`for`循环来进行交叉验证，而是使用`cross_val_predict`函数。这个函数可以为我们自动完成数据拆分、训练和预测的过程。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_77.png)

首先，我们重新引入`KFold`对象，指定拆分数目和是否打乱数据。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_79.png)

以下是使用`cross_val_predict`的代码：
```python
from sklearn.model_selection import cross_val_predict

![](img/195ec22cbacf9fa0cc96b582f6eb3531_81.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_83.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_84.png)

kf = KFold(n_splits=3, shuffle=True)
predictions = cross_val_predict(estimator, X, y, cv=kf)
```

![](img/195ec22cbacf9fa0cc96b582f6eb3531_86.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_88.png)

我们将之前创建的管道`estimator`、特征数据`X`、目标数据`y`以及`KFold`对象`kf`传入`cross_val_predict`。函数会自动进行3折交叉验证：每次用2/3的数据训练，预测剩下的1/3，循环三次后，就得到了数据集中每一个样本的预测值（但每次预测使用的训练数据子集都不同）。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_90.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_92.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_94.png)

我们可以检查预测值的长度，它与原始数据框的长度相同。然后计算这些预测值与原始`y`值的R²分数，结果与我们手动循环得到的结果几乎一致。

**重要提示**：`cross_val_predict`函数在运行过程中并没有最终拟合一个模型。它生成了三个不同的模型（对应三次训练），并给出了所有预测结果。因此，在运行`cross_val_predict`之后，我们的`estimator`对象本身并没有被拟合（除非你之前单独拟合过它）。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_96.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_98.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_99.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_101.png)

---

![](img/195ec22cbacf9fa0cc96b582f6eb3531_103.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_105.png)

## 总结与下节预告

![](img/195ec22cbacf9fa0cc96b582f6eb3531_107.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_109.png)

本节课中我们一起学习了如何将数据预处理步骤整合到交叉验证流程中。我们介绍了Scikit-learn中强大的`Pipeline`工具，它可以将多个处理步骤串联，简化代码。我们还学习了`cross_val_predict`函数，它能自动完成交叉验证的拆分、训练和预测，进一步提高了效率。

![](img/195ec22cbacf9fa0cc96b582f6eb3531_111.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_112.png)

完成本节后，我们为下一阶段的学习打下了基础。下一节我们将讨论**超参数调优**。我们已经学习了模型如何学习参数，但超参数（如正则化强度、树的深度等）需要我们自己选择。现在，随着`cross_val_predict`的引入，我们可以开始思考：在众多可用的超参数中，我们该如何系统地选择和调整它们，以改变模型的输出并找到最佳性能呢？

![](img/195ec22cbacf9fa0cc96b582f6eb3531_114.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_115.png)

![](img/195ec22cbacf9fa0cc96b582f6eb3531_116.png)

我们下节课再见。