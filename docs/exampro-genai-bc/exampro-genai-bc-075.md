# 75：WhisperX逐字转录 Part1 🎤

![](img/6dc2d2b929dadb59921c8c2b37a474ba_1.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_3.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_5.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_7.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_9.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_11.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_12.png)

在本节课中，我们将学习如何使用Whisper和WhisperX进行自动语音识别，并尝试获取逐字时间戳。我们将从环境配置开始，逐步探索模型的使用、音频格式处理以及如何通过Docker容器解决依赖问题。

![](img/6dc2d2b929dadb59921c8c2b37a474ba_14.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_16.png)

## 环境配置与模型选择

![](img/6dc2d2b929dadb59921c8c2b37a474ba_18.png)

上一节我们介绍了课程目标，本节中我们来看看如何搭建一个用于自动语音识别的开发环境。

![](img/6dc2d2b929dadb59921c8c2b37a474ba_20.png)

首先，我们需要创建一个新的项目目录并设置Python环境。

以下是创建和激活Conda环境的步骤：
```bash
conda create -n asr_task python=3.11
conda activate asr_task
conda install ipykernel
```

接下来，安装必要的Python库。
```bash
pip install transformers ipywidgets scipy torch torchaudio
```

Whisper模型有不同的大小，性能与资源消耗各异。我们将使用`small`模型，它在英语和日语识别上表现良好，且对显存要求适中。

![](img/6dc2d2b929dadb59921c8c2b37a474ba_22.png)

## 使用Hugging Face Whisper进行转录

![](img/6dc2d2b929dadb59921c8c2b37a474ba_24.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_26.png)

环境配置好后，我们来看看如何使用Hugging Face的Transformers库加载Whisper模型并进行转录。

![](img/6dc2d2b929dadb59921c8c2b37a474ba_28.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_30.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_32.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_34.png)

核心步骤是创建语音识别管道并指定模型。
```python
from transformers import pipeline
import torch

![](img/6dc2d2b929dadb59921c8c2b37a474ba_36.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_38.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_39.png)

# 检查并设置设备
device = "cuda:0" if torch.cuda.is_available() else "cpu"
print(f"Using device: {device}")

# 创建语音识别管道
asr_pipeline = pipeline(
    "automatic-speech-recognition",
    model="openai/whisper-small",
    device=device
)
```

![](img/6dc2d2b929dadb59921c8c2b37a474ba_41.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_43.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_45.png)

加载音频文件并执行转录。
```python
file_name = "jp_sample.wav"
result = asr_pipeline(file_name, return_timestamps=True)
print(result)
```

![](img/6dc2d2b929dadb59921c8c2b37a474ba_47.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_49.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_51.png)

有时音频文件格式可能导致问题。如果遇到错误，可以使用FFmpeg进行转换，确保其采样率等参数符合要求。
```bash
ffmpeg -i input.wav -ar 16000 -ac 1 -c:a pcm_s16le output.wav
```

![](img/6dc2d2b929dadb59921c8c2b37a474ba_53.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_54.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_56.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_57.png)

## 探索逐字时间戳与WhisperX

![](img/6dc2d2b929dadb59921c8c2b37a474ba_59.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_61.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_63.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_65.png)

基础的Whisper模型返回的是句子或段落级别的时间戳。为了获得逐字时间戳，我们需要使用WhisperX。

![](img/6dc2d2b929dadb59921c8c2b37a474ba_67.png)

然而，直接安装WhisperX可能会遇到CUDA版本不匹配等复杂的依赖问题。一个更简单的解决方案是使用Docker容器，它包含了所有预配置的环境。

以下是使用Docker运行WhisperX的命令：
```bash
docker run --gpus all -v $(pwd):/app ghcr.io/m-bain/whisperx:latest --model large-v3 --language ja --output_dir /app/output /app/wake_up_converted.wav
```

![](img/6dc2d2b929dadb59921c8c2b37a474ba_69.png)

这个命令会下载WhisperX的Docker镜像，并将当前目录挂载到容器的`/app`路径，然后对指定的音频文件进行转录，结果将输出到本地目录。

![](img/6dc2d2b929dadb59921c8c2b37a474ba_71.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_73.png)

## 总结

![](img/6dc2d2b929dadb59921c8c2b37a474ba_75.png)

![](img/6dc2d2b929dadb59921c8c2b37a474ba_77.png)

本节课中我们一起学习了自动语音识别的基础流程。我们首先配置了Python环境并安装了Whisper模型，成功实现了音频转录。接着，我们遇到了获取逐字时间戳的需求，并探索了WhisperX这一解决方案。由于本地环境依赖复杂，我们最终采用了Docker容器来便捷地运行WhisperX，从而能够处理日语等语言并获取更精细的单词级时间戳信息。这个过程展示了在AI开发中灵活运用不同工具和方法来解决实际问题的思路。