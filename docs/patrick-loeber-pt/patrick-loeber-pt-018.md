# 018：使用 Flask 与 Heroku 创建并部署深度学习应用 🚀

在本节课中，我们将学习如何将一个训练好的 PyTorch 模型封装成 Web API，并使用 Flask 框架和 Heroku 云平台进行部署。最终，我们将创建一个可以接收手写数字图片并返回预测结果的在线应用。

## 概述 📋

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_1.png)

我们将分步完成以下任务：
1.  创建一个简单的 Flask 应用，提供一个 REST API 端点。
2.  编写代码加载 PyTorch 模型并对上传的图片进行推理。
3.  在本地测试整个应用流程。
4.  使用 Gunicorn 作为生产服务器，并将应用部署到 Heroku 平台。

现在，让我们从设置开发环境开始。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_3.png)

## 1. 项目初始化与环境搭建

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_5.png)

首先，我们需要创建一个新的项目目录并设置 Python 虚拟环境，以隔离项目依赖。

以下是创建和激活虚拟环境的步骤：

*   **创建项目目录**：`mkdir pytorch_flask_tutorial && cd pytorch_flask_tutorial`
*   **创建虚拟环境**：`python3 -m venv venv`
*   **激活虚拟环境**：
    *   **macOS/Linux**: `source venv/bin/activate`
    *   **Windows**: `venv\Scripts\activate`

> **注意**：Windows 系统上的命令可能略有不同，具体可参考 [Flask 官方安装指南](https://flask.palletsprojects.com/en/stable/installation/)。

环境激活后，我们安装必要的依赖包。

以下是需要安装的核心包：

*   `pip install flask`
*   `pip install torch torchvision`
*   `pip install requests` (用于后续测试)
*   `pip install gunicorn` (用于生产环境部署)

安装完成后，我们创建应用的主要代码结构。

## 2. 创建 Flask 应用骨架

我们在项目内创建一个 `app` 目录，并在其中编写 Flask 应用的主文件。

首先，在 `app` 目录下创建 `main.py` 文件，并构建一个基础的 API 端点。

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/predict', methods=['POST'])
def predict():
    # 这是一个临时的返回，用于测试API是否通畅
    return jsonify({'result': 1})

if __name__ == '__main__':
    app.run(debug=True)
```

这个 `predict` 函数将是我们处理预测请求的核心。接下来，我们需要在本地运行并测试这个基础应用。

在终端中，进入 `app` 目录，并设置 Flask 环境变量以启动开发服务器。

```bash
export FLASK_APP=main.py
export FLASK_ENV=development
flask run
```

> **注意**：在 Windows 上，设置环境变量应使用 `set` 命令，例如 `set FLASK_APP=main.py`。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_7.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_8.png)

服务器启动后，默认运行在 `http://localhost:5000`。为了测试 `/predict` 端点，我们创建一个简单的测试脚本。

在项目根目录下创建 `test` 文件夹和 `test.py` 文件。

```python
import requests

# 向本地运行的Flask应用发送POST请求
response = requests.post('http://localhost:5000/predict')
print(response.text)  # 预期输出: {"result": 1}
```

运行测试脚本 `python test.py`，如果看到 `{"result": 1}` 的返回，说明 Flask 应用的基础框架已搭建成功。

上一节我们搭建了 Flask 应用的基础框架并验证了 API 的连通性。本节中，我们将把重心转移到模型推理部分，实现从接收图片到返回预测结果的完整逻辑。

## 3. 准备 PyTorch 模型

我们需要一个训练好的模型。这里以 MNIST 手写数字识别为例，使用一个简单的前馈神经网络。

假设我们已有训练脚本（例如 `tutorial_13_feed_forward.py`），在训练完成后，使用以下代码保存模型：

```python
torch.save(model.state_dict(), 'mnist_ffn.pth')
```

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_10.png)

**关键点**：确保训练时使用的数据预处理变换与后续推理时一致。例如，在训练中我们使用了 `ToTensor()` 和 `Normalize((0.1307,), (0.3081,))` 两个变换。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_12.png)

将保存好的模型文件 `mnist_ffn.pth` 复制到我们的 `app` 目录下。

## 4. 实现模型推理模块

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_14.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_15.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_17.png)

在 `app` 目录下，我们创建一个新的文件 `torch_utils.py`，用于封装所有与 PyTorch 模型相关的操作。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_19.png)

这个模块主要完成三件事：加载模型、预处理图像、执行预测。

首先，我们定义模型结构并加载训练好的权重。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_21.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_22.png)

```python
import torch
import torch.nn as nn
import torchvision.transforms as transforms
from PIL import Image
import io

# 1. 定义模型结构 (需与训练时一致)
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

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_24.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_25.png)

# 超参数
input_size = 784  # 28x28 像素
hidden_size = 500
num_classes = 10

# 2. 实例化模型并加载权重
model = NeuralNet(input_size, hidden_size, num_classes)
model.load_state_dict(torch.load('mnist_ffn.pth', map_location=torch.device('cpu')))
model.eval()  # 重要：将模型设置为评估模式
```

接下来，我们编写图像预处理函数。这个函数接收图像的二进制数据，并将其转换为模型所需的张量格式。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_27.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_28.png)

```python
def transform_image(image_bytes):
    """将上传的图片字节数据转换为模型输入张量"""
    transform = transforms.Compose([
        transforms.Grayscale(num_output_channels=1), # 转换为灰度图
        transforms.Resize((28, 28)),                 # 调整大小为28x28
        transforms.ToTensor(),                       # 转换为张量
        transforms.Normalize((0.1307,), (0.3081,))   # 归一化，参数与训练时一致
    ])

    # 从字节数据创建PIL图像
    image = Image.open(io.BytesIO(image_bytes))
    # 应用变换并添加批次维度 (batch dimension)
    return transform(image).unsqueeze(0)
```

最后，我们编写预测函数，它使用预处理后的张量进行前向传播并返回预测结果。

```python
def get_prediction(image_tensor):
    """对输入张量进行预测，返回预测的类别索引"""
    with torch.no_grad():  # 禁用梯度计算，节省内存和计算资源
        outputs = model(image_tensor)
        _, predicted = torch.max(outputs.data, 1)
        return predicted
```

至此，模型推理的后端逻辑已准备完毕。接下来，我们需要在 Flask 的主应用中集成这个模块。

## 5. 集成模型到 Flask API

回到 `app/main.py` 文件，我们导入刚写好的工具函数，并完善 `predict` 函数。

首先，在文件顶部添加导入语句。

```python
from torch_utils import transform_image, get_prediction
```

现在，我们来充实 `predict` 函数。这个函数需要处理以下流程：
1.  检查请求中是否包含文件。
2.  验证文件格式（如 PNG、JPG）。
3.  读取文件并调用 `transform_image` 进行预处理。
4.  调用 `get_prediction` 进行推理。
5.  将结果以 JSON 格式返回。

同时，我们需要添加一些错误处理来提高应用的健壮性。

以下是更新后的 `predict` 函数核心代码：

```python
# 定义允许的文件扩展名
ALLOWED_EXTENSIONS = {'png', 'jpg', 'jpeg'}
def allowed_file(filename):
    return '.' in filename and filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS

@app.route('/predict', methods=['POST'])
def predict():
    if request.method == 'POST':
        # 1. 检查请求中是否有文件部分
        if 'file' not in request.files:
            return jsonify({'error': 'No file part'})
        file = request.files['file']
        # 如果用户没有选择文件，浏览器也可能提交一个空文件
        if file.filename == '':
            return jsonify({'error': 'No selected file'})
        # 2. 检查文件类型是否允许
        if not allowed_file(file.filename):
            return jsonify({'error': 'File type not supported'})

        try:
            # 3. 读取文件字节并转换
            img_bytes = file.read()
            tensor = transform_image(img_bytes)
            # 4. 进行预测
            prediction = get_prediction(tensor)
            # 5. 准备返回数据
            data = {'prediction': prediction.item(), 'class_name': str(prediction.item())}
            return jsonify(data)
        except Exception as e:
            # 简单的异常处理
            return jsonify({'error': 'Error during prediction'})
```

现在，我们的 Flask 应用已经具备了完整的图像预测功能。让我们在本地进行更全面的测试。

## 6. 本地测试完整流程

首先，确保 Flask 开发服务器正在运行（如果已停止，再次执行 `flask run`）。

我们修改之前的测试脚本 `test.py`，使其能够上传一张真实的 MNIST 数字图片进行测试。

```python
import requests

# 替换为你本地保存的MNIST数字图片路径，例如一个数字7的图片
with open('7.png', 'rb') as f:
    img = f.read()

# 向预测API发送POST请求，附带图片文件
response = requests.post('http://localhost:5000/predict',
                         files={'file': ('7.png', img, 'image/png')})

print(response.json())
```

运行测试脚本 `python test.py`。如果一切正常，你将收到一个包含预测数字（例如 `7`）的 JSON 响应。

你还可以使用绘图工具（如系统自带的画图软件）自己绘制一个黑底白字的数字图片，保存为 PNG 格式，并用测试脚本上传，观察模型的预测结果。

本地测试成功后，我们的应用就准备就绪了。接下来，我们将着手将其部署到生产环境的 Heroku 平台。

上一节我们完成了本地 Flask 应用与 PyTorch 模型的集成与测试。本节中，我们来看看如何为生产环境配置应用，并将其部署到 Heroku 云平台。

## 7. 为生产环境配置应用

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_30.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_32.png)

在部署到 Heroku 之前，我们需要进行几项配置，以确保应用能在生产服务器上稳定运行。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_34.png)

首先，我们需要一个 WSGI（Web Server Gateway Interface）服务器来替代 Flask 内置的开发服务器。这里我们使用 **Gunicorn**。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_36.png)

在项目根目录（`pytorch_flask_tutorial/`）下创建 `wsgi.py` 文件，作为 Gunicorn 的入口点。

```python
from app.main import app

if __name__ == "__main__":
    app.run()
```

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_38.png)

接着，我们需要指定 Heroku 运行应用所需的命令。在根目录创建 `Procfile` 文件（无扩展名）。

```yaml
web: gunicorn wsgi:app
```

然后，我们需要固定 Python 版本。在根目录创建 `runtime.txt` 文件。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_40.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_41.png)

```
python-3.8.13
```

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_43.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_45.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_47.png)

**关键调整**：Heroku 对应用大小有限制，而完整的 PyTorch 包体积很大。因此，我们必须使用 **CPU 版本** 的 PyTorch。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_49.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_50.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_52.png)

更新 `requirements.txt` 文件，使用针对 Linux 的 PyTorch CPU 版本安装命令。

```txt
torch==1.9.0+cpu
torchvision==0.10.0+cpu
-f https://download.pytorch.org/whl/torch_stable.html
flask
gunicorn
pillow
requests
```

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_54.png)

> **注意**：`torch` 和 `torchvision` 的版本号后必须带有 `+cpu` 后缀，并指定 PyTorch 的 wheel 文件索引链接。

最后，由于项目结构变化，我们需要更新 `app/torch_utils.py` 中模型文件的加载路径，使其适应部署后的绝对路径。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_56.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_57.png)

```python
# 将原来的相对路径改为从'app'包内查找
model.load_state_dict(torch.load('app/mnist_ffn.pth', map_location=torch.device('cpu')))
```

同时，更新 `app/main.py` 中的导入语句。

```python
from app.torch_utils import transform_image, get_prediction
```

现在，我们可以在本地模拟 Heroku 环境进行测试。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_59.png)

```bash
# 在项目根目录下执行
heroku local
```

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_60.png)

此命令会读取 `Procfile`，并使用 Gunicorn 在本地启动应用。你可以再次运行 `test.py` 脚本（确保 URL 指向 `http://localhost:5000`）来验证配置是否正确。

## 8. 部署到 Heroku

在部署之前，请确保你已经拥有 Heroku 账户并安装了 Heroku CLI。你可以通过 `heroku login` 命令登录。

以下是部署步骤：

首先，在项目根目录初始化 Git 仓库并创建 `.gitignore` 文件，忽略虚拟环境等不必要的文件。

```bash
git init
echo "venv/" >> .gitignore
echo "__pycache__/" >> .gitignore
echo "*.pyc" >> .gitignore
echo "test/" >> .gitignore # 忽略测试目录
```

然后，在 Heroku 上创建一个新的应用。

```bash
heroku create flask-pytorch-mnist-demo
```

将 Heroku 的 Git 仓库添加为远程仓库。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_62.png)

```bash
heroku git:remote -a flask-pytorch-mnist-demo
```

提交代码并推送到 Heroku。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_64.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_65.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_66.png)

```bash
git add .
git commit -m "Initial deployment commit"
git push heroku master
```

`git push` 命令会触发 Heroku 的构建过程，它会自动安装 `requirements.txt` 中的依赖并按照 `Procfile` 启动应用。这个过程可能需要几分钟。

部署成功后，终端会显示你的应用在线地址，例如 `https://flask-pytorch-mnist-demo.herokuapp.com`。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_68.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_69.png)

## 9. 测试线上应用

部署完成后，我们修改测试脚本，将请求发送到线上地址进行最终验证。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_71.png)

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_72.png)

更新 `test.py` 中的 URL：

```python
response = requests.post('https://flask-pytorch-mnist-demo.herokuapp.com/predict',
                         files={'file': ('8.png', img, 'image/png')})
```

运行测试脚本，如果收到正确的预测结果，恭喜你！你的 PyTorch 模型已经成功通过 Flask API 部署到了互联网上，任何人都可以通过发送 HTTP 请求来使用它进行手写数字识别。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_74.png)

## 总结 🎉

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_76.png)

在本节课中，我们一起学习了将一个 PyTorch 模型投入实际使用的完整流程：

1.  **环境搭建**：创建虚拟环境并安装依赖。
2.  **后端开发**：使用 Flask 构建提供预测功能的 REST API。
3.  **模型集成**：加载训练好的 PyTorch 模型，并编写图像预处理和推理代码。
4.  **错误处理**：在 API 中添加文件检查和异常捕获，使应用更健壮。
5.  **生产配置**：使用 Gunicorn 作为 WSGI 服务器，并配置 Heroku 所需的文件（`Procfile`, `runtime.txt`, 适配 CPU 的 `requirements.txt`）。
6.  **部署上线**：通过 Git 将代码部署到 Heroku 云平台。

![](img/04aa32dd29565ddb6ddf93fb7c63abd3_78.png)

你现在已经掌握了创建和部署一个简易深度学习 Web 应用的核心技能。你可以尝试用更复杂的模型（如 CNN）、不同的数据集（如 CIFAR-10，需要返回类别名称映射）来扩展这个应用，或者为前端添加一个简单的 HTML 页面来上传图片和显示结果。