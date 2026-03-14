# PyTorch 极简实战教程 P18：使用 Flask 和 Heroku 部署 PyTorch 模型 🚀

![](img/a4f608f66d892f12b3640cd5547c24da_1.png)

在本节课中，我们将学习如何将一个训练好的 PyTorch 模型封装成一个 Web 应用，并使用 Flask 框架创建 REST API，最终将其部署到 Heroku 云平台上。我们将以手写数字识别（MNIST）为例，构建一个可以接收用户上传的图片并返回预测结果的完整应用。

![](img/a4f608f66d892f12b3640cd5547c24da_3.png)

## 概述 📋

![](img/a4f608f66d892f12b3640cd5547c24da_5.png)

我们将按照以下步骤进行：
1.  创建项目目录和虚拟环境。
2.  安装必要的依赖包。
3.  编写一个简单的 Flask 应用框架。
4.  加载并集成一个预训练的 PyTorch 模型。
5.  实现图像预处理和预测的 API 端点。
6.  在本地测试应用。
7.  配置应用以适配 Heroku 部署环境。
8.  将应用部署到 Heroku 并在线测试。

![](img/a4f608f66d892f12b3640cd5547c24da_7.png)

## 1. 项目初始化与环境搭建

首先，我们需要创建一个新的项目目录并设置 Python 虚拟环境，以隔离项目依赖。

以下是具体步骤：

*   **创建项目目录**：`mkdir pytorch_flask_2 && cd pytorch_flask_2`
*   **创建虚拟环境**：`python3 -m venv venv`
    *   **注意**：在 Windows 系统上，命令可能为 `python -m venv venv`。
*   **激活虚拟环境**：
    *   **macOS/Linux**: `source venv/bin/activate`
    *   **Windows**: `venv\Scripts\activate`
*   **安装核心依赖**：
    *   `pip install flask`
    *   `pip install torch torchvision`

## 2. 创建 Flask 应用框架

上一节我们搭建好了基础环境，本节中我们来看看如何创建一个最简单的 Flask 应用。

在项目根目录下创建一个 `app` 文件夹，并在其中创建 `main.py` 文件。

```python
# app/main.py
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/predict', methods=['POST'])
def predict():
    # 这是一个临时的响应，后续将替换为真正的预测逻辑
    return jsonify({'results': 1})

![](img/a4f608f66d892f12b3640cd5547c24da_9.png)

![](img/a4f608f66d892f12b3640cd5547c24da_10.png)

![](img/a4f608f66d892f12b3640cd5547c24da_11.png)

if __name__ == '__main__':
    app.run(debug=True)
```

![](img/a4f608f66d892f12b3640cd5547c24da_12.png)

现在，我们可以在终端中启动这个应用进行测试。

以下是启动和测试应用的命令：

*   **设置环境变量并启动**：
    *   `export FLASK_APP=app/main.py` (macOS/Linux)
    *   `export FLASK_ENV=development` (macOS/Linux)
    *   `set FLASK_APP=app\main.py` (Windows)
    *   `set FLASK_ENV=development` (Windows)
    *   `flask run`
*   **创建测试脚本**：在项目根目录创建 `test.py` 文件，使用 `requests` 库测试 API。
    ```python
    # test.py
    import requests
    response = requests.post('http://localhost:5000/predict')
    print(response.text)
    ```
*   **运行测试**：在另一个终端执行 `python test.py`，应看到返回 `{"results": 1}`。

## 3. 准备 PyTorch 模型

![](img/a4f608f66d892f12b3640cd5547c24da_14.png)

我们的 Flask 应用框架已经可以运行，接下来需要集成一个能够进行图像分类的 PyTorch 模型。我们将使用一个在 MNIST 数据集上训练好的简单前馈神经网络。

![](img/a4f608f66d892f12b3640cd5547c24da_16.png)

![](img/a4f608f66d892f12b3640cd5547c24da_18.png)

首先，我们需要获取并运行训练代码来生成模型文件。

![](img/a4f608f66d892f12b3640cd5547c24da_20.png)

![](img/a4f608f66d892f12b3640cd5547c24da_21.png)

![](img/a4f608f66d892f12b3640cd5547c24da_22.png)

以下是模型训练和保存的步骤：

![](img/a4f608f66d892f12b3640cd5547c24da_24.png)

![](img/a4f608f66d892f12b3640cd5547c24da_26.png)

1.  **获取训练代码**：从 PyTorch 教程仓库中找到基础前馈神经网络的代码（例如教程 P13）。
2.  **修改并运行**：在训练代码末尾添加模型保存语句。
    ```python
    # 在训练循环结束后添加
    torch.save(model.state_dict(), 'mnist_ffn.pth')
    ```
3.  **执行训练**：运行训练脚本 `python tutorial_13_ffn.py`。训练完成后会生成 `mnist_ffn.pth` 模型权重文件。
4.  **复制模型文件**：将生成的 `mnist_ffn.pth` 文件复制到我们的 `app` 目录下。

![](img/a4f608f66d892f12b3640cd5547c24da_28.png)

![](img/a4f608f66d892f12b3640cd5547c24da_29.png)

## 4. 实现模型推理逻辑

![](img/a4f608f66d892f12b3640cd5547c24da_31.png)

现在我们已经有了模型文件，本节中我们将在 `app` 目录下创建一个新的文件 `torch_utils.py`，专门用于封装模型加载、图像预处理和预测的函数。

![](img/a4f608f66d892f12b3640cd5547c24da_32.png)

```python
# app/torch_utils.py
import torch
import torch.nn as nn
import torchvision.transforms as transforms
from PIL import Image
import io

![](img/a4f608f66d892f12b3640cd5547c24da_34.png)

![](img/a4f608f66d892f12b3640cd5547c24da_35.png)

![](img/a4f608f66d892f12b3640cd5547c24da_36.png)

# 1. 定义模型结构（需与训练时一致）
class NeuralNet(nn.Module):
    def __init__(self, input_size, hidden_size, num_classes):
        super(NeuralNet, self).__init__()
        self.fc1 = nn.Linear(input_size, hidden_size)
        self.relu = nn.ReLU()
        self.fc2 = nn.Linear(hidden_size, num_classes)

    def forward(self, x):
        out = self.fc1(x)
        out = self.relu(out)
        out = self.fc2(out)
        return out

![](img/a4f608f66d892f12b3640cd5547c24da_37.png)

# 超参数
input_size = 784 # 28x28
hidden_size = 500
num_classes = 10

![](img/a4f608f66d892f12b3640cd5547c24da_39.png)

# 2. 加载模型
def load_model(model_path='app/mnist_ffn.pth'):
    model = NeuralNet(input_size, hidden_size, num_classes)
    # 加载权重，map_location='cpu'确保在CPU上加载
    model.load_state_dict(torch.load(model_path, map_location='cpu'))
    model.eval() # 设置为评估模式
    return model

model = load_model()

![](img/a4f608f66d892f12b3640cd5547c24da_41.png)

![](img/a4f608f66d892f12b3640cd5547c24da_43.png)

# 3. 图像预处理函数
def transform_image(image_bytes):
    """
    将上传的图片字节数据转换为模型所需的张量。
    """
    transform = transforms.Compose([
        transforms.Grayscale(num_output_channels=1), # 转为灰度图
        transforms.Resize((28, 28)),                 # 调整大小为28x28
        transforms.ToTensor(),                       # 转为张量
        transforms.Normalize((0.1307,), (0.3081,))   # 使用MNIST的均值和标准差归一化
    ])
    image = Image.open(io.BytesIO(image_bytes))
    return transform(image).unsqueeze(0) # 增加一个批次维度

![](img/a4f608f66d892f12b3640cd5547c24da_45.png)

# 4. 预测函数
def get_prediction(image_tensor):
    """
    对输入的图像张量进行预测。
    """
    outputs = model(image_tensor)
    _, predicted = torch.max(outputs.data, 1)
    return predicted
```

## 5. 完善 Flask API 端点

模型推理逻辑已经准备就绪，现在我们需要修改 `app/main.py` 中的 `/predict` 端点，使其能够接收图片、调用模型并返回真正的预测结果。

我们还需要添加一些错误处理，例如检查文件是否存在、格式是否支持等。

```python
# app/main.py
from flask import Flask, request, jsonify
from app.torch_utils import transform_image, get_prediction
import io

app = Flask(__name__)

# 辅助函数：检查文件扩展名是否允许
def allowed_file(filename):
    ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg'}
    return '.' in filename and filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS

@app.route('/predict', methods=['POST'])
def predict():
    if request.method == 'POST':
        # 1. 检查请求中是否有文件
        if 'file' not in request.files:
            return jsonify({'error': 'No file provided'}), 400
        file = request.files['file']
        # 2. 检查文件名是否有效
        if file.filename == '':
            return jsonify({'error': 'No file selected'}), 400
        # 3. 检查文件类型是否允许
        if not allowed_file(file.filename):
            return jsonify({'error': 'File type not supported. Please upload a PNG, JPG, or JPEG image.'}), 400

        try:
            # 4. 读取文件字节并转换
            img_bytes = file.read()
            tensor = transform_image(img_bytes)
            # 5. 进行预测
            prediction = get_prediction(tensor)
            # 6. 返回结果
            data = {'prediction': prediction.item(), 'class_name': str(prediction.item())}
            return jsonify(data)
        except Exception as e:
            # 7. 处理预测过程中的错误
            return jsonify({'error': 'Error during prediction. Please ensure the image is a clear digit.'}), 500

if __name__ == '__main__':
    app.run(debug=True)
```

![](img/a4f608f66d892f12b3640cd5547c24da_47.png)

![](img/a4f608f66d892f12b3640cd5547c24da_49.png)

更新测试脚本 `test.py`，使其能够上传一张本地图片进行测试。

![](img/a4f608f66d892f12b3640cd5547c24da_51.png)

```python
# test.py
import requests

![](img/a4f608f66d892f12b3640cd5547c24da_53.png)

# 使用本地图片文件进行测试
with open('7.png', 'rb') as f:
    img_bytes = f.read()

response = requests.post('http://localhost:5000/predict',
                         files={'file': ('7.png', img_bytes, 'image/png')})
print(response.json())
```

运行 `python test.py`，如果一切正常，你将收到类似 `{'prediction': 7, 'class_name': '7'}` 的响应。你也可以用画图工具自己画一个数字（白字黑底）保存为图片进行测试。

![](img/a4f608f66d892f12b3640cd5547c24da_55.png)

![](img/a4f608f66d892f12b3640cd5547c24da_57.png)

## 6. 为 Heroku 部署做准备

我们的应用在本地运行良好，接下来需要配置一些文件，以便将其部署到 Heroku 云平台。

以下是部署前的配置步骤：

![](img/a4f608f66d892f12b3640cd5547c24da_59.png)

1.  **安装生产服务器**：Heroku 推荐使用 Gunicorn。在虚拟环境中安装：`pip install gunicorn`
2.  **创建 `wsgi.py` 文件**：在项目根目录创建此文件，作为 Gunicorn 的入口点。
    ```python
    # wsgi.py
    from app.main import app
    if __name__ == "__main__":
        app.run()
    ```
3.  **创建 `Procfile` 文件**：告诉 Heroku 如何启动应用。
    ```
    # Procfile
    web: gunicorn wsgi:app
    ```
4.  **创建 `runtime.txt` 文件**：指定 Python 版本。
    ```
    # runtime.txt
    python-3.8.13
    ```
5.  **生成 `requirements.txt` 文件**：列出所有依赖。**关键步骤**：为了减小部署包体积，我们需要指定安装 PyTorch 的 CPU 版本。
    *   首先运行 `pip freeze > requirements.txt`。
    *   然后编辑 `requirements.txt` 文件，将 `torch` 和 `torchvision` 的行替换为：
        ```
        torch==1.9.0+cpu
        torchvision==0.10.0+cpu
        -f https://download.pytorch.org/whl/torch_stable.html
        ```
6.  **创建 `.gitignore` 文件**：忽略虚拟环境等不必要的文件。可以从 [github/gitignore](https://github.com/github/gitignore/blob/main/Python.gitignore) 获取模板。

![](img/a4f608f66d892f12b3640cd5547c24da_60.png)

## 7. 部署到 Heroku

所有配置文件已就绪，现在可以将我们的应用推送到 Heroku。

以下是部署命令序列：

![](img/a4f608f66d892f12b3640cd5547c24da_62.png)

*   **登录 Heroku**：`heroku login -i`
*   **创建 Heroku 应用**：`heroku create flask-pytorch-tutorial` (应用名需唯一)
*   **初始化 Git 仓库并提交代码**：
    *   `git init`
    *   `git add .`
    *   `git commit -m "Initial commit"`
*   **设置 Heroku 远程仓库并推送代码**：
    *   `heroku git:remote -a flask-pytorch-tutorial`
    *   `git push heroku master`
*   **查看部署日志**：推送后，Heroku 会自动构建和部署。在终端观察输出，直到出现 `Verifying deploy... done.` 字样。
*   **获取应用在线地址**：部署成功后，可以使用 `heroku open` 命令打开应用主页，或记下类似 `https://flask-pytorch-tutorial.herokuapp.com` 的 URL。

![](img/a4f608f66d892f12b3640cd5547c24da_63.png)

## 8. 测试线上应用

最后，让我们修改测试脚本，指向我们刚部署的线上应用 URL，进行最终测试。

![](img/a4f608f66d892f12b3640cd5547c24da_65.png)

![](img/a4f608f66d892f12b3640cd5547c24da_66.png)

![](img/a4f608f66d892f12b3640cd5547c24da_67.png)

```python
# test_heroku.py
import requests

![](img/a4f608f66d892f12b3640cd5547c24da_69.png)

![](img/a4f608f66d892f12b3640cd5547c24da_70.png)

# 使用 Heroku 应用地址
app_url = 'https://flask-pytorch-tutorial.herokuapp.com'

![](img/a4f608f66d892f12b3640cd5547c24da_71.png)

with open('my_digit_8.png', 'rb') as f:
    img_bytes = f.read()

![](img/a4f608f66d892f12b3640cd5547c24da_73.png)

![](img/a4f608f66d892f12b3640cd5547c24da_74.png)

response = requests.post(f'{app_url}/predict',
                         files={'file': ('my_digit_8.png', img_bytes, 'image/png')})
print('Online App Response:', response.json())
```

![](img/a4f608f66d892f12b3640cd5547c24da_76.png)

运行 `python test_heroku.py`，如果返回正确的预测结果，恭喜你，你的 PyTorch 模型已经成功部署为线上服务！

![](img/a4f608f66d892f12b3640cd5547c24da_77.png)

## 总结 🎉

![](img/a4f608f66d892f12b3640cd5547c24da_79.png)

在本节课中，我们一起学习了将一个 PyTorch 模型部署为 Web 应用的完整流程。

![](img/a4f608f66d892f12b3640cd5547c24da_81.png)

我们首先使用 Flask 创建了一个简单的 REST API 框架，然后集成并封装了 PyTorch 模型的加载和推理逻辑。接着，我们在本地进行了充分的测试。最后，通过配置 `Procfile`、`requirements.txt` 等文件，并使用 Git 将应用成功部署到了 Heroku 云平台，使其能够通过互联网被访问。

![](img/a4f608f66d892f12b3640cd5547c24da_83.png)

这个过程涵盖了从本地开发到线上部署的关键步骤，为你将来部署更复杂的机器学习应用打下了坚实的基础。