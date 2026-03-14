# 课程P83：YOLOv3模型预测与效果展示 🚀

![](img/076f1f1acc2a15b26fe091e37291845e_1.png)

![](img/076f1f1acc2a15b26fe091e37291845e_3.png)

在本节课中，我们将学习如何加载训练好的YOLOv3模型，并使用它对新的图像进行目标检测。我们将从指定测试图像开始，逐步完成模型加载、前向传播、非极大值抑制以及结果可视化的完整流程。

![](img/076f1f1acc2a15b26fe091e37291845e_5.png)

---

![](img/076f1f1acc2a15b26fe091e37291845e_7.png)

## 1. 准备工作与参数配置

![](img/076f1f1acc2a15b26fe091e37291845e_9.png)

上一节我们介绍了模型的训练过程，本节中我们来看看如何使用训练好的模型进行预测。首先，我们需要指定测试图像所在的文件夹路径。

![](img/076f1f1acc2a15b26fe091e37291845e_11.png)

![](img/076f1f1acc2a15b26fe091e37291845e_13.png)

在代码中，我们打开了一个名为 `detect.py` 的文件，这是我们的检测函数。第一步是配置参数，指定待检测图像的路径。

![](img/076f1f1acc2a15b26fe091e37291845e_15.png)

```python
# 示例：在配置文件中指定图像文件夹路径
image_folder = './data/simple/'
```

这里，`./data/simple/` 文件夹中包含了一系列待测试的图像，例如狗、自行车、卡车、鸟等不同大小和内容的图片。我们将把这些图像输入到网络中进行检测。

![](img/076f1f1acc2a15b26fe091e37291845e_17.png)

---

![](img/076f1f1acc2a15b26fe091e37291845e_19.png)

## 2. 模型加载与设置

接下来，我们需要构建并加载训练好的YOLOv3模型。预测时使用的网络结构与训练时完全相同。

![](img/076f1f1acc2a15b26fe091e37291845e_21.png)

以下是核心步骤：
1.  选择运行设备（CPU或GPU）。
2.  使用 `Darknet` 类构建模型。
3.  加载预训练的权重文件（例如 `darknet53.conv.74`）。
4.  将模型设置为评估模式（`eval()`），这意味着在预测过程中不会更新模型参数。

![](img/076f1f1acc2a15b26fe091e37291845e_23.png)

```python
# 构建模型并加载权重
model = Darknet(config_path).to(device)
model.load_state_dict(torch.load(weights_path))
model.eval()  # 设置为评估模式
```

设置评估模式后，模型只进行前向传播计算，不进行反向传播和参数更新。

![](img/076f1f1acc2a15b26fe091e37291845e_25.png)

![](img/076f1f1acc2a15b26fe091e37291845e_27.png)

---

## 3. 数据加载与预处理

我们需要创建一个数据加载器（DataLoader）来读取指定文件夹中的图像数据。

以下是数据加载的关键步骤：
*   根据配置的 `image_folder` 路径创建数据集。
*   设置批处理大小（`batch_size`）等参数。
*   同时，需要加载一个将类别ID映射为类别名称的文件（如 `coco.names`），以便将预测的数字结果转换为“狗”、“自行车”等可读标签。

![](img/076f1f1acc2a15b26fe091e37291845e_29.png)

```python
# 创建数据加载器
dataset = ImageFolder(image_folder, transform=preprocess_transform)
dataloader = DataLoader(dataset, batch_size=batch_size, shuffle=False)

![](img/076f1f1acc2a15b26fe091e37291845e_31.png)

# 加载类别名称映射
names = load_classes(names_path)  # 例如：{0: 'person', 1: 'bicycle', ...}
```

![](img/076f1f1acc2a15b26fe091e37291845e_33.png)

---

![](img/076f1f1acc2a15b26fe091e37291845e_35.png)

![](img/076f1f1acc2a15b26fe091e37291845e_37.png)

## 4. 执行预测与后处理

![](img/076f1f1acc2a15b26fe091e37291845e_39.png)

现在，我们可以遍历数据加载器，对每一批图像进行预测。

![](img/076f1f1acc2a15b26fe091e37291845e_41.png)

以下是预测循环中的核心操作：
1.  **前向传播**：将图像数据输入模型，得到原始预测输出。
2.  **非极大值抑制（NMS）**：模型会预测出大量候选框。我们需要使用NMS算法来筛选出最可能代表真实物体的、互不重叠的最佳边界框。其核心公式是依据置信度和IoU（交并比）进行筛选。
3.  **结果转换**：将筛选后的边界框坐标从网络输出格式还原到原始图像尺寸。

![](img/076f1f1acc2a15b26fe091e37291845e_43.png)

```python
for batch_i, (img_paths, input_imgs) in enumerate(dataloader):
    input_imgs = input_imgs.to(device)

    with torch.no_grad():  # 不计算梯度，加速推理
        detections = model(input_imgs)  # 前向传播
        detections = non_max_suppression(detections, conf_thres, nms_thres)  # 执行NMS
```

![](img/076f1f1acc2a15b26fe091e37291845e_45.png)

![](img/076f1f1acc2a15b26fe091e37291845e_47.png)

---

![](img/076f1f1acc2a15b26fe091e37291845e_49.png)

## 5. 结果可视化

![](img/076f1f1acc2a15b26fe091e37291845e_51.png)

最后一步是将检测结果绘制在原始图像上并保存。

![](img/076f1f1acc2a15b26fe091e37291845e_53.png)

以下是可视化的主要过程：
*   遍历NMS处理后的检测结果。
*   对于每个检测到的物体，获取其边界框坐标、置信度和类别ID。
*   根据坐标在图像上绘制矩形框。
*   将类别名称和置信度作为标签标注在框旁。
*   将带标注的图像保存到输出文件夹（如 `./output/`）中。

![](img/076f1f1acc2a15b26fe091e37291845e_55.png)

![](img/076f1f1acc2a15b26fe091e37291845e_57.png)

执行完代码后，会在输出文件夹生成带检测框的图像。例如，一张包含狗和自行车的图片会被正确标记出“dog”和“bicycle”，并显示相应的置信度。

![](img/076f1f1acc2a15b26fe091e37291845e_59.png)

---

![](img/076f1f1acc2a15b26fe091e37291845e_61.png)

## 总结

![](img/076f1f1acc2a15b26fe091e37291845e_63.png)

![](img/076f1f1acc2a15b26fe091e37291845e_65.png)

本节课中我们一起学习了YOLOv3模型预测的完整流程。我们首先配置了测试图像路径，然后加载了训练好的模型权重并将其设置为评估模式。接着，我们使用数据加载器读取图像，通过模型进行前向传播得到预测结果，并利用非极大值抑制（NMS）算法对预测框进行筛选。最后，我们将筛选后的边界框和类别标签可视化在原始图像上。整个过程清晰地展示了如何将一个训练好的目标检测模型应用于实际场景。