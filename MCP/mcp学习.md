    import asyncio
    import mcp.types as types
    from mcp.server import Server
    from mcp.server.stdio import stdio_server
    
    app = Server("example-server")
    
    @app.list_resources()
    async def list_resources() -> list[types.Resource]:
        return [
            types.Resource(
                uri="example://resource",
                name="Example Resource"
            )
        ]
    
    async def main():
        async with stdio_server() as streams:
            await app.run(
                streams[0],
                streams[1],
                app.create_initialization_options()
            )
    
    if __name__ == "__main__":
        asyncio.run(main)

让我更详细地解释这两个关键点：

## URI 中的连接符号

在代码中的 URI `"example://resource"` 中，双斜杠(`//`)的作用是：

1. **协议分隔符**：这是 URI (统一资源标识符) 的标准格式，其中双斜杠分隔了 URI 的方案/协议部分和资源路径部分。
2. **方案定义**：`example:` 定义了一个自定义的方案/协议名称，类似于我们常见的 `http://`, `https://`, `ftp://` 等。
3. **命名空间划分**：这表明 `resource` 是在 `example` 命名空间下的一个资源。

在这种模式下，URI 可以被解析为：

- 方案/协议：`example`
- 层级部分：`resource`

这种自定义 URI 模式常用于服务系统中，为资源提供唯一的标识符，而不依赖于物理存储位置。

## main 函数中的 streams

`streams` 是通过 `stdio_server()` 创建的标准输入/输出流：

```python
async with stdio_server() as streams:
    await app.run(
        streams[0],
        streams[1],
        app.create_initialization_options()
    )
```

这里：

1. **通信通道**：`streams` 是一个包含两个元素的列表或元组，其中：
   - `streams[0]` 代表**输入流**，用于接收来自客户端的消息
   - `streams[1]` 代表**输出流**，用于向客户端发送消息
2. **进程间通信**：`stdio_server()` 建立了基于标准输入/输出的通信机制，这在以下场景特别有用：
   - 语言服务器协议 (LSP) 实现
   - 跨进程通信
   - 命令行工具与其他程序的集成
3. **异步上下文管理**：通过 `async with` 语句，确保了流的正确打开和关闭，避免资源泄漏。

这种设计让服务器能够通过标准输入/输出流接收请求并发送响应，非常适合作为一个独立进程被其他程序调用和管理，例如编辑器插件、开发工具等。这是一种轻量级的进程间通信方法，不需要设置网络端口或套接字。