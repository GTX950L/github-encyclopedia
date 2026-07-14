# 附录E：Conventional Commits（约定式提交）规范 📝

> 写好 Commit Message，让你的 Git 历史清晰、可读、可自动化。

---

## 什么是 Conventional Commits？

**Conventional Commits**（约定式提交）是一套提交信息的规范，它规定了 Commit Message 的格式，让提交历史更加清晰。

### 为什么需要规范？

```
❌ 没有规范的历史：
├── 修复了一些东西
├── 更新
├── 修改
├── 111
└── 最终版

✅ 有规范的历史：
├── feat: 添加用户登录功能
├── fix(api): 修复登录超时问题
├── docs: 更新 README 安装说明
└── chore: 升级依赖版本
```

> 哪个更容易理解？哪个更适合生成变更日志（Changelog）？答案很明显。

---

## 基本格式

```
<类型>(<范围>): <描述>

[可选的正文]

[可选的脚注]
```

### 最简单的例子

```bash
git commit -m "fix: 修复登录页面样式错乱"
```

### 带范围的例子

```bash
git commit -m "feat(api): 添加用户注册接口"
```

### 带详细说明的例子

```bash
git commit -m "feat: 添加深色模式支持

实现了完整的深色模式切换功能：
- 使用 CSS 变量管理颜色
- 自动跟随系统主题
- 用户可手动切换
- 偏好设置保存到 localStorage

Closes #123"
```

---

## 类型大全

### 核心类型

| 类型 | 含义 | 是否影响版本号 |
|------|------|-------------|
| `feat` | 新功能（Feature） | 次版本号+1 |
| `fix` | Bug 修复 | 修订号+1 |
| `BREAKING CHANGE` | 不兼容的变更 | 主版本号+1 |

### 推荐类型

| 类型 | 含义 | 使用场景 |
|------|------|----------|
| `feat` | 新功能 | 添加了一个用户可见的新功能 |
| `fix` | Bug 修复 | 修复了一个 Bug |
| `docs` | 文档变更 | 修改 README、注释、文档 |
| `style` | 代码格式 | 格式化、分号、空格（不影响代码逻辑） |
| `refactor` | 重构 | 重构代码（不修 Bug 不加功能） |
| `perf` | 性能优化 | 性能提升的代码变更 |
| `test` | 测试相关 | 添加或修改测试 |
| `build` | 构建系统 | 修改构建工具、依赖（webpack、npm） |
| `ci` | CI/CD | 修改 CI 配置（GitHub Actions、Jenkins） |
| `chore` | 杂项 | 其他不修改源码的变更 |
| `revert` | 回退 | 回退之前的提交 |

---

## 使用场景示例

### 日常开发

```bash
# 添加新功能
git commit -m "feat: 添加用户头像上传功能"

# 修复 Bug
git commit -m "fix: 修复头像上传时文件类型校验错误"

# 更新文档
git commit -m "docs: 更新 API 文档中的示例代码"

# 重构代码
git commit -m "refactor: 提取用户验证逻辑为独立模块"
```

### 团队协作

```bash
# 修复特定模块
git commit -m "fix(auth): 修复 Token 过期未自动刷新的问题"

# 前端修改
git commit -m "feat(ui): 添加响应式导航栏"

# 后端修改
git commit -m "feat(api): 添加批量删除接口"
```

### 不兼容的变更

```bash
# 方式1：在类型后加 !
git commit -m "feat!: 重构 API 认证机制（不兼容旧版本）"

# 方式2：在脚注中标注
git commit -m "feat: 替换 JWT 为 OAuth 2.0

BREAKING CHANGE: 旧版本的 Token 不再有效，需要重新登录。"
```

---

## 如何自动检查 Commit 格式

### 方法1：使用 commitlint

```bash
# 安装 commitlint
npm install -g @commitlint/cli @commitlint/config-conventional

# 创建配置文件（commitlint.config.js）
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js

# 配合 Husky（Git Hooks 工具）
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit $1'
```

### 方法2：在 GitHub Actions 中检查 PR 标题

```yaml
# .github/workflows/pr-lint.yml
name: Lint PR

on:
  pull_request:
    types: [opened, edited, synchronize]

jobs:
  check-pr-title:
    runs-on: ubuntu-latest
    steps:
      - name: Check PR title format
        uses: actions/github-script@v7
        with:
          script: |
            const title = context.payload.pull_request.title
            const pattern = /^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)(\(.+\))?!?: .+/
            if (!pattern.test(title)) {
              core.setFailed(`PR 标题不符合 Conventional Commits 规范！
                格式：<类型>(<范围>): <描述>
                示例：feat: 添加登录功能
                       fix(api): 修复超时问题
                你的标题：${title}`)
            }
```

---

## 配合工具自动生成 Changelog

### 使用 standard-version

```bash
# 安装
npm install -g standard-version

# 自动生成 CHANGELOG.md 并提升版本号
standard-version

# 查看生成的 CHANGELOG.md 内容
cat CHANGELOG.md
```

**生成的示例**：

```markdown
# Changelog

## [1.2.0] (2026-07-14)

### Features
- 添加用户头像上传功能
- 添加深色模式支持

### Bug Fixes
- 修复登录超时不弹提示的问题
- 修复头像上传时文件类型校验错误
```

### 在 GitHub Actions 中自动生成

```yaml
name: Release

on:
  push:
    branches: [main]

jobs:
  release:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      - run: npm ci
      - run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## 常见的提交信息示例

### 新功能

```bash
feat: 添加用户注册功能
feat(auth): 添加微信登录支持
feat(ui): 添加深色模式切换按钮
```

### 修复

```bash
fix: 修复首页白屏问题
fix(api): 修复登录接口返回 500
fix(db): 修复用户表索引重复问题
```

### 文档

```bash
docs: 更新安装说明
docs(api): 补充接口文档示例
docs(readme): 添加项目截图
```

### 重构/优化

```bash
refactor: 提取通用工具函数
perf: 优化列表渲染性能（减少 50% 重绘）
style: 格式化代码（统一使用 2 空格缩进）
```

### 测试

```bash
test: 添加用户登录单元测试
test(api): 添加注册接口集成测试
test: 修复测试用例中的断言错误
```

### CI/CD

```bash
ci: 添加 GitHub Actions 自动测试
ci: 优化构建缓存策略
ci: 修复部署脚本中的环境变量问题
```

### 杂项

```bash
chore: 升级依赖版本
chore: 配置 ESLint
build: 升级 webpack 到 v6
revert: 回退用户头像功能
```

---

## 最佳实践

### 1. 用英文还是中文？

```bash
# ✅ 推荐：用英文（全球统一）
feat: add user login function

# ✅ 也可以用中文（团队约定）
feat: 添加用户登录功能
```

### 2. 描述用祈使句

```bash
# ✅ 正确：祈使句
feat: add login button     ← "添加登录按钮"

# ❌ 错误：过去式
feat: added login button   ← 已添加

# ❌ 错误：正在进行
feat: adding login button  ← 正在添加
```

### 3. 描述要简短，但要有信息量

```bash
# ❌ 太简单，不知道改了啥
fix: fix bug

# ❌ 太长，不适合行内显示
feat: add new feature for user to upload their profile picture and crop it

# ✅ 刚好
feat: add user avatar upload with cropping
```

### 4. 不要用大写字母开头

```bash
# ✅ 正确
feat: add login function

# ❌ 错误
feat: Add login function
```

### 5. 结尾不要加句号

```bash
# ✅ 正确
feat: add login function

# ❌ 错误
feat: add login function.
```

### 6. 范围（scope）的使用

```bash
# 模块名、组件名、文件名都可以作为范围

# 按模块
feat(auth): add login function
feat(api): add user endpoint
feat(db): add user table migration

# 按组件
fix(Button): fix hover color
feat(Navbar): add responsive menu
```

---

## 总结

```
格式：<类型>(<可选范围>): <描述>
例子：feat(api): add user login endpoint

类型：
  feat     = 新功能（用户可见）
  fix      = Bug 修复
  docs     = 文档变更
  refactor = 代码重构
  perf     = 性能优化
  test     = 测试相关
  ci       = CI/CD 配置
  chore    = 杂项
  style    = 格式调整（不影响功能）
  build    = 构建工具
  revert   = 回退提交

优点：
  ✅ 提交历史清晰可读
  ✅ 自动生成 Changelog
  ✅ 自动语义化版本控制
  ✅ 团队协作更高效
```

---

<p align="right">
  <a href="chapter30-discussions.md">← 第30章</a>
  &nbsp;|&nbsp;
  <a href="../README.md">返回首页 →</a>
</p>
