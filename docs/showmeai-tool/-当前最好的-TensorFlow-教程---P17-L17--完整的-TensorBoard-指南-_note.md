# TensorFlow 教程 P17：📊 完整的 TensorBoard 指南

在本节课中，我们将学习如何使用 TensorBoard 这一强大的可视化工具，来更轻松地理解模型训练过程中的错误、监控性能并优化模型。TensorBoard 能帮助我们可视化训练指标、模型结构、数据分布等，是深度学习开发中不可或缺的助手。

我们将从最基础的准确度和损失图开始，逐步学习如何可视化图像、创建混淆矩阵、查看模型计算图、分析参数分布、进行超参数搜索以及使用投影器理解数据表示。最后，我们还会简要介绍 TensorFlow Profiler 来分析训练性能。

---

## 📈 1. 可视化训练指标（损失与准确度）

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_1.png)

上一节我们介绍了本课程的目标，本节中我们来看看如何可视化模型训练过程中最基础的指标：损失和准确度。这是监控模型是否在学习以及学习效果如何的最直接方式。

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_2.png)

使用 Keras 的回调（Callback）功能是记录这些指标最简单的方法。以下代码展示了如何设置 TensorBoard 回调并在训练时自动记录数据：

```python
import tensorflow as tf

# ... 数据加载和预处理代码 ...

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_4.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_6.png)

model = tf.keras.Sequential([...]) # 定义你的模型

model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

# 创建 TensorBoard 回调
log_dir = "tb_callback_dir"
tensorboard_callback = tf.keras.callbacks.TensorBoard(log_dir=log_dir,
                                                       histogram_freq=1)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_8.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_10.png)

# 开始训练，传入回调
model.fit(ds_train,
          epochs=5,
          validation_data=ds_test,
          callbacks=[tensorboard_callback],
          verbose=2)
```

训练完成后，在命令行使用 `tensorboard --logdir tb_callback_dir` 启动 TensorBoard，然后在浏览器中打开 `localhost:6006`，即可在 `SCALARS` 标签页下看到训练和验证的损失、准确度曲线图。

---

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_12.png)

## 🖼️ 2. 可视化图像数据

在模型训练中，尤其是在应用了数据增强（如随机裁剪、颜色变换）后，可视化输入图像可以确保数据预处理操作符合预期。

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_14.png)

以下是创建图像网格并写入 TensorBoard 的步骤：

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_16.png)

1.  定义一个函数将 Matplotlib 图形转换为 TensorFlow 图像张量。
2.  定义一个函数创建包含多张图像及其标签的网格图。
3.  在训练循环中，定期调用这些函数并将图像写入 TensorBoard。

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_18.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_19.png)

核心函数 `plot_to_image` 和 `image_grid` 的代码如下（部分源自官方教程）：

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_21.png)

```python
import matplotlib.pyplot as plt
import numpy as np
import tensorflow as tf
from io import BytesIO

def plot_to_image(figure):
    """将 Matplotlib 图形转换为 PNG 张量。"""
    buf = BytesIO()
    plt.savefig(buf, format='png')
    plt.close(figure)
    buf.seek(0)
    image = tf.image.decode_png(buf.getvalue(), channels=4)
    image = tf.expand_dims(image, 0)
    return image

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_23.png)

def image_grid(data, labels, class_names):
    """创建带标签的图像网格。"""
    data = np.array(data)
    assert len(data.shape) == 4 # (batch, H, W, C)
    num_images = data.shape[0]

    size = int(np.ceil(np.sqrt(num_images)))
    fig = plt.figure(figsize=(size, size))
    for i in range(num_images):
        plt.subplot(size, size, i+1)
        plt.xticks([])
        plt.yticks([])
        plt.grid(False)
        # 如果是灰度图
        if data.shape[-1] == 1:
            plt.imshow(data[i], cmap=plt.cm.binary)
        else:
            plt.imshow(data[i])
        plt.xlabel(class_names[labels[i]])
    return fig

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_25.png)

# 在训练循环中调用
with train_writer.as_default():
    figure = image_grid(images, labels, class_names)
    tf.summary.image("Training data", plot_to_image(figure), step=epoch)
```

在 TensorBoard 的 `IMAGES` 标签页中，你可以滑动查看不同训练步骤（step）下的输入图像。

---

## 🧮 3. 创建混淆矩阵

混淆矩阵能清晰展示模型在各个类别上的分类性能，特别是哪些类别容易被混淆。

我们的策略是在每个批次（batch）后更新一个累积的混淆矩阵，并在每个训练周期（epoch）结束时将其归一化并可视化。

以下是实现步骤：

1.  使用 `sklearn.metrics.confusion_matrix` 计算每个批次的混淆矩阵。
2.  累积所有批次的矩阵。
3.  定义一个函数绘制美观的、带概率值的混淆矩阵。
4.  将绘制好的矩阵图像写入 TensorBoard。

核心函数 `plot_confusion_matrix` 代码如下：

```python
from sklearn.metrics import confusion_matrix
import numpy as np

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_27.png)

def plot_confusion_matrix(cm, class_names):
    """绘制混淆矩阵。"""
    size = len(class_names)
    fig = plt.figure(figsize=(size, size))
    plt.imshow(cm, interpolation='nearest', cmap=plt.cm.Blues)
    plt.title("Confusion Matrix")
    plt.colorbar()
    tick_marks = np.arange(size)
    plt.xticks(tick_marks, class_names, rotation=45)
    plt.yticks(tick_marks, class_names)

    # 归一化并显示概率文本
    cm_normalized = cm.astype('float') / cm.sum(axis=1)[:, np.newaxis]
    cm_normalized = np.round(cm_normalized, 2)
    threshold = cm_normalized.max() / 2.
    for i in range(size):
        for j in range(size):
            color = "white" if cm_normalized[i, j] > threshold else "black"
            plt.text(j, i, cm_normalized[i, j],
                     horizontalalignment="center",
                     color=color)
    plt.tight_layout()
    plt.ylabel('Predicted label')
    plt.xlabel('True label')
    return fig

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_29.png)

# 在训练循环中累积和绘制
cm = np.zeros((len(class_names), len(class_names)))
for batch, (x_batch, y_batch) in enumerate(ds_train):
    # ... 模型预测 ...
    y_pred = model(x_batch)
    y_pred_labels = tf.argmax(y_pred, axis=1)
    cm += confusion_matrix(y_batch, y_pred_labels, labels=range(len(class_names)))

with train_writer.as_default():
    figure = plot_confusion_matrix(cm/(batch+1), class_names) # 归一化
    tf.summary.image("Confusion Matrix", plot_to_image(figure), step=epoch)
```

在 TensorBoard 的 `IMAGES` 标签页中，你可以看到每个 epoch 结束后更新的混淆矩阵。

---

## 🕸️ 4. 可视化模型计算图

对于复杂的模型，可视化其计算图有助于理解数据流和层间连接，对于调试非常有用。

TensorBoard 可以自动追踪并可视化用 `@tf.function` 装饰的函数或 Keras 模型的计算图。

以下是记录一个简单函数计算图的示例：

```python
import tensorflow as tf

# 创建一个文件写入器用于记录图
log_dir = "logs/graph"
writer = tf.summary.create_file_writer(log_dir)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_31.png)

# 定义一个函数并用 @tf.function 装饰以生成静态图
@tf.function
def my_function(x, y):
    return tf.nn.relu(tf.matmul(x, y))

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_33.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_34.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_36.png)

# 生成一些随机数据作为输入
x = tf.random.uniform((3, 3))
y = tf.random.uniform((3, 3))

# 开启追踪并运行函数
tf.summary.trace_on(graph=True, profiler=True)
z = my_function(x, y)
with writer.as_default():
    tf.summary.trace_export(name="function_trace",
                            step=0,
                            profiler_outdir=log_dir)
```

在 TensorBoard 的 `GRAPHS` 标签页中，你可以看到 `my_function` 内部 `matmul` 和 `relu` 操作的详细计算图。对于 Keras 模型，直接调用 `model(x)` 并进行类似追踪即可。

---

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_39.png)

## 📊 5. 可视化参数与梯度分布

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_41.png)

在 `DISTRIBUTIONS` 和 `HISTOGRAMS` 标签页，TensorBoard 可以展示模型权重、偏置、激活值以及梯度在整个训练过程中的分布变化。这对于诊断梯度消失/爆炸、权重初始化问题至关重要。

使用 Keras 回调时，设置 `histogram_freq=1` 即可在每个 epoch 记录这些分布。在自定义训练循环中，你需要手动记录：

```python
with train_writer.as_default():
    for layer in model.layers:
        for weight in layer.weights:
            tf.summary.histogram(f"{layer.name}/{weight.name}", weight, step=epoch)
    # 记录梯度 (需要先获取梯度)
    # for grad, var in zip(gradients, model.trainable_variables):
    #     tf.summary.histogram(f"{var.name}/gradient", grad, step=epoch)
```

观察这些分布图可以帮助你判断模型是否在健康地学习。

---

## 🔍 6. 使用 HParams 进行超参数搜索

TensorBoard 的 HParams 插件可以系统地记录和比较不同超参数组合下的模型性能。

以下是使用 HParams API 的基本流程：

1.  定义你要调优的超参数（如学习率、 dropout 率、网络单元数）。
2.  修改训练代码，使其能接收一个超参数字典。
3.  在每次训练运行（run）开始时，用 `hp.hparams()` 记录该次运行使用的超参数。
4.  在训练结束时，记录评估指标（如准确度）。

示例代码结构如下：

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_43.png)

```python
from tensorboard.plugins.hparams import api as hp

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_45.png)

# 1. 定义超参数
HP_NUM_UNITS = hp.HParam('num_units', hp.Discrete([32, 64, 128]))
HP_DROPOUT = hp.HParam('dropout', hp.Discrete([0.1, 0.2, 0.3]))
HP_LEARNING_RATE = hp.HParam('learning_rate', hp.Discrete([1e-3, 1e-4]))

# 2. 训练函数，接收超参数字典
def train_one_epoch(hparams):
    # 根据 hparams 构建模型和优化器
    model = create_model(units=hparams[HP_NUM_UNITS], dropout=hparams[HP_DROPOUT])
    optimizer = tf.keras.optimizers.Adam(learning_rate=hparams[HP_LEARNING_RATE])
    # ... 训练循环 ...
    accuracy = ... # 计算准确度
    return accuracy

# 3. 遍历超参数组合并运行
for num_units in HP_NUM_UNITS.domain.values:
    for dropout_rate in HP_DROPOUT.domain.values:
        for lr in HP_LEARNING_RATE.domain.values:
            hparams = {
                HP_NUM_UNITS: num_units,
                HP_DROPOUT: dropout_rate,
                HP_LEARNING_RATE: lr,
            }
            # 为每次运行创建独立的日志目录
            run_dir = f"logs/hparam_tuning/units_{num_units}_dropout_{dropout_rate}_lr_{lr}"
            with tf.summary.create_file_writer(run_dir).as_default():
                # 记录超参数
                hp.hparams(hparams)
                # 运行训练并记录指标
                accuracy = train_one_epoch(hparams)
                tf.summary.scalar('accuracy', accuracy, step=1)
```

在 TensorBoard 的 `HPARAMS` 标签页，你可以并排比较所有实验，通过平行坐标图、散点图矩阵等工具，直观地找出最佳超参数组合。

---

## 🌌 7. 使用投影器理解数据表示

投影器（Projector）是 TensorBoard 中一个非常酷的功能，它可以将高维特征（如图像经过模型提取的特征向量）降维到 2D 或 3D 空间，帮助我们理解模型是如何“看待”和区分不同数据的。

使用投影器需要准备三样东西：
1.  **特征向量**：每个数据点的高维表示。
2.  **元数据文件**：包含每个数据点标签的 `.tsv` 文件。
3.  **精灵图像**（可选）：将所有数据点图片拼接成一张大图，用于在投影空间中显示。

以下是关键步骤代码：

```python
import os
import tensorflow as tf
from tensorboard.plugins import projector

def plot_to_projector(feature_vectors, labels, images, class_names, log_dir):
    """
    将特征向量、标签和图像设置到投影器中。
    feature_vectors: 形状为 [num_samples, feature_dim] 的张量。
    images: 形状为 [num_samples, H, W, C] 的原始图像，用于生成精灵图。
    """
    # 清理旧日志目录
    if tf.io.gfile.exists(log_dir):
        tf.io.gfile.rmtree(log_dir)
    tf.io.gfile.makedirs(log_dir)

    # 1. 保存特征向量
    feature_vector_var = tf.Variable(feature_vectors, name='embeddings')
    checkpoint = tf.train.Checkpoint(embedding=feature_vector_var)
    checkpoint.save(os.path.join(log_dir, "embeddings.ckpt"))

    # 2. 保存元数据（标签）
    metadata_path = os.path.join(log_dir, 'metadata.tsv')
    with open(metadata_path, 'w') as f:
        for label in labels:
            f.write(f'{class_names[label]}\n')

    # 3. 创建并保存精灵图像
    sprite_path = os.path.join(log_dir, 'sprite.png')
    sprite_image = create_sprite_image(images) # 需要实现此函数
    tf.keras.utils.save_img(sprite_path, sprite_image)

    # 4. 创建投影器配置
    config = projector.ProjectorConfig()
    embedding = config.embeddings.add()
    embedding.tensor_name = 'embedding/.ATTRIBUTES/VARIABLE_VALUE'
    embedding.metadata_path = 'metadata.tsv'
    embedding.sprite.image_path = 'sprite.png'
    embedding.sprite.single_image_dim.extend([images.shape[1], images.shape[2]]) # 图像高和宽

    # 5. 写入配置
    projector.visualize_embeddings(log_dir, config)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_47.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_49.png)

# 假设我们有一个批次的数据
# 获取特征向量（例如，通过模型的某一层）
# features = model.predict(images)
# 或者，为了简单，直接将图像展平作为特征（仅用于演示）
features = tf.reshape(images, [images.shape[0], -1])
plot_to_projector(features, labels, images, class_names, "logs/projector")
```

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_51.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_52.png)

在 TensorBoard 的 `PROJECTOR` 标签页，你可以选择 PCA、t-SNE 等降维算法，并看到数据点在三维空间中的聚类情况。点击点可以查看对应的原始图片，直观验证模型学习到的表示是否合理。

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_54.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_55.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_57.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_59.png)

---

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_61.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_63.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_65.png)

## ⚡ 8. 使用 TensorFlow Profiler 分析性能

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_67.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_68.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_70.png)

TensorFlow Profiler 可以帮助你定位训练过程中的性能瓶颈，例如是数据读取（I/O）慢还是计算慢。

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_72.png)

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_74.png)

使用 Profiler 最简单的方法是通过 `tf.keras.callbacks.TensorBoard` 回调的 `profile_batch` 参数。

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_76.png)

```python
# 创建一个回调，分析第 500 到 520 个批次
log_dir = "logs/profile_" + datetime.now().strftime("%Y%m%d-%H%M%S")
tensorboard_callback = tf.keras.callbacks.TensorBoard(log_dir=log_dir,
                                                       profile_batch='500,520')

model.fit(ds_train,
          epochs=10,
          callbacks=[tensorboard_callback])
```

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_78.png)

在 TensorBoard 的 `PROFILE` 标签页中，你可以看到：
*   **概览页面**：显示训练步骤时间的分解，指出程序是“输入受限”还是“计算受限”。
*   **输入管道分析器**：如果输入是瓶颈，它会给出优化建议（如启用 `cache`、`prefetch`、`interleave` 等）。
*   **TensorFlow 统计信息**：显示各类操作消耗的时间。
*   **追踪查看器**：提供 GPU 和 CPU 上操作的毫秒级时间线，用于深入分析。

通过分析这些数据，你可以有针对性地优化数据管道或模型计算，提升训练效率。

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_80.png)

---

## 📝 总结

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_82.png)

本节课中我们一起学习了 TensorBoard 的核心功能。我们从可视化基础的损失和准确度曲线开始，逐步掌握了如何查看输入图像、分析混淆矩阵、审视模型计算图、监控参数分布。我们还利用 HParams 插件进行了超参数搜索，使用投影器深入理解了模型的数据表示，最后借助 Profiler 对训练性能进行了分析。

![](img/41a5fdcb23b17bc7ced9ef9ac3dfb245_84.png)

TensorBoard 的这些工具共同构成了一个强大的模型调试、监控和解释套件。熟练使用它们，能让你更高效地开发、优化和理解你的深度学习模型。