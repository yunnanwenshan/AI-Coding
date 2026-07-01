# 模块 3:Skill 工具选型

---

## 3.1 一句话选型规则

> 装 superpowers + rtk + codebase-memory-mcp + agent-browser 四个,其他按需。

---

## 3.2 工具栈总览

```
AI Coding 工具栈(给 50 人的最小可用集)
═══════════════════════════════════════════════
方法论   superpowers           (TDD 强制 / review 自动化)
压缩     rtk                  (token 砍 60-90%)
索引     codebase-memory-mcp  (代码库知识图谱)
浏览器   agent-browser        (E2E / 抓网页)

选装:
- oh-my-claudecode     (5 人以上团队才需要)
- everything-claude-code (想偷懒直接拿 163K 人的配置)
- pm-skills            (产品经理)
- mcp-codegraph        (架构师 / 负责重构的人)
- playwright-mcp       (微软官方,功能更全但更重)
```

---

## 3.3 4 件套详细说明

### 3.3.1 superpowers(方法论)

| 维度 | 内容 |
|---|---|
| 项目 | https://github.com/obra/superpowers |
| Stars | 主流 |
| 定位 | 给 Claude Code 装一套「软件工程方法论」skill 框架 |
| 核心能力 | TDD 强制、红绿循环、code review 自动化、规划 → 实现 → 验证 |
| 安装 | `git clone https://github.com/obra/superpowers.git ~/.claude/skills/superpowers` |
| 适用 | 所有人 |

**用了之后 agent 会自动**:
1. 接到任务先写测试(红)
2. 实现代码到测试过(绿)
3. 重构(REFACTOR)
4. 自动 review
5. 用 TodoWrite 跟踪进度

**典型用法**:

```bash
claude -q "用 superpowers 重构 auth 模块"
# Claude 会自动调用 superpowers 的 TDD skill
```

### 3.3.2 rtk(Rust Token Killer)

| 维度 | 内容 |
|---|---|
| 项目 | https://github.com/rtk-ai/rtk |
| 官网 | https://www.rtk-ai.app/ |
| 定位 | 包一层 shell,把命令输出压缩 60-90% |
| 核心能力 | 透明代理所有 shell 命令,改写输出 |
| 安装 | `curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/main/install.sh \| bash` |
| 适用 | 所有人 |

**对比效果**:

```bash
# 原始 git status
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   src/auth/login.go
        modified:   src/auth/middleware.go

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
# ... 500 token

# rtk git status
[modified] src/auth/login.go
[modified] src/auth/middleware.go
[untracked] README.md
# ... 30 token
```

**省 token 收益**:
- 50 人 × 每天 200 次 shell × 平均 200 token 节省
- 每天节省 ≈ 2,000,000 token(约 $6-20)

### 3.3.3 codebase-memory-mcp

| 维度 | 内容 |
|---|---|
| 项目 | https://github.com/DeusData/codebase-memory-mcp |
| 定位 | 把代码库建成知识图谱,agent 可以语义查询 |
| 核心能力 | 不靠 grep,直接问"Auth 模块在哪" |
| 安装 | 见模块 2 |
| 适用 | 重构、跨文件改动多的人 |

**vs grep 的对比**:

```bash
# 传统 grep
$ rg "auth" --type go
src/auth/login.go:42:func Login(user User) ...
src/auth/middleware.go:18:func Middleware(next http.Handler) ...
# 需要自己判断哪个是 Auth 入口

# codebase-memory-mcp
$ claude -q "Auth 模块的入口在哪里?它依赖哪些模块?"
# Agent 直接回答:"入口是 src/auth/login.go 的 Login 函数,
# 依赖 session(go-session)和 redis cache 两个模块"
```

**适用项目规模**:
- 代码量 > 5 万行
- 跨文件改动频繁
- 新人 onboarding 慢

### 3.3.4 agent-browser

| 维度 | 内容 |
|---|---|
| 定位 | Rust 编写的 headless 浏览器自动化 CLI |
| 核心能力 | navigate / click / type / screenshot |
| 安装 | `brew install agent-browser`(或 cargo install) |
| 适用 | 做 E2E 测试、抓网页数据的人 |

**vs playwright-mcp**:

| 维度 | agent-browser | playwright-mcp |
|---|---|---|
| 语言 | Rust | TS |
| 大小 | 小 | 重 |
| 启动速度 | 快 | 慢 |
| 功能 | 够用 | 更全 |
| 微软支持 | 否 | 是 |

**小团队选 agent-browser,大型 E2E 项目选 playwright-mcp**。

---

## 3.4 选装工具说明

### 3.4.1 oh-my-claudecode(团队多 agent 编排)

| 维度 | 内容 |
|---|---|
| 项目 | https://github.com/Yeachan-Heo/oh-my-claudecode |
| 定位 | 多 agent 编排,主会话派活给子 agent |
| 核心能力 | 子委派、上下文隔离、ultrawork 模式 |
| 适用 | 5 人以上团队 |
| 不适用 | 1-2 人小组 |

**和 superpowers 的区别**:
- superpowers:方法论 skill
- oh-my-claudecode:多 agent 编排框架

可以一起用,层次不同。

### 3.4.2 everything-claude-code(ECC)

| 维度 | 内容 |
|---|---|
| 项目 | https://github.com/affaan-m/everything-claude-code |
| Stars | 163K |
| 定位 | 一站式 plugin 集合,内含 48 个 sub-agent + 135 个 skill |
| 适用 | 想偷懒、不想自己选的人 |
| 风险 | 装得多,不一定都用得上,认知负担大 |

**建议**:先装 4 件套,跑 2 周再加 ECC。

### 3.4.3 pm-skills

| 维度 | 内容 |
|---|---|
| 定位 | 产品经理的 skill 集合 |
| 核心能力 | PRDs、用户故事、优先级排序 |
| 适用 | 产品 / PM |

**不适用**:研发(研发有 superpowers 就够了)。

### 3.4.4 mcp-codegraph

| 维度 | 内容 |
|---|---|
| 定位 | 把代码结构建成图,显示调用关系 |
| 适用 | 架构师、负责人 |
| 不适用 | 日常开发者(用 codebase-memory-mcp 就够) |

---

## 3.5 选型矩阵(给团队负责人)

| 团队规模 | 推荐配置 |
|---|---|
| 个人 | Claude Code + superpowers |
| 5 人小组 | + rtk + codebase-memory-mcp |
| 20 人团队 | + ECC + oh-my-claudecode |
| 跨部门 50+ | 上面全套 + 内网 skill registry + agent-browser |

---

## 3.6 详细操作:本节怎么练

### 练习 1:装 4 件套并跑通

```bash
# 1. 装 superpowers
git clone https://github.com/obra/superpowers.git ~/.claude/skills/superpowers

# 2. 装 rtk
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/main/install.sh | bash
eval "$(rtk init zsh)"
source ~/.zshrc

# 3. 装 codebase-memory-mcp
# 见模块 2.3.3

# 4. 装 agent-browser
brew install agent-browser
```

### 练习 2:对比 rtk 效果

```bash
# 找一个有改动的 git repo
cd ~/your-project

# 原始
git status

# 压缩
rtk git status

# 看 token 节省
echo "原始 token 数 vs rtk token 数"
```

### 练习 3:用 superpowers 跑 TDD

```bash
mkdir -p ~/tdd-demo && cd ~/tdd-demo
git init
claude -q "用 superpowers 写一个计算斐波那契数列第 N 项的函数"
# 观察 Claude 是否:
# 1. 先写测试
# 2. 实现代码
# 3. 跑测试
# 4. 重构
```

### 练习 4:用 codebase-memory-mcp 查代码

```bash
# 准备一个有 100+ 文件的项目
cd ~/your-big-project

# 启动 Claude
claude

# 问
> Auth 模块的入口在哪?它依赖哪些模块?
> 数据库连接是怎么管理的?
> 这个项目里有多少个 API endpoint?
```

---

## 3.7 验收标准

每个学员必须:

- [ ] 装好 4 件套
- [ ] 跑通 rtk 对比 demo
- [ ] 跑通 superpowers TDD demo
- [ ] 跑通 codebase-memory-mcp 一次查询
- [ ] 跑通 agent-browser 一次 screenshot

提交「我的工具选型表」:

```
| 我装了什么 | 装它的理由 | 跑通的截图 |
|---|---|---|
| Claude Code | 主力 agent | <截图> |
| superpowers | 强制 TDD | <截图> |
| rtk | 省 token | <截图> |
| ... | ... | ... |
```

---

## 3.8 现场讲稿(讲师参考,20 分钟)

**第 1 段(5 min)**:画工具栈图,讲"4 件套够用"。

**第 2 段(8 min)**:挑 2 个重点讲:
- superpowers(TDD 强制,review 自动化)
- rtk(token 压缩 60-90%,公司能省的钱)

**第 3 段(5 min)**:讲选装工具的边界,什么时候加 ECC,什么时候加 oh-my-claudecode。

**第 4 段(2 min)**:讲「**多一个就是噪音**」——不要无脑装。

---

## 3.9 常见问题 FAQ

**Q1:为什么不直接装 ECC(163K stars)?**
A:ECC 装得多,不一定都用得上。先装 4 件套跑 2 周,再加 ECC 才有感觉。

**Q2:agent-browser 够用吗?还是必须 playwright-mcp?**
A:小项目 agent-browser 够。要做复杂的 E2E(多 tab、网络拦截)用 playwright-mcp。

**Q3:superpowers 和 oh-my-claudecode 冲突吗?**
A:不冲突。一个是方法论(skill 层),一个是多 agent 编排(框架层)。

**Q4:装这些会拖慢 Claude Code 吗?**
A:会有一点,首次加载多 1-2 秒。skill 加载完后缓存在内存里。

**Q5:rtk 会不会破坏某些命令的输出?**
A:有风险。建议只对输出冗长的命令用(rtk git status, rtk ls, rtk find),关键命令直接走原生命令。

---

## 3.10 公司内网 Skill 仓库建议

```bash
# 在 GitLab 建一个 skills 镜像仓
# 推 superpowers / rtk / codebase-memory-mcp / agent-browser

# 学员统一从这个仓拉
git clone https://git.internal.company.com/ai-skills/superpowers.git ~/.claude/skills/superpowers

# 内部增量 skill 也放这里
# 例如公司内部的 code review checklist skill
```

**好处**:
- 速度快(内网)
- 可控(能改)
- 可审计(能看)