# 004：深入理解torch.compile中的Guards - 工作原理、开销与优化方法 🛡️

![](img/a0194f0cb4df1c7603e381eb50d601f5_0.png)

在本节课中，我们将学习PyTorch编译器的核心概念之一：Guards。我们将了解Guards是什么，它们如何工作，以及它们对编译时间和运行时性能的影响。最后，我们将探讨几种优化Guards开销的方法。

![](img/a0194f0cb4df1c7603e381eb50d601f5_2.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_4.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_6.png)

## 什么是Guards？

![](img/a0194f0cb4df1c7603e381eb50d601f5_8.png)

Guards是PyTorch编译器中的一个重要概念。它们本质上是在程序特定执行路径上追踪时遇到的所有条件的编码。Guards是确保编译后代码正确性的核心机制。

![](img/a0194f0cb4df1c7603e381eb50d601f5_10.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_11.png)

上一节我们介绍了Guards的基本概念，本节中我们来看看它们的具体作用。

![](img/a0194f0cb4df1c7603e381eb50d601f5_13.png)

一个编译后的计算图仅在创建时所做的所有假设都成立时才有效。这些假设就是Guards。例如，Guards可以确保输入张量的形状、步长或模型中的常量值没有改变。

## Guards如何融入编译系统？

![](img/a0194f0cb4df1c7603e381eb50d601f5_15.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_16.png)

为了理解Guards，我们需要建立一个关于`torch.compile`如何工作的心智模型。

![](img/a0194f0cb4df1c7603e381eb50d601f5_18.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_19.png)

当用户调用一个被`torch.compile`装饰的函数时，编译器并非在装饰时立即被调用。实际上，编译器是在函数**第一次被调用时**才被触发。此时，编译器会根据当前的输入`X`和模型`model`的状态来特化计算图。

![](img/a0194f0cb4df1c7603e381eb50d601f5_21.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_23.png)

因此，编译是**特化**的，它基于编译时刻观察到的条件。Guards就是用来记录这些特化条件的。编译出的计算图只有在所有Guards都验证通过（即为真）时才有效。

我们可以将这个过程建模为：每个用户函数映射到一个**编译单元**列表。每个编译单元包含一个计算图及其对应的Guards集合。执行时，系统会遍历这个列表，检查每个编译单元的Guards。如果某个单元的Guards全部通过，则执行其对应的编译图。

以下是编译单元列表的示意结构：
```python
# 伪代码表示编译单元列表
compile_units = [
    CompileUnit(graph=graph1, guards=[guard1, guard2, ...]),
    CompileUnit(graph=graph2, guards=[guard3, guard4, ...]),
    ...
]
```

## 为什么会有这么多Guards？

![](img/a0194f0cb4df1c7603e381eb50d601f5_25.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_27.png)

在我们的简单例子中，程序代码很少，但生成的Guards列表却很长。这可能会让人感到不安。

![](img/a0194f0cb4df1c7603e381eb50d601f5_29.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_31.png)

原因在于，`torch.compile`默认选择**保持正确性**。这意味着，如果用户有可能更改模型的任何部分（例如，子模块的属性），从而导致先前编译的计算图失效，那么编译器就必须为这种可能性设置Guard。

![](img/a0194f0cb4df1c7603e381eb50d601f5_33.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_35.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_37.png)

例如，考虑一个`LayerNorm`模块，它有一个`eps`属性。用户可能在运行时修改这个值：
```python
model.layer_norm.eps = 1e-5
```
如果编译后的图使用了旧的`eps`值，那么修改后，图就不再正确。因此，为了保持正确性，编译器必须对`model.layer_norm.eps`设置Guard。

这种设计选择带来了**正确性开销**：我们通过引入大量Guards来确保安全，但这可能会带来性能开销。

## Guards带来的开销

Guards主要带来两种开销：

![](img/a0194f0cb4df1c7603e381eb50d601f5_39.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_40.png)

1.  **编译时间开销**：每当Guard失败（即条件改变），就会触发重新编译。重新编译会走完整个`torch.compile`流水线，增加总的编译时间。
2.  **运行时开销**：每次调用函数时，都需要检查Guards。此外，在最坏情况下，系统可能需要遍历多个编译单元并检查多组Guards才能找到匹配的图。对于推理时间很短的模型，Guard开销可能会变得非常明显。

![](img/a0194f0cb4df1c7603e381eb50d601f5_42.png)

上一节我们了解了Guards的开销类型，本节中我们来看看如何优化编译时间开销。

![](img/a0194f0cb4df1c7603e381eb50d601f5_44.png)

## 如何减少由Guards引起的重新编译？

以下是几种常见的导致重新编译的模式及应对方法：

以下是避免因输入张量形状变化而重新编译的方法：

*   **使用 `mark_dynamic`**：如果你知道某个维度（如批次大小）会动态变化，可以指示编译器将其视为符号形状，而非静态形状。
    ```python
    torch.compile.mark_dynamic(x, 0)  # 将张量 x 的第0维标记为动态
    ```
    这比设置 `dynamic=True` 更精确，后者会使整个模型动态化，可能影响性能。

以下是避免因常量值变化而重新编译的方法：

*   **将常量包装为张量**：如果函数中有一个不断变化的浮点常量，默认情况下编译器会对其精确值设置Guard。将其包装为张量可以避免此问题，因为编译器只对张量的形状设置Guard，而不对其数据设置Guard。
    ```python
    # 导致重新编译
    def func(x, c):
        return x + c
    # 避免重新编译
    def func(x, c):
        c_tensor = torch.tensor(c)
        return x + c_tensor
    ```

![](img/a0194f0cb4df1c7603e381eb50d601f5_46.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_48.png)

以下是避免因模块属性变化而重新编译的方法：

*   **使用 `allow_unspecialized_ints_in_tensor_on_nn_modules` 配置**：如果模块属性是参与计算的整数，可以将其转换为符号整数。这样编译器就不会假设它是常量。
    ```python
    @torch.compile(options={"allow_unspecialized_ints_in_tensor_on_nn_modules": True})
    def forward(self, x):
        return x + self.counter  # counter 被视为符号整数
    ```

![](img/a0194f0cb4df1c7603e381eb50d601f5_50.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_52.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_54.png)

以下是调整重新编译限制的方法：

![](img/a0194f0cb4df1c7603e381eb50d601f5_56.png)

*   **调整 `recompile_limit`**：编译器默认限制一个函数最多重新编译8次，超过后将回退到即时执行模式。如果你有合理的理由需要更多次编译，可以增加此限制，但请谨慎使用。
    ```python
    @torch.compile(options={"recompile_limit": 20})
    def func(x):
        ...
    ```

![](img/a0194f0cb4df1c7603e381eb50d601f5_58.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_60.png)

![](img/a0194f0cb4df1c7603e381eb50d601f5_62.png)

## 如何减少Guards的运行时开销？

即使没有重新编译，检查大量Guards也会带来运行时开销。以下是两种降低此开销的“逃生舱口”：

1.  **`skip_guards_when_unsafe` (自动)**：
    *   **原理**：当你的模型经过充分“预热”（即已经看到了所有可能的编译场景）并且环境稳定后，你不再需要所有Guards，只需要能区分不同编译单元的少量关键Guards。此选项会自动分析并丢弃非关键的Guards。
    *   **使用方法**：
        ```python
        torch._dynamo.config.skip_guards_when_unsafe = True
        ```
    *   **注意**：之所以标记为“unsafe”，是因为如果在设置此选项后修改了模型（如更改`eps`），编译器将无法察觉，从而可能静默地使用错误的计算图。

2.  **Guards Filter API (手动控制，开发中)**：
    *   **原理**：在编译时提供一个钩子函数，让开发者可以基于Guard的类型等信息，手动决定保留或跳过哪些Guards。
    *   **使用方法**（概念性示例）：
        ```python
        def my_guard_filter(guard_list):
            # 分析guard_list，返回一个布尔列表，True表示保留，False表示跳过
            keep = []
            for guard in guard_list:
                if guard.is_tensor_guard(): # 只保留张量相关的Guards
                    keep.append(True)
                else:
                    keep.append(False)
            return keep

        @torch.compile(guards_filter=my_guard_filter)
        def func(x):
            ...
        ```
    *   **优势**：提供更精细的控制，适合生产系统。未来计划提供一些预制的过滤器（如“仅保留张量Guards”、“跳过所有内置模块的Guards”）。

## Guards的实现优化

在底层，为了提升性能，Guards的实现采用了多种优化：

*   **树形结构**：Guards被组织成树形结构，便于高效遍历和检查。
*   **C++实现**：关键路径的Guard检查被移到C++层以提高速度。
*   **快速失败**：如果某个编译单元经常失败，系统会将其失败概率高的Guards移到检查顺序的前面，以便尽快跳过它。
*   **最近使用策略**：系统采用MRU策略，将最近成功使用的编译单元放在列表前面，以提高缓存命中率。

## 总结

![](img/a0194f0cb4df1c7603e381eb50d601f5_64.png)

本节课中我们一起学习了PyTorch `torch.compile` 中Guards的核心知识。我们了解到：

![](img/a0194f0cb4df1c7603e381eb50d601f5_66.png)

1.  **Guards是什么**：它们是确保编译图正确性的特化条件编码。
2.  **Guards的作用**：它们将用户函数映射到一系列编译单元，并在运行时验证条件以分派正确的计算图。
3.  **Guards的开销**：包括由重新编译引起的**编译时间开销**和由运行时检查引起的**运行时开销**。
4.  **优化编译时间**：可以通过`mark_dynamic`、将常量包装为张量、使用特定配置以及调整`recompile_limit`来减少不必要的重新编译。
5.  **优化运行时开销**：可以利用`skip_guards_when_unsafe`（自动）或未来的Guards Filter API（手动）来减少需要检查的Guards数量，从而降低运行时开销。

![](img/a0194f0cb4df1c7603e381eb50d601f5_68.png)

理解Guards是掌握`torch.compile`行为、诊断性能问题并进行有效优化的关键。