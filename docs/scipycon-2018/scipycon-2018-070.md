# 70：打包的纯粹乐趣 🎁

## 概述

在本课程中，我们将学习如何创建、构建和分发 Python 软件包。我们将从理解什么是软件包开始，逐步深入到如何构建纯 Python 包、如何上传到 PyPI，以及如何处理包含二进制扩展的复杂包。最后，我们还将介绍 Conda 打包系统，这是一个在科学计算领域广泛使用的强大工具。

---

## 第一部分：Python 打包基础

### 什么是软件包？

软件包是可以通过包管理器安装的代码集合。在 Python 世界中，“包”这个词有两种相关但不同的含义：
1.  磁盘上的一组特定文件结构，即一个 Python 包。
2.  一个可以通过 `pip install` 等包管理器管理的发行版。

包管理器通常连接到存储库，如 PyPI 或 anaconda.org，你可以从中获取公开的软件包。包管理器的主要目标是让你能够轻松安装软件，并自动处理其依赖关系。

### 源码包与二进制包

软件包主要有两种形式：
*   **源码包**：包含原始的源代码。
*   **二进制包**：包含已编译好、可以直接在系统上运行的代码。

对于纯 Python 代码，源码包和二进制包的行为基本相同。但是，如果包中包含需要编译的 C 扩展或库，从源码构建可能会非常具有挑战性。这就是二进制包存在的原因——它们可以直接安装并运行，无需用户本地编译。

### 主要的包管理器

*   **pip**：Python 官方的包管理器，从 PyPI 拉取包。它主要处理 Python 包。
*   **Conda**：一个跨平台的包管理器，不仅支持 Python，还支持 C、Fortran、R 等多种语言。它特别适合科学计算领域，因为它能很好地管理二进制依赖。
*   **操作系统包管理器**：如 apt、yum、homebrew 等，它们也可能提供 Python 包，但通常由发行版的维护者负责打包。

---

![](img/de3c33275b0634e99f078bacb2cc0b52_1.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_3.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_5.png)

## 第二部分：创建 Python 包

![](img/de3c33275b0634e99f078bacb2cc0b52_7.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_9.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_11.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_13.png)

上一节我们介绍了包的基本概念，本节我们来看看如何实际创建一个 Python 包。

![](img/de3c33275b0634e99f078bacb2cc0b52_15.png)

### Python 模块与包

*   **模块**：一个单独的命名空间，通常是一个 `.py` 文件。
*   **包**：一个包含 `__init__.py` 文件的目录，它可以包含其他模块和子包。

`__init__.py` 文件的存在使目录被 Python 识别为一个包。当导入包时，会执行该文件中的代码。

### 为什么需要打包？

![](img/de3c33275b0634e99f078bacb2cc0b52_17.png)

将代码打包可以让你：
*   方便地使用 `pip` 等工具进行安装和管理。
*   轻松地将代码分发给他人，无论是通过 PyPI、Conda Forge，还是直接给同事。

![](img/de3c33275b0634e99f078bacb2cc0b52_19.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_21.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_23.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_24.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_26.png)

### 标准包目录结构

![](img/de3c33275b0634e99f078bacb2cc0b52_28.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_30.png)

遵循约定俗成的目录结构非常重要。一个典型的项目结构如下：

```
my_package/
├── LICENSE.txt
├── README.md
├── setup.py          # 构建和描述包的核心文件
├── bin/              # 可执行脚本目录
├── my_package/       # 主包目录
│   ├── __init__.py
│   └── module_a.py
└── tests/            # 测试目录
    └── test_module_a.py
```

![](img/de3c33275b0634e99f078bacb2cc0b52_32.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_34.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_36.png)

### `setup.py` 文件

`setup.py` 是包的心脏，它描述了包的信息和构建方式。它导入了 `setuptools` 模块中的 `setup` 函数。

![](img/de3c33275b0634e99f078bacb2cc0b52_38.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_40.png)

以下是一个简单的 `setup.py` 示例：

```python
from setuptools import setup

setup(
    name='my_package',
    version='0.1.0',
    author='Your Name',
    author_email='your.email@example.com',
    packages=['my_package'],  # 指定要安装的包
    install_requires=[        # 指定依赖
        'numpy>=1.14',
        'requests',
    ],
    scripts=['bin/my_script'], # 指定可执行脚本
)
```

### 开发模式安装

开发模式允许你在不重新安装包的情况下修改源代码，这对于开发非常方便。

使用以下命令进行开发模式安装：
```bash
pip install -e .
```
或者
```bash
python setup.py develop
```

![](img/de3c33275b0634e99f078bacb2cc0b52_42.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_44.png)

这两种方式都会在 `site-packages` 中创建一个链接（`.pth` 文件），指向你的源代码目录，使得 Python 总能找到最新的代码。

---

## 第三部分：构建并上传到 PyPI

![](img/de3c33275b0634e99f078bacb2cc0b52_46.png)

上一节我们学会了如何组织代码并创建 `setup.py`，本节我们将学习如何构建发行版并分享给全世界。

### 关键术语

![](img/de3c33275b0634e99f078bacb2cc0b52_48.png)

*   **PyPI**：Python 包索引，是官方的 Python 包仓库。
*   **pip**：从 PyPI 安装包的工具。
*   **源码发行版**：包含 `setup.py` 和所有源代码的归档文件（如 `.tar.gz`）。
*   **构建发行版/二进制发行版**：包含预构建文件（如编译好的扩展）的发行版，无需本地编译。**Wheel**（`.whl` 文件）是这种发行版的标准格式。
*   **虚拟环境**：用于隔离项目依赖的工具，如 `venv` 或 `conda`。

### 构建发行版

![](img/de3c33275b0634e99f078bacb2cc0b52_50.png)

以下是常见的构建任务和命令：

1.  **构建源码发行版**：
    ```bash
    python setup.py sdist
    ```

2.  **构建 Wheel**：
    ```bash
    pip wheel . -w dist/
    ```

3.  **从源码发行版安装**：
    ```bash
    pip install my_package-0.1.0.tar.gz
    ```

4.  **从 Wheel 安装**：
    ```bash
    pip install my_package-0.1.0-py3-none-any.whl
    ```

### 使用 `twine` 上传到 PyPI

`twine` 是一个专门用于安全上传包到 PyPI 的工具。

1.  首先，在 [test.pypi.org](https://test.pypi.org) 和 [pypi.org](https://pypi.org) 上注册账户。
2.  安装 `twine`：
    ```bash
    pip install twine
    ```
3.  上传到 **测试 PyPI**（推荐先在此测试）：
    ```bash
    twine upload --repository-url https://test.pypi.org/legacy/ dist/*
    ```
4.  上传到 **正式 PyPI**：
    ```bash
    twine upload dist/*
    ```

**重要提示**：为了自动化并避免在日志中泄露密码，可以创建 `~/.pypirc` 文件来存储凭证（确保文件权限安全）。

---

## 第四部分：二进制扩展与依赖管理

在科学计算中，高性能通常需要编译代码（C/C++/Fortran）。本节我们将探讨构建二进制扩展的复杂性以及如何管理其依赖。

### 为什么需要二进制扩展？

*   **性能**：编译代码比解释型 Python 代码快得多。
*   **并行计算**：可以绕过 Python 的全局解释器锁（GIL）。
*   **内存管理**：对内存布局有更精细的控制。
*   **利用现有库**：许多成熟的科学计算库是用这些语言编写的。

### 构建工具链

![](img/de3c33275b0634e99f078bacb2cc0b52_52.png)

构建一个二进制扩展涉及多层工具：

![](img/de3c33275b0634e99f078bacb2cc0b52_54.png)

1.  **编译器**：如 GCC、Clang、MSVC，将源代码转换为机器码。
2.  **链接器**：将多个编译后的对象文件合并成共享库（如 `.so`、`.dll`）。
3.  **构建系统**：如 Make、Ninja、MSBuild，用于组织编译和链接命令。
4.  **系统探测工具**：如 CMake、Autotools，用于探测系统环境并生成构建文件。
5.  **包管理器**：如 pip 或 Conda，负责解析依赖并调用底层工具链。

![](img/de3c33275b0634e99f078bacb2cc0b52_56.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_58.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_60.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_62.png)

### 二进制兼容性挑战

![](img/de3c33275b0634e99f078bacb2cc0b52_64.png)

二进制兼容性关键在于 **C 运行时库** 的兼容性，因为操作系统和 CPython 本身都是用 C 写的。

![](img/de3c33275b0634e99f078bacb2cc0b52_66.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_68.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_69.png)

*   **Linux**：依赖 Glibc 版本。通常使用较旧的 Glibc 构建以确保在更多系统上运行。可以使用 `manylinux` Docker 镜像来构建兼容性广泛的 Wheel。
*   **macOS**：通过设置 `MACOSX_DEPLOYMENT_TARGET` 来指定支持的最低系统版本。
*   **Windows**：兼容性与 Visual Studio 编译器版本紧密相关。不同版本的 Python 可能需要特定版本的 VS 编译器（例如，Python 2.7 需要 VS 2008）。

### 使用 `scikit-build` 简化构建

![](img/de3c33275b0634e99f078bacb2cc0b52_71.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_73.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_75.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_77.png)

`scikit-build` 是一个改进的构建系统生成器，它基于 `setuptools` 并集成 CMake，使得构建 C/C++ 扩展更加简单和可靠。

一个使用 `scikit-build` 的项目，其 `setup.py` 可能如下所示：

```python
from skbuild import setup

setup(
    name="hello",
    version="1.0.0",
    packages=["hello"],
    cmake_args=['-DSOME_FLAG=ON'], # 传递给 CMake 的参数
)
```

然后，你可以像构建纯 Python 包一样使用 `pip` 来构建和安装它。

### Conda 在二进制构建中的角色

Conda 极大地简化了二进制依赖的管理：
*   它提供了跨平台的编译器工具链。
*   可以轻松安装非 Python 的依赖库（如 C 库的头文件和动态库）。
*   通过 `conda-build` 工具，可以创建包含复杂依赖关系的二进制 Conda 包。

![](img/de3c33275b0634e99f078bacb2cc0b52_79.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_80.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_82.png)

---

## 第五部分：Conda 打包深入

上一节我们看到了二进制构建的复杂性，本节我们聚焦于 Conda 这个强大的工具，它让二进制包的创建和分发变得可控。

![](img/de3c33275b0634e99f078bacb2cc0b52_84.png)

### Conda 包与 Wheel 的对比

![](img/de3c33275b0634e99f078bacb2cc0b52_86.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_88.png)

| 特性 | Conda 包 | Wheel |
| :--- | :--- | :--- |
| **运行环境** | 仅限 Conda 环境 | 任何 Python 环境 |
| **适用范围** | 通用（任何语言） | 主要针对 Python |
| **依赖管理** | 强大的外部依赖管理 | 有限，主要处理 Python 依赖 |
| **ABI 兼容性** | 通过通道管理，可灵活定义 | 通过 Wheel 标签系统精确定义 |

![](img/de3c33275b0634e99f078bacb2cc0b52_90.png)

### Conda 构建系统：`conda-build`

`conda-build` 是一个自动化构建 Conda 包的工具。它的核心是一个名为 `meta.yaml` 的配方文件。

![](img/de3c33275b0634e99f078bacb2cc0b52_92.png)

#### 基础 `meta.yaml` 结构

一个最简单的配方只需要名称和版本：

```yaml
package:
  name: my_dummy_package
  version: 1.0
```

#### 关键部分详解

1.  **`source`**：指定源代码位置（URL、Git 仓库、本地路径）。
    ```yaml
    source:
      path: .  # 使用当前目录的源码
    ```

![](img/de3c33275b0634e99f078bacb2cc0b52_94.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_96.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_98.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_100.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_102.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_103.png)

2.  **`build`**：定义构建过程。
    *   `script`：指定构建脚本（Unix 用 `build.sh`，Windows 用 `bld.bat`）。
    *   `number`：构建编号，用于区分同一版本的多次构建。
    ```yaml
    build:
      script: python -m pip install . --no-deps
    ```

![](img/de3c33275b0634e99f078bacb2cc0b52_105.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_107.png)

3.  **`requirements`**：定义依赖关系，分为三个环境：
    *   `build`：构建时需要的**工具**（如编译器、CMake）。
    *   `host`：构建时需要的**库和解释器**（如 Python、依赖库的头文件）。构建的输出会放在这里。
    *   `run`：运行时需要的依赖。
    ```yaml
    requirements:
      build:
        - {{ compiler('c') }}  # 使用 Conda 提供的编译器抽象
      host:
        - python
        - numpy
      run:
        - python
        - numpy
    ```

![](img/de3c33275b0634e99f078bacb2cc0b52_109.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_111.png)

4.  **`test`**：定义自动化测试。
    ```yaml
    test:
      imports:
        - my_package
      commands:
        - python -c "import my_package; print('OK')"
    ```

![](img/de3c33275b0634e99f078bacb2cc0b52_113.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_115.png)

#### 高级特性

*   **选择器**：允许根据平台或其它条件执行不同的操作。
    ```yaml
    build:
      script: build.sh  # [unix]
      script: bld.bat  # [win]
    ```
*   **Jinja2 模板**：在配方中使用变量和逻辑。
    ```yaml
    {% set version = "1.2.3" %}
    package:
      name: my_pkg
      version: {{ version }}
    ```
*   **变体（Variants）**：通过 `conda_build_config.yaml` 文件定义构建矩阵（如针对不同 Python 版本构建）。
*   **运行导出（Run Exports）**：允许一个包声明其依赖的兼容性范围，并自动传递给依赖它的下游包。这是管理二进制兼容性的强大机制。

![](img/de3c33275b0634e99f078bacb2cc0b52_117.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_119.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_120.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_121.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_123.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_125.png)

### 分享你的 Conda 包

构建成功后，你可以将包上传到：
*   **你自己的 Anaconda.org 频道**：用于个人或团队分享。
*   **Conda-Forge**：一个社区驱动的 Conda 包仓库。你可以通过提交“ feedstock”（包含配方的独立仓库）来为社区贡献包。

![](img/de3c33275b0634e99f078bacb2cc0b52_127.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_129.png)

---

![](img/de3c33275b0634e99f078bacb2cc0b52_131.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_133.png)

## 总结

在本课程中，我们一起学习了 Python 打包的完整旅程：

![](img/de3c33275b0634e99f078bacb2cc0b52_135.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_137.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_139.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_141.png)

1.  **基础**：理解了模块、包、源码发行版和二进制发行版的概念。
2.  **创建**：学会了如何组织项目结构，编写 `setup.py`，并使用开发模式。
3.  **分发**：掌握了使用 `twine` 构建 Wheel 和源码发行版，并将它们上传到 PyPI 的流程。
4.  **二进制**：深入探讨了构建 C/C++ 扩展的复杂性，包括工具链、二进制兼容性挑战，以及如何使用 `scikit-build` 和 `conda` 来简化这一过程。
5.  **Conda**：全面了解了 `conda-build` 系统，如何编写 `meta.yaml` 配方，以及 Conda 在管理复杂、跨语言依赖方面的强大能力。

![](img/de3c33275b0634e99f078bacb2cc0b52_143.png)

![](img/de3c33275b0634e99f078bacb2cc0b52_145.png)

打包起初可能令人望而生畏，但一旦掌握了这些工具和概念，它就会变成一种强大的技能，让你能可靠地构建、分享和部署代码。祝大家打包愉快！