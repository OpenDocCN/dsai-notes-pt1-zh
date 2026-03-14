# 第二三四部分 15：探索性数据分析演示

![](img/19551fbead4605cec90f3bc34a33fd84_0.png)

在本节课中，我们将学习如何对数据集进行探索性数据分析。我们将从检查数据的基本信息开始，逐步深入到分析数据分布、处理缺失值与重复值，并最终进行趋势分析。

---

## 从数据概览开始

上一节我们介绍了数据集的基本情况，本节中我们来看看如何获取数据的详细信息。

现在，它将开始提供分类列中唯一值的信息，并可视化数值列的分布。可以看到它已经生成了解决方案。首先，它开始说明分类列的唯一值，并为此提供了分布可视化。这里甚至提供了代码。让我们展开查看。

以下是用于理解分类列唯一值的代码：

```python
# 第二三四部分 检查分类列的唯一值
categorical_columns = ['gender', 'item_purchased', 'category', 'location', 'size']
for col in categorical_columns:
    unique_vals = df[col].unique()
    print(f"列 '{col}' 的唯一值: {unique_vals}")
```

![](img/19551fbead4605cec90f3bc34a33fd84_2.png)

![](img/19551fbead4605cec90f3bc34a33fd84_3.png)

这里的结果是分类列的唯一值，例如性别、购买物品、类别、位置、尺寸等。

![](img/19551fbead4605cec90f3bc34a33fd84_4.png)

![](img/19551fbead4605cec90f3bc34a33fd84_5.png)

![](img/19551fbead4605cec90f3bc34a33fd84_6.png)

---

![](img/19551fbead4605cec90f3bc34a33fd84_7.png)

![](img/19551fbead4605cec90f3bc34a33fd84_8.png)

## 可视化数值列分布

![](img/19551fbead4605cec90f3bc34a33fd84_9.png)

![](img/19551fbead4605cec90f3bc34a33fd84_10.png)

接下来，他们提供了部分关键数值列的分布可视化。他们重点关注了年龄、购买金额、评分和先前购买次数。

以下是用于生成可视化的代码：

```python
import matplotlib.pyplot as plt
import seaborn as sns

# 第二三四部分 选择要可视化的数值列
numerical_columns = ['age', 'purchase_amount', 'review_rating', 'previous_purchases']

# 第二三四部分 创建子图
fig, axes = plt.subplots(2, 2, figsize=(12, 10))
axes = axes.ravel()

for i, col in enumerate(numerical_columns):
    sns.histplot(df[col], kde=True, ax=axes[i])
    axes[i].set_title(f'{col} 的分布')
    axes[i].set_xlabel(col)
    axes[i].set_ylabel('频数')

plt.tight_layout()
plt.show()
```

![](img/19551fbead4605cec90f3bc34a33fd84_12.png)

![](img/19551fbead4605cec90f3bc34a33fd84_13.png)

你可以看到可视化的输出。这里是所选数值列的分布图：购买金额的分布、评分的分布以及先前购买次数的分布。这些可视化提供了对客户数据特征和行为的洞察。

![](img/19551fbead4605cec90f3bc34a33fd84_14.png)

你希望继续EDA的其他步骤，还是专注于任何特定的分析？

![](img/19551fbead4605cec90f3bc34a33fd84_16.png)

---

## 处理数据质量问题

现在我想再问一个问题，因为我想非常确定是否存在缺失值。如果有，我们如何处理它们，以及如何识别这些问题对我来说非常重要。

现在，我向我的助手提问：数据中是否存在任何缺失值？处理这些缺失值的方法是什么？如何识别并删除重复值（如果有的话）？同时，验证每列的数据类型，是否有需要转换的？最后，我是否遗漏了什么？

现在，我们得到了解决方案。因为我明确提到了如果存在任何缺失值，以及如何处理它们，删除重复值（如果有的话），并验证数据类型。它开始为我提供解决方案。

**缺失值处理**：我们之前已经检查过缺失值，但我会再次确认并处理缺失值。

**重复值处理**：它将检查并从数据集中删除任何重复的行。

**数据类型验证**：现在你可以看到相关的代码。

![](img/19551fbead4605cec90f3bc34a33fd84_18.png)

以下是检查缺失值的代码：

![](img/19551fbead4605cec90f3bc34a33fd84_19.png)

![](img/19551fbead4605cec90f3bc34a33fd84_20.png)

![](img/19551fbead4605cec90f3bc34a33fd84_21.png)

```python
# 第二三四部分 检查缺失值
missing_values = df.isnull().sum()
print("缺失值统计:")
print(missing_values)
```

![](img/19551fbead4605cec90f3bc34a33fd84_22.png)

![](img/19551fbead4605cec90f3bc34a33fd84_23.png)

结果显示有零个缺失值。

现在你可以看到，如果有任何重复值，识别并删除它们。以下是删除重复值后的行数以及被删除的重复值数量。结果是零，意味着其中没有重复值。

现在，你可以使用以下代码检查数据类型验证：

```python
# 第二三四部分 验证数据类型
data_types = df.dtypes
print("各列数据类型:")
print(data_types)
```

现在它询问：你是否希望将特定列转换为分类数据类型，还是继续进行其他分析？我对这个结果感到满意。

---

## 进行趋势分析

![](img/19551fbead4605cec90f3bc34a33fd84_25.png)

现在我告诉它继续进行其他分析。

![](img/19551fbead4605cec90f3bc34a33fd84_26.png)

![](img/19551fbead4605cec90f3bc34a33fd84_27.png)

我们得到了解决方案，因为我提到要进行其他分析。它提供了我们可以执行的各种分析：客户细分、趋势分析、基于位置、评分、支付方式的表现分析、流失分析和相关性分析。

![](img/19551fbead4605cec90f3bc34a33fd84_28.png)

![](img/19551fbead4605cec90f3bc34a33fd84_29.png)

对于这些细节，我想了解趋势分析。所以我提到了趋势分析。它询问：请告诉我你想深入研究哪种分析，或者你是否有任何具体的问题或分析想法。我选择了趋势分析。

现在，我们开始。它询问，因为我请求进行趋势分析。它说明：要进行全面的趋势分析，时间相关数据是必不可少的。这将允许我们跟踪销售情况、客户访问量和产品偏好随时间的变化。

然而，我们的数据集没有特定的日期相关列。因此，它说明：鉴于这个限制，我们仍然可以基于数据中提供的“季节”列来推导趋势。我们可以按季节分析销售趋势、按季节分析平均支出、按季节分析热门商品以及按季节分析评分。

我想分析按季节划分的热门商品。让我们看看它会生成什么。

现在，我们得到了输出：按季节划分的热门商品。为了确定每个季节最受欢迎的商品，我们将按季节和购买物品对数据进行分组。

以下是按季节和购买物品分组并统计出现次数的代码：

```python
# 第二三四部分 按季节和物品分组，统计购买次数
popular_items_by_season = df.groupby(['season', 'item_purchased']).size().reset_index(name='count')

![](img/19551fbead4605cec90f3bc34a33fd84_31.png)

# 第二三四部分 找出每个季节最受欢迎的物品
most_popular_by_season = popular_items_by_season.loc[popular_items_by_season.groupby('season')['count'].idxmax()]
print(most_popular_by_season)
```

![](img/19551fbead4605cec90f3bc34a33fd84_32.png)

![](img/19551fbead4605cec90f3bc34a33fd84_33.png)

这里还有一个演示：在秋季，最受欢迎的商品是夹克，有54次购买；在春季是运动衫；在夏季是裤子；在冬季是太阳镜。

![](img/19551fbead4605cec90f3bc34a33fd84_34.png)

现在它询问：了解到像夹克在秋季很受欢迎是很有趣的，这是由于天气较冷所预期的。然而，太阳镜在冬季最受欢迎是令人惊讶的，可能受到其他因素或促销活动的影响。

你想深入研究任何其他趋势或分析吗？

---

## 总结与展望

现在我告诉它，我想进行进一步的分析。我对目前的理解感到满意，但让我们在接下来的视频中更深入地探讨EDA的下一部分。我们将继续我们的数据探索之旅，基于目前所学的内容。

敬请期待。

---

![](img/19551fbead4605cec90f3bc34a33fd84_36.png)

在本节课中，我们一起学习了探索性数据分析的基本步骤。我们从检查数据的基本信息和分布开始，然后处理了数据质量问题，如缺失值和重复值。最后，我们基于现有数据进行了趋势分析，例如按季节查找热门商品。这些步骤是理解数据集和为进一步分析做准备的基础。