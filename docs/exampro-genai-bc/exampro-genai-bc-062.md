# 62：视觉小说UI滑块实现教程

![](img/02e91e1e898b239782016249d85ace35_1.png)

![](img/02e91e1e898b239782016249d85ace35_3.png)

![](img/02e91e1e898b239782016249d85ace35_5.png)

## 概述

在本节课中，我们将学习如何为视觉小说游戏创建一个设置页面，并重点实现一个可交互的音量滑块UI组件。我们将从现有按钮功能验证开始，逐步构建设置页面的UI元素，包括背景面板、按钮以及核心的音量滑块。

---

## 验证现有按钮功能

![](img/02e91e1e898b239782016249d85ace35_7.png)

上一节我们完成了基础按钮的创建。现在，我们需要验证按钮的事件发射功能是否正常工作。

以下是验证步骤：
*   点击游戏界面中的“Continue game”或“Settings”按钮。
*   观察浏览器控制台是否打印出相应的日志信息（例如“Settings”）。
*   确认事件能够被正确触发和捕获。

![](img/02e91e1e898b239782016249d85ace35_1.png)

如图所示，按钮点击事件已成功触发并打印日志，这证明我们的基础事件系统运行良好。接下来，我们将专注于构建设置页面。

---

## 规划设置页面UI结构

设置页面将作为一个新的UI层，覆盖在现有游戏界面上方。我们不需要切换整个游戏场景，只需创建并显示这个UI层。

首先，我们需要分析设置页面包含哪些配置项。参考配置文件，我们可能需要以下控件：
*   背景音乐音量滑块
*   音效音量滑块
*   字体大小滑块
*   打字机速度滑块
*   自动播放速度滑块
*   语言切换（下拉菜单或按钮）
*   应用/取消按钮

![](img/02e91e1e898b239782016249d85ace35_9.png)

在本教程中，我们将首先实现背景音乐音量滑块。

在代码的 `create` 函数中，我们规划了以下创建步骤：
```javascript
this.createBackground() // 创建背景面板
this.createBgmVolumeSlider() // 创建背景音乐音量滑块
this.createSfxVolumeSlider() // 创建音效音量滑块
this.createFontSlider() // 创建字体滑块
this.createActions() // 创建操作按钮（应用/取消）
```

![](img/02e91e1e898b239782016249d85ace35_11.png)

---

## 创建设置页面的基础框架

首先，我们需要创建设置页面的容器和背景，使其能够覆盖整个屏幕。

![](img/02e91e1e898b239782016249d85ace35_13.png)

在 `SettingsUI.js` 的 `createBackground` 方法中，我们创建一个矩形作为背景：
```javascript
createBackground() {
    const width = this.scene.cameras.main.width;
    const height = this.scene.cameras.main.height;
    this.bg = this.scene.add.rectangle(0, 0, width, height, 0x222222);
    this.bg.setOrigin(0, 0); // 将原点设置为左上角，确保覆盖全屏
    this.bg.setAlpha(0.9); // 设置一定透明度
}
```
关键点在于通过 `scene.cameras.main` 获取游戏视口的宽度和高度，并设置矩形的原点 `(0,0)` 到左上角，从而确保背景覆盖整个屏幕。

![](img/02e91e1e898b239782016249d85ace35_15.png)

![](img/02e91e1e898b239782016249d85ace35_17.png)

![](img/02e91e1e898b239782016249d85ace35_19.png)

![](img/02e91e1e898b239782016249d85ace35_46.png)

![](img/02e91e1e898b239782016249d85ace35_21.png)

![](img/02e91e1e898b239782016249d85ace35_23.png)

![](img/02e91e1e898b239782016249d85ace35_25.png)

如图所示，灰色半透明背景已成功覆盖全屏。接下来，我们创建操作按钮。

---

![](img/02e91e1e898b239782016249d85ace35_27.png)

## 添加应用与取消按钮

![](img/02e91e1e898b239782016249d85ace35_29.png)

![](img/02e91e1e898b239782016249d85ace35_30.png)

设置页面需要“应用”和“取消”按钮来确认或放弃更改。我们可以复用之前创建的按钮组件。

在 `createActions` 方法中，我们创建两个按钮：
```javascript
createActions() {
    const buttonWidth = 300;
    const buttonHeight = 80;

    // 创建应用按钮
    this.applyButton = this.scene.uiManager.createButton({
        x: 100,
        y: 100,
        width: buttonWidth,
        height: buttonHeight,
        text: 'Apply',
        name: 'settings_apply'
    });

    // 创建取消按钮
    this.cancelButton = this.scene.uiManager.createButton({
        x: 100,
        y: 200, // 放在应用按钮下方
        width: buttonWidth,
        height: buttonHeight,
        text: 'Cancel',
        name: 'settings_cancel'
    });
}
```
按钮的位置是临时的，后续需要进行布局调整。现在，我们开始实现本节课的核心——可拖动的滑块组件。

![](img/02e91e1e898b239782016249d85ace35_32.png)

![](img/02e91e1e898b239782016249d85ace35_34.png)

![](img/02e91e1e898b239782016249d85ace35_36.png)

---

![](img/02e91e1e898b239782016249d85ace35_38.png)

## 实现UI滑块组件

![](img/02e91e1e898b239782016249d85ace35_40.png)

![](img/02e91e1e898b239782016249d85ace35_42.png)

![](img/02e91e1e898b239782016249d85ace35_44.png)

滑块是设置页面的核心交互元素。我们需要创建一个通用的 `UISlider` 类，它可以被复用于音量、字体大小等设置。

![](img/02e91e1e898b239782016249d85ace35_46.png)

![](img/02e91e1e898b239782016249d85ace35_48.png)

![](img/02e91e1e898b239782016249d85ace35_49.png)

我们创建了一个新文件 `UISlider.js`。以下是该组件的核心结构：
```javascript
class UISlider {
    constructor(scene, config) {
        this.scene = scene;
        this.x = config.x;
        this.y = config.y;
        this.width = config.width || 380;
        this.min = config.min || 0;
        this.max = config.max || 100;
        this.value = config.value || this.min;
        this.name = config.name; // 例如 “bgm_volume”

        this.createTrack();
        this.createHandle();
        this.setupDrag();
        this.updateDisplay();
    }

    createTrack() {
        // 创建滑块的轨道（一条线或矩形）
        this.track = this.scene.add.rectangle(this.x, this.y, this.width, 6, 0x666666);
    }

    createHandle() {
        // 创建滑块的拖动柄
        this.handle = this.scene.add.circle(this.x, this.y, 10, 0xffffff);
        this.handle.setInteractive({ draggable: true });
    }

    setupDrag() {
        // 设置拖动柄的拖拽事件
        this.scene.input.setDraggable(this.handle);
        this.handle.on('drag', (pointer, dragX, dragY) => {
            // 限制拖动柄在轨道范围内移动
            const newX = Phaser.Math.Clamp(dragX, this.x - this.width/2, this.x + this.width/2);
            this.handle.x = newX;

            // 根据柄的位置计算当前值
            const percent = (newX - (this.x - this.width/2)) / this.width;
            this.value = this.min + percent * (this.max - this.min);

            // 触发值改变事件
            this.scene.events.emit(`ui_slider_${this.name}_changed`, this.value);
            this.updateDisplay();
        });
    }

    updateDisplay() {
        // 更新滑块上的数值标签（可选）
        if (this.valueText) this.valueText.destroy();
        this.valueText = this.scene.add.text(this.x + this.width/2 + 20, this.y, Math.round(this.value).toString(), { fill: '#fff' });
    }
}
```
这个类封装了滑道的创建、拖拽柄的交互、数值计算与事件发射。关键逻辑在于 `setupDrag` 方法，它将像素位置映射到配置的数值范围 `[min, max]` 内，并在值变化时发出事件。

![](img/02e91e1e898b239782016249d85ace35_51.png)

![](img/02e91e1e898b239782016249d85ace35_53.png)

![](img/02e91e1e898b239782016249d85ace35_54.png)

![](img/02e91e1e898b239782016249d85ace35_56.png)

![](img/02e91e1e898b239782016249d85ace35_66.png)

![](img/02e91e1e898b239782016249d85ace35_58.png)

![](img/02e91e1e898b239782016249d85ace35_60.png)

如图所示，滑块组件已成功渲染到屏幕上。接下来，我们需要在设置页面中使用这个滑块。

---

## 在设置页面中集成音量滑块

现在，我们在 `SettingsUI.js` 中创建背景音乐音量滑块，并将其与游戏的音频管理系统连接起来。

![](img/02e91e1e898b239782016249d85ace35_62.png)

![](img/02e91e1e898b239782016249d85ace35_64.png)

在 `createBgmVolumeSlider` 方法中：
```javascript
createBgmVolumeSlider() {
    // 1. 从游戏设置中获取当前音量值
    const currentVolume = this.scene.game.settings.get('bgmVolume');
    // 假设设置中存储的是0.0到1.0的小数，而滑块显示0到100
    const sliderValue = currentVolume * 100;

    // 2. 创建滑块实例
    this.bgmSlider = new UISlider(this.scene, {
        x: 200,
        y: 300,
        width: 400,
        min: 0,
        max: 100,
        value: sliderValue,
        name: 'bgm_volume'
    });

    // 3. 监听滑块值变化事件
    this.scene.events.on(`ui_slider_bgm_volume_changed`, (newValue) => {
        // 将滑块值（0-100）转换回音频系统所需的小数值（0.0-1.0）
        const audioVolume = newValue / 100;
        // 实时调整音频音量（预览效果）
        this.scene.game.audio.setBgmVolume(audioVolume);
    });
}
```
这里有一个重要的映射关系：游戏设置中音量通常存储为 `0.0` 到 `1.0` 的小数，而滑块为了用户友好，显示为 `0` 到 `100` 的整数。因此需要在两者之间进行转换。

![](img/02e91e1e898b239782016249d85ace35_88.png)

![](img/02e91e1e898b239782016249d85ace35_66.png)

![](img/02e91e1e898b239782016249d85ace35_68.png)

当拖动滑块时，游戏背景音乐的音量会实时改变。然而，目前的变化只是临时的预览，点击“应用”按钮后才会永久保存到设置中。

![](img/02e91e1e898b239782016249d85ace35_70.png)

---

![](img/02e91e1e898b239782016249d85ace35_72.png)

## 处理滑块值的范围映射与持久化

![](img/02e91e1e898b239782016249d85ace35_74.png)

![](img/02e91e1e898b239782016249d85ace35_76.png)

在实际测试中，我们发现音量范围 `0-100` 对应的实际声音变化并不线性，且最大值 `100` 对应的音量过大。我们需要调整滑块值与实际音频增益之间的映射关系。

![](img/02e91e1e898b239782016249d85ace35_78.png)

假设我们希望滑块范围 `0-100` 映射到更合理的音频增益范围 `0.0 - 0.2`。这需要在事件处理中进行数学映射：
```javascript
this.scene.events.on(`ui_slider_bgm_volume_changed`, (newValue) => {
    // 线性映射：将滑块值从 [0, 100] 映射到音频增益 [0.0, 0.2]
    const audioGain = (newValue / 100) * 0.2;
    this.scene.game.audio.setBgmVolume(audioGain);
});
```
同时，在初始化滑块时，需要进行反向映射，将存储的音频增益值转换为滑块显示值：
```javascript
const currentGain = this.scene.game.settings.get('bgmVolume'); // 例如 0.1
const sliderValue = (currentGain / 0.2) * 100; // 映射回 0-100 范围，例如 50
```

![](img/02e91e1e898b239782016249d85ace35_80.png)

![](img/02e91e1e898b239782016249d85ace35_82.png)

![](img/02e91e1e898b239782016249d85ace35_84.png)

![](img/02e91e1e898b239782016249d85ace35_94.png)

![](img/02e91e1e898b239782016249d85ace35_86.png)

经过范围映射调整后，滑块的拖动体验和音量变化更加符合预期。最后，我们需要通过“应用”按钮将最终的滑块值保存到游戏设置中。

![](img/02e91e1e898b239782016249d85ace35_88.png)

---

## 连接应用按钮与设置保存

“应用”按钮的职责是捕获当前所有UI控件（如滑块）的值，并将其持久化到游戏设置中。

我们为“应用”按钮添加点击事件：
```javascript
// 在 createActions 方法中，为应用按钮添加事件
this.applyButton.on('click', () => {
    // 1. 获取滑块当前值
    const finalSliderValue = this.bgmSlider.value; // 0-100

    // 2. 映射为实际设置值并保存
    const finalBgmGain = (finalSliderValue / 100) * 0.2;
    this.scene.game.settings.set('bgmVolume', finalBgmGain);

    // 3. （可选）立即应用设置到音频系统
    this.scene.game.audio.setBgmVolume(finalBgmGain);

    // 4. 隐藏设置页面
    this.hide();
});
```
“取消”按钮的逻辑更简单，只需丢弃未保存的更改并隐藏页面：
```javascript
this.cancelButton.on('click', () => {
    // 恢复为原来的音量设置（从持久化设置中重新读取并应用）
    const originalVolume = this.scene.game.settings.get('bgmVolume');
    this.scene.game.audio.setBgmVolume(originalVolume);
    this.hide();
});
```

![](img/02e91e1e898b239782016249d85ace35_90.png)

---

![](img/02e91e1e898b239782016249d85ace35_92.png)

## 总结

![](img/02e91e1e898b239782016249d85ace35_94.png)

本节课中我们一起学习了如何为视觉小说游戏构建一个交互式的设置页面，并重点实现了一个功能完整的UI滑块组件。我们完成了以下工作：

1.  **验证了基础事件系统**，确保按钮交互正常。
2.  **规划并创建了设置页面的UI框架**，包括全屏背景和操作按钮。
3.  **设计并实现了可重用的 `UISlider` 类**，它封装了轨道、拖拽柄、数值计算和事件发射。
4.  **将滑块集成到设置页面**，用于控制背景音乐音量，并处理了数值范围映射。
5.  **连接了UI与游戏系统**，实现了音量的实时预览和通过“应用/取消”按钮进行持久化保存。

目前，我们实现了最核心的音量滑块。在接下来的课程中，我们将基于相同的模式，快速创建其他设置滑块（如音效、字体大小），并实现切换按钮（Toggle）用于布尔类型的设置项，最终完成一个功能全面的游戏设置界面。