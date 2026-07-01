# 模块 7:Agent 开发(进阶选修)

---

## 7.1 谁需要学这个

- 想给公司写内部 agent(自动化日报 / 自动 code review)
- 想 fork 一个 agent 改造成自己用的
- 想理解 Claude Code / Codex 内部原理
- 想做 AI Native 创业

**不适用**:纯业务开发者 → 学会用即可,不必学开发。

---

## 7.2 七个开源参考项目

按学习价值 / 难度 / 适合谁分四类。

### 7.2.1 Codex(openai/codex)

| 维度 | 内容 |
|---|---|
| 链接 | https://github.com/openai/codex |
| 语言 | Rust |
| 类型 | 官方 CLI |
| 学习价值 | 看 OpenAI 怎么实现 shell 闭环 |
| 难度 | ★★★★ |

**重点读**:
- `codex-rs/core/src/codex.rs` 主循环
- `codex-rs/core/src/tools/` 工具实现
- `codex-rs/core/src/model_provider.rs` 模型接入

### 7.2.2 Hermes(NousResearch/hermes-agent)

| 维度 | 内容 |
|---|---|
| 链接 | https://github.com/NousResearch/hermes-agent |
| 语言 | Python |
| 类型 | 社区 CLI |
| 学习价值 | 看多 provider / 多平台 / skill 体系怎么设计 |
| 难度 | ★★★ |

**重点读**:
- `run_agent.py` 主循环
- `agent/` prompt builder / 上下文压缩
- `tools/` 工具注册
- `gateway/platforms/` 多平台适配
- `cron/` 调度系统

### 7.2.3 ZeroClaw(zeroclaw-labs/zeroclaw)

| 维度 | 内容 |
|---|---|
| 链接 | https://github.com/zeroclaw-labs/zeroclaw |
| 语言 | Rust |
| 类型 | 本地隐私优先的 agent |
| 学习价值 | 看本地优先 / 多 channel / 隐私保护怎么设计 |
| 难度 | ★★★★ |

**重点读**:
- 主循环
- 多 channel 适配
- 本地存储

### 7.2.4 教材:hello-agents(datawhalechina)

| 维度 | 内容 |
|---|---|
| 链接 | https://github.com/datawhalechina/hello-agents |
| 语言 | Python 教程 |
| 类型 | 中文教材 |
| 学习价值 | 从 0 到 1 写 agent |
| 难度 | ★★ |

**重点读**:
- ReAct 范式
- Plan-and-Execute 范式
- 代码示例

### 7.2.5 Google ADK(Agent Development Kit 全家桶)

| 维度 | 内容 |
|---|---|
| 主站 | https://adk.dev/ |
| 官方文档 | https://google.github.io/adk-docs/ |
| 定位 | Google 官方开源的 agent 开发框架,生产级 |
| 当前主推 | ADK Python(1.x 已发布,2.0 文档在写) |
| 模型 | model-agnostic(Gemini 优化,但兼容其他) |
| 部署 | deployment-agnostic(本地 / 容器 / Cloud Run) |

**多语言支持**(2026 年初 ADK Java / Go 1.0 同时发布,补齐 Python 独占的尴尬):

| 语言 | 仓库 | 当前版本 | 特点 |
|---|---|---|---|
| Python | google/adk-python | 1.x(2.0 文档中) | 主推、文档最全、生态最大 |
| Go | google/adk-go | **2.0(2026-06 发布)** | 云原生、强类型、高并发 |
| TypeScript | google/adk-ts | 1.0 | 前端/Node 生态 |
| Java | google/adk-java | 1.0 | 企业 Java 团队 |
| Kotlin | google/adk-kotlin | 1.0 | Android / KMP |

**ADK Go 2.0 关键特性**(2026-06 刚发布):
- 把 agent workflow 表达成**图**(node + edge)
- 原生支持分支、重试、人工审批、并发 fan-out
- 替代"ad-hoc 控制流"(写一堆 if-else + for 循环)

### 7.2.6 google/adk-go(Google 官方 Go SDK)

| 维度 | 内容 |
|---|---|
| 链接 | https://github.com/google/adk-go |
| 语言 | Go |
| 类型 | Google 官方 |
| 学习价值 | 看清 Google 怎么把 agent workflow 抽象成图 |
| 难度 | ★★★(需要 Go 基础 + agent 概念) |

**适用场景**:
- 公司技术栈是 Go(微服务、K8s、云原生)
- 需要生产级 agent(不是 demo)
- 想要 Google 同款架构但不想被 Gemini 锁死

**重点读**:
- `agent/` 核心抽象
- `graph/` workflow as graph
- `tools/` 工具注册
- `examples/` 官方示例

### 7.2.7 zavora-ai/adk-rust(社区 ADK-Rust 移植)

| 维度 | 内容 |
|---|---|
| 链接 | https://github.com/zavora-ai/adk-rust |
| 官网 | https://adk-rust.com/ |
| 语言 | Rust |
| 类型 | 社区移植版(对齐 Google ADK 规范) |
| Stars | 41(2026-07,新项目) |
| 学习价值 | 看 Rust 怎么实现 type-safe + high-perf agent |
| 难度 | ★★★★(需要 Rust 基础) |

**核心特性**:
- 生产级 Rust 实现
- 模块化架构(models / tools / memory / realtime voice)
- 模型无关、类型安全、极致性能
- 模仿 Google ADK 设计,API 类似

**适用场景**:
- 公司技术栈是 Rust(系统编程、嵌入式、WebAssembly)
- 性能/内存是硬约束
- 想要 Python/Go ADK 的设计但语言必须是 Rust

**重点读**:
- `crates/adk-core/` 核心抽象
- `crates/adk-graph/` workflow 图
- `crates/adk-tool/` 工具
- `examples/` 示例

### 7.2.8 五个 ADK 怎么选

| 你的情况 | 推荐 |
|---|---|
| Python 生态 / 快速原型 / 学习 | **ADK Python**(google/adk-python) |
| Go 生态 / 云原生 / 生产级 | **ADK Go 2.0**(google/adk-go) |
| 前端 / Node 生态 | **ADK TypeScript**(google/adk-ts) |
| Java 企业 | **ADK Java**(google/adk-java) |
| Rust / 性能敏感 | **ADK-Rust**(zavora-ai/adk-rust,社区移植) |

**注意**:
- google/* 四个是 Google 官方,文档齐全、长期支持
- zavora-ai/* 是社区维护,新项目(41 stars),生产用需要评估

### 7.2.9 ADK vs Hermes / Codex 的定位区别

| 维度 | ADK(全家桶) | Hermes | Codex |
|---|---|---|---|
| 定位 | **开发框架**(给你写 agent 的 SDK) | **成品 agent CLI**(开箱即用) | **成品 agent CLI** |
| 形态 | 库 / SDK | CLI / 网关 | CLI |
| 上手成本 | 高(要先学 ADK 概念) | 中 | 低 |
| 适合谁 | 想自研 agent 的团队 | 想立刻用的人 | 日常编码主力 |
| 模型 | 任意 | 任意 | 主要 OpenAI |

**类比**:
- ADK = React(给你组件库)
- Hermes / Codex = Chrome(成品浏览器)

你是**用浏览器**(直接用 Hermes/Codex),还是**写浏览器**(用 ADK)?

### 7.2.10 七个开源参考项目总览

| 类别 | 项目 | 语言 | 适合谁 |
|---|---|---|---|
| 成品 CLI | openai/codex | Rust | 看 OpenAI 怎么实现 shell 闭环 |
| 成品 CLI | NousResearch/hermes-agent | Python | 看多 provider / 多平台怎么设计 |
| 成品 CLI | zeroclaw-labs/zeroclaw | Rust | 看本地隐私优先怎么设计 |
| 教材 | datawhalechina/hello-agents | Python 教程 | 从 0 到 1 写 agent |
| 开发框架 | google/adk-python | Python | Python 团队、生产级 |
| 开发框架 | google/adk-go | Go | **Go 团队首选**(2026-06 2.0) |
| 开发框架 | google/adk-ts / java / kotlin | 各 | 对应技术栈 |
| 开发框架 | zavora-ai/adk-rust | Rust | Rust 团队、性能敏感(社区移植) |

**记忆口诀**:
- "**用现成**"看前三个(成品 CLI)
- "**写自己的**"看后四个(开发框架)
- hello-agents 是教材,从 0 开始看

---

## 7.3 Agent 最小骨架

```
┌─────────────────────────────────────────┐
│  LLM(模型)                              │
│  + 消息历史 + 上下文压缩                 │
│  + Tool 调用循环                        │
│  + Skill / Prompt 加载                  │
│  + MCP 客户端                           │
│  + 凭据池 + 多 Provider 切换            │
└─────────────────────────────────────────┘
       │             │
       ▼             ▼
   Tool/Shell    Memory/Session
```

---

## 7.4 一个最小 Agent 的 Python 实现(教学用)

```python
# minimal_agent.py
import anthropic
import json

client = anthropic.Anthropic()
messages = []
tools = []

def chat(user_input):
    messages.append({"role": "user", "content": user_input})
    
    while True:
        response = client.messages.create(
            model="claude-sonnet-4-20250514",
            messages=messages,
            tools=tools,
            max_tokens=4096,
        )
        
        # 工具调用循环
        if response.stop_reason == "tool_use":
            for block in response.content:
                if block.type == "tool_use":
                    tool_name = block.name
                    tool_input = block.input
                    # 调用工具(此处简化)
                    tool_result = {"result": f"call {tool_name}({tool_input})"}
                    messages.append({
                        "role": "assistant",
                        "content": response.content,
                    })
                    messages.append({
                        "role": "user",
                        "content": [{
                            "type": "tool_result",
                            "tool_use_id": block.id,
                            "content": json.dumps(tool_result),
                        }],
                    })
            continue
        
        # 普通回复
        assistant_text = ""
        for block in response.content:
            if hasattr(block, "text"):
                assistant_text += block.text
        messages.append({
            "role": "assistant",
            "content": assistant_text,
        })
        return assistant_text

if __name__ == "__main__":
    print(chat("你好"))
```

**讲解**:
- 这是一个**最小骨架**,50 行
- 真实 agent(Hermes/Codex)是这个的 1000 倍

---

## 7.5 给 Hermes 加一个自定义 Tool(详细操作)

### Step 1:装 Hermes

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
hermes setup
```

### Step 2:加 tool

在 `~/.hermes/hermes-agent/tools/` 下创建一个文件:

```python
# tools/wiki_search.py
import os
import requests
from tools.registry import registry

def check_requirements() -> bool:
    return bool(os.getenv("WIKI_API_URL"))

def wiki_search(query: str, limit: int = 5) -> str:
    """Search internal wiki for documents matching query.
    
    Args:
        query: Search query string
        limit: Max results to return (default 5)
    """
    try:
        resp = requests.get(
            f"{os.getenv('WIKI_API_URL')}/search",
            params={"q": query, "limit": limit},
            headers={"Authorization": f"Bearer {os.getenv('WIKI_TOKEN')}"},
            timeout=10,
        )
        resp.raise_for_status()
        return json.dumps({"success": True, "data": resp.json()})
    except Exception as e:
        return json.dumps({"success": False, "error": str(e)})

registry.register(
    name="wiki_search",
    toolset="wiki",
    schema={
        "name": "wiki_search",
        "description": "Search internal wiki for documents matching query",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {"type": "string", "description": "Search query"},
                "limit": {"type": "integer", "description": "Max results", "default": 5},
            },
            "required": ["query"],
        },
    },
    handler=lambda args, **kw: wiki_search(
        query=args.get("query", ""),
        limit=args.get("limit", 5),
    ),
    check_fn=check_requirements,
    requires_env=["WIKI_API_URL", "WIKI_TOKEN"],
)
```

### Step 3:配置环境变量

```bash
# ~/.hermes/.env
WIKI_API_URL=https://wiki.internal.company.com/api
WIKI_TOKEN=***
```

### Step 4:重启 Hermes,跑通

```bash
hermes chat -q "搜一下 wiki 里关于'周报系统'的文档"
# Agent 会调用 wiki_search 工具
```

---

## 7.6 给 Codex 加自定义命令(简化版)

Codex CLI 不像 Hermes 那么容易扩展,但可以**通过配置文件**做一些定制:

```toml
# ~/.codex/config.toml
[model]
default = "gpt-5"

[sandbox]
mode = "workspace-write"

[custom_commands]
deploy = "bash scripts/deploy.sh"
test = "npm test"
```

然后:

```bash
codex --custom deploy
codex --custom test
```

---

## 7.7 ReAct 范式 vs Plan-and-Execute

### ReAct(Reasoning + Acting)

```
Thought 1: 我需要查 user 表
Action 1: db.query("SELECT * FROM users WHERE id = 1")
Observation 1: [{id:1, name:"张三"}]
Thought 2: 我现在知道名字了,回复用户
Action 2: Final Answer("用户叫张三")
```

**适用**:简单任务、对话式 agent

### Plan-and-Execute

```
Plan:
1. 查 user 表
2. 查 user 的 order 表
3. 汇总返回

Execute:
- Step 1: db.query(...)
- Step 2: db.query(...)
- Step 3: 汇总

Reflect:
- 步骤 1 成功
- 步骤 2 失败,重试
- 步骤 3 成功
```

**适用**:复杂任务、多步骤、需要反思

### 选哪个

| 场景 | 推荐 |
|---|---|
| 单步骤问答 | ReAct |
| 多步骤调研 | Plan-and-Execute |
| 需要回滚 | Plan-and-Execute |
| 流式对话 | ReAct |
| Code generation | Plan-and-Execute |

---

## 7.8 详细操作:本节怎么练

### 练习 1:跑通 hello-agents 教程

```bash
git clone https://github.com/datawhalechina/hello-agents.git
cd hello-agents
# 跟着 README 跑
```

### 练习 2:读 Hermes 主循环

```bash
git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
cat run_agent.py | head -100
# 理解 agent loop
```

### 练习 3:给 Hermes 加一个 wiki tool

按 7.5 的步骤做。

### 练习 4:写一个最小 agent

按 7.4 的代码,把它改成你公司内部能用的工具。

### 练习 5:跑通 ADK Go 示例(2026-07 新增)

```bash
# 前置:Go 1.22+
go version

# 1. 克隆
git clone https://github.com/google/adk-go.git
cd adk-go

# 2. 跑官方示例
cd examples/quickstart
go mod tidy
go run main.go

# 3. 观察
# - agent 是怎么定义的
# - workflow 是怎么表达成图的
# - tool 是怎么注册的
```

### 练习 6:跑通 ADK-Rust 示例(2026-07 新增,选做)

```bash
# 前置:Rust 1.75+
rustc --version

# 1. 克隆
git clone https://github.com/zavora-ai/adk-rust.git
cd adk-rust

# 2. 跑 examples
cargo run --example simple_agent

# 3. 对比 Google ADK 的设计
# 看 Rust 版本有什么不同(类型约束、async、性能)
```

---

## 7.9 验收标准

基础(必做):
- [ ] 跑通 hello-agents 教程
- [ ] 读懂 Hermes 主循环
- [ ] 成功给 Hermes 加一个自定义 tool 并跑通
- [ ] 写一个 200 行的最小 agent

进阶(选做):
- [ ] 跑通 google/adk-go 的 quickstart 示例
- [ ] 跑通 zavora-ai/adk-rust 的 simple_agent 示例
- [ ] 能讲清 ADK Python / Go / Rust 三者的适用场景

---

## 7.10 现场讲稿(讲师参考,4h)

**第 1 段(30 min)**:讲 Agent 最小骨架。
**第 2 段(60 min)**:过三个开源项目。
**第 3 段(90 min)**:演示加自定义 tool。
**第 4 段(60 min)**:学员实练 + 答疑。

---

## 7.11 常见问题 FAQ

**Q1:学 Agent 开发要先学 Rust 吗?**
A:不用。先学 Python(Hermes)。Codex 的 Rust 是给性能优化用的,不是入门必学。

**Q2:我自己写的 agent 能商用吗?**
A:取决于底层模型。Hermes 用 Claude/GPT → 受 API ToS 约束。自己用开源模型(Qwen/DeepSeek)→ 自由度高。

**Q3:agent 和工作流(workflow)有什么区别?**
A:工作流是预先定义步骤(像 n8n / Airflow);agent 是动态决策(根据上下文决定下一步)。**agent 更灵活,工作流更可控**。

**Q4:什么时候该自己写 agent,什么时候用现成的?**
A:**能用现成就用现成**。自己写只在你有特殊需求时。

**Q5:agent 开发是不是 AI 时代最稀缺技能?**
A:**是之一**。但"会用 agent"比"会写 agent"更重要。50 人里 5 人会写就够,其余 45 人会用就行。

**Q6:Google ADK 跟 LangChain / LlamaIndex 是一回事吗?(2026-07 新增)**
A:不是。LangChain / LlamaIndex 是**Python 库**,ADK 是**Google 官方多语言框架**。ADK 更适合生产级、多 agent 编排、需要长期支持的企业项目;LangChain 更适合快速实验、RAG 场景。

**Q7:ADK Go 2.0 的"workflow as graph"是什么意思?(2026-07 新增)**
A:把 agent 的工作流(分类 → 分支 → 重试 → fan-out → 合并)表达成**图结构**(node + edge),而不是写一堆 if-else + for 循环。好处:可视化、可调试、易扩展。坏处:简单任务反而繁琐。

**Q8:adk-rust 是 Google 官方的吗?(2026-07 新增)**
A:**不是**。zavora-ai 是社区组织,adk-rust 是社区移植版,模仿 Google ADK 的设计但用 Rust 重写。Google 官方目前只有 Python / Go / TypeScript / Java / Kotlin 五个语言版本,没有 Rust。

**Q9:我们应该用 ADK 还是 Hermes?**
A:看需求。
- **用现成**:直接 Hermes / Codex,不要重新发明轮子
- **自己写 agent**:有 Python 团队用 ADK Python,有 Go 团队用 ADK Go,有 Rust 团队用 ADK-Rust
- **生产级 + 长期**:ADK(Goolge 官方支持)
- **内部小工具**:Hermes 够用

**Q10:公司 Go 技术栈,选什么?**
A:**google/adk-go 2.0**(2026-06 发布)。Google 官方维护 + Go 生态原生 + workflow as graph。云原生首选。