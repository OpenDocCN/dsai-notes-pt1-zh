#  008：策略篇 🚀

在本节课中，我们将学习如何将机器学习模型的预测转化为可执行的交易策略。我们将探讨策略的核心组成部分，包括入场/出场信号、仓位管理以及杠杆的使用，并通过代码演示一个基于时间序列预测的基本策略。

---

## 概述

在之前的课程中，我们从基础数据结构开始，逐步构建了时间序列模型。本节课是系列中的一个关键环节，我们将聚焦于**策略**。一个优秀的模型能提供统计优势（预测），但策略决定了我们如何**执行**这个优势。就像拥有一个完美的扑克算牌系统，如果下注策略不当，优势也会荡然无存。因此，策略与模型本身同等重要。

---

## 回顾与准备：构建分类模型

上一节我们学习了分类模型，本节中我们将以此为基础构建策略。首先，我们需要加载并训练一个简单的逻辑回归分类模型，用于预测价格涨跌方向。

以下是加载库和数据的代码：

```python
import pandas as pd
import numpy as np
import torch
import torch.nn as nn
from sklearn.preprocessing import StandardScaler

# 加载数据
data = pd.read_csv('btc_usd_futures.csv', index_col=0, parse_dates=True)
print(data.head())
```

接下来，我们创建模型的特征（滞后收益）和目标（方向标签）：

```python
# 计算对数收益率
data['close_log_return'] = np.log(data['close'] / data['close'].shift(1))

# 创建滞后特征
lags = 3
for lag in range(1, lags+1):
    data[f'lag_{lag}'] = data['close_log_return'].shift(lag)

# 删除包含NaN的行
data.dropna(inplace=True)

# 将收益率编码为方向标签 (1: 上涨, 0: 下跌)
data['direction'] = data['close_log_return'].map(lambda x: 1 if x > 0 else 0)

# 检查标签分布是否平衡
print(data['direction'].value_counts())
```

然后，我们分割数据集并训练模型：

```python
# 按时间顺序分割数据集（避免数据泄露）
split_idx = int(len(data) * 0.8)
train_data = data.iloc[:split_idx].copy()
test_data = data.iloc[split_idx:].copy()

# 定义特征和目标
features = [f'lag_{i}' for i in range(1, lags+1)]
target = 'direction'

# 标准化特征
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(train_data[features])
X_test_scaled = scaler.transform(test_data[features])

# 转换为PyTorch张量
X_train_tensor = torch.tensor(X_train_scaled, dtype=torch.float32)
y_train_tensor = torch.tensor(train_data[target].values, dtype=torch.float32).view(-1, 1)
X_test_tensor = torch.tensor(X_test_scaled, dtype=torch.float32)
y_test_tensor = torch.tensor(test_data[target].values, dtype=torch.float32).view(-1, 1)

# 定义逻辑回归模型
class LogisticRegressionModel(nn.Module):
    def __init__(self, input_dim):
        super().__init__()
        self.linear = nn.Linear(input_dim, 1)
        self.sigmoid = nn.Sigmoid()

    def forward(self, x):
        return self.sigmoid(self.linear(x))

model = LogisticRegressionModel(input_dim=len(features))
criterion = nn.BCELoss()
optimizer = torch.optim.Adam(model.parameters(), lr=0.01)

# 训练模型
epochs = 100
for epoch in range(epochs):
    model.train()
    optimizer.zero_grad()
    outputs = model(X_train_tensor)
    loss = criterion(outputs, y_train_tensor)
    loss.backward()
    optimizer.step()

![](img/ed521e90292db6d6a88e2b60a275d44d_1.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_2.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_3.png)

# 在测试集上评估
model.eval()
with torch.no_grad():
    test_outputs = model(X_test_tensor)
    test_predictions = (test_outputs > 0.5).float()
    accuracy = (test_predictions == y_test_tensor).sum().item() / len(y_test_tensor)
    print(f'测试集方向准确率: {accuracy:.2%}')
```

![](img/ed521e90292db6d6a88e2b60a275d44d_5.png)

现在，我们已经有了一个能提供预测方向的模型。接下来，我们将探讨如何利用这些预测来制定交易策略。

![](img/ed521e90292db6d6a88e2b60a275d44d_7.png)

---

![](img/ed521e90292db6d6a88e2b60a275d44d_9.png)

## 策略核心一：入场与出场信号

![](img/ed521e90292db6d6a88e2b60a275d44d_11.png)

策略的第一步是决定何时开仓（入场）和平仓（出场）。入场和出场信号大致可分为两类：**时间驱动型**和**条件驱动型**。

以下是两种信号类型的简要说明：

![](img/ed521e90292db6d6a88e2b60a275d44d_13.png)

*   **时间驱动型信号**：基于固定的时间间隔进行交易。例如，我们的模型预测下一小时的价格方向，那么我们就在每个小时开始时开仓，并在小时结束时平仓。这种方法与时间序列模型高度契合，能产生大量交易机会，适合高频策略。
*   **条件驱动型信号**：基于特定的市场条件触发交易。例如，当价格偏离移动平均线超过2个标准差时做空，低于2个标准差时做多。这类策略交易频率较低，可能无法充分利用时间序列模型的预测能力。

![](img/ed521e90292db6d6a88e2b60a275d44d_14.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_16.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_18.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_20.png)

对于我们的模型，**时间驱动型信号**是更自然的选择。它允许我们进行高频交易，即使单次胜率优势很小，累积起来也可能产生可观的收益。

---

![](img/ed521e90292db6d6a88e2b60a275d44d_22.png)

## 策略核心二：仓位管理（交易规模）

确定了交易时机，接下来需要决定每次投入多少资金。仓位管理直接关系到策略的盈利能力和风险控制。主要分为静态和动态两种方式。

以下是两种仓位管理方法的对比：

*   **静态仓位**：每次交易投入固定金额（例如，始终交易50美元）。这种方法简单，但不考虑账户资金的增长或回撤。
*   **动态仓位**：根据当前账户净值或其他规则调整交易规模。其中，**复利加仓**是一种常见且强大的动态方法，它将上一笔交易的利润或亏损 reinvest 到下一笔交易中。

**复利加仓**与对数收益率的可加性完美匹配。我们可以通过计算交易对数收益率的累积和来模拟复利增长：

```python
# 假设 `trade_log_returns` 是每笔交易的收益率序列
compound_log_return = trade_log_returns.cumsum()
# 初始资本
initial_capital = 50
equity_curve = initial_capital * np.exp(compound_log_return)
```

与静态仓位相比，在具有统计优势的情况下，复利加仓能显著放大最终收益。

![](img/ed521e90292db6d6a88e2b60a275d44d_24.png)

---

## 策略核心三：杠杆的使用

杠杆允许我们交易比本金更多的资金，从而放大收益。但同时，它也同比例放大了亏损。

使用杠杆的关键公式是调整交易规模：

`实际交易规模 = 计划交易规模 × 杠杆倍数`

在策略中应用杠杆时，需要谨慎选择倍数。过高的杠杆会使策略在很小的价格反向波动下就面临清算风险。杠杆的使用应与策略的夏普比率（风险调整后收益）相匹配，并且**绝对不建议在没有统计优势的情况下使用杠杆**。

---

## 代码实战：构建完整策略

![](img/ed521e90292db6d6a88e2b60a275d44d_26.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_27.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_28.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_30.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_32.png)

现在，让我们将以上概念整合，基于我们的分类模型构建一个简单的策略。我们将使用时间驱动入场、复利加仓，并考虑杠杆。

首先，根据模型预测生成交易信号和计算交易收益率：

![](img/ed521e90292db6d6a88e2b60a275d44d_34.png)

```python
# 在测试集上生成预测
with torch.no_grad():
    probas = model(X_test_tensor).numpy().flatten()
    binary_preds = (probas > 0.5).astype(int)

# 将预测标签 (0,1) 映射为交易方向信号 (1: 做多, -1: 做空)
test_data.loc[:, 'signal'] = np.where(binary_preds == 1, 1, -1)

# 计算单笔交易的理论对数收益率（未考虑手续费）
# 方向正确则收益为正，错误则为负
test_data.loc[:, 'trade_log_return'] = test_data['signal'] * test_data['close_log_return']
```

接着，我们实现一个带杠杆的复利加仓策略：

```python
def run_strategy_with_leverage(trade_returns, initial_capital=50, leverage=1.0):
    """
    运行带杠杆的复利加仓策略。
    trade_returns: 每笔交易的对数收益率序列
    initial_capital: 初始本金
    leverage: 杠杆倍数
    """
    # 计算复利累积对数收益
    compound_log_returns = trade_returns.cumsum()

    # 计算每笔交易前的账户净值（Pre-trade Value）
    # 第一笔交易前为初始本金乘以杠杆
    pre_trade_value = initial_capital * leverage * np.exp(compound_log_returns.shift(1))
    pre_trade_value.iloc[0] = initial_capital * leverage  # 填充第一个NaN

    # 计算每笔交易后的账户净值（Post-trade Value）
    # Post = Pre * exp(单笔收益率)
    post_trade_value = pre_trade_value * np.exp(trade_returns)

    # 计算单笔交易的盈亏（Gross P&L）
    trade_pnl = post_trade_value - pre_trade_value

    # 计算权益曲线（考虑所有交易后的总资产）
    equity_curve = initial_capital * np.exp(compound_log_returns) * leverage

    return pre_trade_value, post_trade_value, trade_pnl, equity_curve

# 运行策略
pre_val, post_val, trade_pnl, equity = run_strategy_with_leverage(
    test_data['trade_log_return'],
    initial_capital=50,
    leverage=2.0  # 使用2倍杠杆
)

print(f"最终账户净值: ${equity.iloc[-1]:.2f}")
print(f"总收益率: {(equity.iloc[-1] / 50 - 1) * 100:.2f}%")
```

![](img/ed521e90292db6d6a88e2b60a275d44d_36.png)

通过对比不同杠杆倍数下的最终收益和回撤，我们可以找到适合该策略的“杠杆甜点区”。

---

![](img/ed521e90292db6d6a88e2b60a275d44d_38.png)

## 总结与核心要点

本节课中，我们一起学习了量化交易策略的核心组成部分：

1.  **入场与出场信号**：我们比较了时间驱动型与条件驱动型信号，并指出时间驱动型信号更适合与时间序列预测模型结合，能产生高频交易机会。
2.  **仓位管理**：我们探讨了静态仓位与动态仓位的区别，并演示了**复利加仓**如何利用对数收益率的可加性来放大策略的长期收益。
3.  **杠杆使用**：我们了解了杠杆是一把双刃剑，它能放大收益，也能加速亏损。杠杆的使用必须与策略的统计优势相匹配，并严格控制风险。

**关键结论**：一个成功的算法交易策略，其总收益 `Alpha` 可以近似看作：

`Alpha ≈ 统计优势 × 执行效率`

而 `执行效率` 又受到 `仓位管理（复利）` 和 `杠杆运用` 的深刻影响。因此，在投入大量精力优化模型的同时，务必投入同等甚至更多的精力去设计和优化你的交易执行策略。

---

## 课后练习

![](img/ed521e90292db6d6a88e2b60a275d44d_40.png)

![](img/ed521e90292db6d6a88e2b60a275d44d_42.png)

1.  **手续费影响分析**：选择一家你熟悉的加密货币交易所，查找其“吃单”（Taker）手续费率。修改策略代码，在计算盈亏时扣除 round-trip（开仓+平仓）手续费。观察这个简单的预测策略在支付手续费后是否仍然盈利。
2.  **做市策略模拟**：研究同一家交易所的“挂单”（Maker）手续费率（通常为负，即返佣）。模拟一个理想情况（订单总能立即成交），计算在获得手续费返佣后，策略的净收益情况。思考在现实中运行做市策略可能面临的挑战（如订单未成交）。
3.  **时间尺度分析**：将原始数据重采样为不同的时间间隔（如4小时、1天、1周），重新训练模型并运行策略。观察交易频率降低后，策略的收益和夏普比率有何变化。思考哪种时间尺度更适合作为“吃单”策略运行。