---
title: MCP 协议到底是什么？用外卖平台的故事讲清楚
date: 2026-06-17 17:30:00
tags:
  - AI
  - MCP
  - 开发工具
categories:
  - 技术
cover: https://images.unsplash.com/photo-1677442136019-21780ecad995?w=800
---

你有没有点过外卖？美团、饿了么这些平台，连接了你和无数餐厅。你不需要知道每家餐厅的厨房在哪、厨师是谁、食材从哪来——你只需要在 App 上点餐，外卖小哥就会把饭送到你手上。

MCP 协议做的事情，就是给 AI 装上一个"外卖平台"。

<!-- more -->

## 先说一个真实的痛点

假设你是一个开发者，想让 AI 帮你做这些事：

- 查一下公司数据库里有多少用户
- 读取本地的一个配置文件
- 调用天气 API 看明天天气
- 操作 Git 提交代码

**没有 MCP 之前**，你需要：

1. 为 Claude 写一套数据库查询代码
2. 为 GPT 写一套文件读取代码
3. 为 Gemini 写一套 API 调用代码
4. 每个 AI 模型都要单独适配，格式还不一样

这就像——你想点外卖，但美团只能点麦当劳，饿了么只能点肯德基，想吃火锅得再装一个 App。每个平台都有自己的规则，餐厅要为每个平台单独对接。

**有了 MCP 之后**：

1. 写一个"数据库 MCP Server"
2. 所有 AI 模型都能用它查数据库
3. 不用重复开发，不用适配不同格式

这就像——有了统一的外卖平台标准，餐厅只需要对接一次，所有平台都能点它的餐。

## 用一张图说清楚

```
┌─────────────────────────────────────────────────────────┐
│                    你（用户）                            │
│                      ↓ 提问                              │
│  ┌───────────────────────────────────────────────────┐  │
│  │              AI 应用（Claude/GPT/Gemini）          │  │
│  │                        ↓                           │  │
│  │              MCP Client（内置的"外卖小哥"）         │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                         │
                         │ "我要查数据库"（标准协议）
                         ↓
┌─────────────────────────────────────────────────────────┐
│              MCP Server（"餐厅"）                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ 数据库查询   │  │ 文件读取     │  │ API 调用    │     │
│  │   Server    │  │   Server    │  │   Server    │     │
│  └─────────────┘  └─────────────┘  └─────────────┘     │
└─────────────────────────────────────────────────────────┘
                         │
                         ↓
              ┌─────────────────────┐
              │  真正的数据/服务     │
              │  数据库、文件、API   │
              └─────────────────────┘
```

**翻译成人话**：

- **你**：问 AI "帮我查一下数据库有多少用户"
- **AI 应用**：收到问题，调用 MCP Client
- **MCP Client**：像外卖小哥一样，去找对应的 MCP Server
- **MCP Server**：像餐厅一样，执行真正的操作（查数据库）
- **结果**：返回给你"有 1024 个用户"

## 三个核心概念

MCP 里有三个角色，用外卖平台来理解：

### 1. Host（宿主）= 外卖平台 App

就是你用的 AI 应用，比如：
- Claude Desktop
- Cursor（代码编辑器）
- 你用代码写的 AI 程序

它负责运行 AI 模型，管理 MCP Client。

### 2. MCP Client（客户端）= 外卖小哥

负责在 AI 应用和 MCP Server 之间传递消息。每个 Client 连接一个 Server。

就像外卖小哥：接单 → 取餐 → 送餐。

### 3. MCP Server（服务器）= 餐厅

提供具体的功能。比如：
- 数据库 Server：可以查询数据库
- 文件系统 Server：可以读写文件
- GitHub Server：可以操作 Git 仓库

每个 Server 专注做一件事，做好做精。

## MCP Server 能提供什么？

一个 MCP Server 可以提供三种能力：

### 1. Tools（工具）= 菜品

让 AI 可以**执行操作**。就像餐厅提供的菜品，你可以点（调用）。

```json
{
  "name": "查询用户数量",
  "description": "查询数据库中的用户总数",
  "需要的参数": {
    "表名": "users"
  }
}
```

AI 可以说："帮我调用'查询用户数量'这个工具"。

### 2. Resources（资源）= 食材清单

让 AI 可以**读取数据**。就像餐厅的食材清单，你可以查看（读取）。

```json
{
  "uri": "数据库://users 表",
  "name": "用户表",
  "description": "包含所有用户信息"
}
```

AI 可以说："让我看看用户表里有什么字段"。

### 3. Prompts（提示词）= 推荐菜单

预定义的**提示词模板**。就像餐厅的推荐套餐，直接用就行。

```json
{
  "name": "数据分析模板",
  "description": "帮你分析数据并生成报告",
  "参数": ["数据表名", "分析维度"]
}
```

AI 可以说："用数据分析模板，帮我分析用户表"。

## 实际例子：5 分钟搭建一个 MCP Server

假设你想让 AI 可以查询天气，只需要几步：

### 第一步：创建项目

```bash
mkdir weather-mcp-server
cd weather-mcp-server
npm init -y
npm install @modelcontextprotocol/sdk
```

### 第二步：写代码

```javascript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

// 1. 创建服务器
const server = new Server(
  { name: "weather-server", version: "1.0.0" },
  { capabilities: { tools: {} } }
);

// 2. 注册工具：告诉 AI "我能做什么"
server.setRequestHandler("tools/list", async () => ({
  tools: [
    {
      name: "get_weather",
      description: "获取指定城市的天气",
      inputSchema: {
        type: "object",
        properties: {
          city: { type: "string", description: "城市名称，比如"北京"" }
        },
        required: ["city"]
      }
    }
  ]
}));

// 3. 处理调用：AI 调用时，真正执行的代码
server.setRequestHandler("tools/call", async (request) => {
  const { name, arguments: args } = request.params;
  
  if (name === "get_weather") {
    // 这里可以调用真实的天气 API
    const weather = {
      city: args.city,
      temperature: "25°C",
      condition: "晴天"
    };
    return {
      content: [{ type: "text", text: JSON.stringify(weather, null, 2) }]
    };
  }
});

// 4. 启动服务器
const transport = new StdioServerTransport();
await server.connect(transport);
```

### 第三步：配置 AI 应用

在 Claude Desktop 的配置文件中添加：

```json
{
  "mcpServers": {
    "weather": {
      "command": "node",
      "args": ["/path/to/weather-server/index.js"]
    }
  }
}
```

### 第四步：使用

重启 Claude Desktop，然后你就可以说：

> "帮我查一下北京今天天气怎么样"

Claude 会自动调用你的天气 MCP Server，返回结果。

## MCP vs 其他方案

| 场景 | 没有 MCP | 有 MCP |
|------|----------|--------|
| 给 Claude 加数据库功能 | 写一套 Claude 适配代码 | 用现成的数据库 MCP Server |
| 给 GPT 加同样的功能 | 再写一套 GPT 适配代码 | 同一个 Server，直接能用 |
| 换一个新的 AI 模型 | 再写一套适配代码 | 同一个 Server，直接能用 |
| 维护成本 | N 套代码要维护 | 1 套代码搞定 |

## 为什么 MCP 很重要？

**1. 开发者不用重复造轮子**

社区已经有大量现成的 MCP Server：
- 文件系统、数据库、GitHub、Slack、Google Drive...
- 你只需要配置，不用写代码

**2. AI 应用更容易扩展功能**

想让 AI 能操作新服务？装个 MCP Server 就行。

**3. 安全可控**

MCP Server 运行在本地，数据不经过第三方。你可以控制 AI 能访问什么、不能访问什么。

## 哪些 AI 应用支持 MCP？

目前支持 MCP 的应用：

- **Claude Desktop**：Anthropic 官方支持
- **Cursor**：代码编辑器，内置 MCP 支持
- **Windsurf**：代码编辑器
- **Cline**：VS Code 插件
- **自定义应用**：用 MCP SDK 自己开发

## 真实案例：OpenAI 用 MCP 构建语音 Agent

说了这么多理论，来看一个真实的案例——OpenAI 官方发布的 **MCP-Powered Agentic Voice Framework**。

### 场景：保险公司的电话客服

用户打电话问："我的保单覆盖哪些疾病？"

**传统方式**：需要人工客服查系统、翻保单、回答问题。效率低，成本高。

**用 MCP + 语音 Agent**：

```
用户打电话："我的保单覆盖哪些疾病？"
        ↓
语音识别 → 文字
        ↓
AI Agent 收到问题
        ↓
通过 MCP 调用"知识库检索" Server
        ↓
查询保单数据库，找到相关信息
        ↓
AI 生成回答
        ↓
语音合成 → 播放给用户
```

### 架构设计

OpenAI 用三层架构实现这个系统：

```
┌─────────────────────────────────────────────────────────┐
│                    用户（语音输入）                        │
└─────────────────────────┬───────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────┐
│              OpenAI Agents SDK（编排层）                  │
│         负责协调多个 Agent 协同工作                        │
└─────────────────────────┬───────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────┐
│                MCP 协议（中间件）                         │
│         标准化接口，连接 Agent 和外部工具                  │
└───────┬─────────────┬─────────────┬─────────────────────┘
        ▼             ▼             ▼
   ┌─────────┐  ┌─────────┐  ┌─────────┐
   │ 文本检索  │  │ Web搜索  │  │ SQLite  │
   │ MCP服务  │  │ MCP服务  │  │ MCP服务  │
   └─────────┘  └─────────┘  └─────────┘
```

### 关键技术点

| 组件 | 作用 |
|------|------|
| **OpenAI Agents SDK** | 编排多个 Agent，处理语音输入 |
| **MCP 协议** | 标准化 Agent 和工具的通信 |
| **MCP Servers** | 提供具体能力（搜索、数据库、检索） |

### 为什么用 MCP 而不是直接调用？

1. **Agent 和工具解耦**：Agent 不直接调用工具，通过 MCP 标准协议。换工具不用改 Agent 代码
2. **工具可插拔**：想加新的数据源？装个新的 MCP Server 就行
3. **多 Agent 协同**：不同 Agent 可以共享同一套 MCP Server

### 代码示例

```python
from openai import OpenAI
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

# 1. 创建 MCP 客户端，连接知识库 Server
server_params = StdioServerParameters(
    command="python",
    args=["knowledge_base_server.py"]
)

async with stdio_client(server_params) as (read, write):
    async with ClientSession(read, write) as session:
        # 2. 初始化连接
        await session.initialize()
        
        # 3. 调用工具：检索保单信息
        result = await session.call_tool(
            name="search_policy",
            arguments={
                "query": "疾病覆盖范围",
                "policy_id": "POL-12345"
            }
        )
        
        # 4. 用 AI 生成自然语言回答
        client = OpenAI()
        response = client.chat.completions.create(
            model="gpt-4",
            messages=[
                {"role": "system", "content": "你是保险客服，用简洁明了的语言回答用户问题"},
                {"role": "user", "content": f"用户问：我的保单覆盖哪些疾病？\n\n检索结果：{result}"}
            ]
        )
        
        # 5. 语音合成，播放给用户
        # ... TTS 代码 ...
```

### 适用行业

这个模式不只适用于保险，任何需要"电话客服 + 知识库查询"的场景都能用：

| 行业 | 应用场景 |
|------|----------|
| 保险 | 保单查询、理赔流程 |
| 医疗 | 预约挂号、症状咨询 |
| 政务 | 政策咨询、办事指南 |
| 电商 | 订单查询、售后服务 |
| 金融 | 账户查询、理财建议 |

## 总结

MCP 解决了一个简单但重要的问题：**让 AI 能用统一的方式调用各种外部工具**。

以前：每个 AI 模型一套规则，每个工具一套适配，混乱不堪。
现在：一个标准，所有 AI 都能用，所有工具都能接。

就像 USB-C 统一了充电接口，MCP 正在统一 AI 的工具调用接口。

如果你想让 AI 能做更多事情，MCP 是目前最好的方案。

---

**官方文档**：https://modelcontextprotocol.io/

**GitHub**：https://github.com/modelcontextprotocol

**MCP Server 列表**：https://github.com/modelcontextprotocol/servers

**OpenAI 语音 Agent 案例**：https://developers.openai.com/cookbook/examples/partners/mcp_powered_voice_agents/mcp_powered_agents_cookbook
