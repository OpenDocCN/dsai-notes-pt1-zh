# 52：使用 Python 进行地理空间数据分析入门 🗺️

![](img/81e8c90c191b11ba7052bce0b0522fb5_1.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_3.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_5.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_7.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_9.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_11.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_13.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_15.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_16.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_17.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_19.png)

在本课程中，我们将学习如何使用 Python 进行地理空间数据分析。课程将分为两部分：第一部分介绍 GeoPandas 及相关库，用于处理地理空间数据；第二部分介绍 PySAL 库，用于更深入的地理空间数据科学分析。

![](img/81e8c90c191b11ba7052bce0b0522fb5_21.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_23.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_25.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_27.png)

## 概述 📋

![](img/81e8c90c191b11ba7052bce0b0522fb5_29.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_31.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_33.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_35.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_37.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_39.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_41.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_42.png)

欢迎来到地理空间数据分析教程。我是来自加州大学河滨分校的 Serge Rey，同时也是本次 SciPy 会议的联合主席之一。本次教程将由我、来自比利时的 Joris（GeoPandas 核心成员）、来自布里斯托大学的 Levi Wolf（PySAL 团队成员）以及来自利物浦大学的 Danny Arribas-Bel（PySAL 团队成员）共同完成。这是 GeoPandas 和 PySAL 的首次联合教程。

![](img/81e8c90c191b11ba7052bce0b0522fb5_44.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_46.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_48.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_50.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_52.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_54.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_56.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_58.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_59.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_61.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_63.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_65.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_67.png)

本次课程分为四个部分。Joris 将开始第一部分，Danny 接替，中间休息后，将由我和 Levi 负责下半场。由于我们有四位讲师，如果大家在安装或学习过程中遇到问题，可以随时寻求帮助。

![](img/81e8c90c191b11ba7052bce0b0522fb5_69.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_71.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_73.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_75.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_76.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_78.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_80.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_82.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_84.png)

如果你没有在本地安装好环境，可以尝试使用 Binder。在 GeoPandas 的代码仓库中有一个“启动 Binder”按钮，它会在线创建一个包含所有材料的可运行环境。虽然速度可能不快，但可以作为本地环境无法工作时的备用方案。

![](img/81e8c90c191b11ba7052bce0b0522fb5_85.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_87.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_89.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_91.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_93.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_95.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_97.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_98.png)

在开始之前，我们先了解一下大家的背景。有多少人之前使用过 GeoPandas？有多少人使用过 PySAL 或相关库？对于尚未使用过 GeoPandas 的学员，你们是否使用过 Pandas 库？很好，大多数人都用过。这意味着我们假设大家对 Pandas 有一定了解，不会详细解释 Pandas 的所有功能。

![](img/81e8c90c191b11ba7052bce0b0522fb5_100.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_102.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_104.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_105.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_107.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_109.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_111.png)

课程将从 GeoPandas 及相关库的介绍开始，学习如何读取数据、进行查询和操作。休息之后，我们将进入 PySAL 部分，专注于地理空间数据科学分析。

![](img/81e8c90c191b11ba7052bce0b0522fb5_113.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_115.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_116.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_118.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_119.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_120.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_122.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_123.png)

## 第一部分：GeoPandas 入门 🚀

![](img/81e8c90c191b11ba7052bce0b0522fb5_125.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_126.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_128.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_129.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_131.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_133.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_135.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_136.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_138.png)

上一节我们介绍了课程的整体安排，本节我们将开始学习 GeoPandas。

![](img/81e8c90c191b11ba7052bce0b0522fb5_140.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_142.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_144.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_146.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_147.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_149.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_151.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_152.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_153.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_155.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_157.png)

### 启动 Jupyter Notebook

![](img/81e8c90c191b11ba7052bce0b0522fb5_159.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_161.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_162.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_163.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_165.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_167.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_169.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_171.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_173.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_175.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_177.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_179.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_181.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_183.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_185.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_187.png)

首先，在你下载或通过 Git 克隆的目录中启动 Jupyter Notebook。你应该能看到类似这样的界面。我们将从第一个笔记本开始：“notebook 1 - 地理空间数据介绍”。

![](img/81e8c90c191b11ba7052bce0b0522fb5_189.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_191.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_193.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_194.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_196.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_198.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_199.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_201.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_202.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_204.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_205.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_206.png)

### 矢量数据与栅格数据

![](img/81e8c90c191b11ba7052bce0b0522fb5_208.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_210.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_212.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_214.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_215.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_216.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_218.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_220.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_221.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_222.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_223.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_225.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_227.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_229.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_231.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_233.png)

在昨天的 Cartopy 教程中，有人能解释一下矢量数据和栅格数据的区别吗？是的，矢量数据，正如标题所示，是本教程的重点。我们主要关注多边形（如城市中的街区）、点和线串这类地理空间数据。另一方面，栅格数据通常是卫星图像等像素图像。本教程将主要聚焦于矢量数据。

![](img/81e8c90c191b11ba7052bce0b0522fb5_235.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_237.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_239.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_241.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_242.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_244.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_246.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_248.png)

因为矢量数据（例如城市中的兴趣点）通常除了位置信息外，还有关于这些点或多边形的附加属性。这些属性通常以表格形式存储，每一行对应一个空间位置的元数据。因此，这非常自然地适合使用表格，进而在 Python 中使用 Pandas 的 DataFrame。这就是我们将要处理的数据类型。

![](img/81e8c90c191b11ba7052bce0b0522fb5_250.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_251.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_253.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_254.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_256.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_258.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_260.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_262.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_264.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_266.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_267.png)

### 读取地理空间数据

![](img/81e8c90c191b11ba7052bce0b0522fb5_269.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_271.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_273.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_275.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_277.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_279.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_281.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_283.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_284.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_286.png)

首先，我们需要获取数据。在地理空间数据领域，有许多不同的文件格式，例如 shapefile、GeoJSON、GeoPackage 等。GeoPandas 提供了一种简单的方法来读取所有这些不同的数据格式，它底层使用了 GDAL 库。GDAL 是整个开源地理空间生态系统的基础库，被许多库和应用程序用于读写数据。

![](img/81e8c90c191b11ba7052bce0b0522fb5_288.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_290.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_292.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_294.png)

以下是一个从我们提供的材料中读取数据的例子。我们提供了一个从 Natural Earth 数据网站下载的包含全球所有国家的 zip 文件。我们可以使用 `geopandas.read_file` 函数来读取这些数据。

![](img/81e8c90c191b11ba7052bce0b0522fb5_296.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_298.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_300.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_301.png)

```python
import geopandas as gpd
countries = gpd.read_file('path/to/natural_earth_data.zip')
```

![](img/81e8c90c191b11ba7052bce0b0522fb5_303.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_305.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_307.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_309.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_310.png)

查看前几行数据，你会看到一个包含所有国家名称、大洲、人口、GDP 等的数据集。这里有一个名为 `geometry` 的列，这很重要。因为我们使用 GeoPandas 读取，所以返回的是一个 GeoDataFrame。它与普通 DataFrame 的区别在于这个 `geometry` 列。在这个例子中，几何类型是多边形，因为国家是多边形。我们也可以快速绘制它。

![](img/81e8c90c191b11ba7052bce0b0522fb5_312.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_314.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_316.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_318.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_319.png)

```python
countries.plot()
```

![](img/81e8c90c191b11ba7052bce0b0522fb5_321.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_323.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_324.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_326.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_328.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_329.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_331.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_333.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_335.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_337.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_339.png)

你会看到一张世界地图。

![](img/81e8c90c191b11ba7052bce0b0522fb5_341.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_343.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_345.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_347.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_349.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_351.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_353.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_355.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_356.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_358.png)

### GeoDataFrame 与 GeoSeries

![](img/81e8c90c191b11ba7052bce0b0522fb5_360.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_361.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_363.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_365.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_366.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_368.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_370.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_372.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_373.png)

我们得到的数据集是一个 GeoDataFrame。正如之前解释的，我们有一个 `geometry` 列，其他列是这些几何图形的属性。与普通 DataFrame 相比，GeoDataFrame 有一些特殊之处：你可以通过 `.geometry` 属性访问几何列。

![](img/81e8c90c191b11ba7052bce0b0522fb5_375.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_377.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_378.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_379.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_381.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_383.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_385.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_387.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_389.png)

```python
countries.geometry
```

![](img/81e8c90c191b11ba7052bce0b0522fb5_391.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_393.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_395.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_397.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_399.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_400.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_402.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_404.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_405.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_407.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_408.png)

这将返回一个 GeoSeries。GeoDataFrame 是 DataFrame 的子类，GeoSeries 是 Series 的子类。因此，它拥有普通 Series 的所有功能，并额外增加了一些专门用于地理空间数据的方法和属性。例如，我们可以通过 GeoSeries 的 `.area` 属性获取每个多边形的面积。

![](img/81e8c90c191b11ba7052bce0b0522fb5_410.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_411.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_413.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_415.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_417.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_419.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_421.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_423.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_425.png)

```python
countries.geometry.area
```

![](img/81e8c90c191b11ba7052bce0b0522fb5_427.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_429.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_431.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_433.png)

在后续教程中，我们还会看到 GeoDataFrame 和 GeoSeries 上可用的许多其他方法。

![](img/81e8c90c191b11ba7052bce0b0522fb5_435.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_437.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_439.png)

当然，它仍然是一个 DataFrame。所以，你熟悉的所有 Pandas 操作仍然有效。例如，我们可以访问人口列并计算平均值。

![](img/81e8c90c191b11ba7052bce0b0522fb5_441.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_443.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_445.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_446.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_448.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_450.png)

```python
countries['pop_est'].mean()
```

![](img/81e8c90c191b11ba7052bce0b0522fb5_451.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_453.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_455.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_457.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_458.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_460.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_462.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_464.png)

或者，我们可以过滤数据集，获取一个子集。例如，我们可以获取所有非洲国家。

![](img/81e8c90c191b11ba7052bce0b0522fb5_466.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_468.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_469.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_471.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_472.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_474.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_476.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_478.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_480.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_482.png)

```python
africa = countries[countries['continent'] == 'Africa']
```

![](img/81e8c90c191b11ba7052bce0b0522fb5_484.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_485.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_487.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_489.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_490.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_492.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_494.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_496.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_498.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_500.png)

这样，你就可以使用熟悉的 Pandas 语法轻松地对数据进行子集筛选。

![](img/81e8c90c191b11ba7052bce0b0522fb5_502.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_504.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_506.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_508.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_509.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_511.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_513.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_514.png)

### 几何类型：点、线和多边形

![](img/81e8c90c191b11ba7052bce0b0522fb5_516.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_517.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_519.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_521.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_523.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_525.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_527.png)

到目前为止，我们看到了多边形的例子。但正如开头所说，数据也可以是点或线串。一个几何图形可以是多个点、多条线或多个多边形的组合。对于我们的国家数据，几何类型是多边形。现在，我们将从 Natural Earth 网站导入其他一些数据集：城市（用点表示）和河流（用线串表示）。

![](img/81e8c90c191b11ba7052bce0b0522fb5_529.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_531.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_533.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_535.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_537.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_538.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_539.png)

```python
cities = gpd.read_file('path/to/cities.shp')
rivers = gpd.read_file('path/to/rivers.shp')
```

![](img/81e8c90c191b11ba7052bce0b0522fb5_541.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_543.png)

如果你检查河流几何图形的类型，你会看到它是 Shapely 的 `LineString`。Shapely 是 GeoPandas 底层用于表示点、线和多边形等几何对象的库。它也是提供所有特定地理空间方法的库，实际上它通过接口调用 GEOS 库（例如 PostgreSQL 的 PostGIS 也使用相同的 C 库）。

![](img/81e8c90c191b11ba7052bce0b0522fb5_545.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_547.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_549.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_551.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_553.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_554.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_556.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_558.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_559.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_561.png)

### 创建几何对象

![](img/81e8c90c191b11ba7052bce0b0522fb5_563.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_565.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_567.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_568.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_570.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_572.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_574.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_575.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_576.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_578.png)

到目前为止，我们都是从文件读取数据。如果需要，你也可以自己构造一些点或多边形。例如，使用 `Point` 并提供 X 和 Y 坐标（经度和纬度），或者使用 `Polygon` 并提供坐标列表。

![](img/81e8c90c191b11ba7052bce0b0522fb5_580.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_581.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_583.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_585.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_587.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_589.png)

```python
from shapely.geometry import Point, Polygon
point = Point(0, 0)
polygon = Polygon([(0, 0), (1, 0), (1, 1), (0, 1)])
```

在教程的后续部分，我们还会看到 GeoDataFrame 上可用的许多方法。重要的是要记住，这些方法通常也适用于单个几何对象。例如，对于多边形，我们可以使用 `.area` 属性获取面积。对于单个多边形，我们也可以获取其面积。

![](img/81e8c90c191b11ba7052bce0b0522fb5_591.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_593.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_595.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_596.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_598.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_600.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_602.png)

### 坐标参考系统

![](img/81e8c90c191b11ba7052bce0b0522fb5_604.png)

之前参加过 Cartopy 教程的人现在应该对地图投影系统的重要性相当熟悉了。GeoPandas 本身对坐标参考系统是“无知”的，它不会自动进行重投影。因此，你需要确保，例如在将多个图层叠加在一起时，它们具有相同的坐标参考系统。但它提供了一些功能来实现这一点。

GeoDataFrame 和 GeoSeries 都有一个 `crs` 属性，表示坐标参考系统。在这个例子中，对于国家数据，我们看到它是 EPSG:4326，这表示它是基于 WGS84 的最广泛使用的经纬度坐标系。

![](img/81e8c90c191b11ba7052bce0b0522fb5_606.png)

我们可以直接绘制，不使用任何特定的地图投影。但我们也可以改变它。例如，我们可以使用 `to_crs` 方法将我们的 GeoSeries 从一个坐标参考系统转换到另一个。简单的方法是提供 EPSG 代码。大多数坐标参考系统都有这样的代码。在这个例子中，我将把坐标转换为 UTM 投影。

![](img/81e8c90c191b11ba7052bce0b0522fb5_608.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_610.png)

```python
countries_utm = countries.to_crs(epsg=32633)
```

![](img/81e8c90c191b11ba7052bce0b0522fb5_612.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_614.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_616.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_618.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_620.png)

如果你查看转换后的数据，坐标范围会变得更大。如果你绘制它，你会看到现在有了不同的投影，格陵兰岛看起来比之前大得多。

![](img/81e8c90c191b11ba7052bce0b0522fb5_622.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_624.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_626.png)

在 Cartopy 教程中，我们看到了这些投影系统或地图投影的重要性。通常，坐标参考系统很重要，例如，如果你想绘图，你有不同的选项。转换功能也很重要，如果你从不同来源获取数据，它们通常以不同的参考系统提供，你可能希望将它们转换为一个通用的系统。

![](img/81e8c90c191b11ba7052bce0b0522fb5_628.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_629.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_631.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_633.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_635.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_636.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_638.png)

对于 GeoPandas 来说，第二点也很重要。你可以用 GeoPandas 计算距离，但它只是假设你的坐标在笛卡尔平面上。因此，如果你想要一个可以理解的距离度量（例如两点之间有多少公里），你通常希望单位是米而不是度。所以，如果你有一个经纬度数据集，并想用 GeoPandas 计算以米为单位的距离，你需要将其转换为一个以米为单位而不是度的坐标参考系统。这是你在 GeoPandas 中可能想要这样做的另一个好理由。

![](img/81e8c90c191b11ba7052bce0b0522fb5_640.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_642.png)

### 组合绘图

![](img/81e8c90c191b11ba7052bce0b0522fb5_644.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_646.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_648.png)

最后，作为这个初步介绍的结束，这里有一个如何将不同要素绘制在一起的例子。我已经展示了如何绘制国家数据集。我们将结果分配给一个轴对象，这样我们就可以使用该轴来添加其他图层，例如河流和城市。由于它基于 Matplotlib，因此典型的样式设置方法在这里也可用。

![](img/81e8c90c191b11ba7052bce0b0522fb5_650.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_652.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_654.png)

```python
ax = countries.plot(edgecolor='black', facecolor='none')
rivers.plot(ax=ax, color='blue')
cities.plot(ax=ax, color='red', markersize=5)
```

这样，我们就将点、线串和多边形绘制在了一起。

## 练习：案例研究 - 冲突映射 🧭

![](img/81e8c90c191b11ba7052bce0b0522fb5_656.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_658.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_659.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_661.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_663.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_665.png)

上一节我们介绍了 GeoPandas 的基础知识，本节我们将通过一个练习来实践。

![](img/81e8c90c191b11ba7052bce0b0522fb5_667.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_669.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_671.png)

现在，我们将进入一个包含更详细练习的笔记本，以便你可以开始练习。这个笔记本是关于“案例研究：冲突映射”的。我会简要介绍一下数据集，然后你就可以开始练习了。

![](img/81e8c90c191b11ba7052bce0b0522fb5_673.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_675.png)

我在这里提供的数据集是关于刚果东部手工采矿点的数据。一个名为 IPIS 的组织访问了采矿点，并调查了不同采矿点的数据，包括工人数量、是否受武装团体控制、开采的矿物等。这是对刚果采矿点的一项调查。

![](img/81e8c90c191b11ba7052bce0b0522fb5_677.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_679.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_681.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_683.png)

他们提供了一个公共 API 来获取数据。这里有一个如何直接与其 API 交互的例子，但让我们从练习开始，读取 GeoJSON 数据，因为你也可以将其下载为 GeoJSON 文件。这个数据集在代码仓库的 `data` 目录中可用，所以第一个练习是从 GeoJSON 文件读取数据并检查前几行。

![](img/81e8c90c191b11ba7052bce0b0522fb5_685.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_687.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_689.png)

在开始之前的一个注意事项：我建议你真正自己尝试，但如果你遇到困难而我们又不在身边，你可以先尝试询问我们，或者给你一个提示。但如果你想查看解决方案，或者你已经完成并想进行比较，你可以随时取消注释并运行该单元格，你会看到一个可能的解决方案。它可能与你的解决方案不完全相同，但这并不意味着你的解决方案不好，因为通常有几种方法可以做某事。这样，你可以检查它，如果你比我们进度快，也许可以更进一步。

![](img/81e8c90c191b11ba7052bce0b0522fb5_691.png)

你可以尝试这个练习，如果准备好了，可以运行这里的代码，并完成下面的练习。你至少可以做到这里的“点 2”。我将讲解一些第一个练习，并强调我们遇到的一些问题。

![](img/81e8c90c191b11ba7052bce0b0522fb5_693.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_695.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_697.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_699.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_700.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_702.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_703.png)

首先，要读取文件，我们使用 `geopandas.read_file`，然后传递目录或文件名路径，像这样，我们将其称为 `mining_visits`。

![](img/81e8c90c191b11ba7052bce0b0522fb5_705.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_707.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_709.png)

```python
mining_visits = gpd.read_file('data/mining_sites.geojson')
```

![](img/81e8c90c191b11ba7052bce0b0522fb5_711.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_713.png)

我们可以使用 Pandas 的功能来检查前几行。为了给你一点背景，因为有很多列，而我们不需要所有列，所以我在这里提供了一些代码来选择列的子集。

![](img/81e8c90c191b11ba7052bce0b0522fb5_715.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_717.png)

现在看看我们这里有什么数据：我们有点数据，所以我们的采矿点是点。数据包括开采的矿物、是否有武装团体、工人数量、矿山名称、ID 以及访问日期等。

我在这里提供了一些代码来创建数据的子集：我们只选取那些由 IPIS 访问过的、至少有超过零名工人的活跃矿山，并按日期排序。由于一些矿山被多次访问，我通过按代码分组并取最后一个（如果矿山被多次访问，则取最后一次）来进行去重。最后，由于分组操作，我们失去了它是 GeoDataFrame 的事实，所以需要转换回来。

现在，我们有一个包含两千多个采矿点的数据集。

我们将在整个练习中使用的另一个数据集是关于保护区的数据，即刚果的国家公园。同样，我们提供了获取数据的链接。如果你以类似的方式操作，你会看到类似的结果。

如果你查看两个数据集的坐标参考系统，你会发现它们不同。例如，如果你想将它们绘制在一起以查看它们的关系，你需要先将两个数据集转换为一个共同的参考系统。

另一个原因是，如果你想进行基于距离的计算。例如，这里我们将从 Shapely 导入一个 `Point` 来创建一个单点。我们需要使用这些坐标，但要注意：在 GeoPandas 和 Cartopy 中，顺序始终是经度、纬度（X 和 Y）。所以，1.66 是南纬（Y），我们需要将其放在最后。然后问题是计算所有矿山到这个点的距离，并找到五个最小的。布卡武是基伍省较大的城市之一，靠近卢旺达边境。

![](img/81e8c90c191b11ba7052bce0b0522fb5_719.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_721.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_723.png)

你如何计算这个？我们有我们的矿点，我们可以使用 `.distance` 方法。如果你给它一个单点，它将计算数据集中每个点到该点的距离。如果你想找到五个最近的供应商（即最小的距离），我们可以对这些距离进行排序。前五个值在这里，但 0.38 这个值在度单位下意义不大。因此，如果你想在 GeoPandas 中使用距离，你需要将坐标参考系统转换为以米为单位而不是经纬度的系统。

![](img/81e8c90c191b11ba7052bce0b0522fb5_725.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_727.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_729.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_730.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_732.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_734.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_736.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_738.png)

因此，有两个理由需要转换我们的坐标参考系统。让我们转换它们。第一个数据集已经是经纬度。第二个数据集（保护区数据）下载时是 World Mercator 投影。我们将把两者都转换为当地的 UTM 区域。通常，如果你的数据不覆盖整个世界，而是某个国家或城市，你可以使用当地的 UTM 区域，这样只要你的数据相对适合该 UTM 区域，你在形状或计算的距离上就不会有大的失真。

![](img/81e8c90c191b11ba7052bce0b0522fb5_740.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_742.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_744.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_746.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_747.png)

如何转换我们的数据？记住，我们可以使用 `.to_crs` 方法。然后我们提供我们想要的 EPSG 代码。我们的数据原来是经纬度，现在你看到不再是这种情况了。我们可以称之为 `utm`。然后快速展示另一个数据集。我们可以对两者都这样做：采矿点和保护区。如果我们现在将它们绘制在一起，你会看到它们在同一个区域。我给它绿色，让点小一点，这样我们可以使用典型的 Matplotlib 关键字来设置样式。我们还可以设置一些透明度以便叠加。点太多，可能看不清效果，但你可以使用典型的 Matplotlib 功能。我们可能还想关闭坐标轴，因为在这种情况下，x 和 y 标签不那么有趣。我们还可以让它更大一点，比如 15x15。

接下来的练习，我试图分几步快速自定义绘图。你可以随时查看这些练习，但对于本教程，我们现在将切换到一些实际的空间操作。为此，你可以打开第二个笔记本。

## 第二部分：空间操作、关系和叠加操作 🔗

![](img/81e8c90c191b11ba7052bce0b0522fb5_749.png)

欢迎来到教程的第二部分。我是利物浦大学的 Danny Arribas-Bel。在第二部分中，我们将研究所谓的空间关系或空间谓词，即如何将具有位置的不同观测联系起来。

在开始之前，让我们导入 Pandas 和 GeoPandas，并读取所有数据，以便后续使用。

![](img/81e8c90c191b11ba7052bce0b0522fb5_751.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_753.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_755.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_757.png)

这几乎就像回到中学时代，你有不同的形状和物体，你想以不同的方式组合它们。在 GeoPandas 支持的空间操作中，有很多种。我们今天要看的是这些类型：如果我们有两个对象（可以是多边形、点或线等所有矢量对象），我们可以检查一个是否在另一个内部，它们是否接触，一条线是否与另一个多边形交叉，或者两个对象是否重叠等。这只是几个例子，有一整套空间关系列表。就像生活中所有重要的事情一样，维基百科上有一个页面专门介绍这个。

![](img/81e8c90c191b11ba7052bce0b0522fb5_759.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_761.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_763.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_765.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_767.png)

我们设计这部分的方式是，首先在个体层面（原子层面）查看这些关系。例如，你有一个多边形、一个点或一条线，你想检查该点是否在多边形内，或者该线是否与多边形交叉。这在概念上（至少对我来说）是理解 GeoPandas 最终在做什么更有用的方式。但在大多数情况下，你不会只有一个多边形或一条线，你会有一个由许多线段组成的整个街道网络，并且你想检查每个线段，而你不想手动完成。GeoPandas 使得将这些操作广播到大量观测变得非常容易。

![](img/81e8c90c191b11ba7052bce0b0522fb5_769.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_771.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_772.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_773.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_775.png)

在开始之前，让我们从原子关系开始。你可以看出这不是我写的，而是团队中的比利时人编写的这些练习。让我们随机挑选一个例子，选择比利时这个国家。

![](img/81e8c90c191b11ba7052bce0b0522fb5_777.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_779.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_781.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_783.png)

我们查询国家数据 DataFrame，保留所有名称列等于“Belgium”的观测（世界上只有一个比利时），然后对于整行，我们只保留几何列。因为这仍然会给我们一个 GeoDataFrame，而我们想要查询的正是几何图形，所以我们使用这个叫做 `.squeeze()` 的辅助方法。如果 GeoDataFrame 中只有一个观测，它会将其压缩成一个 GeoSeries，并且因为那只是一个单列，它实际上会将其压缩成地理对象。

![](img/81e8c90c191b11ba7052bce0b0522fb5_785.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_787.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_789.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_791.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_793.png)

如果我们这样做，笔记本有一个非常好的渲染/打印方法，所以当你只运行 `belgium` 时，你会得到比利时的形状。对象一完成。让我们做第二个：巴黎。团队中另一位成员碰巧在巴黎工作，尽管他不是法国人。让我们选择巴黎和布鲁塞尔作为两个点，这将给我们两个点对象。我们采取的方法完全相同：查询 DataFrame，只保留几何图形，然后将其压缩成地理对象。

快速提问：当你刚刚创建比利时地图时，你没有调用 `.plot()` 方法，所以有一个内置的绘图功能？是的，问题是：我只是输入了 `belgium`，突然间它就绘制了比利时，我不必说 `belgium.plot()`。原因是，实际上让我快速解释一下区别。如果我这样做，它会打印多边形，然后绘制多边形。但因为 `belgium` 已经是多边形对象，所以有一个内置的渲染/快速打印功能，会在笔记本上打印多边形。这只是为了快速查看，因为你不能真正用它来组合不同的形状或任何东西，它只是一个快速的查看。这是内置在 GeoPandas 中的，但只适用于 Jupyter Notebook 或富控制台显示。它是 Shapely 的特殊漂亮表示。如果你想查看坐标，你需要使用打印语句。

![](img/81e8c90c191b11ba7052bce0b0522fb5_795.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_797.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_799.png)

然后我们对两个点（巴黎和布鲁塞尔）做同样的事情，我们可以看到这也给出了类似的表示，但因为它是一个点，除非你有多个点或更复杂的点对象，否则不是非常有用。但它是存在的，有时当我不知道数据集并想确保它是我认为的样子时，我实际上经常使用它。

![](img/81e8c90c191b11ba7052bce0b0522fb5_801.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_803.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_805.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_807.png)

最后，让我们创建一条线。我们做的是从 Shapely 导入 `LineString` 类。这是另一个地理或几何对象，API 设计得非常简洁。你只需要传递一个单独点的列表，它会给你一条从点一到点二的线。在这个例子中，我们只是创建一条从巴黎到布鲁塞尔的线。与其他对象一样，这条线并不复杂，所以对我们帮助不大。但如果你有一个更复杂的轨迹，它仍然会渲染，并且对探索有用。

![](img/81e8c90c191b11ba7052bce0b0522fb5_809.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_811.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_813.png)

![](img/81e8c90c191b11ba7052bce0b0522fb5_814.png)

然后我们也可以组合它们。一旦我们有了对象，就像 Python 和 Pandas 处理数据一样，我们可以非常轻松地组合它们。在 GeoPandas 中，我们可以创建一个包含不同几何图形的 GeoSeries。通常，至少在大多数用例中，你不会希望在一个数据集中有不同的几何类型（例如将点与多边形和线混合），但如果你愿意，你可以这样做，而且非常容易。同样，你可以创建一个包含所有对象的列表的 GeoSeries，然后直接将其传递给绘图函数，它会给我们一个图。回到