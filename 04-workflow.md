# 模块 4:AI Coding 9 步闭环

> 现场讲 30 分钟。核心:流程图 + 3 个最常踩的坑。

---

## 4.1 9 步流程图

```
① 需求澄清 ────► ② 方案设计 ────► ③ 任务规划
    │                 │                 │
    ▼                 ▼                 ▼
user_story.md    design.md        tasks.md
    │                                 │
    ▼                                 ▼
⑨ 接口文档 ◄──── ⑧ 端到端测试 ◄── ⑦ HTTP API 测试
    │                 │                 │
    └────────────────►│◄────────────────┘
                      │
                      ▼
              ⑥ 集成测试 ─────► ⑤ Code Review
                                  │
                                  ▼
                              ④ TDD 开发
                            (RED→GREEN→REFACTOR)
```

---

## 4.2 每步对应工具与产物

| 步骤 | 工具 | 关键动作 | 产物 |
|---|---|---|---|
| ① 需求澄清 | pm-skills + 主 agent | 5W1H + 范围锁定 | user_story.md |
| ② 方案设计 | 主 agent + mcp-codegraph | architecture + 接口 | design.md |
| ③ 任务规划 | sub-agent | 拆任务、写验收 | tasks.md |
| ④ TDD 开发 | superpowers + 子 agent | 先写测试再写代码 | test + impl |
| ⑤ Code Review | ECC review agent / 人工 | 自动 review + 修复 | reviewed diff |
| ⑥ 集成测试 | 测试 agent | 多模块联调 | integration_test.log |
| ⑦ HTTP API 测试 | agent-browser / playwright-mcp | 跑接口 | api_test.log |
| ⑧ 端到端测试 | playwright-mcp | 走业务路径 | e2e_report.md |
| ⑨ 接口文档 | 主 agent | 自动生成 | openapi.yaml |

---

## 4.3 每一步的详细做法

### 4.3.1 需求澄清(人 + agent)

**目标**:把"做什么"说清楚,**不**说"怎么做"。

**模板**:

```markdown
# user_story.md

## 用户故事
作为 [角色],我想 [动作],以便 [价值]。

## 范围(必须做)
1. 用户能用手机号注册
2. 注册时收短信验证码
3. 验证码 5 分钟内有效

## 范围(不做)
1. 不做邮箱注册
2. 不做第三方登录
3. 不做找回密码

## 验收标准
- [ ] 输入合法手机号,能收到验证码
- [ ] 验证码错误时,提示"验证码错误,请重试"
- [ ] 5 分钟后验证码失效

## 边界
- 短信服务商:阿里云(待选)
- 频率限制:每分钟 1 条
- 国际化:仅中文
```

**坑**:不写"不做" → agent 会自己加东西。

### 4.3.2 方案设计(人主导,agent 辅助)

**目标**:确定架构 + 接口,**不**写代码。

**模板**:

```markdown
# design.md

## 架构
[客户端] → [API 网关] → [Auth Service] → [User Service] → [MySQL]
                                  ↓
                              [Redis 缓存]

## 模块划分
- Auth Service:负责登录、注册、token 管理
- User Service:负责用户信息 CRUD
- 短信服务:由 Auth Service 调用阿里云 SDK

## 接口
POST /api/auth/register
POST /api/auth/login
POST /api/auth/verify-sms

## 数据模型
users 表(id, phone, password_hash, created_at)
sms_codes 表(phone, code, expires_at)

## 技术选型
- 语言:Go
- 框架:Gin
- DB:MySQL 8
- 缓存:Redis 7
```

**坑**:让 agent 全权设计 → 方案不切实际,要么过度设计。

### 4.3.3 任务规划(agent 为主,人 review)

**目标**:把方案拆成可执行任务,每个任务都能在 1-2 小时内完成。

**模板**:

```markdown
# tasks.md

## 任务列表
- [ ] T1:初始化 Go 项目,配 Gin/MySQL/Redis 连接
- [ ] T2:写 users 表迁移 + Model
- [ ] T3:写注册接口 POST /api/auth/register
- [ ] T4:写阿里云短信发送工具
- [ ] T5:写登录接口 POST /api/auth/login
- [ ] T6:写 JWT token 生成 + 中间件
- [ ] T7:集成测试(注册→登录→访问受保护接口)
- [ ] T8:E2E 测试(用 agent-browser 跑完整流程)

## 依赖关系
T2 → T3 → T4 → T7
T3 → T5 → T6 → T7
T7 → T8
```

**坑**:任务太大 → agent 跑到一半上下文爆掉。**单任务不超过 500 行代码改动**。

### 4.3.4 TDD 开发(superpowers 强制)

**目标**:先写测试,再写实现。

**流程**(superpowers 自动跑):

```
1. RED    写一个失败的测试
2. GREEN  写最小代码让测试过
3. REFACTOR 重构(保持测试过)
4. 循环到所有功能实现
```

**示例**:

```go
// 1. 先写测试(auth_test.go)
func TestRegister(t *testing.T) {
    // 准备
    db := setupTestDB()
    service := NewAuthService(db)
    
    // 执行
    user, err := service.Register("13800138000", "123456")
    
    // 断言
    assert.NoError(t, err)
    assert.NotEmpty(t, user.ID)
}
```

```bash
# 跑测试(应该 fail)
go test ./...
# FAIL: TestRegister - Register not implemented yet
```

```go
// 2. 写实现(auth.go)
func (s *AuthService) Register(phone, password string) (*User, error) {
    user := &User{Phone: phone, PasswordHash: hash(password)}
    return s.db.Create(user)
}
```

```bash
# 跑测试(应该 pass)
go test ./...
# PASS
```

### 4.3.5 Code Review(自动 + 人工)

**自动 review**(ECC agent):

```bash
claude -q "用 everything-claude-code 的 review agent 检查这个 PR"
```

**人工 review 重点**:
- 业务逻辑是否正确
- 边界条件是否覆盖
- 是否引入了不必要的依赖
- 性能是否有明显问题

**坑**:只靠自动 review → 漏业务问题。**自动 review 是助手,人工 review 是底线**。

### 4.3.6 集成测试

**目标**:多个模块一起跑,验证交互。

```bash
# 启动所有服务
docker-compose up -d

# 跑集成测试
go test -tags=integration ./...

# 看覆盖率
go test -tags=integration -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
```

**坑**:集成测试和单测混在一起 → 跑得慢、定位问题难。**单测和集成测试分开 tag**。

### 4.3.7 HTTP API 测试(agent-browser / playwright-mcp)

**目标**:验证 HTTP 接口行为。

```bash
# 用 agent-browser
agent-browser navigate "http://localhost:8080/api/auth/register"
agent-browser type "input[name=phone]" "13800138000"
agent-browser type "input[name=password]" "test123"
agent-browser click "button[type=submit]"
agent-browser screenshot /tmp/register-result.png
```

**或者用 curl + jq**:

```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"phone":"13800138000","password":"test123"}' | jq
```

**坑**:只测 happy path → 出问题全在边界条件。**至少测 3 种情况:正常 / 异常 / 边界**。

### 4.3.8 端到端测试

**目标**:走通完整业务流程。

```bash
# playwright-mcp 录制脚本
# 完整流程:注册 → 收短信 → 验证 → 登录 → 访问个人中心
playwright codegen http://localhost:8080
# 录制成 Playwright 脚本
npx playwright test
```

**坑**:E2E 测试太脆(改 UI 就挂)。**E2E 只测关键路径,不测每个细节**。

### 4.3.9 接口文档(自动生成)

```bash
# 用 swag(Go)
swag init
# 生成 docs/swagger.json

# 或用 OpenAPI Generator
npx @openapitools/openapi-generator-cli generate \
  -i openapi.yaml \
  -g python \
  -o client/
```

**坑**:手写文档 → 一定过期。**从代码生成,代码改文档自动改**。

---

## 4.4 三个最容易踩的坑

### 坑 1:跳过 ① ③ 直接让 agent 写代码

**症状**:
- agent 写到一半跑偏
- 改完发现需求理解错了
- 返工 2-3 倍时间

**对策**:
- ①③ 必须人写或人 review
- 不写 user_story.md 不让 agent 动

### 坑 2:不写测试

**症状**:
- agent 改一处炸三处
- 上线后回归 bug 频出
- 重构不敢动

**对策**:
- superpowers 强制 TDD
- CI 里加 `coverage >= 80%` 的卡口

### 坑 3:不写接口文档 / 文档过期

**症状**:
- 前后端联调扯皮
- 新人看不懂老接口
- 客户端代码和文档不一致

**对策**:
- 文档从代码生成(OpenAPI/swag)
- CI 里加"文档必须和代码同步"的卡口

---

## 4.5 详细操作:本节怎么练

### 练习 1:跑通一个 demo

```bash
mkdir -p ~/workflow-demo && cd ~/workflow-demo
git init

# 步骤 ①
claude -q "用 user_story.md 模板,帮我写一个 todo 列表的需求"
# 输出:user_story.md

# 步骤 ②
claude -q "基于 user_story.md,帮我写 design.md"
# 输出:design.md

# 步骤 ③
claude -q "基于 design.md,拆成 tasks.md"
# 输出:tasks.md

# 步骤 ④
claude -q "用 superpowers 实现 tasks.md 的 T1-T3"
# 输出:测试 + 实现

# 步骤 ⑤
claude -q "review 刚才的改动"
# 输出:review 报告
```

### 练习 2:对照 9 步检查清单

每完成一步,在 README 里打勾:

```
- [x] ① user_story.md
- [x] ② design.md
- [x] ③ tasks.md
- [x] ④ test + impl
- [ ] ⑤ reviewed diff
- [ ] ⑥ integration test
- [ ] ⑦ api test
- [ ] ⑧ e2e test
- [ ] ⑨ openapi.yaml
```

---

## 4.6 验收标准

每个学员必须:

- [ ] 跑通 9 步完整流程一次
- [ ] 产出 9 个对应文件
- [ ] 提交 demo PR(可以是个 todo 列表 / 简单计算器)

---

## 4.7 现场讲稿(讲师参考,30 分钟)

**第 1 段(8 min)**:画流程图。强调 ①②③ 是人写的,④⑤ 用 superpowers 强制。

**第 2 段(10 min)**:讲 3 个坑(每个 3 分钟)。

**第 3 段(8 min)**:演示一个最小例子(todo 列表)。

**第 4 段(4 min)**:讲「不要跳过 ①③」。

---

## 4.8 进阶:9 步不是死顺序

实操中,经常需要回滚:

```
① → ② → ③ → ④ → 发现 ② 假设错了 → 回 ② → 重做 ③ → ...
```

**接受迭代,但不能跳过**:

- ① 不写 → 后面必返工
- ③ 不写 → agent 跑一半爆
- ④⑤ 不用 superpowers → 测试覆盖率上不去
- ⑨ 不写 → 后面的人接不上

---

## 4.9 常见问题 FAQ

**Q1:小改动也要走 9 步吗?**
A:不用。9 步适用于**有完整需求的项目**。单文件改动、改 typo、跑命令 → 直接让 agent 干。

**Q2:agent 写的设计稿能直接用吗?**
A:不能。**方案必须人 review**。agent 给的是候选方案,人是决策者。

**Q3:任务拆多大合适?**
A:单任务 1-2 小时能完成,代码改动 < 500 行。**超了拆细**。

**Q4:E2E 测试要覆盖多少?**
A:只覆盖**关键路径**。把所有功能都写成 E2E 会很脆,改 UI 全挂。

**Q5:文档自动生成会不会不准确?**
A:有延迟(代码改完没重新生成)。**CI 跑生成,把生成的文档 commit 进去**。