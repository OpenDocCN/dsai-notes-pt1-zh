# 机器学习课程 P5：使用 Scikit-learn 管道 🚀

![](img/6759ee1a4176e478df5915cb285d57a7_0.png)

![](img/6759ee1a4176e478df5915cb285d57a7_2.png)

在本节课中，我们将学习如何使用 Scikit-learn 的管道（Pipeline）来构建和优化机器学习工作流。我们将通过一个预测芝加哥海滩波浪高度的实际案例，演示如何将数据预处理、特征转换和模型训练步骤优雅地串联起来。

![](img/6759ee1a4176e478df5915cb285d57a7_4.png)

![](img/6759ee1a4176e478df5915cb285d57a7_6.png)

![](img/6759ee1a4176e478df5915cb285d57a7_7.png)

## 概述

![](img/6759ee1a4176e478df5915cb285d57a7_9.png)

![](img/6759ee1a4176e478df5915cb285d57a7_11.png)

我们一直在学习使用线性回归模型。通常，在将数据输入模型进行分析之前，我们需要对其进行一系列转换。因此，我们的工作流最终会形成一条“管道”：先进行一系列数据转换，最后使用一个估计器（在 Scikit-learn 中称为 Estimator）进行预测。

![](img/6759ee1a4176e478df5915cb285d57a7_13.png)

本节课，我们将使用一个关于芝加哥密歇根湖沿岸海滩波浪测量的数据集。我们将尝试根据传感器数据（如波周期、水温、浊度等）以及海滩位置来预测波浪高度。

![](img/6759ee1a4176e478df5915cb285d57a7_15.png)

![](img/6759ee1a4176e478df5915cb285d57a7_17.png)

## 数据探索与问题定义

首先，让我们观察数据。下图展示了所有数据的散点图，X轴是波周期，Y轴是波高。从图中可以看出，波周期和波高之间的关系并非简单的线性关系。

![](img/6759ee1a4176e478df5915cb285d57a7_19.png)

![](img/6759ee1a4176e478df5915cb285d57a7_6.png)

当我们按不同海滩分别绘制数据时，可以发现不同海滩的波浪模式存在差异。这表明“海滩”是一个重要的预测变量。

![](img/6759ee1a4176e478df5915cb285d57a7_21.png)

![](img/6759ee1a4176e478df5915cb285d57a7_13.png)

![](img/6759ee1a4176e478df5915cb285d57a7_23.png)

![](img/6759ee1a4176e478df5915cb285d57a7_24.png)

基于以上观察，我们将构建四个不同的模型来对比分析：
1.  **模型1**：仅使用波周期进行简单的线性回归。
2.  **模型2**：使用波周期进行多项式回归（拟合曲线）。
3.  **模型3**：仅使用海滩名称进行分类预测。
4.  **模型4**：结合海滩名称和波周期（多项式）进行预测。

![](img/6759ee1a4176e478df5915cb285d57a7_26.png)

![](img/6759ee1a4176e478df5915cb285d57a7_28.png)

![](img/6759ee1a4176e478df5915cb285d57a7_29.png)

## 模型构建与评估

![](img/6759ee1a4176e478df5915cb285d57a7_31.png)

上一节我们定义了问题，本节我们将具体构建这四个模型并评估其性能。

![](img/6759ee1a4176e478df5915cb285d57a7_32.png)

![](img/6759ee1a4176e478df5915cb285d57a7_34.png)

### 准备工作：导入与数据分割

![](img/6759ee1a4176e478df5915cb285d57a7_36.png)

![](img/6759ee1a4176e478df5915cb285d57a7_37.png)

首先，我们导入必要的库并分割数据集。

![](img/6759ee1a4176e478df5915cb285d57a7_39.png)

![](img/6759ee1a4176e478df5915cb285d57a7_40.png)

```python
import pandas as pd
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import PolynomialFeatures, OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline

![](img/6759ee1a4176e478df5915cb285d57a7_42.png)

![](img/6759ee1a4176e478df5915cb285d57a7_43.png)

# 假设 df 是已加载并清理好的数据框
# 特征列包括 ‘Wave Period‘, ‘Water Temperature‘, ‘Beach Name‘ 等
# 目标列是 ‘Wave Height‘

![](img/6759ee1a4176e478df5915cb285d57a7_45.png)

![](img/6759ee1a4176e478df5915cb285d57a7_46.png)

![](img/6759ee1a4176e478df5915cb285d57a7_47.png)

# 分割数据
train_df, test_df = train_test_split(df, test_size=0.25, random_state=42)
print(f"训练集大小: {len(train_df)}， 测试集大小: {len(test_df)}")
```

![](img/6759ee1a4176e478df5915cb285d57a7_48.png)

![](img/6759ee1a4176e478df5915cb285d57a7_50.png)

![](img/6759ee1a4176e478df5915cb285d57a7_51.png)

### 模型1：基于波周期的简单线性回归

![](img/6759ee1a4176e478df5915cb285d57a7_53.png)

![](img/6759ee1a4176e478df5915cb285d57a7_55.png)

我们从最简单的模型开始，仅使用波周期进行线性拟合。

![](img/6759ee1a4176e478df5915cb285d57a7_57.png)

![](img/6759ee1a4176e478df5915cb285d57a7_59.png)

![](img/6759ee1a4176e478df5915cb285d57a7_61.png)

```python
# 定义特征和目标
features = [‘Wave Period‘]
X_train = train_df[features]
y_train = train_df[‘Wave Height‘]

![](img/6759ee1a4176e478df5915cb285d57a7_63.png)

# 创建并训练模型
model1 = LinearRegression()
model1.fit(X_train, y_train)

![](img/6759ee1a4176e478df5915cb285d57a7_65.png)

![](img/6759ee1a4176e478df5915cb285d57a7_66.png)

![](img/6759ee1a4176e478df5915cb285d57a7_68.png)

# 在测试集上评估
X_test = test_df[features]
y_test = test_df[‘Wave Height‘]
score1 = model1.score(X_test, y_test)
print(f"模型1测试集R²分数: {score1:.4f}")

![](img/6759ee1a4176e478df5915cb285d57a7_70.png)

# 使用交叉验证获得更稳健的评估
cv_scores1 = cross_val_score(model1, X_train, y_train, cv=5)
print(f"模型1交叉验证平均R²分数: {cv_scores1.mean():.4f}")
```

![](img/6759ee1a4176e478df5915cb285d57a7_72.png)

![](img/6759ee1a4176e478df5915cb285d57a7_73.png)

![](img/6759ee1a4176e478df5915cb285d57a7_75.png)

**结果分析**：模型1的得分非常低（甚至可能为负），这在意料之中，因为我们试图用一条直线去拟合明显非线性的关系。

![](img/6759ee1a4176e478df5915cb285d57a7_77.png)

![](img/6759ee1a4176e478df5915cb285d57a7_78.png)

![](img/6759ee1a4176e478df5915cb285d57a7_80.png)

### 模型2：基于波周期的多项式回归

![](img/6759ee1a4176e478df5915cb285d57a7_82.png)

![](img/6759ee1a4176e478df5915cb285d57a7_83.png)

由于关系非线性，我们尝试为模型提供更多信息，例如波周期的平方、立方等特征。Scikit-learn 的 `PolynomialFeatures` 转换器可以自动完成这项工作。

![](img/6759ee1a4176e478df5915cb285d57a7_85.png)

以下是手动创建多项式特征的示例：

![](img/6759ee1a4176e478df5915cb285d57a7_87.png)

![](img/6759ee1a4176e478df5915cb285d57a7_88.png)

![](img/6759ee1a4176e478df5915cb285d57a7_89.png)

![](img/6759ee1a4176e478df5915cb285d57a7_90.png)

```python
# 手动创建多项式特征示例
demo_df = train_df[[‘Wave Period‘]].copy()
demo_df[‘Period_squared‘] = demo_df[‘Wave Period‘] ** 2
demo_df[‘Period_cubed‘] = demo_df[‘Wave Period‘] ** 3
```

![](img/6759ee1a4176e478df5915cb285d57a7_91.png)

![](img/6759ee1a4176e478df5915cb285d57a7_93.png)

![](img/6759ee1a4176e478df5915cb285d57a7_94.png)

更优雅的方式是使用 `Pipeline` 将特征转换和模型训练结合在一起：

```python
# 使用管道构建多项式回归模型
model2 = Pipeline([
    (‘poly‘, PolynomialFeatures(degree=2, include_bias=False)), # 转换为二次多项式特征
    (‘lr‘, LinearRegression()) # 应用线性回归
])

![](img/6759ee1a4176e478df5915cb285d57a7_96.png)

model2.fit(X_train, y_train)
score2 = model2.score(X_test, y_test)
cv_scores2 = cross_val_score(model2, X_train, y_train, cv=5)
print(f"模型2测试集R²分数: {score2:.4f}")
print(f"模型2交叉验证平均R²分数: {cv_scores2.mean():.4f}")
```

![](img/6759ee1a4176e478df5915cb285d57a7_98.png)

![](img/6759ee1a4176e478df5915cb285d57a7_100.png)

**结果分析**：模型2的性能相比模型1有显著提升。通过引入多项式特征，线性回归模型现在可以拟合一条曲线，从而更好地捕捉数据模式。

![](img/6759ee1a4176e478df5915cb285d57a7_101.png)

![](img/6759ee1a4176e478df5915cb285d57a7_102.png)

![](img/6759ee1a4176e478df5915cb285d57a7_104.png)

### 模型3：基于海滩名称的预测

![](img/6759ee1a4176e478df5915cb285d57a7_105.png)

![](img/6759ee1a4176e478df5915cb285d57a7_106.png)

![](img/6759ee1a4176e478df5915cb285d57a7_107.png)

“海滩名称”是分类数据（字符串），不能直接用于线性回归。我们需要将其转换为数值形式。一种常见但错误的方法是给每个海滩分配一个数字（如1，2，3…），因为这会给模型强加一个不存在的顺序关系。

![](img/6759ee1a4176e478df5915cb285d57a7_109.png)

![](img/6759ee1a4176e478df5915cb285d57a7_110.png)

![](img/6759ee1a4176e478df5915cb285d57a7_111.png)

正确的方法是使用 **独热编码（One-Hot Encoding）**。它将一个具有 `n` 个类别的特征转换为 `n` 个二进制特征，每个特征代表一个类别是否存在。

![](img/6759ee1a4176e478df5915cb285d57a7_113.png)

```python
# 使用管道构建基于海滩的模型
features_cat = [‘Beach Name‘]
X_train_cat = train_df[features_cat]

![](img/6759ee1a4176e478df5915cb285d57a7_115.png)

![](img/6759ee1a4176e478df5915cb285d57a7_116.png)

![](img/6759ee1a4176e478df5915cb285d57a7_117.png)

model3 = Pipeline([
    (‘onehot‘, OneHotEncoder()), # 对海滩名称进行独热编码
    (‘lr‘, LinearRegression())
])

![](img/6759ee1a4176e478df5915cb285d57a7_119.png)

![](img/6759ee1a4176e478df5915cb285d57a7_121.png)

![](img/6759ee1a4176e478df5915cb285d57a7_123.png)

model3.fit(X_train_cat, y_train)

![](img/6759ee1a4176e478df5915cb285d57a7_125.png)

![](img/6759ee1a4176e478df5915cb285d57a7_127.png)

X_test_cat = test_df[features_cat]
score3 = model3.score(X_test_cat, y_test)
cv_scores3 = cross_val_score(model3, X_train_cat, y_train, cv=5)
print(f"模型3测试集R²分数: {score3:.4f}")
print(f"模型3交叉验证平均R²分数: {cv_scores3.mean():.4f}")
```

![](img/6759ee1a4176e478df5915cb285d57a7_129.png)

![](img/6759ee1a4176e478df5915cb285d57a7_131.png)

![](img/6759ee1a4176e478df5915cb285d57a7_132.png)

**结果分析**：仅使用海滩信息进行预测，其效果已经超过了只使用波周期的简单线性回归，甚至可能与多项式回归模型效果相当，这印证了海滩是一个强预测因子。

![](img/6759ee1a4176e478df5915cb285d57a7_134.png)

![](img/6759ee1a4176e478df5915cb285d57a7_136.png)

### 模型4：结合海滩与波周期（多项式）的模型

![](img/6759ee1a4176e478df5915cb285d57a7_137.png)

![](img/6759ee1a4176e478df5915cb285d57a7_138.png)

现在，我们希望结合两个信息源：分类变量“海滩名称”和数值变量“波周期”（需多项式扩展）。这需要使用 `ColumnTransformer` 来对数据框的不同列应用不同的转换。

![](img/6759ee1a4176e478df5915cb285d57a7_140.png)

```python
# 定义要使用的特征
features_combined = [‘Beach Name‘, ‘Wave Period‘]
X_train_combined = train_df[features_combined]

![](img/6759ee1a4176e478df5915cb285d57a7_142.png)

# 创建列转换器，指定不同列应用的转换
preprocessor = ColumnTransformer(
    transformers=[
        (‘beach‘, OneHotEncoder(), [‘Beach Name‘]), # 对‘Beach Name‘列进行独热编码
        (‘period‘, PolynomialFeatures(degree=2, include_bias=False), [‘Wave Period‘]) # 对‘Wave Period‘列进行多项式扩展
    ])

![](img/6759ee1a4176e478df5915cb285d57a7_144.png)

![](img/6759ee1a4176e478df5915cb285d57a7_146.png)

# 构建完整的管道
model4 = Pipeline([
    (‘preprocessor‘, preprocessor), # 第一步：应用列转换器
    (‘regressor‘, LinearRegression()) # 第二步：线性回归
])

![](img/6759ee1a4176e478df5915cb285d57a7_147.png)

![](img/6759ee1a4176e478df5915cb285d57a7_149.png)

model4.fit(X_train_combined, y_train)

![](img/6759ee1a4176e478df5915cb285d57a7_151.png)

![](img/6759ee1a4176e478df5915cb285d57a7_153.png)

X_test_combined = test_df[features_combined]
score4 = model4.score(X_test_combined, y_test)
cv_scores4 = cross_val_score(model4, X_train_combined, y_train, cv=5)
print(f"模型4测试集R²分数: {score4:.4f}")
print(f"模型4交叉验证平均R²分数: {cv_scores4.mean():.4f}")
```

![](img/6759ee1a4176e478df5915cb285d57a7_155.png)

![](img/6759ee1a4176e478df5915cb285d57a7_156.png)

**结果分析**：模型4结合了海滩信息和波周期的多项式特征，其性能是所有模型中最优的，能够解释更多的方差。这证明了组合多个相关特征的有效性。

![](img/6759ee1a4176e478df5915cb285d57a7_158.png)

## 模型对比与管道调试

![](img/6759ee1a4176e478df5915cb285d57a7_160.png)

![](img/6759ee1a4176e478df5915cb285d57a7_162.png)

![](img/6759ee1a4176e478df5915cb285d57a7_164.png)

我们可以方便地对比四个模型的交叉验证性能：

![](img/6759ee1a4176e478df5915cb285d57a7_165.png)

![](img/6759ee1a4176e478df5915cb285d57a7_167.png)

```python
models = [‘模型1: 周期线性‘, ‘模型2: 周期多项式‘, ‘模型3: 海滩‘, ‘模型4: 海滩+周期多项式‘]
cv_means = [cv_scores1.mean(), cv_scores2.mean(), cv_scores3.mean(), cv_scores4.mean()]

![](img/6759ee1a4176e478df5915cb285d57a7_168.png)

![](img/6759ee1a4176e478df5915cb285d57a7_170.png)

![](img/6759ee1a4176e478df5915cb285d57a7_171.png)

for name, score in zip(models, cv_means):
    print(f"{name}: 平均CV R² = {score:.3f}")
```

![](img/6759ee1a4176e478df5915cb285d57a7_173.png)

![](img/6759ee1a4176e478df5915cb285d57a7_174.png)

![](img/6759ee1a4176e478df5915cb285d57a7_175.png)

此外，管道对象可以像字典一样访问其步骤，便于调试：

![](img/6759ee1a4176e478df5915cb285d57a7_176.png)

```python
# 查看模型4预处理后的特征（转换后的数据）
transformed_data = model4.named_steps[‘preprocessor‘].fit_transform(X_train_combined)
# 可以将其转换为DataFrame以便查看（需要获取特征名称，此处略）
print(f"转换后特征形状: {transformed_data.shape}")
```

## 最终模型选择与测试

![](img/6759ee1a4176e478df5915cb285d57a7_178.png)

![](img/6759ee1a4176e478df5915cb285d57a7_179.png)

根据交叉验证结果，我们选择性能最好的模型4作为最终模型。我们使用全部训练数据重新拟合，并在预留的测试集上进行最终评估，这最能反映模型在未知数据上的泛化能力。

![](img/6759ee1a4176e478df5915cb285d57a7_181.png)

```python
# 使用最佳模型（model4）在测试集上进行最终预测和评估
final_predictions = model4.predict(X_test_combined)
final_score = model4.score(X_test_combined, y_test)
print(f"最终模型在测试集上的R²分数: {final_score:.4f}")
```

![](img/6759ee1a4176e478df5915cb285d57a7_183.png)

![](img/6759ee1a4176e478df5915cb285d57a7_185.png)

![](img/6759ee1a4176e478df5915cb285d57a7_187.png)

## 总结

![](img/6759ee1a4176e478df5915cb285d57a7_188.png)

在本节课中，我们一起学习了 Scikit-learn 管道的强大功能。通过构建从简单到复杂的四个模型，我们掌握了：

![](img/6759ee1a4176e478df5915cb285d57a7_190.png)

![](img/6759ee1a4176e478df5915cb285d57a7_192.png)

*   **`Pipeline`**：用于将数据预处理、特征工程和模型训练步骤顺序连接，使代码简洁且易于复用。
*   **`PolynomialFeatures`**：用于将数值特征转换为多项式特征，以捕捉非线性关系。
*   **`OneHotEncoder`**：用于将分类变量转换为机器学习模型可以理解的数值格式（独热编码）。
*   **`ColumnTransformer`**：用于对数据框的不同列并行应用不同的转换，是处理混合类型特征（数值+分类）的关键工具。

![](img/6759ee1a4176e478df5915cb285d57a7_194.png)

我们从仅能解释极少方差的简单线性模型出发，通过逐步引入多项式特征和分类信息，最终构建了一个能更好预测波浪高度的复合模型。这个过程清晰地展示了如何通过特征工程和管道化工作流来系统地改进机器学习模型。