# 64：视觉小说场景与事件总线 🎮

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_1.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_3.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_4.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_6.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_8.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_10.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_11.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_12.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_14.png)

在本节课中，我们将学习如何为视觉小说游戏实现场景切换功能，并解决游戏内事件管理混乱的问题。我们将创建一个新的游戏场景UI，并重构事件总线系统，以确保不同场景间的事件能够正确注册和注销。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_16.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_17.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_19.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_21.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_23.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_25.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_27.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_29.png)

---

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_31.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_33.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_35.png)

## 场景切换与音频问题 🎵

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_37.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_39.png)

上一节我们完成了设置菜单。现在，我们需要让“新游戏”按钮能够切换到游戏主场景。

我们首先尝试使用Phaser的`scene.start`方法来切换场景。在全局管理器中，我们调用`this.scene.start('game')`来启动名为“game”的场景。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_41.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_42.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_43.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_45.png)

```javascript
// 在菜单场景中启动游戏场景
this.scene.start('game');
```

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_47.png)

然而，我们遇到了一个问题：背景音乐在场景切换后重复播放。这是因为当新场景（游戏场景）启动时，它也会执行创建背景音乐的代码，而旧场景（菜单场景）的音乐并未自动停止。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_49.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_51.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_53.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_55.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_57.png)

**解决方案**是：在游戏场景的创建函数中，暂时注释掉创建背景音乐的代码，或者确保在创建新音频前停止旧的音频。这揭示了Phaser的一个特性：场景切换时，前一个场景的资源（如音频）不会自动停止，除非显式设置或进行管理。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_59.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_61.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_63.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_65.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_67.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_69.png)

---

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_71.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_73.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_75.png)

## 创建游戏场景UI 🖥️

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_77.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_78.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_80.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_82.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_84.png)

成功切换到游戏场景后，我们需要为该场景创建一个用户界面。这个UI将包含设置、加载、快速保存等操作按钮。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_86.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_87.png)

我们创建一个新的UI组件类：`GameUIActions`。这个类继承自我们之前创建的`BaseUIActions`基类，用于管理游戏内的操作按钮。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_89.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_91.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_93.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_95.png)

以下是`GameUIActions`类的核心结构：

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_97.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_99.png)

```javascript
class GameUIActions extends BaseUIActions {
    constructor(scene, x, y) {
        super(scene);
        this.x = x;
        this.y = y;
        this.create();
    }

    create() {
        // 创建水平排列的按钮容器
        const buttonData = [
            { text: '设置', event: 'gm-settings' },
            { text: '加载', event: 'gm-load' },
            { text: '快速保存', event: 'gm-quicksave' },
            { text: '保存', event: 'gm-save' }
        ];
        // 遍历按钮数据并创建按钮
        buttonData.forEach(data => {
            this.createButton(data.text, data.event);
        });
    }
}
```

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_101.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_103.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_105.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_107.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_109.png)

在游戏场景（`GameScene`）中，我们需要实例化这个`GameUIActions`组件并显示它。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_111.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_113.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_115.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_117.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_119.png)

```javascript
// 在GameScene的create方法中
create() {
    // ... 其他初始化代码 ...
    this.uiGameActions = new GameUIActions(this, this.cameras.main.width, 0);
    this.uiGameActions.show();
}
```

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_121.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_123.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_125.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_126.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_128.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_130.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_132.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_134.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_135.png)

---

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_137.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_139.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_141.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_143.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_145.png)

## 事件总线的重构与问题 🔄

在整合UI和事件时，我们遇到了事件管理混乱的问题。事件监听器分散在各个场景和全局管理器中，导致难以维护和潜在的冲突。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_147.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_149.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_151.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_152.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_154.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_156.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_158.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_160.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_162.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_164.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_166.png)

我们的目标是重构事件系统，使其更清晰、易于管理。我们决定创建一个`BaseScene`基类，所有主要场景（如`MenuScene`、`GameScene`）都继承自它。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_168.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_170.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_172.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_174.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_176.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_178.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_180.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_182.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_184.png)

`BaseScene`的核心职责是：
1.  在场景创建时自动注册事件监听器。
2.  在场景关闭或切换时自动注销事件监听器，防止内存泄漏和事件重复触发。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_186.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_187.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_189.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_191.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_193.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_195.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_197.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_199.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_201.png)

以下是`BaseScene`的简化实现：

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_203.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_205.png)

```javascript
class BaseScene extends Phaser.Scene {
    constructor(config) {
        super(config);
    }

    create() {
        // 调用子类的具体创建逻辑
        if (this.onCreate) this.onCreate();
        // 注册事件
        this.registerEvents();
    }

    // 由子类实现具体的事件注册逻辑
    registerEvents() {
        // 示例：this.events.on('some-event', this.handler, this);
    }

    // 由子类实现具体的事件注销逻辑
    deregisterEvents() {
        // 示例：this.events.off('some-event', this.handler, this);
    }

    // 提供一个安全的场景切换方法
    changeScene(key) {
        this.deregisterEvents(); // 切换前注销当前场景的事件
        this.scene.start(key);
    }
}
```

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_207.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_209.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_211.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_213.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_214.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_216.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_218.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_220.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_222.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_223.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_225.png)

然后，我们的`MenuScene`和`GameScene`可以继承`BaseScene`：

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_227.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_229.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_231.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_232.png)

```javascript
class MenuScene extends BaseScene {
    constructor() {
        super({ key: 'menu' });
    }

    onCreate() {
        // 菜单特有的初始化代码
        this.createMenuUI();
    }

    registerEvents() {
        // 注册菜单场景特有的事件，例如“新游戏”
        this.events.on('start-game', this.startGame, this);
    }

    deregisterEvents() {
        // 注销菜单场景的事件
        this.events.off('start-game', this.startGame, this);
    }

    startGame() {
        // 使用基类的方法安全切换场景
        this.changeScene('game');
    }
}
```

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_234.png)

通过这种方式，我们确保了事件监听器的生命周期与场景的生命周期绑定，避免了事件重复监听和无法注销的问题。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_236.png)

---

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_238.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_240.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_242.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_244.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_246.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_248.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_250.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_252.png)

## 整合设置菜单 ⚙️

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_254.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_256.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_258.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_260.png)

最后，我们需要将设置菜单功能整合到游戏场景的UI中。当点击游戏场景UI中的“设置”按钮时，应该能打开之前创建的设置面板。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_262.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_264.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_266.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_268.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_270.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_272.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_274.png)

我们在游戏场景中实例化设置UI，并为其绑定事件。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_276.png)

```javascript
// 在GameScene中
create() {
    // ... 其他代码 ...
    this.uiSettings = new SettingsUI(this, 0, 0);
    this.uiSettings.hide(); // 初始隐藏

    // 监听游戏动作UI发出的打开设置事件
    this.events.on('gm-settings', () => {
        this.uiSettings.show();
        this.uiGameActions.hide(); // 可选：隐藏动作按钮
    });

    // 监听设置UI发出的取消事件
    this.events.on('cancel-settings', () => {
        this.uiSettings.hide();
        this.uiGameActions.show(); // 重新显示动作按钮
    });
}
```

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_278.png)

通过事件总线（`this.events`）进行通信，游戏动作UI和设置UI之间实现了松耦合的交互。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_280.png)

---

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_282.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_284.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_286.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_288.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_290.png)

## 总结 📝

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_292.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_293.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_294.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_296.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_298.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_300.png)

本节课中我们一起学习了：
1.  **场景切换**：使用`this.scene.start(key)`进行场景切换，并注意处理跨场景的音频等资源管理。
2.  **UI组件化**：创建了`GameUIActions`类来管理游戏场景内的操作按钮，提高了代码的可复用性和组织性。
3.  **事件总线重构**：通过创建`BaseScene`基类，将事件监听器的注册和注销逻辑与场景生命周期绑定，解决了事件管理混乱和潜在的内存泄漏问题。
4.  **功能整合**：利用事件总线，将设置菜单无缝集成到游戏场景中，实现了UI组件间的通信。

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_302.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_303.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_305.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_307.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_309.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_311.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_313.png)

![](img/3db46d12bb0cc1019a3b6a2ceb5f22eb_315.png)

通过本节的学习，我们不仅为游戏添加了核心的交互功能，还建立了一个更健壮、可维护的事件驱动架构，为后续开发复杂功能打下了坚实的基础。