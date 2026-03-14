# 66：视觉小说对话框教程

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_1.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_2.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_3.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_4.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_6.png)

## 概述
在本节课中，我们将学习如何为视觉小说游戏创建和实现一个功能完整的对话框系统。我们将从加载外部故事数据开始，逐步构建对话框UI，包括对话框背景、角色名称框、对话文本显示以及“下一步”按钮。过程中会涉及数据管理、UI组件定位、文本更新逻辑等核心概念。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_8.png)

---

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_10.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_11.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_12.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_14.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_16.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_18.png)

## 章节 1：项目结构与数据加载

上一节我们完成了游戏基础框架的搭建，本节中我们来看看如何组织项目数据并加载外部故事文件。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_20.png)

为了将游戏数据与代码逻辑分离，我们创建一个独立的数据文件夹来存放故事内容。

1.  在项目根目录下创建一个名为 `stories` 的新文件夹。
2.  在该文件夹内创建一个名为 `main.json` 的JSON文件，用于存放主要的剧情数据。
3.  将之前硬编码在代码中的故事结构（包含章节、场景、对话等）剪切并粘贴到这个JSON文件中。
4.  确保JSON格式有效（使用双引号，而非单引号）。

**核心概念：外部数据加载**
游戏通过预加载（Preload）阶段加载这些外部JSON文件，并为它们分配唯一的标识符（ID），以便在游戏运行时快速访问。

```javascript
// 在预加载器中加载故事数据
this.load.json('story-main', 'assets/data/stories/main.json');
```

---

## 章节 2：重构管理器与数据流

在分离了数据之后，我们需要调整代码中的数据流。原先的 `StoryManager` 和 `DialogueManager` 功能有所重叠。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_22.png)

我们决定简化结构，让 `DialogueManager` 主要负责管理对话状态和流程。`StoryManager` 则专注于更高层次的故事进度和章节切换。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_24.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_26.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_28.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_30.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_32.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_34.png)

1.  修改 `DialogueManager`，使其从全局管理器（`Globals`）中获取必要的引用。
2.  在 `DialogueManager` 中实现 `loadSceneData` 方法，该方法根据场景ID从缓存中获取对应的对话数据。
3.  创建一个 `Mappings` 数据文件（如 `mappings.json`），用于存储角色ID到显示名称的映射关系，实现角色名称的集中管理。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_36.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_38.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_40.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_42.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_44.png)

**核心概念：数据映射**
通过映射文件将程序内部使用的标识符（如 `”alex”`）转换为面向玩家的显示名称（如 `”亚历克斯”`）。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_46.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_48.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_50.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_52.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_54.png)

```json
// mappings.json 示例
{
  "characters": {
    "player": "玩家",
    "alex": "亚历克斯",
    "narrator": "旁白"
  }
}
```

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_56.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_58.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_60.png)

---

## 章节 3：创建基础对话框UI

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_62.png)

现在，我们开始构建用户界面。首先创建对话框的基础视觉组件。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_64.png)

我们将在 `DialogueUI` 类中构建UI。第一步是创建一个位于屏幕底部的对话框背景框。

1.  在 `DialogueUI` 的 `create` 方法中，使用 Phaser 的 `this.add.image` 创建一个图像作为对话框背景。
2.  计算对话框的位置和尺寸，使其水平居中，并紧贴屏幕底部。
3.  使用 `setOrigin` 方法将图像的定位点设置在底部中心（0.5, 1），便于定位。
4.  引入 `margin` 和 `padding` 常量，使UI元素布局更灵活、美观。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_66.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_68.png)

**核心概念：UI定位与原点**
在Phaser中，游戏对象的 `(x, y)` 坐标默认是其图像的中心点。通过 `setOrigin` 可以改变这个参考点，例如设为 `(0, 0)` 代表左上角，`(0.5, 1)` 代表底部中心，这极大简化了布局计算。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_70.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_72.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_74.png)

```javascript
// 创建对话框背景
this.dialogBox = this.add.image(centerX, screenHeight - margin, ‘dialog-box’);
this.dialogBox.setOrigin(0.5, 1); // 将原点设置在底部中心
this.dialogBox.setDisplaySize(screenWidth - margin*2, 300); // 设置显示大小
```

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_76.png)

---

## 章节 4：添加角色名称框与文本

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_78.png)

对话框通常包含一个显示当前说话角色名称的区域。接下来我们实现这个功能。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_80.png)

在对话框背景的上方添加一个角色名称框，并在其中显示角色名。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_82.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_84.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_86.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_88.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_90.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_92.png)

1.  创建另一个图像作为名称框，并将其位置设置为紧贴对话框背景的顶部。
2.  创建一个Phaser文本对象，将其置于名称框内，用于显示角色名称。
3.  在 `DialogueUI` 的 `update` 方法中，实时从 `DialogueManager` 获取当前对话节点的说话者ID。
4.  通过查询 `Mappings` 数据，将说话者ID转换为显示名称，并更新到名称文本对象上。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_94.png)

**核心概念：游戏循环与更新**
`update` 方法在每一帧都会被调用。我们将获取并更新角色名称的逻辑放在这里，确保UI能实时响应游戏状态的变化。

```javascript
update() {
  if (this.dialogueManager.isLoaded()) {
    const speakerId = this.dialogueManager.currentNode.speakerId;
    const speakerName = this.mappings.characters[speakerId] || speakerId;
    this.nameText.setText(speakerName);
  }
}
```

---

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_96.png)

## 章节 5：实现对话文本显示

对话框的核心是显示对话文本。我们将实现同时显示日文原文和英文翻译的功能。

在对话框背景区域内创建两个文本区域，分别用于显示日文和英文。

1.  创建 `japaneseText` 和 `englishText` 两个Phaser文本对象。
2.  设置它们的宽度，使其在对话框的 `padding` 区域内自动换行。
3.  定位日文文本在对话框靠上的位置。
4.  根据日文文本的显示高度，动态计算英文文本的位置，使其显示在日文下方。
5.  在 `update` 方法中，从当前对话节点获取 `japanese` 和 `english` 字段，并更新对应的文本对象。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_98.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_99.png)

**核心概念：动态布局**
通过 `displayHeight` 属性获取文本渲染后的实际高度，据此计算下方元素的位置，实现自适应的垂直布局。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_101.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_103.png)

```javascript
// 创建日文文本
this.japaneseText = this.add.text(dialogX + padding, dialogY + padding, ‘’, {
  fontSize: ‘28px’,
  wordWrap: { width: dialogWidth - padding * 2 }
});

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_105.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_107.png)

// 在update中更新文本
const currentNode = this.dialogueManager.currentNode;
if (currentNode) {
  this.japaneseText.setText(currentNode.japanese);
  this.englishText.setText(currentNode.english);
  // 根据日文文本高度定位英文文本
  this.englishText.y = this.japaneseText.y + this.japaneseText.displayHeight + 10;
}
```

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_109.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_111.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_113.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_114.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_116.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_118.png)

---

## 章节 6：集成“下一步”按钮

为了让玩家能够控制对话节奏，我们需要添加一个“下一步”按钮。

我们将创建一个UI按钮，点击后触发对话前进到下一句。

1.  使用 `UIManager` 的 `createButton` 方法创建一个按钮。`UIManager` 封装了按钮的通用样式和交互逻辑。
2.  将按钮定位在对话框的右下角。
3.  为按钮的 `‘click’` 事件添加监听器。当点击时，调用 `DialogueManager` 的 `advanceDialogue` 方法。
4.  `advanceDialogue` 方法负责从当前对话数据中找到下一个有效的对话节点ID，并更新当前节点。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_120.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_121.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_123.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_125.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_127.png)

**核心概念：事件驱动交互**
通过为UI元素添加事件监听器（如 `‘click’`），将用户输入转化为游戏内的具体动作，这是交互式应用的基础。

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_129.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_131.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_133.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_135.png)

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_137.png)

```javascript
// 创建下一步按钮
this.nextButton = this.uiManager.createButton({
  x: dialogBox.x + dialogBox.displayWidth/2 - 100,
  y: dialogBox.y - 50,
  text: ‘Next >’,
  onClick: () => {
    this.dialogueManager.advanceDialogue();
    this.updateDialogueText(); // 更新显示的文本
  }
});
```

---

![](img/c1179ecf1e5548b317a38a1f2c14b2f0_139.png)

## 总结
本节课中我们一起学习了构建视觉小说对话框系统的完整流程。我们从**分离数据与逻辑**开始，创建了外部故事文件。然后**重构了管理器**，明确了职责分工。接着，我们逐步**构建了对话框UI**：包括底部的背景框、顶部的角色名称框、以及双语的对话文本显示。最后，我们**添加了交互功能**，通过“下一步”按钮让玩家可以推进剧情。

关键的技术点包括：通过预加载管理外部资源、利用 `update` 循环实现UI状态同步、使用 `setOrigin` 和动态计算进行精确的UI定位、以及通过事件监听器处理用户输入。虽然还有一些细节可以优化（如字体美化、打字机效果、音频同步等），但一个可运行的基础对话系统已经成功搭建起来。