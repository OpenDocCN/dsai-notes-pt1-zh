# 61：视觉小说UI管理器与事件总线

## 概述
在本节课中，我们将学习如何为视觉小说游戏重构UI系统，并实现一个事件总线（EventBus）来管理组件间的通信。我们将把杂乱的UI代码模块化，创建可复用的UI组件，并建立一个清晰的事件驱动架构。

---

![](img/3af1cad902b1cf76c2483d395ed4a853_1.png)

## 重构UI管理器与按钮系统

![](img/3af1cad902b1cf76c2483d395ed4a853_3.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_4.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_6.png)

上一节我们处理了游戏设置和保存系统。本节中，我们来看看如何优化游戏的用户界面。

![](img/3af1cad902b1cf76c2483d395ed4a853_8.png)

目前，我们的UI代码与游戏场景逻辑紧密耦合，这导致代码难以维护和扩展。我们的目标是创建一个独立的UI管理器（UI Manager）和可复用的UI组件。

![](img/3af1cad902b1cf76c2483d395ed4a853_10.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_12.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_13.png)

### 创建UI管理器
首先，我们在全局管理器（Global Manager）中初始化一个UI管理器实例。

![](img/3af1cad902b1cf76c2483d395ed4a853_15.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_17.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_19.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_21.png)

```javascript
// 在全局管理器中
this.uiManager = new UIManager();
```

![](img/3af1cad902b1cf76c2483d395ed4a853_23.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_25.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_27.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_29.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_30.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_32.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_33.png)

UI管理器将负责提供创建UI元素的通用方法和默认样式，使得在不同场景中创建一致的UI变得更加容易。

![](img/3af1cad902b1cf76c2483d395ed4a853_35.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_37.png)

### 设计可复用的UI按钮组件
原始的按钮创建代码分散在菜单场景中，包含了大量重复的设置（如尺寸、位置、纹理、交互事件）。我们将其提取为一个独立的`UIButton`类。

![](img/3af1cad902b1cf76c2483d395ed4a853_39.png)

以下是`UIButton`类构造函数的基本结构：
```javascript
class UIButton {
    constructor(scene, options) {
        this.scene = scene;
        this.options = options;
        this.validateOptions();
        this.createImage();
        this.createText();
        this.setupInteractivity();
    }
    // ... 其他方法（验证选项、创建图像/文本、设置交互性）
}
```

![](img/3af1cad902b1cf76c2483d395ed4a853_41.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_42.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_44.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_46.png)

这个类接收一个场景引用和一个配置选项对象，然后自动创建按钮的视觉元素并设置交互逻辑。

### 重构菜单UI
我们将菜单场景中的按钮创建逻辑移到一个新的`MenuUI`类中。这个类利用UI管理器和`UIButton`类来清晰地定义每个按钮。

以下是定义按钮数据并创建按钮的示例：
```javascript
createButtons() {
    const buttonData = [
        { text: '新游戏', eventHandler: 'new_game' },
        { text: '继续', eventHandler: 'continue' },
        { text: '设置', eventHandler: 'open_settings' },
        { text: '加载', eventHandler: 'load' }
    ];

    buttonData.forEach((data, index) => {
        this.createButton(index, data.text, data.eventHandler);
    });
}
```

![](img/3af1cad902b1cf76c2483d395ed4a853_48.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_50.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_52.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_54.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_56.png)

这种方法使得添加、移除或调整按钮变得非常简单，所有布局逻辑都集中在同一个地方。

---

## 实现事件总线（EventBus）

上一节我们创建了结构清晰的UI组件。本节中，我们来看看如何让这些组件与游戏的其他部分进行通信。

直接调用函数会导致组件间紧密耦合。我们引入一个事件总线作为中央通信枢纽，组件可以发出（emit）事件或监听（on）其他组件发出的事件，而无需直接引用对方。

![](img/3af1cad902b1cf76c2483d395ed4a853_58.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_60.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_62.png)

### 事件总线的工作原理
事件总线是一个发布-订阅模式的实现。其核心概念非常简单：
*   **发布（Emit）**： 当一个事件发生时（例如按钮被点击），组件会向事件总线“发出”一个带有名称和可选数据的事件。
*   **订阅（On）**： 游戏的其他部分可以“监听”特定名称的事件。当该事件被发出时，所有监听它的回调函数都会被触发。

### 集成事件总线到UI按钮
在`UIButton`类中，当发生交互（如点击）时，我们不再直接调用游戏逻辑，而是通过事件总线发出一个事件。

```javascript
// 在UIButton的setupInteractivity方法中
this.image.on('pointerdown', () => {
    this.scene.game.globalManager.eventBus.emit(`ui_button_${this.options.eventHandler}`, { source: this });
});
```

![](img/3af1cad902b1cf76c2483d395ed4a853_64.png)

### 在全局管理器中监听事件
然后，在游戏的核心逻辑（例如全局管理器）中，我们监听这些事件并执行相应的操作。

```javascript
// 在全局管理器的初始化中
this.eventBus.on('ui_button_new_game', (eventData) => {
    console.log('新游戏按钮被点击！', eventData);
    this.startNewGame();
});

this.eventBus.on('ui_button_open_settings', (eventData) => {
    console.log('设置按钮被点击！', eventData);
    this.openSettingsScreen();
});
```

![](img/3af1cad902b1cf76c2483d395ed4a853_66.png)

---

![](img/3af1cad902b1cf76c2483d395ed4a853_68.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_70.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_72.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_74.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_76.png)

## 调试与验证

![](img/3af1cad902b1cf76c2483d395ed4a853_78.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_80.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_82.png)

在实现新系统后，进行测试至关重要。我们通过检查控制台日志来验证事件是否被正确发出和接收。

![](img/3af1cad902b1cf76c2483d395ed4a853_84.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_86.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_88.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_89.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_91.png)

1.  确保`UIButton`发出的事件名称与全局管理器中监听的名称完全匹配。
2.  点击游戏菜单中的按钮，观察控制台是否有对应的日志输出。
3.  验证点击按钮后，预期的游戏功能（如开始新游戏）是否被正确触发。

![](img/3af1cad902b1cf76c2483d395ed4a853_93.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_95.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_96.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_98.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_100.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_102.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_104.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_106.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_108.png)

通过这种方式，我们确认了事件总线正在有效地连接UI层与游戏逻辑层。

---

![](img/3af1cad902b1cf76c2483d395ed4a853_110.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_112.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_114.png)

## 总结

![](img/3af1cad902b1cf76c2483d395ed4a853_116.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_117.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_119.png)

本节课中我们一起学习了如何重构视觉小说游戏的UI系统并实现事件总线。

![](img/3af1cad902b1cf76c2483d395ed4a853_120.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_121.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_122.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_124.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_125.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_127.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_129.png)

我们首先将混乱的UI代码重构为模块化的结构，创建了`UIManager`和可复用的`UIButton`组件，使得UI创建变得清晰且易于维护。

![](img/3af1cad902b1cf76c2483d395ed4a853_131.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_133.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_135.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_137.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_139.png)

接着，我们引入了**事件总线（EventBus）** 这一核心概念，用它来解耦UI组件与游戏逻辑。组件通过发出事件来通信，其他部分通过监听事件来响应，这大大提高了代码的灵活性和可维护性。

![](img/3af1cad902b1cf76c2483d395ed4a853_141.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_142.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_143.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_145.png)

![](img/3af1cad902b1cf76c2483d395ed4a853_147.png)

最终，我们建立了一个坚实的架构基础，使得后续添加游戏设置界面、对话系统或其他复杂功能变得更加容易。在下一节课中，我们将利用这个新架构来实现游戏的设置页面。