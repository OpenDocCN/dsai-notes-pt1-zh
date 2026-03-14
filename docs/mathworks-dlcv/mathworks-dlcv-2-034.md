# 深度学习在计算机视觉中的应用：34：使用第三方模型 🧩

![](img/2be7c85cc26044540793b8b5488ce58c_1.png)

在本节课中，我们将要学习如何在 MATLAB 与其他深度学习平台（如 TensorFlow 和 PyTorch）之间导入和导出模型。掌握这项技能，可以让你轻松地与他人共享模型，或者利用其他平台构建的优秀网络。

![](img/2be7c85cc26044540793b8b5488ce58c_3.png)

![](img/2be7c85cc26044540793b8b5488ce58c_5.png)

---

![](img/2be7c85cc26044540793b8b5488ce58c_7.png)

## 概述

![](img/2be7c85cc26044540793b8b5488ce58c_9.png)

在本系列课程中，你一直在 MATLAB 中使用深度学习网络。但如果你需要与不使用 MATLAB 的同事共享模型，或者想使用在其他平台上构建的网络，手动翻译这些网络架构会非常繁琐。幸运的是，MATLAB 可以自动将模型导入或导出到多种其他深度学习平台。

![](img/2be7c85cc26044540793b8b5488ce58c_11.png)

上一节我们介绍了在单一平台内的工作流程，本节中我们来看看如何跨越平台边界，实现模型的互操作。

![](img/2be7c85cc26044540793b8b5488ce58c_13.png)

---

![](img/2be7c85cc26044540793b8b5488ce58c_15.png)

## 导出 MATLAB 模型到 TensorFlow 🚀

![](img/2be7c85cc26044540793b8b5488ce58c_17.png)

![](img/2be7c85cc26044540793b8b5488ce58c_19.png)

首先，我们来看一个将 MATLAB 模型导出到 TensorFlow 的例子。

![](img/2be7c85cc26044540793b8b5488ce58c_21.png)

1.  **加载模型**：首先，从课程文件中加载一个紧固件分类模型。
    ```matlab
    load('fastenersClassificationModel.mat');
    ```

2.  **导出网络**：然后，使用 `exportNetworkToTensorFlow` 函数导出网络，并指定生成的 Python 包名称。
    ```matlab
    exportNetworkToTensorFlow(net, 'my_tensorflow_model');
    ```

![](img/2be7c85cc26044540793b8b5488ce58c_23.png)

3.  **在 Python 中使用**：创建完成后，只需两行代码即可将模型加载到 Python 中。
    ```python
    import my_tensorflow_model
    model = my_tensorflow_model.load_model()
    ```
    之后，它可以像任何其他 TensorFlow 模型一样使用。

![](img/2be7c85cc26044540793b8b5488ce58c_25.png)

**注意**：MATLAB 中的某些层具有 TensorFlow 中不可用的功能。因此，你可能需要在导出的模型中定义自定义层。如果出现这种情况，导出模型时会收到警告，其中详细说明了需要定义哪些层以及在代码中的何处进行定义。

![](img/2be7c85cc26044540793b8b5488ce58c_27.png)

在本例中，函数成功翻译了模型，因此我们不需要编写任何额外的代码。

![](img/2be7c85cc26044540793b8b5488ce58c_29.png)

![](img/2be7c85cc26044540793b8b5488ce58c_31.png)

---

![](img/2be7c85cc26044540793b8b5488ce58c_33.png)

## 从 TensorFlow 导入模型到 MATLAB 🔄

![](img/2be7c85cc26044540793b8b5488ce58c_35.png)

现在，假设你想将 TensorFlow 或 PyTorch 等平台的模型导入 MATLAB。你可能希望可视化网络以更好地理解其架构，使用它对一组新图像进行预测或检测，或者以此网络为起点进行迁移学习。

![](img/2be7c85cc26044540793b8b5488ce58c_37.png)

![](img/2be7c85cc26044540793b8b5488ce58c_38.png)

在视频的剩余部分，你将把一个简单的分类模型从 TensorFlow 导入 MATLAB，并使用课程文件中的交通标志数据集进行迁移学习。

![](img/2be7c85cc26044540793b8b5488ce58c_40.png)

以下是导入模型并进行迁移学习的步骤：

1.  **导入模型**：你可以使用代码或 **Deep Network Designer** 应用程序导入模型。这里我们展示基于应用程序的工作流程。
    *   在应用程序中，选择要从中导入模型的平台。
    *   然后选择存储模型的文件夹。应用程序会自动将模型转换为 MATLAB 网络，根据模型的复杂性，这可能需要几分钟时间。

![](img/2be7c85cc26044540793b8b5488ce58c_42.png)

2.  **检查导入报告**：模型导入后，应用程序会显示一份报告，描述模型的任何潜在问题，例如未自动转换的自定义层。在本例中，我们的层没有任何问题。

![](img/2be7c85cc26044540793b8b5488ce58c_44.png)

3.  **探索网络**：让我们探索导入模型的层和参数。你会发现这个模型并不复杂，输入尺寸为 **32x32** 像素，只有两个卷积层。但你可以使用相同的工作流程导入具有任意层数的模型。

![](img/2be7c85cc26044540793b8b5488ce58c_46.png)

![](img/2be7c85cc26044540793b8b5488ce58c_48.png)

4.  **导出到工作区**：检查完网络后，将其导出到 MATLAB 工作区。现在，你可以像使用任何其他深度学习分类器一样使用这个模型。

![](img/2be7c85cc26044540793b8b5488ce58c_50.png)

---

![](img/2be7c85cc26044540793b8b5488ce58c_52.png)

## 使用导入的模型进行迁移学习 🎯

![](img/2be7c85cc26044540793b8b5488ce58c_54.png)

让我们使用它和交通标志数据集进行迁移学习。

![](img/2be7c85cc26044540793b8b5488ce58c_56.png)

![](img/2be7c85cc26044540793b8b5488ce58c_58.png)

以下是具体操作步骤：

![](img/2be7c85cc26044540793b8b5488ce58c_60.png)

1.  **准备数据**：首先，导入训练数据并将其拆分为训练集和验证集。
    ```matlab
    [imdsTrain, imdsValidation] = splitEachLabel(imds, 0.7, 'randomized');
    ```

![](img/2be7c85cc26044540793b8b5488ce58c_62.png)

![](img/2be7c85cc26044540793b8b5488ce58c_64.png)

2.  **调整数据尺寸**：然后调整数据大小以匹配网络输入。
    ```matlab
    inputSize = net.Layers(1).InputSize;
    augimdsTrain = augmentedImageDatastore(inputSize, imdsTrain);
    augimdsValidation = augmentedImageDatastore(inputSize, imdsValidation);
    ```

![](img/2be7c85cc26044540793b8b5488ce58c_66.png)

![](img/2be7c85cc26044540793b8b5488ce58c_67.png)

3.  **修改网络层**：替换模型的最后三层以适应类别数量。
    ```matlab
    numClasses = numel(categories(imdsTrain.Labels));
    layers = [
        net.Layers(1:end-3)
        fullyConnectedLayer(numClasses)
        softmaxLayer
        classificationLayer
    ];
    ```

![](img/2be7c85cc26044540793b8b5488ce58c_69.png)

4.  **训练模型**：最后，设置训练选项并训练模型。
    ```matlab
    options = trainingOptions('sgdm', ...
        'InitialLearnRate', 0.001, ...
        'ValidationData', augimdsValidation, ...
        'Plots', 'training-progress');
    netTransfer = trainNetwork(augimdsTrain, layers, options);
    ```

![](img/2be7c85cc26044540793b8b5488ce58c_71.png)

5.  **评估模型**：训练完成后，在测试图像上评估它。对于如此简单的网络，这些结果已经相当不错。

![](img/2be7c85cc26044540793b8b5488ce58c_73.png)

![](img/2be7c85cc26044540793b8b5488ce58c_75.png)

---

![](img/2be7c85cc26044540793b8b5488ce58c_77.png)

## 总结与扩展 📚

![](img/2be7c85cc26044540793b8b5488ce58c_79.png)

在本视频中，你看到了如何使用代码和 Deep Network Designer 应用程序导出到 TensorFlow 以及从 TensorFlow 导入。对于 PyTorch 等其他第三方平台，过程是相同的。

![](img/2be7c85cc26044540793b8b5488ce58c_80.png)

转换通常是完全自动化的，但偶尔你可能需要编写代码来翻译自定义层。要查看支持哪些层，并了解更多关于 MATLAB 与不同外部平台之间互操作性的信息，请查阅官方文档。这是获取最新信息的最佳方式，因为支持的层和平台数量一直在增长。

![](img/2be7c85cc26044540793b8b5488ce58c_82.png)

![](img/2be7c85cc26044540793b8b5488ce58c_84.png)

本节课中我们一起学习了跨平台模型互操作的基本方法，这为你在更广阔的深度学习生态系统中灵活工作打下了基础。