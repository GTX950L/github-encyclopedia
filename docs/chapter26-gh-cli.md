# 第26章：GitHub CLI 使用 🖥️

> 不想打开网页？GitHub CLI 让你在终端里完成所有操作！

## 📋 目录

- [什么是 GitHub CLI？](#什么是-github-cli)
- [安装 GitHub CLI](#安装-github-cli)
- [登录 GitHub](#登录-github)
- [常用命令](#常用命令)
- [高级技巧](#高级技巧)
- [常见问题](#常见问题)

---

## 什么是 GitHub CLI？

### 基本定义

**GitHub CLI（`gh`）** 是 GitHub 官方提供的**命令行工具**。

### 为什么需要？

| 操作 | 网页点击 | GitHub CLI |
|------|----------|--------------|
| **创建 Issue** | 打开浏览器 → 点击 → 填写 → 提交 | ✅ `gh issue create` |
| **克隆仓库** | 复制地址 → `git clone` | ✅ `gh repo clone 用户名/仓库名` |
| **创建 PR** | 打开浏览器 → 点击 → 填写 → 提交 | ✅ `gh pr create` |

> 💡 **适合人群**：
> - 喜欢命令行的开发者
> - 需要批量操作的场景
> - 想提高效率的资深用户

---

## 安装 GitHub CLI

### Windows

```powershell
# 方法1：使用 winget（推荐）✅
winget install GitHub.cli

# 方法2：使用 Chocolatey
choco install gh

# 方法3：使用 Scoop
scoop install gh
```

### macOS

```bash
# 使用 Homebrew（推荐）✅
brew install gh

# 使用 MacPorts
sudo port install gh
```

### Linux

```bash
# Ubuntu/Debian
sudo apt install gh

# Fedora
sudo dnf install gh

# Arch Linux
sudo pacman -S github-cli
```

### 验证安装

```bash
gh --version
# 输出：gh version 2.94.0 (2026-06-10)
```

---

## 登录 GitHub

### 步骤详解

#### 第1步：运行登录命令

```bash
gh auth login
```

#### 第2步：按提示操作

```
? What account do you want to log in to?
> GitHub.com
  GitHub Enterprise Server

→ 选择 GitHub.com（如果你是个人用户）
```

```
? What is your preferred protocol for Git operations?
> HTTPS
  SSH

→ 推荐选择 HTTPS（简单）
→ 如果你配置了 SSH Key，选择 SSH
```

```
? How would you like to authenticate GitHub CLI?
> Login with a web browser（推荐）✅
  Paste an authentication token
```

```
? How would you like to authenticate GitHub CLI?
> Login with a web browser

→ 会显示一个 8 位验证码（如：ABCD-1234）
→ 复制它
→ 浏览器会自动打开 GitHub 授权页面
→ 粘贴验证码
→ 点击 Authorize github
→ 完成！
```

---

## 常用命令

### 1️⃣ 仓库操作

#### 创建仓库

```bash
# 在当前目录创建仓库并推送到 GitHub
gh repo create my-project --public --source=. --push

# 创建私有仓库
gh repo create my-private-project --private --source=. --push
```

#### 克隆仓库

```bash
# 基本格式
gh repo clone 用户名/仓库名

# 示例
gh repo clone GTX950L/github-encyclopedia

# 克隆到指定目录
gh repo clone GTX950L/github-encyclopedia -- my-encyclopedia
```

#### 查看仓库列表

```bash
# 查看你的所有仓库
gh repo list

# 查看某个用户的仓库
gh repo list 用户名

# 只显示仓库名（适合脚本）
gh repo list --json name --jq '.[].name'
```

---

### 2️⃣ Issue 操作

#### 创建 Issue

```bash
# 命令行交互式创建
gh issue create

# 直接指定标题和描述
gh issue create --title "Bug: 登录失败" --body "描述..."

# 从文件读取描述
gh issue create --title "Feature: 深色模式" --body-file feature.md
```

#### 列出 Issue

```bash
# 列出所有打开的 Issue
gh issue list

# 过滤：只显示 Bug 标签的
gh issue list --label bug

# 过滤：只显示指派给自己的
gh issue list --assignee @me
```

#### 查看 Issue

```bash
# 查看某个 Issue
gh issue view 123

# 在浏览器中打开
gh issue view 123 --web

# 在编辑器中打开（查看详细讨论）
gh issue view 123 --comments
```

---

### 3️⃣ PR 操作

#### 创建 PR

```bash
# 交互式创建（推荐）✅
gh pr create

# 指定标题和描述
gh pr create --title "添加登录功能" --body "描述..."

# 指定目标分支
gh pr create --base develop --head feature/login
```

#### 列出 PR

```bash
# 列出所有打开的 PR
gh pr list

# 过滤：只显示需要你审查的
gh pr list --review-requested
```

#### 审查 PR

```bash
# 查看某个 PR 的代码变更
gh pr view 123

# 在本地 checkout 这个 PR（用于测试）
gh pr checkout 123
```

#### 合并 PR

```bash
# 合并 PR（自动检测合并方式）
gh pr merge 123

# 指定合并方式
gh pr merge 123 --squash   # 压缩合并
gh pr merge 123 --rebase   # 变基合并
gh pr merge 123 --merge    # 普通合并
```

---

### 4️⃣ 实用命令

#### 快速打开浏览器

```bash
# 打开当前仓库的 GitHub 页面
gh browse

# 打开某个 Issue
gh issue view 123 --web

# 打开 Actions 页面
gh browse --actions
```

#### 查看通知

```bash
# 在终端查看通知
gh notification

# 交互式处理通知
gh notification --all
```

#### 生成 Token

```bash
# 生成新的 Token
gh auth token
```

---

## 高级技巧

### 1️⃣ 配置别名（Alias）✅ 推荐！

```bash
# 给常用命令设置别名
gh alias set issue 'issue list --assignee @me'
gh alias set prs 'pr list --review-requested'

# 使用别名
gh issue   # 等效于：gh issue list --assignee @me
gh prs     # 等效于：gh pr list --review-requested
```

---

### 2️⃣ 批量操作

#### 示例：给所有仓库点 Star

```bash
# 获取你的所有仓库，然后点 Star
gh repo list --json nameWithOwner --jq '.[].nameWithOwner' |
while read repo; do
    gh repo star "$repo"
    echo "Starred: $repo"
done
```

#### 示例：批量关闭 Issue

```bash
# 关闭所有带有 "wontfix" 标签的 Issue
gh issue list --label wontfix --json number --jq '.[].number' |
while read num; do
    gh issue close "$num" --reason "wont fix"
    echo "Closed: #$num"
done
```

---

### 3️⃣ 在脚本中使用 `gh`

#### 示例：自动创建每日 Issue

```bash
#!/bin/bash
# daily-issue.sh

DATE=$(date +%Y-%m-%d)
TITLE="Daily standup: $DATE"

BODY="## 昨天做了什么？
- 

## 今天计划做什么？
- 

## 有什么阻碍？
- "

gh issue create --title "$TITLE" --body "$BODY"
```

**配合 cron 定时运行**：

```bash
# 每天早上 9 点自动创建 Issue
0 9 * * * /path/to/daily-issue.sh
```

---

## 常见问题

### Q1: `gh` 命令和 `git` 命令有什么区别？

**A**: 

| 命令 | 作用 |
|------|------|
| **`git`** | 操作本地仓库（commit、push、branch） |
| **`gh`** | 操作 GitHub 平台（Issue、PR、Actions） |

**配合使用**：

```bash
# 使用 git 提交代码
git add .
git commit -m "feat: 添加功能"
git push

# 使用 gh 创建 PR
gh pr create --title "添加功能"
```

---

### Q2: 可以在服务器上使用 `gh` 吗？

**A**: ✅ **可以！**

**配置方法**：

```bash
# 使用 Token 登录（非交互式）
echo $GITHUB_TOKEN | gh auth login --with-token
```

**在 GitHub Actions 中使用**：

```yaml
# GitHub Actions 自带 gh 命令
- name: Create Issue on failure
  if: failure()
  run: gh issue create --title "Build failed" --body "查看日志"
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

### Q3: 如何更新 `gh` 到最新版本？

**A**: 

| 安装方式 | 更新命令 |
|----------|------------|
| **winget** | `winget upgrade GitHub.cli` |
| **Homebrew** | `brew upgrade gh` |
| **apt** | `sudo apt update && sudo apt upgrade gh` |

---

## 📝 实战练习

### 练习1：第一次使用 `gh`

**任务**：
1. 安装 GitHub CLI
2. 运行 `gh auth login` 登录
3. 使用 `gh repo clone` 克隆一个仓库
4. 使用 `gh issue list` 查看 Issue

---

### 练习2：创建别名提高效率

**任务**：
1. 设置别名：`issue` → `issue list --assignee @me`
2. 设置别名：`prs` → `pr list --review-requested`
3. 使用别名，体验效率提升

---

### 练习3：编写自动化脚本

**任务**：
1. 写一个脚本：自动创建 Issue
2. 配合 cron，每天早上自动运行
3. 查看 GitHub，确认 Issue 被创建

---

## 📚 延伸阅读

- [第17章：GitHub Token 完全指南](chapter17-token-guide.md) - Token 的详细用法
- [第27章：GitHub API 详解](chapter27-github-api.md) - 使用 API 开发工具
- [第19章：GitHub Actions 入门](chapter19-actions-intro.md) - 自动化工作流

---

## 📝 本章小结

你现在学会了：

- ✅ 什么是 GitHub CLI，为什么需要它
- ✅ 如何安装和登录
- ✅ 常用命令（仓库、Issue、PR 操作）
- ✅ 高级技巧（别名、批量操作、脚本自动化）

### 💡 核心要点

> **`gh` 是提升效率的神器！**
> 
> 熟练掌握后，你可以：
> - 不用打开浏览器，在终端完成所有操作
> - 批量管理多个仓库
> - 编写自动化脚本

---

<p align="right">
  <a href="chapter25-git-advanced.md">← 上一章</a>
  &nbsp;|&nbsp;
  <a href="chapter27-github-api.md">下一章 →</a>
</p>