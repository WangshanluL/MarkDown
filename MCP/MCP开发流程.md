# 1.在linux环境下搭建uv环境

### 1. uv工具入门使用指南

#### 1.1 uv入门介绍

​        MCP开发要求借助uv进行虚拟环境创建和依赖管理。`uv` 是一个**Python 依赖管理工具**，类似于 `pip` 和 `conda`，但它更快、更高效，并且可以更好地管理 Python 虚拟环境和依赖项。它的核心目标是**替代** **`pip`****、****`venv`** **和** **`pip-tools`**，提供更好的性能和更低的管理开销。

**`uv`** **的特点**：

1. **速度更快**：相比 `pip`，`uv` 采用 Rust 编写，性能更优。
2. **支持 PEP 582**：无需 `virtualenv`，可以直接使用 `__pypackages__` 进行管理。
3. **兼容** **`pip`**：支持 `requirements.txt` 和 `pyproject.toml` 依赖管理。
4. **替代** **`venv`**：提供 `uv venv` 进行虚拟环境管理，比 `venv` 更轻量。
5. **跨平台**：支持 Windows、macOS 和 Linux。

#### 1.2 uv安装流程

**方法 1：使用** **`pip`** **安装（适用于已安装** **`pip`** **的系统）**

```Bash
pip install uv
```

**方法 2：使用** **`curl`** **直接安装**

如果你的系统没有 `pip`，可以直接运行：

```Bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

这会自动下载 `uv` 并安装到 `/usr/local/bin`。

#### 1.3 uv的基本用法介绍

​        安装 `uv` 后，你可以像 `pip` 一样使用它，但它的语法更简洁，速度也更快。注意，以下为使用语法示例，不用实际运行。

- **安装 Python 依赖**

```Bash
uv pip install requests
```

与 `pip install requests` 类似，但更快。

- **创建虚拟环境**

```Bash
uv venv myenv
```

等效于 `python -m venv myenv`，但更高效。

- **激活虚拟环境**

```Bash
source myenv/bin/activate  # Linux/macOS
myenv\Scripts\activate     # Windows
```

- **安装** **`requirements.txt`**

```Bash
uv pip install -r requirements.txt
```

- **直接运行 Python 项目**

如果项目中包含 `pyproject.toml`，你可以直接运行：

```Bash
uv run python script.py
```

这等效于：

```Bash
pip install -r requirements.txt
python script.py
```

但 `uv` 速度更快，管理更高效。

> 为什么MCP更推荐使用uv进行环境管理？

> MCP 依赖的 Python 环境可能包含多个模块，`uv` 通过 `pyproject.toml` 提供更高效的管理方式，并且可以避免 `pip` 的一些依赖冲突问题。此外，`uv` 的包管理速度远超 `pip`，这对于 MCP 这样频繁管理依赖的项目来说是一个很大的优势。

接下来我们尝试先构建一个 MCP 客户端，确保基本逻辑可用，然后再逐步搭建 MCP 服务器进行联调，这样可以**分阶段排查问题**，避免一上来就涉及太多复杂性。

### 2.MCP极简客户端搭建流程

#### 2.1 创建 MCP 客户端项目

```Bash
# 创建项目目录
uv init mcp-client
cd mcp-client
```

![image-20250415112159886](./assets/image-20250415112159886.png)





















让我用一个更具体的例子来解释Resources的概念。

想象你正在开发一个电子商务应用程序，需要让LLM（大型语言模型）访问产品信息和用户信息。

### 实际例子：电子商务应用

```python
from mcp.server.fastmcp import FastMCP

# 创建MCP应用
mcp = FastMCP("电子商城应用")

# 模拟数据库
products_db = {
    "101": {"name": "智能手机", "price": 3999, "description": "最新款智能手机，拍照效果极佳"},
    "102": {"name": "笔记本电脑", "price": 6999, "description": "轻薄高性能笔记本"}
}

users_db = {
    "001": {"name": "张三", "email": "zhangsan@example.com", "vip": True},
    "002": {"name": "李四", "email": "lisi@example.com", "vip": False}
}

# 定义资源

@mcp.resource("products://all")
def get_all_products() -> str:
    """获取所有产品列表"""
    return str(list(products_db.keys()))

@mcp.resource("products://{product_id}")
def get_product(product_id: str) -> str:
    """获取特定产品的详细信息"""
    if product_id in products_db:
        return str(products_db[product_id])
    return "产品不存在"

@mcp.resource("users://{user_id}/profile")
def get_user_profile(user_id: str) -> str:
    """获取用户个人资料"""
    if user_id in users_db:
        return str(users_db[user_id])
    return "用户不存在"
```

### 资源的使用方式

当LLM需要访问数据时，它可以通过这些资源获取：

1. **获取所有产品**
   - 资源URI: `products://all`
   - 返回: `['101', '102']`
2. **获取特定产品信息**
   - 资源URI: `products://101`
   - 返回: `{'name': '智能手机', 'price': 3999, 'description': '最新款智能手机，拍照效果极佳'}`
3. **获取用户资料**
   - 资源URI: `users://001/profile`
   - 返回: `{'name': '张三', 'email': 'zhangsan@example.com', 'vip': True}`

### 为什么要这样设计？

1. **简单的数据获取**：LLM可以轻松请求它需要的数据，类似于浏览器请求网页
2. **不执行复杂计算**：资源只是提供数据，不处理复杂逻辑
3. **动态访问**：通过路径参数（如`{product_id}`），可以动态访问不同的数据
4. **类似REST但更简单**：概念类似于REST API的GET请求，但专为LLM设计

这样，当你的应用程序与LLM集成时，LLM可以通过这些定义好的"资源"获取它需要的信息，而不需要直接访问你的数据库或核心业务逻辑。







我明白了，您希望同时看到客户端和服务器端的示例代码。下面我将为Prompts提供完整的示例，包括服务器端如何定义提示模板，以及客户端如何使用这些提示模板：

### Prompts完整示例：客户支持系统

#### 服务器端代码 (server.py)

```python
from mcp.server.fastmcp import FastMCP
from mcp.server.fastmcp.prompts import base

# 创建MCP应用
mcp = FastMCP("客户支持系统")

# 定义提示模板

@mcp.prompt()
def product_inquiry(product_name: str) -> str:
    """客户询问产品信息的提示模板"""
    return f"""请提供关于{product_name}的详细信息，包括：
    - 主要功能
    - 价格区间
    - 适用人群
    - 常见问题
    """

@mcp.prompt()
def handle_complaint(complaint_text: str, customer_level: str) -> list[base.Message]:
    """处理客户投诉的对话模板"""
    return [
        base.UserMessage("我需要提交以下投诉:"),
        base.UserMessage(complaint_text),
        base.AssistantMessage("非常抱歉听到您遇到了问题。作为我们的" + 
                              ("尊贵" if customer_level == "VIP" else "valued") + 
                              "客户，我们会优先处理您的问题。"),
        base.AssistantMessage("能否请您提供一些额外细节，比如购买日期和订单号？")
    ]

@mcp.prompt()
def technical_support(device_type: str, problem_description: str) -> list[base.Message]:
    """技术支持对话模板"""
    common_solutions = {
        "手机": "请尝试重启设备，并检查是否有系统更新",
        "电脑": "请检查所有连接线缆，并尝试重启计算机",
        "智能家居": "请确认设备已连接到WiFi网络，并且应用程序已更新到最新版本"
    }
    
    default_solution = "请尝试重启设备并检查连接"
    suggestion = common_solutions.get(device_type, default_solution)
    
    return [
        base.SystemMessage(f"这是一个关于{device_type}问题的技术支持对话"),
        base.UserMessage(f"我的{device_type}出现了以下问题：{problem_description}"),
        base.AssistantMessage(f"我理解您的{device_type}遇到了问题。首先，{suggestion}。这解决了问题吗？")
    ]

# 启动服务器
if __name__ == "__main__":
    mcp.start()
```

#### 客户端代码 (client.py)

```python
from mcp.client import Client

# 连接到MCP服务器
client = Client("http://localhost:8000")  # 假设服务器运行在本地8000端口

# 示例1: 使用product_inquiry提示模板
def get_product_info(product_name):
    # 获取提示模板
    prompt = client.prompts.product_inquiry(product_name=product_name)
    
    # 将提示发送给LLM获取回复
    response = client.llm.complete(prompt=prompt)
    
    print(f"关于{product_name}的信息:\n{response}")

# 示例2: 使用handle_complaint提示模板进行对话
def submit_complaint(complaint_text, customer_level):
    # 获取对话模板
    conversation = client.prompts.handle_complaint(
        complaint_text=complaint_text, 
        customer_level=customer_level
    )
    
    # 将对话模板发送给LLM
    for message in conversation:
        if isinstance(message, dict) and message.get("role") == "user":
            print(f"用户: {message['content']}")
        elif isinstance(message, dict) and message.get("role") == "assistant":
            print(f"助手: {message['content']}")
    
    # 用户继续对话
    user_input = input("您的回复: ")
    
    # 将用户输入添加到对话中并获取LLM回复
    response = client.llm.chat(
        messages=conversation + [{"role": "user", "content": user_input}]
    )
    
    print(f"助手: {response}")

# 示例3: 使用technical_support提示进行技术支持
def get_tech_support(device_type, problem):
    # 获取技术支持对话模板
    support_conversation = client.prompts.technical_support(
        device_type=device_type,
        problem_description=problem
    )
    
    # 启动技术支持会话
    session = client.llm.create_chat_session(initial_messages=support_conversation)
    
    # 显示初始对话
    for message in support_conversation:
        if hasattr(message, "role") and message.role == "system":
            continue  # 系统消息通常不展示给用户
        elif hasattr(message, "role") and message.role == "user":
            print(f"用户: {message.content}")
        elif hasattr(message, "role") and message.role == "assistant":
            print(f"助手: {message.content}")
    
    # 交互式对话
    while True:
        user_input = input("您的回复 (输入'退出'结束): ")
        if user_input.lower() == "退出":
            break
            
        # 获取LLM回复
        response = session.send_message(user_input)
        print(f"助手: {response}")


# 运行示例
if __name__ == "__main__":
    print("===== 产品信息查询 =====")
    get_product_info("智能手表")
    
    print("\n===== 投诉处理 =====")
    submit_complaint("我购买的耳机右侧没有声音", "VIP")
    
    print("\n===== 技术支持 =====")
    get_tech_support("手机", "无法连接WiFi")
```

### 运行效果示例

假设运行客户端代码，以下是可能的输出：

```
===== 产品信息查询 =====
关于智能手表的信息:
智能手表是一种可穿戴设备，提供多种功能超越传统手表。

主要功能:
- 健康监测（心率、血氧、睡眠质量）
- 运动追踪（步数、卡路里、运动模式）
- 通知提醒（电话、短信、应用通知）
- 时间显示与闹钟
- 部分型号支持独立通话和移动支付

价格区间:
- 入门级: 299-599元
- 中端: 699-1499元
- 高端: 1999-3999元

适用人群:
- 运动爱好者
- 健康监测需求人群
- 商务人士需要便捷查看通知
- 科技爱好者

常见问题:
1. 电池续航时间多久？通常1-7天不等，取决于型号和使用频率
2. 防水性能如何？大多数支持生活防水，部分支持游泳
3. 是否需要配对手机？大多数智能手表需要与手机配对使用
4. 兼容性如何？请确认与您手机系统的兼容性(Android/iOS)

===== 投诉处理 =====
用户: 我需要提交以下投诉:
用户: 我购买的耳机右侧没有声音
助手: 非常抱歉听到您遇到了问题。作为我们的尊贵客户，我们会优先处理您的问题。
助手: 能否请您提供一些额外细节，比如购买日期和订单号？
您的回复: 我上周二在官网购买的，订单号是XYZ12345
助手: 谢谢您提供订单信息。我已经记录了您的投诉，并创建了一个优先处理工单。我们有两个选择：1) 我可以安排耳机的免费更换，将在3个工作日内送达；2) 我们可以提供技术支持，尝试解决右侧耳机的问题。您更倾向于哪种解决方案？

===== 技术支持 =====
用户: 我的手机出现了以下问题：无法连接WiFi
助手: 我理解您的手机遇到了问题。首先，请尝试重启设备，并检查是否有系统更新。这解决了问题吗？
您的回复 (输入'退出'结束): 我已经重启了，但仍然连不上
助手: 感谢您尝试重启设备。让我们尝试一些其他故障排除步骤：

1. 请进入设置 > WiFi，关闭WiFi开关，等待10秒后再打开
2. 检查您是否不小心开启了飞行模式
3. 尝试忘记该WiFi网络，然后重新连接并输入密码
4. 检查路由器是否正常工作，尝试重启路由器

这些操作中的任何一个解决了问题吗？
您的回复 (输入'退出'结束): 重启路由器后解决了，谢谢！
助手: 太好了！很高兴听到重启路由器解决了问题。这是一个常见的解决方案，因为路由器有时会出现临时故障或缓冲区溢出。

为防止将来发生此类问题，建议您:
1. 每隔几周重启一次路由器
2. 确保路由器固件保持最新
3. 检查路由器的放置位置，确保信号覆盖良好

如果您遇到任何其他技术问题，请随时联系我们的支持团队。祝您使用愉快！
您的回复 (输入'退出'结束): 退出
```

### 解释

在这个完整示例中:

1. **服务器端**:
   - 定义了三种不同类型的提示模板
   - 每种模板接受不同的参数并返回不同格式的提示内容
   - 使用装饰器将它们注册到MCP框架中
2. **客户端**:
   - 连接到MCP服务器
   - 调用服务器上定义的提示模板，传递必要的参数
   - 使用这些提示与LLM进行交互
   - 处理LLM的回复并展示给用户

这种方式的优势在于:

- 提示模板在服务器端集中管理和维护
- 客户端可以轻松重用这些模板而无需复制代码
- 可以轻松更新和优化提示而不需要修改客户端代码
- 确保了所有客户端使用相同的、经过优化的提示策略