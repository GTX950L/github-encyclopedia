# 第19章：GitHub Actions 入门 ⚡

> GitHub Actions 是内置的 CI/CD 工具。让你的代码自动测试、构建、部署！

## 📋 目录

- [什么是 CI/CD？](#什么是-cicd)
- [什么是 GitHub Actions？](#什么是-github-actions)
- [第一个 Workflow](#第一个-workflow)
- [常用 Actions 示例](#常用-actions-示例)
- [常见问题](#常见问题)

---

## 什么是 CI/CD？

### 基本定义

| 术语 | 全称 | 说明 |
|------|------|------|
| **CI** | Continuous Integration（持续集成） | 每次提交代码，自动运行测试 |
| **CD** | Continuous Deployment（持续部署） | 测试通过后，自动部署到服务器 |

### 比喻

```
❌ 不用 CI/CD（手动）：
│
├── 你修改了代码
├── 手动运行测试（忘了怎么办？）
├── 测试通过后，手动部署
├── 部署过程容易出错
└── 浪费时间！

✅ 使用 CI/CD（自动）：
│
├── 你提交代码
├── GitHub Actions 自动运行测试
├── 测试通过 → 自动部署
└── 你只需要看着就行了！
```

---

## 什么是 GitHub Actions？

### 核心概念

| 概念 | 说明 |
|------|------|
| **Workflow** | 一个自动化流程（如：测试 → 构建 → 部署） |
| **Event** | 触发 Workflow 的事件（如：push、PR、定时） |
| **Job** | Workflow 中的一个任务（如：运行测试） |
| **Step** | Job 中的一个步骤（如：安装依赖） |
| **Action** | 可复用的命令（如：setup-node） |

---

## 第一个 Workflow

### 场景：每次提交代码，自动运行测试

#### 第1步：创建 Workflow 文件

```bash
# 创建目录
mkdir -p .github/workflows

# 创建 Workflow 文件
touch .github/workflows/test.yml
```

#### 第2步：编写 Workflow

```yaml
# .github/workflows/test.yml

name: Run Tests  # Workflow 的名字

# 触发器
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# 任务
jobs:
  test:
    runs-on: ubuntu-latest  # 运行环境

    steps:
      # 步骤1：检出代码
      - name: Checkout code
        uses: actions/checkout@v4

      # 步骤2：安装 Node.js
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      # 步骤3：安装依赖
      - name: Install dependencies
        run: npm install

      # 步骤4：运行测试
      - name: Run tests
        run: npm test
```

#### 第3步：提交并推送

```bash
git add .github/workflows/test.yml
git commit -m "ci: 添加 GitHub Actions 自动测试"
git push origin main
```

#### 第4步：查看运行结果

```
1. 进入仓库页面 → Actions 标签
2. 你会看到 Workflow 的运行记录
3. 点击某个运行 → 查看详细日志
```

---

## 常用 Actions 示例

### 1️⃣ 自动部署到 GitHub Pages ✅ 推荐！

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4

      - name: Build
        run: |
          npm install
          npm run build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

> 📝 **详细步骤**见 [第20章：GitHub Pages](chapter20-pages-website.md)

---

### 2️⃣ 自动发布到 npm

```yaml
name: Publish to npm

on:
  release:
    types: [ published ]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          registry-url: 'https://registry.npmjs.com'

      - name: Publish
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

---

### 3️⃣ 自动打标签（Tag）

```yaml
name: Create Tag

on:
  push:
    branches: [ main ]

jobs:
  tag:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Bump version and create tag
        uses: anothrnick/github-tag-action@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          DEFAULT_BUMP: patch
```

---

## 常见问题

### Q1: Actions 的免费额度是多少？

**A**: 

| 账号类型 | 每月免费分钟数 | 说明 |
|----------|----------------|------|
| **Free** | 2000 分钟 | 个人使用够用 |
| **Pro** | 3000 分钟 | $7/月 |
| **Team** | 3000 分钟 | $9/月/人 |

**如何节省？**

```
✅ 好的做法：
├── 只在 main 分支运行测试（不对 feature 分支运行）
├── 使用缓存（cache）
└── 合并多个 Step 到一个 Job

❌ 不好的做法：
├── 每次 push 都运行全套测试
└── 重复安装依赖（不使用缓存）
```

---

### Q2: Workflow 运行失败怎么办？

**A**: 

**调试步骤**：

```
1. 进入 Actions 页面
2. 点击失败的运行
3. 查看日志（红色部分）
4. 修复问题
5. 重新推送
```

**常见错误**：

| 错误 | 原因 | 解决方法 |
|------|------|----------|
| `npm: command not found` | 没有安装 Node.js | 添加 `setup-node` Action |
| `Permission denied` | Token 权限不足 | 检查 `secrets` 配置 |
| `Timeout` | 任务运行超时 | 增加超时时间或优化任务 |

---

### Q3: 可以使用自己的服务器运行 Actions 吗？

**A**: ✅ **可以！** 这叫 **Self-hosted Runner**。

**为什么需要？**

```
GitHub 提供的 Runner：
├── ✅ 免费（有额度限制）
├── ✅ 不需要维护
└── ❌ 运行速度可能较慢

自己的 Runner：
├── ✅ 更快
├── ✅ 无额度限制
└── ❌ 需要自己维护服务器
```

**如何配置？**

```
Settings → Actions → Runners → Add runner
│
按照提示操作即可。
```

---

## 📝 实战练习

### 练习1：添加第一个 Workflow

**任务**：
1. 在一个 Node.js 项目中创建 `.github/workflows/test.yml`
2. 配置为：每次 push 到 main 时运行 `npm test`
3. 提交并推送
4. 查看 Actions 页面，确认运行成功

---

### 练习2：添加自动部署

**任务**：
1. 配置 GitHub Pages
2. 创建部署 Workflow
3. 推送后，确认网站自动更新

---

## 📚 延伸阅读

- [第20章：GitHub Pages 搭建网站](chapter20-pages-website.md) - 免费托管个人博客
- [第27章：GitHub API 详解](chapter27-github-api.md) - 使用 API 开发工具
- [附录C：GitHub Actions 市场](appendix-c-actions-marketplace.md) - 查找现成的 Actions

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 CI/CD，为什么需要它
- ✅ 什么是 GitHub Actions
- ✅ 如何创建第一个 Workflow
- ✅ 常用 Actions 示例（测试、部署、发布）
- ✅ 如何调试失败的 Workflow

### 💡 核心要点

> **GitHub Actions 是免费且强大的 CI/CD 工具。**
> 
> 即使是个人项目，也建议使用 Actions 自动运行测试。
> 这能确保你的代码始终保持可运行状态。

---

<p align="right">
  <a href="chapter18-ai-automation.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter20-pages-website.md">下一章 →</a>
</p>
