# 007：使用语音交互的智能代理工作流 🎤

![](img/f90e79bd9787a02bc9bfcefbd9b04124_0.png)

## 概述

在本节课中，我们将学习如何为智能代理添加语音交互功能。我们将把之前基于文本的反馈机制，升级为通过自然语言语音进行交互。你将学会如何集成音频处理模型，并创建一个能够“听懂”用户语音指令的智能代理工作流。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_2.png)

---

## 回顾与过渡

上一节我们介绍了如何通过文本为代理提供反馈。本节中，我们来看看如何让代理接收并处理用户的语音反馈。我们将为代理添加多模态能力，使其能够捕获用户说出的音频反馈。

首先，让我们通过可视化工具回顾一下目前已构建的工作流。

以下是所有必要的导入语句和云服务密钥配置。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_4.png)

```python
# 导入必要的库和配置密钥
import ...
api_key = "your_api_key_here"
```

![](img/f90e79bd9787a02bc9bfcefbd9b04124_5.png)

![](img/f90e79bd9787a02bc9bfcefbd9b04124_6.png)

这是我们上一课使用的完整工作流代码。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_7.png)

```python
# 完整的Warag工作流定义
workflow = ...
```

执行上述代码后，我们使用可视化工具来查看工作流结构。

工作流相当复杂，因此我们需要稍微缩小视图。这个可视化非常有趣，因为它清晰地展示了我们遵循的完整“快乐路径”。

流程从开始、设置、解析表单、生成问题、提问、填写申请表开始。接着是一个外部步骤，用于获取人类反馈。获得反馈后，你可以选择停止，或者完成循环并再次执行。

---

## 从文本到语音的转变

现在，我们来做一个更有趣的改动：将文本反馈改为用户大声说出的实际语音。

为了实现这一点，我们将使用OpenAI的另一个模型：**Whisper**。LlamaIndex内置了使用Whisper将音频文件转录为文本的方法。

以下是一个函数，它接收一个文件并使用Whisper返回文本。

```python
def transcribe_audio(file_path):
    """
    使用Whisper模型将音频文件转录为文本。
    参数:
        file_path: 音频文件的路径。
    返回:
        转录后的文本字符串。
    """
    # 加载文件
    audio_file = load_file(file_path)
    # 实例化Whisper阅读器
    whisper_reader = WhisperReader()
    # 从文档中获取文本
    document = whisper_reader.load_data(audio_file)
    text = document[0].text
    return text
```

这与我们之前用于RAG的文档类型相同。在使用它之前，我们需要从麦克风捕获一些音频，这涉及一些额外的步骤。

首先，我们创建一个回调函数，将数据保存到一个全局变量中，以便在获得转录结果后有地方存储它。

```python
transcription_global = None

def store_transcription(text):
    global transcription_global
    transcription_global = text
```

---

## 创建音频捕获界面

接下来，你将使用**Gradio**。Gradio有特殊的组件，可以在笔记本内渲染，用于创建从麦克风捕获音频的界面。

当音频被捕获时，它会调用`transcribe_speech`函数处理录制的数据，并调用`store_transcription`存储结果。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_9.png)

以下是定义输入、输出并运行该函数的代码。

```python
import gradio as gr

def transcribe_speech(audio):
    # 此处是音频转录逻辑
    text = transcribe_audio(audio)
    store_transcription(text)
    return text

# 定义Gradio界面
inputs = gr.Audio(source="microphone", type="filepath")
outputs = gr.Textbox(label="转录文本")
interface = gr.Interface(fn=transcribe_speech, inputs=inputs, outputs=outputs)
interface.launch()
```

![](img/f90e79bd9787a02bc9bfcefbd9b04124_11.png)

在Gradio中，你进一步定义了一个包含此麦克风输入和输出的可视化界面，然后启动它。我们创建了一个Blocks界面，放入一些标签页，然后启动它。

Gradio为我们设置了这个非常酷的界面，它可以监听我们的麦克风。让我们来试试。

我们说：“Hey, computer, can you hear me?”，然后提交。它正确地转录了音频。很好。

Gradio非常强大，只需几行代码就能完成所有这些并打印出转录文本。

让我们确保转录结果已保存在之前设置的全局变量中。

```python
print(transcription_global)
```

结果在那里。我们将再次运行Gradio，因此最好关闭正在使用的Gradio界面，否则你的Gradio界面会相互冲突。

---

## 构建转录处理器类

现在，我们将创建一个全新的类：**TranscriptionHandler**。我将逐步讲解它的功能。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_13.png)

首先，我们创建一个队列来保存我们的转录值。每次我们录制一些内容时，都会使用`store_transcription`方法将其放入队列。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_14.png)

```python
import queue
import threading
import time

class TranscriptionHandler:
    def __init__(self):
        self.queue = queue.Queue()
        self.interface = None

    def store_transcription(self, text):
        """将转录文本存入队列"""
        self.queue.put(text)

    def create_interface(self):
        """创建Gradio录音界面"""
        # ... 界面创建逻辑，与之前类似，但调用self.store_transcription
        def gradio_transcribe(audio):
            text = transcribe_audio(audio)
            self.store_transcription(text)
            return text
        # 创建并启动Gradio界面
        self.interface = gr.Interface(...)
        self.interface.launch(inbrowser=True, share=False)

    def get_transcription(self):
        """从队列中获取转录文本，每0.5秒检查一次"""
        while True:
            try:
                # 非阻塞方式从队列获取
                text = self.queue.get_nowait()
                # 关闭界面
                if self.interface:
                    self.interface.close()
                return text
            except queue.Empty:
                # 队列为空，等待0.5秒再检查
                time.sleep(0.5)
```

`create_interface`方法与之前使用的界面和转录逻辑相同，只是它将结果存储在队列中而不是全局变量里。你可以看到它调用了我们在这里定义的`self.store_transcription`。

然后我们像之前一样启动转录界面。但我们做的新事情是，每0.5秒轮询一次，等待有内容进入队列。这个`while True`循环将永远继续，每次休眠0.5秒。但如果队列中有内容，它会注意到，并关闭界面，然后返回结果。

现在你有了一个转录处理器，你可以在工作流中获取人类输入时使用它，而不是键盘接口。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_16.png)

---

## 集成语音反馈到工作流

我们完全不需要为了使其工作而更改工作流。因此，和之前一样，我们设置工作流，传入我们的模拟简历和模拟申请表，然后等待一个`input_required`事件。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_18.png)

现在，我们不再获取键盘输入，而是调用我们的转录处理器。一旦转录处理器给我们提供了转录文本，我们就将其作为`human_response`事件发送出去。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_20.png)

![](img/f90e79bd9787a02bc9bfcefbd9b04124_22.png)

到了关键时刻，让我们试试看。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_24.png)

我做了个改动，但没有告诉你：为了让我们更清楚地了解底层发生了什么，我让它打印出它在思考时得到的每一个问题和答案。

你可以在这里看到它正在这样做。

好的，它已经问完了所有问题。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_26.png)

现在它启动了一个Gradio界面，向我们征求反馈。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_27.png)

![](img/f90e79bd9787a02bc9bfcefbd9b04124_29.png)

从问题的答案中我们可以看到，它犯了和之前一样的错误：项目组合是候选人做过的项目列表，而不是一个URL。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_31.png)

因此，让我们使用语音反馈来告诉它我们需要更改这一点。

我们说：“The portfolio field should be a URL.”

我们提交。

LLM读取了该转录文本，并正确地判定我给出了一些反馈。

现在你可以看到我们的问题和答案正在生成。每一个都附加了这个额外的反馈字段。每个问题都得到了相同的反馈：“portfolio字段应该是一个URL”，而这实际上只对其中一个问题有用。你可以思考一下，如果在一个更生产化的应用中，你会如何改进这一点。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_33.png)

它再次问完了所有问题，并启动了另一个Gradio界面征求我们的反馈。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_34.png)

![](img/f90e79bd9787a02bc9bfcefbd9b04124_35.png)

![](img/f90e79bd9787a02bc9bfcefbd9b04124_36.png)

让我们告诉它这次做得很好，因为它确实做得很好。项目组合是一个URL，正如我们所要求的。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_37.png)

![](img/f90e79bd9787a02bc9bfcefbd9b04124_38.png)

我们说：“That‘s great. Good job.”

LLM正确地将我的积极反馈解释为一切正常，并输出了表单。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_40.png)

![](img/f90e79bd9787a02bc9bfcefbd9b04124_41.png)

---

![](img/f90e79bd9787a02bc9bfcefbd9b04124_43.png)

## 总结 🎉

恭喜你！你已经成功创建了一个能够响应人类语音反馈的AI代理。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_45.png)

本节课中，我们一起学习了：
1.  **回顾复杂工作流**：通过可视化工具理解了代理的完整执行路径。
2.  **集成语音识别**：利用OpenAI的Whisper模型将音频转换为文本。
3.  **构建交互界面**：使用Gradio快速创建麦克风音频捕获界面。
4.  **创建处理器类**：设计`TranscriptionHandler`类来管理音频输入队列和界面生命周期。
5.  **无缝替换反馈方式**：将原有的文本输入节点替换为语音输入节点，而无需改动核心工作流逻辑。
6.  **实现语音交互循环**：代理能够接收语音反馈，理解其内容，并根据反馈调整其行为，完成多轮交互。

![](img/f90e79bd9787a02bc9bfcefbd9b04124_46.png)

![](img/f90e79bd9787a02bc9bfcefbd9b04124_47.png)

你现在拥有一个可以通过自然语言对话进行指导和修正的智能文档处理代理了。