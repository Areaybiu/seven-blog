---
title: 多 Agent 协同：从"单打独斗"到"团队作战"，四大框架怎么选？
date: 2026-06-17 18:00:00
tags:
  - AI
  - Agent
  - CrewAI
  - AutoGen
  - LangGraph
  - MetaGPT
categories:
  - 技术
cover: https://images.unsplash.com/photo-1552664730-d307ca884978?w=800
---

你有没有这种体验：让 AI 帮你写一个完整的项目，它一开始干得挺好，写着写着就开始"幻觉"——忘了前面的需求，改了后面的代码，最后给你一个四不像的东西。

问题出在哪？**一个 Agent 干所有活，就像一个人同时当产品经理、设计师、程序员、测试员——不崩才怪。**

<!-- more -->

## 单 Agent 的天花板

先看一个真实场景：让 AI 做一个完整的网站。

```
单 Agent 要做的事：
1. 需求分析
2. 架构设计
3. 前端开发
4. 后端开发
5. 数据库设计
6. 测试
7. 部署

问题：
- 上下文窗口就那么大，干到第 3 步就忘了第 1 步说了啥
- 没人审查，写错了自己发现不了
- 一个人干所有活，效率低到离谱
- 一旦出错，从头再来
```

这不是 AI 笨，是**架构问题**——你不能指望一个 Agent 像瑞士军刀一样什么都能干好。

## 多 Agent 的核心思想

**一个 Agent 只干一件事，多个 Agent 组成团队。**

就像真实的软件公司：

| 角色 | 职责 | 对应 Agent |
|------|------|-----------|
| 产品经理 | 需求分析、写 PRD | Research Agent |
| 架构师 | 技术选型、设计文档 | Architect Agent |
| 前端工程师 | 写页面、写组件 | Frontend Agent |
| 后端工程师 | 写接口、写逻辑 | Backend Agent |
| 测试工程师 | 写测试用例、找 Bug | QA Agent |
| DevOps | 部署、运维 | DevOps Agent |

每个 Agent 专注自己的领域，通过协作完成整个项目。

## 四大框架对比

目前主流的多 Agent 框架有四个，各有特点：

| 框架 | 核心思想 | 开发者 | GitHub Stars | 适合场景 |
|------|----------|--------|--------------|----------|
| **CrewAI** | 角色制团队 | CrewAI Inc | 30k+ | 生产环境、企业级 |
| **AutoGen** | 对话式协作 | 微软 | 40k+ | 研究探索、灵活对话 |
| **LangGraph** | 状态机 | LangChain | 10k+ | 复杂流程、精确控制 |
| **MetaGPT** | 模拟软件公司 | 深度求索 | 50k+ | 自动开发软件 |

下面逐个详解。

---

## 框架一：CrewAI —— 角色制团队

### 核心思想

CrewAI 把 Agent 当成**公司员工**——每个 Agent 有明确的角色（Role）、目标（Goal）和背景故事（Backstory）。

### 工作流程

```
┌─────────────────────────────────────────────────────────┐
│                    CrewAI 团队                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌─────────────┐         ┌─────────────┐               │
│  │   研究员     │ ──────► │   写手      │               │
│  │             │  交接   │             │               │
│  │ 角色：研究   │         │ 角色：写作   │               │
│  │ 目标：找信息 │         │ 目标：写文章 │               │
│  └─────────────┘         └─────────────┘               │
│         │                       │                       │
│         ▼                       ▼                       │
│  ┌─────────────┐         ┌─────────────┐               │
│  │  执行任务    │         │  执行任务    │               │
│  │  搜索信息    │         │  撰写文章    │               │
│  └─────────────┘         └─────────────┘               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 代码示例

```python
from crewai import Agent, Task, Crew, Process

# 1. 定义角色
researcher = Agent(
    role="高级研究分析师",
    goal="找到关于{topic}的最新、最准确信息",
    backstory="""你是一位资深的研究分析师，擅长从海量信息中
    提取关键洞察。你总是能找到最权威的数据源。""",
    tools=[search_tool, web_scraper],
    verbose=True
)

writer = Agent(
    role="技术博客作者",
    goal="写出通俗易懂、引人入胜的技术文章",
    backstory="""你是一位经验丰富的技术博主，擅长把复杂的
    技术概念用大白话讲清楚。你的文章总是既专业又好读。""",
    verbose=True
)

reviewer = Agent(
    role="内容审核员",
    goal="确保文章准确、无误、高质量",
    backstory="""你是一位严谨的审核员，对细节要求极高。
    你会检查每一个数据、每一个代码示例。""",
    verbose=True
)

# 2. 定义任务
research_task = Task(
    description="深入研究{topic}的最新进展、技术原理、应用场景",
    expected_output="一份详细的研究报告，包含关键数据和洞察",
    agent=researcher
)

write_task = Task(
    description="根据研究报告，写一篇面向开发者的技术博客",
    expected_output="一篇 2000 字以上的技术博客，包含代码示例",
    agent=writer,
    context=[research_task]  # 依赖研究任务的输出
)

review_task = Task(
    description="审核文章的准确性、可读性、代码示例是否正确",
    expected_output="审核报告，列出需要修改的地方",
    agent=reviewer,
    context=[write_task]  # 依赖写作任务的输出
)

# 3. 组建团队
crew = Crew(
    agents=[researcher, writer, reviewer],
    tasks=[research_task, write_task, review_task],
    process=Process.sequential,  # 顺序执行：研究 → 写作 → 审核
    verbose=True
)

# 4. 开始工作
result = crew.kickoff(inputs={"topic": "AI Agent 协同"})
print(result)
```

### 核心特点

| 特点 | 说明 |
|------|------|
| **角色定义** | 每个 Agent 有明确的角色、目标、背景 |
| **任务依赖** | 任务之间可以传递上下文 |
| **工具绑定** | 每个 Agent 可以绑定不同的工具 |
| **顺序/并行** | 支持顺序执行和并行执行 |
| **人类介入** | 可以在关键节点加入人工审核 |

### 适合场景

- ✅ 生产环境：稳定可靠，适合企业级应用
- ✅ 内容生产：研究 → 写作 → 审核的流程
- ✅ 数据分析：采集 → 清洗 → 分析 → 报告
- ✅ 客服系统：分流 → 处理 → 质检

---

## 框架二：AutoGen —— 对话式协作

### 核心思想

AutoGen 是微软开源的框架，核心理念是**让 Agent 像人一样对话**——不是流水线式的任务传递，而是自由讨论、协商、达成共识。

### 工作流程

```
┌─────────────────────────────────────────────────────────┐
│                    AutoGen 对话                         │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌─────────────┐         ┌─────────────┐               │
│  │  用户代理    │ ◄─────► │   助手      │               │
│  │             │  对话   │             │               │
│  │ 代表用户    │         │ 提供建议    │               │
│  │ 执行代码    │         │ 生成代码    │               │
│  └─────────────┘         └─────────────┘               │
│         │                       │                       │
│         │    ┌─────────────┐    │                       │
│         └───►│   专家      │◄───┘                       │
│              │             │                            │
│              │ 提供专业意见 │                            │
│              └─────────────┘                            │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 代码示例

```python
from autogen import AssistantAgent, UserProxyAgent, GroupChat, GroupChatManager

# 1. 创建 Agent
planner = AssistantAgent(
    name="产品经理",
    system_message="""你是产品经理，负责需求分析和项目规划。
    你会提出需求、讨论优先级、确认方案。""",
    llm_config={"model": "gpt-4"}
)

developer = AssistantAgent(
    name="开发者",
    system_message="""你是全栈开发者，负责技术实现。
    你会评估技术可行性、写代码、解决技术问题。""",
    llm_config={"model": "gpt-4"}
)

reviewer = AssistantAgent(
    name="审查员",
    system_message="""你是代码审查员，负责质量把关。
    你会检查代码质量、安全性、最佳实践。""",
    llm_config={"model": "gpt-4"}
)

user_proxy = UserProxyAgent(
    name="用户",
    human_input_mode="TERMINATE",  # 收到 TERMINATE 时停止
    code_execution_config={"work_dir": "coding"}
)

# 2. 创建群聊
groupchat = GroupChat(
    agents=[user_proxy, planner, developer, reviewer],
    messages=[],
    max_round=20
)

manager = GroupChatManager(groupchat=groupchat)

# 3. 开始对话
user_proxy.initiate_chat(
    manager,
    message="我想做一个待办事项 App，帮我分析需求、设计方案、写代码"
)
```

### 核心特点

| 特点 | 说明 |
|------|------|
| **自由对话** | Agent 之间可以自由讨论，不受流程限制 |
| **人类介入** | 支持 Human-in-the-Loop，关键时刻可以人工干预 |
| **群聊模式** | 多个 Agent 同时参与讨论 |
| **代码执行** | 可以直接执行 Agent 生成的代码 |
| **灵活终止** | 可以设置终止条件，自动结束对话 |

### 适合场景

- ✅ 研究探索：需要多角度讨论的问题
- ✅ 需求讨论：产品、设计、开发三方协商
- ✅ 代码审查：多人审查、讨论修改方案
- ✅ 教学场景：老师、学生、助教互动

---

## 框架三：LangGraph —— 状态机

### 核心思想

LangGraph 把 Agent 的工作流当成**状态机**——用图（Graph）定义节点（Node）和边（Edge），精确控制每一步的执行流程。

### 工作流程

```
┌─────────────────────────────────────────────────────────┐
│                   LangGraph 状态机                      │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌─────────┐     ┌─────────┐     ┌─────────┐          │
│  │  开始   │ ──► │  研究   │ ──► │  写作   │          │
│  └─────────┘     └─────────┘     └─────────┘          │
│                                       │                 │
│                                       ▼                 │
│                                 ┌─────────┐            │
│                                 │  审查   │            │
│                                 └─────────┘            │
│                                  ╱       ╲             │
│                                 ╱         ╲            │
│                                ▼           ▼           │
│                          ┌─────────┐ ┌─────────┐       │
│                          │ 通过    │ │ 重写    │       │
│                          └─────────┘ └─────────┘       │
│                               │         │              │
│                               │         └──────┐       │
│                               ▼                │       │
│                          ┌─────────┐           │       │
│                          │  结束   │ ◄─────────┘       │
│                          └─────────┘                   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 代码示例

```python
from langgraph.graph import Graph, END
from typing import TypedDict, Annotated
import operator

# 1. 定义状态
class AgentState(TypedDict):
    topic: str
    research: str
    article: str
    quality: float
    feedback: str

# 2. 定义节点
def researcher(state: AgentState) -> AgentState:
    """研究节点：搜索信息、整理资料"""
    topic = state["topic"]
    # 调用搜索工具
    research_result = search_tool.run(f"搜索 {topic} 的最新信息")
    return {"research": research_result}

def writer(state: AgentState) -> AgentState:
    """写作节点：根据研究结果写文章"""
    research = state["research"]
    # 调用 LLM 写文章
    article = llm.invoke(f"根据以下研究写一篇博客：\n{research}")
    return {"article": article}

def reviewer(state: AgentState) -> AgentState:
    """审查节点：检查文章质量"""
    article = state["article"]
    # 调用 LLM 审查
    review = llm.invoke(f"审查这篇文章的质量，给出 0-1 的分数和反馈：\n{article}")
    quality = float(review["score"])
    feedback = review["feedback"]
    return {"quality": quality, "feedback": feedback}

def should_continue(state: AgentState) -> str:
    """条件判断：质量达标就结束，否则重写"""
    if state["quality"] >= 0.8:
        return "end"
    else:
        return "rewrite"

# 3. 构建图
workflow = Graph()

workflow.add_node("researcher", researcher)
workflow.add_node("writer", writer)
workflow.add_node("reviewer", reviewer)

workflow.add_edge("researcher", "writer")
workflow.add_edge("writer", "reviewer")

workflow.add_conditional_edges(
    "reviewer",
    should_continue,
    {
        "end": END,
        "rewrite": "writer"  # 质量不达标，回到写作节点
    }
)

workflow.set_entry_point("researcher")

# 4. 编译并执行
app = workflow.compile()
result = app.invoke({"topic": "AI Agent 协同"})
```

### 核心特点

| 特点 | 说明 |
|------|------|
| **状态管理** | 每个节点可以读写共享状态 |
| **条件分支** | 支持 if-else 逻辑，动态决定下一步 |
| **循环支持** | 可以实现"审查不通过就重写"的循环 |
| **持久化** | 状态可以保存到数据库，支持断点续传 |
| **可视化** | 可以生成流程图，直观看到执行路径 |

### 适合场景

- ✅ 复杂流程：需要条件分支、循环的工作流
- ✅ 质量控制：审查 → 修改 → 再审查的循环
- ✅ 有状态任务：需要保存中间结果的任务
- ✅ 生产部署：稳定性要求高的场景

---

## 框架四：MetaGPT —— 模拟软件公司

### 核心思想

MetaGPT 的核心理念是 **Code = SOP(Team)**——模拟真实软件公司的组织架构和标准流程，让 AI Agent 像真正的开发团队一样协作。

### 工作流程

```
┌─────────────────────────────────────────────────────────┐
│                   MetaGPT 软件公司                      │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌─────────────┐         ┌─────────────┐               │
│  │  产品经理    │ ──────► │   架构师    │               │
│  │             │  PRD    │             │               │
│  │ 输出：需求文档│         │ 输出：设计文档│               │
│  └─────────────┘         └─────────────┘               │
│                                │                        │
│                                ▼                        │
│  ┌─────────────┐         ┌─────────────┐               │
│  │   测试员    │ ◄────── │   程序员    │               │
│  │             │  代码   │             │               │
│  │ 输出：测试报告│         │ 输出：源代码 │               │
│  └─────────────┘         └─────────────┘               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 代码示例

```python
from metagpt.software_company import SoftwareCompany
from metagpt.roles import ProductManager, Architect, ProjectManager, Engineer

# 1. 创建公司
company = SoftwareCompany()

# 2. 招聘员工
company.hire([
    ProductManager(),    # 产品经理
    Architect(),         # 架构师
    ProjectManager(),    # 项目经理
    Engineer(),          # 工程师
])

# 3. 启动项目
company.start_project("做一个待办事项 App，支持添加、删除、标记完成")

# 4. 运行
await company.run()
```

### 核心特点

| 特点 | 说明 |
|------|------|
| **角色完整** | 产品经理、架构师、程序员、测试员一应俱全 |
| **SOP 驱动** | 按照标准流程执行，不是随意对话 |
| **自动文档** | 自动生成 PRD、设计文档、API 文档 |
| **代码生成** | 直接生成可运行的项目代码 |
| **全局视野** | 每个角色都能看到其他角色的输出 |

### 适合场景

- ✅ 软件开发：从需求到代码的完整流程
- ✅ 原型验证：快速验证产品想法
- ✅ 学习参考：了解软件开发的完整流程
- ✅ 代码生成：需要生成完整项目代码

---

## 四大框架深度对比

### 架构对比

| 维度 | CrewAI | AutoGen | LangGraph | MetaGPT |
|------|--------|---------|-----------|---------|
| **协作模式** | 流水线 | 自由对话 | 状态机 | SOP 流程 |
| **控制粒度** | 中 | 低 | 高 | 中 |
| **灵活性** | 中 | 高 | 高 | 低 |
| **学习曲线** | 低 | 中 | 高 | 中 |
| **生产就绪** | ✅ | ⚠️ | ✅ | ⚠️ |

### 代码复杂度对比

| 框架 | 最小可用代码行数 | 说明 |
|------|-----------------|------|
| **CrewAI** | ~30 行 | 最简单，定义角色和任务即可 |
| **AutoGen** | ~20 行 | 简单场景很简单，复杂场景很复杂 |
| **LangGraph** | ~50 行 | 需要定义状态、节点、边 |
| **MetaGPT** | ~10 行 | 最简单，但定制性有限 |

### 适用场景对比

| 场景 | 推荐框架 | 原因 |
|------|----------|------|
| 内容生产（博客、报告） | **CrewAI** | 角色清晰，流程简单 |
| 需求讨论、头脑风暴 | **AutoGen** | 自由对话，灵活协商 |
| 复杂业务流程 | **LangGraph** | 精确控制，支持分支和循环 |
| 软件开发 | **MetaGPT** | 模拟真实开发流程 |
| 研究探索 | **AutoGen** | 支持多角度讨论 |
| 生产环境部署 | **CrewAI / LangGraph** | 稳定可靠 |

---

## 如何选择？

### 选 CrewAI 如果你：

- 想快速上手，不想折腾
- 需要明确的角色分工
- 做内容生产、数据分析等标准化流程
- 需要部署到生产环境

### 选 AutoGen 如果你：

- 需要 Agent 之间自由讨论
- 需要人类介入（Human-in-the-Loop）
- 做研究探索、需求讨论
- 不介意对话流程不可控

### 选 LangGraph 如果你：

- 需要精确控制每一步
- 有复杂的条件分支和循环
- 需要保存中间状态
- 对稳定性要求很高

### 选 MetaGPT 如果你：

- 想让 AI 自动开发软件
- 需要完整的文档输出
- 想模拟真实开发团队
- 项目需求比较明确

---

## 实战建议

### 1. 从小开始

别一上来就搞复杂的多 Agent 系统。先用单 Agent 解决简单问题，遇到瓶颈再考虑多 Agent。

### 2. 明确职责

每个 Agent 只做一件事。如果你发现一个 Agent 在做两件不同的事，拆成两个 Agent。

### 3. 设计好通信

Agent 之间怎么传递信息？用共享状态？用消息队列？用数据库？这是架构设计的关键。

### 4. 加入审查

多 Agent 系统最容易出的问题是"错误传播"——一个 Agent 的错误被下一个 Agent 放大。加入审查节点，及时发现问题。

### 5. 监控和日志

多 Agent 系统比单 Agent 复杂得多，必须有完善的监控和日志，否则出了问题都不知道在哪。

---

## 总结

| 框架 | 一句话总结 | 核心优势 | 核心劣势 |
|------|-----------|----------|----------|
| **CrewAI** | 像公司团队一样协作 | 简单易用、生产就绪 | 灵活性一般 |
| **AutoGen** | 像开会一样讨论 | 自由灵活、支持人工 | 流程不可控 |
| **LangGraph** | 像状态机一样精确 | 精确控制、支持循环 | 学习曲线陡 |
| **MetaGPT** | 像软件公司一样开发 | 完整流程、自动文档 | 定制性有限 |

没有最好的框架，只有最适合的框架。根据你的场景选择合适的工具，才是正确的做法。

---

**参考资源**：

- CrewAI 官方文档：https://docs.crewai.com/
- AutoGen GitHub：https://github.com/microsoft/autogen
- LangGraph 官方文档：https://langchain-ai.github.io/langgraph/
- MetaGPT GitHub：https://github.com/geekan/MetaGPT
