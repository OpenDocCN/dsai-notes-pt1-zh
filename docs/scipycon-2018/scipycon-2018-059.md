# 59：异质主体建模的开源工具 🧠

![](img/9ea5c2cb03436ef757c558f33faae725_1.png)

![](img/9ea5c2cb03436ef757c558f33faae725_3.png)

![](img/9ea5c2cb03436ef757c558f33faae725_5.png)

![](img/9ea5c2cb03436ef757c558f33faae725_7.png)

![](img/9ea5c2cb03436ef757c558f33faae725_9.png)

![](img/9ea5c2cb03436ef757c558f33faae725_11.png)

![](img/9ea5c2cb03436ef757c558f33faae725_13.png)

在本节课中，我们将学习异质主体建模的基本概念、传统经济学建模方法的局限性，以及一个名为HARK的开源工具包如何通过模块化、可扩展的框架来革新这一领域。

![](img/9ea5c2cb03436ef757c558f33faae725_15.png)

![](img/9ea5c2cb03436ef757c558f33faae725_17.png)

![](img/9ea5c2cb03436ef757c558f33faae725_19.png)

![](img/9ea5c2cb03436ef757c558f33faae725_21.png)

![](img/9ea5c2cb03436ef757c558f33faae725_23.png)

![](img/9ea5c2cb03436ef757c558f33faae725_25.png)

![](img/9ea5c2cb03436ef757c558f33faae725_27.png)

## 概述：什么是动态经济建模？

![](img/9ea5c2cb03436ef757c558f33faae725_29.png)

![](img/9ea5c2cb03436ef757c558f33faae725_31.png)

![](img/9ea5c2cb03436ef757c558f33faae725_33.png)

![](img/9ea5c2cb03436ef757c558f33faae725_35.png)

![](img/9ea5c2cb03436ef757c558f33faae725_37.png)

![](img/9ea5c2cb03436ef757c558f33faae725_39.png)

上一节我们介绍了课程主题。本节中，我们来看看动态经济建模的核心思想。它本质上是研究主体（如消费者、家庭、企业）在面临风险和不确定性时，如何随时间做出最优决策的控制问题。

![](img/9ea5c2cb03436ef757c558f33faae725_41.png)

![](img/9ea5c2cb03436ef757c558f33faae725_43.png)

![](img/9ea5c2cb03436ef757c558f33faae725_45.png)

![](img/9ea5c2cb03436ef757c558f33faae725_47.png)

![](img/9ea5c2cb03436ef757c558f33faae725_49.png)

![](img/9ea5c2cb03436ef757c558f33faae725_51.png)

![](img/9ea5c2cb03436ef757c558f33faae725_53.png)

一个动态模型通常包含以下要素：
*   **状态空间**：主体可能处于的各种情况。
*   **控制空间**：主体可以采取的不同行动。
*   **约束**：在特定状态下可以做出的选择限制。
*   **冲击与不确定性**：随时间发生在主体身上的随机事件。
*   **动态过程**：当前的状态、控制和冲击如何共同决定下一期的状态。
*   **偏好或决策规则**：主体如何评价不同的经历序列，并据此做出选择。在传统经济学中，这通常意味着最大化某个**效用函数**。

![](img/9ea5c2cb03436ef757c558f33faae725_55.png)

![](img/9ea5c2cb03436ef757c558f33faae725_57.png)

![](img/9ea5c2cb03436ef757c558f33faae725_59.png)

![](img/9ea5c2cb03436ef757c558f33faae725_61.png)

![](img/9ea5c2cb03436ef757c558f33faae725_63.png)

## 模型分类与“代表性主体”的局限

![](img/9ea5c2cb03436ef757c558f33faae725_65.png)

![](img/9ea5c2cb03436ef757c558f33faae725_67.png)

![](img/9ea5c2cb03436ef757c558f33faae725_69.png)

![](img/9ea5c2cb03436ef757c558f33faae725_71.png)

理解了动态模型的基本要素后，我们进一步对模型进行分类。模型可以是无限期或有限期的，也可以是离散时间或连续时间的。我们今天主要讨论**离散时间模型**。

![](img/9ea5c2cb03436ef757c558f33faae725_73.png)

![](img/9ea5c2cb03436ef757c558f33faae725_75.png)

对于非经济学背景的听众，一个令人惊讶的事实是：许多宏观经济模型完全抽象掉了人与人之间的差异。它们假设所有消费者可以聚合成一个“代表性消费者”，所有企业聚合成一个“代表性企业”。这种简化在某些假设下可能成立，但许多学者认为，正是忽略了**异质性**，是导致我们对像“大衰退”这样的事件理解不足的背景因素之一。

![](img/9ea5c2cb03436ef757c558f33faae725_77.png)

![](img/9ea5c2cb03436ef757c558f33faae725_79.png)

![](img/9ea5c2cb03436ef757c558f33faae725_81.png)

![](img/9ea5c2cb03436ef757c558f33faae725_83.png)

![](img/9ea5c2cb03436ef757c558f33faae725_85.png)

![](img/9ea5c2cb03436ef757c558f33faae725_87.png)

![](img/9ea5c2cb03436ef757c558f33faae725_89.png)

因此，我们关注的模型是包含**异质性**的模型。这种异质性既包括“事前异质性”（如主体有不同的偏好或完全不同的类型），也包括“事后异质性”（即不同的随机冲击序列导致不同的结果分布）。关键在于，个体视为外生的某些动态变量，实际上是整个系统内生的，是所有主体行为和状态的共同结果。这就需要引入“均衡”概念：主体的信念与基于信念行动所产生的现实结果之间需要保持一致。

![](img/9ea5c2cb03436ef757c558f33faae725_91.png)

![](img/9ea5c2cb03436ef757c558f33faae725_93.png)

![](img/9ea5c2cb03436ef757c558f33faae725_95.png)

![](img/9ea5c2cb03436ef757c558f33faae725_97.png)

![](img/9ea5c2cb03436ef757c558f33faae725_99.png)

## 传统方法的挑战与HARK的解决方案

![](img/9ea5c2cb03436ef757c558f33faae725_101.png)

![](img/9ea5c2cb03436ef757c558f33faae725_103.png)

![](img/9ea5c2cb03436ef757c558f33faae725_105.png)

![](img/9ea5c2cb03436ef757c558f33faae725_107.png)

上一节我们指出了异质主体建模的重要性，但进入这个领域并实现上述均衡非常困难。传统上，你需要经历漫长的学习过程，并掌握许多未被系统传授的“秘密知识”。

![](img/9ea5c2cb03436ef757c558f33faae725_109.png)

当前经济学界的编程实践存在诸多问题：
*   代码仅为单一目的临时编写，缺乏规划和风格。
*   大量重复造轮子。
*   文档缺失、过时或晦涩难懂。
*   不同模型之间难以结合和比较。

![](img/9ea5c2cb03436ef757c558f33faae725_111.png)

造成这些问题的原因包括学科惯性、将代码视为需要保密的“商业秘密”，以及同行评审中难以深入核查代码。

![](img/9ea5c2cb03436ef757c558f33faae725_113.png)

![](img/9ea5c2cb03436ef757c558f33faae725_115.png)

![](img/9ea5c2cb03436ef757c558f33faae725_117.png)

为了解决这些问题，Econ-ARC组织推出了**异质主体资源与工具包**。HARK是一个用于求解离散时间异质主体模型的**模块化、可扩展、可互操作的开源工具包**。它旨在：
1.  **降低入门门槛**：提供通用框架，使模型的核心组件（动态、行为、加总、信念形成）可分离、可模块化、可轻松替换和组合。
2.  **便于教学**：提供超越基础数值方法、直接针对模型构建的教学资源。
3.  **便于比较与审计**：使不同模型更容易在相同基础上进行比较和验证。
4.  **连接不同范式**：其框架旨在兼容最优化主体和基于规则的“自主体”，试图在传统经济学与基于主体的建模之间架起桥梁。

![](img/9ea5c2cb03436ef757c558f33faae725_119.png)

## HARK的核心机制与实例演示

![](img/9ea5c2cb03436ef757c558f33faae725_121.png)

![](img/9ea5c2cb03436ef757c558f33faae725_123.png)

![](img/9ea5c2cb03436ef757c558f33faae725_125.png)

了解了HARK的目标后，我们来看看它的具体实现机制。HARK采用面向对象编程思想。所有类型的主体都继承自一个顶级超类`AgentType`，其核心方法是`solve()`，这是一个通用的逆向归纳循环。通过这种统一的框架，不同类型的主体可以很好地协同工作。

![](img/9ea5c2cb03436ef757c558f33faae725_127.png)

考虑一个基础的消费-储蓄模型，主体面临收入冲击。其数学模型可以表示为：

![](img/9ea5c2cb03436ef757c558f33faae725_129.png)

![](img/9ea5c2cb03436ef757c558f33faae725_131.png)

**公式**：
主体最大化期望效用：`E[∑ β^t * u(c_t)]`
约束条件：`a_{t+1} = R * (a_t + y_t - c_t)`
其中，`u(c) = c^(1-ρ)/(1-ρ)`，`y_t` 受永久性和暂时性冲击影响，且存在借贷约束 `a_t >= 0`。

![](img/9ea5c2cb03436ef757c558f33faae725_133.png)

![](img/9ea5c2cb03436ef757c558f33faae725_135.png)

![](img/9ea5c2cb03436ef757c558f33faae725_137.png)

![](img/9ea5c2cb03436ef757c558f33faae725_139.png)

![](img/9ea5c2cb03436ef757c558f33faae725_141.png)

在HARK中，这被定义为`IndShockConsumerType`类，该类的实例则填充具体的参数值（如风险厌恶系数ρ、贴现因子β等）。求解后，我们可以得到主体的**消费函数**。

![](img/9ea5c2cb03436ef757c558f33faae725_143.png)

![](img/9ea5c2cb03436ef757c558f33faae725_145.png)

![](img/9ea5c2cb03436ef757c558f33faae725_146.png)

![](img/9ea5c2cb03436ef757c558f33faae725_148.png)

![](img/9ea5c2cb03436ef757c558f33faae725_150.png)

![](img/9ea5c2cb03436ef757c558f33faae725_152.png)

HARK的强大之处在于其可扩展性。模型树中的每个模型不仅指定了`AgentType`的子类，还指定了其求解器的子类。

![](img/9ea5c2cb03436ef757c558f33faae725_154.png)

![](img/9ea5c2cb03436ef757c558f33faae725_156.png)

![](img/9ea5c2cb03436ef757c558f33faae725_158.png)

![](img/9ea5c2cb03436ef757c558f33faae725_160.png)

![](img/9ea5c2cb03436ef757c558f33faae725_162.png)

以下是展示HARK如何通过简单修改快速构建新模型的例子：

![](img/9ea5c2cb03436ef757c558f33faae725_164.png)

![](img/9ea5c2cb03436ef757c558f33faae725_166.png)

![](img/9ea5c2cb03436ef757c558f33faae725_168.png)

![](img/9ea5c2cb03436ef757c558f33faae725_170.png)

![](img/9ea5c2cb03436ef757c558f33faae725_172.png)

![](img/9ea5c2cb03436ef757c558f33faae725_174.png)

![](img/9ea5c2cb03436ef757c558f33faae725_175.png)

![](img/9ea5c2cb03436ef757c558f33faae725_177.png)

1.  **添加差异化利率**：假设借贷利率高于储蓄利率。在HARK中，这只需修改约8行代码，主要是初始化方法和相关计算。结果消费函数会在零点（即既不储蓄也不借贷的状态）出现**折点**。
2.  **添加偏好冲击**：假设消费的边际效用会随机波动。加入这个特性大约需要20行代码。
3.  **组合特性**：如果想要一个同时包含差异化利率和短期偏好冲击的模型（例如用于研究发薪日贷款），在HARK中变得非常简单。只需创建一个新的求解器类，**同时继承**上述两个特性对应的求解器类即可。

![](img/9ea5c2cb03436ef757c558f33faae725_179.png)

![](img/9ea5c2cb03436ef757c558f33faae725_181.png)

![](img/9ea5c2cb03436ef757c558f33faae725_183.png)

![](img/9ea5c2cb03436ef757c558f33faae725_184.png)

![](img/9ea5c2cb03436ef757c558f33faae725_186.png)

![](img/9ea5c2cb03436ef757c558f33faae725_188.png)

![](img/9ea5c2cb03436ef757c558f33faae725_190.png)

![](img/9ea5c2cb03436ef757c558f33faae725_192.png)

![](img/9ea5c2cb03436ef757c558f33faae725_194.png)

**代码示例（概念性）**：
```python
# 继承自差异化利率求解器和偏好冲击求解器
class NewSolver(DiffInterestSolver, PrefShockSolver):
    def __init__(self, **kwargs):
        # 初始化两个父类的属性
        DiffInterestSolver.__init__(self, **some_args)
        PrefShockSolver.__init__(self, **other_args)
        # ... 可能的其他初始化
```
通过这种多重继承，新的复杂模型可以迅速从现有模块中构建出来。

![](img/9ea5c2cb03436ef757c558f33faae725_196.png)

![](img/9ea5c2cb03436ef757c558f33faae725_198.png)

![](img/9ea5c2cb03436ef757c558f33faae725_200.png)

## 总结与展望

![](img/9ea5c2cb03436ef757c558f33faae725_202.png)

![](img/9ea5c2cb03436ef757c558f33faae725_204.png)

![](img/9ea5c2cb03436ef757c558f33faae725_206.png)

![](img/9ea5c2cb03436ef757c558f33faae725_208.png)

![](img/9ea5c2cb03436ef757c558f33faae725_210.png)

本节课中，我们一起学习了异质主体建模的概念、传统方法的不足，以及HARK工具包如何通过其面向对象、模块化的设计来应对这些挑战。HARK使得快速构建、修改和组合复杂的经济模型成为可能，旨在提高研究效率、可重复性和可比较性。

![](img/9ea5c2cb03436ef757c558f33faae725_212.png)

![](img/9ea5c2cb03436ef757c558f33faae725_214.png)

![](img/9ea5c2cb03436ef757c558f33faae725_216.png)

![](img/9ea5c2cb03436ef757c558f33faae725_218.png)

![](img/9ea5c2cb03436ef757c558f33faae725_220.png)

HARK只是Econ-ARC宏伟蓝图的第一块基石。该组织希望将此类框架扩展到经济学的其他子领域，如货币经济学、农业与应用经济学等，并得到了包括NumFOCUS在内的多家机构支持。最终目标是建立一个开放、协作的计算经济学生态系统，让研究者能更专注于经济思想本身，而非重复的基础设施建设。