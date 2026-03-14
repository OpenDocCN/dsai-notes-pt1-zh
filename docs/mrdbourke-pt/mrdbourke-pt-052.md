# 52：训练与测试循环 🚂

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_0.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_2.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_3.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_4.png)

在本节课中，我们将学习如何为分类问题构建并运行一个完整的训练与测试循环。我们将从理解模型输出的原始值（logits）开始，逐步将其转换为预测概率和标签，并最终编写代码来训练和评估我们的第一个分类模型。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_5.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_6.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_7.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_9.png)

---

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_11.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_13.png)

## 概述

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_15.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_17.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_19.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_21.png)

在前几节中，我们为分类问题创建了一个模型。现在，我们将进入模型训练阶段。训练循环的核心步骤包括：前向传播、计算损失、反向传播和优化器更新。同时，我们还需要一个测试循环来评估模型在未见数据上的性能。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_23.png)

---

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_25.png)

## 核心概念：Logits、预测概率与预测标签

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_27.png)

在深入代码之前，我们需要理解几个关键概念。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_29.png)

**Logits** 是模型未经处理的原始输出，即模型各层 `forward` 函数直接产生的值。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_31.png)

为了将 logits 转换为更容易理解的**预测概率**，我们需要使用激活函数。对于二分类问题，我们使用 **Sigmoid** 函数；对于多分类问题，则使用 **Softmax** 函数。本节课我们处理的是二分类数据，因此使用 Sigmoid。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_33.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_35.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_36.png)

最后，我们需要将预测概率转换为**预测标签**。这是因为在评估模型时，我们需要将模型的预测格式与真实的测试标签格式保持一致，以便进行“苹果对苹果”的比较。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_38.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_40.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_42.png)

**公式表示如下：**
1.  **Logits (原始输出):** `y_logits = model(X)`
2.  **预测概率 (Sigmoid激活):** `y_pred_probs = torch.sigmoid(y_logits)`
3.  **预测标签 (四舍五入):** `y_pred_labels = torch.round(y_pred_probs)`

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_44.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_45.png)

---

## 设置环境与数据

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_47.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_49.png)

首先，我们需要设置随机种子以确保结果的可复现性，并将数据移动到目标设备（例如GPU）上，以编写设备无关的代码。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_51.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_53.png)

```python
# 设置随机种子
torch.manual_seed(42)
torch.cuda.manual_seed(42) # 如果使用CUDA设备

# 设置训练轮数
epochs = 100

# 将数据移动到目标设备（例如GPU）
X_train, y_train = X_train.to(device), y_train.to(device)
X_test, y_test = X_test.to(device), y_test.to(device)
```

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_55.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_56.png)

---

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_58.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_59.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_61.png)

## 构建训练循环

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_62.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_64.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_66.png)

训练循环包含几个标准步骤。以下是构建训练循环的具体过程。

**训练循环步骤：**
1.  将模型设置为训练模式：`model.train()`。
2.  进行前向传播，得到 logits。
3.  使用激活函数（Sigmoid）将 logits 转换为预测概率，再转换为预测标签。
4.  使用损失函数计算损失。注意，我们使用的是 `BCEWithLogitsLoss`，它期望接收 logits 作为输入，这比先使用 Sigmoid 再用 `BCELoss` 在数值上更稳定。
5.  使用自定义的准确率函数计算训练准确率。
6.  将优化器的梯度归零：`optimizer.zero_grad()`。
7.  执行反向传播计算梯度：`loss.backward()`。
8.  优化器根据梯度更新模型参数：`optimizer.step()`。

```python
# 训练循环示例代码框架
model_0.train()
for epoch in range(epochs):
    # 1. 前向传播
    y_logits = model_0(X_train).squeeze() # 移除多余的维度
    y_pred = torch.round(torch.sigmoid(y_logits)) # 从 logits -> 概率 -> 标签

    # 2. 计算损失和准确率
    loss = loss_fn(y_logits, y_train) # BCEWithLogitsLoss 接收 logits
    acc = accuracy_fn(y_true=y_train, y_pred=y_pred) # 自定义准确率函数

    # 3. 优化器梯度归零
    optimizer.zero_grad()

    # 4. 损失反向传播
    loss.backward()

    # 5. 优化器步进（更新参数）
    optimizer.step()
```

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_68.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_70.png)

---

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_72.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_73.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_74.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_75.png)

## 构建测试循环

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_76.png)

在测试循环中，我们使用模型在测试集上进行预测并评估性能。

**测试循环步骤：**
1.  将模型设置为评估模式：`model.eval()`。
2.  使用 `torch.inference_mode()` 上下文管理器进行前向传播，这会禁用梯度计算以提升效率和节省内存。
3.  计算测试集的 logits、预测概率和预测标签。
4.  计算测试损失和测试准确率。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_78.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_80.png)

```python
# 测试循环示例代码框架
model_0.eval()
with torch.inference_mode():
    # 1. 前向传播
    test_logits = model_0(X_test).squeeze()
    test_pred = torch.round(torch.sigmoid(test_logits))

    # 2. 计算测试损失和准确率
    test_loss = loss_fn(test_logits, y_test)
    test_acc = accuracy_fn(y_true=y_test, y_pred=test_pred)
```

---

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_82.png)

## 整合并运行循环

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_84.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_85.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_87.png)

现在，我们将训练和测试循环整合在一起，并定期打印输出结果以监控训练过程。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_89.png)

以下是整合后的循环结构，每10个epoch打印一次训练和测试的损失与准确率。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_90.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_91.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_92.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_93.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_95.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_96.png)

```python
for epoch in range(epochs):
    ### 训练步骤 ###
    model_0.train()
    y_logits = model_0(X_train).squeeze()
    y_pred = torch.round(torch.sigmoid(y_logits))
    loss = loss_fn(y_logits, y_train)
    acc = accuracy_fn(y_true=y_train, y_pred=y_pred)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

    ### 测试步骤 ###
    model_0.eval()
    with torch.inference_mode():
        test_logits = model_0(X_test).squeeze()
        test_pred = torch.round(torch.sigmoid(test_logits))
        test_loss = loss_fn(test_logits, y_test)
        test_acc = accuracy_fn(y_true=y_test, y_pred=test_pred)

    # 每10个epoch打印一次信息
    if epoch % 10 == 0:
        print(f"Epoch: {epoch} | Loss: {loss:.5f}, Acc: {acc:.2f}% | Test Loss: {test_loss:.5f}, Test Acc: {test_acc:.2f}%")
```

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_98.png)

运行此代码后，你可能会发现模型的损失没有明显下降，准确率也徘徊在50%左右（相当于随机猜测）。这表明模型没有有效地学习。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_100.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_101.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_103.png)

---

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_105.png)

## 可视化诊断问题

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_107.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_109.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_111.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_112.png)

当模型表现不佳时，可视化是强大的诊断工具。我们可以绘制模型的决策边界，直观地看它是如何对数据进行分类的。

我们使用一个预先编写好的辅助函数 `plot_decision_boundary` 来可视化。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_114.png)

```python
# 绘制决策边界以查看模型分类效果
plt.figure(figsize=(12, 6))
plt.subplot(1, 2, 1)
plt.title("Train")
plot_decision_boundary(model_0, X_train, y_train)
plt.subplot(1, 2, 2)
plt.title("Test")
plot_decision_boundary(model_0, X_test, y_test)
```

可视化结果会清晰地显示，模型试图用一条直线来分割圆形的数据。这正是问题所在：我们的模型仅由线性层构成，而线性层只能学习线性关系（直线）。对于非线性的圆形数据，仅用直线是无法有效分割的。

---

## 总结

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_116.png)

本节课中我们一起学习了如何构建一个完整的PyTorch训练与测试循环。我们涵盖了从理解logits、预测概率到预测标签的转换过程，并实现了包含前向传播、损失计算、反向传播和参数更新的标准循环。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_118.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_119.png)

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_120.png)

关键收获是，我们发现了当前线性模型在处理非线性数据（如圆形数据）时的局限性。模型只能尝试用直线分割数据，导致性能不佳。这为我们下一节课引入非线性激活函数来增强模型能力埋下了伏笔。

![](img/1ce9f6a4e2ebe16f980c6ec80d94093f_122.png)

在进入下一部分之前，你可以尝试将训练轮数增加到1000，观察模型性能是否有所改善，并思考其背后的原因。