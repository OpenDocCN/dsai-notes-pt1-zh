# 77：WhisperX匹配原始转录

![](img/1843eed7a517c2ec8e2624f004739853_1.png)

![](img/1843eed7a517c2ec8e2624f004739853_2.png)

![](img/1843eed7a517c2ec8e2624f004739853_4.png)

![](img/1843eed7a517c2ec8e2624f004739853_6.png)

![](img/1843eed7a517c2ec8e2624f004739853_7.png)

![](img/1843eed7a517c2ec8e2624f004739853_9.png)

![](img/1843eed7a517c2ec8e2624f004739853_11.png)

![](img/1843eed7a517c2ec8e2624f004739853_13.png)

![](img/1843eed7a517c2ec8e2624f004739853_15.png)

## 概述

![](img/1843eed7a517c2ec8e2624f004739853_17.png)

![](img/1843eed7a517c2ec8e2624f004739853_19.png)

![](img/1843eed7a517c2ec8e2624f004739853_21.png)

在本节课中，我们将探讨一个实际业务场景：如何将已有的、准确的视频字幕与WhisperX生成的逐字符时间戳进行匹配，从而获得逐词高亮功能。我们将学习如何获取原始字幕、处理音频、运行WhisperX，并最终使用大语言模型（LLM）将两者对齐。

![](img/1843eed7a517c2ec8e2624f004739853_23.png)

![](img/1843eed7a517c2ec8e2624f004739853_25.png)

![](img/1843eed7a517c2ec8e2624f004739853_27.png)

![](img/1843eed7a517c2ec8e2624f004739853_29.png)

![](img/1843eed7a517c2ec8e2624f004739853_31.png)

![](img/1843eed7a517c2ec8e2624f004739853_33.png)

![](img/1843eed7a517c2ec8e2624f004739853_35.png)

## 业务场景与挑战

![](img/1843eed7a517c2ec8e2624f004739853_37.png)

![](img/1843eed7a517c2ec8e2624f004739853_38.png)

![](img/1843eed7a517c2ec8e2624f004739853_40.png)

![](img/1843eed7a517c2ec8e2624f004739853_42.png)

上一节我们介绍了如何使用WhisperX生成逐字符的转录。本节中，我们来看看一个常见的业务需求。

![](img/1843eed7a517c2ec8e2624f004739853_44.png)

![](img/1843eed7a517c2ec8e2624f004739853_45.png)

![](img/1843eed7a517c2ec8e2624f004739853_47.png)

![](img/1843eed7a517c2ec8e2624f004739853_49.png)

![](img/1843eed7a517c2ec8e2624f004739853_50.png)

![](img/1843eed7a517c2ec8e2624f004739853_52.png)

![](img/1843eed7a517c2ec8e2624f004739853_54.png)

假设你运营一个语言学习平台，平台上的视频已经配备了准确的字幕。当用户观看视频时，你希望实现单词随播放高亮的效果。直接使用WhisperX等转录服务可能不够准确，但你已经拥有正确的转录文本。因此，核心挑战在于：**如何将已有的准确字幕与WhisperX生成的、带有时间戳但不一定字符正确的转录进行匹配，从而获得逐词的时间信息。**

![](img/1843eed7a517c2ec8e2624f004739853_56.png)

![](img/1843eed7a517c2ec8e2624f004739853_58.png)

![](img/1843eed7a517c2ec8e2624f004739853_60.png)

![](img/1843eed7a517c2ec8e2624f004739853_62.png)

![](img/1843eed7a517c2ec8e2624f004739853_64.png)

我们的思路是：利用LLM强大的理解和匹配能力，以准确字幕为“真相源”，从WhisperX的输出中提取对应的时间戳，最终生成一个结合了正确文本和精确时间码的新文件。

![](img/1843eed7a517c2ec8e2624f004739853_66.png)

![](img/1843eed7a517c2ec8e2624f004739853_68.png)

![](img/1843eed7a517c2ec8e2624f004739853_70.png)

![](img/1843eed7a517c2ec8e2624f004739853_72.png)

![](img/1843eed7a517c2ec8e2624f004739853_74.png)

![](img/1843eed7a517c2ec8e2624f004739853_76.png)

![](img/1843eed7a517c2ec8e2624f004739853_78.png)

![](img/1843eed7a517c2ec8e2624f004739853_80.png)

![](img/1843eed7a517c2ec8e2624f004739853_82.png)

![](img/1843eed7a517c2ec8e2624f004739853_84.png)

![](img/1843eed7a517c2ec8e2624f004739853_86.png)

## 第一步：准备数据源

![](img/1843eed7a517c2ec8e2624f004739853_88.png)

![](img/1843eed7a517c2ec8e2624f004739853_90.png)

![](img/1843eed7a517c2ec8e2624f004739853_92.png)

![](img/1843eed7a517c2ec8e2624f004739853_94.png)

![](img/1843eed7a517c2ec8e2624f004739853_96.png)

![](img/1843eed7a517c2ec8e2624f004739853_98.png)

![](img/1843eed7a517c2ec8e2624f004739853_100.png)

![](img/1843eed7a517c2ec8e2624f004739853_102.png)

![](img/1843eed7a517c2ec8e2624f004739853_104.png)

为了测试这个流程，我们需要一个拥有准确字幕的视频源。以下是寻找和准备数据源的步骤。

![](img/1843eed7a517c2ec8e2624f004739853_106.png)

![](img/1843eed7a517c2ec8e2624f004739853_108.png)

![](img/1843eed7a517c2ec8e2624f004739853_110.png)

![](img/1843eed7a517c2ec8e2624f004739853_111.png)

1.  **寻找可靠来源**：我们选择了一个提供“可理解日语”教学视频的频道。这些视频通常配有精心制作的字幕，准确性高。
2.  **获取原始字幕**：通过YouTube的字幕功能，我们可以下载视频的原始日文字幕文件（通常为`.srt`或`.vtt`格式）。
    *   使用工具如 `youtube-transcript-api` 或 `yt-dlp` 可以自动化下载字幕。
    *   下载后，需要清理字幕文件中的时间码和序号，只保留纯文本。
    *   最终保存为 `og_comic_learn.txt` 作为我们的“准确转录源”。
3.  **下载并准备音频**：为了运行WhisperX，我们需要视频的音频文件。
    *   使用 `yt-dlp` 下载最佳音质的音频，并转换为WAV格式。
    *   为确保WhisperX的最佳识别效果，建议使用特定的音频参数。根据经验，有效的配置如下：
        ```bash
        # 示例：使用 yt-dlp 和 ffmpeg 转换音频
        yt-dlp -x --audio-format wav --audio-quality 0 --postprocessor-args "-acodec pcm_s16le -ac 1 -ar 24000" -o "comic_learn.wav" <视频URL>
        ```
    *   关键参数解释：
        *   `-acodec pcm_s16le`: 指定PCM 16位小端编码，这是WAV的标准格式。
        *   `-ac 1`: 设置为单声道（Mono）。
        *   `-ar 24000`: 设置采样率为24000 Hz。
    *   将处理好的音频保存为 `comic_learn_standard.wav`。

![](img/1843eed7a517c2ec8e2624f004739853_113.png)

![](img/1843eed7a517c2ec8e2624f004739853_115.png)

![](img/1843eed7a517c2ec8e2624f004739853_117.png)

![](img/1843eed7a517c2ec8e2624f004739853_119.png)

![](img/1843eed7a517c2ec8e2624f004739853_121.png)

![](img/1843eed7a517c2ec8e2624f004739853_123.png)

![](img/1843eed7a517c2ec8e2624f004739853_124.png)

## 第二步：使用WhisperX生成转录

![](img/1843eed7a517c2ec8e2624f004739853_126.png)

![](img/1843eed7a517c2ec8e2624f004739853_128.png)

![](img/1843eed7a517c2ec8e2624f004739853_130.png)

![](img/1843eed7a517c2ec8e2624f004739853_132.png)

现在，我们使用WhisperX来处理音频文件，生成带有逐字符时间戳的转录。

![](img/1843eed7a517c2ec8e2624f004739853_134.png)

![](img/1843eed7a517c2ec8e2624f004739853_136.png)

![](img/1843eed7a517c2ec8e2624f004739853_138.png)

![](img/1843eed7a517c2ec8e2624f004739853_140.png)

![](img/1843eed7a517c2ec8e2624f004739853_142.png)

![](img/1843eed7a517c2ec8e2624f004739853_144.png)

![](img/1843eed7a517c2ec8e2624f004739853_146.png)

1.  **运行WhisperX**：通过Docker容器运行WhisperX，指定日语模型和大模型以获取更佳效果。
    ```bash
    docker run -it --gpus all -v $(pwd):/data whisperx/whisperx:latest --model large --language ja --output_format json --output_dir /data/output comic_learn_standard.wav
    ```
2.  **理解输出**：WhisperX会生成一个JSON文件（例如 `comic_learn_standard.json`）。这个文件结构包含 `segments`（段落），每个段落下又有 `words`，而每个 `word` 实际上是由 `chars`（字符）数组构成，每个字符都带有 `start`（开始时间）和 `end`（结束时间）。这正是我们需要的“带有时间戳但不一定字符正确”的转录数据。

![](img/1843eed7a517c2ec8e2624f004739853_148.png)

![](img/1843eed7a517c2ec8e2624f004739853_150.png)

![](img/1843eed7a517c2ec8e2624f004739853_151.png)

![](img/1843eed7a517c2ec8e2624f004739853_153.png)

![](img/1843eed7a517c2ec8e2624f004739853_154.png)

**注意**：对于长音频文件，WhisperX内部会自动进行分块处理（例如每30秒一块）。我们的输出JSON包含了所有块的结果。

![](img/1843eed7a517c2ec8e2624f004739853_156.png)

![](img/1843eed7a517c2ec8e2624f004739853_158.png)

![](img/1843eed7a517c2ec8e2624f004739853_160.png)

![](img/1843eed7a517c2ec8e2624f004739853_162.png)

![](img/1843eed7a517c2ec8e2624f004739853_164.png)

## 第三步：使用LLM对齐转录

![](img/1843eed7a517c2ec8e2624f004739853_166.png)

这是最核心的一步。我们将编写一个Python脚本，利用LLM（如OpenAI GPT-4或Claude）将准确的原始字幕与WhisperX的详细输出进行对齐。

![](img/1843eed7a517c2ec8e2624f004739853_168.png)

![](img/1843eed7a517c2ec8e2624f004739853_170.png)

![](img/1843eed7a517c2ec8e2624f004739853_172.png)

![](img/1843eed7a517c2ec8e2624f004739853_174.png)

![](img/1843eed7a517c2ec8e2624f004739853_176.png)

以下是实现对齐脚本的关键步骤和代码逻辑：

![](img/1843eed7a517c2ec8e2624f004739853_178.png)

![](img/1843eed7a517c2ec8e2624f004739853_180.png)

![](img/1843eed7a517c2ec8e2624f004739853_182.png)

![](img/1843eed7a517c2ec8e2624f004739853_184.png)

1.  **加载数据**：读取原始字幕文件（`og_comic_learn.txt`）和WhisperX的JSON输出文件（`comic_learn_standard.json`）。
2.  **构建提示词（Prompt）**：设计一个清晰的系统提示词，指导LLM完成对齐任务。提示词需要阐明：
    *   **角色**：你是一个专业的日语语言对齐系统。
    *   **输入**：
        *   `original_transcript`: 准确的无时间码字幕文本。
        *   `whisperx_data`: WhisperX生成的、包含逐字符时间戳的转录数据。
    *   **任务**：以 `original_transcript` 为真相源，从 `whisperx_data` 中找出每个词（或自然短语）对应的时间码（`start`, `end`）。专注于语音匹配，因为WhisperX可能发音正确但字符写错。
    *   **输出格式**：要求LLM严格输出指定格式的JSON，包含 `words` 列表，每个词有 `text`, `start`, `end` 字段。
    *   **示例**：提供一个简明的输入输出示例，帮助LLM理解任务。
3.  **调用LLM API**：将构建好的提示词发送给LLM（例如OpenAI的ChatCompletion接口）。
4.  **解析与保存**：解析LLM返回的JSON内容，并将其保存为最终的对齐文件（如 `comic_learn_aligned.json`）。

![](img/1843eed7a517c2ec8e2624f004739853_186.png)

![](img/1843eed7a517c2ec8e2624f004739853_188.png)

![](img/1843eed7a517c2ec8e2624f004739853_190.png)

![](img/1843eed7a517c2ec8e2624f004739853_192.png)

![](img/1843eed7a517c2ec8e2624f004739853_194.png)

![](img/1843eed7a517c2ec8e2624f004739853_196.png)

![](img/1843eed7a517c2ec8e2624f004739853_198.png)

![](img/1843eed7a517c2ec8e2624f004739853_200.png)

![](img/1843eed7a517c2ec8e2624f004739853_202.png)

![](img/1843eed7a517c2ec8e2624f004739853_204.png)

![](img/1843eed7a517c2ec8e2624f004739853_206.png)

![](img/1843eed7a517c2ec8e2624f004739853_208.png)

![](img/1843eed7a517c2ec8e2624f004739853_209.png)

以下是脚本的核心代码框架：

![](img/1843eed7a517c2ec8e2624f004739853_211.png)

![](img/1843eed7a517c2ec8e2624f004739853_212.png)

![](img/1843eed7a517c2ec8e2624f004739853_214.png)

![](img/1843eed7a517c2ec8e2624f004739853_216.png)

![](img/1843eed7a517c2ec8e2624f004739853_218.png)

```python
import json
import openai
from dotenv import load_dotenv
import os

load_dotenv()
openai.api_key = os.getenv("OPENAI_API_KEY")

def align_transcripts(original_path, whisperx_path, output_path):
    # 1. 加载数据
    with open(original_path, 'r', encoding='utf-8') as f:
        original_transcript = f.read()
    with open(whisperx_path, 'r', encoding='utf-8') as f:
        whisperx_data = json.load(f)

    # 2. 构建提示词
    system_prompt = """你是一个专业的日语语言对齐系统。你的任务是将准确的日语转录文本与WhisperX生成的时间戳进行逐词对齐。"""
    user_prompt = f"""
    原始准确转录（无时间码）：
    ```text
    {original_transcript}
    ```

    WhisperX数据（包含字符级时间戳）：
    ```json
    {json.dumps(whisperx_data, ensure_ascii=False)[:5000]} 
    ```

    请将原始转录中的每个词或自然短语，与WhisperX数据中的时间信息对齐。
    专注于语音匹配，因为字符可能不准确但发音正确。
    请输出一个JSON对象，包含一个“words”数组，每个元素有“text”（文本）、“start”（开始时间）、“end”（结束时间）字段。
    """

    # 3. 调用LLM
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": system_prompt},
            {"role": "user", "content": user_prompt}
        ],
        temperature=0.1
    )

    # 4. 解析并保存结果
    aligned_content = response.choices[0].message.content
    # 尝试从返回内容中提取JSON
    aligned_json = json.loads(aligned_content)
    with open(output_path, 'w', encoding='utf-8') as f:
        json.dump(aligned_json, f, ensure_ascii=False, indent=2)
    print(f"对齐完成，结果已保存至：{output_path}")

if __name__ == "__main__":
    align_transcripts("og_comic_learn.txt", "comic_learn_standard.json", "comic_learn_aligned.json")
```

![](img/1843eed7a517c2ec8e2624f004739853_220.png)

![](img/1843eed7a517c2ec8e2624f004739853_222.png)

![](img/1843eed7a517c2ec8e2624f004739853_224.png)

**注意事项与优化**：
*   **上下文长度**：如果音频很长，WhisperX的JSON数据可能非常大，会超出LLM的上下文窗口限制。解决方案包括：
    *   **数据精简**：删除JSON中不必要的字段（如`score`），缩写键名（如`word`->`w`, `start`->`s`），移除空格和换行。
    *   **分块处理**：将原始字幕和WhisperX数据都按段落分块，分别进行对齐，最后合并结果。
*   **提示词工程**：提供清晰、具体的示例可以极大提高对齐质量。
*   **模型选择**：不同的LLM（GPT-4, Claude, 本地模型）在成本和效果上各有权衡，可根据实际情况选择。
*   **错误处理**：代码中应增加对LLM返回内容格式的校验和错误处理。

## 总结

本节课中，我们一起学习并实践了一个完整的解决方案，用于为已有准确字幕的视频生成逐词时间戳。

1.  **明确需求**：语言学习平台需要将准确字幕与音频时间轴对齐，实现单词高亮。
2.  **数据准备**：获取原始字幕，并下载、转换出适合WhisperX处理的音频文件。
3.  **语音识别**：利用WhisperX生成带有精细时间戳（字符级）的转录文本。
4.  **智能对齐**：通过精心设计的提示词，借助大语言模型（LLM）的理解与匹配能力，将准确字幕文本与WhisperX的时间信息进行对齐，最终输出一份结合了正确文本和精确时间码的JSON文件。

![](img/1843eed7a517c2ec8e2624f004739853_226.png)

![](img/1843eed7a517c2ec8e2624f004739853_228.png)

![](img/1843eed7a517c2ec8e2624f004739853_229.png)

![](img/1843eed7a517c2ec8e2624f004739853_231.png)

这个过程展示了如何将传统的语音识别工具（WhisperX）与现代的大语言模型相结合，解决实际业务中“数据再加工”的难题。虽然在实际操作中可能会遇到上下文长度限制、提示词优化、成本控制等挑战，但整个技术路径是可行且强大的。