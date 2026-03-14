# 017：CUDA中的支持功能

![](img/f7ca11280ba862f9bab5cb100f28be19_1.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_3.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_5.png)

在本节课中，我们将要学习CUDA编程中的一些支持功能，包括如何使用Makefile自动化编译和运行流程，如何使用CUDA-GDB调试程序，以及如何使用性能分析工具（如nvprof）来评估和优化CUDA程序的性能。

---

## Makefile基础

上一节我们介绍了CUDA中的函数类型。本节中，我们来看看如何利用Makefile来管理CUDA项目的编译和运行。

![](img/f7ca11280ba862f9bab5cb100f28be19_7.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_9.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_11.png)

Makefile是一组包含目标和变量的命令集合，用于自动化编译、链接和清理项目文件。它特别适用于管理大型项目中文件之间的依赖关系。

以下是Makefile的基本结构：

```
target: prerequisites
    command
```

![](img/f7ca11280ba862f9bab5cb100f28be19_13.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_15.png)

*   **target**：可以是生成的文件名，也可以是要执行的操作名称。
*   **prerequisites**：是执行该目标所依赖的文件或其他目标。
*   **command**：是生成目标或执行操作所需的一系列shell命令。**注意：命令前必须使用Tab字符缩进，而不是空格。**

![](img/f7ca11280ba862f9bab5cb100f28be19_17.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_19.png)

### 一个简单的Makefile示例

以下是一个打印“Hello”的简单Makefile规则：

```
hello:
    echo Hello
```

当在终端运行 `make hello` 命令时，它会执行 `echo Hello`，输出“Hello”。

![](img/f7ca11280ba862f9bab5cb100f28be19_21.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_23.png)

### 用于CUDA程序的Makefile示例

![](img/f7ca11280ba862f9bab5cb100f28be19_25.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_27.png)

让我们为一个打印“Hello”32次的简单CUDA程序编写Makefile。我们希望它能自动完成编译、运行和清理。

```
hello:
    nvcc hello.cu -o hello

![](img/f7ca11280ba862f9bab5cb100f28be19_29.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_31.png)

run: hello
    ./hello

clean: run
    rm hello
```

这个Makefile包含三个规则：
1.  `hello`：编译 `hello.cu` 文件，生成可执行文件 `hello`。
2.  `run`：依赖于 `hello` 目标。在 `hello` 编译完成后，运行 `./hello`。
3.  `clean`：依赖于 `run` 目标。在程序运行后，删除生成的可执行文件 `hello`。

执行 `make clean` 将按顺序触发这三个目标。

![](img/f7ca11280ba862f9bab5cb100f28be19_33.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_35.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_37.png)

### 在Makefile中使用变量

![](img/f7ca11280ba862f9bab5cb100f28be19_39.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_41.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_43.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_45.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_47.png)

我们可以使用变量来使Makefile更清晰、更易于维护。

![](img/f7ca11280ba862f9bab5cb100f28be19_49.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_51.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_53.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_55.png)

```
FILE1 = hello
FILE2 = run
FILE3 = clean

![](img/f7ca11280ba862f9bab5cb100f28be19_57.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_59.png)

$(FILE1):
    nvcc hello.cu -o hello

$(FILE2): $(FILE1)
    ./hello

$(FILE3): $(FILE2)
    rm hello
```

变量通过 `=` 赋值，并通过 `$(变量名)` 引用。

### 使用 `all` 目标管理多个任务

![](img/f7ca11280ba862f9bab5cb100f28be19_61.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_63.png)

`all` 目标常用于指定默认要构建的一系列目标。

```
all: hello1 hello2 hello3

hello1:
    nvcc hello1.cu -o hello1
    ./hello1

![](img/f7ca11280ba862f9bab5cb100f28be19_65.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_67.png)

hello2:
    nvcc hello2.cu -o hello2
    ./hello2

![](img/f7ca11280ba862f9bab5cb100f28be19_69.png)

hello3:
    nvcc hello3.cu -o hello3
    ./hello3

![](img/f7ca11280ba862f9bab5cb100f28be19_71.png)

clean:
    rm hello1 hello2 hello3
```

运行 `make all` 将按顺序编译并运行 `hello1`、`hello2` 和 `hello3`，最后可以运行 `make clean` 进行清理。

![](img/f7ca11280ba862f9bab5cb100f28be19_73.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_75.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_77.png)

### Makefile中的条件与循环

![](img/f7ca11280ba862f9bab5cb100f28be19_79.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_80.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_82.png)

Makefile支持条件判断和循环，以增加灵活性。

**条件判断示例**：
```
TARGET_CPU = x86_64

ifeq ($(TARGET_CPU), x86_64)
    ARCH_FLAG = -m64
else
    ARCH_FLAG = -m32
endif
```

**循环示例**：
```
run_tests:
    for number in 1 2 3 4 5; do \
        ./my_program input$$number.txt output$$number.txt; \
    done
```

### 综合示例：自动化测试多个图数据

假设我们有一个CUDA程序 `vertex_removal.cu`，需要对 `test_bench.zip` 中的多个图文件（`t1.mtx` 到 `t10.mtx`）进行测试。

```
all: vertex_removal
    unzip -o examples/test_cases/test_bench.zip -d .
    number=1; \
    while [ $$number -le 10 ]; do \
        if [ $$number -ne 5 ]; then \
            ./vertex_removal t$$number.mtx vr_graph_$$number.txt; \
        fi; \
        number=$$((number + 1)); \
    done

vertex_removal:
    nvcc vertex_removal.cu -o vertex_removal

![](img/f7ca11280ba862f9bab5cb100f28be19_84.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_86.png)

clean:
    rm -f vertex_removal *.txt
```

这个Makefile自动化了以下流程：
1.  编译 `vertex_removal.cu`。
2.  解压测试数据。
3.  使用循环对除 `t5.mtx` 外的所有测试图运行程序，并将结果输出到对应文件。
4.  `clean` 目标用于清理生成的可执行文件和输出文件。

---

## 使用CUDA-GDB调试

调试并行程序比调试串行程序更具挑战性，因为线程执行顺序的不确定性可能导致不同的结果。CUDA-GDB是GNU调试器(GDB)的扩展，专门用于调试运行在GPU硬件上的CUDA程序。

![](img/f7ca11280ba862f9bab5cb100f28be19_88.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_90.png)

### 调试示例：空指针解引用

考虑以下存在错误的CUDA内核：
```cpp
__global__ void myKernel(int *x) {
    *x = 0; // 潜在的空指针解引用
}
int main() {
    int *d_x; // 仅在主机端声明指针，未在设备上分配内存
    myKernel<<<2, 10>>>(d_x);
    cudaDeviceSynchronize();
    return 0;
}
```
此代码中，设备指针 `d_x` 未被分配设备内存，导致内核中尝试解引用非法地址。

### 使用 `cudaGetLastError` 捕获错误

一种调试方法是使用 `cudaGetLastError` 函数。
```cpp
cudaError_t err = cudaGetLastError();
if (err != cudaSuccess) {
    printf("CUDA error: %s\\n", cudaGetErrorString(err));
}
```
运行有问题的程序后，此代码会输出类似 `CUDA error: an illegal memory access was encountered` 的信息，提示存在非法内存访问。

### 使用CUDA-GDB进行交互式调试

更强大的方法是使用CUDA-GDB。
1.  **编译调试版本**：使用 `-G` 和 `-g` 标志编译CUDA代码，生成调试信息。
    ```bash
    nvcc -G -g my_program.cu -o my_program_debug
    ```
2.  **启动调试**：
    ```bash
    cuda-gdb my_program_debug
    ```
3.  **设置断点并运行**：在 `(cuda-gdb)` 提示符下：
    ```
    (cuda-gdb) break main
    (cuda-gdb) run
    ```
4.  **分析错误**：程序崩溃后，CUDA-GDB会显示错误位置（如 `my_program.cu:6`）、引发错误的线程信息（网格、块、线程ID）以及指针的值（可能为 `0x0`），从而帮助定位空指针解引用问题。

### 常用的CUDA-GDB命令

*   `info cuda kernels`：列出当前所有CUDA内核的信息。
*   `info cuda threads`：显示当前内核中线程的状态。
*   `cuda thread block thread`：切换调试焦点到指定的线程块和线程。
*   `info cuda lanes`：显示特定线程（通道）的详细信息。
*   `break filename.cu:23 if threadIdx.x == 1`：在特定文件和行号设置条件断点。

---

## 性能分析：nvprof

性能分析是优化CUDA程序的关键步骤。`nvprof` 是NVIDIA提供的命令行性能分析工具，属于非侵入式分析，无需修改源代码。

![](img/f7ca11280ba862f9bab5cb100f28be19_92.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_94.png)

### 使用nvprof进行性能分析

假设我们有一个程序调用了三个内核（`kernel1`、`kernel2`、`kernel3`）各100次。
1.  **直接分析**：
    ```bash
    nvprof ./my_cuda_program
    ```
    `nvprof` 会输出每个内核的执行时间、调用次数以及占总时间的百分比。
    ```
    ==PROF== Profiling result:
    Time(%)     Time   Calls      Kernel
      39.2%   1.23ms     100     kernel2
      33.1%   1.04ms     100     kernel3
      26.5%   0.83ms     100     kernel1
      95.0%   --        300     CUDA API (cudaLaunchKernel)
    ```
    从结果可知，`kernel2` 耗时最多，是首要的优化目标。同时，CUDA内核启动开销也占据了显著时间。

2.  **分析特定代码区域**：可以使用 `nvprof` 的API在代码中标记分析范围。
    ```cpp
    #include <nvToolsExt.h>
    nvtxRangePushA("MyCodeSection");
    // ... 要分析的代码 ...
    nvtxRangePop();
    ```
    然后运行：
    ```bash
    nvprof --analysis-metrics -o profile.nvvp ./my_cuda_program
    ```
    生成的 `profile.nvvp` 文件可以用更可视化的工具（如NVIDIA Nsight Systems）进行深入分析。

`nvprof` 可以提供丰富的指标，包括内存吞吐量、缓存命中率、分支分化等，帮助开发者全面了解程序在GPU上的行为。

---

![](img/f7ca11280ba862f9bab5cb100f28be19_96.png)

![](img/f7ca11280ba862f9bab5cb100f28be19_98.png)

本节课中我们一起学习了CUDA编程中的三项重要支持功能：使用Makefile自动化项目构建流程，使用CUDA-GDB调试GPU内核代码，以及使用nvprof工具进行程序性能分析。掌握这些工具能显著提升CUDA程序的开发效率和运行性能。