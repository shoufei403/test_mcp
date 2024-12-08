# Model Context Protocol (MCP) Quickstart

## 概述

MCP（Model Context Protocol）是一个开放协议，用于在主机应用程序（如Claude Desktop）和本地服务之间建立安全连接。本快速入门指南将帮助您：

- 设置本地SQLite数据库
- 通过MCP将Claude Desktop连接到数据库
- 安全地查询和分析数据

## MCP工作原理

MCP遵循客户端-服务器架构：

- **MCP主机**：如Claude Desktop、IDE或AI工具，希望通过MCP访问资源
- **MCP客户端**：与服务器保持1:1连接的协议客户端
- **MCP服务器**：通过标准化的Model Context Protocol公开特定功能的轻量级程序
- **本地资源**：MCP服务器可以安全访问的计算机资源（数据库、文件、服务）
- **远程资源**：MCP服务器可以连接的互联网资源（如API）

## 先决条件

- macOS或Windows
- 最新版Claude Desktop
- uv 0.4.18或更高版本
- Git
- SQLite

## 安装步骤

### 1. 创建示例数据库

使用SQLite创建测试数据库：

```bash
sqlite3 ~/test.db <<EOF
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT,
    price REAL
);

INSERT INTO products (name, price) VALUES 
    ('Widget', 19.99),
    ('Gadget', 29.99),
    ('Gizmo', 39.99);
EOF
```

### 2. 配置Claude Desktop

打开配置文件 `~/Library/Application Support/Claude/claude_desktop_config.json`：

```json
{
    "mcpServers": {
        "sqlite": {
            "command": "uvx",
            "args": ["mcp-server-sqlite", "--db-path", "/Users/YOUR_USERNAME/test.db"]
        }
    }
}
```

## 注意事项

- 当前仅支持连接本地MCP服务器
- 远程MCP连接尚未支持
- 仅在Claude Desktop应用中可用，Web界面不支持

## 安全性

Model Context Protocol确保：
- 仅可执行预批准的数据库操作
- 通过定义明确的接口与本地数据交互
- 完全控制数据访问权限