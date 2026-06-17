---
title: MCP 协议：给 AI 装上"万能接口"，终于不用每个工具都写适配器了
date: 2026-06-17 17:30:00
tags:
  - AI
  - MCP
  - 开发工具
  - Agent
categories:
  - 技术
cover: https://images.unsplash.com/photo-1677442136019-21780ecad995?w=800
---

你有没有这种体验：想让 AI 帮你查个数据库，得写一套适配器；想让它调用公司内部 API，又得写一套；想让它读个文件，还得写一套。每个 AI 工具（Claude、GPT、Gemini）的接口都不一样，同样的功能要重复开发 N 遍。

<!-- more -->

## 痛点：AI 工具集成的困境

现在的 AI 应用开发，面临一个尴尬的现实：

1. **每个 AI 模型都有自己的工具调用方式**：Claude 用 Tool Use，GPT 用 Function Calling，Gemini 用 Extensions，格式各不相同
2. **每个外部服务都要写适配器**：数据库、API、文件系统、Git... 每个都要写一套代码
3. **重复劳动严重**：同样的"读文件"功能，给 Claude 写一遍，给 GPT 又写一遍
4. **维护成本高**：AI 模型升级了，适配器可能要重写

这就像早期的手机充电器——每家厂商一个接口，出门得带一堆线。

## MCP：AI 世界的 USB-C

**MCP（Model Context Protocol，模型上下文协议）** 是 Anthropic 在 2024 年 11 月推出的开放标准，目标很简单：**让 AI 应用和外部工具之间的通信标准化**。

你可以把 MCP 理解为 **AI 世界的 USB-C 接口**：

- **USB-C 之前**：每个设备一个充电口，混乱不堪
- **USB-C 之后**：一个接口搞定所有设备，统一标准

MCP 对 AI 应用做的就是这件事——定义一套标准协议，让 AI 模型可以用同一种方式调用各种外部工具。

## MCP 的核心架构

MCP 采用 **客户端-服务器架构**，有三个核心角色：

```
┌─────────────────────────────────────────────────────────────┐
│                      Host（宿主应用）                        │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              MCP Client（MCP 客户端）                │   │
│  │         负责与 MCP Server 建立连接和通信              │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ 标准化协议（JSON-RPC 2.0）
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                   MCP Server（MCP 服务器）                   │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐        │
│  │   Tools     │  │  Resources  │  │  Prompts    │        │
│  │   工具      │  │   资源      │  │   提示词    │        │
│  └─────────────┘  └─────────────┘  └─────────────┘        │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │   外部服务/工具   │
                    │  数据库、API、   │
                    │  文件系统、Git   │
                    └─────────────────┘
```

### 三个核心角色

| 角色 | 说明 | 例子 |
|------|------|------|
| **Host（宿主）** | 运行 AI 模型的应用 | Claude Desktop、Cursor、你的自定义应用 |
| **MCP Client** | 客户端，负责与 Server 通信 | 内置在 Host 中，一个 Host 可以有多个 Client |
| **MCP Server** | 服务器，提供具体功能 | 文件系统 Server、数据库 Server、GitHub Server |

### 三大能力

MCP Server 可以提供三种类型的能力：

#### 1. Tools（工具）
让 AI 可以执行操作，比如：
- 查询数据库
- 调用 API
- 发送邮件
- 执行代码

```json
{
  "name": "query_database",
  "description": "查询 PostgreSQL 数据库",
  "inputSchema": {
    "type": "object",
    "properties": {
      "sql": {
        "type": "string",
        "description": "SQL 查询语句"
      }
    }
  }
}
```

#### 2. Resources（资源）
让 AI 可以读取数据，比如：
- 读取文件内容
- 获取数据库表结构
- 读取配置信息

```json
{
  "uri": "file:///path/to/file.txt",
  "name": "配置文件",
  "description": "应用的配置文件",
  "mimeType": "text/plain"
}
```

#### 3. Prompts（提示词）
预定义的提示词模板，比如：
- 代码审查模板
- 数据分析模板
- 报告生成模板

## 实际例子：用 MCP 连接数据库

假设你想让 Claude 查询你的 PostgreSQL 数据库，传统方式需要写一堆适配代码。用 MCP，只需要几行配置：

### 1. 安装 MCP Server

```bash
# 安装 PostgreSQL MCP Server
npm install -g @modelcontextprotocol/server-postgres
```

### 2. 配置 Claude Desktop

在 `claude_desktop_config.json` 中添加：

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-postgres",
        "postgresql://localhost:5432/mydb"
      ]
    }
  }
}
```

### 3. 直接使用

配置完成后，你可以直接对 Claude 说：

> "帮我查询 users 表中有多少活跃用户"

Claude 会自动调用 MCP Server 执行 SQL 查询，返回结果。**不需要写任何适配代码**。

## MCP 的优势

| 优势 | 说明 |
|------|------|
| 🔌 **标准化** | 一次开发，到处使用。同一个 MCP Server 可以被 Claude、GPT、Gemini 等任何支持 MCP 的 AI 使用 |
| 🔒 **安全性** | 权限控制在 Server 端，AI 不能直接访问敏感数据 |
| 🧩 **模块化** | 每个功能独立一个 Server，按需组合 |
| 🔄 **可复用** | 社区已经有大量现成的 MCP Server，直接用 |
| 📈 **可扩展** | 需要新功能？写一个新的 MCP Server 就行 |

## 现有的 MCP Server 生态

社区已经开发了大量的 MCP Server：

| Server | 功能 | GitHub Stars |
|--------|------|--------------|
| **filesystem** | 文件系统读写 | 1.2k+ |
| **postgres** | PostgreSQL 数据库 | 800+ |
| **github** | GitHub API 集成 | 1.5k+ |
| **slack** | Slack 消息发送 | 600+ |
| **google-drive** | Google Drive 文件 | 400+ |
| **brave-search** | 网络搜索 | 700+ |

完整列表：https://github.com/modelcontextprotocol/servers

## 如何开发自己的 MCP Server

用 TypeScript 开发一个简单的 MCP Server：

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

// 创建 MCP Server
const server = new Server(
  { name: "my-server", version: "1.0.0" },
  { capabilities: { tools: {} } }
);

// 注册工具
server.setRequestHandler("tools/list", async () => ({
  tools: [
    {
      name: "get_weather",
      description: "获取天气信息",
      inputSchema: {
        type: "object",
        properties: {
          city: { type: "string", description: "城市名称" }
        },
        required: ["city"]
      }
    }
  ]
}));

// 处理工具调用
server.setRequestHandler("tools/call", async (request) => {
  const { name, arguments: args } = request.params;
  
  if (name === "get_weather") {
    const weather = await fetchWeather(args.city);
    return {
      content: [{ type: "text", text: JSON.stringify(weather) }]
    };
  }
});

// 启动服务器
const transport = new StdioServerTransport();
await server.connect(transport);
```

## MCP 与其他方案对比

| 对比项 | MCP | Function Calling | 自定义适配器 |
|--------|-----|------------------|--------------|
| 标准化 | ✅ 统一标准 | ❌ 各模型不同 | ❌ 完全自定义 |
| 可复用 | ✅ 一次开发 | ❌ 每个模型重写 | ❌ 无法复用 |
| 生态 | ✅ 社区丰富 | ⚠️ 依赖厂商 | ❌ 无 |
| 安全性 | ✅ Server 端控制 | ⚠️ 取决于实现 | ⚠️ 取决于实现 |
| 学习成本 | ⚠️ 中等 | ✅ 低 | ⚠️ 高 |

## 适用场景

1. **企业内部 AI 助手**：连接内部数据库、API、文档系统
2. **开发工具集成**：让 AI 可以操作 Git、Jira、CI/CD
3. **数据分析平台**：AI 直接查询数据库，生成报告
4. **自动化工作流**：AI 调用各种 API 完成复杂任务

## 注意事项

⚠️ **安全第一**：MCP Server 可以执行操作，务必做好权限控制

⚠️ **性能考虑**：MCP Server 是独立进程，频繁调用会有延迟

⚠️ **版本兼容**：MCP 协议还在快速迭代，注意版本兼容性

## 总结

MCP 解决了 AI 工具集成的核心痛点——**标准化**。

以前：
- 每个 AI 模型一套工具调用方式
- 每个外部服务一套适配代码
- 维护成本高，复用性差

现在：
- 一个 MCP Server，所有 AI 都能用
- 社区已有大量现成 Server，开箱即用
- 专注业务逻辑，不用重复造轮子

如果你正在开发 AI 应用，MCP 值得深入了解。它可能成为 AI 工具集成的事实标准。

---

**官方文档**：https://modelcontextprotocol.io/

**GitHub**：https://github.com/modelcontextprotocol

**Server 列表**：https://github.com/modelcontextprotocol/servers
