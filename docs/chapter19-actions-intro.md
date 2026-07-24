# 第19章：GitHub Actions 入门 ⚡

> GitHub Actions 是内置的 CI/CD 工具。让你的代码自动测试、构建、部署！

## 📋 目录

- [什么是 CI/CD？](#什么是-cicd)
- [什么是 GitHub Actions？](#什么是-github-actions)
- [第一个 Workflow](#第一个-workflow)
- [常用 Actions 示例](#常用-actions-示例)
- [可复用工作流](#可复用工作流)
- [GitHub Container Registry](#github-container-registry)
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

## ♻️ 可复用工作流

### 为什么需要可复用工作流？

当多个项目需要相同的 CI/CD 流程时，**不要复制粘贴** —— 使用可复用工作流（Reusable Workflows）：

```
❌ 错误做法：
├── project-a/.github/workflows/ci.yml（500 行）
├── project-b/.github/workflows/ci.yml（复制粘贴，500 行）
└── project-c/.github/workflows/ci.yml（复制粘贴，500 行）
    → 改一个地方，三个都要改！

✅ 正确做法：
├── shared/.github/workflows/ci.yml（定义一次）
├── project-a/.github/workflows/ci.yml（调用）
├── project-b/.github/workflows/ci.yml（调用）
└── project-c/.github/workflows/ci.yml（调用）
    → 改一个地方，全部生效！
```

### 定义可复用工作流

在被调用的仓库中创建工作流，加上 `workflow_call` 触发器：

```yaml
# .github/workflows/reusable-ci.yml（被调用方）
name: Reusable CI

on:
  workflow_call:         # ✅ 关键：允许被其他工作流调用
    inputs:              # 定义输入参数
      node-version:
        description: "Node.js 版本"
        required: false
        type: string
        default: "20"
    secrets:             # 定义需要传入的密钥
      NPM_TOKEN:
        required: false

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
      - run: npm ci
      - run: npm test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run build
```

### 调用可复用工作流

```yaml
# project-a/.github/workflows/ci.yml（调用方）
name: CI

on: [push, pull_request]

jobs:
  call-ci:
    uses: GTX950L/shared-workflows/.github/workflows/reusable-ci.yml@v1
    with:
      node-version: "22"
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

> 💡 **调用规范**：`uses: owner/repo/.github/workflows/file.yml@ref`
> - `@v1`：按 tag 引用
> - `@main`：按分支引用（不推荐，可能不稳定）
> - `@abc123`：按 commit SHA 引用（最安全）

### Composite Actions（复合 Action）

如果不想整条工作流复用，只想复用**一组步骤**，用 Composite Action：

```yaml
# .github/actions/setup-project/action.yml
name: "Setup Project"
description: "安装依赖并运行 Lint"

inputs:
  node-version:
    description: "Node.js 版本"
    required: true

runs:
  using: "composite"
  steps:
    - uses: actions/setup-node@v4
      with:
        node-version: ${{ inputs.node-version }}

    - name: Install dependencies
      shell: bash
      run: npm ci

    - name: Run linter
      shell: bash
      run: npm run lint
```

在工作流中使用：

```yaml
steps:
  - uses: actions/checkout@v4
  - uses: ./.github/actions/setup-project
    with:
      node-version: "20"
  - run: npm test
```

> 💡 **区别总结**：
> | 特性 | Reusable Workflow | Composite Action |
> |------|------------------|-----------------|
> | 复用粒度 | 整个 Job | 一组 Step |
> | 跨仓库调用 | ✅ 支持 | ❌ 仅限当前仓库 |
> | 支持 `needs` | ✅ | ❌ |
> | 适用场景 | 完整的 CI/CD 流程 | 重复的配置步骤 |

---

## 🐳 GitHub Container Registry

### 什么是 Container Registry？

**GitHub Container Registry（ghcr.io）** 是 GitHub 自带的 Docker 镜像仓库。你可以在 GitHub 上存储和分享 Docker 容器镜像，就像存放代码一样。

### 为什么用 ghcr.io 而不是 Docker Hub？

| 特性 | ghcr.io | Docker Hub |
|------|---------|------------|
| **和 GitHub 集成** | ✅ 原生集成 | ❌ 需要额外配置 |
| **权限控制** | ✅ 使用仓库权限 | ❌ 独立权限系统 |
| **免费额度** | ✅ 公开镜像无限流量 | ✅ 公开镜像无限 |
| **匿名拉取** | ✅ 公开镜像无需登录 | ✅ 公开镜像无需登录 |
| **国内访问** | ⚠️ 一般 | ⚠️ 一般 |

### 推送镜像到 ghcr.io

#### 第一步：创建 Personal Access Token

```
1. Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. 勾选权限：
   ├── Contents: Read & Write
   └── Packages: Read & Write
3. 生成 Token 并保存
```

#### 第二步：登录 ghcr.io

```bash
echo $GITHUB_TOKEN | docker login ghcr.io -u GTX950L --password-stdin
```

#### 第三步：构建并推送镜像

```bash
# 构建镜像（标签格式：ghcr.io/用户名/仓库名:版本）
docker build -t ghcr.io/GTX950L/my-app:latest .

# 推送
docker push ghcr.io/GTX950L/my-app:latest
```

#### 第四步：在 Actions 中自动构建推送

```yaml
name: Build and Push Docker Image

on:
  push:
    branches: [main]
    tags:
      - 'v*'

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write  # ⚠️ 需要写入 packages 权限

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Log in to GHCR
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=semver,pattern={{version}}
            type=raw,value=latest,enable={{is_default_branch}}

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

### 拉取镜像

```bash
# 公开镜像不需要登录
docker pull ghcr.io/GTX950L/my-app:latest

# 私有镜像需要登录
echo $GITHUB_TOKEN | docker login ghcr.io -u GTX950L --password-stdin
docker pull ghcr.io/GTX950L/private-app:latest
```

### 管理镜像

```
在 GitHub 网页上：
1. 进入仓库页面
2. 点击右侧 Packages 区域
3. 可以查看、删除、设置镜像权限
```

> 💡 **小贴士**：ghcr.io 的镜像权限默认继承自仓库。私有仓库中的镜像也是私有的，公开仓库中的镜像也是公开的。

---

## ⚠️ Actions 安全最佳实践

### 1. 固定 Action 版本

```yaml
# ❌ 不安全：使用 main 分支（可能被篡改）
uses: actions/checkout@main

# ✅ 安全：固定主版本
uses: actions/checkout@v4

# ✅ 最安全：固定完整 commit SHA
uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11
```

### 2. 使用最小权限原则

```yaml
# 在 workflow 级别指定权限
permissions:
  contents: read        # 只读代码（默认有写权限）
  issues: write         # 需要写 Issue
  pull-requests: read   # 只读 PR
```

### 3. 谨慎使用第三方 Actions

```yaml
# 使用第三方 Action 前检查：
# ✅ Star 数高（说明使用的人多）
# ✅ 最近有更新（说明在维护）
# ✅ 是 Verified Creator（GitHub 认证）
# ✅ 有安全审计记录

# 推荐：优先使用 GitHub 官方 Actions
uses: actions/checkout@v4
uses: actions/setup-node@v4
uses: actions/cache@v4
```

### 4. 限制 Workflow 触发器

```yaml
# ❌ 不安全：任何 push 都触发
on: [push]

# ✅ 安全：只监听特定分支和路径
on:
  push:
    branches: [main]
    paths:
      - 'src/**'
      - 'tests/**'
```

### 5. 保护敏感信息

```yaml
# ✅ 使用 secrets 存储敏感信息
- name: Deploy
  run: deploy-script.sh
  env:
    API_KEY: ${{ secrets.API_KEY }}  # 不会出现在日志中

# ❌ 不要在 workflow 中硬编码
- name: Bad
  run: deploy-script.sh --api-key "my-secret-key"  # 日志可见！
```

### 6. 使用 Dependabot 自动更新 Actions

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
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
- ✅ 可复用工作流和 Composite Action 的区别与使用
- ✅ 如何使用 GitHub Container Registry 管理 Docker 镜像
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
