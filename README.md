# FastMCP 邮件服务器

基于 FastMCP 框架构建的邮件收发 MCP 服务器，支持多种邮箱服务商的邮件收发和附件处理功能。

在 https://modelscope.cn/mcp/servers/s3219521aa/MCP_Agent_Challenge_email163_mcp 的基础上增加了用户自定义发送邮箱的功能。

## 🚀 FastMCP 框架特性

FastMCP 提供了简洁的 Python 风格接口，只需使用装饰器即可将函数封装为 MCP 工具：

```python
from fastmcp import FastMCP

mcp = FastMCP("EmailService")

@mcp.tool()
def send_text_email(to_addr: str, subject: str, content: str, account: str, password: str) -> dict:
    """发送纯文本邮件"""
    # 实现邮件发送逻辑
    return {"status": "success", "message": "邮件发送成功"}

if __name__ == "__main__":
    mcp.run(transport="stdio")
```

## 📧 提供的邮件工具

邮件接收
- `get_newest_email`: 获取最新的未读邮件
- `check_emails`: 检查指定类型和数量的邮件  
- `save_attachment`: 保存邮件附件到指定路径

邮件发送
- `send_text_email`: 发送纯文本邮件
- `send_html_email`: 发送HTML格式邮件
- `send_email_with_attachment`: 发送带附件的邮件

服务器配置
- `get_server_config`: 自动获取邮箱服务器配置


工具调用示例

```json
{
  "name": "send_text_email",
  "arguments": {
    "to_addr": "recipient@example.com",
    "subject": "测试邮件",
    "content": "这是一封测试邮件",
    "account": "your-email@163.com", 
    "password": "your-auth-code"
  }
}
```

## ⚡ 快速测试

使用 MCP Inspector 一键拉起 PyPI 包进行测试：

```bash
npx @modelcontextprotocol/inspector uvx fastmcp_email_server
```
相关包已发布至 [PyPI](https://pypi.org/project/fastmcp-email-server/)


MCP 客户端测试配置

```json
{
  "mcpServers": {
    "fastmcp_email_server_pypi": {
      "command": "uvx",
      "args": [
        "fastmcp-email-server"
      ]
    }
  }
}
```

## 📦 本地安装与运行（使用 uv）

安装依赖

```bash
uv sync
```

以模块方式运行（推荐）

```bash
uv run -m fastmcp_email_server
```

或使用安装的脚本入口：

```bash
uv run fastmcp-email-server
```

直接运行源码（可选）

```bash
uv run python src/fastmcp_email_server/server.py
```


使用 MCP Inspector 进行调试（可选）

```bash
uv run mcp dev src/fastmcp_email_server/server.py
```

在支持 MCP 的客户端（Claude Desktop、Cherry Studio、Cursor 等）中配置（可选）

```json
{
  "mcpServers": {
    "email": {
  "command": "uv",
  "args": [
    "run",
    "-m",
    "fastmcp_email_server"
  ],
      "env": {}
    }
  }
}
```

## 🔐 邮箱授权码说明

大多数邮箱服务商需要使用授权码而不是登录密码：

- **163/126邮箱**: 邮箱设置 → 客户端授权密码 → 开启并获取授权码
- **QQ邮箱**: 设置 → 账户 → 开启POP3/IMAP服务并获取授权码
- **Gmail**: 开启两步验证并生成应用专用密码

支持的邮箱服务商

- 163邮箱（163.com）
- 126邮箱（126.com）  
- QQ邮箱（qq.com）
- Gmail（gmail.com）
- Outlook/Hotmail（outlook.com, hotmail.com）
- 阿里云邮箱（aliyun.com）
- 新浪邮箱（sina.com）
  
## 📁 项目结构

```
src/
└── fastmcp_email_server/
    ├── __init__.py          # 包入口，提供命令行主函数
    ├── __main__.py          # 支持 python -m 形式运行
    ├── server.py            # FastMCP 服务器主文件
    ├── email_config.py      # 邮箱服务器配置
    ├── send_163.py          # 邮件发送功能
    └── receive_163.py       # 邮件接收功能
```

## ☁ ModelScope 部署

- 可将 `fastmcp_email_server` 作为模型服务部署到 ModelScope 平台，便于快速上线邮件相关的 MCP 工具能力。
- 见：https://modelscope.cn/mcp/servers/RIckyQ/FastMCP_email_server 
- 支持 Streamable HTTP 和 SSE 协议。
