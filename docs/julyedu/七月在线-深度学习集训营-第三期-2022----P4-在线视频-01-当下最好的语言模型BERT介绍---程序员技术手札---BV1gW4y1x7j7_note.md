# 🧠 七月在线-深度学习集训营 第三期[2022] - P4：当下最好的语言模型BERT介绍

![](img/e38887d0cfad027411d3837bd0fd0102_0.png)

![](img/e38887d0cfad027411d3837bd0fd0102_2.png)

在本节课中，我们将要学习当下效果卓越的语言模型BERT。课程将从Attention机制开始，逐步深入到Transformer的核心结构，并最终详细解析BERT模型的原理、预训练任务以及如何在实际任务中进行微调。我们将确保内容简单直白，让初学者能够跟上。

---

## 🔍 第一部分：Attention机制回顾

上一节我们介绍了课程的整体安排，本节中我们来看看Attention机制。对于序列生成类任务，如机器翻译、文本摘要，通常采用Sequence-to-Sequence模型。

Sequence-to-Sequence模型包含编码器（Encoder）和解码器（Decoder）两部分。编码器将输入序列编码为一个上下文向量（Context），解码器则根据该向量生成输出序列。然而，该模型有一个致命缺点：当输入序列过长时，靠前的信息容易丢失。

为了解决长序列信息丢失的问题，Attention机制应运而生。Attention不是模型，而是一种机制。它模仿人类阅读时的行为：在回答问题时，只关注文章中相关的段落，而非通读全文。

在引入Attention的Sequence-to-Sequence模型中，编码器会输出所有时间步的隐藏状态，而不仅仅是最末的上下文向量。解码器的每个时间步会计算一个权重分布，对编码器的所有隐藏状态进行加权求和，从而动态地聚焦于输入序列的不同部分。

以下是Attention机制的核心思想，可以概括为一个加权求和的过程：

**公式：** `Attention(Q, K, V) = softmax(QK^T / sqrt(d_k)) V`

其中，`Q`（Query）代表解码器的当前状态，`K`（Key）和`V`（Value）通常代表编码器的所有隐藏状态。通过计算`Q`和`K`的相似度得到权重，再对`V`进行加权求和。

![](img/e38887d0cfad027411d3837bd0fd0102_4.png)

![](img/e38887d0cfad027411d3837bd0fd0102_5.png)

![](img/e38887d0cfad027411d3837bd0fd0102_7.png)

![](img/e38887d0cfad027411d3837bd0fd0102_9.png)

---

## 🏗️ 第二部分：Transformer核心结构

理解了Attention机制后，本节我们来看看Transformer模型。Transformer完全基于Attention机制构建，是BERT模型的核心。

![](img/e38887d0cfad027411d3837bd0fd0102_11.png)

![](img/e38887d0cfad027411d3837bd0fd0102_13.png)

Transformer同样采用编码器-解码器架构。其编码器由N个相同的层堆叠而成，每层包含两个子层：
1.  **多头自注意力机制（Multi-Head Self-Attention）**
2.  **前馈神经网络（Feed-Forward Network）**

![](img/e38887d0cfad027411d3837bd0fd0102_15.png)

![](img/e38887d0cfad027411d3837bd0fd0102_17.png)

每个子层后都接有残差连接（Residual Connection）和层归一化（Layer Normalization），这有助于缓解梯度消失和过拟合。

解码器结构类似，但在其第一个多头注意力子层中加入了掩码（Mask），以防止在训练时“看到”未来的信息，确保自回归生成的正确性。

![](img/e38887d0cfad027411d3837bd0fd0102_19.png)

![](img/e38887d0cfad027411d3837bd0fd0102_21.png)

---

### 2.1 Scaled Dot-Product Attention

Transformer中使用的Attention变体是Scaled Dot-Product Attention。其计算公式如下：

**公式：** `Attention(Q, K, V) = softmax(QK^T / sqrt(d_k)) V`

与简单的点积注意力（Dot-Product Attention）相比，它多了一个缩放因子 `1 / sqrt(d_k)`。`d_k` 是Key的维度。加入缩放因子是为了防止点积结果方差过大，导致经过Softmax函数后梯度太小，影响模型训练。

---

### 2.2 多头注意力机制（Multi-Head Attention）

![](img/e38887d0cfad027411d3837bd0fd0102_23.png)

![](img/e38887d0cfad027411d3837bd0fd0102_25.png)

单一的Attention机制可能只关注到一种模式的信息。为了让模型能够同时关注来自不同表示子空间的信息，Transformer引入了多头注意力机制。

其做法是：
1.  将`Q`, `K`, `V`通过不同的线性变换矩阵投影多次，得到多组`Q`, `K`, `V`。
2.  对每一组分别进行Scaled Dot-Product Attention计算，得到多个输出。
3.  将多个输出拼接起来，再通过一个线性变换层进行整合和降维。

![](img/e38887d0cfad027411d3837bd0fd0102_27.png)

![](img/e38887d0cfad027411d3837bd0fd0102_29.png)

![](img/e38887d0cfad027411d3837bd0fd0102_31.png)

![](img/e38887d0cfad027411d3837bd0fd0102_33.png)

**代码示意：**
```python
# 伪代码示意多头注意力
def multi_head_attention(Q, K, V, num_heads):
    # 1. 线性投影，分割成多头
    Q_proj = split_heads(linear(Q), num_heads)
    K_proj = split_heads(linear(K), num_heads)
    V_proj = split_heads(linear(V), num_heads)
    
    # 2. 对每个头计算注意力
    head_outputs = []
    for i in range(num_heads):
        attn_output = scaled_dot_product_attention(Q_proj[i], K_proj[i], V_proj[i])
        head_outputs.append(attn_output)
    
    # 3. 合并多头输出并线性变换
    concat_output = concatenate(head_outputs)
    output = linear(concat_output)
    return output
```

在Transformer编码器中，使用的是**自注意力（Self-Attention）**，即`Q`, `K`, `V`均来自同一个输入序列，用于捕捉序列内部的依赖关系。

---

## 🤖 第三部分：BERT模型详解

掌握了Transformer的编码器后，本节我们来学习BERT模型。BERT的全称是Bidirectional Encoder Representations from Transformers，即**双向Transformer编码器表示**。

BERT的本质是一个多层Transformer编码器的堆叠。它有两种规格：
*   **BERT-base**: 12层Transformer，隐藏层维度768，12个注意力头。
*   **BERT-large**: 24层Transformer，隐藏层维度1024，16个注意力头。

BERT是一个预训练模型，其应用分为两个阶段：**预训练（Pre-training）** 和 **微调（Fine-tuning）**。

---

![](img/e38887d0cfad027411d3837bd0fd0102_35.png)

### 3.1 BERT的预训练任务

![](img/e38887d0cfad027411d3837bd0fd0102_37.png)

![](img/e38887d0cfad027411d3837bd0fd0102_39.png)

BERT通过两个无监督任务在海量文本上进行预训练，从而学习通用的语言表示。

![](img/e38887d0cfad027411d3837bd0fd0102_41.png)

**任务一：掩码语言模型（Masked Language Model, MLM）**
类似于完形填空。随机掩盖输入序列中15%的Token，然后让模型预测被掩盖的Token。为了防止[MASK]标记在微调阶段从未出现，BERT采用以下策略：
*   80%的概率替换为`[MASK]`
*   10%的概率替换为随机Token
*   10%的概率保持不变

![](img/e38887d0cfad027411d3837bd0fd0102_43.png)

![](img/e38887d0cfad027411d3837bd0fd0102_45.png)

**任务二：下一句预测（Next Sentence Prediction, NSP）**
判断句子B是否是句子A的下一句。训练数据中，50%的句子B是A的实际下一句（正例），50%是随机选取的（负例）。这个任务帮助模型理解句子间关系。

BERT的输入由三部分嵌入相加而成：
1.  **Token Embedding**: 词嵌入。
2.  **Segment Embedding**: 段落嵌入，用于区分两个句子（如句子A全为0，句子B全为1）。
3.  **Position Embedding**: 位置嵌入，为序列提供位置信息。

输入序列以`[CLS]`标记开头，句子间用`[SEP]`标记分隔。`[CLS]`标记的最终输出常被用作整个序列的聚合表示，用于分类任务。

![](img/e38887d0cfad027411d3837bd0fd0102_47.png)

![](img/e38887d0cfad027411d3837bd0fd0102_49.png)

![](img/e38887d0cfad027411d3837bd0fd0102_51.png)

![](img/e38887d0cfad027411d3837bd0fd0102_53.png)

---

![](img/e38887d0cfad027411d3837bd0fd0102_55.png)

![](img/e38887d0cfad027411d3837bd0fd0102_57.png)

![](img/e38887d0cfad027411d3837bd0fd0102_59.png)

## 🛠️ 第四部分：BERT实战——文本相似度微调

理论部分我们已经介绍了BERT的原理，本节中我们来看看如何在实际任务中微调BERT。我们将以“文本相似度判断”任务为例。

![](img/e38887d0cfad027411d3837bd0fd0102_61.png)

![](img/e38887d0cfad027411d3837bd0fd0102_63.png)

微调的本质是在预训练好的BERT模型基础上，针对特定任务和数据集，对模型参数进行小幅调整。

![](img/e38887d0cfad027411d3837bd0fd0102_65.png)

![](img/e38887d0cfad027411d3837bd0fd0102_67.png)

![](img/e38887d0cfad027411d3837bd0fd0102_69.png)

![](img/e38887d0cfad027411d3837bd0fd0102_71.png)

以下是微调BERT的主要步骤：

![](img/e38887d0cfad027411d3837bd0fd0102_73.png)

![](img/e38887d0cfad027411d3837bd0fd0102_75.png)

1.  **准备数据**：准备训练集、验证集和测试集，格式通常为`(句子A，句子B，标签)`。
2.  **定义数据处理器**：需要继承`DataProcessor`类，并实现四个方法：获取训练集、验证集、测试集和标签列表。核心是将数据转换为`InputExample`对象。
3.  **修改运行脚本**：BERT官方提供了`run_classifier.py`等脚本。我们需要在其中注册自定义的数据处理器，并通过命令行参数指定任务名称。
4.  **执行训练**：通过命令行运行脚本，指定必要的参数，如预训练模型路径、数据目录、输出目录等。

**关键代码片段示例（自定义DataProcessor）：**
```python
class MyProcessor(DataProcessor):
    def get_train_examples(self, data_dir):
        # 读取你的训练数据文件，例如TSV格式
        df = pd.read_csv(os.path.join(data_dir, "train.tsv"), sep='\t')
        examples = []
        for (i, row) in df.iterrows():
            guid = f"train-{i}"
            text_a = row['sentence1']
            text_b = row['sentence2']
            # 注意：标签需要转换为字符串类型
            label = str(row['label'])
            examples.append(InputExample(guid=guid, text_a=text_a, text_b=text_b, label=label))
        return examples
    # 类似地实现 get_dev_examples, get_test_examples, get_labels 方法
```

在运行微调后，模型会在验证集上评估性能（如准确率），并将微调后的模型保存到指定目录，供后续预测使用。

![](img/e38887d0cfad027411d3837bd0fd0102_77.png)

![](img/e38887d0cfad027411d3837bd0fd0102_79.png)

![](img/e38887d0cfad027411d3837bd0fd0102_81.png)

![](img/e38887d0cfad027411d3837bd0fd0102_83.png)

---

![](img/e38887d0cfad027411d3837bd0fd0102_85.png)

![](img/e38887d0cfad027411d3837bd0fd0102_87.png)

![](img/e38887d0cfad027411d3837bd0fd0102_89.png)

![](img/e38887d0cfad027411d3837bd0fd0102_91.png)

## 🚀 第五部分：BERT的演进与总结

![](img/e38887d0cfad027411d3837bd0fd0102_93.png)

![](img/e38887d0cfad027411d3837bd0fd0102_95.png)

![](img/e38887d0cfad027411d3837bd0fd0102_97.png)

在BERT之后，研究者们提出了诸多改进模型，如：
*   **GPT系列**：使用Transformer解码器，采用单向语言模型进行预训练。
*   **ERNIE（百度）**：在MLM任务中掩盖整个实体或短语，而非单个字词，以增强语义理解。
*   **XLNet**：采用排列语言模型，克服了BERT中`[MASK]`标记在预训练与微调时不一致的问题，并引入了Transformer-XL以处理更长序列。
*   **RoBERTa**：主要改进了BERT的训练策略，如使用更大批次、更多数据、动态掩码等，去除了NSP任务。

![](img/e38887d0cfad027411d3837bd0fd0102_99.png)

![](img/e38887d0cfad027411d3837bd0fd0102_101.png)

![](img/e38887d0cfad027411d3837bd0fd0102_103.png)

尽管新模型层出不穷，但由于生态、中文支持及性价比等因素，BERT及其变体仍然是工业界最常用的预训练模型之一。

![](img/e38887d0cfad027411d3837bd0fd0102_105.png)

![](img/e38887d0cfad027411d3837bd0fd0102_107.png)

---

![](img/e38887d0cfad027411d3837bd0fd0102_109.png)

![](img/e38887d0cfad027411d3837bd0fd0102_111.png)

![](img/e38887d0cfad027411d3837bd0fd0102_113.png)

### 本节课总结

![](img/e38887d0cfad027411d3837bd0fd0102_115.png)

![](img/e38887d0cfad027411d3837bd0fd0102_117.png)

![](img/e38887d0cfad027411d3837bd0fd0102_119.png)

![](img/e38887d0cfad027411d3837bd0fd0102_121.png)

![](img/e38887d0cfad027411d3837bd0fd0102_122.png)

本节课我们一起学习了从Attention到Transformer，再到BERT的完整知识体系：
1.  **Attention机制**是一种加权求和机制，解决了长序列信息丢失问题。
2.  **Transformer**是基于Attention的编码器-解码器模型，其核心是多头自注意力机制。
3.  **BERT**是双向Transformer编码器的堆叠，通过MLM和NSP任务进行预训练，学习通用的语言表示。
4.  **微调（Fine-tuning）** 是将预训练BERT模型适配到特定下游任务的关键步骤，我们以文本相似度任务为例进行了实战讲解。

![](img/e38887d0cfad027411d3837bd0fd0102_124.png)

![](img/e38887d0cfad027411d3837bd0fd0102_126.png)

![](img/e38887d0cfad027411d3837bd0fd0102_128.png)

![](img/e38887d0cfad027411d3837bd0fd0102_130.png)

理解Transformer的结构是掌握BERT的基础，而熟练进行微调则是将BERT应用于实际项目的关键。建议大家课后多复习，并动手完成微调实践。