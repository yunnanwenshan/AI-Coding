# 模块 1:模型与 Agent 全景(2026-07 版)

---

## 1.1 三个核心概念(必须先讲清)

```
        模型(底层能力)
            │
            ▼
   ┌────────────────┐
   │   Agent        │  ← 执行体:工具调用 / 上下文管理
   │   Claude Code  │
   │   Codex CLI    │
   │   Hermes / Zed │
   └────────────────┘
            │
            ▼
        指令(Prompt + Skill)
```

**一句话总结**:模型换不动,Agent 可以换,指令必须自己写。

---

## 1.1.1 四种工程范式:从「怎么问」到「怎么让 Agent 跑起来」

这一节是给学员的「概念地图」,讲清楚四个工程领域分别解决什么问题、什么阶段出现、彼此关系。

### 一句话总览

四个概念**不是并列选择**,而是**从内到外的四层**。越往外越新、越工程化、越接近生产级。

```
┌────────────────────────────────────────────────┐
│ Harness Engineering(承载层)  ← 2026 新概念      │
│   - agent 周围的脚手架:tool/memory/日志/安全    │
├────────────────────────────────────────────────┤
│ Loop Engineering(循环层)     ← 2026 新概念      │
│   - observe → decide → act → verify 的循环      │
│   - 控制 agent 跑多快、何时停、错了怎么办       │
├────────────────────────────────────────────────┤
│ Context Engineering(上下文层) ← 2024-2025 兴起   │
│   - 给模型「看到什么」:历史/文档/检索/工具结果  │
├────────────────────────────────────────────────┤
│ Prompt Engineering(指令层)   ← 2022-2024 主流   │
│   - 怎么写一句话让模型听懂                       │
└────────────────────────────────────────────────┘
```

### 1. Prompt Engineering(提示词工程)

- **出现阶段**:2022 ChatGPT 爆发 → 2023-2024 成为主流技能
- **核心问题**:**「怎么写一句话让模型听懂」**
- **解决问题**:
  - 角色设定("你是资深 Go 工程师")
  - 格式约束("输出 Markdown 表格")
  - 思维链("先想 A,再想 B,最后给结论")
- **代表技术**:few-shot、CoT(Chain of Thought)、ReAct、role prompting
- **代表实践**:"Prompt Hero"、LangChain Hub、Prompt 模板市场
- **典型产物**:一个 `.prompt.md` 文件

### 2. Context Engineering(上下文工程)

- **出现阶段**:2024-2025 兴起,Anthropic 在 2025 年发布 *Effective Context Engineering for AI Agents* 后正式成为行业术语
- **核心问题**:**「模型需要看到什么才能答好」**
- **解决问题**:
  - **RAG**:把文档检索出来塞进上下文
  - **Memory**:历史对话怎么压缩、摘要、保留
  - **Tool 结果**:工具调用返回几千 token 怎么取舍
  - **System prompt vs User prompt**:哪些放系统、哪些放用户
  - **上下文预算**:Anthropic 在 Claude Code 默认把 tool 响应限制在 25000 token
- **关键观点**(Anthropic):「be thoughtful and keep your context informative, yet tight」
- **代表实践**:Claude Code 的 `/compact` 命令、Cursor 的 @codebase 索引、codebase-memory-mcp
- **典型产物**:system prompt + 检索器 + 压缩器

### 3. Harness Engineering(承载工程)

- **出现阶段**:2026 新词,由 Addy Osmani、Martin Fowler、OpenAI 内部推动。Stripe 一周跑 1300 个 AI PR 是这个概念的标杆案例
- **核心问题**:**「agent 周围的脚手架怎么搭」**
- **解决问题**:
  - **Tool 连接**:agent 能调哪些工具、工具怎么发现
  - **Memory 存储**:短期 / 长期 / 跨会话怎么存
  - **错误处理**:agent 调工具失败怎么办
  - **日志 / 追踪**:每个工具调用怎么审计
  - **安全 / 权限**:能不能跑 `rm -rf`、能不能访问敏感文件
  - **CI 集成**:agent 改完代码怎么进 CI
- **关键公式**(Addy Osmani 提出):`coding agent = AI model(s) + harness`
- **与 Context Engineering 的区别**:Context 关心「喂什么」,Harness 关心「整个运行环境」
- **代表实践**:Claude Code 的 hooks、Codex CLI 的 approval-mode、Superpowers 的 TDD 框架
- **典型产物**:hooks.yaml、permission rules、MCP server 配置

### 4. Loop Engineering(循环工程)

- **出现阶段**:2026 新词,与 Harness Engineering 同时兴起
- **核心问题**:**「agent 怎么循环迭代、什么时候停」**
- **解决问题**:
  - **循环结构**:ReAct / Plan-and-Execute / Reflexion 等范式怎么选
  - **Evaluator(评分器)**:单独的 agent 或程序给生成结果打分,没达标就重做
  - **退出条件**:达到标准停、超过预算停、超时停
  - **重试策略**:失败后是重做、换工具、还是回滚
  - **节奏控制**:agent 跑多快、有没有 throttle
- **关键观点**(loop engineering 倡导者):「Without feedback, AI is just generating guesses」
- **与 Harness Engineering 的区别**:Harness 是「agent 周围有什么」,Loop 是「agent 每次循环做什么」
- **代表实践**:superpowers 的 RED-GREEN-REFACTOR 循环、Claude Code 的 plan mode(plan → execute → review)
- **典型产物**:一个 evaluator + 一个循环驱动

### 四者关系与演进逻辑

| 阶段 | 主流工程范式 | 解决的核心问题 | 标志性事件 |
|---|---|---|---|
| 2022-2024 | Prompt Engineering | 模型听不听得懂 | ChatGPT 爆发,LangChain 流行 |
| 2024-2025 | Context Engineering | 模型看不看得全 | 长上下文模型出现,RAG 成熟 |
| 2025-2026 | Harness Engineering | agent 跑得稳不稳 | Claude Code / Codex 工业化,Stripe 一周 1300 PR |
| 2026- | Loop Engineering | agent 跑得好不好 | evaluator 模式成熟,自我迭代成为可能 |

### 给学员的实操建议

- **第 1 周**:把现有 prompt 写好(Prompt Engineering)
- **第 2 周**:用 codebase-memory-mcp / RAG 让 agent 看到上下文(Context Engineering)
- **第 3 周**:配 hooks、权限、CI 集成(Harness Engineering)
- **第 4 周**:设计 evaluator + 循环退出条件(Loop Engineering)

**一句话总结**:**学 AI Coding 的学习路径,就是从 Prompt → Context → Harness → Loop 的演进路径**。

### 推荐阅读

- Anthropic *Effective Context Engineering for AI Agents*:https://www.anthropic.com/engineering/building-effective-agents
- Addy Osmani *Agent Harness Engineering*:https://addyosmani.com/blog/agent-harness-engineering/
- MindStudio *Loop Engineering vs Harness Engineering*:https://www.mindstudio.ai/blog/loop-engineering-vs-harness-engineering

---

## 1.2 主流模型(2026-07 当前主推版)

### 1.2.1 Anthropic Claude 系列

| 模型 | 发布时间 | 强项 | 弱项 | 典型场景 | 产品地址 |
|---|---|---|---|---|---|
| Claude Opus 4.8 | 2026-05 | 旗舰推理、长上下文重构、复杂推理、安全编辑 | 速度一般、贵 | 跨文件改动、架构重构、最难任务 | [claude.com](https://claude.com/) · [定价](https://claude.com/pricing) |
| Claude Sonnet 5 | 2026-06 | 新一代主力,性能/价格平衡 | 比 Haiku 慢 | 日常编码主力 | [claude.com](https://claude.com/) · [定价](https://claude.com/pricing) |
| Claude Sonnet 4.6 | (上一代) | 仍是稳定生产选择 | 比 Sonnet 5 略弱 | 已有 CI 流水线 | [claude.com](https://claude.com/) · [定价](https://claude.com/pricing) |
| Claude Haiku 4.5 | 近期 | 快、便宜 | 复杂推理弱 | commit message / 简单 review / 批量任务 | [claude.com](https://claude.com/) · [定价](https://claude.com/pricing) |
| Claude Mythos | 2026 受限发布 | 顶级推理能力 | 暂未公开 | 未公开(仅少数公司可用) | [claude.com](https://claude.com/) |

**选 Claude 的信号**:
- 跨文件改动 ≥ 3 个文件
- 需要长上下文(10 万 token+)
- 安全编辑(不愿意 agent 乱删东西)
- 推理 / Agent / 长任务默认选 Opus 4.8
- 价格敏感场景选 Sonnet 5
- 速度 / 成本极敏感场景选 Haiku 4.5

### 1.2.2 OpenAI GPT 系列

| 模型 | 发布时间 | 强项 | 弱项 | 典型场景 | 产品地址 |
|---|---|---|---|---|---|
| GPT-5.5(代号 "Spud") | 2026-04 | 旗舰推理,通用能力强 | 贵 | 复杂任务、推理 | [chatgpt.com](https://chatgpt.com/) · [定价](https://openai.com/chatgpt/pricing/) |
| GPT-5.5 Instant | 2026-05 | 默认模型,替代 GPT-5.3 Instant | 复杂推理一般 | ChatGPT 默认 | [chatgpt.com](https://chatgpt.com/) |
| GPT-5.4 Pro / Thinking | 同期 | Pro 高级版 | 价格高 | 高级订阅用户 | [chatgpt.com](https://chatgpt.com/) |
| GPT-5.1 系列 | **已退役(2026-03)** | - | - | - | - |

**注意**:GPT-5.1 在 2026-03-11 已下架,不要再选它。

### 1.2.3 Google Gemini 系列

| 模型 | 发布时间 | 强项 | 弱项 | 典型场景 | 产品地址 |
|---|---|---|---|---|---|
| Gemini 3.5 Flash | 2026-05(I/O) | 速度极快、价格低 | 复杂推理一般 | 实时开发工作流 | [deepmind.google](https://deepmind.google/models/gemini/) |
| Gemini 3.1 Pro | 近期 | 复杂任务、创意概念 | 编辑精度一般 | 一次性喂整个文档库 | [deepmind.google](https://deepmind.google/models/gemini/) |
| Gemini 3.5 Pro | 推迟到 2026-06 | 顶级推理 | 价格高 | 高级订阅 | [deepmind.google](https://deepmind.google/models/gemini/) |

**选 Gemini 的信号**:
- 需要 200 万 token 超长上下文
- 视频/图像多模态理解
- 在 Google Cloud 生态内

### 1.2.4 国产模型

| 模型 | 发布时间 | 强项 | 弱项 | 典型场景 | 产品地址 |
|---|---|---|---|---|---|
| DeepSeek V4 Preview | 2026-04 | 中文好、价格低、推理强、新一代预览 | 仍在迭代 | 国内团队、降本、早期采用 | [api-docs.deepseek.com](https://api-docs.deepseek.com/) |
| GLM 5.2 | 近期 | 编程 + 长上下文 | - | 政企内网、Kilo Code 集成 | [z.ai](https://z.ai/) |
| MiniMax M3 | 2026-06-01 | 编码 + Agent、1M token 上下文(MSA)、原生多模态、开源权重 | 新模型、仍在生态完善 | 国内业务、长上下文场景 | [minimax.io](https://www.minimax.io/models/text/m3) |

---

## 1.3 AI Coding Agent 分类(本地 vs 云端 × 通用 vs 垂直)

### 1.3.1 总览矩阵(2026-07 版)

```
                    本地(本地机/内网)              云端(SaaS)
通用编程(CLI)   ┃ Claude Code CLI              ┃ (无典型,IDE 为主)
                ┃ Codex CLI                    ┃
                ┃ Hermes CLI                   ┃
                ┃ OpenCode / OpenClaw          ┃
                ┃ Google Antigravity Go CLI    ┃
通用编程(IDE)   ┃ Zed                          ┃ Cursor
                ┃ Antigravity IDE              ┃ Windsurf(Codeium/Cognition)
                ┃ Kiro(AWS spec-driven)        ┃ GitHub Copilot
                ┃ Trae IDE(字节,原 MarsCode)    ┃ Augment Code
                ┃ Cline / Continue             ┃ Trae IDE 云版
                ┃ Zed AI                       ┃ Replit Agent
垂直全栈生成    ┃ (多为云端)                    ┃ v0(Vercel,2026 转 full-stack)
                ┃                              ┃ Lovable(原 GPT Engineer)
                ┃                              ┃ Bolt.new(StackBlitz)
                ┃                              ┃ Base44(Wix,2026-06 自研模型)
自主 Agent     ┃                              ┃ Devin(Cognition)
                ┃                              ┃ Jules(Google,异步)
```

### 1.3.2 详细说明(给学员参考)

#### 本地通用编程 Agent

| Agent | 语言/形态 | 强项 | 上手难度 | 适合谁 | 产品地址 |
|---|---|---|---|---|---|
| Claude Code | Node.js / CLI | 工具调用最稳、生态最全(Skill/MCP) | 低 | 后端 / 全栈主力 | [anthropic.com/product/claude-code](https://www.anthropic.com/product/claude-code) |
| Codex CLI | Rust / CLI | shell 闭环强、GPT-5.5 集成、500 万周活 | 低 | 日常编码主力 | [developers.openai.com/codex](https://developers.openai.com/codex/) · [chatgpt.com/codex](https://chatgpt.com/codex) |
| Hermes | Python / CLI | 多 provider、本地优先、跨平台 gateway | 中 | 多 provider / 隐私场景 | [hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com/) · [GitHub](https://github.com/NousResearch/hermes-agent) |
| OpenCode | Go / CLI | 开源、可私有化 | 中 | 私有化部署 | [GitHub](https://github.com/opencode-ai/opencode) |
| Antigravity CLI | Go / CLI | Google 出品、Gemini 集成、agent-first | 中 | Google 生态 | [antigravity.google](https://antigravity.google/) |
| Zed | Rust / IDE | 启动 0.4 秒、输入延迟 2ms、原生 agentic | 低 | 追求性能 | [zed.dev](https://zed.dev/) |
| Cline / Continue | TS / VSCode 插件 | 装在 VSCode 上 | 低 | 已有 VSCode 工作流 | [GitHub Cline](https://github.com/cline/cline) · [Continue](https://www.continue.dev/) |
| Aider | Python / CLI | git commit 驱动 | 中 | 喜欢 git 风格的人 | [aider.chat](https://aider.chat/) |

#### 云端 SaaS(IDE 形态)

| 工具 | 母公司 | 强项 | 弱项 | 适合谁 | 产品地址 |
|---|---|---|---|---|---|
| Cursor | Cursor Inc. | 补全 + Composer 多文件编辑 + @codebase 索引 | IDE 锁定 | 全栈、个人开发者 | [cursor.com](https://cursor.com/) · [定价](https://cursor.com/pricing) |
| GitHub Copilot | GitHub/Microsoft | 生态最大、价格低、企业管控 | 复杂推理一般 | 企业团队标配 | [github.com/features/copilot](https://github.com/features/copilot) |
| Windsurf | Codeium → Cognition | Cascade 多步骤 agent | 新兴、不稳定 | 想试新工具的人 | [codeium.com/windsurf](https://codeium.com/windsurf) |
| Trae IDE | 字节跳动 | SOLO 模式、多 agent、价格低 | 国内为主 | 国内团队 | [trae.ai](https://www.trae.ai/download) |
| Kiro IDE | AWS | spec-driven(转 requirements→design→tasks→code) | 新兴 | 企业 spec 流程 | [kiro.dev](https://kiro.dev/) · [下载](https://kiro.dev/downloads/) |
| Antigravity IDE | Google DeepMind | Gemini 3 Pro 集成、桌面 app | 新兴 | Google 生态 | [antigravity.google](https://antigravity.google/product/antigravity-ide) |
| Augment Code | Augment | 大代码库性能强 | 价格高 | 大型企业 | [augmentcode.com](https://www.augmentcode.com/) |
| Replit Agent | Replit | 浏览器内跑全栈 + 部署 | 复杂项目有限制 | 全栈快速原型 | [replit.com](https://replit.com/) |

#### 垂直专精(全栈生成平台)

这是 2026 年增长最快的赛道。**面向"非程序员"或"想快速原型"的人**。

| 工具 | 母公司 | 强项 | 弱项 | 适合谁 | 产品地址 |
|---|---|---|---|---|---|
| v0 | Vercel | Next.js 全栈、600 万用户、生产 sandbox | 绑 Vercel 生态 | 前端原型、产品 demo | [v0.app](https://v0.app/) |
| Lovable | Lovable AB | 聊天式 UI 生成、SaaS 应用 | 复杂逻辑有限 | 创业者、产品经理 | [lovable.dev](https://lovable.dev/) |
| Bolt.new | StackBlitz | 浏览器内 WebContainer、React Native | 性能有限 | 快速原型 | [bolt.new](https://bolt.new/) |
| Base44 | Wix | 自研模型(2026-06 发布)、vibe coding | 新兴 | 非技术创业者 | [base44.com](https://base44.com/) |

#### 自主 Agent(异步云端)

| 工具 | 母公司 | 强项 | 弱项 | 适合谁 | 产品地址 |
|---|---|---|---|---|---|
| Devin | Cognition | "AI 软件工程师",自主 PR | 价格高、需要监督 | 复杂迁移任务 | [devin.ai](https://devin.ai/) · [定价](https://devin.ai/pricing) |
| Jules | Google | 异步 coding agent、VM 跑、GitHub PR 交付 | 仍在公测 | 长时间后台任务 | [jules.google](https://jules.google/) |

---

## 1.4 不同角色应该选什么(2026-07 版)

### 1.4.1 按角色推荐

| 角色 | 推荐组合 | 理由 |
|---|---|---|
| **后端工程师** | Claude Code CLI + Cursor IDE | Claude 跨文件重构 + Cursor 日常补全 |
| **前端工程师** | v0(原型)+ Cursor(正式)+ Claude Code(复杂逻辑) | v0 快速出 UI,Cursor 维护 |
| **全栈工程师** | Claude Code + Codex + Antigravity | 跨栈需要多模型 |
| **DevOps / SRE** | Codex CLI + Hermes | shell 闭环强、可脚本化 |
| **数据工程师** | Gemini 3.5 Flash(长上下文)+ Claude Code | SQL/Python 长查询 |
| **架构师 / 重构** | Antigravity + Claude Code + mcp-codegraph | 跨项目分析 |
| **产品经理** | Lovable / Bolt.new(原型)+ pm-skills | 不写代码也能出 demo |
| **创业者(非技术)** | Base44 / Lovable | vibe coding 平台,自研模型越来越强 |
| **设计师** | v0 + Figma + Bolt.new | UI 优先 |
| **学生 / 学习者** | Cursor + Claude Code 免费层 | 上手快 |

### 1.4.2 按公司规模推荐

| 公司规模 | 推荐组合 | 理由 |
|---|---|---|
| 个人 | Cursor + Claude Code | 上手最快 |
| 5 人以下 | + Codex CLI + superpowers | 多工具覆盖 |
| 20-50 人 | + Hermes(多 provider 池)+ ECC + oh-my-claudecode | 团队编排 |
| 50-200 人 | + Antigravity(Kiro 备选)+ 内网 Skill registry | 标准化 |
| 200 人+ | + 自研 Agent 网关 + Jules/Devin(异步任务) | 流水线化 |

### 1.4.3 按使用场景推荐

| 场景 | 首选 | 备选 |
|---|---|---|
| 写新功能 | Claude Code + superpowers(TDD) | Cursor Composer |
| 重构老代码 | Claude Code + codebase-memory-mcp + Antigravity | Kiro spec-driven |
| 修 bug | Codex CLI(快速定位) | Claude Code(深度分析) |
| 写脚本 | Codex CLI + shell | Aider |
| 写前端原型 | v0 / Bolt.new | Lovable |
| 写全栈新应用 | Lovable / Bolt.new / Base44 | Claude Code + Cursor |
| 长文档分析 | Gemini 3.5 Flash(200 万 token) | Claude Sonnet 5(20 万 token) |
| 异步大任务 | Jules(Google) / Devin | Antigravity CLI |
| 企业私有化 | GLM 5.2 + OpenCode | DeepSeek V4 + 本地 Agent |

---

## 1.5 学员必须会选的 2 个 Agent

**Claude Code**(主力) + **Codex**(辅助):

| 维度 | Claude Code | Codex |
|---|---|---|
| 适合任务 | 复杂重构、跨文件、TDD | 快速补全、shell 脚本 |
| 学习曲线 | 中 | 低 |
| Skill 生态 | 最丰富(superpowers/ECC) | 一般 |
| 当前模型 | Claude Opus 4.8 / Sonnet 5 | GPT-5.5 |
| 计费 | 按 token | 按 token |

---

## 1.6 详细操作:本节内容怎么练

### 练习 1:让三个模型做同一件事,对比结果

```bash
# 同一个任务:"解释当前目录 main.go 的功能"
claude -q "解释当前目录 main.go 的功能"
codex -q "解释当前目录 main.go 的功能"
# 如果有 Gemini CLI:
gemini -q "解释当前目录 main.go 的功能"
```

观察:
- 谁答得最准?
- 谁会用工具(Read/Grep)?
- 谁的代码风格更像你的项目?

### 练习 2:看一张完整的工具栈图

打开 `appendix-cheatsheet.md`,对着图把每个工具指认一遍。

### 练习 3:画你自己的「模型选型矩阵」

格式:

```
| 我当前的项目 | 推荐模型 | 推荐 Agent | 理由 |
|---|---|---|---|
| 后端重构 | Claude Opus 4.8 | Claude Code | 跨文件多 |
| 写脚本 | GPT-5.5 | Codex CLI | 单文件快 |
| 读 PM 文档 | Gemini 3.5 Flash | Claude Code web | 长上下文 |
| 前端原型 | - | v0 / Lovable | 快 |
```

### 练习 4:试用一个 vibe coding 平台

打开 https://v0.app 或 https://lovable.dev 或 https://base44.com,做一个简单 demo(比如 todo 列表),体验「非程序员怎么用 AI 写代码」。

---

## 1.7 验收标准

讲完之后,每个学员能回答:

1. 「模型 × Agent × 指令」三者关系是什么?
2. 跨文件重构用什么模型?(答 Claude Opus 4.8 / Sonnet 5)
3. 200 万 token 上下文的模型是谁?(答 Gemini 3.5 Flash / 3.1 Pro)
4. Cursor / Claude Code / Codex 的本质区别是什么?
5. vibe coding 平台有哪些?面向谁?(v0/Lovable/Bolt.new/Base44,面向非程序员或快速原型)
6. 自主 Agent 有哪些?和 CLI 的区别?(Devin/Jules,异步云端)

---

## 1.8 现场讲稿(讲师参考,30 分钟)

**第 1 段(8 min)**:画上面那个三层图,讲三者关系。

**第 2 段(10 min)**:讲模型表。只挑 5 家:
- Anthropic(Opus 4.8 / Sonnet 5)
- OpenAI(GPT-5.5 / GPT-5.5 Instant)
- Google(Gemini 3.5 Flash / 3.1 Pro)
- DeepSeek(V4 Preview)
- 国产合规替代:GLM 5.2、minimax m3

**第 3 段(8 min)**:讲 Agent 分类,重点对比:
- **CLI 派**:Claude Code / Codex / Hermes
- **IDE 派**:Cursor / Windsurf / Trae / Antigravity
- **vibe coding 派**:v0 / Lovable / Bolt.new / Base44
- **自主 Agent 派**:Devin / Jules

强调:你的 Cursor 经验**不是包袱,是过渡形态**,但 vibe coding 平台是新赛道,要关注。

**第 4 段(4 min)**:讲「学 AI Coding 不是学工具,是学怎么把任务拆给 agent」。

---

## 1.9 常见问题 FAQ

**Q1:已经有 Cursor,还要不要装 Claude Code?**
A:要。Cursor 是 IDE 形态,CLI 工具在 CI/CD、远程服务器、脚本化场景不可替代。

**Q2:Hermes 是不是冗余的?**
A:不是冗余,是不同定位。Hermes 的优势是多 provider 切换 + 本地运行 + 跨平台 gateway。先让 5 个骨干试,不要 50 人一起上。

**Q3:公司必须用国产模型怎么办?**
A:DeepSeek V4 + GLM 5.2 + minimax m3 是国内三个选择。Codex/Claude Code 通过 API 调,模型层可替换。

**Q4:模型会不会很快过时?**
A:会(看看 GPT-5.1 已经退役)。所以选 Agent 而不是选模型。Agent 之上,模型可以随时换。

**Q5:vibe coding 平台(像 Base44)能代替 Claude Code 吗?**
A:不能。**vibe coding 面向非程序员或快速原型**,生产级复杂项目还是 CLI/IDE agent 的天下。两者不冲突,是不同赛道。

**Q6:Devin 和 Claude Code 区别是什么?**
A:Devin 是**异步自主 agent**(云端 VM 跑,自动开 PR),Claude Code 是**同步 CLI**(你在本地,实时交互)。Devin 适合"扔给它一个 8 小时任务",Claude Code 适合"我和它一起干活"。

**Q7:Trae(MarsCode 改名)和 Cursor 比哪个好?**
A:Trae 国内访问快、价格低、SOLO 模式强;Cursor 国际化、社区大、Composer 成熟。**国内团队推 Trae,海外团队推 Cursor**。

**Q8:Base44 自研模型意味着什么?**
A:2026-06-29 Wix 让 Base44 用自研模型。这是 vibe coding 平台**摆脱对 OpenAI/Anthropic 依赖**的信号。意味着这个赛道会越来越细分。

**Q9:Kiro(spec-driven)适合谁?**
A:适合**企业团队**,尤其是有完整 spec 流程的(政府、金融、医疗)。AWS 出品,AWS 生态内最佳。

**Q10:Antigravity 和 Antigravity IDE 什么关系?**
A:同一个团队(Google DeepMind),两个产品:**CLI(Go 写的命令行)+ IDE(桌面应用)**。CLI 给 DevOps,IDE 给全栈开发者。