# 第28章：企业级最佳实践 🏢

> 个人项目和团队项目不同。本章详解企业级开发的最佳实践。

## 📋 目录

- [企业 vs 个人开发](#企业-vs-个人开发)
- [分支策略](#分支策略)
- [代码审查规范](#代码审查规范)
- [CI/CD 最佳实践](#cicd-最佳实践)
- [Release 管理](#release-管理)
- [常见问题](#常见问题)

---

## 企业 vs 个人开发

### 核心区别

| 方面 | 个人开发 | 企业开发 |
|------|----------|----------|
| **分支策略** | 随意 | ⚠️ 严格规范 |
| **代码审查** | 可选 | ✅ 必须 |
| **测试覆盖率** | 随意 | ⚠️ 要求高 |
| **文档** | 随意 | ✅ 必须完善 |
| **权限管理** | 随意 | ⚠️ 严格 |

---

## 分支策略

### GitHub Flow（推荐企业使用！）✅

```
main（生产环境，始终可发布）
│
└── feature/xxx（功能分支）
    │
    └── 开发完成后，创建 PR → 审查 → 合并到 main
```

**为什么推荐？**
- ✅ 简单，容易理解
- ✅ 适合持续部署（CD）
- ✅ GitHub 官方推荐

---

### 保护主分支 ⚠️ 必须！

#### 配置步骤

```
1. 进入仓库 Settings → Branches
2. 点击 Add branch protection rule
3. 填写 Branch name pattern: main
4. 勾选以下选项（✅ 推荐）：
   ├── ✅ Require a pull request before merging
   │   └── ✅ Require approvals（至少 1 人批准）
   ├── ✅ Require status checks to pass before merging
   │   └── 选择 CI/CD 检查（如 test、build）
   ├── ✅ Require conversation resolution before merging
   │   └── 所有 PR 讨论必须解决
   └── ✅ Include administrators
       └── 即使是管理员也要遵守规则！
```

> ⚠️ **效果**：
> - 不能直接 `git push` 到 main
> - 必须创建 PR
> - 必须通过代码审查
> - 必须通过 CI/CD 检查

---

## 代码审查规范

### 为什么需要代码审查？

| 好处 | 说明 |
|------|------|
| **提高代码质量** | 发现 Bug、改进设计 |
| **知识共享** | 团队成员了解彼此的代码 |
| **防止恶意代码** | 避免有人提交后门 |
| **统一代码风格** | 保持项目一致性 |

---

### 审查清单 ✅

#### 作为审查者

| 检查项 | 说明 |
|--------|------|
| **代码是否正确？** | 有没有明显的 Bug？ |
| **代码是否清晰？** | 变量名是否易懂？注释是否充分？ |
| **是否有测试？** | 新功能是否有单元测试？ |
| **是否通过 CI？** | 所有自动化测试是否通过？ |
| **是否符合规范？** | 代码风格是否符合团队约定？ |

#### 作为提交者

| 做法 | 说明 |
|------|------|
| **PR 要小** | 一个 PR 只做一件事（不超过 500 行） |
| **写清楚描述** | 说明为什么需要这个修改 |
| **自己先审查** | 提交前自己先检查一遍 |
| **回应要及时** | 审查意见提出后，24 小时内回应 |

---

### 如何礼貌地提出修改意见？

#### ❌ 不好的做法

```
这段代码写得很烂。
你有没有脑子？
这样写肯定有 Bug。
```

#### ✅ 好的做法

```
💡 建议：这里的变量名可以改成 `userList`，更清晰。

🐛 发现潜在问题：如果 `users` 为 `null`，这行会报错。建议加一个判断。

📝 小建议：这个函数有点长（超过 50 行），是否可以拆分成几个小函数？

✅ 总体来说写得很好！只有一个小建议...
```

> 💡 **核心原则**：
> - 对代码，不对人
> - 解释为什么需要修改
> - 提供解决方案，不只是指出问题

---

## CI/CD 最佳实践

### 企业级 CI/CD 配置

#### 完整的 `.github/workflows/ci.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]  # 测试多个 Node 版本
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci  # 使用 ci 而不是 install（更严格）

      - name: Run linter
        run: npm run lint

      - name: Run tests
        run: npm test

      - name: Upload coverage
        uses: codecov/codecov-action@v4
        with:
          files: ./coverage/coverage.xml

  build:
    needs: test  # 测试通过后才构建
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build
        run: |
          npm ci
          npm run build

      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/
```

---

### 代码覆盖率要求 ✅

```
企业通常要求：
│
├── ✅ 新功能：测试覆盖率 > 80%
├── ✅ Bug 修复：必须有回归测试
└── ❌ 不允许：没有测试的代码合并到 main
```

**如何在 GitHub 上强制？**

```
1. 在仓库 Settings → Branches → 分支保护规则
2. 勾选 "Require status checks to pass"
3. 添加 `test` 和 `coverage` 为必通过的检查
```

---

## Release 管理

### 语义化版本（Semantic Versioning）

#### 版本号格式：`MAJOR.MINOR.PATCH`

| 部分 | 示例 | 何时增加？ |
|------|------|----------|
| **MAJOR** | 2.0.0 | 有不兼容的 API 修改 |
| **MINOR** | 1.2.0 | 添加新功能（向后兼容） |
| **PATCH** | 1.1.1 | Bug 修复（向后兼容） |

> 💡 **示例**：
> - `1.0.0`：第一个正式版本
> - `1.0.1`：修复了一个 Bug
> - `1.1.0`：添加了一个新功能
> - `2.0.0`：重构了 API，不兼容 v1.x

---

### 使用 GitHub Releases

#### 步骤

```
1. 进入仓库页面 → Releases（在右侧）
2. 点击 "Create a new release"
3. 填写信息：
   ├── Tag version: v1.0.0
   ├── Release title: v1.0.0 - Initial Release
   ├── Description:（变更日志）
   └── 可以附加编译好的文件（如 .exe、.zip）
4. 点击 "Publish release"
```

#### 自动生成 Release（推荐！）✅

使用 GitHub Actions 自动生成 Release：

```yaml
name: Auto Release

on:
  push:
    tags:
      - 'v*'  # 推送以 v 开头的 tag 时触发

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Generate changelog
        run: |
          # 使用工具自动生成变更日志
          npx conventional-changelog-cli -p

      - name: Create Release
        uses: softprops/action-gh-release@v2
        with:
          files: dist/*
          body: ${{ steps.changelog.outputs.content }}
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## 📋 CODEOWNERS（代码负责人）

### 什么是 CODEOWNERS？

**CODEOWNERS** 是 GitHub 的一个机制，可以为不同文件/目录**指定负责人**。当有人修改这些文件时，对应的负责人会自动被要求审查 PR。

### 配置方式

在仓库根目录或 `docs/` 目录创建 `CODEOWNERS` 文件：

```
# .github/CODEOWNERS

# 全局负责人（整个仓库）
* @GTX950L

# 前端代码由前端团队负责
/src/components/ @frontend-team

# API 文档由后端团队负责
/docs/api/ @backend-team

# 配置文件由 DevOps 团队负责
*.yml @devops-team
*.yaml @devops-team
Dockerfile @devops-team

# 测试文件由所有人负责（不自动指派）
/tests/ @team-lead @qa-team
```

### 效果

```
当你提交一个修改 src/components/Button.tsx 的 PR：
│
├── 自动指派 @frontend-team 作为 Reviewer
├── 他们收到通知
├── 必须有人批准才能合并
└── 保护分支规则生效
```

> 💡 CODEOWNERS 也可以结合分支保护规则使用：勾选 "Require approval from code owners" 来强制要求负责人批准。

---

## 🏷️ Semantic PR（语义化 PR）

### 为什么要语义化？

语义化 PR 让项目历史更加清晰，也方便自动生成变更日志。

### PR 标题格式

```
<类型>(<范围>): <简短描述>

# 示例
feat: 添加用户登录功能
fix(api): 修复登录接口超时
docs: 更新 README 安装说明
refactor: 重构数据库查询逻辑
```

### 常见类型

| 类型 | 含义 | 是否影响版本号 |
|------|------|-------------|
| `feat` | 新功能 | 次版本号+1 |
| `fix` | Bug 修复 | 修订号+1 |
| `docs` | 文档更新 | 不改变 |
| `style` | 代码格式（不影响功能） | 不改变 |
| `refactor` | 重构（不修 Bug 不加功能） | 不改变 |
| `test` | 添加测试 | 不改变 |
| `chore` | 构建/工具链变更 | 不改变 |
| `perf` | 性能优化 | 不改变 |
| `ci` | CI/CD 配置变更 | 不改变 |

### 配合 Semantic Release 自动发布

```yaml
# .github/workflows/release.yml
name: Semantic Release

on:
  push:
    branches: [main]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

> 💡 **推荐**：在 PR 模板中提示提交者使用语义化格式，或者在 CI 中自动检查 PR 标题是否符合格式。

---

## 权限管理

### 企业级权限配置

| 角色 | 权限 | 适用人群 |
|------|------|----------|
| **Admin** | 全部权限（包括删除仓库） | 技术负责人 |
| **Maintain** | 可以合并 PR、管理 Issues | 技术 Leader |
| **Write** | 可以推送代码、创建 PR | 核心开发者 |
| **Triage** | 可以管理 Issues 和 PR | 产品经理 |
| **Read** | 只能查看 | 实习生、外包 |

#### 如何配置？

```
1. 进入仓库 Settings → Collaborators and teams
2. 点击 "Add people"
3. 输入用户名或邮箱
4. 选择角色
5. 点击 "Add"
```

---

## 常见问题

### Q1: 企业应该使用 Public 还是 Private 仓库？

**A**: 

| 场景 | 推荐 |
|------|----------|
| **开源项目** | ✅ Public |
| **内部工具** | ✅ Private |
| **核心产品** | ✅ Private + 仅团队成员可访问 |

---

### Q2: 如何保证代码质量？

**A**: 多层防护：

```
第1层：代码风格检查（Linter）
│
├── 使用 ESLint（JavaScript）、Pylint（Python）等
└── 在 CI 中强制检查

第2层：代码审查（Code Review）
│
├── 至少 1 人批准才能合并
└── 使用 PR 模板，确保质量

第3层：自动化测试（CI）
│
├── 单元测试覆盖率 > 80%
└── 所有测试通过才能合并

第4层：定期重构
    │
    └── 每季度的技术债处理
```

---

### Q3: 如何处理技术债？

**A**: 

**建议**：

```
1. 在 Issue 中标记技术债
   │
   ├── 标签：tech-debt
   └── 指派给相关负责人

2. 每个 Sprint 分配 20% 时间处理技术债
   │
   └── 避免债台高筑

3. 定期重构
   │
   └── 每个季度安排一次重构 Sprint
```

---

## 📝 实战练习

### 练习1：配置分支保护

**任务**：
1. 在你的仓库启用分支保护
2. 设置至少 1 人批准才能合并
3. 设置必须通过 CI 检查
4. 测试：尝试直接 push 到 main（应该被拒绝）

---

### 练习2：配置 CI/CD 流水线

**任务**：
1. 创建 `.github/workflows/ci.yml`
2. 配置为：测试 → 构建 → 部署
3. 提交一个 PR，查看 CI 是否自动运行
4. 确认所有检查通过后，才能合并

---

### 练习3：创建第一个 Release

**任务**：
1. 在仓库中打一个 tag：`git tag v1.0.0`
2. 推送到 GitHub：`git push origin v1.0.0`
3. 在 GitHub 上创建 Release
4. 填写变更日志

---

## 📚 延伸阅读

- [第7章：分支管理](chapter07-branch-management.md) - 分支的基本操作
- [第8章：合并代码](chapter08-merge-code.md) - 合并分支的详细讲解
- [第19章：GitHub Actions 入门](chapter19-actions-intro.md) - 自动化工作流

---

## 📝 本章小结

你现在了解了：

- ✅ 企业开发和个人开发的区别
- ✅ 如何配置分支保护规则
- ✅ 代码审查的最佳实践
- ✅ CI/CD 的企业级配置
- ✅ 如何管理 Release 和版本号

### 💡 核心要点

> **企业级开发的核心是：流程和规范。**
> 
> 通过分支保护、代码审查、CI/CD，可以确保：
> - 🔒 主分支始终稳定
> - 📊 代码质量有保障
> - 🚀 发布流程自动化

---

<p align="right">
  <a href="chapter27-github-api.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="appendix-a-project-structure.md">附录A：项目文件组成 →</a>
</p>

---

<p align="center">
  <b>🎉 下一站：云端与社区！</b><br>
  继续阅读 <a href="chapter29-codespaces.md">第29章：Codespaces 云开发</a><br>
  或者 <a href="chapter30-discussions.md">第30章：Discussions 社区</a>
</p>
