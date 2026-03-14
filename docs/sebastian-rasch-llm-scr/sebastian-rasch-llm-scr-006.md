# 6：分类微调 🎯

![](img/a21651512054bcc1fc26be3f4331c4ce_1.png)

在本章中，我们将学习如何对预训练好的GPT模型进行微调，使其能够执行文本分类任务。我们将以垃圾邮件分类为例，展示如何将通用语言模型适配到特定的下游应用。

![](img/a21651512054bcc1fc26be3f4331c4ce_3.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_4.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_6.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_8.png)

---

![](img/a21651512054bcc1fc26be3f4331c4ce_10.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_12.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_14.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_16.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_17.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_19.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_21.png)

## 数据集准备 📊

![](img/a21651512054bcc1fc26be3f4331c4ce_23.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_24.png)

上一节我们完成了模型的预训练。本节中，我们来看看如何为分类任务准备数据。

我们将使用一个公开的短信垃圾邮件数据集。以下是准备步骤：

1.  **下载并加载数据**：从UCI机器学习仓库下载数据集，并使用Pandas加载。
    ```python
    import pandas as pd
    df = pd.read_csv('SMSSpamCollection.tsv', sep='\t', header=None, names=['label', 'text'])
    ```

![](img/a21651512054bcc1fc26be3f4331c4ce_26.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_28.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_29.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_31.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_32.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_34.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_36.png)

2.  **平衡数据集**：原始数据中非垃圾邮件（ham）远多于垃圾邮件（spam）。我们进行下采样以平衡类别。
    ```python
    spam_count = df['label'].value_counts()['spam']
    ham_balanced = df[df['label'] == 'ham'].sample(n=spam_count, random_state=123)
    balanced_df = pd.concat([ham_balanced, df[df['label'] == 'spam']])
    ```

![](img/a21651512054bcc1fc26be3f4331c4ce_38.png)

3.  **编码标签**：将文本标签（‘ham’, ‘spam’）转换为数字标签（0, 1）。
    ```python
    label_map = {'ham': 0, 'spam': 1}
    balanced_df['label'] = balanced_df['label'].map(label_map)
    ```

4.  **划分数据集**：将数据按70%（训练）、10%（验证）、20%（测试）的比例划分。
    ```python
    def random_split(df, train_frac, val_frac):
        df_shuffled = df.sample(frac=1, random_state=123).reset_index(drop=True)
        train_end = int(len(df) * train_frac)
        val_end = train_end + int(len(df) * val_frac)
        train_df = df_shuffled[:train_end]
        val_df = df_shuffled[train_end:val_end]
        test_df = df_shuffled[val_end:]
        return train_df, val_df, test_df
    ```

---

## 创建数据加载器 🔄

准备好数据集后，我们需要创建PyTorch数据加载器（DataLoader）以供训练使用。这里有一个关键点：文本序列长度不一致。

![](img/a21651512054bcc1fc26be3f4331c4ce_40.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_41.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_43.png)

为了解决这个问题，我们需要将所有序列填充（pad）到相同的长度。我们选择使用数据集中最长序列的长度作为标准长度，并使用特定的填充标记（pad token）进行填充。

![](img/a21651512054bcc1fc26be3f4331c4ce_45.png)

以下是实现自定义数据集（Dataset）类的核心代码：

```python
class SpamDataset(Dataset):
    def __init__(self, csv_path, tokenizer, max_length=None, pad_token_id=50256):
        self.data = pd.read_csv(csv_path)
        self.tokenizer = tokenizer
        self.pad_token_id = pad_token_id

        # 对文本进行编码（分词）
        self.encoded_texts = [self.tokenizer.encode(text) for text in self.data['text']]

        # 确定最大长度
        if max_length is None:
            self.max_length = max(len(seq) for seq in self.encoded_texts)
        else:
            self.max_length = max_length

        # 对所有序列进行填充或截断
        self.padded_texts = []
        for seq in self.encoded_texts:
            if len(seq) > self.max_length:
                seq = seq[:self.max_length] # 截断
            else:
                # 填充
                pad_len = self.max_length - len(seq)
                seq = seq + [self.pad_token_id] * pad_len
            self.padded_texts.append(seq)

    def __len__(self):
        return len(self.data)

    def __getitem__(self, idx):
        input_ids = torch.tensor(self.padded_texts[idx])
        label = torch.tensor(self.data.iloc[idx]['label'])
        return input_ids, label
```

创建好数据集后，就可以用`DataLoader`进行包装，设置批次大小（batch size）并启用数据打乱（shuffle）。

---

## 加载预训练模型 🏗️

![](img/a21651512054bcc1fc26be3f4331c4ce_47.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_48.png)

在开始微调之前，我们需要加载基础模型。我们将复用第5章的代码，加载OpenAI发布的最小GPT-2模型（124M参数）的权重。

```python
from previous_chapters import GPTModel, download_and_load_gpt2

model_config = GPTConfig()
model = GPTModel(model_config)
model_size = sum(p.numel() for p in model.parameters())
print(f"Model size: {model_size:,} parameters")

![](img/a21651512054bcc1fc26be3f4331c4ce_50.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_51.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_53.png)

# 下载并加载预训练权重
model = download_and_load_gpt2(model, model_config, model_size)
```

![](img/a21651512054bcc1fc26be3f4331c4ce_55.png)

加载完成后，我们可以测试模型是否工作正常。但请注意，此时的预训练模型仅能生成文本，无法直接理解指令或进行分类。

![](img/a21651512054bcc1fc26be3f4331c4ce_57.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_58.png)

---

![](img/a21651512054bcc1fc26be3f4331c4ce_60.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_61.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_63.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_64.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_66.png)

## 修改模型架构以适应分类 🛠️

![](img/a21651512054bcc1fc26be3f4331c4ce_68.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_70.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_71.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_73.png)

原始的GPT模型输出层维度是词表大小（例如50,257），用于预测下一个词。对于二分类任务，我们只需要两个输出节点。

![](img/a21651512054bcc1fc26be3f4331c4ce_75.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_76.png)

因此，我们需要替换模型的输出层，并冻结（freeze）大部分预训练参数，只微调最后几层，这是一种高效且能防止过拟合的策略。

![](img/a21651512054bcc1fc26be3f4331c4ce_78.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_80.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_81.png)

以下是修改步骤：

1.  **冻结所有权重**：首先，将模型中所有参数的`requires_grad`属性设为`False`，使其在训练中不被更新。
    ```python
    for param in model.parameters():
        param.requires_grad = False
    ```

2.  **替换输出层**：将原来的输出层（`model.out_head`）替换为一个新的线性层，其输出维度为类别数（2）。
    ```python
    embed_dim = model.config.emb_dim
    num_classes = 2
    torch.manual_seed(123)
    model.out_head = nn.Linear(embed_dim, num_classes)
    ```

3.  **解冻部分层**：为了使模型更好地适应新任务，我们解冻（unfreeze）最后一个Transformer块和最后的层归一化（LayerNorm），允许它们参与微调。
    ```python
    # 解冻最后一个Transformer块
    for param in model.trf_blocks[-1].parameters():
        param.requires_grad = True
    # 解冻最后的层归一化
    for param in model.final_norm.parameters():
        param.requires_grad = True
    ```

![](img/a21651512054bcc1fc26be3f4331c4ce_83.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_85.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_87.png)

修改后，模型对于给定输入，将输出一个形状为`[batch_size, 2]`的张量，代表两个类别的原始分数（logits）。

![](img/a21651512054bcc1fc26be3f4331c4ce_89.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_90.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_92.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_94.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_96.png)

---

![](img/a21651512054bcc1fc26be3f4331c4ce_98.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_100.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_101.png)

## 实现评估指标：损失与准确率 📈

![](img/a21651512054bcc1fc26be3f4331c4ce_103.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_104.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_106.png)

在开始训练循环前，我们需要定义如何计算分类损失和准确率。这是评估模型性能的关键。

![](img/a21651512054bcc1fc26be3f4331c4ce_108.png)

**核心思想**：对于分类任务，我们只关心模型对输入序列**最后一个位置**的输出。因为得益于自注意力机制中的因果掩码（causal mask），最后一个位置的上下文向量包含了之前所有令牌的信息。

![](img/a21651512054bcc1fc26be3f4331c4ce_110.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_112.png)

1.  **计算预测标签**：获取最后一个位置的logits，并找出值最大的索引，即为预测的类别。
    ```python
    with torch.no_grad():
        logits = model(input_ids) # shape: [batch_size, seq_len, 2]
        last_token_logits = logits[:, -1, :] # 取最后一个位置的输出
        predicted_labels = torch.argmax(last_token_logits, dim=-1)
    ```

2.  **计算准确率**：比较预测标签与真实标签，统计正确的预测数量。
    ```python
    def calc_accuracy_loader(data_loader, model, device, num_batches=None):
        model.eval()
        correct_predictions, num_examples = 0, 0
        with torch.no_grad():
            for i, (input_batch, target_batch) in enumerate(data_loader):
                if num_batches is not None and i >= num_batches:
                    break
                input_batch, target_batch = input_batch.to(device), target_batch.to(device)
                logits = model(input_batch)
                last_token_logits = logits[:, -1, :]
                predicted_labels = torch.argmax(last_token_logits, dim=-1)
                num_examples += predicted_labels.shape[0]
                correct_predictions += (predicted_labels == target_batch).sum().item()
        accuracy = correct_predictions / num_examples
        return accuracy
    ```

3.  **计算损失**：同样基于最后一个位置的logits，使用交叉熵损失（CrossEntropyLoss）。
    ```python
    def calc_loss_batch(input_batch, target_batch, model, device):
        input_batch, target_batch = input_batch.to(device), target_batch.to(device)
        logits = model(input_batch)
        last_token_logits = logits[:, -1, :] # 形状: [batch_size, 2]
        loss = torch.nn.functional.cross_entropy(last_token_logits, target_batch)
        return loss
    ```

---

## 微调模型 🚀

现在，所有准备工作就绪，我们可以开始微调模型了。训练循环与第5章的预训练类似，但目标变成了最小化分类损失。

以下是训练循环的核心步骤：

![](img/a21651512054bcc1fc26be3f4331c4ce_114.png)

1.  将模型设置为训练模式（`model.train()`）。
2.  遍历数据加载器中的每个批次（batch）。
3.  清零优化器的梯度（`optimizer.zero_grad()`）。
4.  将数据移动到指定设备（如GPU），并前向传播计算损失。
5.  执行反向传播计算梯度（`loss.backward()`）。
6.  使用优化器更新模型参数（`optimizer.step()`）。
7.  定期在验证集上评估损失和准确率，以监控训练进度和泛化能力。

![](img/a21651512054bcc1fc26be3f4331c4ce_116.png)

我们使用AdamW优化器，并设置适当的学习率和权重衰减（weight decay）来防止过拟合。

```python
import time
start_time = time.time()

train_losses, val_losses = [], []
train_accs, val_accs = [], []

optimizer = torch.optim.AdamW(model.parameters(), lr=5e-5, weight_decay=0.1)

![](img/a21651512054bcc1fc26be3f4331c4ce_118.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_119.png)

for epoch in range(num_epochs):
    model.train()
    for batch_idx, (input_batch, target_batch) in enumerate(train_loader):
        # ... 训练步骤 ...
        pass

    # 每个epoch结束后，在训练集和验证集上计算准确率
    train_acc = calc_accuracy_loader(train_loader, model, device, num_batches=5)
    val_acc = calc_accuracy_loader(val_loader, model, device, num_batches=5)
    train_accs.append(train_acc)
    val_accs.append(val_acc)
    print(f"Epoch {epoch+1}: Train Acc {train_acc:.3f}, Val Acc {val_acc:.3f}")

![](img/a21651512054bcc1fc26be3f4331c4ce_121.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_122.png)

end_time = time.time()
execution_time_minutes = (end_time - start_time) / 60
print(f"Training completed in {execution_time_minutes:.2f} minutes.")
```

![](img/a21651512054bcc1fc26be3f4331c4ce_124.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_125.png)

训练完成后，我们可以在独立的测试集上评估模型的最终性能，获得一个无偏的准确率估计。

---

![](img/a21651512054bcc1fc26be3f4331c4ce_127.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_129.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_131.png)

## 使用微调后的模型进行预测 🔮

![](img/a21651512054bcc1fc26be3f4331c4ce_133.png)

模型训练并评估完成后，我们就可以用它来对新的文本消息进行分类了。

![](img/a21651512054bcc1fc26be3f4331c4ce_135.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_137.png)

我们需要编写一个便利函数，其流程与训练时的数据处理和模型前向传播一致：

![](img/a21651512054bcc1fc26be3f4331c4ce_139.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_140.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_142.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_144.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_145.png)

1.  使用相同的分词器（tokenizer）对新文本进行编码。
2.  将编码后的序列填充（或截断）到训练时使用的固定长度（`max_length`）。
3.  添加批次维度，并将数据移动到模型所在的设备。
4.  将模型设置为评估模式（`model.eval()`），在不计算梯度的上下文（`torch.no_grad()`）中进行前向传播。
5.  获取最后一个位置的logits，并取argmax得到预测的类别（0或1）。

![](img/a21651512054bcc1fc26be3f4331c4ce_146.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_147.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_148.png)

```python
def classify_text(text, model, tokenizer, device, max_length, pad_token_id=50256):
    model.eval()
    # 1. 编码文本
    input_ids = tokenizer.encode(text)
    # 2. 填充/截断
    if len(input_ids) > max_length:
        input_ids = input_ids[:max_length]
    else:
        pad_len = max_length - len(input_ids)
        input_ids = input_ids + [pad_token_id] * pad_len
    # 3. 转换为张量并添加批次维度
    input_tensor = torch.tensor(input_ids, device=device).unsqueeze(0) # shape: [1, max_length]
    # 4. 前向传播
    with torch.no_grad():
        logits = model(input_tensor)
        last_token_logits = logits[:, -1, :]
        predicted_label = torch.argmax(last_token_logits, dim=-1).item()
    # 5. 返回结果
    return "spam" if predicted_label == 1 else "not spam"

# 示例用法
new_text = "Congratulations! You've won a free vacation. Call now to claim."
prediction = classify_text(new_text, model, tokenizer, device, max_length=120)
print(f"Prediction: {prediction}")
```

![](img/a21651512054bcc1fc26be3f4331c4ce_150.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_152.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_154.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_156.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_158.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_159.png)

最后，别忘了保存微调好的模型，以便后续部署或使用。

![](img/a21651512054bcc1fc26be3f4331c4ce_161.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_163.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_164.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_165.png)

```python
torch.save(model.state_dict(), "text_classifier.pth")
```

---

## 总结 📝

本节课中我们一起学习了如何将预训练的大语言模型（GPT）微调用于文本分类任务。我们完成了从数据准备、模型修改、训练循环到最终预测的完整流程。

核心步骤包括：
1.  **准备数据**：加载、平衡、划分数据集，并创建支持填充的数据加载器。
2.  **加载与修改模型**：加载预训练权重，冻结大部分参数，并替换输出层以适应分类任务。
3.  **定义评估指标**：实现基于最后一个令牌输出的准确率和损失计算函数。
4.  **执行微调**：使用优化器在训练集上最小化分类损失，并监控验证集性能。
5.  **评估与使用**：在测试集上评估最终模型，并编写函数对新文本进行分类。

![](img/a21651512054bcc1fc26be3f4331c4ce_167.png)

![](img/a21651512054bcc1fc26be3f4331c4ce_168.png)

这种方法不仅适用于垃圾邮件分类，还可以轻松扩展到情感分析、新闻分类、意图识别等多种文本分类场景。通过微调预训练模型，我们能够以相对较少的数据和计算资源，构建出性能强大的专用分类器。