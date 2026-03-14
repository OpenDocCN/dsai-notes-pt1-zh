#  27：设置数据科学工作环境 💻

![](img/03d110ffd5950bd68a929442d41267dd_0.png)

在本节课中，我们将学习如何为数据科学工作设置你的计算机。你将配置所有必要的工具，以便能够完成后续课程中的所有任务，并准备好接手实际的数据科学项目。

---

上一节我们介绍了课程的整体目标，本节中我们来看看具体需要安装和配置哪些工具。一个专业的数据科学工作环境需要几个核心组件。

以下是设置工作环境的主要步骤：

1.  **安装编程语言**：数据科学的核心编程语言是 Python。我们需要安装 Python 及其包管理工具 pip。
2.  **安装代码编辑器**：一个强大的代码编辑器能极大提升效率。我们将安装 Visual Studio Code。
3.  **安装版本控制工具**：为了管理代码和协作，我们需要安装 Git。
4.  **配置 Python 环境**：为了避免项目间的依赖冲突，我们将学习使用虚拟环境。
5.  **安装核心数据科学库**：最后，我们将安装如 NumPy、Pandas、Matplotlib 等必不可少的 Python 库。

---

首先，我们需要安装 Python。你可以从 Python 官网下载安装程序。安装时，请务必勾选“Add Python to PATH”选项，这样就能在命令行中直接使用 `python` 和 `pip` 命令。

安装完成后，你可以在终端或命令提示符中输入以下命令来验证安装是否成功：
```bash
python --version
pip --version
```

接下来，安装代码编辑器。我们推荐使用 Visual Studio Code，因为它轻量、免费且功能强大，拥有丰富的数据科学相关扩展。从 VS Code 官网下载并安装即可。

然后，安装版本控制系统 Git。Git 能帮助我们跟踪代码变化并与他人协作。从 Git 官网下载安装程序，按照默认设置安装即可。安装后，可以在终端使用 `git --version` 命令验证。

为了管理不同项目的 Python 包依赖，我们需要使用虚拟环境。虚拟环境可以为每个项目创建独立的 Python 运行环境。使用以下命令创建并激活一个虚拟环境：
```bash
# 创建虚拟环境
python -m venv my_data_science_env

# 在 Windows 上激活
my_data_science_env\Scripts\activate
# 在 macOS/Linux 上激活
source my_data_science_env/bin/activate
```
激活后，你的命令行提示符前会出现环境名称，表示你已进入该虚拟环境。

最后，在激活的虚拟环境中，安装数据科学的核心库。我们将使用 `pip` 进行安装。以下是安装关键库的命令：
```bash
pip install numpy pandas matplotlib scikit-learn jupyter
```
这些库分别用于数值计算 (`numpy`)、数据处理 (`pandas`)、数据可视化 (`matplotlib`)、机器学习 (`scikit-learn`) 和交互式编程 (`jupyter`)。

---

![](img/03d110ffd5950bd68a929442d41267dd_2.png)

本节课中我们一起学习了如何从零开始设置一个专业的数据科学工作环境。我们安装了 Python、VS Code 编辑器、Git 版本控制工具，配置了虚拟环境，并安装了核心的数据科学 Python 库。现在，你的计算机已经准备就绪，可以开始迎接布鲁诺交给你的实际客户项目了。