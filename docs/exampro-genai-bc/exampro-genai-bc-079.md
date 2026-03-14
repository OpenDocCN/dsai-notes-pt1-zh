# 79：在Lightning AI和Replicate上运行OmniGen 🚀

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_1.png)

在本节课中，我们将学习如何在本地资源不足的情况下，利用云端平台（Lightning AI和Replicate）来运行OmniGen模型。我们将探索环境配置、依赖安装、模型运行以及处理常见错误的完整流程。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_3.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_4.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_6.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_7.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_9.png)

## 概述

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_11.png)

OmniGen是一个强大的生成式AI模型，但其运行对计算资源（尤其是GPU显存）要求较高。当本地计算机资源不足时，我们可以转向云端计算平台。本节将演示如何在Lightning AI Studio中设置环境并尝试运行OmniGen，同时也会介绍使用Replicate这一托管服务的备选方案。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_13.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_15.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_17.png)

## 尝试在Lightning AI上运行

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_19.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_20.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_22.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_23.png)

上一节我们遇到了本地运行OmniGen的显存限制。本节中，我们来看看如何利用云端GPU资源来绕过这个限制。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_25.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_27.png)

我尝试重启电脑并清空显存，但发现关闭Visual Studio Code或某些浏览器能释放被占用的内存。查阅OmniGen的要求后，我发现它可能需要约40GB的显存，这超出了我本地显卡的能力范围。因此，我决定尝试使用Lightning AI平台。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_29.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_31.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_33.png)

### 登录并创建工作室

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_34.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_36.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_37.png)

首先，登录Lightning AI平台。平台界面会显示可用的计算资源。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_39.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_40.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_1.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_42.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_44.png)

我搜索了OmniGen，发现平台上已有用户创建的相关模板，这简化了我们的启动过程。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_46.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_48.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_50.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_51.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_53.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_6.png)
![](img/9af27a1fc0d942b7f7eb6cb20e932c14_7.png)

接下来，我创建了一个新的工作室，将其命名为“OmniGen-Test”，并选择了GPU实例。平台清晰地显示了每种实例的每小时成本。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_55.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_57.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_9.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_59.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_61.png)

我选择了配备1个T4 GPU的最低配置，确认并启动了环境。启动过程需要一些时间。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_63.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_65.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_67.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_69.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_11.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_71.png)

### 配置环境与安装依赖

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_73.png)

环境启动后，我们需要配置正确的PyTorch版本。通过`nvidia-smi`命令，我查看到当前CUDA版本为12.2。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_75.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_77.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_19.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_79.png)

根据OmniGen仓库的`requirements.txt`文件，我们需要安装特定版本的库。以下是关键步骤：

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_81.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_83.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_85.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_87.png)

1.  **克隆仓库**：
    ```bash
    git clone <OmniGen仓库地址>
    ```
2.  **创建Conda环境（可选）**：平台已有默认环境，我们直接在其中操作。检查Python版本为3.10，符合要求。
3.  **安装依赖**：根据`requirements.txt`安装所有包。注意，平台可能已预装部分包。
    ```bash
    pip install -r requirements.txt
    ```
4.  **特别注意Transformers版本**：OmniGen可能需要`transformers==4.45.2`。如果已安装更高版本，需降级：
    ```bash
    pip install transformers==4.45.2
    ```

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_89.png)

### 运行模型与遇到的错误

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_91.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_92.png)

依赖安装完成后，我尝试运行OmniGen提供的示例代码来生成图像。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_94.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_95.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_46.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_97.png)

模型开始下载并加载，但随后遇到了一个错误：`unpacked non-iterable None type`。这与之前在本地遇到的错误相同。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_99.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_53.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_101.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_102.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_104.png)

检查系统资源监控，发现GPU显存使用量约为11GB，并未超出T4显卡的16GB限制，因此问题可能不在资源上。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_106.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_108.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_110.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_101.png)

### 排查与解决尝试

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_112.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_114.png)

根据社区讨论，这个错误可能与库版本冲突有关。以下是尝试的解决方案：

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_116.png)

1.  **确保版本完全匹配**：再次检查并强制安装`requirements.txt`中指定的所有版本，尤其是`transformers`。
2.  **使用`pip install -e .`方式安装**：进入克隆的仓库目录，执行此命令以“可编辑模式”安装，确保依赖解析正确。
    ```bash
    cd omnigen
    pip install -e .
    ```
3.  **重启内核并清理输出**：在Jupyter或VS Code中重启内核，然后再次运行代码。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_118.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_119.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_120.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_122.png)

尽管进行了多次尝试，错误依然存在。这表明在Lightning AI的这个特定环境或配置下运行OmniGen存在兼容性问题。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_124.png)

## 使用Replicate托管服务

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_126.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_128.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_130.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_132.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_133.png)

由于在Lightning AI上自行配置遇到障碍，我们转向更简单的方案——使用Replicate。Replicate提供了预配置的OmniGen模型，只需通过API或Web界面即可调用，无需管理环境。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_135.png)

以下是使用Replicate的核心步骤：

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_137.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_139.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_141.png)

1.  **访问OmniGen模型页面**：在Replicate上找到OmniGen模型。
2.  **配置输入**：上传源图像，并编写修改提示（prompt）。例如：
    *   `Remove the woman‘s earrings.`
    *   `Change the white sweater to a black band T-shirt.`
    *   `Add a nose piercing and neck tattoo.`
    *   `Change the book for a Nintendo Switch.`
3.  **运行并等待**：提交任务后，Replicate会处理请求。首次运行（冷启动）可能较慢，后续运行（热启动）会更快。平台会显示预估时间和成本（本例中约为0.14美元）。
4.  **获取结果**：任务完成后，可以直接下载生成的图像。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_143.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_144.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_146.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_118.png)
*通过Replicate界面提交生成任务*

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_147.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_149.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_150.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_152.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_154.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_184.png)
*生成结果保存至指定目录*

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_156.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_157.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_158.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_160.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_162.png)

## 性能考量与硬件对比

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_164.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_166.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_168.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_170.png)

在等待模型运行的过程中，我们对比了不同GPU的性能，这对于选择计算平台很重要：

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_172.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_174.png)

*   **本地RTX 4060**：拥有8GB显存，性能足够但显存可能成为运行大模型的瓶颈。
*   **云端T4（Lightning AI）**：拥有16GB显存，显存更大但计算性能可能略低于消费级显卡。
*   **云端L40S（Replicate）**：拥有48GB显存，适合需要大显存的模型，但单任务成本较高。
*   **云端H100**：顶级计算卡，性能远超上述显卡，但获取成本和等待时间也最高。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_176.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_178.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_180.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_182.png)

选择平台时，需要在**成本**、**可用性**（等待时间）、**显存大小**和**计算速度**之间做出权衡。对于实验和原型开发，Replicate这类按需付费的托管服务通常更便捷。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_184.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_186.png)

## 总结

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_187.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_189.png)

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_190.png)

本节课中我们一起学习了当本地资源不足时运行OmniGen模型的两种云端方案。

我们首先尝试在**Lightning AI**上自行配置环境，经历了环境创建、依赖安装和版本调试的全过程，虽然最终因特定兼容性错误未能成功，但熟悉了在云端开发环境中操作的工作流。

随后，我们转向了更简单直接的**Replicate**托管服务。通过其Web界面，我们轻松上传图片、输入修改指令并支付少量费用后获得了生成结果，这体现了托管服务“开箱即用”的优势。

![](img/9af27a1fc0d942b7f7eb6cb20e932c14_192.png)

核心收获是：对于复杂的开源模型，**自行部署**能提供最大的灵活性和控制力，但会面临环境配置和调试的挑战；而**使用托管服务**则能极大降低入门门槛，快速验证想法，适合大多数应用场景和初学者。你可以根据项目需求、预算和技术能力来选择最合适的路径。