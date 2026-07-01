# 模块 1:模型与 Agent 全景(2026-07 版)

> 现场讲 30 分钟。文档写详细可操作,课后自学约 60 分钟。
> 版本说明:2026-07-01 核验,所有版本号均经 web 搜索确认。

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

## 1.2 主流模型(2026-07 当前主推版)

> 重要警告:模型版本迭代极快。**只记"谁家最强 + 当前主推",不记具体数字**。
> 以下为 2026-07-01 核验版本号,可能 3-6 个月内再换一轮。

### 1.2.1 Anthropic Claude 系列

| 模型 | 发布时间 | 强项 | 弱项 | 典型场景 |
|---|---|---|---|---|
| Claude Opus 4.8 | 2026-05 | 长上下文重构、复杂推理、安全编辑 | 速度一般、贵 | 跨文件改动、架构重构 |
| Claude Sonnet 5 | 2026-06 | 性能/价格平衡的新一代主力 | 比 Haiku 慢 | 日常编码主力 |
| Claude Sonnet 4.6 | (上一代) | 仍是稳定生产选择 | 比 Sonnet 5 略弱 | 已有 CI 流水线 |
| Claude Haiku 4.5 | 近期 | 快、便宜 | 复杂推理弱 | commit message / 简单 review |
| Claude Mythos | 2026 受限发布 | 顶级推理能力 | 暂未公开 | 未公开 |

**选 Claude 的信号**:
- 跨文件改动 ≥ 3 个文件
- 需要长上下文(10 万 token+)
- 安全编辑(不愿意 agent 乱删东西)

### 1.2.2 OpenAI GPT 系列

| 模型 | 发布时间 | 强项 | 弱项 | 典型场景 |
|---|---|---|---|---|
| GPT-5.5(代号 "Spud") | 2026-04 | 旗舰推理,通用能力强 | 贵 | 复杂任务、推理 |
| GPT-5.5 Instant | 2026-05 | 默认模型,替代 GPT-5.3 Instant | 复杂推理一般 | ChatGPT 默认 |
| GPT-5.4 Pro / Thinking | 同期 | Pro 高级版 | 价格高 | 高级订阅用户 |
| GPT-5.1 系列 | **已退役(2026-03)** | - | - | - |

**注意**:GPT-5.1 在 2026-03-11 已下架,不要再选它。

### 1.2.3 Google Gemini 系列

| 模型 | 发布时间 | 强项 | 弱项 | 典型场景 |
|---|---|---|---|---|
| Gemini 3.5 Flash | 2026-05(I/O) | 速度极快、价格低 | 复杂推理一般 | 实时开发工作流 |
| Gemini 3.1 Pro | 近期 | 复杂任务、创意概念 | 编辑精度一般 | 一次性喂整个文档库 |
| Gemini 3.5 Pro | 推迟到 2026-06 | 顶级推理 | 价格高 | 高级订阅 |

**选 Gemini 的信号**:
- 需要 200 万 token 超长上下文
- 视频/图像多模态理解
- 在 Google Cloud 生态内

### 1.2.4 国产模型

| 模型 | 发布时间 | 强项 | 弱项 | 典型场景 |
|---|---|---|---|---|
| DeepSeek V3.2 | 2025-12 | 中文好、价格低、推理强 | 英文代码风格一般 | 国内团队、降本 |
| DeepSeek V4 Preview | 2026-04 | 新一代预览 | 仍在迭代 | 早期采用 |
| Kimi K2.5 | 2026-01 | 多模态(MoonViT 视觉编码器) | 工具调用相对新 | 中文文档分析 |
| Kimi K2.6 | 近期 | 长程编码、300 子 agent 编排 | 资源消耗大 | 多 agent 场景 |
| Kimi K2.7 Code | 2026-06 | 1.06T 参数 MoE,真正的开源 | 模型大 | 私有化部署 |
| GLM 5 | 2026-02 | 编程 + 长程 agent | 英文生态弱 | 政企内网 |
| GLM 5.2 | 近期 | 编程 + 长上下文 | - | Kilo Code 集成 |
| Qwen3-Coder 系列 | 2026 | 1.5B-480B MoE 全档 | 480B 部署成本高 | 私有化部署首选 |
| MiniMax | 持续更新 | 多模态、价格低 | 通用基模 | 国内业务 |

### 1.2.5 选模型的三条规则(2026-07 版)

1. **代码改动跨 3 个文件以上 → Claude Opus 4.8 / Sonnet 5**
2. **单文件补全 / shell 脚本 → GPT-5.5 / Codex + GPT-5.5 Instant**
3. **整个文档库分析 → Gemini 3.5 Flash / 3.1 Pro**(200 万 token)

**国内合规替代**:
- DeepSeek V3.2 / V4(降本)
- Qwen3-Coder(开源私有化)
- GLM 5.2(政企)

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

| Agent | 语言/形态 | 强项 | 上手难度 | 适合谁 |
|---|---|---|---|---|
| Claude Code | Node.js / CLI | 工具调用最稳、生态最全(Skill/MCP) | 低 | 后端 / 全栈主力 |
| Codex CLI | Rust / CLI | shell 闭环强、GPT-5.5 集成、500 万周活 | 低 | 日常编码主力 |
| Hermes | Python / CLI | 多 provider、本地优先、跨平台 gateway | 中 | 多 provider / 隐私场景 |
| OpenCode | Go / CLI | 开源、可私有化 | 中 | 私有化部署 |
| Antigravity CLI | Go / CLI | Google 出品、Gemini 集成、agent-first | 中 | Google 生态 |
| Zed | Rust / IDE | 启动 0.4 秒、输入延迟 2ms、原生 agentic | 低 | 追求性能 |
| Cline / Continue | TS / VSCode 插件 | 装在 VSCode 上 | 低 | 已有 VSCode 工作流 |
| Aider | Python / CLI | git commit 驱动 | 中 | 喜欢 git 风格的人 |

#### 云端 SaaS(IDE 形态)

| 工具 | 母公司 | 强项 | 弱项 | 适合谁 |
|---|---|---|---|---|
| Cursor | Cursor Inc. | 补全 + Composer 多文件编辑 + @codebase 索引 | IDE 锁定 | 全栈、个人开发者 |
| GitHub Copilot | GitHub/Microsoft | 生态最大、价格低、企业管控 | 复杂推理一般 | 企业团队标配 |
| Windsurf | Codeium → Cognition | Cascade 多步骤 agent | 新兴、不稳定 | 想试新工具的人 |
| Trae IDE | 字节跳动 | SOLO 模式、多 agent、价格低 | 国内为主 | 国内团队 |
| Kiro IDE | AWS | spec-driven(转 requirements→design→tasks→code) | 新兴 | 企业 spec 流程 |
| Antigravity IDE | Google DeepMind | Gemini 3 Pro 集成、桌面 app | 新兴 | Google 生态 |
| Augment Code | Augment | 大代码库性能强 | 价格高 | 大型企业 |
| Replit Agent | Replit | 浏览器内跑全栈 + 部署 | 复杂项目有限制 | 全栈快速原型 |

#### 垂直专精(全栈生成平台)

> 这是 2026 年增长最快的赛道。**面向"非程序员"或"想快速原型"的人**。

| 工具 | 母公司 | 强项 | 弱项 | 适合谁 |
|---|---|---|---|---|
| v0 | Vercel | Next.js 全栈、600 万用户、生产 sandbox | 绑 Vercel 生态 | 前端原型、产品 demo |
| Lovable | Lovable AB | 聊天式 UI 生成、SaaS 应用 | 复杂逻辑有限 | 创业者、产品经理 |
| Bolt.new | StackBlitz | 浏览器内 WebContainer、React Native | 性能有限 | 快速原型 |
| Base44 | Wix | 自研模型(2026-06 发布)、vibe coding | 新兴 | 非技术创业者 |

#### 自主 Agent(异步云端)

| 工具 | 母公司 | 强项 | 弱项 | 适合谁 |
|---|---|---|---|---|
| Devin | Cognition | "AI 软件工程师",自主 PR | 价格高、需要监督 | 复杂迁移任务 |
| Jules | Google | 异步 coding agent、VM 跑、GitHub PR 交付 | 仍在公测 | 长时间后台任务 |

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
| 企业私有化 | Qwen3-Coder 480B + 本地 Agent | GLM 5.2 + OpenCode |

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
- DeepSeek(V3.2 / V4 Preview)
- Qwen(Qwen3-Coder 全档)

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
A:DeepSeek V3.2 + Qwen3-Coder + GLM 5.2 是开源可私有化的三个选择。Codex/Claude Code 通过 API 调,模型层可替换。

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