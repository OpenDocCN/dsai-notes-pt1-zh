# 106：使用Flask部署Web应用程序 🚀

![](img/b92ca5738b30ec2623e6d19f11a3ec38_0.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_1.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_3.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_5.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_6.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_8.png)

在本节课中，我们将学习如何使用Flask框架来创建和部署一个Python Web应用程序。Flask是一个轻量级的“微框架”，能够帮助我们快速、轻松地构建Web应用。我们将了解Flask的核心功能、如何安装它，并一步步创建一个能响应请求的简单Web应用。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_10.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_11.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_13.png)

## 什么是Flask？🤔

![](img/b92ca5738b30ec2623e6d19f11a3ec38_15.png)

Flask是一个用于使用Python快速、轻松创建Web应用程序的微框架。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_17.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_18.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_19.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_20.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_22.png)

Flask支持**CRUD**操作，即通过发起**POST**、**PUT**、**GET**、**UPDATE**和**DELETE**请求来实现**创建**、**读取**、**更新**和**删除**功能。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_24.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_26.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_28.png)

以下是Flask应用程序的基本结构。请注意Flask包的标志。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_30.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_31.png)

## Flask中的CRUD操作

![](img/b92ca5738b30ec2623e6d19f11a3ec38_33.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_34.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_36.png)

上一节我们介绍了Flask的基本概念，本节中我们来看看如何使用Flask进行具体的CRUD操作。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_38.png)

以下是Flask中不同HTTP方法对应的操作：

![](img/b92ca5738b30ec2623e6d19f11a3ec38_40.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_41.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_42.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_43.png)

*   **POST**和**PUT**请求用于创建对象或数据。例如，可以使用POST和PUT请求来创建一个用户。
    *   两者的区别在于：**POST**在每次请求时都会创建对象或数据。
    *   而**PUT**仅在第一次请求时创建对象或数据，并在后续的请求中持续更新该对象或数据。
*   在大多数Web应用程序中，你会看到使用**POST**来创建对象或数据。
*   可以使用**GET**请求从服务器读取数据。
*   可以使用**UPDATE**请求来更新现有的数据或对象。
*   可以使用**DELETE**请求来删除现有的数据或对象。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_45.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_46.png)

需要注意的是，大多数Web应用程序倾向于使用**POST**进行创建、更新和删除对象及数据，而使用**GET**进行读取。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_47.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_49.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_50.png)

另一个视频详细解释了POST、PUT和GET请求。接下来，让我们看看如何使用Flask创建Web应用程序。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_52.png)

## 创建Flask Web应用程序 🛠️

![](img/b92ca5738b30ec2623e6d19f11a3ec38_54.png)

上一节我们了解了Flask支持的请求类型，本节我们将动手创建一个Flask应用。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_56.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_58.png)

以下是创建Flask Web应用程序的步骤：

1.  **安装Flask包**：第一步是使用Python的标准包管理器Pip安装Flask包。像所有其他包一样，你可以使用命令 `pip install flask`，这将获取Flask的最新版本。
    *   **代码示例**：`pip install flask`
2.  **导入Flask模块**：安装Flask包后，就可以创建Web应用程序了。接下来，从flask包中导入Flask模块。为此，输入 `from flask import Flask`。
    *   **代码示例**：`from flask import Flask`
3.  **实例化Flask类**：创建一个Flask类的对象作为Web应用程序，例如命名为“我的第一个应用程序”，并将其存储为`app`。
    *   **代码示例**：`app = Flask("My first application")`
    *   大多数应用程序为了清晰起见使用引用名`app`。然而，`app`只是一个引用名称，你可以使用任何其他名称。
4.  **定义路由和方法**：添加路由以及访问该路由时将调用的方法。例如，输入 `@app.route('/')`。
    *   在这个例子中，既没有指定GET也没有指定POST。当你不指定请求类型时，默认是GET请求。因此，在这个例子中，该端点现在将能够为根路由`/`提供GET请求服务。
5.  **编写处理函数**：将方法写为`def hello():`。当系统访问上一步中定义的API端点时，将调用`hello`方法。它不接受任何参数，并返回字符串“Hello world!”。
    *   **代码示例**：
        ```python
        @app.route('/')
        def hello():
            return 'Hello world!'
        ```
6.  **添加运行条件并启动应用**：添加一个条件，即Web应用程序仅在`__name__`属性设置为`__main__`时才运行。默认情况下，`__name__`被设置为`__main__`，除非被显式更改。你可以像运行任何其他Python应用程序一样保存和运行它。输入 `app.run(debug=True)` 以在开发环境中启动服务器。
    *   **代码示例**：
        ```python
        if __name__ == '__main__':
            app.run(debug=True)
        ```

![](img/b92ca5738b30ec2623e6d19f11a3ec38_60.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_62.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_63.png)

要启动开发环境中的服务器，你需要将代码保存在一个Python文件中，并像运行任何其他Python应用程序一样运行它。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_65.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_67.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_68.png)

当Web应用程序服务器启动时，它会提供可以访问该应用程序的IP地址和端口。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_70.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_71.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_72.png)

要检查端点，你可以打开浏览器并连接到服务器输出中看到的这个端点（通常是`127.0.0.1:5000`），然后看到从Web服务器应用程序返回的字符串“Hello world!”。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_74.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_75.png)

## 使用Flask模板 📄

![](img/b92ca5738b30ec2623e6d19f11a3ec38_76.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_77.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_79.png)

上一节我们创建了一个返回简单文本的应用，本节我们来看看如何用Flask渲染更复杂的HTML页面。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_81.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_82.png)

模板不过是Web应用程序提供的预创建的HTML页面。它们可以是静态的，也可以是动态的。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_84.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_86.png)

默认情况下，Flask应用程序在根目录下名为`templates`的目录中查找模板。如果模板需要使用图像、样式表或JavaScript文件，这些文件存储在根目录下名为`static`的文件夹中。静态页面按原样渲染。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_88.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_89.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_91.png)

动态页面通常包含为每个请求动态填充的信息。这些页面通常基于作为参数传递的值。参数可以通过URL传递，也可以作为请求参数传递。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_93.png)

让我们看一个示例Flask应用程序。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_95.png)

1.  **导入必要模块**：首先导入Flask以创建Web应用程序，导入`request`以处理传入的请求，导入`render_template`以渲染静态和动态HTML页面。
    *   **代码示例**：`from flask import Flask, render_template, request`
2.  **实例化Flask并设置静态文件夹**：接下来，实例化Flask并设置静态文件夹。例如：`app = Flask("My first application")`。默认文件夹名是`static`，但只要显式设置，你可以将静态内容放在不同名称的目录中。
3.  **定义端点**：注意这个Web应用程序中有三个端点。
    *   **第一个是 `/sample`**：这将渲染一个静态HTML页面。此HTML中的图像来源于静态目录。
        *   **代码示例**：
            ```python
            @app.route('/sample')
            def get_sampleHTML():
                return render_template('sample.html')
            ```
    *   **第二个是 `/user/<username>`**：其中尖括号内的`username`是参数。页面使用我们在URL中传递的参数进行渲染。
        *   **代码示例**：
            ```python
            @app.route('/user/<username>', methods=['GET'])
            def get_user(username):
                return render_template('user.html', username=username)
            ```
    *   **第三个是 `/user`**：其中用户名作为请求参数传递。页面使用作为请求传递的参数进行渲染。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_97.png)

## 总结 📝

![](img/b92ca5738b30ec2623e6d19f11a3ec38_99.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_100.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_101.png)

本节课中我们一起学习了如何使用Flask框架。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_103.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_105.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_107.png)

*   你了解到Flask是一个用于创建Web应用程序的微框架，并支持CRUD操作。
*   学习了如何使用Pip安装Flask包。
*   掌握了使用Flask创建Web应用程序的步骤：导入Flask、实例化Flask、运行应用程序。
*   最后，你还学会了如何使用Flask渲染静态和动态模板。

![](img/b92ca5738b30ec2623e6d19f11a3ec38_109.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_111.png)

![](img/b92ca5738b30ec2623e6d19f11a3ec38_113.png)

通过本课的学习，你已经具备了使用Flask搭建简单Web应用的基础知识。