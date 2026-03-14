# 工程与科学计算机视觉：15：为分类准备图像 🖼️

![](img/1db534febaeab70d21be2d7eab8351b7_1.png)

![](img/1db534febaeab70d21be2d7eab8351b7_3.png)

在本节课中，我们将学习如何为图像分类任务准备数据。具体来说，我们将学习如何调整图像数据存储的标签、划分训练集与测试集，以及从图像中提取用于机器学习的特征。

## 数据准备与标签调整

![](img/1db534febaeab70d21be2d7eab8351b7_5.png)

要训练一个分类模型，首先需要将图像转化为数字集合。

上一节我们介绍了课程目标，本节中我们来看看数据准备的第一步：调整标签和划分数据集。

首先，您将学习如何调整图像数据存储的标签，并将数据拆分为训练集和测试集。您将使用的数据集不需要大量的图像处理。然而，对于某些数据集，此步骤可能包括额外的处理以实现有效的特征提取，例如使用空间滤波去除噪声。

![](img/1db534febaeab70d21be2d7eab8351b7_7.png)

![](img/1db534febaeab70d21be2d7eab8351b7_8.png)

接下来，您将创建用于机器学习的特征。

![](img/1db534febaeab70d21be2d7eab8351b7_10.png)

这里将使用易于解释的简单特征，例如标准差。

![](img/1db534febaeab70d21be2d7eab8351b7_12.png)

![](img/1db534febaeab70d21be2d7eab8351b7_14.png)

最终目标是将混凝土的图像（一些有裂缝，一些没有裂缝）转化为一个表格。其中每张图像都有一个标签和描述它的特征。

![](img/1db534febaeab70d21be2d7eab8351b7_16.png)

![](img/1db534febaeab70d21be2d7eab8351b7_18.png)

让我们从分配标签开始。

![](img/1db534febaeab70d21be2d7eab8351b7_20.png)

幸运的是，在这个混凝土数据集中，您的图像已经被分类到两个文件夹中，文件夹名称描述了图像内容。在本例中，“positive”表示有裂缝的图像，“negative”表示无裂缝的图像。

![](img/1db534febaeab70d21be2d7eab8351b7_22.png)

当您从此图像集合创建数据存储时，可以使用文件夹名称作为标签。当前的标签是“negative”和“positive”。

![](img/1db534febaeab70d21be2d7eab8351b7_24.png)

为了使这些标签更具描述性，可以使用 `renamecats` 函数将现有标签更改为“no crack”和“crack”。

接下来，将数据存储拆分为训练集和测试集。

![](img/1db534febaeab70d21be2d7eab8351b7_26.png)

`splitEachLabel` 命令从每个标签中取一个子集，并将它们分配给一个新的数据存储。您输入的比例决定了每个子集的大小。对于这种规模的数据集，通常使用80%的图像进行训练，剩余的20%作为测试集，用于后续评估最终模型。

为了避免偏向于将某个标签中的哪些图像放入训练集和测试集，请使用随机化选项。在本视频的其余部分，您将只使用训练集中的图像。

## 特征提取

现在您的数据集已准备就绪，下一步是提取特征。

观察几张图像。请注意，与较浅的混凝土相比，裂缝颜色非常深。因此，您可能会预期，与有裂缝的图像相比，无裂缝的图像具有更高的平均强度值和更少的强度差异。

![](img/1db534febaeab70d21be2d7eab8351b7_28.png)

让我们利用这些观察结果，基于强度创建特征。

从数据存储中读取第一张图像。请记住，在进行大多数计算之前，需要将图像转换为 `double` 数据类型。然后转换为灰度图像以隔离像素的强度值。

![](img/1db534febaeab70d21be2d7eab8351b7_30.png)

基于我们在标签之间观察到的强度差异，使用每张图像强度的**均值**和**标准差**作为特征是合理的。

![](img/1db534febaeab70d21be2d7eab8351b7_31.png)

为了将特征与其原始图像关联起来，使用 `fileparts` 函数提取文件名。

因为您需要多次重复此过程，所以初始化变量并使用 `while` 循环，为训练集中的每张图像提取特征。

以下是提取特征的核心代码流程：

```matlab
% 初始化变量
imageFeatures = [];
imageNames = {};
imageLabels = {};

![](img/1db534febaeab70d21be2d7eab8351b7_33.png)

% 循环处理每张图像
while hasdata(imdsTrain)
    [img, info] = read(imdsTrain);
    % 转换为灰度图
    imgGray = rgb2gray(im2double(img));
    % 计算特征：均值和标准差
    featureVector = [mean(imgGray(:)), std(imgGray(:))];
    % 存储结果
    imageFeatures = [imageFeatures; featureVector];
    [~, name, ~] = fileparts(info.Filename);
    imageNames = [imageNames; name];
    imageLabels = [imageLabels; info.Label];
end

% 创建最终表格
featureTable = table(imageLabels, imageNames, imageFeatures(:,1), imageFeatures(:,2), ...
                     'VariableNames', {'Label', 'FileName', 'MeanIntensity', 'StdIntensity'});
```

![](img/1db534febaeab70d21be2d7eab8351b7_35.png)

最后，创建一个包含所有输出的表格。包括图像标签、文件名和特征。

![](img/1db534febaeab70d21be2d7eab8351b7_37.png)

## 创建可重用函数

您可能希望每次从新的混凝土图像集中提取特征时都执行完全相同的步骤，因此最好将此代码保存为一个函数。

![](img/1db534febaeab70d21be2d7eab8351b7_39.png)

将该函数保存为代码文件，放在与本视频其余代码相同的文件夹中。给它起一个描述性的名称，例如 `extractConcreteFeatures`。

![](img/1db534febaeab70d21be2d7eab8351b7_41.png)

现在，您可以在需要时调用此函数来提取这些特征。

![](img/1db534febaeab70d21be2d7eab8351b7_43.png)

确保保存函数创建的表格，稍后将使用它来训练图像分类模型。

![](img/1db534febaeab70d21be2d7eab8351b7_45.png)

同时，保存您的训练和测试数据存储。让我们通过在训练集上调用新函数来测试它。

![](img/1db534febaeab70d21be2d7eab8351b7_47.png)

很好。现在您有了一个表格，其中包含每张图像的两个特征和一个标签。

![](img/1db534febaeab70d21be2d7eab8351b7_49.png)

## 特征评估与总结

现在面临一个重要问题：您准备好进行分类了吗？

![](img/1db534febaeab70d21be2d7eab8351b7_51.png)

![](img/1db534febaeab70d21be2d7eab8351b7_52.png)

在训练模型之前，最好先研究一下您的特征。

在这种情况下，您可能想知道所选特征是否具有足够的描述性，能够合理地区分有裂缝和无裂缝的混凝土图像。

![](img/1db534febaeab70d21be2d7eab8351b7_54.png)

![](img/1db534febaeab70d21be2d7eab8351b7_55.png)

为了探索您提取的特征是否有助于区分这些标签，使用 `gscatter` 函数绘制按标签着色的强度均值和强度标准差。

![](img/1db534febaeab70d21be2d7eab8351b7_57.png)

![](img/1db534febaeab70d21be2d7eab8351b7_58.png)

哇，您可以看到橙色（有裂缝）类别和蓝色（无裂缝）类别之间有明显的区别。

似乎很可能找到一个决策边界，能够大致分离这些类别。

![](img/1db534febaeab70d21be2d7eab8351b7_60.png)

![](img/1db534febaeab70d21be2d7eab8351b7_61.png)

现在您已经准备好了数据并提取了特征，您已拥有开始训练机器学习模型所需的一切。

![](img/1db534febaeab70d21be2d7eab8351b7_63.png)

本节课中我们一起学习了为图像分类准备数据的关键步骤：调整数据标签、划分数据集、从图像中提取特征（如强度均值和标准差），并将这些过程封装为可重用的函数。这些步骤是将图像转化为机器学习模型可理解数据的基础。