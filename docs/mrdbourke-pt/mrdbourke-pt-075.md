#  75：绘制最佳模型预测 📊

![](img/5fe613290c0a54bf9b620caa134f356f_1.png)

![](img/5fe613290c0a54bf9b620caa134f356f_2.png)

在本节课中，我们将学习如何可视化我们训练好的深度学习模型的预测结果。我们将通过绘制图像及其对应的预测标签，直观地评估模型的性能。

---

## 概述

![](img/5fe613290c0a54bf9b620caa134f356f_4.png)

![](img/5fe613290c0a54bf9b620caa134f356f_6.png)

上一节我们介绍了如何评估模型的整体准确率。本节中，我们将遵循数据探索者的格言“可视化、可视化、再可视化”，通过绘制图像及其预测结果，更直观地理解模型的表现。

![](img/5fe613290c0a54bf9b620caa134f356f_8.png)

![](img/5fe613290c0a54bf9b620caa134f356f_10.png)

![](img/5fe613290c0a54bf9b620caa134f356f_12.png)

我们有一些预测类别，也有一些用于比较的真实标签。虽然可以通过数字比较，但可视化能提供更直观的洞察。我们将编写代码来绘制这些图像及其预测。

![](img/5fe613290c0a54bf9b620caa134f356f_14.png)

![](img/5fe613290c0a54bf9b620caa134f356f_16.png)

![](img/5fe613290c0a54bf9b620caa134f356f_18.png)

---

![](img/5fe613290c0a54bf9b620caa134f356f_20.png)

![](img/5fe613290c0a54bf9b620caa134f356f_21.png)

![](img/5fe613290c0a54bf9b620caa134f356f_22.png)

![](img/5fe613290c0a54bf9b620caa134f356f_24.png)

## 绘制预测图像

![](img/5fe613290c0a54bf9b620caa134f356f_26.png)

![](img/5fe613290c0a54bf9b620caa134f356f_27.png)

![](img/5fe613290c0a54bf9b620caa134f356f_29.png)

![](img/5fe613290c0a54bf9b620caa134f356f_31.png)

![](img/5fe613290c0a54bf9b620caa134f356f_32.png)

以下是绘制预测图像的步骤。我们将创建一个 3x3 的子图网格来展示 9 个随机测试样本。

首先，创建一个 Matplotlib 图形，并设置其尺寸。我们选择 3 行 3 列的布局，这在实践中效果很好。

![](img/5fe613290c0a54bf9b620caa134f356f_34.png)

![](img/5fe613290c0a54bf9b620caa134f356f_35.png)

![](img/5fe613290c0a54bf9b620caa134f356f_36.png)

![](img/5fe613290c0a54bf9b620caa134f356f_37.png)

```python
import matplotlib.pyplot as plt

![](img/5fe613290c0a54bf9b620caa134f356f_39.png)

![](img/5fe613290c0a54bf9b620caa134f356f_40.png)

![](img/5fe613290c0a54bf9b620caa134f356f_42.png)

![](img/5fe613290c0a54bf9b620caa134f356f_44.png)

fig = plt.figure(figsize=(9, 9))
rows, cols = 3, 3
```

![](img/5fe613290c0a54bf9b620caa134f356f_46.png)

![](img/5fe613290c0a54bf9b620caa134f356f_48.png)

![](img/5fe613290c0a54bf9b620caa134f356f_50.png)

![](img/5fe613290c0a54bf9b620caa134f356f_51.png)

接下来，我们遍历测试样本，为每个样本创建一个子图。

![](img/5fe613290c0a54bf9b620caa134f356f_53.png)

![](img/5fe613290c0a54bf9b620caa134f356f_55.png)

```python
for i, sample in enumerate(test_samples):
    # 创建子图，索引从1开始
    ax = fig.add_subplot(rows, cols, i+1)
```

![](img/5fe613290c0a54bf9b620caa134f356f_57.png)

![](img/5fe613290c0a54bf9b620caa134f356f_59.png)

在子图中，我们首先绘制目标图像。需要移除批次维度并使用灰度色彩映射。

![](img/5fe613290c0a54bf9b620caa134f356f_61.png)

```python
    # 绘制图像，移除批次维度
    plt.imshow(sample.squeeze(), cmap='gray')
```

![](img/5fe613290c0a54bf9b620caa134f356f_63.png)

![](img/5fe613290c0a54bf9b620caa134f356f_65.png)

然后，我们需要找到预测标签和真实标签的文本形式，以便于人类阅读。

![](img/5fe613290c0a54bf9b620caa134f356f_67.png)

![](img/5fe613290c0a54bf9b620caa134f356f_68.png)

```python
    # 获取预测标签的文本形式
    pred_label = class_names[pred_classes[i]]
    # 获取真实标签的文本形式
    truth_label = class_names[test_labels[i]]
```

![](img/5fe613290c0a54bf9b620caa134f356f_69.png)

![](img/5fe613290c0a54bf9b620caa134f356f_70.png)

![](img/5fe613290c0a54bf9b620caa134f356f_71.png)

![](img/5fe613290c0a54bf9b620caa134f356f_72.png)

![](img/5fe613290c0a54bf9b620caa134f356f_73.png)

![](img/5fe613290c0a54bf9b620caa134f356f_75.png)

![](img/5fe613290c0a54bf9b620caa134f356f_76.png)

最后，我们为每个子图创建一个标题，比较预测和真实标签。为了更直观，我们可以根据预测是否正确来改变标题的颜色。

![](img/5fe613290c0a54bf9b620caa134f356f_78.png)

![](img/5fe613290c0a54bf9b620caa134f356f_80.png)

```python
    # 创建标题文本
    title_text = f"Pred: {pred_label} | Truth: {truth_label}"
    
    # 根据预测是否正确设置标题颜色
    if pred_label == truth_label:
        color = 'green'  # 正确预测为绿色
    else:
        color = 'red'    # 错误预测为红色
    
    # 设置标题
    ax.set_title(title_text, fontsize=10, color=color)
    # 关闭坐标轴
    ax.axis('off')
```

![](img/5fe613290c0a54bf9b620caa134f356f_82.png)

完成循环后，显示图形。

```python
plt.tight_layout()
plt.show()
```

---

## 代码解析与效果

上述代码执行以下操作：
1.  遍历我们从测试数据集中随机选取的样本。
2.  为每个样本创建子图并绘制图像。
3.  通过索引 `class_names` 列表，将预测和真实标签的数值转换为文本。
4.  创建一个比较两者的标题，并根据预测正确性使用绿色或红色显示。

![](img/5fe613290c0a54bf9b620caa134f356f_84.png)

![](img/5fe613290c0a54bf9b620caa134f356f_86.png)

运行代码后，如果模型预测全部正确，我们将看到所有标题都是绿色的。这种可视化方式让我们能立即识别出模型的表现。例如，预测为“凉鞋”，真实标签也是“凉鞋”，这非常直观。

为了进一步探索，我们可以多次运行随机采样代码，观察模型在不同样本上的表现。有时模型会出错，例如将“外套”预测为“连衣裙”。通过可视化这些错误，我们可以分析模型可能混淆的类别，例如“T恤/上衣”和“衬衫”之间的区别。这不仅能评估模型，有时还能揭示数据标签本身可能存在的问题。

![](img/5fe613290c0a54bf9b620caa134f356f_88.png)

---

## 总结

本节课中，我们一起学习了如何可视化深度学习模型的预测结果。我们编写了代码来绘制测试图像，并对比显示模型的预测标签和真实标签，同时用颜色高亮显示预测的正确与否。

![](img/5fe613290c0a54bf9b620caa134f356f_90.png)

![](img/5fe613290c0a54bf9b620caa134f356f_92.png)

![](img/5fe613290c0a54bf9b620caa134f356f_94.png)

![](img/5fe613290c0a54bf9b620caa134f356f_95.png)

通过这种可视化方法，我们能够超越单纯的准确率数字，获得对模型性能更直观、更深入的理解。它帮助我们识别模型表现良好的地方，更重要的是，发现模型容易出错的特定模式或类别混淆情况。在下一节课中，我们将学习另一种强大的评估工具——混淆矩阵，来进一步量化分析这些错误。