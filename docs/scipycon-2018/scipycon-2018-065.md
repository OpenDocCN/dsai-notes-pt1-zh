# 65：如何建立你自己的开源项目 🚀

在本课程中，我们将学习如何从头开始建立一个完整的开源项目。我们将涵盖从创建代码仓库、编写测试、配置持续集成，到生成文档的整个流程。通过一个名为“HUGS”的示例项目，我们将一步步实践这些关键步骤。

![](img/ae9b964cad0e2ad658805ae80041e533_1.png)

---

## 1. 项目初始化与许可证选择 📄

上一节我们介绍了课程目标，本节中我们来看看如何开始一个开源项目。

创建开源项目的第一步是选择一个合适的软件许可证。许可证规定了他人使用你代码的条款。如果没有许可证，从法律上讲，你仍然保留代码的所有权利，他人不应使用你的代码。

许可证主要分为两大类：
*   **Copyleft 许可证**：例如 GPL、LGPL。这类许可证要求任何衍生代码也必须遵循相同的许可证条款。
*   **宽松许可证**：例如 MIT、BSD、Apache。这类许可证允许他人几乎自由地使用你的代码，通常只需保留原许可证声明。

在科学 Python 社区，通常推荐使用宽松许可证，如 BSD 三条款许可证。

以下是创建 GitHub 仓库的步骤：
1.  登录 GitHub，点击 “New repository”。
2.  填写仓库名称（如 `hugs-yourname`）和描述。
3.  选择公开（Public）。
4.  勾选 “Initialize this repository with a README”。
5.  为 `.gitignore` 模板选择 “Python”。
6.  在许可证下拉菜单中选择 “BSD 3-Clause License”。
7.  点击 “Create repository”。

现在，你的项目已经有了一个包含 README、`.gitignore` 和 `LICENSE` 文件的家。

---

## 2. 获取代码与本地设置 💻

![](img/ae9b964cad0e2ad658805ae80041e533_3.png)

上一节我们创建了在线仓库，本节中我们来看看如何将其克隆到本地并设置开发环境。

首先，将远程仓库克隆到本地计算机。在终端中，使用 `git clone` 命令并粘贴你的仓库 URL。

![](img/ae9b964cad0e2ad658805ae80041e533_5.png)

接下来，我们需要设置 Python 开发环境并获取示例代码。我们使用一个名为 HUGS 的示例项目。

![](img/ae9b964cad0e2ad658805ae80041e533_7.png)

以下是具体步骤：
1.  从 `github.com/jrleeman/settingupopensource` 下载或克隆示例代码。
2.  将下载的 `hugs` 文件夹和 `setup.py` 文件复制到你的本地仓库目录中。
3.  激活之前用 Conda 创建的环境：`conda activate suos`（或 `source activate suos`）。
4.  在仓库根目录下，以可编辑模式安装 HUGS 库：`pip install -e .`

![](img/ae9b964cad0e2ad658805ae80041e533_9.png)

安装成功后，将新添加的代码提交到本地 Git 仓库并推送到 GitHub：
*   `git add -A`
*   `git commit -m “initial commit”`
*   `git push`

![](img/ae9b964cad0e2ad658805ae80041e533_11.png)

---

## 3. 为项目编写测试 ✅

上一节我们设置了本地环境，本节中我们来看看如何为代码编写测试。

测试对于确保代码质量至关重要。它能验证功能正确性、防止回归错误，并提升代码的可维护性。我们将使用 `pytest` 框架。

`pytest` 会自动查找并运行文件名以 `test_` 开头、函数名以 `test_` 开头的测试。

我们的示例项目 HUGS 已经包含了一些测试。运行 `pytest` 命令可以执行现有测试套件。

现在，我们来为 `get_wind_components` 函数添加新的测试。这个函数用于将风速和风向分解为 U（东西）和 V（南北）分量。

以下是需要编写的测试：
*   **测试标量输入**：验证函数处理单个数值（标量）时工作正常。
*   **测试数组输入**：验证函数处理 NumPy 数组时工作正常。

在编写数组测试时，你可能会发现原函数中的 `if wind_dir > 360:` 语句对数组不友好，需要将其改为 `if np.any(wind_dir > 360):`。

除了基本的功能测试，我们还需要测试警告和异常是否被正确触发。

*   **测试警告**：使用 `pytest.warns` 上下文管理器来测试当风向大于 360 度时是否发出警告。
    ```python
    def test_warning_direction():
        """测试风向大于360度时是否发出警告。"""
        with pytest.warns(UserWarning):
            get_wind_components(10, 480)
    ```
*   **测试异常**：使用 `pytest.raises` 上下文管理器来测试当输入无效参数时是否抛出特定异常（如 `ValueError`）。

最后，我们可以使用 `pytest --cov=hugs` 命令来检查代码的测试覆盖率，了解有多少代码被测试执行到。

---

![](img/ae9b964cad0e2ad658805ae80041e533_13.png)

## 4. 配置持续集成 (CI) 与 Travis CI 🔄

![](img/ae9b964cad0e2ad658805ae80041e533_15.png)

![](img/ae9b964cad0e2ad658805ae80041e533_17.png)

![](img/ae9b964cad0e2ad658805ae80041e533_19.png)

上一节我们编写了本地测试，本节中我们来看看如何利用持续集成服务自动运行测试。

持续集成（CI）的核心是每次代码提交时自动运行测试，确保主分支始终处于可工作状态。这对于处理拉取请求（Pull Request）尤其有用，可以自动验证贡献者的代码是否引入错误。

![](img/ae9b964cad0e2ad658805ae80041e533_21.png)

我们将使用 Travis CI 服务。它为开源项目提供免费的 CI 服务。

以下是设置 Travis CI 的步骤：
1.  访问 `travis-ci.com`，使用 GitHub 账号登录并授权。
2.  在 Travis CI 的设置中，选择 “Only select repositories”，并勾选你今天创建的 HUGS 仓库。
3.  在项目根目录创建 `.travis.yml` 配置文件。

一个基本的 `.travis.yml` 配置如下：
```yaml
language: python
sudo: false
branches:
  only:
    - master
python:
  - “3.6”
install:
  - pip install .
script:
  - pytest
```
这个配置告诉 Travis CI：我们是一个 Python 项目，在 Python 3.6 环境下，通过 `pip install .` 安装项目，然后运行 `pytest`。

![](img/ae9b964cad0e2ad658805ae80041e533_23.png)

![](img/ae9b964cad0e2ad658805ae80041e533_25.png)

创建配置文件后，将其提交并推送到一个新的分支（例如 `add-travis`），然后针对该分支创建一个拉取请求。Travis CI 会自动开始构建并运行测试。你可以在 GitHub 的拉取请求页面上看到构建状态（黄色表示进行中，绿色表示成功，红色表示失败）。

如果构建失败，可以点击 “Details” 查看详细的日志来排查问题。例如，可能需要在 `setup.py` 的 `packages` 列表中明确列出所有子包（如 `hugs.calc`, `hugs.plots`）。

你可以轻松扩展配置，让 Travis CI 在多个 Python 版本（如 2.7, 3.5, 3.6）上并行测试。

![](img/ae9b964cad0e2ad658805ae80041e533_27.png)

---

![](img/ae9b964cad0e2ad658805ae80041e533_29.png)

![](img/ae9b964cad0e2ad658805ae80041e533_31.png)

![](img/ae9b964cad0e2ad658805ae80041e533_33.png)

![](img/ae9b964cad0e2ad658805ae80041e533_35.png)

![](img/ae9b964cad0e2ad658805ae80041e533_37.png)

## 5. 代码风格检查与 Flake8 ✨

![](img/ae9b964cad0e2ad658805ae80041e533_39.png)

![](img/ae9b964cad0e2ad658805ae80041e533_41.png)

![](img/ae9b964cad0e2ad658805ae80041e533_43.png)

上一节我们配置了自动化测试，本节中我们来看看如何保持代码风格的一致性。

当项目有多个贡献者时，保持统一的代码风格非常重要。`flake8` 是一个工具，可以自动检查代码是否符合 PEP 8 等风格规范。

我们的环境已经安装了 `pytest-flake8` 插件，因此可以直接通过 `pytest --flake8` 运行检查。

初始运行会报告很多风格问题，例如行过长、导入语句间缺少空行、逗号后缺少空格等。

我们可以通过创建 `setup.cfg` 文件来配置 `flake8`，忽略某些规则或调整参数：
```ini
[flake8]
max-line-length = 95
ignore = F403
```
这会将最大行长度放宽到 95 字符，并忽略关于星号导入（`F403`）的警告。

![](img/ae9b964cad0e2ad658805ae80041e533_45.png)

修复一些明显的风格问题（如在导入间添加空行、在逗号后添加空格）后，我们需要将 `flake8` 检查也集成到 Travis CI 中。

更新 `.travis.yml` 文件：
1.  在 `install` 部分添加 `flake8` 及其插件：`pip install flake8 flake8-docstrings flake8-import-order`
2.  在 `script` 部分添加 `pytest --flake8` 命令。

这样，每次代码提交或拉取请求都会自动进行风格检查，确保代码库的整洁和一致。

![](img/ae9b964cad0e2ad658805ae80041e533_47.png)

---

## 6. 使用 Sphinx 生成项目文档 📖

上一节我们确保了代码风格，本节中我们来看看如何为项目创建专业的文档。

![](img/ae9b964cad0e2ad658805ae80041e533_49.png)

好的文档对于开源项目至关重要。Sphinx 是 Python 社区主流的文档生成工具，它不仅能处理普通的文本文档，还能自动从代码的文档字符串（docstrings）生成 API 文档。

首先，在项目根目录创建 `docs` 文件夹并初始化 Sphinx：
```bash
mkdir docs
cd docs
sphinx-quickstart
```
在交互式问答中，你可以设置项目名、作者、语言等。接受大部分默认选项即可，但务必对 “autodoc: automatically insert docstrings from modules” 选择 “yes”。

初始化后，主要生成以下文件：
*   `source/conf.py`: Sphinx 的主配置文件。
*   `source/index.rst`: 文档的主页。
*   `Makefile`: 用于构建文档。

运行 `make html` 即可在 `build/html/` 目录下生成 HTML 格式的文档。你可以编辑 `index.rst` 文件来添加内容。reStructuredText 使用特定符号表示标题，例如：
```
我的标题
=========
```
要添加新页面（如 `contributing.rst`），需要在 `index.rst` 的 `toctree`（目录树）指令中引用它：
```
.. toctree::
   :maxdepth: 2

   contributing
```
Sphinx 最强大的功能是自动生成 API 文档。创建一个 `api.rst` 文件，并使用 `autofunction` 指令：
```
API 参考
========

![](img/ae9b964cad0e2ad658805ae80041e533_51.png)

.. autofunction:: hugs.calc.get_wind_speed
```
为了让 Sphinx 更好地解析 NumPy 风格的文档字符串，需要在 `conf.py` 文件的 `extensions` 列表中添加 `’sphinx.ext.napoleon’`。

![](img/ae9b964cad0e2ad658805ae80041e533_53.png)

![](img/ae9b964cad0e2ad658805ae80041e533_55.png)

最后，你也可以将文档构建添加到 Travis CI 的检查中，确保文档能够成功生成，没有语法错误。在 `.travis.yml` 的 `script` 部分添加：
```yaml
- cd docs && make html
```

![](img/ae9b964cad0e2ad658805ae80041e533_57.png)

![](img/ae9b964cad0e2ad658805ae80041e533_59.png)

![](img/ae9b964cad0e2ad658805ae80041e533_61.png)

---

![](img/ae9b964cad0e2ad658805ae80041e533_63.png)

![](img/ae9b964cad0e2ad658805ae80041e533_65.png)

![](img/ae9b964cad0e2ad658805ae80041e533_67.png)

## 课程总结 🎉

在本节课中，我们一起学习了建立一个完整开源项目的核心步骤。

![](img/ae9b964cad0e2ad658805ae80041e533_69.png)

我们从**选择许可证和创建 GitHub 仓库**开始，为项目奠定了法律和协作基础。接着，我们学习了如何**设置本地开发环境**并获取代码。

![](img/ae9b964cad0e2ad658805ae80041e533_71.png)

然后，我们深入探讨了**编写自动化测试**的重要性，并使用 `pytest` 为示例函数添加了单元测试、警告测试和异常测试。为了提升效率和质量，我们配置了 **Travis CI 持续集成服务**，实现了代码提交和拉取请求的自动测试。

![](img/ae9b964cad0e2ad658805ae80041e533_73.png)

![](img/ae9b964cad0e2ad658805ae80041e533_75.png)

此外，我们使用 **`flake8` 工具来强制执行代码风格规范**，确保代码库的整洁一致。最后，我们介绍了如何使用 **Sphinx 生成专业文档**，包括手动编写内容和自动从文档字符串生成 API 参考。

![](img/ae9b964cad0e2ad658805ae80041e533_77.png)

![](img/ae9b964cad0e2ad658805ae80041e533_79.png)

通过这一系列步骤，你的开源项目已经具备了良好的基础设施，能够更高效地开发、协作和维护。记住，这些工具和实践是动态的，随着项目成长，你可以探索更多高级功能和服务来不断完善你的项目。