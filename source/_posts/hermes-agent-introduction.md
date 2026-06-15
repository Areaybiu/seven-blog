---
title: Hermes Agent：会"记住你"的 AI 智能体，终于来了
date: 2026-06-14 16:30:00
tags:
  - AI
  - Agent
  - 开源
  - 工具
categories:
  - 技术
cover: https://raw.githubusercontent.com/NousResearch/hermes-agent/main/assets/banner.png
description: 大多数 AI Agent 用完就忘，Hermes Agent 却能越用越懂你。本文带你了解这个来自 Nous Research 的开源项目，看看它凭什么让圈子里讨论不停。
---

## 为什么大多数 AI Agent 用起来让人抓狂？

![Hermes Agent](https://raw.githubusercontent.com/NousResearch/hermes-agent/main/assets/banner.png)

你有没有过这种体验：

- 第一次和 AI 对话，感觉它很聪明
- 第三次对话，开始要重新解释背景
- 第十次对话，和第一次没有任何区别

**它们没有学习。它们只有上下文窗口。**

每次新会话开始，之前说的一切都消失了。你反复解释同一个项目背景、反复纠正同一个错误、反复描述同一个偏好——AI 每次都像失忆了一样从头来过。

这个问题，Hermes Agent 给出了一个目前最系统的回答。

---

## Hermes Agent 是什么？

Hermes Agent 是由 **Nous Research** 开发的开源 AI 智能体框架，2026 年 2 月发布，两个月内 GitHub Stars 突破 4.8 万。

> Nous Research 是一家专注于开源大模型研究的 AI 实验室，他们最知名的产品是 Hermes 系列模型——开源社区里工具调用能力最强的模型之一。简单说：这是一家**既训模型又做基础设施**的团队。

Hermes Agent 的官方定位是：

> **"The agent that grows with you"**（随你成长的 Agent）

这句话不是 slogan，是它的核心架构逻辑。

**一句话总结：** 开源、本地部署、越用越聪明的 AI Agent。能运行在你的电脑、VPS 或 Docker 中，铭记教训，沉淀技能，并通过微信、飞书、QQ 等 APP 与你保持联系。

---

## 核心亮点：三层记忆架构

Hermes Agent 最大的不同，是它把记忆分成了**三个层次**，并且让这三层之间可以自动流动。

### 第一层：会话记忆（Session Memory）

每次对话的内容，通过 FTS5（SQLite 全文索引）存入本地数据库，并由 LLM 自动做摘要压缩。下次开始新对话时，Agent 会主动检索历史，把相关内容召回。

这不是简单的"存日志"——它有跨会话的语义检索能力，找的是**相关性**，不是按时间线硬翻。

### 第二层：知识记忆（Persistent Memory）

Agent 会在对话过程中，主动判断哪些信息值得长期保存——你的偏好、你的项目背景、你反复提到的上下文。它会把这些提炼成结构化的记忆条目，持久存储。

更重要的是：它会**主动记住**。通过 Honcho 组件做"辩证式用户建模"——不只是存你说了什么，而是尝试建立你这个人的心智模型。

### 第三层：技能记忆（Skills System）

这一层是最有创意的设计。

当 Agent 帮你完成了一个复杂任务之后，它会自动把这个"解决方案"提炼成一个**可复用的 Skill 文件**。下次遇到类似任务，它不需要从头想，直接调用这个 Skill，并且在调用过程中持续改进它。

Skills 是标准化的文件格式，兼容 agentskills.io 开放社区——你可以把自己的 Skill 分享出去，也可以从社区下载别人写好的 Skill。

**三层加在一起，构成了官方说的"闭环学习回路"。用大白话说：它用得越多，越懂你，也越能干。**

---

## 不只是终端：15+ 平台统一接入

大多数 Agent 是"进程型"的——你开电脑，打开终端，才有 Agent。你关电脑，Agent 停止工作。

Hermes Agent 的设计思路不同：**让 Agent 活在服务器上，你通过消息平台跟它交互。**

支持的消息平台：

| 平台 | 状态 |
|------|------|
| CLI 终端 | ✅ 默认入口 |
| **微信** | ✅ 原生支持 |
| **飞书** | ✅ 原生支持 |
| **企业微信** | ✅ 原生支持 |
| **钉钉** | ✅ 原生支持 |
| **QQ** | ✅ 原生支持 |
| Telegram | ✅ 原生支持 |
| Discord | ✅ 原生支持 |
| Slack | ✅ 原生支持 |
| WhatsApp | ✅ 原生支持 |
| Email / SMS | ✅ 原生支持 |
| Signal / Matrix | ✅ 原生支持 |
| Home Assistant | ✅ 智能家居 |

这意味着一个非常有趣的使用模式：**你可以在手机微信上给它发任务，它在云服务器上跑，任务完成后推送结果给你。**

---

## 兼容国内外模型供应商

Hermes Agent 支持 **20+ 模型提供商**，包括国内用户熟悉的大模型：

| 国内模型 | 国外模型 |
|---------|---------|
| ✅ Qwen（通义千问） | ✅ Claude |
| ✅ GLM（智谱） | ✅ Gemini |
| ✅ Kimi（月之暗面） | ✅ GPT |
| ✅ MiniMax | ✅ DeepSeek |
| ✅ 小米 MiMo | ✅ OpenRouter |

还支持 **OpenAI 兼容接口**和**本地模型**，适合国内网络与工具环境。

---

## MCP、工具与自动化

Hermes Agent 不只是一个聊天机器人，它是一个**完整的工具系统**：

### 支持的工具

- **终端**：执行命令、运行脚本
- **文件**：读写文件、搜索代码
- **浏览器**：自动化操作网页
- **图片**：生成和分析图片
- **TTS**：文字转语音
- **MCP**：连接外部服务

### 定时任务（Cron）

通过内置的 Cron 调度功能，可以实现：

- 每天早上 8 点搜索 AI 行业新闻，整理成简报
- 每小时备份一次数据库
- 每周五下午发送周报
- 监控网站状态，异常时通知

整个设置过程是自然语言的——你直接告诉它你想要什么，它帮你生成配置并注册。

---

## 实际能用来做什么？

### 1. 软件开发

这是最直接的场景。Hermes Agent 可以：
- 读写文件、执行命令、搜索代码
- 自动创建 Git 分支、提交代码、发起 PR
- 运行测试、调试错误、重构代码
- 支持 Docker、SSH 远程开发环境

### 2. 跨平台信息助手

在微信/飞书上配置接入后，直接在聊天框里发指令，它会搜索、整理、总结，然后回复。

### 3. 长期项目管理

在项目目录下放一个 `AGENTS.md` 文件，每次 Agent 进入这个目录，文件内容会自动注入到对话上下文里。结合 Skills 系统，常见任务都被提炼成 Skill 文件，效率大幅提升。

### 4. 多 Agent 并行工作

可以同时启动多个 Agent 实例，分别处理不同任务，互不干扰。

---

## 和其他 Agent 的对比

| 特性 | Hermes Agent | Claude Code | Cursor |
|------|-------------|-------------|--------|
| 跨会话记忆 | ✅ 三层架构 | ❌ 基本没有 | ❌ 基本没有 |
| 技能自学习 | ✅ Skills 系统 | ❌ | ❌ |
| 多平台接入 | ✅ 15+ 平台（含微信） | ❌ 仅终端 | ❌ 仅 IDE |
| 模型自由切换 | ✅ 20+ 提供商 | ❌ 仅 Anthropic | ❌ 仅 OpenAI/Anthropic |
| 开源 | ✅ MIT 协议 | ❌ | ❌ |
| 定时任务 | ✅ 内置 Cron | ❌ | ❌ |
| MCP 支持 | ✅ | ✅ | ❌ |

---

## 快速开始

### 安装

**Linux / macOS / WSL2：**
```bash
curl -fsSL https://res1.hermesagent.org.cn/install.sh | bash
```

**Windows PowerShell：**
```powershell
irm https://res1.hermesagent.org.cn/install.ps1 | iex
```

### 配置模型

```bash
hermes setup
hermes model
```

通过命令行配置器连接 Kimi、GLM、MiniMax 或任意 OpenAI 接口兼容模型。

### 开始对话

```bash
hermes
```

启动完整 TUI，使用多行输入、命令补全、上下文压缩、工具输出流和会话历史。

### 接入消息网关

```bash
hermes gateway setup
hermes gateway
```

为 Hermes 接入微信、飞书、企业微信、钉钉、QQ、WhatsApp、Discord、Slack 等平台。

### 环境要求

- Python 3.11+
- 支持 Linux、macOS、Windows（原生 + WSL）
- 至少需要一个 LLM 提供商的 API Key

---

## 一些真实的使用感受

用了一段时间后，有几个点让我印象深刻：

**好的方面：**
- 记忆能力是真的强。上次讨论的项目细节，下次开新会话它还记得
- Skills 系统很实用，重复性工作越来越快
- 多平台接入很方便，手机上就能发任务
- 支持国内模型，不用翻墙也能用

**需要注意的：**
- 它能执行真实的系统命令，需要注意安全设置
- 复杂任务偶尔会"跑偏"，需要人工介入
- 学习曲线比普通 AI 对话工具稍高

---

## 总结

Hermes Agent 不是又一个"包了 LLM 的壳子"。它在记忆、学习、多平台这几个维度上，确实做出了和其他 Agent 不同的设计选择。

如果你是：
- **开发者**：想要一个能记住项目上下文的编程助手
- **效率爱好者**：想要一个跨平台的 AI 助手
- **技术探索者**：想了解 AI Agent 的前沿方向

都值得试试。

**相关链接：**
- [GitHub - NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)
- [官方文档](https://hermes-agent.nousresearch.com/docs/)
- [Hermes Agent 中文社区](https://hermesagent.org.cn/)

---

*本文写于 2026 年 6 月 14 日，基于实际使用体验整理。*
