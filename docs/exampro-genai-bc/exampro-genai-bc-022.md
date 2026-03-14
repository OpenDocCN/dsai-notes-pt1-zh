# 22：使用Windsurf实现后端API

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_1.png)

## 概述
在本节课中，我们将学习如何利用AI工具，根据一份详细的技术规格说明书，自动生成一个完整的Go语言后端API项目。我们将看到从零开始构建项目结构、编写代码、处理依赖到最终运行服务器的全过程。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_3.png)

---

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_5.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_7.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_9.png)

## 从技术规格到代码生成

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_11.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_13.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_15.png)

上一节我们完成了后端技术规格说明书的撰写。本节中，我们来看看如何利用这份文档来生成实际的Go语言后端代码。

我们拥有完整且详细的后端技术规格说明书。现在，我们希望基于这份规格书来编写我们的Go后端代码。需要说明的是，我们也可以用它来生成任何类型的后端代码，只需将“Go”这个词替换掉即可。技术规格中可能有一部分专门提到了任务运行器（例如Rake任务），但我们可以将其概括为“任务”。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_17.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_19.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_20.png)

为了更灵活地提取信息，我们可以在文档顶部注明“使用Go构建。Mage是任务运行器。”这样，如果我们想为项目获取更精确的要求，只需修改技术规格即可。这个过程应该相当直接。

现在，我们准备开始生成代码。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_22.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_24.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_26.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_28.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_29.png)

## 制定实施计划

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_31.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_33.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_35.png)

我已经定义了一个技术规格文件。我需要为我们的语言学习门户应用构建一个API后端。所有必需的技术规格都在“后端技术规格”文件中。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_37.png)

我们可以要求AI在开始执行前，先告诉我们它计划做什么，即提供一个实施计划的摘要。在我们同意该计划后，再让它执行命令来创建项目。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_39.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_41.png)

以下是AI根据技术规格制定的构建API后端计划摘要：
*   创建一个新的Go应用。
*   设置Gin框架。
*   创建名为`words`的数据库。
*   实现六个数据表。
*   实现RESTful API端点。
*   项目结构将包含`server`、`internal`、`migrations`、`Magefile.go`、`go.mod`等。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_43.png)

我注意到计划中缺少了`seeds`（种子数据）文件夹。因此，我要求AI将其与`migrations`目录一起分组到一个名为`db`的文件夹中。AI接受了这个修改。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_45.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_47.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_49.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_51.png)

## 调整项目结构

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_53.png)

对于`internal`目录的结构，我有些疑问。这不是Gin特有的，而是Go语言的一个特殊约定目录，用于防止内部包被外部导入。不过，鉴于我们正在用Gin构建Web应用，可以使其更符合Web开发模式。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_55.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_57.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_59.png)

AI建议的结构包含`main`入口点、可共享的`pkg`、以及`app`主应用代码。但这看起来过于复杂。我要求简化。

简化后的结构包含`models`、`handlers`、`database`和`services`。`handlers`用于处理HTTP请求，`database`包含连接设置和配置，`services`是业务逻辑层。这个结构更清晰。

## 开始生成代码

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_61.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_63.png)

现在，我们要求AI在`backend-go`目录中开始构建应用。AI开始执行命令并创建文件。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_65.png)

在生成过程中，我们发现当前环境是Windows PowerShell，而我们需要在WSL（Windows Subsystem for Linux）环境中运行Go项目。因此，我们需要将项目切换到WSL文件系统。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_67.png)

经过一些路径操作，我们成功在WSL中重新打开了项目文件夹。现在，我们回到Windsurf，要求AI重新读取技术规格文件并制定计划，然后在`backend-go`文件夹中实施构建。这次，环境问题应该得到解决。

AI检测到系统未安装Go，并建议安装。我们同意安装。安装完成后，AI继续生成代码。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_69.png)

## 审查生成的代码

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_71.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_72.png)

代码生成完成后，我们来审查一下生成的项目文件。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_74.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_76.png)

*   `go.mod`： 定义了项目模块和依赖，包括Gin、Gorm、SQLite驱动等。值得注意的是，它包含了一个来自字节跳动（Bytedance）的序列化库`sonic`。
*   `Magefile.go`： 任务运行器文件，定义了如`Install`（安装依赖）、`Run`（运行服务器）、`Build`（构建服务器）等任务。
*   `migrations/`： 包含了创建数据库表的迁移文件。检查文件内容，确认了`words`、`groups`、`study_sessions`、`study_activities`、`word_review_items`等表的定义和关联关系是正确的。
*   `main.go`： 主应用入口点。它使用Gin和Gorm，设置了CORS（目前允许所有来源，后期可能需要收紧），并定义了API路由。但看起来并非所有在规格书中列出的端点都已实现。
*   `internal/models/`： 包含了映射数据库表结构的Go结构体。
*   `internal/handlers/`： 目前是空的，HTTP请求处理器尚未生成。
*   `internal/services/`： 目前是空的，业务逻辑层尚未生成。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_78.png)

AI表示它完成了初始设置，但`handlers`和`services`层的实现，以及数据库逻辑和初始化代码是接下来的步骤。

## 继续实现与填充代码

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_80.png)

我们要求AI继续工作，实现剩余的处理器、服务层和数据库逻辑。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_82.png)

AI开始创建`handlers`目录下的文件，如`dashboard.go`、`study.go`、`words.go`。这些处理器文件主要调用对应的服务层函数。真正的业务逻辑和数据库操作将位于服务层。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_84.png)

我们让AI继续完成所有工作。接下来，AI开始实现数据库迁移的运行时管理和添加种子数据。它创建了管理迁移表的代码和一些基础种子数据。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_86.png)

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_88.png)

根据技术规格，我们要求AI不要在迁移文件中加载数据，种子数据应放在单独的种子文件中。AI据此进行了修正。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_90.png)

## 检查进度与查漏补缺

我们检查`main.go`，发现并非所有API端点都已注册。我们要求AI根据技术规格文档，继续实现所有剩余的API端点。

AI完成了剩余端点的实现，包括单词组管理、学习会话管理等，并更新了`main.go`中的路由。

## 运行与测试服务器

AI尝试运行服务器。服务器成功启动，并在端口8080上监听。AI还尝试使用`curl`命令测试API端点。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_92.png)

测试中遇到了问题：数据库表不存在（`no such table: words`），因为迁移和种子数据没有在服务器启动前运行。此外，还有一些关于未使用导入包的编译警告。

![](img/c8b3481ea5537c0d83ea9fbfe8a75399_94.png)

AI开始修复这些问题：添加缺失的依赖、移除未使用的导入、并完善`Magefile.go`中的任务来顺序执行`reset`（重置）、`initDb`（初始化数据库）和`seed`（填充种子数据）操作。

在解决了端口冲突（原8080端口已被占用，改为8081）后，服务器最终成功启动并运行，没有报错。

## 总结

本节课中，我们一起学习了如何利用AI工具，将一份详尽的技术规格说明书转化为一个可运行的Go语言后端API项目。我们经历了以下关键步骤：
1.  **制定计划**： 要求AI先提供实施摘要，确保理解正确。
2.  **生成代码**： AI根据规格自动创建项目结构、模型、处理器、路由等核心代码。
3.  **环境配置**： 处理了WSL环境问题并安装了必要的Go环境。
4.  **迭代完善**： 通过多次交互，指导AI调整结构、补全端点、修复编译错误和运行时问题。
5.  **运行验证**： 最终成功启动了后端服务器。

目前，我们获得了一个理论上功能完整的API后端。在下一节课中，我们将重点为这个后端编写测试代码，以验证所有API端点是否按预期工作，确保项目的可靠性。