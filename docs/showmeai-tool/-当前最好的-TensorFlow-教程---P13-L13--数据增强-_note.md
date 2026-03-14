# TensorFlow 教程 P13：L13 - 数据增强 🚀

![](img/007b8363c10bed4060cf86172290b0d8_1.png)

![](img/007b8363c10bed4060cf86172290b0d8_2.png)

![](img/007b8363c10bed4060cf86172290b0d8_3.png)

![](img/007b8363c10bed4060cf86172290b0d8_5.png)

在本节课中，我们将学习数据增强技术。数据增强是一种通过对训练图像应用随机变换（如翻转、旋转、调整亮度等）来人工扩展数据集的方法。这有助于模型学习更鲁棒的特征，并有效防止过拟合。我们将介绍两种在TensorFlow中实现数据增强的方法。

![](img/007b8363c10bed4060cf86172290b0d8_7.png)

---

![](img/007b8363c10bed4060cf86172290b0d8_8.png)

![](img/007b8363c10bed4060cf86172290b0d8_9.png)

![](img/007b8363c10bed4060cf86172290b0d8_11.png)

## 概述与准备工作

![](img/007b8363c10bed4060cf86172290b0d8_13.png)

![](img/007b8363c10bed4060cf86172290b0d8_15.png)

首先，我们导入必要的库并加载CIFAR-10数据集。这部分代码与之前的课程类似，包括数据归一化、缓存、洗牌、批处理和预取等步骤。

![](img/007b8363c10bed4060cf86172290b0d8_17.png)

```python
import tensorflow as tf
import tensorflow_datasets as tfds

![](img/007b8363c10bed4060cf86172290b0d8_19.png)

![](img/007b8363c10bed4060cf86172290b0d8_20.png)

![](img/007b8363c10bed4060cf86172290b0d8_21.png)

# 加载CIFAR-10数据集
(train_ds, test_ds), ds_info = tfds.load('cifar10',
                                          split=['train', 'test'],
                                          as_supervised=True,
                                          with_info=True)

![](img/007b8363c10bed4060cf86172290b0d8_23.png)

# 归一化函数
def normalize_img(image, label):
    return tf.cast(image, tf.float32) / 255., label

# 准备训练集
train_ds = train_ds.map(normalize_img, num_parallel_calls=tf.data.AUTOTUNE)
train_ds = train_ds.cache()
train_ds = train_ds.shuffle(ds_info.splits['train'].num_examples)
train_ds = train_ds.batch(128)
train_ds = train_ds.prefetch(tf.data.AUTOTUNE)

![](img/007b8363c10bed4060cf86172290b0d8_25.png)

![](img/007b8363c10bed4060cf86172290b0d8_27.png)

# 准备测试集
test_ds = test_ds.map(normalize_img, num_parallel_calls=tf.data.AUTOTUNE)
test_ds = test_ds.batch(128)
test_ds = test_ds.prefetch(tf.data.AUTOTUNE)
```

接下来，我们定义一个简单的卷积神经网络模型作为本课的基础模型。

![](img/007b8363c10bed4060cf86172290b0d8_29.png)

```python
model = tf.keras.Sequential([
    tf.keras.layers.Conv2D(32, 3, activation='relu', input_shape=(32, 32, 3)),
    tf.keras.layers.MaxPooling2D(),
    tf.keras.layers.Conv2D(64, 3, activation='relu'),
    tf.keras.layers.MaxPooling2D(),
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dense(10)
])

![](img/007b8363c10bed4060cf86172290b0d8_31.png)

![](img/007b8363c10bed4060cf86172290b0d8_32.png)

model.compile(optimizer='adam',
              loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
              metrics=['accuracy'])
```

![](img/007b8363c10bed4060cf86172290b0d8_34.png)

---

## 方法一：使用 `tf.data` 管道进行数据增强

上一节我们介绍了基础的数据处理流程。本节中，我们来看看第一种数据增强方法：在 `tf.data` 管道中定义增强函数并映射到数据集。

![](img/007b8363c10bed4060cf86172290b0d8_36.png)

这种方法的核心是定义一个增强函数，其中包含一系列随机图像变换操作。以下是实现步骤：

首先，我们定义一个名为 `augment` 的函数。

```python
def augment(image, label):
    # 1. 调整图像大小（示例，CIFAR-10已为32x32，此处仅为展示）
    new_height = new_width = 32
    image = tf.image.resize(image, [new_height, new_width])

    # 2. 以10%的概率将图像转换为灰度，并复制通道以匹配模型输入
    if tf.random.uniform(()) < 0.1:
        image = tf.image.rgb_to_grayscale(image)
        # 将单通道复制为三通道
        image = tf.tile(image, [1, 1, 3])

    # 3. 随机调整亮度
    image = tf.image.random_brightness(image, max_delta=0.1)

    # 4. 随机调整对比度
    image = tf.image.random_contrast(image, lower=0.8, upper=1.2)

    # 5. 随机水平翻转（50%概率）
    image = tf.image.random_flip_left_right(image)

    # 注意：根据任务谨慎选择增强方式。例如，对于数字识别（MNIST），水平翻转可能改变标签含义。
    return image, label
```

![](img/007b8363c10bed4060cf86172290b0d8_38.png)

![](img/007b8363c10bed4060cf86172290b0d8_39.png)

**重要提示**：数据增强通常在训练时**实时**进行，CPU在准备下一个批次时并行应用这些随机变换，而GPU正在训练当前批次。这有效地增加了模型看到的数据多样性，而不是物理上扩大数据集文件。

定义好函数后，我们将其映射到训练数据集上。

```python
# 在 shuffle 之后、batch 之前应用增强
train_ds = train_ds.shuffle(10000).map(augment, num_parallel_calls=tf.data.AUTOTUNE).batch(128).prefetch(tf.data.AUTOTUNE)
```

![](img/007b8363c10bed4060cf86172290b0d8_41.png)

![](img/007b8363c10bed4060cf86172290b0d8_42.png)

---

![](img/007b8363c10bed4060cf86172290b0d8_44.png)

## 方法二：使用 Keras 预处理层进行数据增强

![](img/007b8363c10bed4060cf86172290b0d8_46.png)

除了在数据管道中进行增强，TensorFlow 2.3.0及以上版本还提供了另一种更集成化的方式：将数据增强作为模型的一部分，即使用Keras预处理层。

![](img/007b8363c10bed4060cf86172290b0d8_48.png)

![](img/007b8363c10bed4060cf86172290b0d8_49.png)

以下是使用 `tf.keras.layers.experimental.preprocessing` 模块构建增强层的方法：

![](img/007b8363c10bed4060cf86172290b0d8_51.png)

![](img/007b8363c10bed4060cf86172290b0d8_52.png)

```python
data_augmentation = tf.keras.Sequential([
    # 调整大小层
    tf.keras.layers.experimental.preprocessing.Resizing(height=32, width=32),
    # 随机水平翻转层
    tf.keras.layers.experimental.preprocessing.RandomFlip(mode='horizontal'),
    # 随机对比度层
    tf.keras.layers.experimental.preprocessing.RandomContrast(factor=0.1),
    # 可以在此添加更多预处理层，如 RandomRotation, RandomZoom 等
])
```

![](img/007b8363c10bed4060cf86172290b0d8_53.png)

![](img/007b8363c10bed4060cf86172290b0d8_55.png)

然后，我们将这个增强序列作为第一层添加到模型中。

![](img/007b8363c10bed4060cf86172290b0d8_57.png)

```python
model = tf.keras.Sequential([
    data_augmentation,  # 数据增强层
    tf.keras.layers.Conv2D(32, 3, activation='relu', input_shape=(32, 32, 3)),
    tf.keras.layers.MaxPooling2D(),
    tf.keras.layers.Conv2D(64, 3, activation='relu'),
    tf.keras.layers.MaxPooling2D(),
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dense(10)
])
```

![](img/007b8363c10bed4060cf86172290b0d8_58.png)

![](img/007b8363c10bed4060cf86172290b0d8_59.png)

![](img/007b8363c10bed4060cf86172290b0d8_60.png)

这种方法将增强过程封装在模型内部，代码更简洁。需要注意的是，这种方式可能不如 `tf.data` 管道并行化效率高，但对于许多应用来说已经足够且非常方便。

---

![](img/007b8363c10bed4060cf86172290b0d8_62.png)

![](img/007b8363c10bed4060cf86172290b0d8_63.png)

## 数据增强的效果与总结

在训练中引入数据增强后，我们观察到模型在第一个训练周期（epoch）后的准确率有所提升（例如从~26%提升至~41%）。这是因为数据增强通过引入多样性，使模型更难简单地“记忆”训练样本，从而作为一种有效的正则化手段，减轻了过拟合。

![](img/007b8363c10bed4060cf86172290b0d8_65.png)

**核心思想**：
*   **公式化理解**：数据增强通过对原始训练样本 `x` 应用一系列随机变换 `T`（如旋转、裁剪、颜色抖动），生成新的训练样本 `x' = T(x)`，而标签 `y` 保持不变（前提是变换不改变语义）。这扩大了训练分布，提高了模型的泛化能力。
*   **与正则化的关系**：类似于Dropout或L2正则化，数据增强通过增加训练数据的“噪声”和多样性，迫使模型学习更本质、更鲁棒的特征，而不是依赖训练集中的偶然性模式。

![](img/007b8363c10bed4060cf86172290b0d8_66.png)

![](img/007b8363c10bed4060cf86172290b0d8_67.png)

![](img/007b8363c10bed4060cf86172290b0d8_68.png)

**选择增强策略的注意事项**：
1.  增强操作不应改变图像的语义标签（例如，水平翻转数字“6”可能会变成“9”）。
2.  并非越多越好，需根据具体任务选择合理的增强组合。
3.  建议查阅官方文档，探索更多可用的增强函数。

![](img/007b8363c10bed4060cf86172290b0d8_70.png)

**实践建议**：
尝试在CIFAR-10数据集上结合本课的数据增强方法以及之前课程学到的L2正则化、Dropout等技术，挑战将测试准确率提升到90%以上。

![](img/007b8363c10bed4060cf86172290b0d8_72.png)

---

![](img/007b8363c10bed4060cf86172290b0d8_74.png)

本节课中我们一起学习了数据增强的两种主要实现方法及其原理。数据增强是提升模型泛化能力、防止过拟合的强大工具，请务必在你的计算机视觉项目中实践应用。