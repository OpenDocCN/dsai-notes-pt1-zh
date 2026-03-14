# 12：Python 中的现代优化方法 🚀

在本节课中，我们将学习 Python 中的现代优化方法。优化是科学和工程中广泛使用的工具，但传统的优化工具栈自计算机诞生以来变化不大。近年来，随着并行计算和非线性问题处理能力的发展，优化方法正朝着处理大规模物理、工程和化学问题的方向演进。我们将从优化基础开始，介绍一些现代方法，并重点讲解 `Mystic` 包的使用。

## 优化基础介绍

上一节我们概述了课程内容，本节中我们来看看优化的基本组成部分。优化通常涉及一个目标函数或成本函数。你可以将其想象成一个表面，优化的目标是找到这个表面上的最低点（最小值），无论是局部最小值还是全局最小值。

![](img/9a512611b7f4945c6d5160b215b8b8da_1.png)

在回归分析中，优化用于最小化数据点与拟合线之间的距离。在分类问题中，优化则是为了找到能最好地区分不同组别的边界线。本质上，优化就是理解目标函数并试图找到其最小值。

以下是优化问题的标准形式：

```python
def objective(x):
    return 1.3*x**2 - 4*x + 6
```

我们通常使用优化器（如 `Nelder-Mead`）来寻找最小值。优化器将目标函数和一个初始解向量作为输入，然后尝试“滚下山坡”找到最低点。

```python
from scipy.optimize import minimize
result = minimize(objective, x0=[0.0], method='Nelder-Mead')
```

![](img/9a512611b7f4945c6d5160b215b8b8da_3.png)

然而，优化器通常是一个“黑盒”。你提供输入，它返回一个结果，但中间过程是未知的。你无法轻易判断是否找到了真正的全局最小值，尤其是在高维或计算成本高昂的问题中。

![](img/9a512611b7f4945c6d5160b215b8b8da_5.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_7.png)

## 应用约束：边界约束

![](img/9a512611b7f4945c6d5160b215b8b8da_9.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_10.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_11.png)

上一节我们介绍了基本的无约束优化，本节中我们来看看如何通过约束来融入物理信息。最常见的约束类型是边界约束（或“盒子约束”），它将搜索限制在变量的特定范围内。

![](img/9a512611b7f4945c6d5160b215b8b8da_13.png)

例如，我们可能只想在区间 `[2, 4]` 内寻找一阶贝塞尔函数的最小值。

![](img/9a512611b7f4945c6d5160b215b8b8da_15.png)

```python
from scipy.optimize import minimize_scalar
from scipy.special import j1

result = minimize_scalar(j1, bounds=(2, 4), method='bounded')
```

通过应用边界约束，优化器不会在指定区域之外进行搜索，这有助于缩小解空间并融入先验知识。

## 非线性测试函数与可视化

为了理解和测试优化器，我们经常使用标准非线性测试函数，如 `Rosenbrock` 函数。`Mystic` 包内置了许多这样的函数。

```python
from mystic.models import rosenbrock
# 获取函数信息和绘图范围
print(rosenbrock.__doc__)
```

可视化目标函数的表面对于理解优化问题和诊断结果至关重要。对于低成本函数，我们可以绘制其三维图像来观察山谷和峰值。

## 局部优化与梯度信息

许多优化算法是局部优化器。它们从一个初始点开始，沿着梯度下降方向寻找最近的局部最小值。提供目标函数的导数（梯度）通常可以帮助优化器更快、更准确地收敛。

```python
from scipy.optimize import minimize
from mystic.models import rosenbrock, rosenbrock_derivative

x0 = [0.5, 0.5, 0.5, 0.5, 0.5] # 5维初始猜测
result_without_deriv = minimize(rosenbrock, x0, method='Nelder-Mead')
result_with_deriv = minimize(rosenbrock, x0, jac=rosenbrock_derivative, method='BFGS')
```

然而，即使有梯度信息，局部优化器也可能陷入错误的局部最小值。对于复杂问题，通常需要从多个随机初始点运行优化器，并选择最佳结果。

## 传统约束方法：惩罚函数

上一节我们看了边界约束，本节中我们来看看另一种融入约束的传统方法：惩罚函数法。惩罚函数通过向目标函数添加一个项来工作，当约束被违反时，该项会显著增加目标函数值，从而在违反约束的区域形成“屏障”。

考虑一个带有约束的优化问题：
- 最小化目标函数：`f(x) = 2*x[0]*x[1] + 2*x[0] - x[0]**2 - 2*x[1]**2`
- 约束条件：
    1. `x[1] >= 1`
    2. `x[0]**3 - x[1] == 0`

在 `scipy.optimize` 中，我们可以使用 `SLSQP` 等支持约束的方法。

```python
import numpy as np
from scipy.optimize import minimize

def objective(x):
    return 2*x[0]*x[1] + 2*x[0] - x[0]**2 - 2*x[1]**2

def derivative(x):
    return np.array([2*x[1] + 2 - 2*x[0], 2*x[0] - 4*x[1]])

# 约束定义
cons = ({'type': 'ineq', 'fun': lambda x: x[1] - 1},
        {'type': 'eq', 'fun': lambda x: x[0]**3 - x[1]})

result_unconstrained = minimize(objective, [0, 0], jac=derivative, method='SLSQP')
result_constrained = minimize(objective, [0, 0], jac=derivative, constraints=cons, method='SLSQP')
```

惩罚函数和这类约束求解器（如线性/二次规划）通常要求目标函数和约束是线性或二次的，这对于高度非线性问题是一个限制。

## 优化方法概览与凸优化

`SciPy` 的 `optimize` 模块提供了从传统方法到现代方法的各种优化器。大多数是无约束的，而约束优化器往往局限于线性或二次规划。

对于严格的凸优化问题（目标函数和约束均为凸），可以使用专门的工具如 `CVXOPT`。它要求将问题表述为矩阵形式：

```
最小化： (1/2) * x.T * P * x + q.T * x
约束条件： G * x <= h
           A * x = b
```

`CVXOPT` 的一个优点是它通常提供求解过程的诊断信息，例如目标函数值随迭代的变化轨迹，这增加了对解的置信度。

## 全局优化方法

对于存在多个局部最小值的问题，局部优化器可能不足。这时需要使用全局优化方法，如差分进化或遗传算法。

```python
from scipy.optimize import differential_evolution
from mystic.models import rosenbrock

bounds = [(-10, 10)] * 5 # 5维问题，每维边界为[-10, 10]
result = differential_evolution(rosenbrock, bounds)
```

![](img/9a512611b7f4945c6d5160b215b8b8da_17.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_18.png)

全局优化器的代价是通常需要大量的函数评估，计算成本很高。它们通过在整个搜索空间内随机“跳跃”来寻找更好的解，直到满足终止条件（例如，多次迭代后没有改进）。

![](img/9a512611b7f4945c6d5160b215b8b8da_20.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_21.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_23.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_25.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_26.png)

## 特殊用途优化

![](img/9a512611b7f4945c6d5160b215b8b8da_28.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_30.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_31.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_33.png)

优化还有许多特殊用途：
*   **最小二乘拟合**：用于参数估计和获取协方差矩阵（误差估计）。
    ```python
    from scipy.optimize import curve_fit
    ```
*   **整数规划**：变量被限制为整数的优化，用于密码学等领域。
*   **求根**：寻找使一组方程等于零的解向量。
    ```python
    from scipy.optimize import root
    ```

## 优化诊断的挑战

传统上，验证优化结果是一项挑战，更像一门艺术而非精确科学。常见做法包括：
1.  可视化解（对于低维问题）。
2.  从多个随机起点运行优化器，选择最佳结果。
3.  检查求解器提供的收敛轨迹日志（如果可用）。
4.  对于拟合问题，检查协方差矩阵。

![](img/9a512611b7f4945c6d5160b215b8b8da_35.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_36.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_38.png)

由于许多优化器是“黑盒”，缺乏内部状态信息，很难确定是否找到了全局最优解，或者求解过程是否正常终止。

![](img/9a512611b7f4945c6d5160b215b8b8da_40.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_42.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_44.png)

## 引入 Mystic：更透明的优化

![](img/9a512611b7f4945c6d5160b215b8b8da_46.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_48.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_49.png)

`Mystic` 库旨在解决传统优化中的一些痛点，提供更透明、可定制和诊断友好的优化环境。

![](img/9a512611b7f4945c6d5160b215b8b8da_51.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_53.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_55.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_57.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_58.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_60.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_62.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_63.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_65.png)

**与 SciPy 兼容的接口**
`Mystic` 提供了与 `scipy.optimize` 类似的单行函数接口，易于上手。

![](img/9a512611b7f4945c6d5160b215b8b8da_67.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_69.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_71.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_73.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_75.png)

```python
from mystic import fmin
from mystic.models import rosenbrock

![](img/9a512611b7f4945c6d5160b215b8b8da_77.png)

result = fmin(rosenbrock, [0, 0, 0], retall=True)
# retall=True 返回所有迭代的解向量，便于分析轨迹
```

**回调函数与监控器**
`Mystic` 允许在优化过程中插入回调函数，例如动态绘制参数变化或记录日志。

![](img/9a512611b7f4945c6d5160b215b8b8da_79.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_81.png)

```python
from mystic.monitors import VerboseLoggingMonitor

![](img/9a512611b7f4945c6d5160b215b8b8da_83.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_85.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_86.png)

monitor = VerboseLoggingMonitor(1, 1) # 每1次迭代打印/记录
result = fmin(rosenbrock, [0,0,0], itermon=monitor)
```

![](img/9a512611b7f4945c6d5160b215b8b8da_88.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_90.png)

日志文件可以通过 `mystic` 的日志阅读器进行可视化，绘制成本函数和参数随迭代的变化，直观判断收敛情况。

![](img/9a512611b7f4945c6d5160b215b8b8da_92.png)

**面向对象的求解器接口**
对于高级定制，`Mystic` 提供了面向对象的接口。你可以创建求解器实例，并独立配置其各个部分：终止条件、种群初始化、变异策略等。

![](img/9a512611b7f4945c6d5160b215b8b8da_94.png)

```python
from mystic.solvers import DifferentialEvolutionSolver
from mystic.termination import VTR, ChangeOverGeneration
from mystic.monitors import VerboseMonitor

![](img/9a512611b7f4945c6d5160b215b8b8da_96.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_98.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_100.png)

solver = DifferentialEvolutionSolver(ndim=9, npop=90) # 9维问题，90个个体
solver.SetRandomInitialPoints(min=[-100]*9, max=[100]*9)
solver.SetEvaluationLimits(generations=2000)

![](img/9a512611b7f4945c6d5160b215b8b8da_102.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_103.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_104.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_105.png)

# 设置复合终止条件：达到目标值或长时间无改进
term = VTR(1e-8) | (ChangeOverGeneration() & VTR(0.0))
solver.SetTermination(term)

![](img/9a512611b7f4945c6d5160b215b8b8da_107.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_109.png)

solver.Solve(rosenbrock)
solution = solver.bestSolution
```

![](img/9a512611b7f4945c6d5160b215b8b8da_111.png)

**灵活的初始点设置**
你可以从任意概率分布中抽取初始点。

```python
from mystic.math import Distribution
import numpy as np

dist = Distribution(np.random.normal, 5, 1) # 均值为5，标准差为1的正态分布
solver.SetSampledInitialPoints(dist)
```

![](img/9a512611b7f4945c6d5160b215b8b8da_113.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_115.png)

**核心特性：核变换与约束算子**
`Mystic` 最强大的特性之一是能够使用“核变换”或“约束算子”来施加复杂的非线性约束。这与传统的惩罚函数不同。惩罚函数是在违反约束的区域修改目标函数表面（添加惩罚项）。而核变换是将整个搜索空间映射到满足约束的子空间上，优化器只在这个有效的子空间内工作。

![](img/9a512611b7f4945c6d5160b215b8b8da_117.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_119.png)

这允许将复杂的物理或工程约束（如等式、不等式、微分约束）直接融入优化过程，大大提高了处理现实世界非线性约束问题的能力。

![](img/9a512611b7f4945c6d5160b215b8b8da_121.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_123.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_124.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_126.png)

```python
# 概念性示例：使用 mystic 的约束
from mystic.constraints import *

![](img/9a512611b7f4945c6d5160b215b8b8da_128.png)

# 定义约束函数
def constraints(x):
    x[0] = x[1]**2  # 例如，一个非线性等式约束
    return x

# 创建约束算子
@with_constraints(constraints)
def constrained_objective(x):
    return rosenbrock(x)

# 现在优化 constrained_objective，解将自动满足约束
```

## 总结

![](img/9a512611b7f4945c6d5160b215b8b8da_130.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_132.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_133.png)

本节课中我们一起学习了 Python 中优化方法的基础和现代进展。我们从优化问题的基本定义开始，探讨了局部优化、全局优化以及传统的约束处理方法（如边界约束和惩罚函数）。我们指出了传统优化工具在诊断性、约束处理能力和透明度方面的局限性。

![](img/9a512611b7f4945c6d5160b215b8b8da_135.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_137.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_138.png)

随后，我们深入介绍了 `Mystic` 优化库，它通过提供详细的求解监控、灵活的求解器配置、可定制的终止条件以及强大的核变换约束处理能力，旨在使优化过程更加透明和强大。`Mystic` 既兼容 `SciPy` 的简单接口，也提供了面向对象的高级接口，适用于从快速原型到复杂非线性约束问题求解的各种场景。

![](img/9a512611b7f4945c6d5160b215b8b8da_140.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_142.png)

![](img/9a512611b7f4945c6d5160b215b8b8da_143.png)

现代优化方法正朝着融入更多物理信息、利用并行计算以及提供更好诊断工具的方向发展，`Mystic` 是这一趋势中的一个代表性工具。