# 7：利用 Jupyter、Rust 和 WebAssembly 实现交互式数据可视化 📊

在本节课中，我们将学习如何结合 Jupyter、Rust 和 WebAssembly 技术，为 YT 数据分析包构建一个高性能的交互式可视化工具。我们将探讨传统方法的局限性，并了解新方法如何通过将计算任务转移到客户端来提升交互体验。

![](img/bb412fc545e07a2acf0d9da61331c993_1.png)

## 讲师介绍

![](img/bb412fc545e07a2acf0d9da61331c993_3.png)

我是 Chelsea，来自伊利诺伊大学厄巴纳-香槟分校的国家超级计算应用中心数据探索实验室。我的研究背景是辐射输运模拟，并且是 YT 项目的维护者之一。我也是 Software Carpentry 的讲师和课程顾问委员会成员。

## 为什么需要交互式可视化？🔍

![](img/bb412fc545e07a2acf0d9da61331c993_5.png)

上一节我们介绍了讲师背景，本节中我们来看看交互式数据可视化的重要性。

![](img/bb412fc545e07a2acf0d9da61331c993_7.png)

![](img/bb412fc545e07a2acf0d9da61331c993_9.png)

交互式可视化允许我们实时调整参数、探索数据细节，从而揭示有趣的科学现象。它使得科学数据对非专业人士、编程新手或刚接触数据集的研究者都更加友好和易于理解。

![](img/bb412fc545e07a2acf0d9da61331c993_11.png)

## 传统方法的挑战 🐢

![](img/bb412fc545e07a2acf0d9da61331c993_13.png)

上一节我们了解了交互式可视化的价值，本节中我们来看看传统实现方式面临的挑战。

![](img/bb412fc545e07a2acf0d9da61331c993_15.png)

![](img/bb412fc545e07a2acf0d9da61331c993_17.png)

在 Jupyter Notebook 中使用标准部件（如 ipywidgets）进行交互时，每次交互（如缩放、平移）都需要将完整的图像数据从服务器发送到客户端浏览器。这个过程涉及以下步骤：

![](img/bb412fc545e07a2acf0d9da61331c993_19.png)

![](img/bb412fc545e07a2acf0d9da61331c993_21.png)

![](img/bb412fc545e07a2acf0d9da61331c993_23.png)

1.  浏览器向服务器发送请求。
2.  服务器计算并生成图像。
3.  图像数据被序列化并传回浏览器。
4.  浏览器下载并显示图像。

![](img/bb412fc545e07a2acf0d9da61331c993_25.png)

![](img/bb412fc545e07a2acf0d9da61331c993_26.png)

![](img/bb412fc545e07a2acf0d9da61331c993_28.png)

![](img/bb412fc545e07a2acf0d9da61331c993_30.png)

![](img/bb412fc545e07a2acf0d9da61331c993_32.png)

这个过程可以概括为以下公式，其中 `n` 是交互次数：
`总时间 = n * (t_request + t_serialize + t_calc_server + t_transfer + t_display)`

![](img/bb412fc545e07a2acf0d9da61331c993_34.png)

![](img/bb412fc545e07a2acf0d9da61331c993_35.png)

对于大型数据集或高分辨率图像，反复传输图像数据会非常缓慢。

![](img/bb412fc545e07a2acf0d9da61331c993_37.png)

![](img/bb412fc545e07a2acf0d9da61331c993_38.png)

## 新策略：将数据与计算移至客户端 ⚡

![](img/bb412fc545e07a2acf0d9da61331c993_40.png)

上一节我们看到了传统方法的瓶颈，本节中我们来看看提出的新解决方案。

![](img/bb412fc545e07a2acf0d9da61331c993_42.png)

我们的核心思路是：**只将原始数据块发送到客户端，并在浏览器中执行图像计算**。这样，后续的交互操作就无需与服务器反复通信。

![](img/bb412fc545e07a2acf0d9da61331c993_44.png)

以下是判断新方法是否更优的简化公式：
`n * t_calc_client + t_transfer_data < n * (t_calc_server + t_transfer_image)`

其中：
*   `n`: 交互次数
*   `t_calc_client`: 在客户端（浏览器）计算图像的时间
*   `t_transfer_data`: 首次传输原始数据的时间
*   `t_calc_server`: 在服务器计算图像的时间
*   `t_transfer_image`: 传输完整图像数据的时间

当交互频繁 (`n` 很大) 或图像很大（导致 `t_transfer_image` 很大）时，新方法的优势尤为明显。

![](img/bb412fc545e07a2acf0d9da61331c993_46.png)

## 技术实现：YT-Py-Canvas 部件 🛠️

![](img/bb412fc545e07a2acf0d9da61331c993_47.png)

![](img/bb412fc545e07a2acf0d9da61331c993_49.png)

![](img/bb412fc545e07a2acf0d9da61331c993_51.png)

![](img/bb412fc545e07a2acf0d9da61331c993_53.png)

![](img/bb412fc545e07a2acf0d9da61331c993_55.png)

上一节我们介绍了新策略的理论基础，本节中我们深入看看其具体实现。

![](img/bb412fc545e07a2acf0d9da61331c993_57.png)

![](img/bb412fc545e07a2acf0d9da61331c993_58.png)

![](img/bb412fc545e07a2acf0d9da61331c993_59.png)

我们构建了一个名为 **YT-Py-Canvas** 的 Jupyter 部件。它的工作流程如下：

![](img/bb412fc545e07a2acf0d9da61331c993_61.png)

1.  **首次渲染**：
    *   从 YT 获取数据（网格中心 `px`, `py`，半宽 `pdx`, `pdy` 和字段值 `value`）。
    *   这些数据通过 Traitlets 同步到部件。
    *   数据被送入一个自定义的 **WebAssembly** 模块中的 `VariableMesh` 数据结构。
    *   根据当前视图，从 `VariableMesh` 中提取数据到 `FixedResolutionBuffer`。
    *   数据数组被传递给另一个 WebAssembly 模块进行**归一化和色彩映射**，生成 RGBA 图像数组。
    *   图像数组最终被渲染到 HTML Canvas 上。

2.  **交互时更新**：
    *   **更改色彩映射**：只需将现有数据数组重新通过色彩映射 WebAssembly 函数处理，无需触动原始网格数据。
    *   **平移/缩放画布**：改变 `FixedResolutionBuffer` 在 `VariableMesh` 中的视图范围，重新提取数据数组，然后生成新的图像。

这种架构确保了只有在必要时才重新计算，极大提升了响应速度。

## 为什么选择 WebAssembly 和 Rust？🚀

![](img/bb412fc545e07a2acf0d9da61331c993_63.png)

![](img/bb412fc545e07a2acf0d9da61331c993_65.png)

上一节我们了解了部件的架构，本节中我们探讨选择这些底层技术的原因。

![](img/bb412fc545e07a2acf0d9da61331c993_67.png)

![](img/bb412fc545e07a2acf0d9da61331c993_69.png)

![](img/bb412fc545e07a2acf0d9da61331c993_71.png)

*   **WebAssembly**：它是一种高性能的二进制指令格式，专为 Web 设计，可以与 JavaScript 高效互操作。它将性能关键的计算放在浏览器沙箱中运行，显著降低了 `t_calc_client`。
*   **Rust**：我们选择 Rust 语言来编写 WebAssembly 模块，因为它内存安全、性能优异，并且对编译到 WebAssembly 有良好的工具链支持（如 `wasm-bindgen` 和 `wasm-pack`）。

以下是一个简单的 Rust 函数示例，它通过 `wasm-bindgen` 暴露给 JavaScript：

![](img/bb412fc545e07a2acf0d9da61331c993_73.png)

```rust
use wasm_bindgen::prelude::*;

![](img/bb412fc545e07a2acf0d9da61331c993_75.png)

![](img/bb412fc545e07a2acf0d9da61331c993_77.png)

![](img/bb412fc545e07a2acf0d9da61331c993_79.png)

#[wasm_bindgen]
pub fn normalize_and_colormap(data: &[f64], min: f64, max: f64, cmap: &str) -> Vec<u8> {
    // ... 实现归一化和色彩映射逻辑 ...
    // 返回 RGBA 字节数组
    vec![]
}
```

![](img/bb412fc545e07a2acf0d9da61331c993_81.png)

![](img/bb412fc545e07a2acf0d9da61331c993_83.png)

使用 `wasm-pack` 可以轻松地将 Rust 项目打包成 npm 包，方便分发和集成。

![](img/bb412fc545e07a2acf0d9da61331c993_85.png)

![](img/bb412fc545e07a2acf0d9da61331c993_87.png)

![](img/bb412fc545e07a2acf0d9da61331c993_89.png)

## 功能演示与潜力 ✨

![](img/bb412fc545e07a2acf0d9da61331c993_91.png)

![](img/bb412fc545e07a2acf0d9da61331c993_93.png)

![](img/bb412fc545e07a2acf0d9da61331c993_94.png)

![](img/bb412fc545e07a2acf0d9da61331c993_96.png)

![](img/bb412fc545e07a2acf0d9da61331c993_98.png)

上一节我们介绍了核心技术，本节中我们通过演示看看它的实际能力。

![](img/bb412fc545e07a2acf0d9da61331c993_100.png)

![](img/bb412fc545e07a2acf0d9da61331c993_101.png)

![](img/bb412fc545e07a2acf0d9da61331c993_103.png)

![](img/bb412fc545e07a2acf0d9da61331c993_105.png)

该部件允许用户：
*   流畅地平移和缩放大型二维数据切片。
*   动态更改色彩映射和显示范围（如对数尺度）。
*   链接多个部件，同步查看同一区域的不同物理量（如密度和温度）。
*   支持 YT 的投影功能，整合多维信息。

![](img/bb412fc545e07a2acf0d9da61331c993_107.png)

这些操作在客户端即时完成，体验远优于传统的服务器端渲染模式。

![](img/bb412fc545e07a2acf0d9da61331c993_109.png)

![](img/bb412fc545e07a2acf0d9da61331c993_111.png)

![](img/bb412fc545e07a2acf0d9da61331c993_113.png)

## 当前限制与未来展望 🔮

![](img/bb412fc545e07a2acf0d9da61331c993_115.png)

![](img/bb412fc545e07a2acf0d9da61331c993_116.png)

![](img/bb412fc545e07a2acf0d9da61331c993_117.png)

上一节我们展示了部件的强大功能，本节中我们也需了解其当前的局限性。

**限制包括**：
1.  目前仅支持二维数据切片，尚不支持完整的立体交互。
2.  传输到浏览器的原始数据大小存在理论上的上限。
3.  功能相比成熟的离线可视化工具仍有限。
4.  需要维护 Python、Rust、JavaScript 多个生态的包，具有一定复杂性。

![](img/bb412fc545e07a2acf0d9da61331c993_119.png)

![](img/bb412fc545e07a2acf0d9da61331c993_120.png)

![](img/bb412fc545e07a2acf0d9da61331c993_122.png)

**未来工作方向**：
1.  扩展至三维数据交互。
2.  集成更多 YT 的分析与可视化功能。
3.  进一步优化部件架构和性能。
4.  提升用户使用的直观性。

![](img/bb412fc545e07a2acf0d9da61331c993_124.png)

## 总结

本节课中我们一起学习了如何利用 Jupyter、Rust 和 WebAssembly 来构建高性能的交互式数据可视化工具。我们分析了传统服务器端渲染方法的瓶颈，提出了将数据与计算移至客户端的优化策略，并深入探讨了 YT-Py-Canvas 部件的实现架构与技术选型。这种方案特别适用于需要频繁交互或处理大型数据集的科学可视化场景，为科学数据的探索与共享提供了新的可能性。

**相关资源**：
*   项目代码与安装：可通过 `pip install yt-py-canvas` 安装。
*   本演讲的幻灯片将在推特上发布。

**致谢**：感谢 Matt Turk、Nathaniel Claren、Nathan Goldbaum、Kacper Kowalik、Britton Smith 以及 Katie Hoe 对本工作的贡献与支持。