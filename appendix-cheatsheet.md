# 附录:一页 A4 速查表(2026-07 版)

---

## A.1 三层关系

```
模型 × Agent × 指令
```

**模型**决定能力,**Agent** 决定执行,**指令**决定约束。三者都对了,产出才稳。

---

## A.2 必装工具

| 类别 | 工具 | 命令 |
|---|---|---|
| 主 agent | Claude Code | `npm i -g @anthropic-ai/claude-code` |
| 辅 agent | Codex CLI | `npm i -g @openai/codex` |
| 方法论 | superpowers | `git clone https://github.com/obra/superpowers.git ~/.claude/skills/superpowers` |
| 压缩 | rtk | `curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/main/install.sh \| bash` |
| 索引 | codebase-memory-mcp | 见模块 2.3.3 |
| 浏览器 | agent-browser | `brew install agent-browser` |

---

## A.3 9 步闭环

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

## A.4 3 个不能跳的步骤

1. **① ③ 必须人写**:不写 user_story.md / tasks.md,agent 跑一半爆
2. **④ 用 superpowers 强制 TDD**:不写测试,改一处炸三处
3. **⑨ 必须从代码生成**:手写文档一定过期

---

## A.5 3 个最容易踩的坑

| 坑 | 症状 | 对策 |
|---|---|---|
| 跳过 ① ③ | agent 跑偏、返工 2-3 倍 | 不写不让 agent 动 |
| 不写测试 | agent 改一处炸三处 | superpowers 强制 |
| 不写文档 | 前后端联调扯皮 | 自动生成 + CI 卡口 |

---

## A.6 模型速查(2026-07 主推版)

### A.6.1 海外主力

| 模型 | 一句话 |
|---|---|
| Claude Opus 4.8 | 跨文件重构旗舰 |
| Claude Sonnet 5 | 新一代主力(2026-06) |
| Claude Haiku 4.5 | 快补全 / commit message |
| GPT-5.5(代号 Spud) | OpenAI 旗舰推理 |
| GPT-5.5 Instant | ChatGPT 默认 |
| Gemini 3.5 Flash | 200 万 token 实时开发 |
| Gemini 3.1 Pro | 复杂任务 + 创意 |

### A.6.2 国产主力

| 模型 | 一句话 |
|---|---|
| DeepSeek V3.2 | 中文 + 价格低 + 国内合规 |
| DeepSeek V4 Preview | 新一代预览 |
| Kimi K2.5 / K2.7 Code | 多模态 / 1T MoE 开源 |
| GLM 5 / 5.2 | 编程 + 长程 agent |
| Qwen3-Coder 1.5B-480B | 开源私有化首选 |
| MiniMax | 国内业务多模态 |

### A.6.3 ⚠️ 已退役别再用

- GPT-5.1 系列(2026-03 已下架)
- Claude Sonnet 4 / Opus 4(已被 4.6/4.8/5 替代,新项目不要用)
- Gemini 2.5 系列(已被 3.1+ 替代)

---

## A.7 Agent 速查(2026-07)

### A.7.1 CLI 形态(主力推荐)

| Agent | 一句话 | 适合 |
|---|---|---|
| Claude Code | 工具调用最稳、生态最全 | 后端 / 全栈 |
| Codex CLI | shell 闭环强、GPT-5.5 集成 | 日常编码 |
| Hermes | 多 provider、本地优先、跨平台 | 多模型 / 隐私 |
| Antigravity CLI | Google 出品、Gemini 集成 | Google 生态 |
| OpenCode | 开源可私有化 | 私有化部署 |
| Aider | git commit 驱动 | git 风格用户 |

### A.7.2 IDE 形态

| Agent | 一句话 | 适合 |
|---|---|---|
| Cursor | 补全 + Composer + @codebase | 个人/小团队 |
| GitHub Copilot | 生态最大、企业管控 | 企业标配 |
| Windsurf | Cascade 多步 agent | 愿意试新的人 |
| Trae IDE(字节) | SOLO 模式、价格低 | 国内团队 |
| Kiro(AWS) | spec-driven → 代码 | 企业 spec 流程 |
| Antigravity IDE | Gemini 3 Pro、桌面 app | Google 生态 |
| Zed | 启动 0.4s、原生 agentic | 追求性能 |

### A.7.3 Vibe Coding 平台(全栈生成)

| 平台 | 一句话 | 适合 |
|---|---|---|
| v0(Vercel) | Next.js 全栈、600 万用户 | 前端原型、产品 demo |
| Lovable | 聊天式 UI、SaaS 应用 | 创业者、产品经理 |
| Bolt.new(StackBlitz) | 浏览器内 WebContainer | 快速原型 |
| Base44(Wix) | 自研模型、vibe coding | 非技术创业者 |

### A.7.4 自主 Agent(异步云端)

| Agent | 一句话 | 适合 |
|---|---|---|
| Devin(Cognition) | "AI 软件工程师"、自主 PR | 复杂迁移任务 |
| Jules(Google) | 异步、VM 跑、GitHub PR 交付 | 长时间后台任务 |

---

## A.8 不同角色选 Agent(速查)

| 角色 | 推荐组合 |
|---|---|
| 后端工程师 | Claude Code + Cursor |
| 前端工程师 | v0(原型)+ Cursor + Claude Code |
| 全栈工程师 | Claude Code + Codex + Antigravity |
| DevOps/SRE | Codex CLI + Hermes |
| 数据工程师 | Gemini 3.5 Flash + Claude Code |
| 架构师/重构 | Antigravity + Claude Code + mcp-codegraph |
| 产品经理 | Lovable / Bolt.new + pm-skills |
| 创业者(非技术) | Base44 / Lovable |
| 设计师 | v0 + Figma + Bolt.new |

---

## A.9 紧急排错

| 问题 | 解决 |
|---|---|
| Claude 登录掉线 | `rm -rf ~/.claude/auth.json && claude` |
| Codex "No model" | `codex login` |
| rtk 没生效 | `eval "$(rtk init zsh)" && source ~/.zshrc` |
| MCP 启动失败 | `claude --debug` 看日志 |
| agent 跑偏 | 回 ① 重写 user_story.md |
| GPT-5.1 模型找不到 | 已退役,改用 GPT-5.5 |

---

## A.10 常用命令速查

```bash
# Claude Code
claude                              # 启动
claude -q "..."                     # 单次问答
claude --resume <session_id>        # 恢复会话
claude /clear                       # 清屏新会话
claude /compact                     # 压缩上下文
claude /help                        # 帮助

# Codex CLI
codex                               # 启动
codex -q "..."                      # 单次问答
codex --model gpt-5.5               # 换模型
codex login                         # 登录
codex --approval-mode full-auto     # 全自动批准

# Hermes
hermes                              # 启动
hermes chat -q "..."                # 单次
hermes setup                        # 配置
hermes model                        # 换模型
hermes doctor                       # 健康检查
hermes skills list                  # 列出 skill
hermes mcp list                     # 列出 MCP
hermes cron list                    # 列出定时任务

# rtk
rtk git status                      # 压缩 git status
rtk ls                              # 压缩 ls
rtk find <pattern>                  # 压缩 find
```

---

## A.11 学完验收

- [ ] 能讲清「模型 × Agent × 指令」三者关系
- [ ] 装好 Claude Code + Codex + 4 件套
- [ ] 跑通 9 步闭环一次(产出 9 个文件)
- [ ] 提交 demo PR
- [ ] 提交一份「我的工具选型表」

---

## A.12 一天能学会的事 vs 一周能学会的事

| 一天 | 一周 |
|---|---|
| 装好工具 | 跑通 9 步闭环 |
| 用 superpowers 跑 TDD | 装 ECC + oh-my-claudecode |
| 用 rtk 省 token | 给 Hermes 加自定义 tool |
| 用 codebase-memory-mcp 查代码 | 读 Anthropic 创业手册 |
| 用 agent-browser 截图 | 帮同事排错 |

---

## A.13 找谁帮忙

- **官方文档**:Claude / Codex / Hermes / MCP / agentskills.io
- **群里**:飞书 / Lark AI Coding 培训群
- **讲师**:刘天平
- **内部 issue**:GitLab `ai-coding-training` 仓

---

**版本**:v2.0 · 2026-07-01
**维护**:研发部 AI 培训小组
**更新说明**:v1 → v2 全面更新模型版本(Claude Sonnet 5/Opus 4.8、GPT-5.5、Gemini 3.5、DeepSeek V4、GLM 5.2 等),新增 Base44/v0/Lovable/Bolt.new/Kiro/Jules/Antigravity 等 7 个新 Agent,补「不同角色选 Agent」速查表,标注已退役模型。