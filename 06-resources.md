# 模块 6:课后资料

> 现场不发讲,只发资料包。每天自学 30 分钟,4 周读完。

---

## 6.1 必读资料(2 小时内看完)

### 6.1.1 Claude Code 官方文档

- **链接**:https://docs.claude.com/en/docs/claude-code
- **重点章节**:
  - Quickstart(必读)
  - Slash commands(常用 /help /clear /compact)
  - Hooks(给高级用户)
  - MCP servers(给工具链搭建)
  - Plugins(给 skill 开发)
- **学习目标**:
  - 能用 `/help` `/compact` `/clear` `/model`
  - 能配 MCP server
  - 能写简单 Hooks

### 6.1.2 Codex CLI 官方文档

- **链接**:https://github.com/openai/codex/blob/main/README.md
- **重点章节**:
  - 安装与登录
  - 命令行参数(`--model` `--quiet` `--approval-mode`)
  - 配置文件(~/.codex/config.toml)
  - Sandbox 模式
- **学习目标**:
  - 能用不同 approval mode
  - 能配自定义 model
  - 能用 sandbox 跑危险命令

### 6.1.3 Agentskills 规范

- **链接**:https://agentskills.io/home
- **重点章节**:
  - Skill 文件结构(frontmatter + body)
  - Skill 触发条件(when to use)
  - Skill 验证清单
- **学习目标**:
  - 能写一个符合规范的 skill
  - 能区分 skill 和 prompt 的区别

### 6.1.4 MCP 协议

- **链接**:https://modelcontextprotocol.io
- **重点章节**:
  - MCP 是什么(JSON-RPC over stdio/HTTP)
  - Tools / Resources / Prompts 三类原语
  - 怎么写一个 MCP server
- **学习目标**:
  - 能看懂 MCP server 的代码
  - 能用现成的 MCP server

---

## 6.2 选读资料(4 小时内看完)

### 6.2.1 superpowers

- **链接**:https://github.com/obra/superpowers
- **重点章节**:
  - README 里的 "skills list"
  - "How it works" 原理
  - "Custom skills" 怎么加自己的 skill
- **学习目标**:
  - 能用 superpowers 跑 TDD
  - 能加自定义 skill

### 6.2.2 rtk(Rust Token Killer)

- **链接**:https://github.com/rtk-ai/rtk
- **重点章节**:
  - README 里的 "supported commands"
  - "configuration"
  - "limitations"(哪些命令不要走 rtk)
- **学习目标**:
  - 能配 rtk shell wrapper
  - 能识别 rtk 失效的场景

### 6.2.3 codebase-memory-mcp

- **链接**:https://github.com/DeusData/codebase-memory-mcp
- **重点章节**:
  - README 里的 "Quick start"
  - "Query syntax"
  - "Performance notes"
- **学习目标**:
  - 能用 codebase-memory-mcp 查代码
  - 知道它和 grep 的边界

### 6.2.4 agent-browser

- **项目主页**:搜索 agent-browser GitHub
- **重点章节**:
  - 命令列表
  - 配置文件
  - 配合 Claude Code 用法
- **学习目标**:
  - 能跑自动化脚本
  - 能截图 / 点击 / 输入

---

## 6.3 进阶资料(有兴趣再看)

### 6.3.1 Hermes Agent

- **官网**:https://hermes-agent.nousresearch.com/docs
- **GitHub**:https://github.com/NousResearch/hermes-agent
- **重点章节**:
  - Quick Start
  - CLI Reference
  - Configuration
  - Skills catalog
  - MCP guide
  - Providers(20+)
- **学习目标**:
  - 能装 Hermes 并配置 provider
  - 能用 cron / webhook / kanban
  - 能写自定义 tool

### 6.3.2 Anthropic《AI Native 创业手册》

- **PDF**:https://cdn.prod.website-files.com/6889473510b50328dbb70ae6/69fe2a55b93bb0732b1fe33c_The-Founders-Playbook-05062026_v3%20(1).pdf
- **适合**:技术负责人、架构师
- **重点章节**:
  - AI Native 公司的定义
  - 团队结构
  - 产品 / 工程 / GTM 的 AI Native 转型
- **学习目标**:
  - 知道 AI Native 公司的典型结构
  - 能对照自己公司做 gap 分析

### 6.3.3 hello-agents(中文教材)

- **链接**:https://github.com/datawhalechina/hello-agents
- **适合**:想自己写 agent 的人
- **重点章节**:
  - Agent 基础原理
  - ReAct / Plan-and-Execute 等范式
  - 代码示例(Python)
- **学习目标**:
  - 能写一个最小 agent
  - 能解释 ReAct 范式

### 6.3.4 oh-my-claudecode

- **链接**:https://github.com/Yeachan-Heo/oh-my-claudecode
- **重点章节**:
  - Quick start
  - "Multi-agent orchestration"
  - "Teams-first" 用法
- **学习目标**:
  - 能用主会话派活给子 agent
  - 能配置 ultrawork 模式

### 6.3.5 everything-claude-code(ECC)

- **链接**:https://github.com/affaan-m/everything-claude-code
- **重点章节**:
  - "Quick start"
  - "Available agents" 48 个 agent 列表
  - "Available skills" 135 个 skill 列表
- **学习目标**:
  - 能装 ECC
  - 能挑出自己需要的 5-10 个 skill

### 6.3.6 zeroclaw

- **链接**:https://github.com/zeroclaw-labs/zeroclaw
- **定位**:本地隐私优先的 agent 框架
- **重点章节**:
  - "Why ZeroClaw"
  - "Multi-channel" 用法
- **学习目标**:
  - 知道 zeroclaw 的本地隐私优势
  - 能跑起来作为 hermes 的替代品

### 6.3.7 Google ADK(2026-07 新增)

- **主站**:https://adk.dev/
- **官方文档**:https://google.github.io/adk-docs/
- **多语言仓库**:
  - Python:https://github.com/google/adk-python
  - Go:https://github.com/google/adk-go(2.0,2026-06)
  - TypeScript:https://github.com/google/adk-ts
  - Java:https://github.com/google/adk-java
  - Kotlin:https://github.com/google/adk-kotlin
- **重点章节**:
  - Quickstart
  - Workflow as graph(Go 2.0 主打)
  - Multi-agent orchestration
  - Tools 注册
- **学习目标**:
  - 知道 ADK 是 Google 官方多语言框架
  - 能根据公司技术栈选对应 ADK
  - 能跑通至少一个语言的 quickstart

### 6.3.8 adk-rust(2026-07 新增)

- **链接**:https://github.com/zavora-ai/adk-rust
- **官网**:https://adk-rust.com/
- **定位**:社区移植的 Rust 版 ADK(对齐 Google ADK 设计)
- **重点章节**:
  - README 里的 "Quick start"
  - "Architecture"
  - 对比 Google ADK 的设计差异
- **学习目标**:
  - 知道 adk-rust 不是 Google 官方
  - 知道 Rust 怎么实现 type-safe agent
  - 能跑通 simple_agent 示例

### 6.3.9 7 个 agent 框架速查表(2026-07 新增)

| 类别 | 项目 | 适合 |
|---|---|---|
| 成品 CLI | Codex / Hermes / ZeroClaw | 直接用 |
| 开发框架 | ADK Python / Go / TS / Java / Kotlin | 自研 agent |
| 开发框架(社区) | adk-rust | Rust 自研 |
| 教材 | hello-agents | 从 0 学 |

---

## 6.4 4 周自学路径

### 第 1 周:基础通识

```
周一  阅读 Claude Code Quickstart
周二  阅读 Codex README
周三  阅读 Agentskills 规范
周四  阅读 superpowers README
周五  跑通模块 5 案例 A 的前半部分
```

### 第 2 周:工具精通

```
周一  阅读 rtk README + 实测对比
周二  阅读 codebase-memory-mcp README + 实测查询
周三  阅读 agent-browser README + 跑一次自动化
周四  阅读 oh-my-claudecode / ECC README
周五  跑通模块 5 案例 A 的后半部分
```

### 第 3 周:流程闭环

```
周一  跑通模块 4 的练习 1(完整 9 步)
周二  阅读 MCP 协议文档
周三  阅读 Hermes Quick Start
周四  阅读 hello-agents(选修)
周五  跑通模块 5 案例 B(重构)
```

### 第 4 周:选型与产出

```
周一  阅读 Anthropic 创业手册(选读)
周二  完成模块 5 案例 C(bug 修复)
周三  写自己的「工具选型表」
周四  提交 demo PR
周五  复盘 + 分享会
```

---

## 6.5 学习方法建议

### 6.5.1 「读 + 跑 + 写」循环

```
读:看官方文档
跑:把示例跑一遍
写:写一篇 200 字的学习笔记发到群里
```

**好处**:
- 主动学习(不是被动看)
- 群里能看到同伴进度
- 学习笔记可搜索可回顾

### 6.5.2 笔记模板

```markdown
# <日期> <主题>

## 学到了什么(3 条)
1. ...
2. ...
3. ...

## 跑通的示例
```bash
claude -q "..."
```

## 还有疑问的
- ...
```

### 6.5.3 不要一次读太多

每份资料**最多 30 分钟**。读不完就跳,回头再读。

---

## 6.6 公司内网资料沉淀

建议在公司 GitLab 建一个 `ai-coding-training` 仓:

```
ai-coding-training/
├── docs/
│   ├── 01-overview.md
│   ├── 02-install.md
│   └── ...
├── recordings/
│   └── 2026-07-01-share.mp4
├── notes/
│   ├── liutianping-day1.md
│   └── ...
└── skills/
    └── company-internal/
        └── code-review-checklist/
```

**每周一次 skill review**:把大家写的 skill 收集起来,挑好的进内网仓。

---

## 6.7 常见问题 FAQ

**Q1:资料太多了,看不完怎么办?**
A:**只读必读资料**。选读资料按兴趣,进阶资料给中高级。

**Q2:看英文文档太慢怎么办?**
A:用 Claude / Codex 直接问:"把这段文档翻译成中文并总结 3 条要点"。

**Q3:学完能涨工资吗?**
A:不能直接涨。但**生产力提升 30%** 是合理的预期,公司会因为这个加薪。

**Q4:这些工具会不会很快过时?**
A:会。但**9 步闭环流程不变**。模型会换、Agent 会换、Skill 会换,但"先想清楚再动手"的流程不会变。

**Q5:我不想学,可以继续用 Cursor 吗?**
A:可以。但请别在群里教别人"Cursor 也能做这个"——那是两个不同的工具栈。