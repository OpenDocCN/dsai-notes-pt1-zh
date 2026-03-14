# 60：修复全局管理器问题 🛠️

![](img/4868cc9dc52af69cc45e18095c137e1c_1.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_2.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_4.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_5.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_6.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_8.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_10.png)

在本节课中，我们将学习如何修复一个视觉小说游戏中的全局管理器问题。具体来说，我们将解决背景音乐在多个场景间重复播放、音量控制失效以及代码结构混乱的问题。我们将通过重构代码，引入全局管理器系统来统一管理音频、设置和存档等功能。

![](img/4868cc9dc52af69cc45e18095c137e1c_12.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_14.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_16.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_18.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_19.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_21.png)

---

![](img/4868cc9dc52af69cc45e18095c137e1c_23.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_25.png)

上一节我们遇到了游戏场景切换时背景音乐管理混乱的问题。本节中，我们来看看如何通过重构代码，建立一个清晰的全局管理器系统来解决这些问题。

## 问题诊断与初步修复

在游戏运行时，点击菜单按钮时背景音乐音量异常增大，并且可能被播放两次。经过检查，发现问题源于音频管理器的初始化逻辑分散在各个场景中，缺乏统一的全局状态管理。

以下是初始问题代码的示例，音乐在`MenuScene`中被直接调用场景的`sound`对象播放：
```javascript
// 问题代码示例：在MenuScene中直接播放音乐
this.sound.add('bgMusic', { loop: true, volume: 0.1 }).play();
```

为了解决这个问题，我们决定创建一个`GlobalManager`类，作为所有需要跨场景共享的管理器（如音频、设置、存档）的中央枢纽。

## 创建全局管理器系统

![](img/4868cc9dc52af69cc45e18095c137e1c_27.png)

首先，我们创建`GlobalManager`类。它的核心职责是初始化并持有对其他管理器的引用，确保它们在游戏生命周期内是单例且可全局访问的。

![](img/4868cc9dc52af69cc45e18095c137e1c_29.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_31.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_33.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_35.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_36.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_38.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_40.png)

```javascript
// GlobalManager 类
class GlobalManager {
    constructor() {
        // 初始化事件总线
        if (!window.eventBus) {
            window.eventBus = new Phaser.Events.EventEmitter();
        }
        this.eventBus = window.eventBus;

        // 初始化各个管理器
        this.settings = new SettingsManager(this);
        this.audio = new AudioManager(this);
        this.save = new SaveManager(this);

        // 将实例存储在全局注册表和window对象上，以便访问
        this.game.registry.set('globalManagers', this);
        window.G = this; // 简写，方便调用
    }

    updateScene(scene) {
        // 为各个管理器更新当前场景引用
        this.audio.scene = scene;
        this.settings.scene = scene;
        this.save.scene = scene;
    }
}
```

![](img/4868cc9dc52af69cc45e18095c137e1c_42.png)

## 重构场景初始化流程

![](img/4868cc9dc52af69cc45e18095c137e1c_44.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_46.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_48.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_50.png)

接下来，我们需要修改游戏的启动流程，确保`GlobalManager`在游戏早期就被创建。

![](img/4868cc9dc52af69cc45e18095c137e1c_52.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_54.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_56.png)

1.  **在BootScene中初始化**：在游戏启动的第一个场景中创建`GlobalManager`实例，并将其存入Phaser的注册表（Registry）。
    ```javascript
    // BootScene.js
    create() {
        // ... 其他初始化代码
        const globalManagers = new GlobalManager(this);
        this.registry.set('globalManagers', globalManagers);
        this.scene.start('PreloadScene');
    }
    ```

![](img/4868cc9dc52af69cc45e18095c137e1c_58.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_60.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_62.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_64.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_66.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_68.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_70.png)

2.  **在PreloadScene中完成初始化**：在资源加载完成后，从注册表中获取`GlobalManager`实例，并调用其完成初始化的方法（例如，让音频管理器加载具体的音效资源）。
    ```javascript
    // PreloadScene.js
    create() {
        // 资源加载完成...
        const g = this.registry.get('globalManagers');
        g.audio.createBGM(); // 创建背景音乐对象
        // ... 进入下一个场景
    }
    ```

![](img/4868cc9dc52af69cc45e18095c137e1c_72.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_74.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_76.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_78.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_80.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_82.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_84.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_86.png)

3.  **在MenuScene中使用全局管理器**：在菜单场景中，我们从注册表获取管理器，并调用其方法来播放音乐，而不是直接操作场景的音频系统。
    ```javascript
    // MenuScene.js
    create() {
        const g = this.registry.get('globalManagers');
        // 更新管理器中的场景引用
        g.updateScene(this);
        // 使用全局音频管理器播放音乐
        if (!g.audio.isBGMPlaying()) {
            g.audio.playBGM();
        }
        // ... 创建菜单按钮等
    }
    ```

## 重构音频管理器

原有的`AudioManager`严重依赖于特定的游戏场景。我们将其重构，使其接收`GlobalManager`作为依赖，并通过`updateScene`方法来动态获取当前场景的上下文。

核心改动包括：
*   将背景音乐对象`bgm`的创建与播放分离。
*   所有音频操作都通过`this.scene.sound`进行，但`this.scene`由全局管理器动态注入。
*   音量设置从全局的`SettingsManager`中读取，确保一致性。

以下是重构后的`AudioManager`关键方法：
```javascript
class AudioManager {
    constructor(globalManager) {
        this.g = globalManager; // 保存对全局管理器的引用
        this.scene = null; // 当前场景，由updateScene设置
        this.bgm = null;
    }

    updateScene(scene) {
        this.scene = scene;
    }

    createBGM() {
        // 从全局设置中获取音量
        const volume = this.g.settings.get('bgmVolume') || 0.1;
        this.bgm = this.scene.sound.add('bgMusic', { loop: true, volume: volume });
    }

    playBGM() {
        if (this.bgm && !this.bgm.isPlaying) {
            this.bgm.play();
        }
    }

    setBGMVolume(volume) {
        if (this.bgm) {
            this.bgm.setVolume(volume);
        }
    }
}
```

![](img/4868cc9dc52af69cc45e18095c137e1c_88.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_90.png)

## 整理项目结构

随着管理器增多，项目文件变得混乱。我们进行了以下整理：
*   创建`/managers`文件夹，将`AudioManager`、`SettingsManager`、`SaveManager`移入其中。
*   在`index.html`中调整脚本加载顺序，确保依赖关系正确（例如，基础工具类先加载，管理器随后，最后是场景脚本）。
*   移除了分散在各处的默认配置代码，将游戏设置的唯一数据源集中到`SettingsManager`中。

![](img/4868cc9dc52af69cc45e18095c137e1c_92.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_93.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_95.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_97.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_99.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_101.png)

## 测试与验证

![](img/4868cc9dc52af69cc45e18095c137e1c_103.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_104.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_106.png)

完成上述重构后，我们重启游戏进行测试：
1.  游戏启动，加载界面正常。
2.  进入主菜单，背景音乐以正常音量播放，且只播放一次。
3.  点击“设置”或“新游戏”按钮，不再有错误发生。
4.  音乐播放状态和音量控制通过全局管理器统一管理，为后续添加更多场景打下了坚实基础。

![](img/4868cc9dc52af69cc45e18095c137e1c_108.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_110.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_112.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_114.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_116.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_118.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_119.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_121.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_122.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_123.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_125.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_127.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_129.png)

---

![](img/4868cc9dc52af69cc45e18095c137e1c_131.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_133.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_135.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_137.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_139.png)

![](img/4868cc9dc52af69cc45e18095c137e1c_141.png)

本节课中我们一起学习了如何诊断和修复跨场景状态管理的问题。通过创建`GlobalManager`作为中央协调者，我们统一了音频、设置等核心功能的管理方式，解决了音乐重复播放和音量失控的问题，并使代码结构更加清晰、易于维护。这个过程也展示了在AI辅助编程时，当代码复杂度增加时，开发者需要深入理解系统设计，并亲自进行关键的重构决策。