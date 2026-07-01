# 模块 2:环境安装

---

## 2.1 完整安装清单

### 2.1.1 基础环境(Mac)

```bash
# 1. Homebrew(没装的话)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 2. Node.js 22+(用 nvm)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh)"
nvm install 22
nvm use 22
nvm alias default 22

# 3. Python 3.12(用 uv,比 pyenv 简单)
curl -LsSf https://astral.sh/uv/install.sh | sh
uv python install 3.12

# 4. 常用工具
brew install ripgrep fd bat jq git gh

# 5. 验证
node --version    # v22.x
python3 --version # 3.12.x
rg --version      # ripgrep 14.x
gh --version      # 2.x
```

### 2.1.2 Claude Code

```bash
# 1. 全局安装
npm install -g @anthropic-ai/claude-code

# 2. 验证
claude --version

# 3. 登录(跳浏览器)
claude
# 按提示登录 Anthropic 账号

# 4. 第一次跑通
mkdir -p ~/ai-coding-demo
cd ~/ai-coding-demo
echo 'console.log("hello")' > index.js
claude -q "解释当前目录 index.js 的功能"
```

### 2.1.3 Codex CLI

```bash
# 1. 全局安装
npm install -g @openai/codex

# 2. 验证
codex --version

# 3. 登录(跳浏览器,用 ChatGPT 账号)
codex
# 或 codex login

# 4. 第一次跑通
cd ~/ai-coding-demo
codex -q "解释当前目录 index.js 的功能"
```

### 2.1.4 Hermes(选装,推荐 5 个骨干先试)

```bash
# 1. 一键安装
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# 2. 验证
hermes --version

# 3. 配置模型(provider 任选)
hermes setup
# 或手动:
hermes model  # 交互式选模型

# 4. 第一次跑通
hermes chat -q "解释当前目录 index.js 的功能"

# 5. 健康检查
hermes doctor
```

### 2.1.5 GitHub CLI

```bash
brew install gh
gh auth login
# 选 HTTPS,选浏览器登录
```

---

## 2.2 官方文档(收藏)

| 工具 | 文档链接 |
|---|---|
| Claude Code | https://docs.claude.com/en/docs/claude-code |
| Codex CLI | https://github.com/openai/codex/blob/main/README.md |
| Hermes | https://hermes-agent.nousresearch.com/docs |
| Agentskills 规范 | https://agentskills.io/home |
| MCP 协议 | https://modelcontextprotocol.io |
| GitHub CLI | https://cli.github.com/manual/ |

---

## 2.3 Skill 工具安装(模块 3 提前准备)

### 2.3.1 superpowers

```bash
# Claude Code 安装 skill
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/obra/superpowers.git

# 重启 claude 后会自动加载
claude -q "/skills"   # 列出已加载的 skill
```

### 2.3.2 rtk(Rust Token Killer)

```bash
# macOS 一键安装
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/main/install.sh | bash

# 验证
rtk --version

# 配置为默认 shell wrapper
# 在 ~/.zshrc 末尾加:
eval "$(rtk init zsh)"
source ~/.zshrc

# 对比效果
git status                  # 原始输出
rtk git status              # 压缩后
```

### 2.3.3 codebase-memory-mcp

```bash
# 通过 npm 全局装(参考项目主页,以最新为准)
npm install -g codebase-memory-mcp

# 配置到 Claude Code(~/.claude/mcp_servers.json)
cat > ~/.claude/mcp_servers.json <<'EOF'
{
  "mcpServers": {
    "codebase-memory": {
      "command": "codebase-memory-mcp",
      "args": ["--path", "/Users/yourname/your-project"]
    }
  }
}
EOF

# 重启 Claude Code 后生效
claude -q "/mcp list"
```

### 2.3.4 agent-browser

```bash
# 参考项目主页命令(以最新为准)
# 一般是 cargo install 或 brew install
brew install agent-browser   # 或参照官方 README

# 验证
agent-browser --version

# 第一次跑通
agent-browser navigate "https://example.com"
agent-browser screenshot
```

---

## 2.4 50 人统一环境规范

### 2.4.1 目录结构(每人统一)

```
~/
├── ai-coding-demo/           # 练习项目
├── .claude/                  # Claude Code 配置
│   ├── settings.json
│   ├── mcp_servers.json
│   └── skills/               # 已安装的 skill
├── .codex/                   # Codex 配置
└── .hermes/                  # Hermes 配置(选装)
```

### 2.4.2 settings.json 模板(Claude Code)

```json
{
  "model": "claude-sonnet-4",
  "theme": "dark",
  "verbose": false,
  "permissions": {
    "allow": [
      "Bash(git:*)",
      "Bash(npm:*)",
      "Bash(make:*)"
    ]
  }
}
```

### 2.4.3 .env 模板

```bash
# Claude
ANTHROPIC_API_KEY=sk-ant-xxx

# OpenAI(Codex 用)
OPENAI_API_KEY=sk-xxx

# Hermes(选装,可指向任意 provider)
OPENROUTER_API_KEY=sk-or-xxx
```

---

## 2.5 常见安装问题与解决方案

### 问题 1:Node 版本不够

```bash
# 报错:Node.js 18+ required
nvm install 22
nvm use 22
```

### 问题 2:Claude 登录后掉线

```bash
# 删除凭据重新登录
rm -rf ~/.claude/auth.json
claude
```

### 问题 3:Codex 提示 "No model selected"

```bash
codex login
# 在 ChatGPT 后台开 API 权限
```

### 问题 4:rtk 装好但没生效

```bash
# 检查 PATH
which rtk
# 应该是 ~/.cargo/bin/rtk 或 /usr/local/bin/rtk

# 检查 zsh wrapper
echo $PATH | tr ':' '\n' | head
# 确保 rtk 在 PATH 里

# 重新 source
source ~/.zshrc
```

### 问题 5:MCP 启动失败

```bash
# 看日志
claude --debug
# 或
cat ~/.claude/logs/*.log | tail -50
```

---

## 2.6 验收清单

每个学员必须完成:

- [ ] `node --version` 输出 ≥ 22
- [ ] `python3 --version` 输出 ≥ 3.12
- [ ] `claude --version` 输出正常
- [ ] `codex --version` 输出正常
- [ ] `claude -q "..."` 能跑通
- [ ] `codex -q "..."` 能跑通
- [ ] 在群里贴 `claude --version` 和 `codex --version` 输出截图

---

## 2.7 公司内网部署建议(给 IT)

### 2.7.1 npm 镜像

```bash
# 内部 npm registry
npm config set registry https://npm.internal.company.com

# 验证
npm view @anthropic-ai/claude-code version
```

### 2.7.2 API 凭据池

50 人共用 1 个 API key 池容易触发风控。建议:

- 每 5-10 人一组,1 个池
- 内部用 hermes/opencode 等支持凭据池的 agent
- 限额(每人每天 N 次)

### 2.7.3 Skill 内网仓库

```bash
# 在公司 GitLab 建一个 skill 镜像仓
git clone https://github.com/obra/superpowers.git
# 推到内网
git remote set-url origin https://git.internal.company.com/skills/superpowers.git
git push -u origin main
```

---

## 2.8 安装时间估算

| 步骤 | 时间 | 难度 |
|---|---|---|
| 基础环境 | 20 min | 低 |
| Claude Code | 5 min | 低 |
| Codex | 5 min | 低 |
| Hermes(选装) | 15 min | 中 |
| Skill 4 件套 | 30 min | 中 |
| 内网配置 | 15 min | 中 |
| **合计** | **90 min** | - |

建议培训前 1 天发安装指引,培训当天只验证,不现场装。