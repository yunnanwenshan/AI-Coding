# 模块 5:案例实练

> 现场讲 30 分钟(只演示案例 A 压缩版)。其他案例留给课后。

---

## 5.1 三个案例总览

| 案例 | 主题 | 时长 | 难度 | 现场演示 |
|---|---|---|---|---|
| A | 全栈新项目启动 | 2 天 | ★★ | ✓ |
| B | 项目重构 | 2 天 | ★★★★ | - |
| C | 小 bug 修复 | 半天 | ★ | - |

---

## 案例 A:全栈新项目启动(2 天,现场演示 30 min)

### 题目

**周报提报与统计系统**

员工每周五提交周报,主管可以查看团队所有人的周报,自动汇总成团队周报。

### 技术栈

- 后端:Go + Gin + GORM
- 前端:React + Vite + Ant Design
- DB:SQLite(简化)
- 工具链:Claude Code + superpowers + rtk

---

### Day 1 上午(4h):需求 → 任务规划

#### Step 1:写 user_story.md

```markdown
# user_story.md

## 用户故事
作为公司员工,我想每周五提交本周工作内容和下周计划,以便主管了解我的工作进度。

作为主管,我想查看团队所有成员的周报,以便汇总成团队周报。

## 范围(必须做)
1. 员工提交周报(本周完成、下周计划、问题与求助)
2. 主管查看本团队所有成员周报
3. 自动汇总成团队周报(按人合并)
4. 周报历史可查

## 范围(不做)
1. 不做评论 / 回复功能
2. 不做附件上传
3. 不做邮件提醒
4. 不做权限细分(只区分员工 / 主管)

## 验收标准
- [ ] 员工能创建 / 编辑本周周报
- [ ] 主管能看到本团队所有人的本周周报
- [ ] 主管能一键汇总成团队周报(可导出 Markdown)
- [ ] 周报历史可按周查看

## 边界
- 用户角色:简单区分员工 / 主管(不做完整 RBAC)
- 数据范围:全员可看自己 + 同团队
- 导出格式:Markdown
```

**操作**:

```bash
mkdir -p ~/projects/weekly-report
cd ~/projects/weekly-report
git init
claude -q "用 pm-skills 模板,帮我们写周报系统的 user_story.md"
```

#### Step 2:写 design.md

```markdown
# design.md

## 架构
[React 前端] → [Gin API] → [SQLite]
       │              │
       └──── JWT ──────┘

## 模块
- 用户认证(登录 / 角色)
- 周报 CRUD(创建、编辑、查询)
- 团队汇总(导出 Markdown)

## 数据模型
users(id, name, email, role, team_id, password_hash)
teams(id, name)
weekly_reports(id, user_id, week_start, content, status, created_at, updated_at)

## 接口
POST   /api/auth/login
GET    /api/reports/me?week=2026-W26
POST   /api/reports
PUT    /api/reports/:id
GET    /api/reports/team/:team_id?week=2026-W26
GET    /api/reports/team/:team_id/summary?week=2026-W26

## 技术选型
- 后端:Go 1.22 + Gin + GORM
- 前端:React 18 + Vite + Ant Design
- DB:SQLite 3
- 认证:JWT
```

**操作**:

```bash
claude -q "基于 user_story.md,写 design.md"
# review 后,人工修订
```

#### Step 3:写 tasks.md

```markdown
# tasks.md

## 后端
- [ ] T1:初始化 Go 项目,配 Gin/GORM/SQLite
- [ ] T2:写 users / teams / weekly_reports 三张表 migration
- [ ] T3:写 User Model + Team Model + WeeklyReport Model
- [ ] T4:写登录接口 POST /api/auth/login(JWT)
- [ ] T5:写 JWT 中间件(校验 token + 注入 user)
- [ ] T6:写创建周报 POST /api/reports
- [ ] T7:写编辑周报 PUT /api/reports/:id
- [ ] T8:写查自己周报 GET /api/reports/me
- [ ] T9:写查团队周报 GET /api/reports/team/:team_id
- [ ] T10:写团队汇总接口(返回 Markdown)
- [ ] T11:集成测试(注册→登录→提交周报→汇总)

## 前端
- [ ] F1:初始化 React 项目,配 Vite + Ant Design + axios
- [ ] F2:登录页 + token 保存
- [ ] F3:周报提交页(本周完成 / 下周计划 / 问题与求助)
- [ ] F4:我的周报历史列表
- [ ] F5:团队周报列表(主管看)
- [ ] F6:团队周报汇总(导出 Markdown)

## 联调
- [ ] I1:前后端联调
- [ ] I2:用 agent-browser 跑完整流程 E2E
- [ ] I3:写接口文档(openapi.yaml)
```

**操作**:

```bash
claude -q "基于 design.md,拆成 tasks.md"
# 人工调整任务粒度(单任务 < 500 行代码)
```

---

### Day 1 下午(4h):后端 TDD 开发

#### T1:初始化 Go 项目

```bash
mkdir -p backend && cd backend
go mod init github.com/yourname/weekly-report

# 用 Claude 生成项目结构
claude -q "用 superpowers,初始化一个 Gin + GORM + SQLite 项目骨架"
```

#### T2-T3:数据模型

```bash
claude -q "用 superpowers,写 users / teams / weekly_reports 三张表的 migration + Model,先写测试"
```

**预期产物**:

```
backend/
├── migrations/
│   ├── 001_users.sql
│   ├── 002_teams.sql
│   └── 003_weekly_reports.sql
├── models/
│   ├── user.go        (含测试)
│   ├── team.go        (含测试)
│   └── weekly_report.go (含测试)
└── db/
    └── db.go
```

#### T4-T5:认证

```bash
claude -q "用 superpowers,实现 POST /api/auth/login(JWT) + 中间件,先写测试"
```

#### T6-T10:周报 CRUD

```bash
# 每个任务独立跑
claude -q "用 superpowers,实现 POST /api/reports,先写测试"
claude -q "用 superpowers,实现 PUT /api/reports/:id,先写测试"
claude -q "用 superpowers,实现 GET /api/reports/me,先写测试"
claude -q "用 superpowers,实现 GET /api/reports/team/:team_id,先写测试"
claude -q "用 superpowers,实现 GET /api/reports/team/:team_id/summary,先写测试"
```

#### T11:集成测试

```bash
claude -q "用 superpowers,写集成测试:注册→登录→提交周报→主管汇总"
```

---

### Day 2 上午(4h):前端 + 联调

#### F1:初始化 React 项目

```bash
cd .. && mkdir -p frontend && cd frontend
npm create vite@latest . -- --template react-ts
npm install antd axios react-router-dom
```

#### F2-F6:前端页面

```bash
# 每个页面独立跑
claude -q "用 React + Ant Design,写登录页 + axios 拦截器注入 token"
claude -q "用 React + Ant Design,写周报提交页(三段:本周完成 / 下周计划 / 问题与求助)"
claude -q "用 React + Ant Design,写我的周报历史列表"
claude -q "用 React + Ant Design,写团队周报列表(主管视角)"
claude -q "用 React + Ant Design,写团队周报汇总页 + 导出 Markdown"
```

#### I1:前后端联调

```bash
# 启动后端
cd ../backend && go run main.go &

# 启动前端
cd ../frontend && npm run dev &

# 浏览器打开
open http://localhost:5173
```

---

### Day 2 下午(4h):E2E + 文档

#### I2:E2E 测试

```bash
# 用 agent-browser 录制
agent-browser navigate "http://localhost:5173"
# 手动登录 → 提交周报 → 切换主管 → 查团队周报 → 导出 Markdown
agent-browser screenshot /tmp/e2e-step1.png
# ...
```

或者用 playwright:

```bash
cd ../frontend
npm install -D @playwright/test
npx playwright init
npx playwright codegen http://localhost:5173
# 录制成脚本
npx playwright test
```

#### I3:接口文档

```bash
# 后端用 swag
cd ../backend
go install github.com/swaggo/swag/cmd/swag@latest
swag init

# 启动后访问 /swagger/index.html
```

---

### 现场演示讲稿(30 min)

**录屏 1(8 min)**:从 0 到 tasks.md
- 给 prompt:"做一个周报系统"
- 看 Claude 写出 user_story.md → design.md → tasks.md
- 关键:**人 review,不让 Claude 自己定**

**录屏 2(12 min)**:T1-T5 后端开发
- 看 superpowers 自动跑 TDD
- 看 RED → GREEN → REFACTOR
- 看测试覆盖率

**录屏 3(10 min)**:E2E + 文档
- 演示 agent-browser 跑流程
- 演示 swag 生成接口文档
- 总结:"9 步走完,一个完整项目"

---

### 验收

- [ ] 提交完整 PR(含前后端 + 测试 + 文档)
- [ ] E2E 录屏通过
- [ ] 接口文档可访问
- [ ] 团队内能跑起来

---

## 案例 B:项目重构(2 天,课后做)

### 题目

把现有的"手动拼 SQL 报表"重构成"DSL 报表引擎"。

### 关键动作

1. **用 codebase-memory-mcp 建索引**
```bash
cd ~/projects/legacy-reports
claude -q "用 codebase-memory-mcp 索引这个项目"
```

2. **用 mcp-codegraph 看依赖图**
```bash
claude -q "用 mcp-codegraph,显示 ReportService 的调用链"
```

3. **识别"上帝函数"**
```bash
claude -q "找出这个项目里最长的 10 个函数,以及它们的入参出参"
```

4. **设计 DSL 语法**
```markdown
# design.md(DSL 设计)

## 旧写法(SQL 拼接)
sql := fmt.Sprintf("SELECT %s FROM %s WHERE %s", cols, table, where)

## 新写法(DSL)
report := Report.
  Select("name", "amount").
  From("orders").
  Where("created_at > ?", startDate).
  GroupBy("name").
  Build()
```

5. **用 superpowers 强制 TDD**
```bash
claude -q "用 superpowers,把 ReportBuilder 的 5 个核心方法先写测试"
```

6. **分阶段迁移**
- 阶段 1:新功能用 DSL
- 阶段 2:旧报表迁移(分批,每批不超过 100 个)
- 阶段 3:删掉旧 SQL 拼接代码

7. **用 ECC review agent 做 code review**
```bash
claude -q "用 everything-claude-code 的 review agent,review 这次重构"
```

### 验收

- [ ] 新功能都用 DSL
- [ ] 80% 旧报表迁移完成
- [ ] 测试覆盖率 ≥ 80%
- [ ] CI 通过

---

## 案例 C:小 bug 修复(半天,课后做)

### 题目

定位并修复"对账系统偶发的对账不平"。

### 关键动作

#### 1. 复现 bug

```bash
cd ~/projects/reconciliation

# 写一个能稳定复现的脚本
cat > reproduce.sh <<'EOF'
#!/bin/bash
# 跑 100 次对账,看是否有对账不平
for i in {1..100}; do
  go run cmd/reconcile/main.go --date=2026-06-15
done
EOF
chmod +x reproduce.sh
```

#### 2. 用 superpowers 的 debugging skill

```bash
claude -q "用 superpowers 的 debugging skill,定位这个 bug"
```

#### 3. 看代码

```bash
claude -q "在 reconcile.go 里,所有可能产生浮点数误差的地方列出来"
```

#### 4. 写 regression test

```go
// regression_test.go
func TestReconcileNoFloatingPointError(t *testing.T) {
    // 用 decimal 包替代 float64
    // ...
}
```

#### 5. 用 ECC review

```bash
claude -q "review 这个 fix"
```

### 验收

- [ ] bug 不再复现(跑 100 次脚本都通过)
- [ ] regression test 在 CI 里
- [ ] 提交 PR

---

## 5.2 三个案例的对比

| 维度 | 案例 A(新项目) | 案例 B(重构) | 案例 C(bug) |
|---|---|---|---|
| 时长 | 2 天 | 2 天 | 半天 |
| 难度 | ★★ | ★★★★ | ★ |
| 重点技能 | 9 步全流程 | codebase-memory + mcp-codegraph | debugging skill |
| 适用学员 | 新人 | 中高级 | 所有人 |

---

## 5.3 案例选择建议

- **新人**:必做案例 A
- **中高级**:案例 A + B
- **所有人**:案例 C(半小时就能跑完)

---

## 5.4 常见问题 FAQ

**Q1:案例 A 的技术栈必须用 Go 吗?**
A:不必须。可以换成 Python/Node/Java。重点是**走完 9 步**。

**Q2:案例 B 的重构真要 2 天吗?**
A:取决于代码量。10 万行代码可能要 1-2 周。这里给的是"压缩版"。

**Q3:案例 C 的 bug 怎么找?**
A:用公司内部的真实 bug,但要先去敏感信息。

**Q4:案例之间可以并行吗?**
A:可以。50 人分 6 组:2 组做 A、2 组做 B、2 组做 C。

**Q5:做不完怎么办?**
A:**不强求做完**。重点是**走完 9 步**,哪怕只跑到第 5 步也比直接跳到第 9 步强。