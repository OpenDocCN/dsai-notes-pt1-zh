# 20：面向数据科学家的Cython教程 🐍

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_1.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_3.png)

在本节课中，我们将学习Cython的基础知识。Cython是Python的一个超集，它允许我们轻松地访问C和C++级别的结构。我们将通过比较纯Python代码和Cython代码的性能，来了解Cython如何加速我们的程序，特别是在数值计算密集型任务中。

---

## 1️⃣ 为什么Python在某些情况下“慢”？

上一节我们介绍了Cython的基本概念。本节中，我们来看看为什么纯Python代码在某些计算密集型任务中可能表现不佳。Python本身并不慢，但对于大量重复的简单操作（如加法），其解释型特性和动态类型检查会带来显著的开销。

Python执行一个简单的加法操作 `a + b` 时，解释器需要执行多个步骤：
1.  **解释字节码**：将 `BINARY_ADD` 这样的字节码指令转换为具体的C API调用。
2.  **动态类型查找**：由于Python的强多态性，解释器在运行时必须检查操作数 `a` 和 `b` 的类型，并查找正确的加法函数（例如，整数加法、浮点数加法或字符串连接）。
3.  **错误检查和内存管理**：进行类型检查、引用计数增减等安全操作。
4.  **结果包装**：将C级别的计算结果重新包装成Python对象返回。

这些步骤对于单次操作来说开销很小，但在循环中执行数百万次时，累积的开销就会变得非常可观。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_5.png)

以下是一个纯Python加法函数及其执行时间的例子：

```python
def foo(a, b):
    return a + b

# 使用IPython的 %timeit 魔法函数计时
%timeit foo(1, 2)
# 输出示例：95.7 ns ± 0.96 ns per loop
```

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_7.png)

---

## 2️⃣ Cython初体验：从编译中获益

上一节我们看到了Python解释器的开销。本节中，我们来看看仅仅将Python代码编译成C扩展，能带来多少性能提升。

Cython是一个将Python代码（`.pyx` 文件）首先“转译”为C代码，然后再编译为机器码（`.so` 或 `.pyd` 文件）的工具。即使不添加任何静态类型声明，这个编译步骤本身也能消除部分解释开销。

以下是使用Cython编译相同加法函数的步骤：

```python
%%cython
def cyfoo(a, b):
    return a + b

# 计时编译后的函数
%timeit cyfoo(1, 2)
# 输出示例：82.8 ns ± 0.5 ns per loop
```

**性能对比**：
*   纯Python `foo`: ~95.7 纳秒
*   Cython `cyfoo`: ~82.8 纳秒
*   **提升**: 约 **13.5%**

这个提升主要来自于跳过了字节码解释步骤，直接调用编译后的函数。然而，真正的性能飞跃来自于添加静态类型声明。

---

## 3️⃣ 静态类型声明：性能飞跃的关键

上一节我们看到编译带来的小幅提升。本节中，我们来看看如何通过声明C级别的静态类型来获得巨大的性能提升。

在Cython中，我们可以使用 `cdef` 关键字来声明变量、函数参数和返回值的C数据类型（如 `int`, `double`）。这允许Cython生成高效的、直接操作内存的C代码，避免了Python对象的动态类型检查和操作开销。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_9.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_11.png)

让我们以计算阶乘的递归函数为例：

**1. 纯Python版本：**

```python
def py_fact(n):
    if n <= 1:
        return 1
    return n * py_fact(n-1)

%timeit py_fact(20)
# 输出示例：3 µs ± 15.3 ns per loop
```

**2. Cython版本（无类型声明）：**
仅编译，逻辑不变。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_13.png)

```python
%%cython
def cy_fact(n):
    if n <= 1:
        return 1
    return n * cy_fact(n-1)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_15.png)

%timeit cy_fact(20)
# 输出示例：1.25 µs ± 4.37 ns per loop
```
**提升**: 约 **2.4倍**

**3. Cython版本（添加参数类型声明）：**
我们声明参数 `n` 为C的 `double` 类型。

```python
%%cython
def cy_fact_double(double n):
    if n <= 1:
        return 1
    return n * cy_fact_double(n-1)

%timeit cy_fact_double(20)
# 输出示例：1 µs ± 2.22 ns per loop
```
**提升**: 约 **3倍**

**4. Cython版本（`cpdef` 与完整类型声明）：**
使用 `cpdef` 创建同时可供C和Python调用的函数，并声明返回类型。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_17.png)

```python
%%cython
cpdef double cp_fact(double n):
    if n <= 1:
        return 1
    return n * cp_fact(n-1)

%timeit cp_fact(20)
# 输出示例：80 ns ± 0.156 ns per loop
```
**提升**: 约 **37.5倍** (相比最初的3µs)

**为什么使用 `double` 而不是 `int`？**
阶乘结果增长极快，使用C的 `int` 或 `long` 类型很容易导致整数溢出，得到错误结果。`double`（双精度浮点数）提供了更大的数值范围，虽然可能有精度损失，但在此类计算中更安全。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_19.png)

**迭代 vs 递归：**
Python对递归优化不佳。将递归改为迭代通常能进一步提升性能。

```python
%%cython
cpdef double cp_fact_loop(int n):
    cdef double r = 1
    cdef int i
    for i in range(2, n+1):
        r *= i
    return r

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_21.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_23.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_25.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_26.png)

%timeit cp_fact_loop(20)
# 输出示例：65 ns ± 0.091 ns per loop
```

---

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_28.png)

## 4️⃣ Cython中的数据类型

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_30.png)

上一节我们看到了类型声明的威力。本节中，我们系统地了解一下Cython中可用的数据类型。

Cython是Python的超集，因此所有Python类型（如 `list`, `dict`, `str`）依然可用。此外，它引入了完整的C类型系统。

### C类型声明 (`cdef`)
使用 `cdef` 声明的变量是纯粹的C变量，只能在Cython代码内部访问，不能直接从Python交互。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_32.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_34.png)

**基本数值类型：**
```python
%%cython
cdef int a = 10          # 有符号整数
cdef unsigned long b = 5 # 无符号长整数
cdef long long c = 100   # 长整型
cdef float d = 3.14      # 单精度浮点数
cdef double e = 2.718    # 双精度浮点数（常用）
cdef long double f = 1.0 # 扩展精度浮点数
```

**复数类型：**
```python
%%cython
cdef float complex fc = 1+2j
cdef double complex dc = 3+4j
```

**字符串/字节类型：**
在Python 3中需区分Unicode字符串和字节。
```python
%%cython
cdef str s = "Hello"     # Python Unicode 字符串对象
cdef bytes b = b"World"  # Python 字节对象
# C级别的字符/字符串（谨慎使用，可能破坏Python的不可变性保证）
cdef char c = 'A'
cdef char *c_buffer = "dangerous"
```

**导入C级别的模块：**
使用 `cimport` 将C结构导入到Cython的C命名空间，用于高性能操作。
```python
%%cython
from libc.math cimport log  # 导入C标准库的log函数，而不是Python的math.log
cimport numpy as cnp       # 导入NumPy的C接口，用于高效数组操作
```

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_36.png)

---

## 5️⃣ 函数定义：`def`, `cdef`, `cpdef`

上一节我们了解了数据类型。本节中，我们来看看在Cython中定义函数的三种不同方式，它们决定了函数的可访问性和性能。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_38.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_40.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_42.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_43.png)

### 1. `def` 函数
*   使用普通的Python `def` 关键字定义。
*   接受Python对象作为参数，返回Python对象。
*   可以从Python代码中正常调用。
*   性能提升有限，主要来自编译优化，但内部操作仍涉及Python对象开销。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_45.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_47.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_49.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_50.png)

```python
%%cython
def add_def(a, b):
    return a + b
# 可从Python调用: add_def(1, 2)
```

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_52.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_53.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_54.png)

### 2. `cdef` 函数
*   使用 `cdef` 关键字定义。
*   接受和返回C类型或Python类型。
*   **只能在Cython模块内部或其他Cython代码中调用，无法直接从Python代码中访问。**
*   性能最高，因为避免了与Python交互的大部分开销。
*   可用于内部计算，作为 `cpdef` 函数的高性能核心。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_56.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_58.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_59.png)

```python
%%cython
cdef int add_cdef(int a, int b):
    return a + b
# 无法从Python直接调用 add_cdef(1, 2)
```

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_61.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_62.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_64.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_66.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_67.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_69.png)

### 3. `cpdef` 函数
*   使用 `cpdef` 关键字定义。
*   **兼具两者优点**：Cython会生成两个版本——一个高效的C函数（供Cython内部调用）和一个Python包装器（供Python代码调用）。
*   参数和返回类型必须是能转换为Python类型的C类型。
*   是提供高性能接口的推荐方式。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_71.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_73.png)

```python
%%cython
cpdef int add_cpdef(int a, int b):
    return a + b
# 可从Python调用: add_cpdef(1, 2)
# 也可在Cython内部被高效调用
```

**异常处理：**
在 `cdef` 或 `cpdef` 函数中，可以使用 `except` 关键字将C级别的错误转换为Python异常。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_75.png)

```python
%%cython
cpdef int divide(int a, int b) except? -1:
    if b == 0:
        raise ZeroDivisionError("division by zero")
    return a / b
```

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_77.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_79.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_80.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_82.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_84.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_85.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_87.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_89.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_90.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_92.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_94.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_96.png)

---

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_98.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_100.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_102.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_103.png)

## 6️⃣ 性能分析与代码注解

上一节我们学会了如何定义函数。本节中，我们学习如何分析和优化Cython代码的性能。

### 使用Python分析器 (`%prun`)
IPython的 `%prun` 魔法命令可以分析代码执行时间。但对于 `cdef` 函数，Python分析器默认无法跟踪，因为它们本质上是C函数。

**启用Cython函数分析：**
在Cython代码块顶部添加指令 `%%cython --profile True`，或在 `.pyx` 文件开头添加 `# cython: profile=True`。这会注入额外的代码，使Python分析器能够跟踪 `cdef` 函数的调用。

```python
%%cython --profile True
def outer(n):
    cdef int i, res = 0
    for i in range(n):
        res += inner(i)
    return res

cdef int inner(int i):
    return i * i

# 然后使用 %prun 分析
%prun outer(1000)
```
**注意**：开启分析会引入额外开销，仅用于开发调试，不应在生产中使用。

### 使用注解报告 (`-a` 标志)
`%%cython -a` 命令会生成一个HTML注解报告，直观显示每一行Cython代码对应的生成C代码量。
*   **白色背景**：该行代码被翻译成高效的、少量的C代码。
*   **黄色背景**：该行代码产生了大量C代码，通常意味着存在Python对象交互、类型未知或函数调用开销，是潜在的优化点。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_105.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_107.png)

优化目标就是通过添加类型声明、使用C函数等方式，尽可能让代码行“变白”。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_109.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_111.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_113.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_115.png)

```python
%%cython -a
def sum_squares(list lst):
    cdef int total = 0
    cdef int i
    cdef int val
    for i in range(len(lst)):
        val = lst[i]  # 这行可能是黄色的，因为lst[i]返回Python对象
        total += val * val
    return total
```
查看报告后，你可能会尝试将 `list` 参数改为类型化的内存视图或NumPy数组以获得更好性能。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_117.png)

---

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_119.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_121.png)

## 7️⃣ 与NumPy高效交互

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_123.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_125.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_127.png)

上一节我们学习了分析工具。本节中，我们来看一个数据科学中的常见场景：优化NumPy数组操作。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_129.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_131.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_133.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_135.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_137.png)

Cython通过 `cimport numpy` 提供了与NumPy C API的直接接口，可以绕过Python调用开销，直接访问数组底层数据。

**示例：计算香农熵**  
公式：$H(p) = -\sum_{i} p_i \log(p_i)$

**1. 纯NumPy版本（基线）：**
```python
import numpy as np
def entropy_numpy(p):
    return -np.sum(p * np.log(p))
```

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_139.png)

**2. 初级Cython版本（提升有限）：**
仅编译和简单声明。
```python
%%cython
import numpy as np
cimport numpy as cnp

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_141.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_143.png)

def entropy_cy_v0(cnp.ndarray p):
    return -np.sum(p * np.log(p)) # 仍调用Python的np.sum和np.log
```

**3. 优化版本1（使用C函数和循环）：**
```python
%%cython
import numpy as np
cimport numpy as cnp
from libc.math cimport log

def entropy_cy_v1(cnp.ndarray[double, ndim=1] p):
    cdef double total = 0.0
    cdef Py_ssize_t i
    for i in range(p.shape[0]):
        if p[i] > 0:
            total += p[i] * log(p[i])
    return -total
```
*   `cnp.ndarray[double, ndim=1]`：声明p是一维双精度浮点数组。
*   `from libc.math cimport log`：导入C的 `log` 函数，而非Python的。
*   手动循环避免调用 `np.sum`。

**4. 优化版本2（使用内存视图，更通用）：**
内存视图语法更简洁，且不限于NumPy数组。
```python
%%cython
from libc.math cimport log

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_145.png)

def entropy_cy_v2(double[:] p_view):
    cdef double total = 0.0
    cdef Py_ssize_t i
    # 可选的：关闭边界检查以获取极致性能（需确保代码安全）
    # with nogil:
    for i in range(p_view.shape[0]):
        if p_view[i] > 0:
            total += p_view[i] * log(p_view[i])
    return -total

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_147.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_149.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_151.png)

# 调用时可以直接传递NumPy数组
import numpy as np
arr = np.random.rand(1000)
arr = arr / arr.sum()
result = entropy_cy_v2(arr)
```

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_153.png)

**关键点**：
*   将 `np.log` 替换为C的 `log`。
*   将通用的 `ndarray` 声明为具体的 `ndarray[double, ndim=1]` 或使用 `double[:]` 内存视图。
*   用C循环替代 `np.sum`。
*   内存视图（`double[:]`）是推荐的方式，它语法干净且支持多种缓冲区协议对象。

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_155.png)

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_157.png)

---

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_159.png)

## 🎯 总结

![](img/b8d9e1d8490d7c84cfc812d37b0318c6_161.png)

本节课中我们一起学习了Cython的核心概念和使用方法：

1.  **动机**：Cython通过将Python代码编译为C扩展，并允许声明静态C类型，可以显著提升计算密集型任务的性能（通常可达数十倍甚至数百倍）。
2.  **关键步骤**：从 `.pyx` 文件到 `.c` 文件（转译），再到 `.so`/`.pyd` 文件（编译），最后被Python导入。
3.  **类型声明**：使用 `cdef` 声明C类型（如 `int`, `double`, `cnp.ndarray`）是获得性能提升的关键。
4.  **函数类型**：
    *   `def`：供Python调用，性能提升有限。
    *   `cdef`：纯C函数，性能最高，但无法从Python直接调用。
    *   `cpdef`：两者结合，是创建高性能接口的推荐方式。
5.  **工具**：使用 `%%cython -a` 生成注解报告来定位性能瓶颈，使用 `--profile` 参数使 `cdef` 函数可被Python分析器跟踪。
6.  **与NumPy集成**：通过 `cimport numpy` 和类型化声明（如 `double[:]` 内存视图），可以直接操作NumPy数组的底层数据，避免Python层开销。

Cython让你能够以接近Python的语法编写高性能的C扩展，特别适合优化循环密集型、数值计算密集型的代码段，是数据科学家和科学计算工程师工具箱中的重要工具。