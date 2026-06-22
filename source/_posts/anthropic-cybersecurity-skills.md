---
title: Anthropic Cybersecurity Skills：给 AI 装上 754 个安全技能，让它成为网络安全专家
date: 2026-06-17 21:00:00
tags:
  - AI
  - 网络安全
  - Agent
  - 开源
categories:
  - 技术
cover: https://images.unsplash.com/photo-1550751827-4bd374c3f58b?w=800
---

你有没有这种体验：让 AI 帮你分析一个安全事件，它给你一堆泛泛而谈的建议，什么"加强密码策略"、"定期更新补丁"——这些谁不知道？但具体怎么分析恶意软件流量？怎么追踪攻击者的 TTP？怎么映射到 MITRE ATT&CK 框架？AI 一脸懵逼。

**问题出在哪？AI 缺乏结构化的安全知识。**

<!-- more -->

## 一、AI 在安全领域的困境

### 1.1 AI 的"安全知识盲区"

当你让 AI 帮你做安全分析时：

```
你：帮我分析这个恶意软件样本
AI：建议你使用杀毒软件扫描...

你：这个攻击者的 TTP 是什么？
AI：TTP 是 Tactics, Techniques, and Procedures 的缩写...

你：帮我映射到 MITRE ATT&CK 框架
AI：MITRE ATT&CK 是一个知识库...
```

**AI 知道概念，但不知道怎么操作。**

### 1.2 真实数据

在企业安全团队中：

| 指标 | 数据 |
|------|------|
| 全球网络安全人才缺口 | 350 万+ |
| 平均安全事件响应时间 | 207 天 |
| 安全分析师培训周期 | 6-12 个月 |
| AI 辅助安全分析效率提升 | 60%+ |

**问题很明显：** 安全人才短缺，培训周期长，AI 可以帮忙但缺乏结构化知识。

---

## 二、解决方案：Anthropic Cybersecurity Skills

### 2.1 一句话

**754 个结构化网络安全技能，让 AI 智能体具备专家级安全分析能力。**

### 2.2 核心数据

| 指标 | 数据 |
|------|------|
| **技能数量** | 754 个 |
| **安全领域** | 26 个 |
| **框架映射** | 5 个 |
| **支持平台** | 20+ 个 |
| **GitHub Stars** | 18.1k ⭐ |
| **Forks** | 2.2k |
| **许可证** | Apache 2.0 |

### 2.3 项目地址

https://github.com/mukul975/Anthropic-Cybersecurity-Skills

---

## 三、五大安全框架映射

这个项目最大的特点是：**每个技能都映射到 5 个行业标准框架**。

### 3.1 MITRE ATT&CK

**是什么：** 全球最权威的攻击者行为知识库

**包含内容：**
- 14 个战术（Tactics）
- 200+ 个技术（Techniques）
- 400+ 个子技术（Sub-techniques）

**示例映射：**

```yaml
skill: analyzing-network-traffic-of-malware
mitre_attack:
  tactic: TA0011 - Command and Control
  technique: T1071 - Application Layer Protocol
  sub_technique: T1071.001 - Web Protocols
```

### 3.2 NIST CSF 2.0

**是什么：** 美国国家标准与技术研究院的网络安全框架

**包含内容：**
- 6 个核心功能：识别、保护、检测、响应、恢复、治理
- 22 个类别
- 106 个子类别

**示例映射：**

```yaml
skill: analyzing-network-traffic-of-malware
nist_csf:
  function: DE - Detect
  category: DE.CM - Security Continuous Monitoring
  subcategory: DE.CM-01 - Networks and network services are monitored
```

### 3.3 MITRE ATLAS

**是什么：** 针对 AI 系统的攻击和防御知识库

**包含内容：**
- AI 特定的攻击技术
- 对抗性机器学习
- AI 系统安全

**示例映射：**

```yaml
skill: analyzing-network-traffic-of-malware
mitre_atlas:
  tactic: ML0001 - ML Model Access
  technique: AML.T0024 - Exfiltration via ML Inference API
```

### 3.4 MITRE D3FEND

**是什么：** 网络安全防御技术知识库

**包含内容：**
- 防御技术分类
- 技术关系映射
- 防御策略

**示例映射：**

```yaml
skill: analyzing-network-traffic-of-malware
d3fend:
  technique: D3-NTA - Network Traffic Analysis
  artifact: D3-NTA - Network Traffic
```

### 3.5 NIST AI RMF

**是什么：** AI 风险管理框架

**包含内容：**
- AI 系统风险管理
- AI 安全治理
- AI 伦理和合规

**示例映射：**

```yaml
skill: analyzing-network-traffic-of-malware
nist_ai_rmf:
  function: GO - Govern
  category: GO.1 - Govern and Map
```

---

## 四、26 个安全领域

### 4.1 领域列表

| 领域 | 技能数量 | 说明 |
|------|----------|------|
| **威胁情报** | 50+ | 威胁分析、IOC 提取、威胁狩猎 |
| **事件响应** | 60+ | DFIR、证据收集、时间线分析 |
| **恶意软件分析** | 40+ | 静态分析、动态分析、逆向工程 |
| **渗透测试** | 50+ | 漏洞评估、利用、后渗透 |
| **安全运营** | 40+ | SIEM、日志分析、告警处理 |
| **网络安全** | 30+ | 流量分析、防火墙、IDS/IPS |
| **应用安全** | 40+ | 代码审计、SAST/DAST、漏洞修复 |
| **云安全** | 30+ | AWS/Azure/GCP 安全、容器安全 |
| **身份认证** | 20+ | IAM、MFA、特权访问管理 |
| **数据安全** | 20+ | 数据分类、加密、DLP |
| **合规审计** | 20+ | GDPR、HIPAA、PCI DSS |
| **AI 安全** | 20+ | 对抗性 ML、AI 模型安全 |
| **区块链安全** | 15+ | 智能合约审计、DeFi 安全 |
| **物联网安全** | 15+ | 设备安全、协议分析 |
| **移动安全** | 15+ | iOS/Android 安全、应用分析 |
| **社会工程** | 10+ | 钓鱼分析、意识培训 |
| **数字取证** | 30+ | 磁盘取证、内存取证、网络取证 |
| **漏洞管理** | 20+ | 漏洞扫描、补丁管理、漏洞评估 |
| **安全架构** | 15+ | 零信任、安全设计、威胁建模 |
| **红队操作** | 20+ | 攻击模拟、绕过技术 |
| **蓝队操作** | 20+ | 防御策略、检测规则 |
| **紫队操作** | 10+ | 攻防协作、效果评估 |
| **安全培训** | 10+ | 技能培训、知识传递 |
| **威胁建模** | 10+ | STRIDE、PASTA、攻击树 |
| **安全工具** | 20+ | 工具使用、自动化脚本 |
| **安全治理** | 10+ | 政策制定、风险管理 |

### 4.2 技能示例

**恶意软件分析技能：**

```yaml
name: analyzing-network-traffic-of-malware
description: 分析恶意软件的网络流量行为
domain: malware_analysis
difficulty: advanced
time_estimate: 2-4 hours

steps:
  1. 使用 Wireshark/Tcpdump 捕获网络流量
  2. 过滤可疑连接
  3. 分析 DNS 查询模式
  4. 识别 C2 通信协议
  5. 提取 IOC（IP、域名、URL）
  6. 映射到 MITRE ATT&CK

tools:
  - Wireshark
  - Tcpdump
  - NetworkMiner
  - Zeek

output:
  - 网络流量报告
  - IOC 列表
  - ATT&CK 映射
```

**事件响应技能：**

```yaml
name: incident-response-forensic-analysis
description: 安全事件的取证分析
domain: incident_response
difficulty: expert
time_estimate: 4-8 hours

steps:
  1. 识别事件范围和影响
  2. 收集易失性证据
  3. 磁盘镜像和分析
  4. 内存取证
  5. 时间线重建
  6. 根因分析
  7. 撰写事件报告

tools:
  - Autopsy
  - Volatility
  - FTK Imager
  - Plaso/log2timeline

output:
  - 取证报告
  - 时间线
  - IOC 列表
  - 修复建议
```

---

## 五、支持的 AI 平台

### 5.1 官方支持

| 平台 | 支持状态 | 说明 |
|------|----------|------|
| **Claude Code** | ✅ 完全支持 | Anthropic 官方 |
| **GitHub Copilot** | ✅ 完全支持 | VS Code 集成 |
| **Codex CLI** | ✅ 完全支持 | OpenAI 命令行 |
| **Cursor** | ✅ 完全支持 | AI 代码编辑器 |
| **Gemini CLI** | ✅ 完全支持 | Google AI |
| **Hermes Agent** | ✅ 完全支持 | Nous Research |
| **Aider** | ✅ 完全支持 | 终端 AI 编程 |
| **Windsurf** | ✅ 完全支持 | Codeium IDE |
| **Cline** | ✅ 完全支持 | VS Code 插件 |

### 5.2 兼容平台

支持任何遵循 agentskills.io 标准的 AI 平台。

---

## 六、安装和使用

### 6.1 安装方式

**方式一：Git Clone**

```bash
git clone https://github.com/mukul975/Anthropic-Cybersecurity-Skills.git
cd Anthropic-Cybersecurity-Skills
```

**方式二：Claude Code 插件市场**

```bash
# 在 Claude Code 中添加插件市场
/plugin marketplace add https://github.com/mukul975/Anthropic-Cybersecurity-Skills.git

# 安装技能
/plugin install anthropic-cybersecurity-skills
```

### 6.2 使用示例

**示例 1：恶意软件分析**

```
你：帮我分析这个恶意软件样本的网络行为

AI：好的，我将使用 analyzing-network-traffic-of-malware 技能。

步骤：
1. 使用 Wireshark 捕获网络流量
2. 过滤 DNS 查询，发现可疑域名：malware-c2.example.com
3. 分析 HTTP 请求，识别 C2 通信协议
4. 提取 IOC：
   - IP: 192.168.1.100
   - 域名: malware-c2.example.com
   - URL: /api/beacon
5. 映射到 MITRE ATT&CK：
   - 战术: Command and Control (TA0011)
   - 技术: Application Layer Protocol (T1071)
```

**示例 2：事件响应**

```
你：我们检测到一个安全事件，帮我做取证分析

AI：好的，我将使用 incident-response-forensic-analysis 技能。

步骤：
1. 识别事件范围：3 台服务器受影响
2. 收集易失性证据：内存转储、网络连接、进程列表
3. 磁盘镜像分析：发现恶意文件 C:\temp\backdoor.exe
4. 时间线重建：
   - 2026-06-15 14:30 - 攻击者初始访问
   - 2026-06-15 14:45 - 横向移动
   - 2026-06-15 15:00 - 数据窃取
5. 根因分析：钓鱼邮件导致凭证泄露
```

**示例 3：渗透测试**

```
你：帮我做一个 Web 应用的漏洞评估

AI：好的，我将使用 web-application-vulnerability-assessment 技能。

步骤：
1. 信息收集：子域名枚举、端口扫描
2. 漏洞扫描：SQL 注入、XSS、CSRF
3. 漏洞验证：确认 3 个高危漏洞
4. 风险评估：CVSS 评分 8.5
5. 修复建议：参数化查询、输入验证、CSRF Token
```

---

## 七、技能文件结构

### 7.1 文件格式

每个技能都是一个 Markdown 文件，包含 YAML frontmatter：

```markdown
---
name: skill-name
description: 技能描述
domain: security_domain
difficulty: beginner/intermediate/advanced/expert
time_estimate: 1-2 hours
mitre_attack:
  tactic: TA0001
  technique: T1001
nist_csf:
  function: DE
  category: DE.CM
---

# 技能名称

## 概述
技能的详细描述

## 步骤
1. 第一步
2. 第二步
...

## 工具
- 工具 1
- 工具 2

## 输出
- 输出 1
- 输出 2
```

### 7.2 目录结构

```
Anthropic-Cybersecurity-Skills/
├── skills/
│   ├── malware_analysis/
│   │   ├── analyzing-network-traffic-of-malware.md
│   │   ├── static-malware-analysis.md
│   │   └── dynamic-malware-analysis.md
│   ├── incident_response/
│   │   ├── incident-response-forensic-analysis.md
│   │   └── evidence-collection.md
│   ├── penetration_testing/
│   │   ├── web-application-vulnerability-assessment.md
│   │   └── network-penetration-testing.md
│   └── ... (26 个领域)
├── mappings/
│   ├── mitre_attack.yaml
│   ├── nist_csf.yaml
│   ├── mitre_atlas.yaml
│   ├── d3fend.yaml
│   └── nist_ai_rmf.yaml
├── tools/
│   ├── validator.py
│   └── index_generator.py
└── docs/
    └── ...
```

---

## 八、实际应用场景

### 8.1 企业安全团队

**场景：** 安全运营中心（SOC）需要快速响应安全事件

**解决方案：**
- 使用事件响应技能快速分析告警
- 使用威胁情报技能提取 IOC
- 使用取证技能重建攻击时间线

**效果：**
- 事件响应时间从 207 天缩短到 24 小时
- 分析效率提升 60%+
- 减少对高级安全专家的依赖

### 8.2 安全咨询公司

**场景：** 需要为客户提供渗透测试服务

**解决方案：**
- 使用渗透测试技能进行漏洞评估
- 使用安全架构技能进行威胁建模
- 使用合规审计技能检查合规性

**效果：**
- 测试覆盖率提升 40%
- 报告质量一致性提高
- 项目交付时间缩短 30%

### 8.3 安全培训

**场景：** 培训初级安全分析师

**解决方案：**
- 使用技能作为培训材料
- 通过实际案例学习安全技能
- 逐步提升技能难度

**效果：**
- 培训周期从 6 个月缩短到 2 个月
- 技能掌握度提升 50%
- 实战能力显著提高

---

## 九、与其他方案对比

### 9.1 对比表格

| 对比项 | Anthropic Cybersecurity Skills | 传统安全培训 | 商业安全工具 |
|--------|-------------------------------|-------------|-------------|
| **成本** | 免费开源 | 高（$5k-20k/人） | 高（$10k-100k/年） |
| **覆盖范围** | 754 个技能 | 有限 | 特定领域 |
| **框架映射** | 5 个框架 | 1-2 个 | 特定框架 |
| **AI 集成** | ✅ 原生支持 | ❌ | 有限 |
| **更新频率** | 持续更新 | 年度更新 | 季度更新 |
| **实战性** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |

### 9.2 核心优势

1. **结构化**：每个技能都有明确的步骤和输出
2. **标准化**：遵循 agentskills.io 开放标准
3. **框架映射**：5 个行业标准框架全覆盖
4. **AI 原生**：专为 AI 智能体设计
5. **社区驱动**：18k+ Stars，持续贡献

---

## 十、未来展望

### 10.1 技能扩展

- 更多安全领域覆盖
- 更多语言支持
- 更多 AI 平台集成

### 10.2 框架更新

- 跟进 MITRE ATT&CK 版本更新
- 支持新的安全框架
- 动态框架映射

### 10.3 AI 增强

- 更智能的技能推荐
- 自动化技能执行
- 学习和优化

---

## 总结

Anthropic Cybersecurity Skills 解决了 AI 在安全领域的核心问题：**缺乏结构化的安全知识**。

**以前：** AI 知道安全概念，但不知道怎么操作。

**现在：** AI 有了 754 个结构化技能，可以像专家一样分析安全事件。

这个项目的意义：

1. **降低门槛**：让初级分析师也能做专家级分析
2. **提高效率**：AI 辅助安全分析，效率提升 60%+
3. **标准化**：统一的安全技能标准
4. **开源免费**：Apache 2.0 许可证

如果你想让 AI 成为你的安全助手，Anthropic Cybersecurity Skills 是目前最好的选择。

---

**参考资源**：

- GitHub：https://github.com/mukul975/Anthropic-Cybersecurity-Skills
- agentskills.io 标准：https://agentskills.io/
- MITRE ATT&CK：https://attack.mitre.org/
- NIST CSF 2.0：https://www.nist.gov/cyberframework
