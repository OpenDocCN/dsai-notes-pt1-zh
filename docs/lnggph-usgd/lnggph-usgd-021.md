# 021：为 LangGraph 添加自定义身份验证 🔐

在本教程中，我们将学习如何为 LangGraph 应用程序添加自定义的身份验证和访问控制功能。我们将分三步进行：首先实现基础的用户认证，然后添加资源级别的授权，最后集成一个真实的生产级身份验证服务。

---

## 概述

LangGraph 平台现已支持自定义身份验证和访问控制。此功能适用于 LangGraph Cloud 和自托管部署。它允许你直接与自己的身份验证服务集成，并在 LangGraph 应用程序中实现自定义权限模型，而无需依赖单独的后端服务器。

我们将通过三个部分来演示如何为现有 LangGraph 应用添加 OAuth 功能。

---

## 第一部分：实现用户身份验证

上一节我们介绍了本教程的目标，本节中我们来看看如何实现基础的身份验证。身份验证会检查每个请求，确保其拥有访问服务所需的凭证，然后返回一个用户对象，供后续进行细粒度控制时使用。

![](img/a050bffbbf0dc283984972667d5c932c_1.png)

首先，我们需要安装 LangGraph 命令行界面并克隆一个新项目。

![](img/a050bffbbf0dc283984972667d5c932c_2.png)

```bash
# 安装 LangGraph CLI 并克隆项目
pip install langgraph-cli
langgraph clone <project-name>
```

克隆项目后，我们可以启动服务器查看其初始状态。安装项目依赖并以可编辑模式运行。

```bash
cd <project-name>
pip install -e .
langgraph dev
```

启动后，你可以测试应用，目前它只是简单地回显响应。这个图的内容并不重要，重要的是我们今天学习的技能可以应用于任何 LangGraph 平台应用。

为了让 LangGraph 知道调用哪个函数来验证每个请求，我们需要使用从 LangGraph SDK 导出的 `auth` 对象。我们将初始化这个对象，并用它来装饰一个自定义函数，告诉 LangGraph 此函数应用于身份验证。

我们的函数将接受一个 `Authorization` 请求头，这是一个在许多验证方案中常见的头部。我们将断言该头部存在，然后检查令牌是否存在于我们的“玩具”数据库中。最后，我们将获取用户信息并返回，这些信息在本教程后续部分会很有用。

以下是实现此功能的代码：

![](img/a050bffbbf0dc283984972667d5c932c_4.png)

![](img/a050bffbbf0dc283984972667d5c932c_5.png)

```python
# security/authentication.py
from langgraph_sdk import Auth

auth = Auth()

@auth.authenticate
async def authenticate(authorization: str | None):
    # 1. 断言 Authorization 头部存在
    if authorization is None:
        raise HTTPException(status_code=401, detail="Missing authorization header")
    
    # 2. 从头部提取令牌（假设格式为 "Bearer <token>"）
    try:
        scheme, token = authorization.split()
        if scheme.lower() != "bearer":
            raise ValueError
    except ValueError:
        raise HTTPException(status_code=401, detail="Invalid authorization header format")
    
    # 3. 检查令牌是否在“玩具”数据库中
    # 这里使用一个简单的字典模拟用户数据库
    user_db = {
        "super_secret_user_1_token": {"user_id": "user_1", "name": "Alice"},
        "super_secret_user_2_token": {"user_id": "user_2", "name": "Bob"},
    }
    
    if token not in user_db:
        raise HTTPException(status_code=401, detail="Invalid token")
    
    # 4. 返回用户信息
    return user_db[token]
```

现在，我们需要让服务器知道这个 `auth` 对象的位置。通过修改 `langgraph.json` 配置文件来实现。

![](img/a050bffbbf0dc283984972667d5c932c_7.png)

![](img/a050bffbbf0dc283984972667d5c932c_9.png)

```json
// langgraph.json
{
  "auth": {
    "file": "./security/authentication.py",
    "variable": "auth"
  }
  // ... 其他配置
}
```

配置完成后，让我们测试我们的实现。我们将创建客户端代码来连接服务器。

首先，创建一个没有有效凭证的客户端，该客户端将无法访问服务。然后，使用正确的请求头重新创建客户端，再次尝试操作，应该会成功。

以下是测试代码示例：

```python
# test_authentication.py
import asyncio
from langgraph_sdk import LangGraphClient

async def test_auth():
    # 测试 1: 无凭证客户端应被阻止
    unauthorized_client = LangGraphClient(base_url="http://localhost:8123")
    try:
        await unauthorized_client.threads.create()
        print("FAIL: Unauthorized request succeeded")
    except Exception:
        print("PASS: Unauthorized request correctly blocked")
    
    # 测试 2: 有凭证客户端应成功
    authorized_client = LangGraphClient(
        base_url="http://localhost:8123",
        headers={"Authorization": "Bearer super_secret_user_1_token"}
    )
    try:
        thread = await authorized_client.threads.create()
        print(f"PASS: Successfully created thread {thread.id}")
        
        # 测试创建运行
        run = await authorized_client.runs.create(thread_id=thread.id, assistant_id="some_assistant")
        print(f"PASS: Successfully created run {run.id}")
    except Exception as e:
        print(f"FAIL: Authorized request failed: {e}")

if __name__ == "__main__":
    asyncio.run(test_auth())
```

运行测试，如果实现正确，我们将看到所有测试通过。未经身份验证的客户端被服务器阻止，而有凭证的客户端能够成功创建线程和运行。

**第一部分总结**：我们学习了如何使用 LangGraph 的 `auth` 对象来拦截未经身份验证的请求。我们创建了一个身份验证函数，将其导出，并在 `langgraph.json` 文件中配置了服务器。然而，目前任何通过身份验证的用户都可以访问任何资源。在下一节中，我们将学习如何创建授权处理程序来过滤资源并使对话私有化。

---

## 第二部分：添加授权处理程序

上一节我们实现了基础身份验证，本节中我们来看看如何添加授权处理程序，以防止已认证用户访问彼此的数据。

在 Part 1 中，我们使用 `auth` 对象并通过 `@auth.authenticate` 装饰一个函数来添加身份验证。要添加授权，我们使用事件处理程序，通过 `@auth.on()` 装饰器来实现。

这意味着在需要对任何资源进行创建、读取或其他操作时，将调用此函数来检查用户是否有权访问该资源。这些函数接收两个参数：
1.  **认证上下文**：包含用户信息以及正在执行的操作。
2.  **值**：实际发送给此操作的数据。对于创建事件，它包含将要保存到数据库的实际数据；对于读取和搜索事件，它只包含搜索或检索信息所需的参数。

每个授权处理程序最多有三项任务：
1.  **创建过滤器**：返回过滤器让系统节点只返回元数据匹配的资源。例如，只返回元数据中 `owner` 键与用户身份匹配的数据。
2.  **处理创建和更新事件**：我们可以添加包含用户身份的元数据，以便保存到数据库。如果不这样做，上述过滤器将永远无法匹配任何内容。
3.  **直接拒绝请求**：在某些条件下直接拒绝访问。

![](img/a050bffbbf0dc283984972667d5c932c_11.png)

![](img/a050bffbbf0dc283984972667d5c932c_12.png)

在讨论第三点之前，让我们先将此处理程序添加到服务中。我们将复制以下处理程序并将其添加到现有的授权文件中。

```python
# security/authorization.py (续接之前的 authentication.py)
from langgraph_sdk import Auth

auth = Auth() # 假设 auth 对象已在此文件中定义

@auth.on("*") # 全局处理程序，匹配所有资源的所有操作
async def authorize_all(context, value):
    user_id = context.user.get("user_id")
    if not user_id:
        # 如果用户信息中没有 user_id，拒绝请求（虽然身份验证已通过，但这是安全后备）
        raise HTTPException(status_code=403, detail="User identity not found")
    
    action = context.action # 例如：'create', 'read', 'search', 'update'
    resource_type = context.resource # 例如：'threads', 'assistants', 'runs'
    
    # 任务 1: 为读取和搜索操作返回过滤器
    if action in ["read", "search"]:
        # 只允许用户访问自己拥有的资源
        return {"filters": {"metadata": {"owner": user_id}}}
    
    # 任务 2: 为创建和更新操作添加所有者元数据
    if action in ["create", "update"]:
        # 确保 value 是字典且包含 metadata 字段
        if not isinstance(value, dict):
            value = {}
        if "metadata" not in value:
            value["metadata"] = {}
        # 将所有者信息注入元数据
        value["metadata"]["owner"] = user_id
        # 返回修改后的值，系统会使用它来保存
        return {"value": value}
    
    # 默认情况下，允许操作继续（不返回任何内容意味着通过）
```

我们添加了这个处理程序，并保持文件的其余部分不变。重启服务后，我们可以运行测试。

对于这些测试，我们将创建两个不同的客户端，一个给 Alice，一个给 Bob。
1.  首先检查 Alice 是否可以创建助手、线程和运行。她应该能与服务交互。
2.  接下来，检查 Bob 是否无法获取 Alice 刚创建的线程信息。如果授权实现正确，他将无法访问此线程。
3.  最后，显示 Bob 仍然可以访问服务，只要他使用自己的信息。我们将列出线程以显示每个用户只能看到自己拥有的数据。

以下是测试代码：

```python
# test_authorization.py
import asyncio
from langgraph_sdk import LangGraphClient

async def test_authorization():
    # 创建 Alice 的客户端
    client_alice = LangGraphClient(
        base_url="http://localhost:8123",
        headers={"Authorization": "Bearer super_secret_user_1_token"} # Alice
    )
    # 创建 Bob 的客户端
    client_bob = LangGraphClient(
        base_url="http://localhost:8123",
        headers={"Authorization": "Bearer super_secret_user_2_token"} # Bob
    )
    
    # 测试 1: Alice 创建资源
    try:
        assistant = await client_alice.assistants.create(name="Alice's Assistant")
        thread_alice = await client_alice.threads.create()
        run = await client_alice.runs.create(thread_id=thread_alice.id, assistant_id=assistant.id)
        print("PASS: Alice successfully created resources.")
    except Exception as e:
        print(f"FAIL: Alice failed to create resources: {e}")
    
    # 测试 2: Bob 无法读取 Alice 的线程
    try:
        fetched_thread = await client_bob.threads.read(thread_id=thread_alice.id)
        print(f"FAIL: Bob should not have access to Alice's thread, but got: {fetched_thread.id}")
    except Exception as e:
        if "403" in str(e) or "not found" in str(e).lower():
            print("PASS: Bob correctly denied access to Alice's thread.")
        else:
            print(f"Unexpected error for Bob: {e}")
    
    # 测试 3: Bob 创建自己的线程
    try:
        thread_bob = await client_bob.threads.create()
        print(f"PASS: Bob successfully created his own thread: {thread_bob.id}")
    except Exception as e:
        print(f"FAIL: Bob failed to create his own thread: {e}")
    
    # 测试 4: 双方只能看到自己的线程
    threads_alice = await client_alice.threads.search()
    threads_bob = await client_bob.threads.search()
    
    print(f"Alice sees {len(threads_alice.data)} thread(s).")
    print(f"Bob sees {len(threads_bob.data)} thread(s).")
    
    # 可选：验证线程 ID 不同
    if len(threads_alice.data) == 1 and len(threads_bob.data) == 1:
        if threads_alice.data[0].id != threads_bob.data[0].id:
            print("PASS: Alice and Bob see distinct threads.")
        else:
            print("FAIL: Alice and Bob see the same thread ID.")
    else:
        print("Check thread counts.")

if __name__ == "__main__":
    asyncio.run(test_authorization())
```

运行测试，应该会再次看到所有绿色对勾。Alice 能够创建资源，Bob 无法访问 Alice 的线程，Bob 创建了自己的线程，并且双方都只看到一个线程。

创建这个授权处理程序很简洁。但如果你想根据资源有不同的行为，或者想更清楚地了解 `value` 字典的内容以获得更多控制权呢？你可以定义作用于单个资源，甚至该资源上特定操作的处理程序。`value` 的类型有对应的路径，因此你知道期望哪些键。

为了说明作用域授权处理程序，我们将创建三个新函数：
1.  第一个函数匹配任何助手操作，并决定拒绝所有请求。这将应用于每个创建、读取、搜索和更新事件。
2.  第二个函数将应用于线程的任何读取事件。由于我们只是读取而不写入，我们可以只返回过滤器，而无需尝试修改任何元数据（修改会被忽略）。
3.  第三个处理程序将匹配线程资源上的任何创建事件。此代码在功能上与我们之前的全局处理程序相同。

通过创建更具作用域的授权处理程序，你可以对部署中实施的访问策略进行更细粒度的控制。

你可能会想知道，如果有两个处理程序都匹配一个操作会发生什么。在这种情况下，LangGraph 只会为给定操作调用最具体的处理程序。这意味着，在线程创建事件上，我们特定的处理程序将被调用，而不是全局处理程序。对于其他操作也是如此。如果我们定义了一个只作用于线程级别的函数，那么它将只被搜索和更新事件调用，而不被读取和创建事件调用，因为后者有更具体的处理程序。LangGraph 不允许注册两个具有完全相同作用域的函数。

![](img/a050bffbbf0dc283984972667d5c932c_14.png)

为了完成第二部分，我将把这些作用域处理程序添加到我们的 `auth` 文件中，然后运行相应的测试。

![](img/a050bffbbf0dc283984972667d5c932c_16.png)

```python
# security/authorization.py (添加作用域处理程序)
@auth.on("assistants.*") # 匹配助手资源的所有操作
async def authorize_assistants(context, value):
    # 拒绝所有对助手的访问
    raise HTTPException(status_code=403, detail="Access to assistants is forbidden for all users.")

@auth.on("threads.read")
async def authorize_threads_read(context, value):
    # 只允许读取用户自己拥有的线程
    user_id = context.user.get("user_id")
    return {"filters": {"metadata": {"owner": user_id}}}

@auth.on("threads.create")
async def authorize_threads_create(context, value):
    # 为创建的线程注入所有者信息
    user_id = context.user.get("user_id")
    if not isinstance(value, dict):
        value = {}
    if "metadata" not in value:
        value["metadata"] = {}
    value["metadata"]["owner"] = user_id
    return {"value": value}
```

重启服务器并查看新测试。现在，我们主要测试的是，由于我们添加了特定的作用域处理程序，没有人可以创建或搜索助手。最后，我们将确认用户仍然能够访问线程资源。

**第二部分总结**：在本例中，我们添加了授权处理程序，以确保用户只能访问他们拥有的资源。我们还实现了更具作用域的处理程序，以在不同资源上实施更定制化和限制性的策略。现在，你已掌握在服务中实现身份验证和授权所需的所有技能。在本教程的第三部分，我们将用真实的生产级身份验证服务替换我们的玩具用户数据库，以展示如何为生产应用程序管理用户。

---

## 第三部分：集成生产级身份验证服务

上一节我们实现了应用内的授权，本节中我们来看看如何连接到一个真实的外部身份验证服务。我们将使用 Supabase 来演示，它让起步变得容易。如果你要跟着操作，请确保你有一个活跃的 Supabase 账户并创建了新项目。

回顾本视频开头，我们的身份验证服务有三个主要组件：
1.  **授权服务器**：在我们的案例中是 Supabase。它负责生成用户令牌给客户端，并由后端服务器验证。
2.  **应用后端**：在我们的案例中就是 LangGraph 应用程序。这是你所有 AI 业务逻辑所需的一切。
3.  **客户端应用程序**：通常是让用户访问你服务的 Web 或移动应用。

我们将实现的流程大致如下：
1.  用户的客户端发起登录。
2.  他们将有效的凭证发送到身份验证服务器，服务器将响应一个签名令牌。
3.  客户端随后在其向我们的应用后端发出的每个请求的头部中包含此令牌。
4.  我们的后端将查看该令牌，并根据身份验证服务器验证它。
5.  如果有效，则继续处理，可以返回对话数据或调用你的应用逻辑。
6.  如果令牌无效，我们的身份验证处理程序将在任何其他处理之前拒绝该请求。

在更新处理程序之前，让我们确保拥有连接服务器所需的信息。我们的服务器需要两条信息：你项目的 URL 和用于连接它的密钥。我们的客户端则需要访问此 URL 和一个公钥。公钥让客户端能够安全地生成访问你服务的凭证。

你可以在 Supabase 项目的设置中找到这些信息。进入项目设置，点击“API”。这些设置包含你的项目 URL、公钥和私有的服务角色 JWT 密钥。将这些信息从仪表板复制到本地的 `.env` 文件中。

```bash
# .env
SUPABASE_URL=your_supabase_project_url
SUPABASE_ANON_KEY=your_supabase_anon_public_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_secret_jwt
```

现在是将身份验证服务器集成进来的时候了。回顾 Part 1 和 Part 2，我们使用 LangGraph 的 `auth` 对象来装饰一个验证用户声明的身份验证函数。为了集成身份验证服务器，我们只需要修改这个 `authenticate` 函数。

我们将通过连接到 Supabase 后端并获取用户信息来实现。如果请求成功，我们可以返回该信息。否则，我们知道服务器已拒绝该请求。为了避免在此处进行 API 调用，你可以改为使用项目设置中的 JWT 密钥来验证用户的持有者令牌，或者至少缓存请求以避免重复调用。

让我们复制这个新的身份验证处理程序并将其添加到我们的应用程序中。我们所有的授权处理程序可以保持不变。

```python
# security/authentication_prod.py (更新后的身份验证)
import os
from supabase import create_client, Client
from langgraph_sdk import Auth
from fastapi import HTTPException

![](img/a050bffbbf0dc283984972667d5c932c_18.png)

auth = Auth()

![](img/a050bffbbf0dc283984972667d5c932c_19.png)

# 从环境变量加载 Supabase 配置
SUPABASE_URL = os.getenv("SUPABASE_URL")
SUPABASE_SERVICE_ROLE_KEY = os.getenv("SUPABASE_SERVICE_ROLE_KEY")

supabase: Client = create_client(SUPABASE_URL, SUPABASE_SERVICE_ROLE_KEY)

@auth.authenticate
async def authenticate_production(authorization: str | None):
    if authorization is None:
        raise HTTPException(status_code=401, detail="Missing authorization header")
    
    try:
        scheme, token = authorization.split()
        if scheme.lower() != "bearer":
            raise ValueError
    except ValueError:
        raise HTTPException(status_code=401, detail="Invalid authorization header format")
    
    # 使用 Supabase 管理 API 验证 JWT 并获取用户信息
    try:
        # 注意：Supabase Python 客户端可能需要使用 admin 方法。
        # 这里假设使用 `auth.get_user` 方法，具体方法请参考 Supabase 文档。
        # 以下为示例逻辑：
        response = supabase.auth.get_user(jwt=token)
        user = response.user
        if user is None:
            raise HTTPException(status_code=401, detail="Invalid or expired token")
        
        # 返回用户信息，格式与之前玩具数据库一致
        return {
            "user_id": user.id,
            "email": user.email,
            # 可以添加其他所需字段
        }
    except Exception as e:
        # 捕获 Supabase 客户端或网络错误
        print(f"Auth validation error: {e}")
        raise HTTPException(status_code=401, detail="Failed to validate authentication")
```

更新 `langgraph.json` 以指向新的身份验证文件。

```json
// langgraph.json
{
  "auth": {
    "file": "./security/authentication_prod.py",
    "variable": "auth"
  }
  // ... 其他配置
}
```

重启服务器进行测试。我们的客户端代码比 Part 1 和 Part 2 更复杂，因为我们需要连接到真实的身份验证服务器。

首先，创建两个用户。你可以在笔记本中运行代码，并提供你可以访问的有效电子邮件，然后包含来自 `.env` 文件的 Supabase 项目 URL 以及你的公钥。记住，你可以从项目设置的 API 设置中获取你的匿名密钥。在继续之前，你必须检查你的电子邮件并确认账户，否则 Supabase 将拒绝进一步的登录尝试。

以下是创建用户和登录的示例客户端代码：

![](img/a050bffbbf0dc283984972667d5c932c_21.png)

![](img/a050bffbbf0dc283984972667d5c932c_22.png)

```python
# test_production_auth.py
import asyncio
import os
from supabase import create_client
from langgraph_sdk import LangGraphClient

SUPABASE_URL = os.getenv("SUPABASE_URL")
SUPABASE_ANON_KEY = os.getenv("SUPABASE_ANON_KEY")

async def test_production_auth():
    # 初始化 Supabase 客户端（用于客户端操作）
    supabase_client = create_client(SUPABASE_URL, SUPABASE_ANON_KEY)
    
    # 1. 创建用户（仅第一次运行需要，之后可以注释掉）
    email_user1 = "alice@example.com"
    password = "secure_password_123"
    
    try:
        # 注意：Supabase 可能需要先配置电子邮件模板或禁用电子邮件确认才能直接登录。
        # 以下为示例，生产环境请妥善处理用户注册流程。
        auth_response = supabase_client.auth.sign_up({
            "email": email_user1,
            "password": password,
        })
        print(f"User created (or exists): {auth_response.user.email}")
        # 重要：检查邮箱并点击确认链接！
    except Exception as e:
        print(f"Sign up might have failed (user may exist): {e}")
    
    # 2. 登录获取令牌
    try:
        sign_in_response = supabase_client.auth.sign_in_with_password({
            "email": email_user1,
            "password": password
        })
        access_token = sign_in_response.session.access_token
        print("Successfully obtained access token.")
    except Exception as e:
        print(f"Login failed: {e}. Ensure email is confirmed.")
        return
    
    # 3. 使用令牌创建 LangGraph 客户端
    client_user1 = LangGraphClient(
        base_url="http://localhost:8123",
        headers={"Authorization": f"Bearer {access_token}"}
    )
    
    # 4. 测试用户能否创建线程
    try:
        thread = await client_user1.threads.create()
        print(f"PASS: User 1 successfully created thread {thread.id}")
    except Exception as e:
        print(f"FAIL: User 1 failed to create thread: {e}")
    
    # 5. 测试未经验证的用户（使用无效令牌）无法访问
    unauthorized_client = LangGraphClient(
        base_url="http://localhost:8123",
        headers={"Authorization": "Bearer invalid_token"}
    )
    try:
        await unauthorized_client.threads.read(thread_id=thread.id)
        print("FAIL: Unauthorized user accessed thread")
    except Exception as e:
        if "401" in str(e) or "403" in str(e):
            print("PASS: Unauthorized user correctly blocked.")
        else:
            print(f"Unexpected error for unauthorized user: {e}")
    
    # 6. （可选）创建第二个用户并测试数据隔离
    # ... 类似之前的测试，使用不同的邮箱/密码登录获取新令牌

if __name__ == "__main__":
    asyncio.run(test_production_auth())
```

运行代码以确认一切是否按预期实现。你应该看到测试通过的绿色对勾，确认我们的身份验证服务按预期工作。

**第三部分总结**：在本节中，你成功设置了身份验证提供程序。你添加了具有电子邮件和密码身份验证的真实用户账户。你将 JWT 令牌验证集成到 LangGraph 服务器中，并实施了适当的授权以确保用户只能访问自己的数据。这些都是创建基于 LangGraph 的全栈应用程序所需的全部技能。

---

## 课程总结

在本节课中，我们一起学习了如何为 LangGraph 应用程序逐步添加完整的身份验证和授权功能。

1.  **第一部分**：我们实现了基础的身份验证，使用 `@auth.authenticate` 装饰器来验证用户凭证，并拦截未经授权的请求。
2.  **第二部分**：我们引入了授权机制，使用 `@auth.on()` 装饰器创建处理程序，实现了资源级别的访问控制，确保用户数据隔离。我们还探讨了全局和作用域处理程序的用法与优先级。
3.  **第三部分**：我们将应用与 Supabase 生产级身份验证服务集成，演示了如何验证 JWT 令牌，从而构建一个安全、可扩展的全栈 AI 应用。

![](img/a050bffbbf0dc283984972667d5c932c_24.png)

你现在已经掌握了在 LangGraph 平台实施自定义身份验证和访问控制的核心技能。要了解更多，你可以尝试使用 LangGraph 平台教程自己完成所有这些操作，也可以查看我们关于在 LangGraph 中实现身份验证和访问控制的概念指南，并查阅 LangGraph SDK 参考以获取有关 `auth` 对象及相关方法的所有详细信息。请继续关注更多关于如何在 LangGraph 平台中实现常见身份验证和访问控制模式的指南。下次见！