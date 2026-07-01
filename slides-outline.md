# 2 小时分享 PPT 大纲

---

## 总体节奏

| 时间 | 内容 | 页数 |
|---|---|---|
| 00:00 - 00:30 | 模块 1 | 8 张 |
| 00:30 - 00:50 | 模块 3 | 6 张 |
| 00:50 - 01:20 | 模块 4 | 10 张 |
| 01:20 - 01:50 | 现场演示 | 0 张(录屏) |
| 01:50 - 02:00 | 答疑 + 总结 | 2 张 |

---

## Slide 1:封面

```
研发部 AI Coding 培训
Claude Code + Codex + Skill 工具栈
2026-07-01
刘天平
```

---

## Slide 2:今天讲什么

```
2 小时讲 3 件大事
1. 模型与 Agent 全景(30 min)
2. Skill 工具选型(20 min)
3. 9 步闭环流程(30 min)
+ 现场演示(30 min)

其余 5 个模块 → 课后自学
```

---

## Slide 3:三层关系

```
        模型(底层能力)
            │
            ▼
       Agent(执行体)
       Claude Code
       Codex
       Hermes
            │
            ▼
       指令(Prompt + Skill)
```

**口述**:模型换不动,Agent 可以换,指令必须自己写。

---

## Slide 4:5 个主力模型(2026-07)

| 模型 | 一句话 |
|---|---|
| Claude Opus 4.8 / Sonnet 5 | 跨文件重构主力 |
| GPT-5.5(代号 Spud)+ Codex | 单文件补全、shell 脚本 |
| Gemini 3.5 Flash / 3.1 Pro | 200 万 token 一次性喂文档 |
| DeepSeek V3.2 / V4 Preview | 中文 / 便宜 / 国内合规 |
| Qwen3-Coder 1.5B-480B | 国产开源 / 私有化部署 |

**口述**:剩下几十个模型,先不管。注意:GPT-5.1 已退役(2026-03),别再用。

## Slide 4.5:模型选择决策树(新增)

```
需要 AI 改你的代码吗?
├── 是 → Claude Code / Codex CLI
│         └── 跨文件多?
│             ├── 是 → Claude Opus 4.8
│             └── 否 → GPT-5.5
└── 否 → 想要快速原型?
        ├── 是 → v0 / Lovable / Bolt.new / Base44
        └── 否 → 想要自主异步任务?
                ├── 是 → Devin / Jules
                └── 否 → 看 Gemini 长上下文
```

---

## Slide 5:Agent 分类(2026-07,4 大类)

```
                    本地                云端
CLI 主力         Claude Code CLI      (少)
                 Codex CLI
                 Hermes
                 Antigravity CLI
IDE 通用         Zed                  Cursor
                 Antigravity IDE      Windsurf / Copilot / Trae / Kiro
全栈 vibe 生成   (无)                 v0 / Lovable / Bolt.new / Base44
自主 Agent       (无)                 Devin / Jules
```

**口述**:2026 年 vibe coding 平台是新增赛道,面向非程序员。

---

## Slide 6:学员必须会的 2 个 Agent

```
主力:Claude Code(复杂重构、跨文件,跑 Claude Opus 4.8 / Sonnet 5)
辅助:Codex CLI(快速补全、shell,跑 GPT-5.5)

Cursor 不是包袱,但 CLI 工具更可控。

扩展(选学):
- IDE 派:Cursor / Windsurf / Trae / Antigravity IDE
- vibe 派:v0 / Lovable / Bolt.new / Base44(非程序员/原型)
- 自主派:Devin / Jules(异步大任务)
```

---

## Slide 7:用三个模型做同一件事(对比)

```
同一个任务:"解释当前目录 main.go 的功能"

Claude Opus 4.8:详细、会用 Read/Grep
GPT-5.5 (Codex):简洁、shell 风格
Gemini 3.5 Flash:均衡、快

→ 看哪个最像你的项目风格
```

---

## Slide 8:模型 × Agent × 指令

```
三者都对了,产出才稳。
三者错一个,效果归零。
```

---

## Slide 9(切换到模块 3):Skill 工具栈

```
AI Coding 工具栈
═══════════════════════════════════════════════
方法论   superpowers           (TDD 强制)
压缩     rtk                  (token 砍 60-90%)
索引     codebase-memory-mcp  (代码库知识图谱)
浏览器   agent-browser        (E2E / 抓网页)
```

---

## Slide 10:4 件套够用

```
一句话选型规则:
> 装 superpowers + rtk + codebase-memory-mcp + agent-browser 四个,其他按需。

多一个就是噪音。
```

---

## Slide 11:superpowers 一句话

```
装了之后,agent 会自动:
1. 先写测试(红)
2. 实现代码到测试过(绿)
3. 重构(REFACTOR)
4. 自动 review
```

---

## Slide 12:rtk 对比效果

```
# 原始 git status
[500 token 的输出]

# rtk git status
[modified] src/auth/login.go
[modified] src/auth/middleware.go
[30 token 的输出]
```

---

## Slide 13:codebase-memory-mcp vs grep

```
传统 grep:告诉你哪些文件有"auth"
codebase-memory:告诉你"Auth 入口是 X,依赖 Y 和 Z"
```

---

## Slide 14:选装工具的边界

| 工具 | 什么时候加 |
|---|---|
| oh-my-claudecode | 5 人以上团队 |
| everything-claude-code | 想偷懒 |
| mcp-codegraph | 架构师 |
| pm-skills | 产品经理 |

---

## Slide 15(切换到模块 4):9 步流程图

```
① user_story.md → ② design.md → ③ tasks.md
                                       ↓
⑨ openapi.yaml ← ⑧ e2e_report.md ← ⑦ api_test.log
       ↑                                ↓
       └──── ⑥ integration_test ← ⑤ reviewed_diff
                                       ↑
                                    ④ TDD
```

---

## Slide 16:每步对应工具

| 步骤 | 工具 | 产物 |
|---|---|---|
| ① 需求澄清 | pm-skills | user_story.md |
| ② 方案设计 | 主 agent + mcp-codegraph | design.md |
| ③ 任务规划 | sub-agent | tasks.md |
| ④ TDD 开发 | superpowers | test + impl |
| ⑤ Code Review | ECC | reviewed diff |
| ⑥ 集成测试 | 测试 agent | integration_test.log |
| ⑦ HTTP API | agent-browser | api_test.log |
| ⑧ E2E | playwright-mcp | e2e_report.md |
| ⑨ 文档 | 主 agent | openapi.yaml |

---

## Slide 17:坑 1 — 跳过 ① ③

```
症状:agent 写到一半跑偏,返工 2-3 倍

对策:
- ① ③ 必须人写或人 review
- 不写 user_story.md / tasks.md 不让 agent 动
```

---

## Slide 18:坑 2 — 不写测试

```
症状:agent 改一处炸三处,不敢重构

对策:
- superpowers 强制 TDD
- CI 加 coverage >= 80% 卡口
```

---

## Slide 19:坑 3 — 不写文档

```
症状:前后端联调扯皮,新人看不懂老接口

对策:
- 文档从代码生成(swag/openapi)
- CI 加"文档必须和代码同步"
```

---

## Slide 20:9 步不是死顺序

```
允许迭代(② 错了回 ② 重做)
但不能跳过:
- 不写 ① → 必返工
- 不写 ③ → agent 跑一半爆
- 不用 ④⑤ → 覆盖率上不去
- 不写 ⑨ → 后面的人接不上
```

---

## Slide 21:小改动不要走 9 步

```
9 步适用于:有完整需求的项目
不需要 9 步的:
- 单文件改动
- 改 typo
- 跑命令
- 简单脚本

直接让 agent 干。
```

---

## Slide 22:4 件套的 token 经济学

```
50 人 × 每天 200 次 shell × 平均 200 token 节省
= 每天节省 ≈ 2,000,000 token(约 $6-20 / 天)

一个月 ≈ $180-600

光 rtk 一项工具,一年省 $2000-7000。
```

---

## Slide 23:50 人分阶段推广

```
第 1 周:5 个骨干先用
第 2 周:加 10 人(共 15)
第 3 周:加 15 人(共 30)
第 4 周:全员 50 人

原因:
- 凭据池风控
- skill 沉淀需要迭代
- 培训资源有限
```

---

## Slide 24:现场演示(录屏)

```
3 段录屏,共 30 分钟
- 录屏 1(8 min):从 0 到 tasks.md
- 录屏 2(12 min):TDD 开发
- 录屏 3(10 min):E2E + 文档

关键演示:
- 一句话 prompt 触发 superpowers
- rtk 把 500 token 压到 30 token
- codebase-memory-mcp 找 Auth 模块
```

---

## Slide 25:课后资料

```
发给学员的资料包
A. 必读(2h):Claude Code / Codex / Agentskills / MCP
B. 选读(4h):superpowers / rtk / codebase-memory-mcp / agent-browser
C. 进阶:Hermes / Anthropic 创业手册 / hello-agents
```

---

## Slide 26:答疑 3 句话

```
1. 不要把 Cursor 经验当包袱——CLI 工具比 IDE 更可控。
2. 第一周别想覆盖所有人——挑 5 个骨干先用起来,再扩散。
3. 学 AI Coding 不是学工具,是学"怎么把任务拆给 agent"。
```

---

## Slide 27:结束

```
谢谢!
下周一 30 分钟答疑会,卡住的现场解决。
课后资料:~/Documents/xsn/ai/ 全部 11 份
```

---

## 备用页(给答疑用)

### 备用 1:Hermes 是什么

```
Hermes 是 Nous Research 出的开源 agent
- 多 provider(20+)
- 多平台(Telegram/Discord/Slack/...)
- 多工具 / 多 skill / 多 MCP
- 可本地运行

什么时候用:
- 想用国内模型 + Claude Code 体验
- 想跨平台用同一个 agent
- 想给团队搭一个统一的 agent 网关
```

### 备用 2:为什么不用 IDE

```
Cursor / Copilot 是 IDE 形态:
- 绑死 IDE
- 难做 CI/CD
- 难做脚本化

Claude Code / Codex 是 CLI 形态:
- 任何环境都能跑
- 可 CI/CD
- 可远程服务器
- 可脚本化
```

### 备用 3:token 省钱的本质

```
原始 git status:500 token
rtk git status:30 token

节省 470 token × N 次调用

Claude Sonnet 5 价格(参考):
输入 $3 / 1M token
输出 $15 / 1M token

50 人 × 每天 200 次调用 = 10000 次
每次省 470 token = 4,700,000 token ≈ $14/天
一年 ≈ $5000

仅 rtk 一项。
```

### 备用 4:为什么不直接上 ECC

```
ECC 有 48 agent + 135 skill
看着很爽,但:
- 装得多不一定都用得上
- 认知负担大
- 容易冲突

建议路径:
1. 先装 4 件套,跑 2 周
2. 再加 ECC 才有感觉
3. 不要一次装 10 个 skill

工具多了 = 噪音多了 = 反而慢
```

### 备用 5:学员问「我应该用哪个 vibe coding 平台」(2026-07 新增)

```
先问自己三个问题:
1. 你会写代码吗?
   - 会 → 用 Cursor / Claude Code,不用 vibe 平台
   - 不会 / 只想原型 → 看下面
2. 你要做什么?
   - 前端页面 → v0(Vercel 生态)
   - 全栈 SaaS → Lovable
   - 纯网页原型 → Bolt.new
   - 移动 App → Bolt.new (React Native)
3. 你在不在 Wix 生态?
   - 是 → Base44(2026-06 自研模型)
   - 否 → Lovable 更通用
```

### 备用 6:学员问「Devin/Jules 和 Claude Code 怎么选」(2026-07 新增)

```
Devin / Jules(异步自主 agent):
- 我扔一个 8 小时任务,它自己跑完
- 我去睡觉,醒来 PR 好了
- 适合:大型迁移、重构、单测补齐

Claude Code(同步 CLI):
- 我坐在电脑前,它跟我实时对话
- 我随时能打断 / 改方向
- 适合:日常开发、调试、学习

类比:
Devin/Jules = 把活外包给一个团队
Claude Code = 跟一个高级工程师结对编程
```

### 备用 7:学员问「GPT-5.1 还能用吗」(2026-07 新增)

```
答:不能。2026-03-11 已下架。

OpenAI 现在主推:
- GPT-5.5(2026-04,代号 Spud)——旗舰
- GPT-5.5 Instant(2026-05)——ChatGPT 默认,替代 5.3 Instant
- GPT-5.4 Pro / Thinking——高级版
```

### 备用 8:模型版本记忆口诀(2026-07 新增)

```
不要记版本号,记"谁家 + 谁最强":
- Anthropic:Opus 4.8 是最强,Sonnet 5 是新主力
- OpenAI:GPT-5.5 是旗舰,Instant 是默认
- Google:Gemini 3.5 Flash 是新发的,3.1 Pro 是稳的
- DeepSeek:V3.2 是稳的,V4 Preview 是新的
- Qwen:Coder 系列看参数(1.5B-480B),按需选

原则:
- 模型 6-12 个月会换一轮
- 不要背版本号,记"主推 + 谁最强"即可
```

---

## 讲前 1 小时检查清单

- [ ] PPT 全部打开过,无乱码
- [ ] 录屏 3 段都已生成,能正常播放
- [ ] 现场用 claude 和 codex 都跑通了一次
- [ ] rtk 已装好,对比效果准备好
- [ ] superpowers 已装好,跑通 TDD
- [ ] 课后资料包已打包,链接已发给学员
- [ ] 答疑群的 QR 码准备好
- [ ] 时钟 / 计时器准备好(防止超时)

---

## 讲后 1 小时

- [ ] 发课程回放
- [ ] 发速查表 PDF
- [ ] 发课后 4 周自学路径
- [ ] 收集学员反馈表